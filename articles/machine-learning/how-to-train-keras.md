---
title: Treinar modelos de Keras de aprendizado profundo
titleSuffix: Azure Machine Learning
description: Saiba como treinar e registrar um modelo de classificação de rede neural Keras profundo em execução no TensorFlow usando o Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 08/01/2019
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 1917ded59275c7a456e8d2fd25412afc91a1327c
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89650637"
---
# <a name="train-and-register-a-keras-classification-model-with-azure-machine-learning"></a>Treinar e registrar um modelo de classificação Keras com Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Este artigo mostra como treinar e registrar um modelo de classificação Keras criado em TensorFlow usando Azure Machine Learning. Ele usa o popular [conjunto de MNIST](http://yann.lecun.com/exdb/mnist/) para classificar dígitos manuscritos usando uma DNN (rede neural profunda) criada usando a [biblioteca Keras Python](https://keras.io) em execução na parte superior do [TensorFlow](https://www.tensorflow.org/overview).

Keras é uma API de rede neural de alto nível capaz de executar a parte superior de outras estruturas de DNN populares para simplificar o desenvolvimento. Com o Azure Machine Learning, você pode expandir rapidamente os trabalhos de treinamento usando recursos de computação em nuvem elástico. Você também pode acompanhar suas execuções de treinamento, modelos de versão, implantar modelos e muito mais.

Se você estiver desenvolvendo um modelo de Keras do zero ou estiver trazendo um modelo existente para a nuvem, Azure Machine Learning poderá ajudá-lo a criar modelos prontos para produção.

Consulte o [artigo conceitual](concept-deep-learning-vs-machine-learning.md) para obter informações sobre as diferenças entre o aprendizado de máquina e o aprendizado profundo.

## <a name="prerequisites"></a>Pré-requisitos

Execute este código em qualquer um destes ambientes:

