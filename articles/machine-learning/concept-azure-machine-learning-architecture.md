---
title: Arquitetura e principais conceitos
titleSuffix: Azure Machine Learning
description: Saiba mais sobre a arquitetura, os termos e os conceitos que compõem Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 08/20/2020
ms.custom: seoapril2019, seodec18
ms.openlocfilehash: c24e9f58154b1523496a82761a8c48ba06dea46c
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651257"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>Como o Azure Machine Learning funciona: Arquitetura e conceitos

Saiba mais sobre a arquitetura e os conceitos para [Azure Machine Learning](overview-what-is-azure-ml.md).  Este artigo fornece uma compreensão de alto nível dos componentes e como eles funcionam juntos para auxiliar no processo de criação, implantação e manutenção de modelos de aprendizado de máquina.

## <a name="workspace"></a><a name="workspace"></a> Espaço

Um [espaço de trabalho do Machine Learning](concept-workspace.md) é o recurso de nível superior para Azure Machine Learning.

:::image type="content" source="media/concept-azure-machine-learning-architecture/architecture.svg" alt-text="Diagrama: arquitetura de Azure Machine Learning de um espaço de trabalho e seus componentes":::

O espaço de trabalho é o local centralizado para:

* Gerencie os recursos que você usa para treinamento e implantação de modelos, como [cálculos](#compute-instance)
* Armazene ativos que você cria ao usar Azure Machine Learning, incluindo:
  * [Ambientes](#environments)
  * [Testes](#experiments)
  * [Pipelines](#ml-pipelines)
  * [Conjunto de dados](#datasets-and-datastores)
  * [Modelos](#models)
  * [Pontos de extremidade](#endpoints)

Um espaço de trabalho inclui outros recursos do Azure que são usados pelo espaço de trabalho:

+ [ACR (registro de contêiner do Azure)](https://azure.microsoft.com/services/container-registry/): registra os contêineres do Docker que você usa durante o treinamento e quando implanta um modelo. Para minimizar os custos, o ACR só é criado quando imagens de implantação são criadas.
+ [Conta de armazenamento do Azure](https://azure.microsoft.com/services/storage/): é usada como o repositório de armazenamento padrão para o espaço de trabalho.  Os blocos de anotações do Jupyter que são usados com suas instâncias de computação Azure Machine Learning também são armazenados aqui.
+ [Insights de aplicativo Azure](https://azure.microsoft.com/services/application-insights/): armazena informações de monitoramento sobre seus modelos.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): armazena segredos que são usados por destinos de computação e outras informações confidenciais que são necessárias para o espaço de trabalho.

Você pode compartilhar um workspace com outras pessoas.

## <a name="computes"></a>Calcula

<a name="compute-targets"></a> Um [destino de computação](concept-compute-target.md) é qualquer computador ou conjunto de máquinas que você usa para executar o script de treinamento ou hospedar sua implantação de serviço. Você pode usar seu computador local ou um recurso de computação remota como um destino de computação.  Com os destinos de computação, você pode iniciar o treinamento em seu computador local e, em seguida, expandir para a nuvem sem alterar o script de treinamento.

Azure Machine Learning introduz duas VMS (máquinas virtuais) baseadas em nuvem totalmente gerenciadas configuradas para tarefas de aprendizado de máquina:

* <a name="compute-instance"></a>**Instância de computação**: uma instância de computação é uma VM que inclui várias ferramentas e ambientes instalados para aprendizado de máquina. O uso principal de uma instância de computação é para sua estação de trabalho de desenvolvimento.  Você pode começar a executar blocos de anotações de exemplo sem a necessidade de configuração. Uma instância de computação também pode ser usada como um destino de computação para trabalhos de treinamento e inferência.

* **Clusters de computação**: clusters de computação são um cluster de VMs com recursos de dimensionamento de vários nós. Os clusters de computação são mais adequados para destinos de computação para trabalhos grandes e produção.  O cluster é dimensionado automaticamente quando um trabalho é enviado.  Use como um destino de computação de treinamento ou para implantação de desenvolvimento/teste.

Para obter mais informações sobre os destinos de computação de treinamento, consulte [Training Compute targets](concept-compute-target.md#train).  Para obter mais informações sobre destinos de computação de implantação, consulte [destinos de implantação](concept-compute-target.md#deploy).

## <a name="datasets-and-datastores"></a>Conjuntos de dados e armazenamentos de dados

[**Azure Machine Learning DataSets**](concept-data.md#datasets)  facilitam o acesso e o trabalho com seus dados. Ao criar um conjunto de dados, você cria uma referência para o local de origem da fonte junto com uma cópia de seus metadados. Como os dados permanecem em seu local existente, você não incorre nenhum custo de armazenamento extra e não arriscará a integridade de suas fontes de dados.

Para obter mais informações, confira [Criar e registrar conjuntos de dados do Azure Machine Learning](how-to-create-register-datasets.md).  Para obter mais exemplos do uso de conjuntos de dados, confira os [exemplos de notebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets-tutorial).

Os conjuntos de valores usam [repositórios de armazenamento](concept-data.md#datastores) para se conectar com segurança aos serviços de armazenamento do Azure. Os armazenamentos de dados armazenam informações de conexão sem colocar suas credenciais de autenticação e a integridade da fonte original em risco. Eles armazenam informações de conexão, como a sua ID de assinatura e a autorização de token em seu Key Vault associado ao espaço de trabalho, para que você possa acessar seu armazenamento com segurança sem precisar embuti-los em seu script.

## <a name="environments"></a>Ambientes

[Espaço de trabalho](#workspace)  >  **Ambientes**

Um [ambiente](concept-environments.md) é o encapsulamento do ambiente em que ocorre o treinamento ou a Pontuação do seu modelo de aprendizado de máquina. O ambiente especifica os pacotes do Python, as variáveis de ambiente e as configurações de software em relação aos seus scripts de treinamento e pontuação.  

Para obter exemplos de código, consulte a seção "gerenciar ambientes" de [como usar ambientes](how-to-use-environments.md#manage-environments).

## <a name="experiments"></a>Testes

[Espaço de trabalho](#workspace)  >  **Experimentos**

Um experimento é um agrupamento de diversas execuções de um determinado script. Ele sempre pertence a um workspace. Quando você envia uma execução, você pode fornecer um nome de experimento. As informações para a execução são armazenadas nesse experimento. Se o nome não existir quando você enviar um experimento, um novo experimento será criado automaticamente.
  
Para obter um exemplo do uso de um experimento, confira [Tutorial: treinar seu primeiro modelo](tutorial-1st-experiment-sdk-train.md).

### <a name="runs"></a>Execuções

[Espaço de trabalho](#workspace)  >  [Experimentos](#experiments)  >  **Executar**

Uma execução é uma execução única de um script de treinamento. Um experimento normalmente conterá várias execuções.

O Azure Machine Learning registra todas as execuções e armazena as seguintes informações no experimento:

* Metadados sobre a execução (carimbo de hora, duração, etc.)
* Métricas registradas pelo seu script
* Arquivos de saída coletados automaticamente pelo experimento ou carregados explicitamente por você
* Um instantâneo do diretório que contém seus scripts, antes da execução

Uma execução é produzida quando você envia um script para fazer o treinamento de um modelo. Uma execução pode ter zero ou mais execuções filho. Por exemplo, a execução de nível superior pode ter duas execuções filho, cada uma delas pode ter sua próprias execuções filho.

### <a name="run-configurations"></a>Configurações de execução

[Espaço de trabalho](#workspace)  >  [Experimentos](#experiments)  >  [Executar](#runs)  >  **Configuração de execução**

Uma configuração de execução é um conjunto de instruções que define como um script deve ser executado em um destino de computação especificado. Ela inclui um amplo conjunto de definições de comportamento, como se deseja usar um ambiente de Python existente ou usar um ambiente de Conda criado a partir de uma especificação.

Uma configuração de execução pode ser persistida em um arquivo dentro do diretório que contém o script de treinamento.   Ou pode ser construído como um objeto na memória e usado para enviar uma execução.

Por exemplo, execute as configurações, consulte [usar um destino de computação para treinar seu modelo](how-to-set-up-training-targets.md).

### <a name="estimators"></a>Avaliadores

Para facilitar o treinamento de modelo com estruturas conhecidas, a classe avaliadora permite que você construa facilmente configurações de execução. Você pode criar e usar um [Estimador](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py&preserve-view=true) genérico de para enviar scripts de treinamento que usam qualquer estrutura de aprendizado que você escolher (como Scikit-learn).

Para obter mais informações sobre os estimadores, consulte [treinar modelos de ml com estimações](how-to-train-ml-models.md).

### <a name="snapshots"></a>Instantâneos

[Espaço de trabalho](#workspace)  >  [Experimentos](#experiments)  >  [Executar](#runs)  >  **Instantâneo**

Ao enviar uma execução, o Azure Machine Learning compacta o diretório que contém o script como um arquivo zip e o envia para o destino de computação. O arquivo zip é expandido e o script é executado lá. O Azure Machine Learning também armazena o arquivo zip como um instantâneo como parte do registro de execução. Qualquer pessoa com acesso ao workspace pode procurar um registro de execução e baixar o instantâneo.


### <a name="logging"></a>Registrando em log

Azure Machine Learning registra automaticamente as métricas de execução padrão para você. No entanto, você também pode [usar o SDK do Python para registrar métricas arbitrárias](how-to-track-experiments.md).

Há várias maneiras de exibir seus logs: status de execução de monitoramento em tempo real ou exibição de resultados após a conclusão. Para obter mais informações, consulte [monitorar e exibir os logs de execução do ml](how-to-monitor-view-training-logs.md).


> [!NOTE]
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]

### <a name="git-tracking-and-integration"></a>Acompanhamento e integração do Git

Quando você inicia uma execução de treinamento em que o diretório de origem é um repositório Git local, as informações sobre o repositório são armazenadas no histórico de execuções. Funciona com execuções enviadas usando um avaliador, pipeline de ML ou execução de script. Também funciona para execuções enviadas do SDK ou da CLI do Machine Learning.

Para obter mais informações, confira [Integração do Git com o Azure Machine Learning](concept-train-model-git-integration.md).

## <a name="models"></a>Modelos

Em sua forma mais simples, um modelo é um trecho de código que usa uma entrada e produz uma saída. A criação de um modelo de aprendizado de máquina envolve a seleção de um algoritmo, o fornecimento de dados e o [ajuste de hiperparâmetros](how-to-tune-hyperparameters.md). O treinamento é um processo iterativo que produz um modelo treinado, que encapsula o modelo aprendido durante o processo de treinamento.

Você pode colocar um modelo que foi treinado fora do Azure Machine Learning. Ou você pode treinar um modelo enviando uma [execução](#runs) de um [experimento](#experiments) para um destino de [computação](#compute-targets) no Azure Machine Learning. Depois de ter um modelo, [Registre o modelo](#register-model) no espaço de trabalho.

O Azure Machine Learning é independente do framework. Quando você cria um modelo, pode usar qualquer estrutura de aprendizado de máquina conhecida, como Scikit-learn, XGBoost, PyTorch, TensorFlow e Chainer.

Para obter um exemplo de treinamento de um modelo usando Scikit-learn, consulte [tutorial: treinar um modelo de classificação de imagem com Azure Machine Learning](tutorial-train-models-with-aml.md).


### <a name="model-registry"></a><a name="register-model"></a> Registro de modelo

[Espaço de trabalho](#workspace)  >  **Modelos** do

O **registro de modelo** permite manter o controle de todos os modelos em seu espaço de trabalho Azure Machine Learning.

Os modelos são identificados por nome e versão. Cada vez que você registra um modelo com o mesmo nome de um já existente, o registro pressupõe que se trata de uma nova versão. A versão é incrementada e o novo modelo é registrado com o mesmo nome.

Você pode fornecer as marcas de metadados adicionais quando registrar o modelo e, em seguida, usar essas marcas ao procurar pelos modelos.

> [!TIP]
> Um modelo registrado é um contêiner lógico para um ou mais arquivos que compõem seu modelo. Por exemplo, se você tiver um modelo armazenado em vários arquivos, poderá registrá-los como um único modelo em seu workspace do Azure Machine Learning. Após o registro, você pode baixar ou implantar o modelo registrado e receber todos os arquivos registrados.

Não é possível excluir um modelo registrado que está sendo usado por uma implantação ativa.

Para obter um exemplo de registro de um modelo, consulte [Treinar um modelo de classificação de imagem com o Azure Machine Learning](tutorial-train-models-with-aml.md).

## <a name="deployment"></a>Implantação

Você implanta um [modelo registrado](#register-model) como um ponto de extremidade de serviço. Você precisa dos seguintes componentes:

* **Ambiente**. Esse ambiente encapsula as dependências necessárias para executar seu modelo para a inferência.
* **Código de Pontuação**. Esse script aceita solicitações, pontua as solicitações usando o modelo e retorna os resultados.
* **Configuração de inferência**. A configuração de inferência especifica o ambiente, o script de entrada e outros componentes necessários para executar o modelo como um serviço.

Para obter mais informações sobre esses componentes, consulte [implantar modelos com Azure Machine Learning](how-to-deploy-and-where.md).

### <a name="endpoints"></a>Pontos de extremidade

[Espaço de trabalho](#workspace)  >  **Pontos de extremidade**

Um ponto de extremidade é uma instanciação do modelo em um serviço Web que pode ser hospedado na nuvem ou um módulo de IoT para implantações de dispositivos integrados.

#### <a name="web-service-endpoint"></a>Ponto de extremidade de serviço Web

Ao implantar um modelo como um serviço Web, o ponto de extremidade pode ser implantado em instâncias de contêiner do Azure, serviço kubernetes do Azure ou FPGAs. O serviço é criado a partir de um modelo, script e arquivos associados. Eles são colocados em uma imagem de contêiner de base, que contém o ambiente de execução do modelo. A imagem tem um balanceamento de carga, ponto de extremidade HTTP que recebe as solicitações de pontuação enviadas para o serviço Web.

Você pode habilitar a telemetria Application Insights ou telemetria de modelo para monitorar seu serviço Web. Os dados de telemetria só podem ser acessados por você.  Ele é armazenado em suas instâncias de conta de armazenamento e Application Insights.

Se você tiver habilitado o dimensionamento automático, o Azure dimensionará automaticamente sua implantação.

Para obter um exemplo de implantação de um modelo como um serviço web, consulte [Implantar um modelo de classificação de imagem nas Instâncias de Contêiner do Azure](tutorial-deploy-models-with-aml.md).

#### <a name="real-time-endpoints"></a>Pontos de extremidade em tempo real

Quando você implanta um modelo treinado no designer (versão prévia), você pode [implantar o modelo como um ponto de extremidade em tempo real](tutorial-designer-automobile-price-deploy.md). Um ponto de extremidade em tempo real geralmente recebe uma única solicitação por meio do ponto de extremidade REST e retorna uma previsão em tempo real. Isso está em contraste com o processamento em lotes, que processa vários valores de uma vez e salva os resultados após a conclusão para um repositório de armazenamento.

#### <a name="pipeline-endpoints"></a>Pontos de extremidade do pipeline

Os pontos de extremidade de pipeline permitem chamar seus [pipelines de ml](#ml-pipelines) programaticamente por meio de um ponto de extremidade REST. Os pontos de extremidade do pipeline permitem automatizar seus fluxos de trabalho de pipeline.

Um ponto de extremidade de pipeline é uma coleção de pipelines publicados. Essa organização lógica permite que você gerencie e chame vários pipelines usando o mesmo ponto de extremidade. Cada pipeline publicado em um ponto de extremidade de pipeline tem controle de versão. Você pode selecionar um pipeline padrão para o ponto de extremidade ou especificar uma versão na chamada REST.
 

#### <a name="iot-module-endpoints"></a>Pontos de extremidade do módulo de IoT

Um ponto de extremidade do módulo de IoT implantado é um contêiner do Docker que inclui seu modelo, o script ou aplicativo associado e as dependências adicionais. Esses módulos são implantados usando o Azure IoT Edge em dispositivos de borda.

Se você tiver habilitado o monitoramento, o Azure coleta dados de telemetria do modelo de dentro do módulo do Azure IoT Edge. Os dados de telemetria estão acessíveis apenas para você e armazenados em sua instância de conta de armazenamento.

O Azure IoT Edge garantirá que seu módulo esteja em execução e monitorará o dispositivo que está hospedando. 
## <a name="automation"></a>Automação

### <a name="azure-machine-learning-cli"></a>CLI do Azure Machine Learning 

A [CLI do Azure Machine Learning](reference-azure-machine-learning-cli.md) é uma extensão do CLI do Azure, uma interface de linha de comando de plataforma cruzada para a plataforma do Azure. Essa extensão fornece comandos para automatizar suas atividades de aprendizado de máquina.

### <a name="ml-pipelines"></a>Pipelines de ML

Você usa [pipelines de Machine Learning](concept-ml-pipelines.md) para criar e gerenciar fluxos de trabalho que unem as fases de aprendizado de máquina. Por exemplo, um pipeline pode incluir preparação de dados, treinamento de modelo, implantação de modelo e fases de inferência/pontuação. Cada fase pode incluir várias etapas, cada uma delas pode ser executada de modo autônomo em vários destinos de computação. 

As etapas do pipeline são reutilizáveis e poderão ser executadas sem realizar novamente as etapas anteriores se o resultado dessas etapas não tiver sido alterado. Por exemplo, você poderá treinar novamente um modelo sem executar pela segunda vez as caras etapas de preparação de dados se os dados não forem alterados. Os pipelines também permitem que cientistas de dados colaborem enquanto trabalham em áreas separadas de um fluxo de trabalho de aprendizado de máquina.

## <a name="interacting-with-your-workspace"></a>Interagindo com seu espaço de trabalho

### <a name="studio"></a>Estúdio

O [Azure Machine Learning Studio](overview-what-is-machine-learning-studio.md) fornece uma exibição da Web de todos os artefatos em seu espaço de trabalho.  Você pode exibir os resultados e os detalhes de seus conjuntos de informações, testes, pipelines, modelos e pontos de extremidade.  Você também pode gerenciar os recursos de computação e os repositórios de armazenamento no estúdio.

O estúdio também é onde você acessa as ferramentas interativas que fazem parte do Azure Machine Learning:

+ [Designer de Azure Machine Learning (versão prévia)](concept-designer.md) para executar etapas de fluxo de trabalho sem escrever código
+ Experiência na Web para o [aprendizado de máquina automatizado](concept-automated-ml.md)
+ [Azure Machine Learning notebooks](how-to-run-jupyter-notebooks.md) para escrever e executar seu próprio código em servidores integrados do Jupyter notebook.
+ [Dados rotulando projetos](how-to-create-labeling-projects.md) para criar, gerenciar e monitorar projetos para rotular seus dados

### <a name="programming-tools"></a>Ferramentas de programação

> [!IMPORTANT]
> As ferramentas marcadas (visualização) abaixo estão atualmente em visualização pública.
> A versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

+  Interaja com o serviço em qualquer ambiente Python com o [SDK do Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true).
+ Interaja com o serviço em qualquer ambiente do R com o [SDK do Azure Machine Learning para R](https://azure.github.io/azureml-sdk-for-r/reference/index.html) (versão prévia).
+ Use [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) para automação.
+ O [Acelerador de Solução de Muitos Modelos](https://aka.ms/many-models) (versão prévia) é baseado no Azure Machine Learning e permite treinar, operar e gerenciar centenas ou até milhares de modelos de machine learning.

## <a name="next-steps"></a>Próximas etapas

Para começar a usar o Azure Machine Learning, confira:

* [O que é Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Criar um workspace de Azure Machine Learning](how-to-manage-workspace.md)
* [Tutorial (parte 1): Treinar um modelo](tutorial-train-models-with-aml.md)
