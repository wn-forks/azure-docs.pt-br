---
title: Instalar contêineres de fala-serviço de fala
titleSuffix: Azure Cognitive Services
description: Instale e execute contêineres de fala. A conversão de fala em texto transcreve, em tempo real, fluxos de áudio em texto que seus aplicativos, ferramentas ou dispositivos podem consumir ou exibir. A conversão de texto em fala converte o texto de entrada em uma fala sintetizada semelhante à humana.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: aahi
ms.openlocfilehash: b242530b09f399a84f10a40ea35e21c1119f52b1
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89320997"
---
# <a name="install-and-run-speech-service-containers-preview"></a>Instalar e executar contêineres do serviço de fala (versão prévia)

Os contêineres permitem executar algumas das APIs de serviço de Fala no seu ambiente. Contêineres são excelentes para especificar requisitos de segurança e governança de dados. Neste artigo, você aprenderá a baixar, instalar e executar um contêiner de Fala.

Os contêineres de Fala permitem que os clientes criem uma arquitetura de aplicativo de fala otimizada para capacidades robustas de nuvem e localidade de borda. Há cinco contêineres diferentes disponíveis. Os dois contêineres padrão são Text **-to-Text**e conversão de **texto em fala**. Os dois contêineres personalizados são **fala personalizada para texto** e **texto em fala personalizado**. O **texto para fala neural** também fornece mais declarações natural, por meio do uso de um modelo mais avançado. Os contêineres de fala têm o mesmo [preço](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) que os serviços de fala do Azure baseados em nuvem.

