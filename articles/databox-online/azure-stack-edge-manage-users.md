---
title: Azure Stack borda gerenciar usuários | Microsoft Docs
description: Descreve como usar o portal do Azure para gerenciar usuários em seu Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/11/2019
ms.author: alkohli
ms.openlocfilehash: 1012ce417fb4952878dada29a9080c70954ed21f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84339630"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-azure-stack-edge"></a>Usar o portal do Azure para gerenciar usuários no seu Azure Azure Stack Edge

Este artigo descreve como gerenciar usuários em seu Azure Stack Edge. Você pode gerenciar o Azure Stack Edge por meio da portal do Azure ou por meio da interface do usuário da Web local. Use o portal do Azure para adicionar, modificar ou excluir usuários.

Neste artigo, você aprenderá como:

> [!div class="checklist"]
> * Adicionar um usuário
> * Modificar usuário
> * Excluir um usuário

## <a name="about-users"></a>Sobre os usuários

Os usuários podem ter privilégio completo ou somente leitura. Como o nome indica, usuários somente leitura podem apenas exibir os dados do compartilhamento. Usuários com privilégio completo podem ler dados de compartilhamento, gravar nesses compartilhamentos e modificar ou excluir os dados de compartilhamento.

 - **Usuário com privilégios completo** – um usuário local com acesso completo.
 - **Usuário somente leitura** – um usuário local com acesso somente leitura. Esses usuários estão associados a compartilhamentos que permitem operações somente leitura.

As permissões do usuário são definidas quando o usuário é criado durante a criação do compartilhamento. No momento, não há suporte para a modificação de permissões de nível de compartilhamento.

## <a name="add-a-user"></a>Adicionar um usuário

Para adicionar um usuário, siga estas etapas no portal do Azure.

1. Na portal do Azure, vá para o recurso de borda do Azure Stack e, em seguida, vá para **visão geral > usuários**. Selecione **+ Adicionar usuário** na barra de comandos.

    ![Selecionar Adicionar usuário](media/azure-stack-edge-manage-users/add-user-1.png)

2. Especifique o nome de usuário e senha do usuário que você deseja adicionar. Confirme a senha e selecione **Adicionar**.

    ![Especificar nome de usuário e senha](media/azure-stack-edge-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Estes usuários são reservados pelo sistema e não devem ser usados: Administrador, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Convidado.  

3. Uma notificação é mostrada quando a criação de usuário é iniciada e concluída. Após a criação do usuário, na barra de comandos, selecione **Atualizar** para exibir a lista atualizada de usuários.


## <a name="modify-user"></a>Modificar usuário

Você pode alterar a senha associada a um usuário após ele ser criado. Selecione na lista de usuários. Insira e confirme a nova senha. Salve as alterações.
 
![Modificar usuário](media/azure-stack-edge-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Excluir um usuário

Para excluir um usuário, siga estas etapas no portal do Azure.


1. Na portal do Azure, vá para o recurso de borda do Azure Stack e, em seguida, vá para **visão geral > usuários**.

    ![Selecionar o usuário a ser excluído](media/azure-stack-edge-manage-users/delete-user-1.png)

2. Selecione um usuário na lista de usuários e, em seguida, selecione **Excluir**.  

   ![Selecione Excluir](media/azure-stack-edge-manage-users/delete-user-2.png)

3. Quando receber a solicitação, confirme a exclusão. 

   ![Confirmar exclusão](media/azure-stack-edge-manage-users/delete-user-3.png)

A lista de usuários é atualizada para refletir o usuário excluído.

![Lista atualizada de usuários](media/azure-stack-edge-manage-users/delete-user-4.png)


## <a name="next-steps"></a>Próximas etapas

- Saiba como [Gerenciar a largura de banda](azure-stack-edge-manage-bandwidth-schedules.md).
