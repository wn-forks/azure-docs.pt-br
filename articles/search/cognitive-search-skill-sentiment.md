---
title: Habilidade cognitiva do Sentiment
titleSuffix: Azure Cognitive Search
description: Extraia uma pontuação de sentimentos positiva negativa do texto em um pipeline de enriquecimento de ia no Azure Pesquisa Cognitiva.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: 895e22d94122539ee68bcac8ee4debce2d824ff0
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935272"
---
# <a name="sentiment-cognitive-skill"></a>Habilidade cognitiva do Sentiment

A habilidade do **Sentiment** avalia o texto não estruturado ao longo de um conjunto negativo positivo e para cada registro, retorna uma pontuação numérica entre 0 e 1. Pontuações próximas de 1 indicam sentimento positivo e pontuações próximas de 0 indicam sentimento negativo. Essa habilidade usa os modelos de machine learning fornecidos pela [Análise de Texto](../cognitive-services/text-analytics/overview.md) nos Serviços Cognitivos.

> [!NOTE]
> À medida que expandir o escopo aumentando a frequência de processamento, adicionando mais documentos ou adicionando mais algoritmos de IA, você precisará [anexar um recurso de Serviços Cognitivos faturável](cognitive-search-attach-cognitive-services.md). As cobranças são geradas ao chamar APIs nos Serviços Cognitivos e para a extração de imagem, como parte do estágio de quebra de documento na Pesquisa Cognitiva do Azure. Não há encargos para extração de texto em documentos.
>
> A execução de habilidades integradas é cobrada nos [preços pagos conforme o uso dos Serviços Cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/) existentes. O preço da extração de imagem é descrito na [página de preços da Pesquisa Cognitiva do Azure](https://azure.microsoft.com/pricing/details/search/).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registro deve ser de 5000 caracteres conforme medido por [`String.Length`](/dotnet/api/system.string.length) . Se você precisar interromper o backup de seus dados antes de enviá-lo ao analisador de sentimentos, use a [habilidade Text Split](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Parâmetros de habilidades

Os parâmetros diferenciam maiúsculas de minúsculas.

| Nome do Parâmetro | DESCRIÇÃO |
|----------------|----------------------|
| `defaultLanguageCode` | (opcional) O código de idioma a ser aplicado a documentos que não especifica explicitamente o idioma. <br/> Consulte [Lista completa dos idiomas com suporte](../cognitive-services/text-analytics/language-support.md) |

## <a name="skill-inputs"></a>Entradas de habilidades 

| Nome de entrada | DESCRIÇÃO |
|--------------------|-------------|
| `text` | O texto a ser analisado.|
| `languageCode`    |  (opcional) Uma cadeia de caracteres que indica o idioma dos registros. Se este parâmetro não for especificado, o valor padrão é “en”. <br/>Consulte [Lista completa dos idiomas com suporte](../cognitive-services/text-analytics/language-support.md).|

## <a name="skill-outputs"></a>Saídas de habilidades

| Nome de saída | DESCRIÇÃO |
|--------------------|-------------|
| `score` | Um valor entre 0 e 1 que representa o sentimento do texto analisado. Valores próximos a 0 têm sentimento negativo, perto de 0,5, têm sentimento neutro, e valores próximo a 1 têm o sentimento positivo.|


##  <a name="sample-definition"></a>Definição de exemplo

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
        }
    ]
}
```

##  <a name="sample-input"></a>Entrada de exemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Saída de exemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Observações
Se estiver vazio, uma pontuação de sensibilidade não é retornada para os registros.

## <a name="error-cases"></a>Casos de erro
Se não há suporte para um idioma, será gerado um erro e nenhuma pontuação de sensibilidade é retornada.

## <a name="see-also"></a>Confira também

+ [Habilidades internas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de qualificações](cognitive-search-defining-skillset.md)