---
title: Controles de segurança para hubs de eventos do Azure
description: Este artigo fornece uma lista de verificação de controles de segurança para avaliar os hubs de eventos do Azure (rede, identidade, proteção de dados, etc.).
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: da20778f1e24372e445d635e675df6484905f195
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85315389"
---
# <a name="security-controls-for-azure-event-hubs"></a>Controles de segurança para hubs de eventos do Azure

Este artigo documenta os controles de segurança criados nos hubs de eventos do Azure.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Rede

| Controle de segurança | Sim/Não | Observações | Documentação |
|---|---|--|--|
| Suporte ao ponto de extremidade de serviço| Sim |  |  |
| Suporte à injeção de VNet| Não | |  |
| Isolamento de rede e suporte de firewall| Sim |  |  |
| Suporte a túnel forçado| Não |  |  |

## <a name="monitoring--logging"></a>Monitorando & log

| Controle de segurança | Sim/Não | Observações| Documentação |
|---|---|--|--|
| Suporte ao monitoramento do Azure (log Analytics, app insights, etc.)| Sim | |  |
| Registro e auditoria do plano de gerenciamento e controle| Sim |  |  |
| Log e auditoria do plano de dados| Sim |   |  |

## <a name="identity"></a>Identidade

| Controle de segurança | Sim/Não | Observações| Documentação |
|---|---|--|--|
| Autenticação| Sim | | [Autorizar o acesso aos hubs de eventos do Azure](authorize-access-event-hubs.md), [autorizar o acesso aos recursos dos hubs de eventos usando Azure Active Directory](authorize-access-azure-active-directory.md), [autorizando o acesso aos recursos dos hubs de eventos usando assinaturas de acesso compartilhado](authorize-access-shared-access-signature.md) |
| Autorização|  Sim | | [Autenticar uma identidade gerenciada com Azure Active Directory para acessar recursos de hubs de eventos](authenticate-managed-identity.md), [autenticar um aplicativo com Azure Active Directory para acessar recursos de hubs de eventos](authenticate-application.md), [autenticar o acesso a recursos de hubs de eventos usando SAS (assinaturas de acesso compartilhado)](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>Proteção de dados

| Controle de segurança | Sim/Não | Observações | Documentação |
|---|---|--|--|
| Criptografia no lado do servidor em repouso: chaves gerenciadas pela Microsoft |  Sim | |  |
| Criptografia no lado do servidor em repouso: chaves gerenciadas pelo cliente (BYOK) | Sim. Disponível para clusters dedicados. | Uma chave gerenciada pelo cliente no Azure keyvault pode ser usada para criptografar os dados em um hub de eventos em repouso. | [Configurar chaves gerenciadas pelo cliente para criptografar dados de hubs de eventos do Azure em repouso usando o portal do Azure](configure-customer-managed-key.md) |
| Criptografia em nível de coluna (serviços de dados do Azure)| N/D | |  |
| Criptografia em trânsito (como criptografia de ExpressRoute, criptografia de vnet e criptografia vnet)| Sim | |  |
| Chamadas criptografadas à API| Sim |  |  |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Controle de segurança | Sim/Não | Observações| Documentação |
|---|---|--|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | |  |

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre os [controles de segurança internos nos serviços do Azure](../security/fundamentals/security-controls.md).
