---
title: ST_ISVALID na linguagem de consulta Azure Cosmos DB
description: Saiba mais sobre a função do sistema SQL ST_ISVALID no Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8fbddbe82ae13585b8259a66dffaeef8024baf5d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "71349370"
---
# <a name="st_isvalid-azure-cosmos-db"></a>ST_ISVALID (Azure Cosmos DB)
 Retorna um valor Booliano que indica se a expressão especificada de Ponto, Polígono ou LineString GeoJSON é válida.  
  
## <a name="syntax"></a>Sintaxe
  
```sql
ST_ISVALID(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   É um ponto geojson, polígono ou expressão de LineString.  
  
## <a name="return-types"></a>Tipos de retorno
  
  Retorna uma expressão booliana.  
  
## <a name="examples"></a>Exemplos
  
  O exemplo a seguir mostra como verificar se um ponto é válido usando ST_VALID.  
  
  Por exemplo, esse ponto tem um valor de latitude que não está no intervalo de valores válido [-90, 90] e, portanto, a consulta retornará false.  
  
  No caso dos polígonos, a especificação GeoJSON exige que o último par de coordenadas fornecido seja igual ao primeiro para criar uma forma fechada. Os pontos em um polígono devem ser especificados no sentido anti-horário. Um polígono especificado no sentido horário representa o inverso da região dentro dele.  
  
```sql
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 Este é o conjunto de resultados.  
  
```json
[{ "b": false }]  
```  

## <a name="next-steps"></a>Próximas etapas

- [Funções espaciais Azure Cosmos DB](sql-query-spatial-functions.md)
- [Funções de sistema do Azure Cosmos DB](sql-query-system-functions.md)
- [Introdução ao Azure Cosmos DB](introduction.md)
