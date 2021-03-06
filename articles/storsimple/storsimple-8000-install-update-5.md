---
title: Instalar a Atualização 5 no dispositivo StorSimple da Série 8000 | Microsoft Docs
description: Explica como instalar a Atualização 5 do StorSimple série 8000 em seu dispositivo StorSimple série 8000.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/13/2017
ms.author: alkohli
ms.openlocfilehash: bbac6eade634ffcfdc47ae3d22b32e0bd429b7c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513186"
---
# <a name="install-update-5-on-your-storsimple-device"></a>Instalar a Atualização 5 em seu dispositivo StorSimple

## <a name="overview"></a>Visão geral

Este tutorial explica como instalar a Atualização 5 em um dispositivo StorSimple que executa uma versão de software anterior pelo Portal do Azure e usando o método de hotfix. O método de hotfix é usado quando você está tentando instalar a Atualização 5 em um dispositivo executando versões anteriores à Atualização 3. O método de hotfix é usado também quando um gateway é configurado em uma interface de rede que não seja DATA 0 do dispositivo StorSimple e quando você está tentando atualizar de uma versão de software anterior à Atualização 1.

A Atualização 5 inclui o software de dispositivo, Storport e Spaceport, as atualizações de segurança do SO e as atualizações do SO, bem como atualizações de firmware de disco.  O software do dispositivo, o Spaceport, o Storport, a segurança e outras atualizações do SO não são atualizações que causam interrupções. As atualizações não interruptivas ou regulares podem ser aplicadas pelo Portal do Azure ou por meio do método de hotfix. As atualizações de firmware de disco causam interrupção e são aplicadas quando o dispositivo está em modo de manutenção, por meio do método de hotfix, usando a interface do Windows PowerShell do dispositivo.

> [!IMPORTANT]
> * A Atualização 5 é uma atualização obrigatória e deve ser instalada imediatamente. Para obter mais informações, consulte [Notas de versão da Atualização 5](storsimple-update5-release-notes.md).
> * Um conjunto de verificações prévias manuais e automáticas para são realizadas antes da instalação para determinar a integridade do dispositivo em termos de conectividade de rede e estado do hardware. Essas pré-verificações serão executadas somente se você aplicar as atualizações no Portal do Azure.
> * É altamente recomendável que, ao atualizar um dispositivo executando versões anteriores à Atualização 3, você instala as atualizações usando o método de hotfix. Se você encontrar problemas, [abra um tíquete de suporte](storsimple-8000-contact-microsoft-support.md).
> * É recomendável instalar a atualização do software e outras atualizações regulares pelo Portal do Azure. Você só deve ir para a interface do Windows PowerShell do dispositivo (para instalar atualizações) se a verificação de pré-atualização de gateway falhar no portal. Dependendo da versão da qual você está atualizando, as atualizações podem levar 4 horas (ou mais) para serem instaladas. As atualizações do modo de manutenção devem ser instaladas por meio da interface do Windows PowerShell do dispositivo. Como as atualizações do modo de manutenção são atualizações que ocasionam interrupção, elas causam tempo de inatividade em seu dispositivo.
> * Se estiver executando o StorSimple Snapshot Manager opcional, verifique se você atualizou a versão do Snapshot Manager para a Atualização 5 antes de atualizar o dispositivo.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-the-azure-portal"></a>Instalar a Atualização 5 por meio do Portal do Azure
Realize as etapas a seguir para atualizar o dispositivo para a [Atualização 5](storsimple-update5-release-notes.md).

> [!NOTE]
> Microsoft efetua pull de informações adicionais de diagnóstico do dispositivo. Como resultado, quando nossa equipe de operações identifica dispositivos com problemas, estamos mais bem equipados para coletar informações do dispositivo e diagnosticar problemas.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Verifique se seu dispositivo está executando a **Atualização 5 do StorSimple Série 8000 (6.3.9600.17845)**. A **Data da última atualização** deve ser modificada.

Agora, você também verá que as atualizações do modo de Manutenção estão disponíveis (essa mensagem pode continuar sendo exibida por até 24 horas após a instalação das atualizações). As etapas para instalar a atualização do modo de manutenção são detalhadas na próxima seção.

