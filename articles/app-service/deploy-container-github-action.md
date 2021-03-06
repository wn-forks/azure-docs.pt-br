---
title: CI/CD personalizado de ações do GitHub
description: Saiba como usar as ações do GitHub para implantar seu contêiner personalizado do Linux no serviço de aplicativo de um pipeline de CI/CD.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: 6af23aba28ce3cda9982878ed08ec515aa25633a
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962597"
---
# <a name="deploy-a-custom-container-to-app-service-using-github-actions"></a>Implantar um contêiner personalizado no serviço de aplicativo usando as ações do GitHub

O [GitHub Actions](https://help.github.com/en/articles/about-github-actions) oferece a flexibilidade para criar um fluxo de trabalho do ciclo de vida de desenvolvimento de software automatizado. Com a [ação de serviço Azure app para contêineres](https://github.com/Azure/webapps-container-deploy), você pode automatizar seu fluxo de trabalho para implantar o [serviço de aplicativo](overview.md) contêineres personalizados usando ações do github.

> [!IMPORTANT]
> No momento, o GitHub Actions está na versão beta. Primeiro, [inscreva-se para ingressar na versão prévia](https://github.com/features/actions) usando sua conta do GitHub.
> 

Um fluxo de trabalho é definido por um arquivo YAML (.yml) no caminho `/.github/workflows/` no repositório. Essa definição contém as várias etapas e os parâmetros que compõem o fluxo de trabalho.

Para um fluxo de trabalho de contêiner de serviço Azure App, o arquivo tem três seções:

|Seção  |Tarefas  |
|---------|---------|
|**Autenticação** | 1. definir uma entidade de serviço. <br /> 2. Crie um segredo do GitHub. |
|**Compilar** | 1. configurar o ambiente. <br /> 2. Crie a imagem de contêiner. |
|**Implantar** | 1. implante a imagem de contêiner. |

## <a name="create-a-service-principal"></a>Criar uma entidade de serviço

Crie uma [entidade de serviço](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) usando o comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) na [CLI do Azure](/cli/azure/). Execute esse comando com o [Azure Cloud Shell](https://shell.azure.com/) no portal do Azure ou selecionando o botão **Experimentar**.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name> \
                            --sdk-auth
```

No exemplo acima, substitua os espaços reservados por sua ID de assinatura e o nome do grupo de recursos. A saída é um objeto JSON com as credenciais de atribuição de função que fornecem acesso ao seu aplicativo do serviço de aplicativo semelhante ao mostrado abaixo. Copie este objeto JSON para mais tarde.

```output 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

> [!IMPORTANT]
> É sempre uma boa prática permitir acesso mínimo. Você pode restringir o escopo no comando AZ CLI acima para o aplicativo do serviço de aplicativo específico e o registro de contêiner do Azure no qual as imagens de contêiner são enviadas por push.

## <a name="configure-the-github-secret"></a>Configurar o segredo do GitHub

No [GitHub](https://github.com/), procure seu repositório, selecione **configurações > segredos > adicionar um novo segredo**.

Cole o conteúdo da saída JSON de [criar uma entidade de serviço](#create-a-service-principal) como o valor da variável secreta. Dê ao segredo o nome como `AZURE_CREDENTIALS` .

Ao configurar o arquivo de fluxo de trabalho posteriormente, você usa o segredo para a entrada `creds` da ação de logon do Azure. Por exemplo:

```yaml
- uses: azure/login@v1
  with:
    creds: ${{ secrets.AZURE_CREDENTIALS }}
```

Da mesma forma, defina os seguintes segredos adicionais para as credenciais de registro de contêiner e defina-as na ação de logon do Docker.

- REGISTRY_USERNAME
- REGISTRY_PASSWORD

## <a name="build-the-container-image"></a>Criar a imagem de contêiner

O exemplo a seguir mostra parte do fluxo de trabalho que cria a imagem do Docker.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - uses: azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
        docker push contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
```

## <a name="deploy-to-an-app-service-container"></a>Implantar em um contêiner do serviço de aplicativo

Para implantar a imagem em um contêiner personalizado no serviço de aplicativo, use a `azure/webapps-container-deploy@v1` ação. Esta ação tem cinco parâmetros:

| **Parâmetro**  | **Explicação**  |
|---------|---------|
| **app-name** | (Obrigatório) Nome do aplicativo do Serviço de Aplicativo | 
| **slot-name** | (Opcional) Insira um slot existente que não seja o slot de produção |
| **images** | Necessária Especifique o nome da (s) imagem (ns) do contêiner totalmente qualificado. Por exemplo, ' myregistry.azurecr.io/nginx:latest ' ou ' Python: 3.7.2-Alpine/'. Para um aplicativo de vários contêineres, vários nomes de imagem de contêiner podem ser fornecidos (separados por várias linhas) |
| **arquivo de configuração** | Adicional Caminho do arquivo Docker-Compose. Deve ser um caminho totalmente qualificado ou relativo ao diretório de trabalho padrão. Necessário para aplicativos de vários contêineres. |
| **contêiner-comando** | Adicional Insira o comando de inicialização. Por exemplo, filename.dll de execução de dotnet ou dotnet |

Veja abaixo o fluxo de trabalho de exemplo para criar e implantar um aplicativo Node.js em um contêiner personalizado no serviço de aplicativo. Observe como a `creds` entrada faz referência ao `AZURE_CREDENTIALS` segredo que você criou anteriormente.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - uses: azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
        docker push contoso.azurecr.io/nodejssampleapp:${{ github.sha }} 
      
    - uses: azure/webapps-container-deploy@v1
      with:
        app-name: 'node-rnc'
        images: 'contoso.azurecr.io/nodejssampleapp:${{ github.sha }}'
    
    - name: Azure logout
      run: |
        az logout
```

## <a name="next-steps"></a>Próximas etapas

Encontre nosso conjunto de ações agrupadas em diferentes repositórios no GitHub, cada um contendo documentação e exemplos para ajudar você a usar o GitHub para CI/CD e implantar seus aplicativos no Azure.

- [Fluxos de trabalho de ações a serem implantados no Azure](https://github.com/Azure/actions-workflow-samples)

- [Logon do Azure](https://github.com/Azure/login)

- [Aplicativo Web do Azure](https://github.com/Azure/webapps-deploy)

- [Aplicativo Web do Azure para contêineres](https://github.com/Azure/webapps-container-deploy)

- [Logon/logoff do Docker](https://github.com/Azure/docker-login)

- [Eventos que disparam fluxos de trabalho](https://help.github.com/en/articles/events-that-trigger-workflows)

- [Implantação do K8s](https://github.com/Azure/k8s-deploy)

- [Fluxos de trabalho iniciais](https://github.com/actions/starter-workflows)