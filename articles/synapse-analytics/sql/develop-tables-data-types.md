---
title: Tipos de dados de tabela no SQL Synapse
description: Recomendações para definir tipos de dados de tabela no Synapse SQL.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 789976949f23467801589343483ebcad11fa5c56
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90032887"
---
# <a name="table-data-types-in-synapse-sql"></a>Tipos de dados de tabela no SQL Synapse

Recomendações para definir tipos de dados de tabela no Synapse SQL. 

## <a name="data-types"></a>Tipos de dados

Synapse SQL dá suporte aos tipos de dados mais usados. Para obter uma lista dos tipos de dados com suporte, consulte [tipos de dados](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) na instrução CREATE TABLE. 

## <a name="minimize-row-length"></a>Minimizar o tamanho da linha

Minimizar o tamanho dos tipos de dados reduz o tamanho da linha, o que leva a um desempenho de consulta melhor. Use o menor tipo de dados que funcione para seus dados.

- Evite definir as colunas de caractere como um tamanho padrão grande. Por exemplo, se o maior valor for 25 caracteres, defina a coluna como VARCHAR(25).
- Evite usar [NVARCHAR][NVARCHAR] quando precisar somente de VARCHAR.
- Quando possível, use NVARCHAR(4000) ou VARCHAR(8000) em vez de NVARCHAR(MAX) ou VARCHAR(MAX).

> [!NOTE]
> Se você estiver usando tabelas externas do polybase para carregar suas tabelas de pool do SQL, o comprimento definido da linha da tabela não pode exceder 1 MB. Quando uma linha com dados de tamanho variável exceder 1 MB, você poderá carregar a linha com BCP, mas não com PolyBase.

## <a name="identify-unsupported-data-types"></a>Identificar tipos de dados sem suporte

Se você estiver migrando seu banco de dados de outro banco de dado SQL, você poderá encontrar tipos que não têm suporte no SQL Synapse. Use esta consulta para descobrir os tipos de dados sem suporte em seu esquema SQL existente.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Soluções alternativas para os tipos de dados sem suporte

A lista a seguir mostra os tipos de dados aos quais o Synapse SQL não dá suporte e fornece alternativas que você pode usar em vez dos tipos de dados sem suporte.

| Tipos de dados sem suporte | Solução alternativa |
| --- | --- |
| [geometria](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [geografia](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(4000) |
| [imagem](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [text](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Divida a coluna em várias colunas fortemente tipadas. |
| [table](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Se estiver usando o pool SQL, você poderá converter em tabelas temporárias. Se estiver usando o SQL (versão prévia), você pode considerar armazenar dados no armazenamento usando o [CETAS](../sql/develop-tables-cetas.md). |
| [timestamp](/sql/t-sql/data-types/date-and-time-types) |Refazer o código para usar [datetime2](/sql/t-sql/data-types/datetime2-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) e a função [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). Somente as constantes são suportados como padrões, portanto, current_timestamp não pode ser definida como uma restrição padrão. Se precisar migrar os valores de versão de linha de uma coluna tipada com o carimbo de data/hora, use [BINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) ou [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) para os valores de versão de linha NOT NULL ou NULL. |
| [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [tipo definido pelo usuário](/sql/relational-databases/native-client/features/using-user-defined-types) |Converta para o tipo de dados nativo quando possível. |
| valores padrão | Os valores padrão dão suporte somente a literais e constantes. |

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o desenvolvimento de tabelas, consulte [Visão geral da tabela](develop-overview.md).