[!INCLUDE [storsimple-8000-install-maintenance-mode-updates](../../includes/storsimple-8000-install-maintenance-mode-updates.md)]

## <a name="install-update-5-as-a-hotfix"></a>Instalar a Atualização 5 como um hotfix

As versões de software que podem ser atualizadas usando o método de hotfix são:

* Atualização 0.1, 0.2, 0.3
* Atualização 1, 1.1, 1.2
* Atualização 2, 2.1, 2.2
* Atualização 3, 3.1
* Atualização 4

> [!NOTE] 
> O método recomendado para instalar a Atualização 5 é por meio do portal do Azure ao tentar atualizar da Atualização 3 e versões posteriores. Ao atualizar um dispositivo executando versões anteriores à Atualização 3, use este procedimento. Você também pode usar este procedimento se a verificação de gateway falhar ao tentar instalar as atualizações por meio do portal do Azure. A verificação falha quando você tem um gateway atribuído a um adaptador de rede diferente de DATA 0 e o dispositivo está executando uma versão de software anterior à Atualização 1.

O método de hotfix envolve as três etapas a seguir:

1. Baixe os hotfixes do Catálogo do Microsoft Update.
2. Instale e verifique os hotfixes do modo normal.
3. Instale e verifique o hotfix do modo de manutenção.

#### <a name="download-updates-for-your-device"></a>Baixar atualizações para seu dispositivo

Você deve baixar e instalar os seguintes hotfixes na ordem descrita e as pastas sugeridas:

| Order | KB | Descrição | Tipo de atualização | Hora da instalação |Instalar na pasta|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Atualização de software<br> Faça o download do _HcsSoftwareUpdate.exe_ e do _CisMSDAgent.exe_ |Regular <br></br>Não interruptiva |Aproxim. 25 minutos |FirstOrderUpdate|

Se estiver atualizando de um dispositivo que executa a Atualização 4, você só precisará instalar as atualizações cumulativas do SO como atualizações de segunda ordem.

| Order | KB | Descrição | Tipo de atualização | Hora da instalação |Instalar na pasta|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |Pacote de atualizações cumulativas do SO <br> Baixar a versão Windows Server 2012 R2 |Regular <br></br>Não interruptiva |- |SecondOrderUpdate|

Se estiver instalando de um dispositivo que executa a Atualização 3 ou anterior, instale o seguinte, além das atualizações cumulativas.

| Order | KB | Descrição | Tipo de atualização | Hora da instalação |Instalar na pasta|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |Atualizações de firmware e driver LSI <br> Atualização de firmware USM (versão 3.38) |Regular <br></br>Não interruptiva |Aproxim. 3 horas <br> (inclui 2A. + 2B. + 2C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |Pacote de atualizações de segurança do SO <br> Baixar a versão Windows Server 2012 R2 |Regular <br></br>Não interruptiva |- |SecondOrderUpdate|
| 2D. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |Pacote de atualizações do SO <br> Baixar a versão Windows Server 2012 R2 |Regular <br></br>Não interruptiva |- |SecondOrderUpdate|


Você também precisa instalar as atualizações de firmware de disco além de todas as atualizações mostradas nas tabelas anteriores. Você pode verificar se precisa de atualizações de firmware de disco executando o cmdlet `Get-HcsFirmwareVersion` . Se estiver executando as versões de firmware `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, você não precisará instalar essas atualizações.

| Order | KB | Descrição | Tipo de atualização | Hora da instalação | Instalar na pasta|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Firmware de disco |Manutenção <br></br>Interruptiva |~ 30 Min. | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Se você estiver atualizando da Atualização 4, o tempo total de instalação será próximo de 4 horas.
> * Antes de usar este procedimento para aplicar a atualização, certifique-se de que ambos os controladores de dispositivo estão online e todos os componentes de hardware estão íntegros.

Execute as seguintes etapas para baixar e instalar os hotfixes.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-8000-install-troubleshooting](../../includes/storsimple-8000-install-troubleshooting.md)]

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre a [versão da Atualização 5](storsimple-update5-release-notes.md).

