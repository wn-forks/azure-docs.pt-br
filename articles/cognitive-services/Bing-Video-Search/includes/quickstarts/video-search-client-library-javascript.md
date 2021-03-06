---
title: Guia de início rápido da biblioteca de clientes JavaScript da Pesquisa de Vídeo do Bing
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.custom: devx-track-javascript
ms.openlocfilehash: 7cf28df4f009b017699c926d1ca54b7e5320a179
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87404100"
---
Use este guia de início rápido para começar a pesquisar notícias com a biblioteca de clientes da Pesquisa de Vídeo do Bing para o JavaScript. Embora a Pesquisa de Vídeo do Bing tenha uma API REST compatível com a maioria das linguagens de programação, a biblioteca de clientes é uma forma fácil de integrar o serviço aos seus aplicativos. O código-fonte desta amostra pode ser encontrado no [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Ele contém mais recursos e anotações.

## <a name="prerequisites"></a>Pré-requisitos

- [Node.js](https://www.nodejs.org/)

Para configurar um aplicativo de console usando a biblioteca de clientes da Pesquisa de Vídeo do Bing:
* Execute `npm install ms-rest-azure` no seu ambiente de desenvolvimento.
* Execute `npm install azure-cognitiveservices-videosearch` no seu ambiente de desenvolvimento.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

1. Crie um arquivo JavaScript em seu IDE ou editor favorito e adicione uma instrução `require()` à biblioteca de clientes da Pesquisa de Vídeo do Bing e o módulo `CognitiveServicesCredentials`. Criar uma variável para a chave de assinatura. 
    
    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
    ```

2. Crie uma instância de `CognitiveServicesCredentials` com sua chave. Em seguida, use-a para criar uma instância do cliente de pesquisa de vídeo.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Enviar a solicitação de pesquisa

1. Use `client.videosOperations.search()` para enviar uma solicitação de pesquisa para a API de Pesquisa de Vídeo do Bing. Quando os resultados da pesquisa forem retornados, use `.then()` para registrar o resultado em log.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de página única](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Confira também 

* [O que é a API da Pesquisa de Vídeo do Bing?](../../overview.md)
* [Exemplos de SDK do .NET dos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)