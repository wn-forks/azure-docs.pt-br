---
title: Remover recursos de uma coleção de movimentação no Azure Resource mover
description: Saiba como remover recursos de uma coleção de movimentação no Azure Resource mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/08/2020
ms.author: raynew
ms.openlocfilehash: 241ccbda67f7a2518d0c44a0d362673922ad4284
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652764"
---
# <a name="remove-resources-from-a-move-collection"></a>Remover recursos de uma coleção de movimentação

Este artigo descreve como remover recursos de uma coleção de movimentação no [Azure Resource mover](overview.md). As coleções de movimentação são usadas ao mover os recursos do Azure entre regiões do Azure.

## <a name="remove-a-resource-portal"></a>Remover um recurso (Portal)

Remova no portal do Resource mover da seguinte maneira:

1. Em **várias regiões**, selecione os recursos que você deseja remover da coleção > **remover**.

    ![Botão para selecionar a remoção](./media/remove-move-resources/portal-select-resources.png)

1. Em **remover recursos**, clique em **remover**.

    ![Botão para selecionar para remover recursos de uma coleção de movimentação](./media/remove-move-resources/remove-portal.png)

## <a name="remove-a-resource-powershell"></a>Remover um recurso (PowerShell)

Remova um recurso (em nosso exemplo, os computadores PSDemoVM) de uma coleção usando o PowerShell, da seguinte maneira:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus - Name PSDemoVM
```
**Expected output** 
 Saída ![ esperada Texto de saída após a remoção de um recurso de uma coleção de movimentação](./media/remove-move-resources/remove-resource.png)

## <a name="remove-a-collection-powershell"></a>Remover uma coleção (PowerShell)

Remova uma coleção de movimentação inteira usando o PowerShell, da seguinte maneira:

```azurepowershell-interactive
# Remove a resource using the resource ID
Remove-AzResourceMoverMoveResource -SubscriptionId  <subscription-id> -ResourceGroupName RegionMoveRG-centralus-westcentralus  -MoveCollectionName MoveCollection-centralus-westcentralus 
```
**Expected output** 
 Saída ![ esperada Texto de saída após a remoção de uma coleção de movimentação](./media/remove-move-resources/remove-collection.png)

## <a name="vm-resource-state-after-removing"></a>Estado do recurso da VM após a remoção

O que acontece quando você remove um recurso de VM de uma coleção de movimentação depende do estado do recurso, conforme resumido na tabela.

###  <a name="remove-vm-state"></a>Remover estado da VM
**Estado do recurso** | **VM** | **Rede**
--- | --- | --- 
**Adicionado à coleção de movimentação** | Excluir da coleção de movimentação. | Excluir da coleção de movimentação. 
**Dependências resolvidas/preparadas pendentes** | Excluir da coleção de movimentação  | Excluir da coleção de movimentação. 
**Preparação em andamento**<br/> (ou qualquer outro Estado em andamento) | A operação de exclusão falha com erro.  | A operação de exclusão falha com erro.
**Falha na preparação** | Excluir da coleção de movimentação.<br/>Exclua tudo criado na região de destino, incluindo discos de réplica. <br/><br/> Os recursos de infraestrutura criados durante a movimentação precisam ser excluídos manualmente. | Excluir da coleção de movimentação.  
**Iniciar movimentação pendente** | Excluir da coleção de movimentação.<br/><br/> Exclua qualquer coisa criada na região de destino, incluindo VM, discos de réplica, etc.  <br/><br/> Os recursos de infraestrutura criados durante a movimentação precisam ser excluídos manualmente. | Excluir da coleção de movimentação.
**Falha ao iniciar movimentação** | Excluir da coleção de movimentação.<br/><br/> Exclua qualquer coisa criada na região de destino, incluindo VM, discos de réplica, etc.  <br/><br/> Os recursos de infraestrutura criados durante a movimentação precisam ser excluídos manualmente. | Excluir da coleção de movimentação.
**Confirmação pendente** | É recomendável descartar a movimentação para que os recursos de destino sejam excluídos primeiro.<br/><br/> O recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí. | É recomendável descartar a movimentação para que os recursos de destino sejam excluídos primeiro.<br/><br/> O recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí. 
**Falha na confirmação** | É recomendável descartar o para que os recursos de destino sejam excluídos primeiro.<br/><br/> O recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí. | É recomendável descartar a movimentação para que os recursos de destino sejam excluídos primeiro.<br/><br/> O recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí.
**Descarte concluído** | O recurso volta para o estado **Iniciar movimentação pendente** .<br/><br/> Ele é excluído da coleção de movimentação, junto com qualquer coisa criada em Target-VM, discos de réplica, cofre, etc.  <br/><br/> Os recursos de infraestrutura criados durante a movimentação precisam ser excluídos manualmente. <br/><br/> Os recursos de infraestrutura criados durante a movimentação precisam ser excluídos manualmente. |  O recurso volta para o estado **Iniciar movimentação pendente** .<br/><br/> Ele é excluído da coleção de movimentação.
**Falha ao descartar** | É recomendável descartar as movimentações para que os recursos de destino sejam excluídos primeiro.<br/><br/> Depois disso, o recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí. | É recomendável descartar as movimentações para que os recursos de destino sejam excluídos primeiro.<br/><br/> Depois disso, o recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí.
**Excluir origem pendente** | Excluído da coleção de movimentação.<br/><br/> Ele não exclui nada criado na região de destino.  | Excluído da coleção de movimentação.<br/><br/> Ele não exclui nada criado na região de destino.
**Falha ao excluir origem** | Excluído da coleção de movimentação.<br/><br/> Ele não exclui nada criado na região de destino. | Excluído da coleção de movimentação.<br/><br/> Ele não exclui nada criado na região de destino.

## <a name="sql-resource-state-after-removing"></a>Estado do recurso SQL após a remoção

O que acontece quando você remove um recurso SQL do Azure de uma coleção de movimentação depende do estado do recurso, conforme resumido na tabela.

**Estado do recurso** | **SQL** 
--- | --- 
**Adicionado à coleção de movimentação** | Excluir da coleção de movimentação. 
**Dependências resolvidas/preparadas pendentes** | Excluir da coleção de movimentação 
**Preparação em andamento**<br/> (ou qualquer outro Estado em andamento)  | A operação de exclusão falha com erro. 
**Falha na preparação** | Excluir da coleção de movimentação<br/><br/>Exclua tudo criado na região de destino. 
**Iniciar movimentação pendente** |  Excluir da coleção de movimentação<br/><br/>Exclua tudo criado na região de destino. O banco de dados SQL existe neste ponto e será excluído. 
**Falha ao iniciar movimentação** | Excluir da coleção de movimentação<br/><br/>Exclua tudo criado na região de destino. O banco de dados SQL existe neste ponto e deve ser excluído. 
**Confirmação pendente** | É recomendável descartar a movimentação para que os recursos de destino sejam excluídos primeiro.<br/><br/> O recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí.
**Falha na confirmação** | É recomendável descartar a movimentação para que os recursos de destino sejam excluídos primeiro.<br/><br/> O recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí. 
**Descarte concluído** |  O recurso volta para o estado **Iniciar movimentação pendente** .<br/><br/> Ele é excluído da coleção de movimentação, junto com qualquer coisa criada no destino, incluindo bancos de dados SQL. 
**Falha ao descartar** | É recomendável descartar as movimentações para que os recursos de destino sejam excluídos primeiro.<br/><br/> Depois disso, o recurso volta para o estado **Iniciar movimentação pendente** e você pode continuar a partir daí. 
**Excluir origem pendente** | Excluído da coleção de movimentação.<br/><br/> Ele não exclui nada criado na região de destino. 
**Falha ao excluir origem** | Excluído da coleção de movimentação.<br/><br/> Ele não exclui nada criado na região de destino. 

## <a name="next-steps"></a>Próximas etapas

Tente [mover uma VM](tutorial-move-region-virtual-machines.md) para outra região com o Resource mover.