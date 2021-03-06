---
title: incluir arquivo
description: incluir arquivo
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.author: diberry
ms.openlocfilehash: 956aa308bf1cb3736c491031239661ec6b295ddb
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77279721"
---
O aplicativo cliente precisa saber se um enunciado não é significativo ou apropriado para o aplicativo. A intenção **None** é adicionada a cada aplicativo como parte do processo de criação para determinar se um enunciado não deve ser atendido pelo aplicativo cliente.

Se o LUIS retornar a intenção **None** para um enunciado, o aplicativo cliente poderá perguntar se o usuário deseja terminar a conversa ou fornecer mais orientações para continuar a conversa.

Se você deixar a intenção **Nenhuma** vazia, um enunciado que deve ser previsto fora do domínio do assunto será previsto em uma das intenções existentes do domínio do assunto. O resultado é que o aplicativo cliente, assim como um chatbot, executará operações incorretas com base em uma previsão incorreta.

1. Selecione **Intenções** no painel esquerdo.

1. Selecione a intenção **None**. Adicione três enunciados que o usuário pode inserir, mas que não são relevantes para o seu aplicativo para pedidos de pizza:

    |Enunciados de exemplo de `None`|
    |--|
    |`Barking dogs are annoying`|
    |`Penguins in the ocean`|

    Esses exemplos não devem usar palavras que você espera no domínio do seu assunto, como `pizza`, `cheese`, `crust`, `pickup` `deliver`.