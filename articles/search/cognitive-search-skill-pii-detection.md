---
title: Habilidade cognitiva de detecção de PII (versão prévia)
titleSuffix: Azure Cognitive Search
description: Extraia e mascarar informações de identificação pessoal do texto em um pipeline de enriquecimento no Azure Pesquisa Cognitiva. Essa habilidade está atualmente em versão prévia pública.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: b2e35ba083e376f519ccbc32c71c1ac9b1e03a41
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935289"
---
#    <a name="pii-detection-cognitive-skill"></a>Habilidade cognitiva de detecção de PII

> [!IMPORTANT] 
> Essa habilidade está atualmente em versão prévia pública. A funcionalidade de versão prévia é fornecida sem um Contrato de Nível de Serviço e, portanto, não é recomendada para cargas de trabalho de produção. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). No momento, não há suporte para Portal ou SDK do .NET.

A habilidade de **detecção de PII** extrai informações de identificação pessoal de um texto de entrada e oferece a opção de mascarar o texto de várias maneiras. Essa habilidade usa os modelos de machine learning fornecidos pela [Análise de Texto](../cognitive-services/text-analytics/overview.md) nos Serviços Cognitivos.

> [!NOTE]
> À medida que expandir o escopo aumentando a frequência de processamento, adicionando mais documentos ou adicionando mais algoritmos de IA, você precisará [anexar um recurso de Serviços Cognitivos faturável](cognitive-search-attach-cognitive-services.md). As cobranças são geradas ao chamar APIs nos Serviços Cognitivos e para a extração de imagem, como parte do estágio de quebra de documento na Pesquisa Cognitiva do Azure. Não há encargos para extração de texto em documentos.
>
> A execução de habilidades integradas é cobrada nos [preços pagos conforme o uso dos Serviços Cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/) existentes. O preço da extração de imagem é descrito na [página de preços da Pesquisa Cognitiva do Azure](https://azure.microsoft.com/pricing/details/search/).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.PIIDetectionSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registro deve ser de 50.000 caracteres conforme medido por [`String.Length`](/dotnet/api/system.string.length). Se você precisar dividir seus dados antes de enviá-los para a habilidade, considere o uso da [habilidade de divisão de texto](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parâmetros de habilidades

Os parâmetros diferenciam maiúsculas de minúsculas e todos são opcionais.

| Nome do parâmetro     | Descrição |
|--------------------|-------------|
| `defaultLanguageCode` |    Código de idioma do texto de entrada. Por enquanto, há `en` suporte apenas para. |
| `minimumPrecision` | Um valor entre 0,0 e 1,0. Se a pontuação de confiança (na `piiEntities` saída) for menor do que o `minimumPrecision` valor definido, a entidade não será retornada nem mascarada. O padrão é 0.0. |
| `maskingMode` | Um parâmetro que fornece várias maneiras de mascarar a PII detectada no texto de entrada. Há suporte para as seguintes opções: <ul><li>`none` (padrão): isso significa que nenhuma máscara será executada e a `maskedText` saída não será retornada. </li><li> `redact`: Esta opção removerá as entidades detectadas do texto de entrada e não as substituirá por nada. Observe que, nesse caso, o deslocamento na `piiEntities` saída será em relação ao texto original e não ao texto mascarado. </li><li> `replace`: Esta opção substituirá as entidades detectadas pelo caractere fornecido no `maskingCharacter` parâmetro.  O caractere será repetido para o comprimento da entidade detectada para que os deslocamentos correspondam corretamente ao texto de entrada, bem como à saída `maskedText` .</li></ul> |
| `maskingCharacter` | O caractere que será usado para mascarar o texto se o `maskingMode` parâmetro for definido como `replace` . Há suporte para as seguintes opções: `*` (padrão), `#` , `X` . Esse parâmetro só pode ser `null` se `maskingMode` não estiver definido como `replace` . |


## <a name="skill-inputs"></a>Entradas de habilidades

| Nome de entrada      | Descrição                   |
|---------------|-------------------------------|
| `languageCode`    | Opcional. O padrão é `en`.  |
| `text`          | O texto para analisar.          |

## <a name="skill-outputs"></a>Saídas de habilidades

| Nome de saída      | DESCRIÇÃO                   |
|---------------|-------------------------------|
| `piiEntities` | Uma matriz de tipos complexos que contêm os seguintes campos: <ul><li>texto (a PII real como extraída)</li> <li>type</li><li>Subtipo</li><li>Score (maior valor significa que é mais provável que seja uma entidade real)</li><li>deslocamento (no texto de entrada)</li><li>comprimento</li></ul> </br> [Possíveis tipos e subtipos podem ser encontrados aqui.](../cognitive-services/text-analytics/named-entity-types.md?tabs=personal) |
| `maskedText` | Se `maskingMode` for definido como um valor diferente de `none` , essa saída será o resultado da cadeia de caracteres do mascaramento executado no texto de entrada, conforme descrito pelo selecionado `maskingMode` .  Se `maskingMode` for definido como `none` , essa saída não estará presente. |

##    <a name="sample-definition"></a>Definição de exemplo

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.PIIDetectionSkill",
    "defaultLanguageCode": "en",
    "minimumPrecision": 0.5,
    "maskingMode": "replace",
    "maskingCharacter": "*",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "piiEntities"
      },
      {
        "name": "maskedText"
      }
    ]
  }
```
##    <a name="sample-input"></a>Entrada de exemplo

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Microsoft employee with ssn 859-98-0987 is using our awesome API's."
           }
      }
    ]
}
```

##    <a name="sample-output"></a>Saída de exemplo

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "piiEntities":[ 
           { 
              "text":"859-98-0987",
              "type":"U.S. Social Security Number (SSN)",
              "subtype":"",
              "offset":28,
              "length":11,
              "score":0.65
           }
        ],
        "maskedText": "Microsoft employee with ssn *********** is using our awesome API's."
      }
    }
  ]
}
```

Observe que os deslocamentos retornados para entidades na saída dessa habilidade são retornados diretamente da [API de análise de texto](../cognitive-services/text-analytics/overview.md), o que significa que, se você estiver usando-os para indexar na cadeia de caracteres original, deverá usar a classe [StringInfo](/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) no .net para extrair o conteúdo correto.  [Mais detalhes podem ser encontrados aqui.](../cognitive-services/text-analytics/concepts/text-offsets.md)

## <a name="error-and-warning-cases"></a>Casos de erro e de aviso
Se não houver suporte para o código de idioma do documento, um aviso será retornado e nenhuma entidade será extraída.
Se o texto estiver vazio, um aviso será gerado.
Se o texto for maior que 50.000 caracteres, somente os primeiros 50.000 caracteres serão analisados e um aviso será emitido.

Se a habilidade retornar um aviso, a saída `maskedText` poderá estar vazia.  Isso significa que, se você espera que a saída exista para entrada em habilidades posteriores, ela não funcionará conforme o esperado. Tenha isso em mente ao escrever sua definição de Skills.

## <a name="see-also"></a>Confira também

+ [Habilidades internas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de qualificações](cognitive-search-defining-skillset.md)