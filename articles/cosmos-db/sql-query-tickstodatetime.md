---
title: TicksToDateTime na linguagem de consulta Azure Cosmos DB
description: Saiba mais sobre a função do sistema SQL TicksToDateTime no Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 89a8dba97725049b86fc6b38c09e0dd125bb48d1
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608702"
---
# <a name="tickstodatetime-azure-cosmos-db"></a>TicksToDateTime (Azure Cosmos DB)

Converte o valor de tiques especificado em um DateTime.
  
## <a name="syntax"></a>Sintaxe
  
```sql
TicksToDateTime (<Ticks>)
```

## <a name="arguments"></a>Argumentos

*Tiques*  

Um valor numérico assinado, o número atual de tiques de 100 nanossegundos decorridos desde a época do UNIX. Em outras palavras, é o número de tiques de 100 nanossegundos decorridos desde 00:00:00 quinta-feira, 1 de janeiro de 1970.

## <a name="return-types"></a>Tipos de retorno

Retorna o valor de cadeia de caracteres do UTC de data e hora do ISO 8601 no formato `YYYY-MM-DDThh:mm:ss.fffffffZ` em que:
  
  |Formatar|Descrição|
  |-|-|
  |AAAA|ano de quatro dígitos|
  |MM|mês de dois dígitos (01 = Janeiro, etc.)|
  |DD|dia de dois dígitos do mês (01 a 31)|
  |T|signifier para o início dos elementos de hora|
  |hh|hora de dois dígitos (00 a 23)|
  |MM|minutos de dois dígitos (00 a 59)|
  |ss|segundos de dois dígitos (00 a 59)|
  |. fffffff|segundos fracionários de sete dígitos|
  |Z|Designador UTC (tempo Universal Coordenado)||
  
  Para obter mais informações sobre o formato ISO 8601, consulte [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

## <a name="remarks"></a>Comentários

TicksToDateTime retornará `undefined` se o valor de tiques especificado for inválido.

## <a name="examples"></a>Exemplos
  
O exemplo a seguir converte os tiques em um DateTime:

```sql
SELECT TicksToDateTime(15943368134575530) AS DateTime
```

```json
[
    {
        "DateTime": "2020-07-09T23:20:13.4575530Z"
    }
]
```  

## <a name="next-steps"></a>Próximas etapas

- [Funções de data e hora Azure Cosmos DB](sql-query-date-time-functions.md)
- [Funções de sistema do Azure Cosmos DB](sql-query-system-functions.md)
- [Introdução ao Azure Cosmos DB](introduction.md)
