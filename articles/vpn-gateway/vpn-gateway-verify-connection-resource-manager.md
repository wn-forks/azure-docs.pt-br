---
title: 'Gateway de VPN do Azure: verificar uma conexão de gateway'
description: Este artigo mostra como verificar a conexão de Gateway de VPN de uma rede virtual.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.custom: devx-track-azurecli
ms.openlocfilehash: 98b7923728616473186eeeeb2a2f03bfece9fae1
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89398521"
---
# <a name="verify-a-vpn-gateway-connection"></a>Verificar uma conexão de Gateway de VPN

Este artigo mostra como verificar uma conexão de gateway de VPN para os modelos de implantação do Resource Manager e clássico.

## <a name="azure-portal"></a>Portal do Azure

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

Para verificar uma conexão de gateway de VPN para o modelo de implantação do Resource Manager utilizando o PowerShell, instale a última versão dos [cmdlets do Azure Resource Manager PowerShell](/powershell/azure/).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>CLI do Azure

Para verificar uma conexão de gateway de VPN para o modelo de implantação do Resource Manager utilizando a CLI do Azure, instale a última versão dos [comandos da CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 ou posterior).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Portal do Azure (clássico)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (clássico)

Para verificar sua conexão de gateway de VPN para o modelo de implantação clássico utilizando o PowerShell, instale as últimas versões dos cmdlets do Azure PowerShell. Certifique-se de baixar e instalar o módulo [Gerenciamento de Serviços](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets). Utilize 'Add-AzureAccount' para entrar no modelo de implantação clássico.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Próximas etapas

* Você pode adicionar máquinas virtuais às suas redes virtuais. Veja [Criar uma máquina virtual](../virtual-machines/windows/quick-create-portal.md) para obter as etapas.
