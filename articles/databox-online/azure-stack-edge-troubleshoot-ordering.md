---
title: Usar o portal do Azure para solucionar problemas de Azure Stack ordenação de borda | Microsoft Docs
description: Descreve como solucionar problemas de Azure Stack ordenação de borda.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 08/11/2020
ms.author: alkohli
ms.openlocfilehash: 8c42ee867bdd20de64fa1c603de47b5f1e7bdb39
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90055938"
---
# <a name="troubleshoot-your-azure-stack-edge-ordering-issues"></a>Solucionar problemas de ordenação do Azure Stack Edge

Este artigo descreve como solucionar problemas de Azure Stack ordenação de borda.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
>
> * Solucionar problemas de ordenação

## <a name="unsupported-subscription-or-region"></a>Assinatura ou região sem suporte

**Descrição do erro:** Em portal do Azure, se você receber o erro:

*Não há suporte para a assinatura ou região selecionada. Escolha uma assinatura ou região diferente.*

![Assinatura ou região sem suporte](media/azure-stack-edge-troubleshoot-ordering/azure-stack-edge-troubleshoot-ordering-01.png)

**Solução sugerida:**  Verifique se você usou uma assinatura com suporte como [Microsoft Enterprise Agreement (ea)](https://azure.microsoft.com/overview/sales-number/), [provedor de soluções de nuvem (CSP)](https://docs.microsoft.com/partner-center/azure-plan-lp)ou [Microsoft Azure sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/). Não há suporte para assinaturas pagas conforme o uso. Para obter mais informações, consulte [Azure Stack pré-requisitos de recursos do Edge](azure-stack-edge-deploy-prep.md#prerequisites).

Há a possibilidade de que a Microsoft possa permitir uma atualização de tipo de assinatura caso a caso. Entre em contato com o [suporte da Microsoft](https://azure.microsoft.com/support/options/) para que eles possam entender suas necessidades e ajustar esses limites adequadamente.

## <a name="selected-subscription-type-not-supported"></a>Tipo de assinatura selecionado sem suporte

**Erro:** Você tem uma assinatura EA, CSP ou patrocinada e Obtém o seguinte erro:

*Não há suporte para o tipo de assinatura selecionado. Certifique-se de usar uma assinatura com suporte. [Saiba mais](azure-stack-edge-deploy-prep.md#prerequisites). Se estiver usando um tipo de assinatura com suporte, verifique se o `Microsoft.DataBoxEdge` provedor está registrado. Para obter informações sobre como registrar-se, consulte [registrar provedor de recursos](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers)*.

**Solução sugerida:** Siga estas etapas para registrar seu provedor de recursos do Azure Stack Edge:

1. Em portal do Azure, acesse **Home**as  >  **assinaturas**iniciais.

2. Selecione a assinatura que você usará para ordenar seu dispositivo.

3. Selecione **provedores de recursos** e, em seguida, procure **Microsoft. DataBoxEdge**.

    ![Registrar provedor de recursos](media/azure-stack-edge-troubleshoot-ordering/azure-stack-edge-troubleshoot-ordering-02.png)

Se você não tiver acesso de proprietário ou colaborador para registrar o provedor de recursos, você verá o seguinte erro: *o nome da assinatura de assinatura &lt; &gt; não tem permissões para registrar o (s) provedor (es) de recursos: Microsoft. DataBoxEdge.*

Para obter mais informações, consulte [registrar provedores de recursos](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers).

## <a name="microsoftdataboxedge-not-registered-for-subscription"></a>Microsoft. DataBoxEdge não registrado para assinatura

**Erro:** No portal do Azure, selecione uma assinatura a ser usada para Azure Stack borda ou Gateway do Data Box e obtenha o seguinte erro:

*Provedor (es) de recursos: Microsoft. DataBoxEdge não está registrado para &lt; o nome da assinatura da assinatura &gt; e você não tem permissões para registrar um provedor de recursos para o &lt; nome &gt; da assinatura da assinatura*.

**Solução sugerida:** Eleve seu acesso à assinatura ou encontre alguém com acesso de proprietário ou colaborador para registrar o provedor de recursos.

## <a name="resource-disallowed-by-policy"></a>Recurso não permitido pela política

**Erro:** No portal do Azure, você tenta registrar um provedor de recursos e obter o seguinte erro:

*O &lt; nome do recurso de recurso não &gt; foi permitido pela política. (Código: RequestDisallowedByPolicy). Iniciativa: negar tipos de recursos geralmente indesejados. Política: tipos de recurso não permitidos.*

**Solução sugerida:** Esse erro ocorre devido a uma política existente do Azure que bloqueia a criação de recursos. As políticas do Azure são definidas pelo administrador do sistema de uma organização para garantir a conformidade ao usar ou criar recursos do Azure. Se alguma dessas políticas estiver bloqueando Azure Stack criação de recursos do Edge, contate o administrador do sistema para editar sua política do Azure.

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre como [solucionar problemas de Azure Stack Edge](azure-stack-edge-troubleshoot.md).
