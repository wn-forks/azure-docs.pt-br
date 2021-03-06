---
title: Configurar uma conexão com uma fonte de dados usando uma identidade gerenciada (versão prévia)
titleSuffix: Azure Cognitive Search
description: Aprenda a configurar uma conexão do indexador com uma fonte de dados usando uma identidade gerenciada (versão prévia)
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: d303de23a04d183d0ca280c3b3591299d883adf7
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88936581"
---
# <a name="set-up-an-indexer-connection-to-a-data-source-using-a-managed-identity-preview"></a>Configurar uma conexão do indexador com uma fonte de dados usando uma identidade gerenciada (versão prévia)

> [!IMPORTANT] 
> O suporte para configurar uma conexão com uma fonte de dados usando uma identidade gerenciada está atualmente em visualização pública. A funcionalidade de versão prévia é fornecida sem um Contrato de Nível de Serviço e, portanto, não é recomendada para cargas de trabalho de produção.

No Azure Cognitive Search, o [indexador](search-indexer-overview.md) é um rastreador que fornece uma maneira de efetuar pull de dados de sua fonte de dados para o Azure Cognitive Search. Um indexador obtém uma conexão de fonte de dados por meio do objeto da fonte de dados criado por você. O objeto da fonte de dados geralmente inclui as credenciais da fonte de dados de destino. Por exemplo, o objeto da fonte de dados poderá incluir uma chave de conta de armazenamento do Azure se você quiser indexar dados de um contêiner de armazenamento de blobs.

Na maioria dos casos, fornecer credenciais diretamente no objeto da fonte de dados não causa problemas, mas pode apresentar alguns desafios:
* Como faço para proteger as credenciais no meu código que cria o objeto da fonte de dados?
* Se minha chave ou senha da conta estiver comprometida, e eu precisar alterá-las, precisarei atualizar meus objetos da fonte de dados com a nova chave ou senha da conta para que meu indexador possa se conectar novamente à fonte de dados.

Essa situação pode ser resolvida configurando uma conexão com uma identidade gerenciada.

## <a name="using-managed-identities"></a>Usar identidades gerenciadas

O recurso de [identidades gerenciadas](../active-directory/managed-identities-azure-resources/overview.md) fornece serviços do Azure com uma identidade gerenciada automaticamente no Azure AD (Azure Active Directory). Você pode usar esse recurso no Azure Cognitive Search para criar um objeto da fonte de dados com uma cadeia de conexão que não inclua credenciais. Em vez disso, o serviço de pesquisa receberá acesso à fonte de dados por meio do RBAC (controle de acesso baseado em função).

Ao configurar uma fonte de dados usando uma identidade gerenciada, você pode alterar as credenciais da fonte de dados enquanto ainda mantém a conexão entre os indexadores e a fonte de dados. Você também pode criar objetos de fonte de dados em seu código sem precisar incluir uma chave de conta ou usar Key Vault para recuperar uma chave de conta.

## <a name="limitations"></a>Limitações

As fontes de dados a seguir dão suporte à configuração de uma conexão de indexador usando identidades gerenciadas. 

* [Armazenamento de Blobs do Azure, Azure Data Lake Storage Gen2 (versão prévia), armazenamento de tabelas do Azure](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Banco de Dados SQL do Azure](search-howto-managed-identities-sql.md)

Atualmente, os seguintes recursos não dão suporte ao uso de identidades gerenciadas para configurar a conexão:
* Repositório de Conhecimento
* Habilidades personalizadas
 
## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre como configurar uma conexão do indexador usando identidades gerenciadas:

* [Armazenamento de Blobs do Azure, Azure Data Lake Storage Gen2 (versão prévia), armazenamento de tabelas do Azure](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Banco de Dados SQL do Azure](search-howto-managed-identities-sql.md)