- Instância de computação do Azure Machine Learning - nenhum download ou instalação é necessária

     - Conclua o [Tutorial: Configure o ambiente e o espaço de trabalho](tutorial-1st-experiment-sdk-setup.md) para criar um servidor de notebook dedicado pré-carregado com o SDK e o exemplo de repositório.
    - Na pasta Samples no servidor do notebook, encontre um notebook completo e expandido navegando até este diretório: **instruções-uso-azureml > treinamento com aprendizado profundo > Train-hiperparameter-ajuste-deploy-com-Keras** pasta.

 - Seu próprio servidor Jupyter Notebook

    - [Instale o SDK do Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true).
    - [Criar um arquivo de configuração de workspace](how-to-configure-environment.md#workspace).
    - [Baixar os arquivos](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras) `mnist-keras.py` de script de exemplo e `utils.py`

    Você também pode encontrar uma versão completa do [Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) deste guia na página de exemplos do github. O notebook inclui seções expandidas que abrangem o ajuste de hiperparâmetro inteligente, implantação de modelo e widgets de notebook.

## <a name="set-up-the-experiment"></a>Configurar o experimento

Esta seção configura o teste de treinamento carregando os pacotes do python necessários, inicializando um espaço de trabalho, criando um experimento e carregando os dados de treinamento e os scripts de treinamento.

### <a name="import-packages"></a>Importar pacotes

Primeiro, importe as bibliotecas Python necessárias.

```Python
import os
import azureml
from azureml.core import Experiment
from azureml.core import Workspace, Run
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Inicializar um workspace

O [espaço de trabalho Azure Machine Learning](concept-workspace.md) é o recurso de nível superior para o serviço. Ele fornece um local centralizado para trabalhar com todos os artefatos que você criar. No SDK do Python, você pode acessar os artefatos do espaço de trabalho criando um [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py&preserve-view=true) objeto.

Crie um objeto de espaço de trabalho a partir do `config.json` arquivo criado na [seção pré-requisitos](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Criar uma experiência

Crie um experimento chamado "Keras-mnist" em seu espaço de trabalho.

```Python
exp = Experiment(workspace=ws, name='keras-mnist')
```

<a name="data-upload"></a>
### <a name="create-a-file-dataset"></a>Criar um conjunto de um arquivo

Um objeto `FileDataset` referencia um ou vários arquivos no armazenamento de dados do workspace ou em URLs públicas. Os arquivos podem ser de qualquer formato e a classe fornece a capacidade de baixar ou montar os arquivos no computador. Ao criar um `FileDataset`, você cria uma referência à localização da fonte de dados. Se você tiver aplicado todas as transformações ao conjunto de dados, elas também serão armazenadas no conjunto de dados. Os dados permanecem na localização existente, portanto, nenhum custo de armazenamento extra é gerado. Confira o guia de [instruções](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets) no pacote `Dataset` para obter mais informações.

```python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
dataset = Dataset.File.from_files(path=web_paths)
```

Use o `register()` método para registrar o conjunto de dados em seu espaço de trabalho para que eles possam ser compartilhados com outras pessoas, reutilizados em vários experimentos e referenciados por nome em seu script de treinamento.

```python
dataset = dataset.register(workspace=ws,
                           name='mnist dataset',
                           description='training and test dataset',
                           create_new_version=True)
```

## <a name="create-a-compute-target"></a>Criar um destino de computação

Crie um destino de computação para a execução do seu trabalho do TensorFlow. Neste exemplo, crie um cluster de computação Azure Machine Learning habilitado para GPU.

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

Para obter mais informações sobre destinos de computação, consulte o artigo [o que é um destino de computação](concept-compute-target.md) .

## <a name="create-a-tensorflow-estimator-and-import-keras"></a>Criar um estimador TensorFlow e importar Keras

O [estimador TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py&preserve-view=true) fornece uma maneira simples de iniciar trabalhos de treinamento do TensorFlow no destino de computação. Como o Keras é executado na parte superior do TensorFlow, você pode usar o estimador TensorFlow e importar a biblioteca Keras usando o `pip_packages` argumento.

Primeiro, obtenha os dados do armazenamento do espaço de trabalho usando a `Dataset` classe.

```python
dataset = Dataset.get_by_name(ws, 'mnist dataset')

# list the files referenced by mnist dataset
dataset.to_path()
```

O estimador TensorFlow é implementado por meio da [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py&preserve-view=true) classe genérica, que pode ser usada para dar suporte a qualquer estrutura. Além disso, crie um dicionário `script_params` que contenha as configurações de hiperparâmetro DNN. Para ver mais informações sobre modelos de treinamento que usam o estimador genérico, consulte [Treinar modelos com Azure Machine Learning usando o estimador](how-to-train-ml-models.md)

```python
from azureml.train.dnn import TensorFlow

script_params = {
    '--data-folder': dataset.as_named_input('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.001
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='keras_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 pip_packages=['keras', 'matplotlib'],
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Enviar uma execução

O [objeto Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py&preserve-view=true) fornece a interface para o histórico de execução enquanto o trabalho está em execução e após sua conclusão.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

À medida que a execução é executada, ela passa pelos seguintes estágios:

- **Preparando**: uma imagem do Docker é criada de acordo com o estimador do TensorFlow. A imagem é carregada no registro de contêiner do espaço de trabalho e armazenada em cache para execuções posteriores. Os logs também são transmitidos para o histórico de execução e podem ser exibidos para monitorar o progresso.

- **Dimensionamento**: o cluster tentará escalar verticalmente se o cluster de ia do lote exigir mais nós para executar a execução do que está disponível no momento.

- **Em execução**: todos os scripts na pasta de script são carregados para o destino de computação, os armazenamentos de dados são montados ou copiados e o entry_script é executado. As saídas de stdout e a pasta./logs são transmitidas para o histórico de execução e podem ser usadas para monitorar a execução.

- **Pós-processamento**: a pasta./Outputs da execução é copiada para o histórico de execuções.

## <a name="register-the-model"></a>Registre o modelo

Depois de treinar o modelo DNN, você poderá registrá-lo em seu espaço de trabalho. O registro de modelo permite que você armazene e versione os modelos no workspace para simplificar o [gerenciamento e a implantação de modelos](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='keras-dnn-mnist', model_path='outputs/model')
```

> [!TIP]
> O modelo que você acabou de registrar é implantado exatamente da mesma maneira que qualquer outro modelo registrado no Azure Machine Learning, independentemente do estimador usado para treinamento. A implantação "como" contém uma seção sobre como registrar modelos, mas você pode pular diretamente para [criar um destino de computação](how-to-deploy-and-where.md#choose-a-compute-target) para implantação, já que você já tem um modelo registrado.

Você também pode baixar uma cópia local do modelo. Isso pode ser útil para fazer o trabalho de validação de modelo adicional localmente. No script de treinamento, `mnist-keras.py` , um objeto TensorFlow de proteção persiste o modelo para uma pasta local (local para o destino de computação). Você pode usar o objeto executar para baixar uma cópia do repositório de armazenamento.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você treinou e registrou um modelo Keras em Azure Machine Learning. Para saber como implantar um modelo, continue em nosso artigo de implantação de modelo.

> [!div class="nextstepaction"]
> [Como e onde implantar modelos](how-to-deploy-and-where.md)
* [Executar as métricas de durante o treinamento de faixa](how-to-track-experiments.md)
* [Ajustar hiperparâmetros](how-to-tune-hyperparameters.md)
* [Implantar um modelo treinado](how-to-deploy-and-where.md)
* [Arquitetura de referência para treinamento de aprendizado profundo distribuído no Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
