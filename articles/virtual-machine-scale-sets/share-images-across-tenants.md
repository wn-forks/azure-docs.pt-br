---
title: Compartilhar imagens da Galeria entre locatários no Azure
description: Saiba como compartilhar imagens de VM entre locatários do Azure usando galerias de imagens compartilhadas.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: imaging
ms.date: 04/05/2019
ms.reviewer: akjosh
ms.custom: akjosh, devx-track-azurecli
ms.openlocfilehash: 542dfbc28621e02cd2842fb174e986bfeb91e6ec
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501671"
---
# <a name="share-gallery-vm-images-across-tenants-in-azure"></a>Compartilhar imagens de VM da Galeria entre locatários no Azure

[!INCLUDE [virtual-machines-share-images-across-tenants](../../includes/virtual-machines-share-images-across-tenants.md)]


## <a name="create-a-scale-set-using-azure-cli"></a>Criar um conjunto de dimensionamento usando a CLI do Azure

Conecte a entidade de serviço para o locatário 1 usando a appID, a chave do aplicativo e a ID do locatário 1. Você pode usar `az account show --query "tenantId"` para obter as IDs de locatário, se necessário.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
Conecte a entidade de serviço para o locatário 2 usando a appID, a chave do aplicativo e a ID do locatário 2:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

Criar o conjunto de dimensionamento. Substitua as informações no exemplo pelo seu próprio.

```azurecli-interactive
az vmss create \
  -g myResourceGroup \
  -n myScaleSet \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Próximas etapas

Se você tiver algum problema, você poderá [solucionar problemas de galerias de imagens compartilhadas](troubleshooting-shared-images.md).
