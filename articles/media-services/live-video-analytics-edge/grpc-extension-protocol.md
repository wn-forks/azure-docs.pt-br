---
title: Protocolo de extensão gRPC – Azure
description: Neste artigo, você aprenderá a usar o protocolo de extensão gRPC para enviar mensagens entre o módulo da Análise Dinâmica de Vídeo e a extensão personalizada de IA ou CV.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 288dcd1a11c7c42d8796d3b17f2bfd56f562aaf1
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89448033"
---
# <a name="grpc-extension-protocol"></a>Protocolo de extensão gRPC

Neste artigo, você aprenderá a usar o protocolo de extensão gRPC para enviar mensagens entre o módulo da Análise Dinâmica de Vídeo e a extensão personalizada de IA ou CV.

O gRPC é uma moderna estrutura de RPC de software livre e alto desempenho que é executada em qualquer ambiente. O serviço de transporte do gRPC usa o streaming bidirecional HTTP/2 entre:

* o cliente gRPC (Análise Dinâmica de Vídeo no módulo do IoT Edge) e 
* o servidor gRPC (sua extensão personalizada).

Uma sessão gRPC é uma conexão individual do cliente gRPC com o servidor gRPC na porta TCP/TLS. 

Em uma só sessão: O cliente envia um descritor de fluxo de mídia seguido de quadros de vídeo ao servidor como uma mensagem [protobuf](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc) na sessão de fluxo gRPC. O servidor valida o descritor de fluxo, analisa o quadro de vídeo e retorna os resultados de inferência como uma mensagem protobuf.

![Contrato de extensão gRPC](./media/grpc-extension-protocol/grpc.png)

## <a name="implementing-grpc-protocol"></a>Como implementar o protocolo gRPC

### <a name="creating-a-grpc-connection"></a>Como criar uma conexão gRPC

A extensão personalizada precisa implementar o seguinte serviço gRPC:

```
service MediaGraphExtension {
  rpc ProcessMediaStream(stream MediaStreamMessage) returns (stream MediaStreamMessage);
}
```

Quando chamado, isso abrirá um fluxo bidirecional para o fluxo das mensagens entre a extensão gRPC e o grafo da Análise Dinâmica de Vídeo. A primeira mensagem enviada nesse fluxo pelas partes conterá um MediaStreamDescriptor, que define quais informações serão enviadas no MediaSamples a seguir.

Por exemplo, a extensão de grafo poderá enviar a mensagem (expressa aqui em JSON) para indicar que ela enviará quadros 416 x 416 codificados em RGB24 inseridos nas mensagens gRPC à extensão personalizada.

```
 {
    "sequence_number": 1,
    "ack_sequence_number": 0,
    "media_stream_descriptor": {
        "graph_identifier": {
            "media_services_arm_id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/microsoft.media/mediaservices/mediaAccountName",
            "graph_instance_name": "mediaGraphName",
            "graph_node_name": "grpcExtension"
        },
        "media_descriptor": {
            "timescale": 90000,
            "video_frame_sample_format": {
                "encoding": "RAW",
                "pixel_format": "RGB24",
                "dimensions": {
                    "width": 416,
                    "height": 416
                },
                "stride_bytes": 1248
            }
        }
    }
}
```

A extensão personalizada, em resposta, enviará a mensagem a seguir para indicar que ela está retornando apenas inferências.

```
{
    "sequence_number": 1,
    "ack_sequence_number": 1,
    "media_stream_descriptor": {
        "extension_identifier": "customExtensionName"    }
}
```

Agora que os dois lados trocaram descritores de mídia, a Análise Dinâmica de Vídeo começará a transmitir quadros para a extensão.

### <a name="sequence-numbers"></a>Números de sequência

O nó de extensão do gRPC e a extensão personalizada mantêm um conjunto separado de números de sequência, que são atribuídos às respectivas mensagens. Esses números de sequência devem aumentar de maneira monotônica começando com 1. `ack_sequence_number` poderá ser ignorado se nenhuma mensagem estiver sendo confirmada, o que pode ocorrer quando a primeira mensagem é enviada.

Uma solicitação precisará ser confirmada depois que a mensagem correspondente for totalmente processada. No caso de uma transferência de memória compartilhada, essa confirmação indica que o remetente pode liberar a memória compartilhada e que o destinatário não vai mais acessá-la. O remetente precisa manter qualquer memória compartilhada até que ela seja confirmada.

### <a name="reading-embedded-content"></a>Como ler o conteúdo inserido

O conteúdo inserido pode ser lido diretamente na mensagem `MediaSample` por meio do campo `content_byte`s. Os dados serão codificados de acordo com o `MediaDescriptor`.

