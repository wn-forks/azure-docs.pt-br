---
title: Planejar eventos de manutenção do Azure
description: Saiba como se preparar para eventos de manutenção planejada no banco de dados SQL do Azure e no Azure SQL Instância Gerenciada.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
ms.date: 08/25/2020
ms.openlocfilehash: 4c7b78f14602632068a19d520aeeb940b543be61
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88948208"
---
# <a name="plan-for-azure-maintenance-events-in-azure-sql-database-and-azure-sql-managed-instance"></a>Planejar eventos de manutenção do Azure no banco de dados SQL do Azure e no Azure SQL Instância Gerenciada
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Saiba como se preparar para eventos de manutenção planejada em seu banco de dados no banco de dados SQL do Azure e no Azure SQL Instância Gerenciada.

## <a name="what-is-a-planned-maintenance-event"></a>O que é um evento de manutenção planejada?

Para manter o banco de dados SQL do Azure e o Azure SQL Instância Gerenciada Services seguros, compatíveis, estáveis e com bom desempenho, as atualizações estão sendo executadas por meio dos componentes de serviço quase continuamente. Graças à arquitetura de serviço moderna e robusta e às tecnologias inovadoras, como a [aplicação de patches](https://aka.ms/azuresqlhotpatching), a maioria das atualizações é totalmente transparente e sem impacto em termos de disponibilidade do serviço. Ainda assim, poucos tipos de atualizações causam interrupções de serviço curtas e exigem tratamento especial. 

Para cada banco de dados, o banco de dados SQL do Azure e o Azure SQL Instância Gerenciada mantêm um quorum de réplicas de banco de dados em que uma réplica é a primária. Em todos os momentos, uma réplica primária deve estar em manutenção online e pelo menos uma réplica secundária deve estar íntegra. Durante a manutenção planejada, os membros do quorum do banco de dados ficarão offline um por vez com a intenção de que há uma réplica primária respondendo e pelo menos uma réplica secundária online para garantir que não haja tempo de inatividade do cliente. Quando a réplica primária precisa ser colocada offline, ocorrerá um processo de reconfiguração/failover no qual uma réplica secundária se tornará a nova primária.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>O que esperar durante um evento de manutenção planejada

O evento de manutenção pode produzir failovers únicos ou múltiplos, dependendo do Constellation das réplicas primárias e secundárias no início do evento de manutenção. Em média, os failovers de 1,7 ocorrem por evento de manutenção planejada. As reconfigurações/failovers geralmente são concluídos em 30 segundos. A média é de 8 segundos. Se já estiver conectado, seu aplicativo deverá se reconectar à nova réplica primária do banco de dados. Se uma nova conexão for tentada enquanto o banco de dados estiver passando por uma reconfiguração antes que a nova réplica primária esteja online, você receberá o erro 40613 (banco de dados indisponível): *"o banco de dados ' {DatabaseName} ' no servidor ' {servername} ' não está disponível no momento. Repita a conexão mais tarde. "* Se o banco de dados tiver uma consulta de execução longa, essa consulta será interrompida durante uma reconfiguração e precisará ser reiniciada.

## <a name="how-to-simulate-a-planned-maintenance-event"></a>Como simular um evento de manutenção planejada

Garantir que seu aplicativo cliente seja resiliente a eventos de manutenção antes da implantação na produção ajudará a reduzir o risco de falhas de aplicativo e contribuirá para a disponibilidade do aplicativo para seus usuários finais. Você pode testar o comportamento do aplicativo cliente durante os eventos de manutenção planejada [iniciando o failover manual](https://aka.ms/mifailover-techblog) por meio do PowerShell, da CLI ou da API REST. Ele produzirá comportamento idêntico à medida que o evento de manutenção colocar a réplica primária offline.

## <a name="retry-logic"></a>Lógica de repetição

Qualquer aplicativo de produção cliente que se conecta a um serviço de banco de dados de nuvem deve implementar uma [lógica de repetição](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors) de conexão robusta. Isso ajudará a tornar os failovers transparentes para os usuários finais ou, pelo menos, minimizar os efeitos negativos.

## <a name="resource-health"></a>Integridade de recursos

Se o banco de dados estiver apresentando falhas de logon, verifique a janela de [Resource Health](../../service-health/resource-health-overview.md#get-started) no [portal do Azure](https://portal.azure.com) para obter o status atual. A seção Histórico de Integridade contém o motivo do tempo de inatividade para cada evento (quando disponível).

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Resource Health](resource-health-to-troubleshoot-connectivity.md) para o banco de dados SQL do Azure e o SQL instância gerenciada do Azure.
- Para obter mais informações sobre a lógica de repetição, consulte [lógica de repetição para erros transitórios](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors).
