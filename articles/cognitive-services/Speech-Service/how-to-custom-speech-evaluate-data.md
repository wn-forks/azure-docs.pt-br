---
title: Avaliar a precisão do serviço de Fala Personalizada-fala
titleSuffix: Azure Cognitive Services
description: Neste documento, você aprenderá a medir de forma quantitativa a qualidade do nosso modelo de fala-para-texto ou do modelo personalizado. Áudio e dados de transcrição com rótulo humano são necessários para testar a precisão e 30 minutos a 5 horas de áudio representativo devem ser fornecidos.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: cadbe79bbe0af2b5cebacb3d0c7c4e910fc7dbb8
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85856839"
---
# <a name="evaluate-custom-speech-accuracy"></a>Avaliar a precisão da Fala Personalizada

Neste documento, você aprenderá a medir de forma quantitativa a qualidade do modelo de fala-para-texto da Microsoft ou de seu modelo personalizado. Áudio e dados de transcrição com rótulo humano são necessários para testar a precisão e 30 minutos a 5 horas de áudio representativo devem ser fornecidos.

## <a name="what-is-word-error-rate-wer"></a>O que é a taxa de erros do Word (WER)?

O padrão do setor para medir a precisão do modelo é o WER ( *taxa de erros do Word* ). O WER conta o número de palavras incorretas identificadas durante o reconhecimento e divide pelo número total de palavras fornecidas na transcrição de rótulo humano (mostrada abaixo como N). Por fim, esse número é multiplicado por 100% para calcular o WER.

![Fórmula do WER](./media/custom-speech/custom-speech-wer-formula.png)

Palavras incorretamente identificadas se enquadram em três categorias:

* Inserção (I): palavras que são adicionadas incorretamente na transcrição de hipótese
* Exclusão (D): palavras que não são detectadas na transcrição da hipótese
* Substituição (ões): palavras que foram substituídas entre referência e hipótese

Aqui está um exemplo:

![Exemplo de palavras identificadas incorretamente](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>Resolver erros e aprimorar o WER

Você pode usar o WER nos resultados de reconhecimento do computador para avaliar a qualidade do modelo que você está usando com seu aplicativo, ferramenta ou produto. Um WER de 5% a 10% é considerado uma boa qualidade e está pronto para uso. Um WER de 20% é aceitável, mas talvez você queira considerar treinamento adicional. Um WER de 30% ou mais sinais de baixa qualidade e requer personalização e treinamento.

A forma como os erros são distribuídos é importante. Quando muitos erros de exclusão são encontrados, isso geralmente ocorre devido à intensidade do sinal de áudio fraco. Para resolver esse problema, você precisará coletar dados de áudio mais perto da origem. Erros de inserção significam que o áudio foi registrado em um ambiente barulhento e crosstalk pode estar presente, causando problemas de reconhecimento. Erros de substituição geralmente são encontrados quando uma amostra insuficiente de termos específicos do domínio foi fornecida como transcrições com rótulo humano ou texto relacionado.

Ao analisar arquivos individuais, você pode determinar quais tipos de erros existem e quais erros são exclusivos para um arquivo específico. Entender os problemas no nível de arquivo ajudará você a aprimorar as melhorias.

## <a name="create-a-test"></a>Criar um teste

Se você quiser testar a qualidade do modelo de linha de base de fala para texto da Microsoft ou de um modelo personalizado que você tenha treinado, você pode comparar dois modelos lado a lado para avaliar a precisão. A comparação inclui resultados de WER e de reconhecimento. Normalmente, um modelo personalizado é comparado com o modelo de linha de base da Microsoft.

Para avaliar modelos lado a lado:

1. Entre no portal de [fala personalizada](https://speech.microsoft.com/customspeech).
2. Navegue até **conversão de fala em texto > Fala Personalizada > [nome do projeto] > teste**.
3. Clique em **Adicionar teste**.
4. Selecione **avaliar exatidão**. Dê ao teste um nome, uma descrição e selecione seu conjunto de testes de áudio e de transcrição com rótulo humano.
5. Selecione até dois modelos que você gostaria de testar.
6. Clique em **Criar**.

Depois que o teste tiver sido criado com êxito, você poderá comparar os resultados lado a lado.

## <a name="side-by-side-comparison"></a>Comparação lado a lado

Depois que o teste for concluído, indicado pela alteração do status para *êxito*, você encontrará um número do WER para ambos os modelos incluídos no teste. Clique no nome do teste para exibir a página de detalhes de teste. Essa página de detalhes lista todos os declarações no conjunto de seus conjuntos de anotações, indicando os resultados de reconhecimento dos dois modelos junto com a transcrição do conjunto de resultados enviado. Para ajudar a inspecionar a comparação lado a lado, você pode alternar vários tipos de erro, incluindo inserção, exclusão e substituição. Ao ouvir o áudio e comparar os resultados de reconhecimento em cada coluna, que mostra a transcrição de rótulo humano e os resultados de dois modelos de fala em texto, você pode decidir qual modelo atende às suas necessidades e onde são necessários treinamentos e aprimoramentos adicionais.

## <a name="next-steps"></a>Próximas etapas

* [Treinar seu modelo](how-to-custom-speech-train-model.md)
* [Implantar o seu modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionais

* [Preparar e testar seus dados](how-to-custom-speech-test-data.md)
* [Inspecione seus dados](how-to-custom-speech-inspect-data.md)
