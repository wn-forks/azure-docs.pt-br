---
title: Usar grupos dinâmicos com o Gerenciamento de Atualizações da Automação do Azure
description: Este artigo mostra como usar grupos dinâmicos com o Gerenciamento de Atualizações da Automação do Azure.
services: automation
ms.subservice: update-management
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 20e6d26808964c8e697c694bd796af2851e7ca48
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87449867"
---
# <a name="use-dynamic-groups-with-update-management"></a>Usar grupos dinâmicos com o Gerenciamento de Atualizações

O Gerenciamento de Atualizações permite que você direcione um grupo dinâmico de VMs do Azure ou não Azure para implantações de atualização. O uso de um grupo dinâmico evita que você precise editar sua implantação para atualizar computadores.

> [!NOTE]
> Grupos dinâmicos não funcionam com VMs clássicas.

É possível definir grupos dinâmicos para computadores Azure ou não Azure a partir do **Gerenciamento de Atualizações** no portal do Azure. Consulte [gerenciar atualizações para VMs](update-mgmt-manage-updates-for-vm.md).

Um grupo dinâmico é definido por uma consulta que a Automação do Azure avalia no momento da implantação. Mesmo se uma consulta de grupo dinâmico recuperar um grande número de máquinas, a Automação do Azure poderá processar apenas um máximo de 1.000 máquinas de cada vez. Veja [Assinatura do Azure e limites, cotas e restrições de serviço](../../azure-resource-manager/management/azure-subscription-service-limits.md#update-management).

> [!NOTE]
> Se quiser atualizar mais de 1.000 computadores, recomendamos que você divida as atualizações entre vários agendamentos de atualização. 

## <a name="define-dynamic-groups-for-azure-machines"></a>Definir grupos dinâmicos para computadores do Azure

Ao definir uma consulta de grupo dinâmico para computadores do Azure, é possível usar os seguintes itens para preencher o grupo dinâmico:

* Subscription
* Grupos de recursos
* Locais
* Marcas

![Selecionar grupos](./media/update-mgmt-groups/select-groups.png)

Para ver os resultados da consulta de grupo dinâmico, clique em **Visualizar**. A visualização mostra a associação de grupo no momento atual. No exemplo, estamos procurando computadores com a marca `Role` para o grupo **BackendServer**. Se mais computadores tiverem essa marca adicionada, eles serão adicionados a quaisquer implantações futuras em relação a esse grupo.

![grupos da visualização](./media/update-mgmt-groups/preview-groups.png)

## <a name="define-dynamic-groups-for-non-azure-machines"></a>Definir grupos dinâmicos para computadores não Azure

Um grupo dinâmico para computadores não Azure usa pesquisas salvas, também chamadas de grupos de computadores. Para saber como criar uma pesquisa salva, veja [Criar um grupo de computadores](../../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Depois que a pesquisa salva for criada, você poderá selecioná-la na lista de pesquisas salvas em **Gerenciamento de Atualizações** no portal do Azure. Clique em **Visualizar** para ver os computadores na pesquisa salva.

![Selecionar grupos](./media/update-mgmt-groups/select-groups-2.png)

## <a name="next-steps"></a>Próximas etapas

Você pode [consultar logs de Azure monitor](update-mgmt-query-logs.md) para analisar avaliações de atualização, implantações e outras tarefas de gerenciamento relacionadas. Ele inclui consultas predefinidas para ajudá-lo a começar.
