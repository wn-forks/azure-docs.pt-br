---
title: 'Início Rápido: Criar alertas personalizados'
description: Entenda, crie e atribua alertas de dispositivo personalizados para o serviço de segurança Central de Segurança do Azure para IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2020
ms.author: mlottner
ms.openlocfilehash: 7638ad070e8ac8bd99cbfb49b99bbb347a243a21
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2020
ms.locfileid: "89505430"
---
# <a name="quickstart-create-custom-alerts"></a>Início Rápido: Criar alertas personalizados

Usando alertas e grupos de segurança personalizados, aproveite ao máximo as informações de segurança de ponta a ponta e o conhecimento categórico do dispositivo para garantir a melhor segurança em solução de IoT.

## <a name="why-use-custom-alerts"></a>Por que usar alertas personalizados?

Você conhece melhor seus dispositivos de IoT.

Para clientes que compreendem totalmente o comportamento esperado do seu dispositivo, a Central de Segurança do Azure para IoT permite traduzir esse reconhecimento em uma política de comportamento do dispositivo e alertar sobre qualquer desvio do comportamento normal e esperado.

## <a name="security-groups"></a>Grupos de segurança

Grupos de segurança permitem que você defina grupos lógicos de dispositivos e gerencie seu estado de segurança de forma centralizada.

Esses grupos podem representar dispositivos com o hardware específico, implantados em um determinado local, ou qualquer outro grupo adequado às suas necessidades específicas de dispositivos.

Grupos de segurança são definidos por uma propriedade de marca do dispositivo gêmeo denominada **SecurityGroup**. Por padrão, cada solução de IoT no Hub IoT tem um grupo de segurança chamado **padrão**. Altere o valor da propriedade **SecurityGroup** para alterar o grupo de segurança de um dispositivo.

Por exemplo:

```
{
  "deviceId": "VM-Contoso12",
  "etag": "AAAAAAAAAAM=",
  "deviceEtag": "ODA1BzA5QjM2",
  "status": "enabled",
  "statusUpdateTime": "0001-01-01T00:00:00",
  "connectionState": "Disconnected",
  "lastActivityTime": "0001-01-01T00:00:00",
  "cloudToDeviceMessageCount": 0,
  "authenticationType": "sas",
  "x509Thumbprint": {
    "primaryThumbprint": null,
    "secondaryThumbprint": null
  },
  "version": 4,
  "tags": {
    "SecurityGroup": "default"
  },
```

Use grupos de segurança para agrupar seus dispositivos em categorias lógicas. Depois de criar os grupos, atribua-os para os alertas personalizados de sua escolha, para a solução de segurança da IoT de ponta a ponta mais eficaz.

## <a name="customize-an-alert"></a>Personalizar um alerta

1. Abra o Hub IoT e selecione **Configurações** no menu **Segurança**. 
1. Clique em **Alertas personalizados**.
1. Escolha um grupo de segurança que você deseja aplicar a personalização.
1. Clique em **Adicionar um alerta personalizado**.
1. Selecione um alerta personalizado na lista suspensa.
1. Edite as propriedades necessárias, clique em **OK**.
1. Certifique-se de clicar em **Salvar**. Sem salvar o novo alerta, o alerta é excluído na próxima vez que você fechar o Hub IoT.

## <a name="alerts-available-for-customization"></a>Alertas disponíveis para personalização

A Central de Segurança do Azure para IoT oferece um grande número de alertas que podem ser personalizados de acordo com suas necessidades específicas. Examine a [tabela de alertas personalizáveis](concept-customizable-security-alerts.md) para obter a severidade do alerta, a fonte de dados, a descrição e as nossas etapas de correção sugeridas se e quando cada alerta for recebido.

## <a name="next-steps"></a>Próximas etapas

Avance para o próximo artigo para saber como implantar um agente de segurança...

> [!div class="nextstepaction"]
> [Implantar um agente de segurança](how-to-deploy-agent.md)
