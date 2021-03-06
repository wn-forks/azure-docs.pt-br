---
title: 'Início Rápido: Configurar e habilitar o Módulo de Segurança para o Azure RTOS'
description: Saiba como integrar e habilitar o Módulo de Segurança para o serviço do Azure RTOS em seu Hub IoT do Azure.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2020
ms.author: mlottner
ms.openlocfilehash: 116361977177b708265232a917e7f31e5f8cd09a
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89515104"
---
# <a name="quickstart-security-module-for-azure-rtos-preview"></a>Início Rápido: Módulo de Segurança para o Azure RTOS (versão prévia)

Este artigo fornece uma explicação dos pré-requisitos antes de começar e explica como habilitar o Módulo de Segurança para o serviço do Azure RTOS em um Hub IoT. Se você não tiver, no momento, um Hub IoT, confira [Criar um Hub IoT usando o portal do Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) para começar a usar.

> [!NOTE]
> O Módulo de Segurança para o Azure RTOS tem suporte apenas em Hubs IoT da camada Standard.

## <a name="prerequisites"></a>Pré-requisitos 

### <a name="supported-devices"></a>Dispositivos com suporte

- ST STM32F746G Discovery Kit
- NXP i.MX RT1060 EVK
- Microchip SAM E54 Xplained Pro EVK

Baixe, compile e execute um dos arquivos .zip referentes à placa e à ferramenta específicas (IAR, IDE do semi ou PC) de sua escolha no [recurso do Módulo de Segurança para o Azure RTOS no GitHub](hhtps://github.com/azure-rtos/azure-iot-preview/releases).

### <a name="azure-resources"></a>Recursos do Azure

A próxima fase para começar é preparar seus recursos do Azure. Você precisará de um Hub IoT, e sugerimos um workspace do Log Analytics. Para o Hub IoT, você precisará da cadeia de conexão do Hub IoT para se conectar ao seu dispositivo. 
  
### <a name="iot-hub-connection"></a>Cadeia Hub IoT

Uma conexão do Hub IoT é necessária para começar. 

1. Abra o **Hub IoT** no portal do Azure.
1. Copie a cadeia de conexão do IoT no [arquivo de configuração](how-to-azure-rtos-security-module.md).


As credenciais de conexão são obtidas da configuração do aplicativo do usuário, em **HOST_NAME**, **DEVICE_ID** e **DEVICE_SYMMETRIC_KEY**.

O Módulo de Segurança para os Azure RTOS usa conexões de middleware do Azure IoT com base no protocolo **MQTT**.


### <a name="log-analytics-workspace"></a>Workspace do Log Analytics

A ingestão do Log Analytics no Hub IoT fica desativada por padrão na solução da Central de Segurança do Azure para IoT. Para habilitá-la para trabalhar com o Módulo de Segurança para o Azure RTOS, faça o seguinte: 
1. No Portal do Azure, vá até o Hub IoT.
1. Selecione **Configurações** no menu **Segurança**.
   :::image type="content" source="media/quickstart/azure-rtos-hub-settings.png" alt-text="Acesso à opção de coleta de dados para o Azure RTOS"::: 
1. Selecione **Coleção de Dados**. 
1. Na opção **Configuração do workspace**, alterne a opção para **Ativado**. 
1. Crie um workspace do Log Analytics ou anexe um existente. Verifique se a opção **Acesso a dados de segurança brutos** está selecionada. 
 :::image type="content" source="media/quickstart/azure-rtos-data-collection-on.png" alt-text="Configuração do Azure RTOS mostrando as opções de coleta de dados e de dados de segurança brutos selecionadas":::
1. Selecione **Salvar**
1. Volte para a lista de recursos do Azure e confirme se você vê o workspace do Log Analytics que criou ou anexou habilitado para o Hub IoT.
    :::image type="content" source="media/quickstart/verify-azure-resource-list.png" alt-text="Verifique sua lista de recursos do Azure para confirmar a adição do workspace correto do Log Analytics a um Hub IoT"::: 

## <a name="next-steps"></a>Próximas etapas

Avance para o próximo artigo para concluir a configuração e a personalização da solução.

> [!div class="nextstepaction"]
> [Configurar o Módulo de Segurança para o Azure RTOS](how-to-azure-rtos-security-module.md)
