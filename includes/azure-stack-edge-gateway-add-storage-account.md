---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/30/2020
ms.author: alkohli
ms.openlocfilehash: 30ca4d330d9b16214396ac81e5ab5722ca0e7569
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89254280"
---
1. No [portal do Azure](https://portal.azure.com/), selecione o recurso do Azure Stack Edge e, em seguida, acesse **Visão Geral**. O dispositivo deve estar online.

2. Selecione **+ Adicionar conta de armazenamento** na barra de comandos do dispositivo. 

   ![Adicionar uma conta de armazenamento](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-1.png)

3. No painel **Adicionar conta de armazenamento do Edge**, especifique as seguintes configurações:

    a. Um nome exclusivo para a conta de armazenamento do Edge em seu dispositivo. Os nomes de conta de armazenamento só podem conter números e letras minúsculas. Caracteres especiais não são permitidos. O nome da conta de armazenamento deve ser exclusivo dentro do dispositivo (não entre os dispositivos).

    b. Uma descrição opcional para as informações sobre os dados que a conta de armazenamento está mantendo.  
    
    c. Por padrão, a conta de armazenamento do Edge é mapeada para uma conta de Armazenamento do Azure na nuvem e os dados da conta de armazenamento são enviados automaticamente por push para a nuvem. Especifique a conta de armazenamento do Azure para a qual sua conta de armazenamento do Edge está mapeada.  

    d. Em seguida, crie um contêiner ou selecione um contêiner existente na conta de armazenamento do Azure. Todos os dados do dispositivo gravados na conta de armazenamento do Edge são automaticamente carregados para o contêiner de armazenamento selecionado na conta do Armazenamento do Azure mapeada.

    <!--![Add a storage account](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-2.png)-->

    e. Depois que todas as opções de conta de armazenamento forem especificadas, selecione **Adicionar** para criar a conta de armazenamento do Edge. Você será notificado quando a conta de armazenamento do Edge tiver sido criada com êxito. A nova conta de armazenamento do Edge é exibida na lista de contas de armazenamento no portal do Azure. 

    
4. Se você selecionar essa nova conta de armazenamento e acessar **Chaves de acesso**, poderá encontrar o ponto de extremidade de serviço de blob e o nome da conta de armazenamento correspondente. Copie essas informações, pois esses valores junto com as chaves de acesso ajudarão você a se conectar à conta de armazenamento do Edge.

    ![Adicionar uma conta de armazenamento](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-4.png)

    Você obtém as chaves de acesso [Conectar-se às APIs locais do dispositivo usando o Azure Resource Manager](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md). 
