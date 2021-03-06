---
title: Política de gerenciamento de API de exemplo – autorizar solicitação usando o autorizador externo
titleSuffix: Azure API Management
description: Exemplo de política de gerenciamento de API do Azure – demonstra como autorizar solicitações usando um autorizador externo encapsulando uma lógica de autenticação/autorização personalizada ou herdada.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 99bf1068042eb7ab0c43e2a683ca7116d2e426f3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75442492"
---
# <a name="authorize-requests-using-external-authorizer"></a>Autorizar solicitações usando um autorizador externo

Este artigo mostra um exemplo de política de gerenciamento de API do Azure que demonstra como proteger o acesso à API usando um autorizador externo que encapsula uma lógica de autenticação/autorização personalizada. Para definir ou editar um código de política, siga as etapas descritas em [Definir ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [exemplos de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código no bloco de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as políticas de APIM:

+ [Políticas de restrição de acesso](../api-management-access-restriction-policies.md)
+ [Exemplos de política](../policy-samples.md)
