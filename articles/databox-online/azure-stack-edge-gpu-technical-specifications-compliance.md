---
title: Microsoft Azure Stack Edge com especificações técnicas de GPU e conformidade | Microsoft Docs
description: Saiba mais sobre as especificações técnicas e a conformidade para seu dispositivo Azure Stack Edge com GPU
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 08/26/2020
ms.author: alkohli
ms.openlocfilehash: 3f354655a612d4085b0a0de45ae1a6e5ee097ade
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89266656"
---
# <a name="technical-specifications-and-compliance-for-azure-stack-edge-with-gpu"></a>Especificações técnicas e conformidade para Azure Stack Edge com GPU 

Os componentes de hardware do seu Azure Stack Edge com uma GPU (unidade de processamento gráfico) integrada aderem às especificações técnicas e aos padrões regulatórios descritos neste artigo. As especificações técnicas descrevem hardware, PSUs (unidades de fonte de alimentação), capacidade de armazenamento, compartimentos e padrões ambientais.

## <a name="compute-and-memory-specifications"></a>Especificações de memória e computação

O dispositivo Microsoft Azure Stack Edge tem as seguintes especificações para computação e memória:

| Especificação           | Valor                  |
|-------------------------|----------------------------|
| CPU                     | 2 X CPU Intel Xeon Silver 4214 (Cascadey Lake)            |
| Memória                  | 128 (8x16 GB) GB de RAM                     |


## <a name="compute-acceleration-specifications"></a>Especificações de aceleração de computação

Uma GPU (unidade de processamento gráfico) está incluída em cada dispositivo de borda Azure Stack que permite cenários de kubernetes, aprendizado profundo e aprendizado de máquina.

