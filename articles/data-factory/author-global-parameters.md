---
title: Parâmetros globais
description: Definir parâmetros globais para cada um de seus ambientes de Azure Data Factory
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
ms.date: 08/31/2020
ms.openlocfilehash: 96fba5c27115dab65f26be80ce03bef35abcdb92
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89230816"
---
# <a name="global-parameters-in-azure-data-factory"></a>Parâmetros globais no Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Os parâmetros globais são constantes em um data factory que podem ser consumidos por um pipeline em qualquer expressão. Eles são úteis quando você tem vários pipelines com valores e nomes de parâmetro idênticos. Ao promover um data factory usando o processo de integração e implantação contínua (CI/CD), você pode substituir esses parâmetros em cada ambiente. 

## <a name="creating-global-parameters"></a>Criando parâmetros globais

Para criar um parâmetro global, vá para a guia *parâmetros globais* na seção *gerenciar* . Selecione **novo** para abrir o navegador lateral de criação.

![Criar parâmetros globais](media/author-global-parameters/create-global-parameter-1.png)

Na barra de navegação lateral, insira um nome, selecione um tipo de dados e especifique o valor do parâmetro.

![Criar parâmetros globais](media/author-global-parameters/create-global-parameter-2.png)

Depois que um parâmetro global é criado, você pode editá-lo clicando no nome do parâmetro. Para alterar vários parâmetros de uma vez, selecione **Editar tudo**.

![Criar parâmetros globais](media/author-global-parameters/create-global-parameter-3.png)

## <a name="using-global-parameters-in-a-pipeline"></a>Usando parâmetros globais em um pipeline

Os parâmetros globais podem ser usados em qualquer [expressão de pipeline](control-flow-expression-language-functions.md). Se um pipeline estiver fazendo referência a outro recurso, como um conjunto de dados ou fluxo, você poderá passar o valor do parâmetro global por meio dos parâmetros desse recurso. Os parâmetros globais são referenciados como `pipeline().globalParameters.<parameterName>` .

![Usando parâmetros globais](media/author-global-parameters/expression-global-parameters.png)

## <a name="global-parameters-in-cicd"></a><a name="cicd"></a> Parâmetros globais no CI/CD

Há duas maneiras de integrar parâmetros globais em sua solução de implantação e integração contínua:

* Incluir parâmetros globais no modelo ARM
* Implantar parâmetros globais por meio de um script do PowerShell

Para a maioria dos casos de uso, é recomendável incluir parâmetros globais no modelo ARM. Isso será integrado nativamente à solução descrita no [documento CI/CD](continuous-integration-deployment.md). Os parâmetros globais serão adicionados como um parâmetro de modelo ARM por padrão, pois eles geralmente mudam de ambiente para ambiente. Você pode habilitar a inclusão de parâmetros globais no modelo ARM do hub de gerenciamento.

![Incluir no modelo ARM](media/author-global-parameters/include-arm-template.png)

A adição de parâmetros globais ao modelo ARM adiciona uma configuração de nível de fábrica que pode substituir outras configurações de nível de fábrica, como uma chave gerenciada pelo cliente ou uma configuração de git em outros ambientes. Se você tiver essas configurações habilitadas em um ambiente com privilégios elevados, como UAT ou PROD, será melhor implantar parâmetros globais por meio de um script do PowerShell nas etapas realçadas abaixo.

### <a name="deploying-using-powershell"></a>Implantando usando o PowerShell

As etapas a seguir descrevem como implantar parâmetros globais por meio do PowerShell. Isso é útil quando sua fábrica de destino tem uma configuração em nível de fábrica, como a chave gerenciada pelo cliente.

Quando você publica uma fábrica ou exporta um modelo ARM com parâmetros globais, uma pasta chamada *globalparameters* é criada com um arquivo chamado *your-factory-name_GlobalParameters.js*. Esse arquivo é um objeto JSON que contém cada tipo de parâmetro global e valor na fábrica publicada.

![Publicando parâmetros globais](media/author-global-parameters/global-parameters-adf-publish.png)

Se você estiver implantando em um novo ambiente, como TEST ou PROD, é recomendável criar uma cópia desse arquivo de parâmetros globais e substituir os valores específicos do ambiente apropriados. Quando você republicar o arquivo de parâmetros globais original será substituído, mas a cópia para o outro ambiente será inalterada.

Por exemplo, se você tiver uma fábrica chamada "ADF-DEV" e um parâmetro global do tipo cadeia de caracteres chamado "ambiente" com um valor "dev", quando você publicar um arquivo chamado *ADF-DEV_GlobalParameters.jsem* será gerado. Se estiver implantando em uma fábrica de teste chamada ' ADF_TEST ', crie uma cópia do arquivo JSON (por exemplo, chamada ADF-TEST_GlobalParameters.js) e substitua os valores de parâmetro pelos valores específicos do ambiente. O parâmetro ' Environment ' pode ter um valor ' test ' agora. 

![Implantando parâmetros globais](media/author-global-parameters/powershell-task.png)

Use o script do PowerShell abaixo para promover parâmetros globais para ambientes adicionais. Adicione uma tarefa de Azure PowerShell DevOps antes de sua Implantação de modelo de ARM. Na tarefa DevOps, você deve especificar o local do novo arquivo de parâmetros, o grupo de recursos de destino e o data factory de destino.

> [!NOTE]
> Para implantar parâmetros globais usando o PowerShell, você deve usar pelo menos a versão 4.4.0 do módulo AZ.

```powershell
param
(
    [parameter(Mandatory = $true)] [String] $globalParametersFilePath,
    [parameter(Mandatory = $true)] [String] $resourceGroupName,
    [parameter(Mandatory = $true)] [String] $dataFactoryName
)

Import-Module Az.DataFactory

$newGlobalParameters = New-Object 'system.collections.generic.dictionary[string,Microsoft.Azure.Management.DataFactory.Models.GlobalParameterSpecification]'

Write-Host "Getting global parameters JSON from: " $globalParametersFilePath
$globalParametersJson = Get-Content $globalParametersFilePath

Write-Host "Parsing JSON..."
$globalParametersObject = [Newtonsoft.Json.Linq.JObject]::Parse($globalParametersJson)

foreach ($gp in $globalParametersObject.GetEnumerator()) {
    Write-Host "Adding global parameter:" $gp.Key
    $globalParameterValue = $gp.Value.ToObject([Microsoft.Azure.Management.DataFactory.Models.GlobalParameterSpecification])
    $newGlobalParameters.Add($gp.Key, $globalParameterValue)
}

$dataFactory = Get-AzDataFactoryV2 -ResourceGroupName $resourceGroupName -Name $dataFactoryName
$dataFactory.GlobalParameters = $newGlobalParameters

Write-Host "Updating" $newGlobalParameters.Count "global parameters."

Set-AzDataFactoryV2 -InputObject $dataFactory -Force
```

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre o [processo de implantação e integração contínua](continuous-integration-deployment.md) do Azure data Factory
* Saiba como usar a [linguagem de expressão de fluxo de controle](control-flow-expression-language-functions.md)