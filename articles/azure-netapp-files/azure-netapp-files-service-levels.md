---
title: Níveis de serviço no Azure NetApp Files | Microsoft Docs
description: Descreve o desempenho de taxa de transferência para os níveis de serviço do Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: b-juche
ms.openlocfilehash: 639f1e09fdb5603965209e5b5ee6c224ad238b76
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533114"
---
# <a name="service-levels-for-azure-netapp-files"></a>Níveis de serviço do Azure NetApp Files
Os níveis de serviço são um atributo de um pool de capacidade. Os níveis de serviço são definidos e diferenciados pela taxa de transferência máxima permitida para um volume no pool de capacidade com base na cota atribuída ao volume.

## <a name="supported-service-levels"></a>Níveis de serviço com suporte

O Azure NetApp Files dá suporte a três níveis de serviço: *ultra*, *Premium*e *Standard*. 

* <a name="Ultra"></a>Ultra armazenamento

    A camada de armazenamento ultra fornece até 128 MiB/s de taxa de transferência por 1 TiB de cota de volume atribuída. 

* <a name="Premium"></a>Armazenamento Premium

    A camada de armazenamento Premium fornece até 64 MiB/s de taxa de transferência por 1 TiB de cota de volume atribuída. 

* <a name="Standard"></a>Armazenamento Standard

    A camada de armazenamento Standard fornece até 16 MiB/s de taxa de transferência por 1 TiB de cota de volume atribuída.

## <a name="throughput-limits"></a>Limites de taxa de transferência

O limite de taxa de transferência para um volume é determinado pela combinação dos seguintes fatores:
* O nível de serviço do pool de capacidade ao qual o volume pertence
* A cota atribuída ao volume  

Esse conceito é ilustrado no diagrama a seguir:

![Ilustração de nível de serviço](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

No exemplo 1 acima, um volume de um pool de capacidade com a camada de armazenamento Premium atribuído a 2 TiB de cota será atribuído a um limite de taxa de transferência de 128 MiB/s (2 TiB * 64 MiB/s). Esse cenário se aplica independentemente do tamanho do pool de capacidade ou do consumo real do volume.

No exemplo 2 acima, um volume de um pool de capacidade com a camada de armazenamento Premium atribuído a 100 GiB de cota será atribuído a um limite de taxa de transferência de 6,25 MiB/s (0, 9765625 TiB * 64 MiB/s). Esse cenário se aplica independentemente do tamanho do pool de capacidade ou do consumo real do volume.

## <a name="next-steps"></a>Próximas etapas

- [Página de preços do Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/)
- [Modelo de custo para o Azure NetApp Files](azure-netapp-files-cost-model.md) 
- [Configurar um pool de capacidade](azure-netapp-files-set-up-capacity-pool.md)
- [Contrato de Nível de Serviço (SLA) para Azure NetApp Files](https://azure.microsoft.com/support/legal/sla/netapp/)
- [Alterar dinamicamente o nível de serviço de um volume](dynamic-change-volume-service-level.md) 