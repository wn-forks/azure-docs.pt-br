---
title: Como executar um teste em lote-LUIS
titleSuffix: Azure Cognitive Services
description: Use os conjuntos de testes em lote do KUIS (Reconhecimento vocal) para localizar enunciados com intenções e entidades incorretas.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/17/2020
ms.author: diberry
ms.openlocfilehash: ee15d08c820492cfa2c0701dfe3b682d57e00187
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344400"
---
# <a name="batch-testing-with-a-set-of-example-utterances"></a>Testes em lote com um conjunto de exemplos de enunciados

 O teste de lote é um teste abrangente em seu modelo treinado atual para medir seu desempenho no LUIS. Os conjuntos de dados usados para teste em lotes não devem incluir exemplos de enunciados nas intenções ou enunciados recebidos do ponto de extremidade de runtime de previsão.

<a name="batch-testing"></a>

## <a name="import-a-dataset-file-for-batch-testing"></a>Importar um arquivo de conjunto de dados para teste de lote

1. Selecione **Testar** na barra superior e selecione **Painel de teste de lote**.

    ![Link de teste de lote](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Selecione **Importar conjunto de dados**. A caixa de diálogo **Importar novo conjunto de dados** é exibida. Selecione **Escolher Arquivo** e localize um arquivo JSON com o [formato JSON](luis-concept-batch-test.md#batch-file-format) correto que contenha * no máximo 1.000 * enunciados para testar.

    Erros de importação são relatados em uma barra de notificação vermelha na parte superior do navegador. Quando uma importação tem erros, nenhum conjunto de dados é criado. Para obter mais informações, confira [Erros comuns](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. No campo **Nome do conjunto de dados**, insira um nome para seu arquivo de conjunto de dados. O arquivo de conjunto de dados inclui uma **matriz de declarações** que inclui a *intenção rotulada* e *entidades*. Examine a sintaxe do [arquivo de lote de exemplo](luis-concept-batch-test.md#batch-file-format).

4. Selecione **Concluído**. O arquivo de conjunto de dados é adicionado.

## <a name="run-rename-export-or-delete-dataset"></a>Executar, renomear, exportar ou excluir o conjunto de dados

Para executar, renomear, exportar ou excluir o conjunto de dados, use o botão de reticências (***...***) no final da linha do conjunto de dados.

> [!div class="mx-imgBorder"]
> ![Captura de tela da lista de testes em lote com opções](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Executar um teste de lote em seu aplicativo treinado

Para executar o teste, selecione o nome do conjunto de um e, em seguida, selecione **executar** na barra de ferramentas contextual. Quando o teste for concluído, essa linha exibirá o resultado do teste do conjunto de dados.

O conjunto de dados que pode ser baixado é o mesmo arquivo que foi carregado para o teste de lote.

|Estado|Significado|
|--|--|
|![Ícone de círculo verde para teste com êxito](./media/luis-how-to-batch-test/batch-test-result-green.png)|Todas as declarações são bem-sucedidas.|
|![Ícone de x vermelho para teste com falha](./media/luis-how-to-batch-test/batch-test-result-red.png)|Pelo menos uma intenção de declaração não correspondeu à previsão.|
|![Ícone Pronto para testar](./media/luis-how-to-batch-test/batch-test-result-blue.png)|O teste está pronto para ser executado.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>

## <a name="view-batch-test-results"></a>Exibir resultados de teste de lote

Para examinar os resultados de teste de lote, selecione **Ver resultados**.

<a name="filter-chart-results-by-intent-or-entity"></a>

## <a name="filter-chart-results"></a>Filtrar resultados do gráfico

Para filtrar o gráfico por uma intenção ou entidade específica, selecione a intenção ou entidade no painel de filtragem direito. Os pontos de dados e sua distribuição são atualizados no grafo de acordo com sua seleção.

![Resultado do teste de lote visualizado](./media/luis-how-to-batch-test/filter-by-entity.png)

## <a name="view-single-point-utterance-data"></a>Exibir dados de declaração de ponto único

No gráfico, passe o mouse sobre um ponto de dados para ver a pontuação de certeza de sua previsão. Selecione um ponto de dados para recuperar sua declaração correspondente na lista de declarações na parte inferior da página.

![Declaração selecionada](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="view-section-data"></a>Exibir dados de seção

No gráfico de quatro seções, selecione o nome da seção, como **Falso Positivo** na parte superior direita do gráfico. Embaixo do gráfico, todas as declarações nessa seção são exibidas embaixo do gráfico em uma lista.

![Declarações selecionadas por seção](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

Nesta imagem anterior, a declaração `switch on` é rotulada com a intenção TurnAllOn, mas recebeu a previsão de Nenhuma intenção. Essa é uma indicação de que a intenção TurnAllOn precisa de mais declarações de exemplo para criar a previsão esperada.

As duas seções do gráfico em vermelho indicam declarações que não corresponderam à previsão esperada. Elas indicam declarações de que o LUIS precisa de mais treinamento.

As duas seções do gráfico em verde corresponderam à previsão esperada.

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Próximas etapas

Se o teste indica que seu aplicativo LUIS não reconhece as intenções e entidades corretas, é possível trabalhar para melhorar o desempenho do seu aplicativo LUIS rotulando mais declarações ou adicionando recursos.

* [Rotular enunciados sugeridos com o LUIS](luis-how-to-review-endpoint-utterances.md)
* [Usar recursos para melhorar o desempenho do aplicativo de LUIS](luis-how-to-add-features.md)
* [Noções básicas sobre o teste de lote com esse tutorial](luis-tutorial-batch-testing.md)
* [Aprenda conceitos de teste de lote](luis-concept-batch-test.md).
