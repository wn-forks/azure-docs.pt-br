---
title: Localizar exemplos de malha de Service Fabric do Azure
description: Aqui está um índice de aplicativos de exemplo de malha Service Fabric disponíveis. O código-fonte nesses exemplos mostra como obter um cenário específico usando o modelo de recursos do Service Fabric.
ms.date: 12/03/2018
ms.topic: conceptual
ms.openlocfilehash: 64d9e4e112f26f83eedd02b034c8191c6f677618
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261268"
---
# <a name="find-service-fabric-mesh-samples"></a>Localizar Exemplos da Malha do Service Fabric

Essa tabela descreve os aplicativos de exemplo da malha do Service Fabric disponíveis. O código-fonte nesses exemplos mostra como obter um cenário específico usando o modelo de recursos do Service Fabric.

Para obter mais informações sobre como implantar modelos diretamente no Azure, confira a [Página de modelo de exemplo do GitHub.](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md)

|Modelo de Exemplo|Descrição do cenário|Código-fonte|Ferramentas para Desenvolvedores|
|------------|--------------------|----------|----------------------|
| [Aplicativo Olá, Mundo](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Página da Web estática hospedada em um contêiner. Para Linux, ele usa nginx, para o Windows, IIS | [Código-fonte](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Não há requisitos |
| [Aplicativo de contador para Volumes de arquivos do Azure](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.md) | Armazene o estado ao montar o volume baseado em arquivos do Azure dentro do contêiner. <br><br> **Observação:** esse modelo requer que um compartilhamento dos Arquivos do Azure já seja provisionado [Instruções](../storage/files/storage-how-to-create-file-share.md) | [Código-fonte](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Ferramentas da Malha do Visual Studio |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolist) | Crie um aplicativo com um serviço de front-end e back-end que use a resolução baseada em DNS. Usado como um tutorial [aqui](./service-fabric-mesh-tutorial-create-dotnetcore.md) | [Código-fonte](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Ferramentas da Malha do Visual Studio |
| [Aplicativo de Objetos Visuais](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Microsserviços de dimensionamento e atualização dentro de um aplicativo. | [Código-fonte](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Ferramentas da Malha do Visual Studio |
| [Aplicativo de votação](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/voting) | Criar um aplicativo com um serviço de front-end e back-end que use a resolução baseada em DNS | [Código-fonte](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Ferramentas de malha do Visual Studio para a versão do Windows, a CLI do VS Code/dotnet pode ser usada para a versão do Linux |
| [Aplicativo de Contador para Volumes Confiáveis do Service Fabric](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter/readme.sfreliablevolume.md)| Armazene o estado ao montar o volume baseado em disco confiável do Service Fabric dentro do contêiner.| [Código-fonte](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Ferramentas da Malha do Visual Studio |
