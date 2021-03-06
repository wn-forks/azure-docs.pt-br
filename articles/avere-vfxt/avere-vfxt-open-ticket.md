---
title: Como obter suporte para o Avere vFXT para Azure
description: Saiba como resolver problemas que podem surgir durante a implantação ou o uso do avere vFXT para o Azure criando um tíquete de suporte por meio do portal do Azure.
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 522d29505d7d10f5f6d97136f270f07a63053d19
ms.sourcegitcommit: 2bab7c1cd1792ec389a488c6190e4d90f8ca503b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88271100"
---
# <a name="get-help-with-your-system"></a>Obter ajuda com o sistema

Para obter ajuda com seu avere vFXT para o sistema do Azure, aqui estão as maneiras de obter suporte:

* **Problema com o Avere vFXT** – use o portal do Azure para abrir um tíquete de suporte para seu Avere vFXT conforme descrito [abaixo](#open-a-support-ticket-for-your-avere-vfxt).
* **Cota** – se tiver um problema relacionado à cota, [solicite um aumento de cota](#request-a-quota-increase)
* **Documentação e exemplos** – se tiver problemas com esta documentação ou exemplos, role até a parte inferior da página com problema e use a seção de **Comentários** para pesquisar entre os problemas existentes ou registrar um novo, se necessário.

## <a name="open-a-support-ticket-for-your-avere-vfxt"></a>Abrir um tíquete de suporte para o Avere vFXT

Se tiver problemas para implantar ou usar o Avere vFXT, solicite ajuda por meio do portal do Azure.

Siga estas etapas para garantir que o tíquete de suporte seja marcado com um recurso de seu cluster. Marcar o tíquete nos ajuda a encaminhá-lo para o recurso de suporte correto.

1. Em [https://portal.azure.com](https://portal.azure.com) , selecione **grupos de recursos**. Navegue até o grupo de recursos que contém o cluster vFXT em que o problema ocorreu e clique em uma das máquinas virtuais de cluster avere.

    ![captura de tela do painel "visão geral" do grupo de recursos do portal do Azure com uma VM específica circulada](media/avere-vfxt-ticket-vm.png)

1. Na página da VM, role para baixo até a parte inferior do painel esquerdo e clique em **Nova solicitação de suporte**.

    ![Captura de tela da página da VM do portal do Azure com a VM da captura de tela anterior. O menu à esquerda é rolado para a parte inferior e ' nova solicitação de suporte ' é circulado.](media/avere-vfxt-ticket-request.png)

1. Na primeira página da solicitação de suporte, escolha o tipo de problema e verifique se a assinatura correta está selecionada.

   Em **serviço**, clique em **todos os serviços** e procure em **armazenamento** para escolher **avere vFXT**.

   Adicione um breve resumo e selecione o tipo de problema.

    ![captura de tela de uma nova exibição de solicitação de suporte no portal do Azure. A guia noções básicas está selecionada. Os itens da tela incluem tipo de problema, assinatura, serviço, resumo e tipo de problema.](media/ticket-basics.png)

   Clique em **Avançar** para continuar.

1. A segunda página do formulário de suporte contém sugestões para corrigir o problema com base na descrição resumida. Clique no botão **Avançar** na parte inferior se você ainda precisar criar um tíquete de suporte.

   ![instantâneo da tela nova solicitação de suporte com a guia soluções selecionada. Um campo de texto no meio tem o título "solução recomendada" e explica as possíveis soluções.](media/ticket-solutions.png)

1. Na terceira página, forneça detalhes-isso inclui informações sobre o cluster, a hora em que o problema ocorreu, a gravidade e como contatá-lo. Preencha as informações e clique no botão **Avançar** na parte inferior.

   ![instantâneo da tela nova solicitação de suporte com a guia detalhes selecionada. Os campos de informações são organizados nas categorias ' detalhes do problema ', ' método de suporte ' e "informações de contato '.](media/ticket-details.png)

1. Examine as informações na página final e clique em **criar**. Uma confirmação e um número de tíquete serão enviados para seu endereço de email e um membro da equipe de suporte entrará em contato com você.

## <a name="request-a-quota-increase"></a>Solicitar um aumento de cota

Leia [Cota para o cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) para saber quais componentes são necessários para implantar o Avere vFXT para Azure. Você pode [solicitar um aumento de cota](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) no portal do Azure.
