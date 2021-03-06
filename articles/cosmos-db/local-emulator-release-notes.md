---
title: Download e notas de versão do emulador do Azure Cosmos
description: Obtenha as notas sobre a versão do emulador do Cosmos do Azure para diferentes versões e informações de download.
ms.service: cosmos-db
ms.topic: tutorial
author: milismsft
ms.author: adrianmi
ms.date: 06/20/2019
ms.openlocfilehash: 12e1c79e610526dec11467cc08c753bf90daa095
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86083450"
---
# <a name="azure-cosmos-emulator---release-notes-and-download-information"></a>Emulador do Azure Cosmos – Notas sobre a versão e informações de download

Este artigo mostra as notas de versão do emulador do Azure Cosmos com uma lista de atualizações de recursos que foram feitas em cada versão. Também lista a versão mais recente do emulador para baixar e usar.

## <a name="download"></a>Baixar

| | |
|---------|---------|
|**Download do MSI**|[Centro de Download da Microsoft](https://aka.ms/cosmosdb-emulator)|
|**Introdução**|[Desenvolver localmente com o emulador do Azure Cosmos](local-emulator.md)|

## <a name="release-notes"></a>Notas de versão

### <a name="2112-07072020"></a>2.11.2 (07/07/2020)

- Esta versão altera o modo de coleta dos rastreamentos ETL necessários na solução de problemas do emulador do Cosmos. As ferramentas do WPR (Runtime de Desempenho do Windows) agora são as ferramentas padrão para a captura de rastreamentos baseados em ETL, ao passo que a antiga captura baseada em LOGMAN foi preterida. Essa alteração é, em parte, necessária porque as últimas atualizações de segurança do Windows tiveram um impacto inesperado sobre o funcionamento do LOGMAN quando executado por meio do emulador do Cosmos.

### <a name="2111-06102020"></a>2.11.1 (10/06/2020)

- Esta versão corrige alguns bugs relacionados ao Data Explorer do emulador. Em determinados casos, ao usar o Data Explorer do emulador por meio de um navegador da Web, não será possível a conexão com o ponto de extremidade do emulador do Cosmos, e todas as ações relacionadas, como a criação de um banco de dados ou um contêiner, terão erros. O segundo problema corrigido está relacionado à criação de um item usando um arquivo JSON com a ação de upload do Data Explorer.

### <a name="2110"></a>2.11.0

- Esta versão introduz o suporte para taxa de transferência provisionada com dimensionamento automático. Esses novos recursos incluem a capacidade de definir um nível máximo personalizado de taxa de transferência provisionada em unidades de solicitação (RU/s), habilitar o dimensionamento automático em bancos de dados e contêineres existentes e suporte programático por meio de SDKs do Azure Cosmos DB.
- Correção de um problema ao consultar uma grande quantidade de documentos (mais de 1GB), em que o emulador falha com o código de status de erro interno 500.

### <a name="292"></a>2.9.2

- Esta versão corrige um bug ao habilitar o suporte para o ponto de extremidade do MongoDb versão 3.2. Ela também adiciona suporte para gerar rastreamentos de ETL para fins de solução de problemas usando WPR em vez de LOGMAN.

### <a name="291"></a>2.9.1

- Essa versão corrige alguns problemas no suporte à API de consulta e restaura a compatibilidade com sistemas operacionais mais antigos, como o Windows Server 2012.

### <a name="290"></a>2.9.0

- Esta versão adiciona a opção para definir a consistência com o prefixo consistente e aumentar os limites máximos para usuários e permissões.

### <a name="272"></a>2.7.2

- Esta versão adiciona suporte ao servidor do MongoDB versão 3.6 para o Emulador Cosmos. Para iniciar um ponto de extremidade do MongoDB que tenha como alvo a versão 3.6 do serviço, inicie o emulador de uma linha de comando do administrador com a opção "/EnableMongoDBEndpoint=3.6".

### <a name="270"></a>2.7.0

- Esta versão corrige uma regressão que impediu os usuários de executar consultas na conta da API do SQL no emulador ao usar clientes .NET com base em .NET Core ou x86.

### <a name="246"></a>2.4.6

- Esta versão fornece paridade com os recursos do serviço Azure Cosmos a partir de julho de 2019, com as exceções indicadas em [Desenvolvimento local com o emulador do Azure Cosmos](local-emulator.md). Ela também corrige vários bugs relacionados ao desligamento do emulador quando invocada pela linha de comando e substituições de endereço IP interno para clientes do SDK que usam a conectividade de modo direto.

### <a name="243"></a>2.4.3

- Inicialização do serviço MongoDB desabilitada por padrão. Somente o ponto de extremidade do SQL está habilitado por padrão. O usuário deve iniciar o ponto de extremidade manualmente usando a opção de linha de comando /EnableMongoDbEndpoint" do emulador. Agora, é como todos os outros pontos de extremidade, como Gremlin, Cassandra e tabela.
- Corrigido um bug no emulador ao iniciar com /AllowNetworkAccess", em que os pontos de extremidade de Gremlin, Cassandra e tabela não processavam corretamente as solicitações de clientes externos.
- Adicione portas de conexão direta às configurações de regras de firewall.

### <a name="240"></a>2.4.0

- Corrigido um problema de falha do emulador ao iniciar quando aplicativos de monitoramento de rede, como cliente do pulso, estão presentes no computador host.
