---
title: incluir arquivo
description: incluir arquivo
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2018
ms.author: nzthiago
ms.custom: include file
ms.openlocfilehash: eca2d3359614875e5bff0c9bb67f006f0a8cdba1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77198319"
---
## <a name="function-app-timeout-duration"></a><a name="timeout"></a>Duração do tempo limite do aplicativo de funções 

A duração do tempo limite de um aplicativo de funções é definida pela `functionTimeout` propriedade no [host.jsno](../articles/azure-functions/functions-host-json.md#functiontimeout) arquivo de projeto. A tabela a seguir mostra os valores padrão e máximo em minutos para os planos e para as diferentes versões de tempo de execução:

| Plano | Versão de tempo de execução | Padrão | Máximo |
|------|---------|---------|---------|
| Consumo | 1.x | 5 | 10 |
| Consumo | 2. x | 5 | 10 |
| Consumo | 3.x | 5 | 10 |
| Premium | 1.x | 30 | Ilimitado |
| Premium | 2. x | 30 | Ilimitado |
| Premium | 3.x | 30 | Ilimitado |
| Serviço de Aplicativo | 1.x | Ilimitado | Ilimitado |
| Serviço de Aplicativo | 2. x | 30 | Ilimitado |
| Serviço de Aplicativo | 3.x | 30 | Ilimitado |

> [!NOTE] 
> Independentemente da configuração do tempo limite do aplicativo de funções, 230 segundos é a quantidade máxima de tempo que uma função disparada por HTTP pode levar para responder a uma solicitação. Isso ocorre devido ao [tempo limite de ociosidade padrão de Azure Load Balancer](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Para tempos de processamento mais longos, considere usar o [padrão assíncrono Durable Functions](../articles/azure-functions/durable/durable-functions-overview.md#async-http) ou [adiar o trabalho real e retornar uma resposta imediata](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions).