| Especificação           | Valor                  |
|-------------------------|----------------------------|
| GPU   | Uma ou duas GPUs nVidia T4 <br> Para obter mais informações, consulte [NVIDIA T4](https://www.nvidia.com/en-us/data-center/tesla-t4/).| 


## <a name="power-supply-unit-specifications"></a>Especificações da unidade de fonte de alimentação

O dispositivo de borda Azure Stack tem duas PSUs (unidades de fonte de alimentação) de 100-240 V com ventiladores de alto desempenho. Essas duas PSUs oferecem uma configuração de alimentação redundante. Se uma PSU falhar, o dispositivo continuará a funcionar normalmente em outra PSU até que o módulo com falha seja substituído. A seguinte tabela lista as especificações técnicas associadas das PSUs.

| Especificação           | 750 W PSU                  |
|-------------------------|----------------------------|
| Alimentação de saída máxima    | 750 W                     |
| Frequência               | 50/60 Hz                   |
| Seleção de faixa de tensão | Variação automática: 100-240 V AC |
| Conectado com a máquina ligada           | Sim                        |


## <a name="network-interface-specifications"></a>Especificações da interface de rede

Seu dispositivo de borda Azure Stack tem seis interfaces de rede, PORT1-PORT6.

| Especificação           | Descrição                 |
|-------------------------|----------------------------|
|  Interfaces de rede    | **interfaces 2 X 1 GbE** – 1 a porta 1 da interface de gerenciamento é usada para configuração inicial e é estática por padrão. Depois que a configuração inicial for concluída, você poderá usar a interface para dados com qualquer endereço IP. No entanto, ao redefinir, a interface reverte de volta para o IP estático. <br>A outra porta 2 da interface é configurável pelo usuário, pode ser usada para transferência de dados e é DHCP por padrão. <br>**4 X 25 interfaces GbE** – essas interfaces de dados, a porta 3 até a porta 6, podem ser configuradas pelo usuário como DHCP (padrão) ou estática. Elas também podem operar como interfaces de 10 GbE.  | 

O dispositivo do Azure Stack Edge tem o seguinte hardware de rede:

* **Adaptador de NDC do Microsoft QLogic Cavium 25g personalizado** -porta 1 até a porta 4.
* **Adaptador de rede do canal dual port 25g ConnectX-4** -porta 5 e porta 6 da Mellanox.

Estes são os detalhes da placa Mellanox:

| Parâmetro           | Descrição                 |
|-------------------------|----------------------------|
| Modelo    | Placa de interface de rede ConnectX®-4 LX                      |
| Descrição do modelo               | 25GbE Dual-Port SFP28; PCIe 3.0 x8; R6 DE ROHS                    |
| Número de peça do dispositivo (R640) | MCX4121A-ACAT  |
| PSID (R640)           | MT_2420110034                         |

Para obter uma lista completa de cabos, comutadores e transceptores com suporte para essas placas de rede, acesse:

- [Matriz de interoperabilidade do adaptador do QLogic Cavium 25g NDC](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
- [Produtos compatíveis com adaptador de rede de canal dual port 25g ConnectX-4](https://docs.mellanox.com/display/ConnectX4LxFirmwarev14271016/Firmware+Compatible+Products).  

## <a name="storage-specifications"></a>Especificações do armazenamento

Os dispositivos de borda Azure Stack têm cinco 2,5 de SSDs de P4610 de controlador de domínio NVMe, cada um com uma capacidade de 1,6 TB. A unidade de inicialização é uma SSD SATA de 240 GB. A capacidade utilizável total para o dispositivo é de aproximadamente 8,28 TB. A tabela a seguir lista a capacidade de armazenamento do dispositivo.

|     Especificação                          |     Valor             |
|--------------------------------------------|-----------------------|
|    SSD (unidades de estado sólido) SATA de inicialização      |    1                  |
|    Número de SSDs do NVMe                     |    5                  |
|    Capacidade SSD de inicialização                       |    240 GB             |
|    Capacidade SSD única do NVMe                |    1,6 TB             |
|    Capacidade total                          |    8,28 TB            |
|    Capacidade total utilizável*                  |    ~ 7,95 TB          |
|    Controlador SAS                          |    HBA330 12 Gbps     |


**Algum espaço está reservado para uso interno.*

<!--Remove based on feedback from Ravi
## Other hardware specifications

Your Azure Stack Edge device also contains the following hardware:

* iDRAC baseboard management
* Performance fans
* Custom ID module-->

## <a name="enclosure-dimensions-and-weight-specifications"></a>Dimensões do compartimento e especificações de peso

As tabelas a seguir listam as diversas especificações de compartimento para dimensões e peso.

### <a name="enclosure-dimensions"></a>Dimensões do compartimento

A tabela a seguir lista as dimensões do compartimento de dispositivo 1U em milímetros e polegadas.

|     Compartimento     |     Milímetros     |     Polegadas     |
|-------------------|---------------------|----------------|
|    Altura         |    44,45            |    1,75"       |
|    Largura          |    434,1            |    17,09"      |
|    Comprimento         |    740,4            |    29,15"      |

A tabela a seguir lista as dimensões do pacote de remessa em milímetros e em polegadas.

|     Pacote     |     Milímetros     |     Polegadas     |
|-------------------|---------------------|----------------|
|    Altura         |    311,2            |    12,25          |
|    Largura          |    642,8            |    25,31          |
|    Comprimento         |    1\.051,1          |    41,38          |

### <a name="enclosure-weight"></a>Peso do compartimento

O pacote do dispositivo pesa 66 kg. e requer duas pessoas para manipulá-lo. O peso do dispositivo depende da configuração do compartimento.

|     Compartimento                                 |     Peso          |
|-----------------------------------------------|---------------------|
|    Peso total, incluindo a embalagem       |    61 lbs.          |
|    Peso do dispositivo                       |    35 lbs.          |

## <a name="enclosure-environment-specifications"></a>Especificações do ambiente de compartimento

Esta seção lista as especificações relacionadas ao ambiente de compartimento, como temperatura, umidade e altitude.

### <a name="temperature-and-humidity"></a>Temperatura e umidade

|     Compartimento         |     Faixa de temperatura ambiente     |     Umidade relativa do ambiente     |     Ponto de condensação máximo     |
|-----------------------|--------------------------------------|--------------------------------------|---------------------------|
|    Operacional        |    10°C - 35°C (50°F - 86°F)         |    10% – 80% sem condensação         |    29° C (84° F)            |
|    Não operacional    |    -40°C a 65°C (-40°F - 149°F)     |    5% – 95% sem condensação.          |    33 °C (91 °F)            |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Fluxo de ar, altitude, choque, vibração, orientação, segurança e EMC

|     Compartimento                           |     Especificações operacionais                                                                                                                                                                                         |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Fluxo de ar                              |    O ar do sistema flui da frente para a traseira. O sistema deve ser operado com uma instalação de exaustão traseira de baixa pressão. <!--Back pressure created by rack doors and obstacles should not exceed 5 pascals (0.5 mm water gauge).-->    |
|    Altitude operacional máxima        |    3048 metros (10.000 pés) com a temperatura de operação máxima desclassificada determinada pelo [especificações de temperatura operacional](#operating-temperature-de-rating-specifications).                                                                                |
|    Altitude máxima não operacional    |    12.000 metros (39.370 pés)                                                                                                                                                                                         |
|    Choque, operacional                   |    6 G por 11 milissegundos em 6 orientações                                                                                                                                                                         |
|    Choque, não operacional               |    71 G por 2 milissegundos em 6 orientações                                                                                                                                                                           |
|    Vibração, operacional               |    0,26 G<sub>RMS</sub> 5 Hz a 350 Hz aleatório                                                                                                                                                                                     |
|    Vibração, não operacional           |    1,88 G<sub>RMS</sub> 10 Hz a 500 Hz por 15 minutos (todos os seis lados testados)                                                                                                                                                  |
|    Orientação e montagem             |    Padrão 19 "montagem em rack (1U)                                                                                                                                                                                       |
|    Segurança e aprovações                 |    EN 60950-1:2006 +A1:2010 +A2:2013 +A11:2009 +A12:2011/IEC 60950-1:2005 ed2 +A1:2009 +A2:2013 EN 62311:2008                                                                                                                                                                       |
|    EMC                                  |    FCC A, ICES-003 <br>EN 55032:2012/CISPR 32:2012  <br>EN 55032:2015/CISPR 32:2015  <br>EN 55024:2010 + A1:2015/CISPR 24:2010 + A1:2015  <br>EN 61000-3-2:2014/IEC 61000-3-2:2014 (classe D)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013                                                                                                                                                                                         |
|    Energia             |    Regulamento da Comissão (UE) n.º 617/2013                                                                                                                                                                                        |
|    RoHS           |    EN 50581:2012                                                                                                                                                                                        |

### <a name="operating-temperature-de-rating-specifications"></a>Especificações de desclassificação da temperatura operacional

|     Desclassificação da temperatura operacional     |     Faixa de temperatura ambiente                                                         |
|--------------------------------------------|------------------------------------------------------------------------------------------|
|    Até 35°C (95°F)                       |    A temperatura máxima deve ser reduzida em 1 °C a cada 300 m (1 °F/547 pés) acima de 950 m (3.117 pés).    |
|    35°C a 40°C (95°F a 104°F)            |    A temperatura máxima deve ser reduzida em 1 °C a cada  1°C/175 m (1°F/319 pés) acima de 950 m (3.117 pés).    |
|    40°C a 45°C (104°F a 113°F)           |    A temperatura máxima deve ser reduzida em 1 °C/125 m a cada 125 m (1 °F/228 pés) acima de 950 m (3.117 pés).    |

## <a name="next-steps"></a>Próximas etapas

[Implantar seu Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md)
