---
title: Como exibir ativos de dados relacionados no Catálogo de Dados do Azure
description: Este artigo explica como exibir os ativos de dados relacionados de um ativo de dados selecionado no Catálogo de Dados do Azure.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: how-to
ms.date: 08/01/2019
ms.openlocfilehash: fb5352b4cc84801296d9e621053672328b02692a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523359"
---
# <a name="how-to-view-related-data-assets-in-azure-data-catalog"></a>Como exibir ativos de dados relacionados no Catálogo de Dados do Azure?
O Catálogo de Dados do Azure permite que você exiba os ativos de dados relacionados a um ativo de dados selecionado exiba as relações entre eles. 

## <a name="supported-data-sources"></a>Fontes de dados com suporte 
Quando você registra os ativos de dados das fontes de dados a seguir, o Catálogo de Dados do Azure registra automaticamente metadados sobre as relações de união entre os ativos de dados selecionados. 

- SQL Server
- Banco de Dados SQL do Azure
- MySQL
- Oracle

> [!NOTE]
> Para o Catálogo de Dados importar a relação entre os ativos de dados, você deve registrar os dois ativos ao mesmo tempo. Se você tiver adicionado um deles separadamente, adicione-o novamente e os outros ativos de dados para importar a relação entre elas.

## <a name="view-related-data-assets"></a>Exibir ativos de dados relacionados
Para exibir os ativos de dados relacionados a um conjunto de dados selecionado, use a guia **Relações** conforme mostrado na imagem a seguir: 

![Catálogo de Dados do Azure – exibir ativos de dados relacionados](media/data-catalog-how-to-view-related-data-assets/relationships-tab.png)

Neste exemplo, existem duas relações para o ativo de dados **ProductSubcategory** selecionado: 

- A coluna ProductSubcategoryID da tabela Produto tem uma relação de chave estrangeira com a coluna ProductSubcategoryID da tabela ProductSubcategory selecionada. 
- A coluna ProductCategoryID da tabela ProductSubCategory tem uma relação de chave estrangeira com a coluna ProductCategoryID da tabela ProductCategory selecionada.

> [!NOTE]
> Observe a direção da seta na exibição de árvore de relações.  

Para ver mais detalhes, como o nome totalmente qualificado da coluna, mova o mouse sobre ela e você verá um pop-up semelhante à imagem a seguir: 

![Catálogo de Dados do Azure – pop-up de relação](media/data-catalog-how-to-view-related-data-assets/relationship-popup.png)

Para incluir relações entre os ativos que já foram registrados, registre novamente esses ativos.

## <a name="next-steps"></a>Próximas etapas
- [Como gerenciar ativos de dados](data-catalog-how-to-manage.md)