> [!IMPORTANT]
> Atualmente, todos os contêineres de fala são oferecidos como parte de uma [Visualização pública "restrita"](../cognitive-services-container-support.md#container-availability-in-azure-cognitive-services). Um comunicado será feito quando os contêineres de fala progredirem para disponibilidade geral (GA).

| Função | Recursos | Última |
|--|--|--|
| Conversão de fala em texto | Analisa sentimentos e transcreve gravações contínuas em tempo real ou de áudio em lotes com resultados intermediários.  | 2.4.0 |
| Fala Personalizada para texto | Usar um modelo personalizado do [portal de fala personalizada](https://speech.microsoft.com/customspeech), transcreve gravações contínuas em tempo real ou de áudio em lotes em texto com resultados intermediários. | 2.4.0 |
| Conversão de texto em fala | Converte texto em fala natural-som com entrada de texto sem formatação ou SSML (linguagem de marcação de síntese de fala). | 1.6.0 |
| Conversão de texto em fala personalizada | Usando um modelo personalizado do [portal de voz personalizado](https://aka.ms/custom-voice-portal), o converte o texto em fala de som natural com entrada de texto sem formatação ou SSML (linguagem de marcação de síntese de fala). | 1.6.0 |
| Texto em fala neural | Converte o texto em voz natural usando uma tecnologia de rede neural profunda, permitindo uma fala mais natural sintetizada. | 1.1.0 |

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/cognitive-services/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos antes de usar os contêineres de fala:

| Obrigatório | Finalidade |
|--|--|
| Mecanismo do Docker | É necessário ter o Mecanismo Docker instalado em um [computador host](#the-host-computer). O Docker fornece pacotes que configuram o ambiente do Docker no [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) e [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para instruções sobre conceitos básicos do Docker e de contêiner, consulte a [visão geral do Docker](https://docs.docker.com/engine/docker-overview/).<br><br> O Docker deve ser configurado para permitir que os contêineres conectem-se e enviem dados de cobrança para o Azure. <br><br> **No Windows**, o Docker também deve ser configurado para dar suporte a contêineres do Linux.<br><br> |
| Familiaridade com o Docker | É necessário ter uma compreensão básica de conceitos do Docker, como registros, repositórios, contêineres e imagens de contêiner, bem como conhecimento dos comandos básicos do `docker`. |
| Recurso de fala | Para usar esses contêineres, será necessário ter:<br><br>Um recurso de _fala_ do Azure para obter a chave de API e o URI de ponto de extremidade associados. Ambos os valores estão disponíveis nas páginas visão geral de **fala** e chaves de portal do Azure. Eles são necessários para iniciar o contêiner.<br><br>**{Api_key}**: uma das duas chaves de recurso disponíveis na página **chaves**<br><br>**{ENDPOINT_URI}**: o ponto de extremidade conforme fornecido na página **visão geral** |


## <a name="request-access-to-the-container-registry"></a>Solicitar acesso ao Registro de contêiner

Preencha e envie o [formulário de solicitação](https://aka.ms/cognitivegate) para solicitar acesso ao contêiner. 


[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>O computador host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Suporte à extensão de vetor avançado

O **host** é o computador que executa o contêiner do Docker. O host *deve dar suporte* a [extensões de vetor avançadas](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2). Você pode verificar o suporte a AVX2 em hosts Linux com o seguinte comando:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```
> [!WARNING]
> O computador host é *necessário* para dar suporte a AVX2. O contêiner *não* funcionará corretamente sem o suporte do AVX2.

### <a name="container-requirements-and-recommendations"></a>Recomendações e requisitos do contêiner

A tabela a seguir descreve a alocação mínima e recomendada de recursos para cada contêiner de fala.

| Contêiner | Mínimo | Recomendadas |
|-----------|---------|-------------|
| Conversão de fala em texto | 2 núcleos, 2 GB de memória | 4 núcleos, 4 GB de memória |
| Fala Personalizada para texto | 2 núcleos, 2 GB de memória | 4 núcleos, 4 GB de memória |
| Conversão de texto em fala | 1 núcleo, 2 GB de memória | 2 núcleos, 3 GB de memória |
| Conversão de texto em fala personalizada | 1 núcleo, 2 GB de memória | 2 núcleos, 3 GB de memória |
| Texto em fala neural | 6 núcleos, 12 GB de memória | 8 núcleos, 16 GB de memória |

* Cada núcleo precisa ser de pelo menos 2,6 GHz (gigahertz) ou mais rápido.

Memória e núcleo correspondem às configurações `--cpus` e `--memory`, que são usadas como parte do comando `docker run`.

> [!NOTE]
> O mínimo e recomendado são baseados nos limites do Docker, *não* nos recursos da máquina host. Por exemplo, os contêineres de conversão de texto na memória mapeiam partes de um modelo de linguagem grande e é *recomendável* que todo o arquivo caiba na memória, que é de 4-6 GB adicionais. Além disso, a primeira execução de qualquer um dos contêineres pode levar mais tempo, pois os modelos estão sendo paginados na memória.

## <a name="get-the-container-image-with-docker-pull"></a>Obter a imagem de contêiner com `docker pull`

As imagens de contêiner para fala estão disponíveis no registro de contêiner a seguir.

# <a name="speech-to-text"></a>[Conversão de fala em texto](#tab/stt)

| Contêiner | Repositório |
|-----------|------------|
| Conversão de fala em texto | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |

# <a name="custom-speech-to-text"></a>[Fala Personalizada para texto](#tab/cstt)

| Contêiner | Repositório |
|-----------|------------|
| Fala Personalizada para texto | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest` |

# <a name="text-to-speech"></a>[Conversão de texto em fala](#tab/tts)

| Contêiner | Repositório |
|-----------|------------|
| Conversão de texto em fala | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

# <a name="neural-text-to-speech"></a>[Texto em fala neural](#tab/ntts)

| Contêiner | Repositório |
|-----------|------------|
| Texto em fala neural | `containerpreview.azurecr.io/microsoft/cognitive-services-neural-text-to-speech:latest` |

# <a name="custom-text-to-speech"></a>[Conversão de texto em fala personalizada](#tab/ctts)

| Contêiner | Repositório |
|-----------|------------|
| Conversão de texto em fala personalizada | `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest` |

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-speech-containers"></a>Pull do Docker para os contêineres de fala

# <a name="speech-to-text"></a>[Conversão de fala em texto](#tab/stt)

#### <a name="docker-pull-for-the-speech-to-text-container"></a>Pull do Docker para o contêiner de conversão de fala em texto

Use o comando [Docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para baixar uma imagem de contêiner do registro de visualização de contêiner.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

> [!IMPORTANT]
> A `latest` marca extrai a `en-US` localidade. Para localidades adicionais, consulte [localidades de fala para texto](#speech-to-text-locales).

#### <a name="speech-to-text-locales"></a>Localidades de conversão de fala em texto

Todas as marcas, exceto `latest` as estão no seguinte formato e diferenciam maiúsculas de minúsculas:

```
<major>.<minor>.<patch>-<platform>-<locale>-<prerelease>
```

A marca a seguir é um exemplo do formato:

```
2.4.0-amd64-en-us-preview
```

Para todas as localidades com suporte do contêiner de **fala a texto** , consulte [marcas de imagem de fala para texto](../containers/container-image-tags.md#speech-to-text).

# <a name="custom-speech-to-text"></a>[Fala Personalizada para texto](#tab/cstt)

#### <a name="docker-pull-for-the-custom-speech-to-text-container"></a>Pull do Docker para o contêiner de Fala Personalizada para texto

Use o comando [Docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para baixar uma imagem de contêiner do registro de visualização de contêiner.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text:latest
```

> [!NOTE]
> O `locale` e o `voice` para contêineres de fala personalizados são determinados pelo modelo personalizado ingerido pelo contêiner.

# <a name="text-to-speech"></a>[Conversão de texto em fala](#tab/tts)

#### <a name="docker-pull-for-the-text-to-speech-container"></a>Pull do Docker para o contêiner de conversão de texto em fala

Use o comando [Docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para baixar uma imagem de contêiner do registro de visualização de contêiner.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

> [!IMPORTANT]
> A `latest` marca extrai a `en-US` localidade e a `ariarus` voz. Para localidades adicionais, consulte [localidades de conversão de texto em fala](#text-to-speech-locales).

#### <a name="text-to-speech-locales"></a>Localidades de conversão de texto em fala

Todas as marcas, exceto `latest` as estão no seguinte formato e diferenciam maiúsculas de minúsculas:

```
<major>.<minor>.<patch>-<platform>-<locale>-<voice>-<prerelease>
```

A marca a seguir é um exemplo do formato:

```
1.6.0-amd64-en-us-ariarus-preview
```

Para todas as localidades com suporte e as vozes correspondentes do contêiner de **conversão de texto em fala** , confira [marcas de imagem de texto em fala](../containers/container-image-tags.md#text-to-speech).

> [!IMPORTANT]
> Ao construir um HTTP POST de *conversão de texto em fala* , a mensagem de [linguagem de marcação de síntese de fala (SSML)](speech-synthesis-markup.md) requer um `voice` elemento com um `name` atributo. O valor é a localidade de contêiner correspondente e voz, também conhecido como ["nome curto"](language-support.md#standard-voices). Por exemplo, a `latest` marca teria um nome de voz de `en-US-AriaRUS` .

# <a name="neural-text-to-speech"></a>[Texto em fala neural](#tab/ntts)

#### <a name="docker-pull-for-the-neural-text-to-speech-container"></a>Pull do Docker para o contêiner de conversão de texto em fala neural

Use o comando [Docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para baixar uma imagem de contêiner do registro de visualização de contêiner.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-neural-text-to-speech:latest
```

> [!IMPORTANT]
> A `latest` marca extrai a `en-US` localidade e a `arianeural` voz. Para localidades adicionais, consulte [localidades de conversão de texto em fala neural](#neural-text-to-speech-locales).

#### <a name="neural-text-to-speech-locales"></a>Localidades de conversão de texto em fala neural

Todas as marcas, exceto `latest` as estão no seguinte formato e diferenciam maiúsculas de minúsculas:

```
<major>.<minor>.<patch>-<platform>-<locale>-<voice>-<prerelease>
```

A marca a seguir é um exemplo do formato:

```
1.1.0-amd64-en-us-arianeural-preview
```

Para todas as localidades com suporte e as vozes correspondentes do contêiner de **conversão de texto em fala neural** , consulte [marcas de imagem de conversão de texto em fala neural](../containers/container-image-tags.md#neural-text-to-speech).

> [!IMPORTANT]
> Ao construir um HTTP POST de *conversão de texto em fala neural* , a mensagem de [linguagem de marcação de síntese de fala (SSML)](speech-synthesis-markup.md) requer um `voice` elemento com um `name` atributo. O valor é a localidade de contêiner correspondente e voz, também conhecido como ["nome curto"](language-support.md#neural-voices). Por exemplo, a `latest` marca teria um nome de voz de `en-US-AriaNeural` .

# <a name="custom-text-to-speech"></a>[Conversão de texto em fala personalizada](#tab/ctts)

#### <a name="docker-pull-for-the-custom-text-to-speech-container"></a>Pull do Docker para o contêiner personalizado de conversão de texto em fala

Use o comando [Docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para baixar uma imagem de contêiner do registro de visualização de contêiner.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech:latest
```

> [!NOTE]
> O `locale` e o `voice` para contêineres de fala personalizados são determinados pelo modelo personalizado ingerido pelo contêiner.

***

## <a name="how-to-use-the-container"></a>Como usar o contêiner

Depois que o contêiner estiver no [computador host](#the-host-computer), use o processo a seguir para trabalhar com o contêiner.

1. [Execute o contêiner](#run-the-container-with-docker-run) com as configurações de cobrança necessárias. Há outros [exemplos](speech-container-configuration.md#example-docker-run-commands) do comando `docker run` disponíveis.
1. [Consulte o ponto de extremidade de previsão do contêiner](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Executar o contêiner com `docker run`

Use o comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para executar o contêiner. Consulte [coletando parâmetros necessários](#gathering-required-parameters) para obter detalhes sobre como obter os `{Endpoint_URI}` `{API_Key}` valores e. [Exemplos](speech-container-configuration.md#example-docker-run-commands) adicionais do `docker run` comando também estão disponíveis.

# <a name="speech-to-text"></a>[Conversão de fala em texto](#tab/stt)

Para executar o contêiner padrão de *fala em texto* , execute o comando a seguir `docker run` .

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa um contêiner de *conversão de fala em texto* da imagem de contêiner.
* Aloca 4 núcleos de CPU e 4 gigabytes (GB) de memória.
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.


#### <a name="analyze-sentiment-on-the-speech-to-text-output"></a>Analisar o sentimentos na saída de fala para texto 

A partir do v 2.2.0 do contêiner de fala a texto, você pode chamar a [API da análise de opiniões v3](../text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md) na saída. Para chamar a análise de sentimentos, você precisará de um ponto de extremidade de recurso API de Análise de Texto. Por exemplo: 
* `https://westus2.api.cognitive.microsoft.com/text/analytics/v3.0-preview.1/sentiment`
* `https://localhost:5000/text/analytics/v3.0-preview.1/sentiment`

Se você estiver acessando um ponto de extremidade de análise de texto na nuvem, será necessário uma chave. Se você estiver executando Análise de Texto localmente, talvez não seja necessário fornecer isso.

A chave e o ponto de extremidade são passados para o contêiner de fala como argumentos, como no exemplo a seguir.

```bash
docker run -it --rm -p 5000:5000 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY} \
CloudAI:SentimentAnalysisSettings:TextAnalyticsHost={TEXT_ANALYTICS_HOST} \
CloudAI:SentimentAnalysisSettings:SentimentAnalysisApiKey={SENTIMENT_APIKEY}
```

Esse comando:

* Executa as mesmas etapas do comando acima.
* Armazena um ponto de extremidade de API de Análise de Texto e uma chave para enviar solicitações de análise de sentimentos. 


# <a name="custom-speech-to-text"></a>[Fala Personalizada para texto](#tab/cstt)

O contêiner de *fala personalizada para texto* depende de um modelo de fala personalizado. O modelo personalizado deve ter sido [treinado](how-to-custom-speech-train-model.md) usando o [portal de fala personalizado](https://speech.microsoft.com/customspeech).

> [!IMPORTANT]
> O modelo de Fala Personalizada precisa ser treinado a partir de uma das seguintes versões de modelo:
> * **20181201 (v 3.3 unificado)**
> * **20190520 (v 4.14 unificado)**
> * **20190701 (v 4.17 unificado)**<br>
> ![Modelo de contêiner do Fala Personalizada Train](media/custom-speech/custom-speech-train-model-container-scoped.png)

A ID do **modelo** de fala personalizado é necessária para executar o contêiner. Ele pode ser encontrado na página de **treinamento** do portal de fala personalizado. No portal de fala personalizado, navegue até a página de **treinamento** e selecione o modelo.
<br>

![Página de treinamento de fala personalizada](media/custom-speech/custom-speech-model-training.png)

Obtenha a **ID do modelo** para usar como o argumento para o `ModelId` parâmetro do `docker run` comando.
<br>

![Detalhes do modelo de fala personalizado](media/custom-speech/custom-speech-model-details.png)

A tabela a seguir representa os vários `docker run` parâmetros e suas descrições correspondentes:

| Parâmetro | Descrição |
|---------|---------|
| `{VOLUME_MOUNT}` | A montagem de [volume](https://docs.docker.com/storage/volumes/)do computador host, que o Docker usa para manter o modelo personalizado. Por exemplo, *C:\CustomSpeech* onde a *unidade C* está localizada no computador host. |
| `{MODEL_ID}` | A **ID do modelo** de fala personalizada da página de **treinamento** do portal de fala personalizado. |
| `{ENDPOINT_URI}` | O ponto de extremidade é necessário para medição e cobrança. Para obter mais informações, consulte [coletando parâmetros necessários](#gathering-required-parameters). |
| `{API_KEY}` | A chave de API é necessária. Para obter mais informações, consulte [coletando parâmetros necessários](#gathering-required-parameters). |

Para executar o contêiner de *fala personalizada para texto* , execute o seguinte `docker run` comando:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 4 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa um contêiner de *fala personalizada para texto* a partir da imagem de contêiner.
* Aloca 4 núcleos de CPU e 4 gigabytes (GB) de memória.
* Carrega o modelo de *fala personalizada para texto* da montagem de entrada de volume, por exemplo, *C:\CustomSpeech*.
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner.
* Baixa o modelo de acordo `ModelId` com o (se não for encontrado na montagem do volume).
* Se o modelo personalizado tiver sido baixado anteriormente, o `ModelId` será ignorado.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.

# <a name="text-to-speech"></a>[Conversão de texto em fala](#tab/tts)

Para executar o contêiner padrão de *conversão de texto em fala* , execute o `docker run` comando a seguir.

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa um contêiner de *conversão de texto em fala* padrão da imagem de contêiner.
* Aloca 1 núcleo de CPU e 2 gigabytes (GB) de memória.
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.

# <a name="neural-text-to-speech"></a>[Texto em fala neural](#tab/ntts)

Para executar o contêiner de *conversão de texto em fala neural* , execute o comando a seguir `docker run` .

```bash
docker run --rm -it -p 5000:5000 --memory 12g --cpus 6 \
containerpreview.azurecr.io/microsoft/cognitive-services-neural-text-to-speech \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa um contêiner de *conversão de texto em fala neural* da imagem de contêiner.
* Aloca 6 núcleos de CPU e 12 gigabytes (GB) de memória.
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.

# <a name="custom-text-to-speech"></a>[Conversão de texto em fala personalizada](#tab/ctts)

O contêiner de *texto em fala personalizado* depende de um modelo de voz personalizado. O modelo personalizado deve ter sido [treinado](how-to-custom-voice-create-voice.md) usando o [portal de voz personalizado](https://aka.ms/custom-voice-portal). A ID do **modelo** de voz personalizado é necessária para executar o contêiner. Ele pode ser encontrado na página de **treinamento** do portal de voz personalizado. No portal de voz personalizado, navegue até a página de **treinamento** e selecione o modelo.
<br>

![Página de treinamento de voz personalizada](media/custom-voice/custom-voice-model-training.png)

Obtenha a **ID do modelo** para usar como o argumento para o `ModelId` parâmetro do comando de execução do Docker.
<br>

![Detalhes do modelo de voz personalizado](media/custom-voice/custom-voice-model-details.png)

A tabela a seguir representa os vários `docker run` parâmetros e suas descrições correspondentes:

| Parâmetro | Descrição |
|---------|---------|
| `{VOLUME_MOUNT}` | A montagem de [volume](https://docs.docker.com/storage/volumes/)do computador host, que o Docker usa para manter o modelo personalizado. Por exemplo, *C:\CustomSpeech* onde a *unidade C* está localizada no computador host. |
| `{MODEL_ID}` | A **ID do modelo** de fala personalizada da página de **treinamento** do portal de voz personalizado. |
| `{ENDPOINT_URI}` | O ponto de extremidade é necessário para medição e cobrança. Para obter mais informações, consulte [coletando parâmetros necessários](#gathering-required-parameters). |
| `{API_KEY}` | A chave de API é necessária. Para obter mais informações, consulte [coletando parâmetros necessários](#gathering-required-parameters). |

Para executar o contêiner de *texto em fala personalizado* , execute o seguinte `docker run` comando:

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
-v {VOLUME_MOUNT}:/usr/local/models \
containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech \
ModelId={MODEL_ID} \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa um contêiner de *conversão de texto em fala personalizado* da imagem de contêiner.
* Aloca 1 núcleo de CPU e 2 gigabytes (GB) de memória.
* Carrega o modelo de *conversão de texto em fala personalizado* da montagem de entrada de volume, por exemplo, *C:\CustomVoice*.
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner.
* Baixa o modelo de acordo `ModelId` com o (se não for encontrado na montagem do volume).
* Se o modelo personalizado tiver sido baixado anteriormente, o `ModelId` será ignorado.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.

***

> [!IMPORTANT]
> As opções `Eula`, `Billing` e `ApiKey` devem ser especificadas para executar o contêiner; caso contrário, o contêiner não será iniciado.  Para mais informações, consulte [Faturamento](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Consultar o ponto de extremidade de previsão do contêiner

> [!NOTE]
> Use um número de porta exclusivo se você estiver executando vários contêineres.

| Contêineres | URL do host do SDK | Protocolo |
|--|--|--|
| Padrão de fala-para-texto e Fala Personalizada para texto | `ws://localhost:5000` | WS |
| Conversão de texto em fala (incluindo Standard, personalizada e neural) | `http://localhost:5000` | HTTP |

Para obter mais informações sobre como usar os protocolos do WSS e HTTPS, consulte [segurança do contêiner](../cognitive-services-container-support.md#azure-cognitive-services-container-security).

### <a name="speech-to-text-standard-and-custom"></a>Conversão de fala em texto (padrão e personalizada)

[!INCLUDE [Query Speech-to-text container endpoint](includes/speech-to-text-container-query-endpoint.md)]

#### <a name="analyze-sentiment"></a>Analisar sentimento

Se você forneceu suas credenciais de API de Análise de Texto [para o contêiner](#analyze-sentiment-on-the-speech-to-text-output), você pode usar o SDK de fala para enviar solicitações de reconhecimento de fala com análise de sentimentos. Você pode configurar as respostas da API para usar um formato *simples* ou *detalhado* .
> [!NOTE]
> o v 1.13 do SDK do Python do serviço de fala tem um problema identificado com a análise de sentimentos. Use v 1.12. x ou anterior se você estiver usando análise de sentimentos no SDK do Python do serviço de fala.

# <a name="simple-format"></a>[Formato simples](#tab/simple-format)

Para configurar o cliente de fala para usar um formato simples, adicione `"Sentiment"` como um valor para `Simple.Extensions` . Se você quiser escolher uma versão específica do modelo de Análise de Texto, substitua `'latest'` na `speechcontext-phraseDetection.sentimentAnalysis.modelversion` configuração da propriedade.

```python
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Simple.Extensions',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentAnalysis.modelversion',
    value='latest',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

`Simple.Extensions` retornará o resultado de sentimentos na camada raiz da resposta.

```json
{
   "DisplayText":"What's the weather like?",
   "Duration":13000000,
   "Id":"6098574b79434bd4849fee7e0a50f22e",
   "Offset":4700000,
   "RecognitionStatus":"Success",
   "Sentiment":{
      "Negative":0.03,
      "Neutral":0.79,
      "Positive":0.18
   }
}
```

# <a name="detailed-format"></a>[Formato detalhado](#tab/detailed-format)

Para configurar o cliente de fala para usar um formato detalhado, adicione `"Sentiment"` como um valor para `Detailed.Extensions` , `Detailed.Options` ou ambos. Se você quiser escolher uma versão específica do modelo de Análise de Texto, substitua `'latest'` na `speechcontext-phraseDetection.sentimentAnalysis.modelversion` configuração da propriedade.

```python
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Detailed.Options',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-PhraseOutput.Detailed.Extensions',
    value='["Sentiment"]',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentAnalysis.modelversion',
    value='latest',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

`Detailed.Extensions` fornece um resultado de sentimentos na camada raiz da resposta. `Detailed.Options` fornece o resultado na `NBest` camada da resposta. Eles podem ser usados separadamente ou em conjunto.

```json
{
   "DisplayText":"What's the weather like?",
   "Duration":13000000,
   "Id":"6a2aac009b9743d8a47794f3e81f7963",
   "NBest":[
      {
         "Confidence":0.973695,
         "Display":"What's the weather like?",
         "ITN":"what's the weather like",
         "Lexical":"what's the weather like",
         "MaskedITN":"What's the weather like",
         "Sentiment":{
            "Negative":0.03,
            "Neutral":0.79,
            "Positive":0.18
         }
      },
      {
         "Confidence":0.9164971,
         "Display":"What is the weather like?",
         "ITN":"what is the weather like",
         "Lexical":"what is the weather like",
         "MaskedITN":"What is the weather like",
         "Sentiment":{
            "Negative":0.02,
            "Neutral":0.88,
            "Positive":0.1
         }
      }
   ],
   "Offset":4700000,
   "RecognitionStatus":"Success",
   "Sentiment":{
      "Negative":0.03,
      "Neutral":0.79,
      "Positive":0.18
   }
}
```

---

Se você quiser desabilitar completamente a análise de sentimentos, adicione um `false` valor a `sentimentanalysis.enabled` .

```python
speech_config.set_service_property(
    name='speechcontext-phraseDetection.sentimentanalysis.enabled',
    value='false',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

### <a name="text-to-speech-standard-neural-and-custom"></a>Conversão de texto em fala (padrão, neural e personalizada)

[!INCLUDE [Query Text-to-speech container endpoint](includes/text-to-speech-container-query-endpoint.md)]

### <a name="run-multiple-containers-on-the-same-host"></a>Executar vários contêineres no mesmo host

Se você pretende executar vários contêineres com portas expostas, execute cada um deles com uma porta exposta diferente. Por exemplo, execute o primeiro contêiner na porta 5000 e o segundo contêiner na porta 5001.

É possível ter esse contêiner e um contêiner dos Serviços Cognitivos do Azure em execução no HOST juntos. Também é possível ter vários contêineres do mesmo contêiner dos Serviços Cognitivos em execução.

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Parar o contêiner

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Solução de problemas

Ao iniciar ou executar o contêiner, você poderá ter problemas. Use uma [montagem](speech-container-configuration.md#mount-settings) de saída e habilite o registro em log. Isso permitirá que o contêiner gere arquivos de log que são úteis ao solucionar problemas.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Cobrança

Os contêineres de fala enviam informações de cobrança para o Azure, usando um recurso de *fala* em sua conta do Azure.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obter mais informações sobre essas opções, consulte [Configurar contêineres](speech-container-configuration.md).

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Resumo

Neste artigo, você aprendeu os conceitos e o fluxo de trabalho para baixar, instalar e executar contêineres de fala. Em resumo:

* A fala fornece quatro contêineres do Linux para o Docker, encapsulando vários recursos:
  * *Conversão de fala em texto*
  * *Fala Personalizada para texto*
  * *Conversão de texto em fala*
  * *Conversão de texto em fala personalizada*
  * *Texto em fala neural*
* As imagens de contêiner são baixadas do registro de contêiner no Azure.
* Imagens de contêiner são executadas no Docker.
* Se estiver usando a API REST (somente conversão de texto em fala) ou o SDK (fala-para-texto ou conversão de texto em fala), especifique o URI do host do contêiner. 
* Você precisa fornecer informações de cobrança ao criar uma instância de um contêiner.

> [!IMPORTANT]
>  Os contêineres dos Serviços Cognitivos não estão licenciados para execução sem estarem conectados ao Azure para medição. Os clientes precisam ativar os contêineres para comunicar informações de cobrança com o serviço de medição em todos os momentos. Os contêineres de Serviços Cognitivos não enviam dados do cliente (por exemplo, a imagem ou o texto que está sendo analisado) para a Microsoft.

## <a name="next-steps"></a>Próximas etapas

* Examinar [definir contêineres](speech-container-configuration.md) para definições de configuração
* Saiba como [usar contêineres de serviço de fala com kubernetes e Helm](speech-container-howto-on-premises.md)
* Usar mais [contêineres de serviços cognitivas](../cognitive-services-container-support.md)
