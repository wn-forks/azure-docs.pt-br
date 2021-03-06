---
title: Configurar um túnel de VPN AlwaysOn
titleSuffix: Azure VPN Gateway
description: Saiba como usar gateways com o Windows 10 Always On para estabelecer e configurar túneis de dispositivo persistentes para o Azure.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: ab951375fdf9c5317e2c3a2f67271666d0987142
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89435838"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Configurar um túnel de dispositivo VPN Always On

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="configure-the-gateway"></a>Configurar o gateway

Configure o gateway de VPN para usar a autenticação IKEv2 e baseada em certificado usando o artigo [Configurar uma conexão VPN ponto a site](vpn-gateway-howto-point-to-site-resource-manager-portal.md) .

## <a name="configure-the-device-tunnel"></a>Configurar o túnel do dispositivo

[!INCLUDE [device tunnel](../../includes/vpn-gateway-vwan-always-on-device.md)]

## <a name="to-remove-a-profile"></a>Para remover um perfil

Para remover o perfil, execute o seguinte comando:

![Limpeza](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Próximas etapas

Para solucionar problemas, consulte [problemas de conexão de ponto a site do Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
