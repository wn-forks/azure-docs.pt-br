---
title: Iniciar, monitorar e cancelar execuções de treinamento em Python
titleSuffix: Azure Machine Learning
description: Saiba como iniciar, definir o status, marcar e organizar seus experimentos de aprendizado de máquina.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 01/09/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 6d0d626fe0720500b436aea73d6df7c8bb08e004
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89649384"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Iniciar, monitorar e cancelar execuções de treinamento em Python
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

O [SDK do Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true), [Machine Learning CLI](reference-azure-machine-learning-cli.md)e [Azure Machine Learning Studio](https://ml.azure.com) fornecem vários métodos para monitorar, organizar e gerenciar suas execuções para treinamento e experimentação.

Este artigo mostra exemplos das seguintes tarefas:

* Monitorar desempenho de execução.
* As execuções de cancelamento ou falha.
* Criar execuções filhas.
* Marcar e localizar execuções.

## <a name="prerequisites"></a>Pré-requisitos

Você precisará dos seguintes itens:

* Uma assinatura do Azure. Caso não tenha uma assinatura do Azure, crie uma conta gratuita antes de começar. Experimente hoje mesmo a [versão gratuita ou paga do Azure Machine Learning](https://aka.ms/AMLFree).

* Um [espaço de trabalho Azure Machine Learning](how-to-manage-workspace.md).

* O SDK do Azure Machine Learning para Python (versão 1.0.21 ou posterior). Para instalar ou atualizar para a versão mais recente do SDK, consulte [instalar ou atualizar o SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true).

    Para verificar sua versão do SDK do Azure Machine Learning, use o seguinte código:

    ```python
    print(azureml.core.VERSION)
    ```

* A [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) e a [extensão da CLI para Azure Machine Learning](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Iniciar uma execução e seu processo de registro em log

### <a name="using-the-sdk"></a>Usar o SDK

Configure seu experimento importando as classes [Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true), [experimento](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py&preserve-view=true), [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py&preserve-view=true)e [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py&preserve-view=true) do pacote [azureml. Core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py&preserve-view=true) .

```python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Inicie uma execução e seu processo de registro em log com o [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#&preserve-view=truestart-logging--args----kwargs-) método.

```python
notebook_run = exp.start_logging()
notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>Usando a CLI

Para iniciar uma execução do seu experimento, use as seguintes etapas:

1. Em um shell ou prompt de comando, use o CLI do Azure para autenticar a sua assinatura do Azure:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

1. Anexe uma configuração de espaço de trabalho à pasta que contém o script de treinamento. Substitua `myworkspace` por seu espaço de trabalho do Azure Machine Learning. Substitua `myresourcegroup` pelo grupo de recursos do Azure que contém seu espaço de trabalho:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Este comando cria um subdiretório `.azureml` que contém os arquivos de ambiente runconfig e conda de exemplo. Ele também contém um arquivo `config.json` que é usado para se comunicar com seu workspace do Azure Machine Learning.

    Para obter mais informações, consulte [az ml folder attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Para iniciar a execução, use o comando a seguir. Ao usar esse comando, especifique o nome do arquivo runconfig (o texto antes de \*.runconfig se você estiver olhando seu sistema de arquivos) em relação ao parâmetro -c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > O `az ml folder attach` comando criou um `.azureml` subdiretório, que contém dois arquivos de exemplo runconfig.
    >
    > Se você tiver um script Python que cria um objeto de configuração de execução programaticamente, poderá usar [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#&preserve-view=truesave-path-none--name-none--separate-environment-yaml-false-) para salvá-lo como um arquivo runconfig.
    >
    > Para obter mais exemplos de arquivos runconfig, consulte [https://github.com/MicrosoftDocs/pipelines-azureml/](https://github.com/MicrosoftDocs/pipelines-azureml/) .

    Para obter mais informações, consulte [az ml run submit-script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

### <a name="using-azure-machine-learning-studio"></a>Usando o Azure Machine Learning Studio

Para iniciar um envio de um pipeline, execute no designer (versão prévia), use as seguintes etapas:

1. Defina um destino de computação padrão para seu pipeline.

1. Selecione **executar** na parte superior da tela do pipeline.

1. Selecione um experimento para agrupar suas execuções de pipeline.

## <a name="monitor-the-status-of-a-run"></a>Monitorar o status de uma execução

### <a name="using-the-sdk"></a>Usar o SDK

Obtenha o status de uma execução com o [`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=trueget-status--) método.

```python
print(notebook_run.get_status())
```

Para obter a ID de execução, o tempo de execução e os detalhes adicionais sobre a execução, use o [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#&preserve-view=trueget-details--) método.

```python
print(notebook_run.get_details())
```

Quando sua execução for concluída com êxito, use o [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=truecomplete--set-status-true-) método para marcá-la como concluída.

```python
notebook_run.complete()
print(notebook_run.get_status())
```

Se você usar o padrão de design do Python `with...as` , a execução se marcará automaticamente como concluída quando a execução estiver fora do escopo. Você não precisa marcar manualmente a execução como concluída.

```python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print(notebook_run.get_status())

print(notebook_run.get_status())
```

### <a name="using-the-cli"></a>Usando a CLI

1. Para exibir uma lista de execuções para o experimento, use o comando a seguir. Substitua `experiment` pelo nome do seu experimento:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Esse comando retorna um documento JSON que lista informações sobre execuções para este experimento.

    Para obter mais informações, consulte [az ml experiment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Para exibir informações sobre uma execução específica, use o comando a seguir. Substituir `runid` pela ID da execução:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Esse comando retorna um documento JSON que lista informações sobre a execução.

    Para obter mais informações, consulte [AZ ml Run show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).


### <a name="using-azure-machine-learning-studio"></a>Usando o Azure Machine Learning Studio

Para exibir o número de execuções ativas para seu experimento no estúdio.

1. Navegue até a seção **experimentos** . 

1. Selecione um experimento.

    Na página experimento, você pode ver o número de destinos de computação ativos e a duração de cada execução. 

1. Selecione um número de execução específico.

1. Na guia **logs** , você pode encontrar logs de diagnóstico e de erros para a execução do pipeline.


## <a name="cancel-or-fail-runs"></a>Cancelar ou falhar execuções

Se você notar um erro ou se sua execução estiver demorando muito para ser concluída, você poderá cancelar a execução.

### <a name="using-the-sdk"></a>Usar o SDK

Para cancelar uma execução usando o SDK, use o [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=truecancel--) método:

```python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_script_run = exp.submit(run_config)
print(local_script_run.get_status())

local_script_run.cancel()
print(local_script_run.get_status())
```

Se sua execução for concluída, mas contiver um erro (por exemplo, o script de treinamento incorreto foi usado), você poderá usar o [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) método para marcá-lo como com falha.

```python
local_script_run = exp.submit(run_config)
local_script_run.fail()
print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>Usando a CLI

Para cancelar uma execução usando a CLI, use o comando a seguir. Substituir `runid` pela ID da execução

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Para obter mais informações, consulte [AZ ml Run Cancel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

### <a name="using-azure-machine-learning-studio"></a>Usando o Azure Machine Learning Studio

Para cancelar uma execução no estúdio, usando as seguintes etapas:

1. Vá para o pipeline em execução na seção **experimentos** ou **pipelines** . 

1. Selecione o número de execução de pipeline que você deseja cancelar.

1. Na barra de ferramentas, selecione **Cancelar**


## <a name="create-child-runs"></a>Criar execuções filhas

Crie execuções filhas para agrupar execuções relacionadas, como para diferentes iterações de ajuste de hiperparâmetro.

> [!NOTE]
> As execuções filhas só podem ser criadas usando o SDK.

Este exemplo de código usa o `hello_with_children.py` script para criar um lote de cinco execuções filhas de dentro de uma execução enviada usando o [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=truechild-run-name-none--run-id-none--outputs-none-) método:

```python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> À medida que eles saem do escopo, as execuções filhas são automaticamente marcadas como concluídas.

Para criar várias execuções filhas com eficiência, use o [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#&preserve-view=truecreate-children-count-none--tag-key-none--tag-values-none-) método. Como cada criação resulta em uma chamada de rede, a criação de um lote de execuções é mais eficiente do que criá-los um a um.

### <a name="submit-child-runs"></a>Enviar execuções filhas

As execuções filhas também podem ser enviadas de uma execução pai. Isso permite que você crie hierarquias de execuções pai e filho. 

Você pode desejar que seu filho seja executado para usar uma configuração de execução diferente da execução do pai. Por exemplo, você pode usar uma configuração menos eficiente baseada em CPU para o pai, ao usar configurações baseadas em GPU para seus filhos. Outro desejo comum é passar cada um dos diferentes argumentos e dados filho. Para personalizar uma execução filho, passe um `RunConfiguration` objeto para o construtor do filho `ScriptRunConfig` . Este exemplo de código, que seria parte do script do `ScriptRunConfig` objeto pai:

- Cria uma `RunConfiguration` recuperação de um recurso de computação nomeado `"gpu-compute"`
- Itera sobre valores de argumento diferentes a serem passados para os `ScriptRunConfig` objetos filhos
- Cria e envia uma nova execução de filho, usando o recurso de computação personalizado e o argumento
- Bloqueia até que todas as execuções do filho sejam concluídas

```python
# parent.py
# This script controls the launching of child scripts
from azureml.core import Run, ScriptRunConfig, RunConfiguration

run_config_for_aml_compute = RunConfiguration()
run_config_for_aml_compute.target = "gpu-compute"
run_config_for_aml_compute.environment.docker.enabled = True 

run = Run.get_context()

child_args = ['Apple', 'Banana', 'Orange']
for arg in child_args: 
    run.log('Status', f'Launching {arg}')
    child_config = ScriptRunConfig(source_directory=".", script='child.py', arguments=['--fruit', arg], run_config = run_config_for_aml_compute)
    # Starts the run asynchronously
    run.submit_child(child_config)

# Experiment will "complete" successfully at this point. 
# Instead of returning immediately, block until child runs complete

for child in run.get_children():
    child.wait_for_completion()
```

Para criar várias execuções filhas com configurações, argumentos e entradas idênticas com eficiência, use o [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#&preserve-view=truecreate-children-count-none--tag-key-none--tag-values-none-) método. Como cada criação resulta em uma chamada de rede, a criação de um lote de execuções é mais eficiente do que criá-los um a um.

Em uma execução de filho, você pode exibir a ID de execução pai:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Consultar execuções filhas

Para consultar as execuções filhas de um pai específico, use o [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=trueget-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) método. O ``recursive = True`` argumento permite consultar uma árvore aninhada de Children e netos.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Marcar e localizar execuções

No Azure Machine Learning, você pode usar as propriedades e marcas para ajudar a organizar e consultar suas execuções para obter informações importantes.

### <a name="add-properties-and-tags"></a>Adicionar propriedades e marcas

#### <a name="using-the-sdk"></a>Usar o SDK

Para adicionar metadados pesquisáveis às suas execuções, use o [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=trueadd-properties-properties-) método. Por exemplo, o código a seguir adiciona a `"author"` Propriedade à execução:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

As propriedades são imutáveis e, portanto, criam um registro permanente para fins de auditoria. O exemplo de código a seguir resulta em um erro, porque já adicionamos `"azureml-user"` como o `"author"` valor da propriedade no código anterior:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Ao contrário das propriedades, as marcas são mutáveis. Para adicionar informações de pesquisa e significativas para os consumidores de seu experimento, use o [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#&preserve-view=truetag-key--value-none-) método.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Você também pode adicionar marcas de cadeia de caracteres simples. Quando essas marcas aparecem no dicionário de marcas como chaves, elas têm um valor de `None` .

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>Usando a CLI

> [!NOTE]
> Usando a CLI, você só pode adicionar ou atualizar marcas.

Para adicionar ou atualizar uma marca, use o seguinte comando:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Para obter mais informações, consulte [AZ ml Run Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Propriedades e marcas de consulta

Você pode consultar execuções em um experimento para retornar uma lista de execuções que correspondem a propriedades e marcas específicas.

#### <a name="using-the-sdk"></a>Usar o SDK

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>Usando a CLI

O CLI do Azure dá suporte a consultas [JMESPath](http://jmespath.org) , que podem ser usadas para filtrar execuções com base em Propriedades e marcas. Para usar uma consulta JMESPath com o CLI do Azure, especifique-o com o `--query` parâmetro. Os exemplos a seguir mostram consultas básicas usando propriedades e marcas:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Para obter mais informações sobre como consultar resultados de CLI do Azure, consulte [consultar CLI do Azure comando de saída](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

### <a name="using-azure-machine-learning-studio"></a>Usando o Azure Machine Learning Studio

1. Navegue até a seção **pipelines** .

1. Use a barra de pesquisa para filtrar os pipelines usando marcas, descrições, nomes de experimentos e nome do emissor.

## <a name="example-notebooks"></a>Blocos de anotações de exemplo

Os blocos de anotações a seguir demonstram os conceitos neste artigo:

* Para saber mais sobre as APIs de log, consulte o [bloco de anotações da API de log](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Para obter mais informações sobre o gerenciamento de execuções com o SDK do Azure Machine Learning, consulte o [bloco de anotações gerenciar execuções](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Próximas etapas

* Para saber como registrar métricas para seus experimentos, consulte [métricas de log durante execuções de treinamento](how-to-track-experiments.md).
* Para saber como monitorar recursos e logs de Azure Machine Learning, consulte [Azure Machine Learning de monitoramento](monitor-azure-machine-learning.md).