### <a name="reading-content-from-shared-memory"></a>Como ler o conteúdo da memória compartilhada

Ao transferir o conteúdo por meio da memória compartilhada, o remetente incluirá `SharedMemoryBufferTransferProperties` no `MediaStreamDescriptor` ao estabelecer uma sessão pela primeira vez. Isso pode ser parecido com o seguinte (expresso em JSON):

```
{
  "handle_name": "inference_client_share_memory_2146989006636459346"
  "length_bytes": 20971520
}
```

Em seguida, o destinatário abrirá o arquivo `/dev/shm/inference_client_share_memory_2146989006636459346`. O remetente enviará uma mensagem `MediaSample`, contendo uma `ContentReference` que indica um local específico nesse arquivo.

```
{
    "timestamp": 143598615750000,
    "content_reference": {
        "address_offset": 519168,
        "length_bytes": 173056
    }
}
```

O destinatário então lerá os dados dessa localização no arquivo.

Para que o contêiner da Análise Dinâmica de Vídeo se comunique pela memória compartilhada, o modo IPC do contêiner precisa ser configurado corretamente. Isso pode ser feito de muitas maneiras, mas estas são algumas configurações recomendadas.

* Ao se comunicar com um mecanismo de inferência do gRPC em execução no dispositivo host, o modo IPC deve ser definido como o host.
* Ao se comunicar com um servidor gRPC em execução em outro módulo do IoT Edge, o modo IPC deve ser definido como compartilhável para o módulo da Análise Dinâmica de Vídeo e `container:liveVideoAnalytics` para a extensão personalizada, em que `liveVideoAnalytics` é o nome do módulo da Análise Dinâmica de Vídeo.

Veja a seguir a aparência disso no dispositivo gêmeo usando a primeira opção acima.

```
"liveVideoAnalytics": {
  "version": "1.0",
  "type": "docker",
  "status": "running",
  "restartPolicy": "always",
  "settings": {
    "image": "mcr.microsoft.com/media/live-video-analytics:1",
    "createOptions": 
      "HostConfig": {
        "IpcMode": "host"
      }
    }
  }
}
```

Para obter mais informações sobre os modos IPC, confira https://docs.docker.com/engine/reference/run/#ipc-settings---ipc.

## <a name="media-graph-grpc-extension-contract-definitions"></a>Definições do contrato de extensão gRPC do grafo de mídia

Esta seção define o contrato gRPC que define o fluxo de dados.

### <a name="protocol-messages"></a>Mensagens de protocolo

![Mensagens de protocolo](./media/grpc-extension-protocol/grpc2.png)
 
### <a name="client-authentication"></a>Autenticação de cliente

Os implementadores de extensões personalizadas podem validar a autenticidade das conexões gRPC de entrada para garantir que elas sejam provenientes do nó de extensão gRPC. O nó fornecerá uma entrada nos cabeçalhos de solicitação para validação.

As credenciais de nome de usuário/senha podem ser usadas para fazer isso. Durante a criação de um nó de extensão gRPC, as credenciais são fornecidas conforme mostrado abaixo:

```
{
  "@type": "#Microsoft.Media.MediaGraphGrpcExtension",
  "name": "{moduleIdentifier}",
  "endpoint": {
    "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
    "url": "tcp://customExtension:8081",
    "credentials": {
      "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
      "username": "username",
      "password": "password"
    }
  }
  // Other fields omitted
}
```

Quando a solicitação do gRPC for enviada, o cabeçalho a seguir será incluído nos metadados da solicitação, imitando a autenticação Básica HTTP.

`x-ms-authentication: Basic (Base64 Encoded username:password)`

## <a name="using-grpc-over-tls"></a>Como usar o gRPC via TLS

Uma conexão gRPC usada para inferência pode ser protegida via TLS. Isso é útil em situações em que não é possível garantir a segurança da rede entre a Análise Dinâmica de Vídeo e o mecanismo de inferência. O TLS criptografará qualquer conteúdo inserido nas mensagens do gRPC, causando sobrecarga adicional de CPU ao transmitir quadros a uma taxa alta.

As opções de verificação de IgnoreHostname e IgnoreSignature não são compatíveis com o gRPC e, portanto, o certificado do servidor, apresentado pelo mecanismo de inferência, precisa conter um CN que corresponda exatamente ao endereço IP/ao nome do host na URL do ponto de extremidade do nó de extensão gRPC.

## <a name="next-steps"></a>Próximas etapas

[Saiba mais sobre o esquema de metadados de inferência](inference-metadata-schema.md)
