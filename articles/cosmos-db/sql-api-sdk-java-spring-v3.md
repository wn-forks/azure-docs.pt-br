---
title: Recursos e notas de versão do Spring data Azure Cosmos DB V3 para a API do SQL
description: Saiba mais sobre o Spring data Azure Cosmos DB V3 para a API do SQL, incluindo datas de lançamento, datas de desativação e alterações feitas entre cada versão do SDK do Java assíncrono do SQL Azure Cosmos DB.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/18/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 4b2d474f25209034034db092ca971bff6b78d73a
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90068722"
---
# <a name="spring-data-azure-cosmos-db-v3-for-core-sql-api-release-notes-and-resources"></a>Spring data Azure Cosmos DB V3 para a API de núcleo (SQL): notas de versão e recursos
> [!div class="op_single_selector"]
> * [SDK v3 do .NET](sql-api-sdk-dotnet-standard.md)
> * [SDK do .NET v2](sql-api-sdk-dotnet.md)
> * [SDK v2 do .NET Core](sql-api-sdk-dotnet-core.md)
> * [SDK v2 do feed de alterações do .NET](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [SDK do Java v4](sql-api-sdk-java-v4.md)
> * [SDK do Java Assíncrono v2](sql-api-sdk-async-java.md)
> * [SDK do Java Síncrono v2](sql-api-sdk-java.md)
> * [Spring data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring data v3](sql-api-sdk-java-spring-v3.md)
> * [Conector do Spark](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [Provedor de recursos REST](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Executor em massa-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Executor em massa – Java](sql-api-sdk-bulk-executor-java.md)

O Spring data Azure Cosmos DB versão 3 for Core (SQL) permite que os desenvolvedores usem Azure Cosmos DB em aplicativos Spring. O Spring data Azure Cosmos DB expõe a interface Spring data para manipular bancos de dados e coleções, trabalhar com documentos e emitir consultas. As APIs de sincronização e assíncrona (reativa) têm suporte no mesmo artefato Maven. 

O Spring data Azure Cosmos DB tem uma dependência da estrutura Spring Data. A equipe do SDK do Azure Cosmos DB lança artefatos do Maven para versões 2,2 e 2,3 do Spring Data.

O [Spring Framework](https://spring.io/projects/spring-framework) é um modelo de programação e configuração que simplifica o desenvolvimento de aplicativos Java. O Spring simplifica o "encanamento" de aplicativos usando a injeção de dependência. Muitos desenvolvedores gostam do Spring, pois tornam a criação e o teste de aplicativos mais simples. O [Spring boot](https://spring.io/projects/spring-boot) estende essa manipulação da estrutura com um olho voltado para o desenvolvimento de aplicativos e microserviços da Web. [Spring data](https://spring.io/projects/spring-data) é um modelo de programação e estrutura para acessar armazenamentos de dados como Azure Cosmos DB do contexto de um aplicativo Spring ou Spring boot. 

Você pode usar o Spring data Azure Cosmos DB em seus aplicativos de [nuvem Spring do Azure](https://azure.microsoft.com/services/spring-cloud/) .

> [!IMPORTANT]  
> Estas notas de versão são para a versão 3 do Spring data Azure Cosmos DB. Você pode encontrar [notas de versão para a versão 2 aqui](sql-api-sdk-java-spring-v2.md). 
>
> O Spring data Azure Cosmos DB dá suporte apenas à API do SQL.
>
> Consulte estes artigos para obter informações sobre o Spring data em outras APIs de Azure Cosmos DB:
> * [Spring data para Apache Cassandra com Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Spring data MongoDB com Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Gremlin de Spring data com Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>

## <a name="start-here"></a>Comece por aqui

# <a name="explore"></a>[Explorar](#tab/explore)

<img src="media/sql-api-sdk-java-spring-v3/up-arrow.png" alt="explore the tabs above" width="80"/>

#### <a name="these-tabs-contain-basic-spring-data-azure-cosmos-db-samples"></a>Essas guias contêm exemplos básicos de Spring data Azure Cosmos DB.

# <a name="pomxml"></a>[pom.xml](#tab/pom)

### <a name="configure-dependencies"></a>Configurar dependências

Dois artefatos do Maven do Spring data Azure Cosmos DB versão 3 estão disponíveis.

- Artefato que depende do Spring data Framework versão 2,2:
  ```xml
  <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-spring-data-2-2-cosmos</artifactId>
      <version>latest</version>
  </dependency>
  ```

- Artefato que depende do Spring data Framework versão 2,3:
  ```xml
  <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-spring-data-2-3-cosmos</artifactId>
      <version>latest</version>
  </dependency>
  ```

# <a name="connect"></a>[Connect](#tab/connect)

### <a name="connect"></a>Conectar

Especifique Azure Cosmos DB conta e detalhes do contêiner. O Spring data Azure Cosmos DB cria automaticamente o cliente e se conecta ao contêiner.

[Application. Properties](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-2-cosmos-java-getting-started/src/main/resources/application.properties):
```
cosmos.uri=${ACCOUNT_HOST}
cosmos.key=${ACCOUNT_KEY}
cosmos.secondaryKey=${SECONDARY_ACCOUNT_KEY}

dynamic.collection.name=spel-property-collection
# Populate query metrics
cosmos.queryMetricsEnabled=true
```

# <a name="doc-ops"></a>[Ops do documento](#tab/docs)

### <a name="document-operations"></a>Operações de documento

[Criar](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Create)]

[Excluir](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Delete)]

# <a name="query"></a>[Consulta](#tab/queries)

[Consulta](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/blob/main/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java): [!code-java[](~/spring-data-azure-cosmos-db-sql-tutorial/azure-spring-data-2-3-cosmos-java-getting-started/src/main/java/com/azure/spring/data/cosmostutorial/SampleApplication.java?name=Query)]

---

## <a name="resources"></a>Recursos

| Recurso | Spring data Framework 2,2 | Spring data Framework 2,3 |
|---|---|
| **Baixe o SDK** | [Maven](https://mvnrepository.com/artifact/com.azure/azure-spring-data-2-2-cosmos) | [Maven](https://mvnrepository.com/artifact/com.azure/azure-spring-data-2-3-cosmos) |
|**Contribuir para o SDK** | [Repositório de Azure Cosmos DB de dados Spring no GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-2-2-cosmos) | [Repositório de Azure Cosmos DB de dados Spring no GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-spring-data-2-3-cosmos) | 
|**Tutorial**| [Tutorial de Azure Cosmos DB de dados Spring no GitHub](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/tree/main/azure-spring-data-2-2-cosmos-java-getting-started) | [Tutorial de Azure Cosmos DB de dados Spring no GitHub](https://github.com/Azure-Samples/azure-spring-data-cosmos-java-sql-api-getting-started/tree/main/azure-spring-data-2-3-cosmos-java-getting-started) |

## <a name="release-history"></a>Histórico de versões

### <a name="300-beta2-unreleased"></a>3.0.0-beta. 2 (não lançado)

### <a name="300-beta1-august-17-2020"></a>3.0.0-beta. 1 (17 de agosto de 2020)
#### <a name="new-features"></a>Novos recursos
* Atualiza a ID do grupo para `com.azure` .
* Atualiza a ID do artefato para `azure-spring-data-2-3-cosmos` .
* Atualiza a dependência do SDK do Azure-Cosmos para `4.3.2-beta.2` .
* Adiciona suporte para entidades de auditoria: gerenciamento automático de `createdBy` `createdDate` campos,, `lastModifiedBy` e `lastModifiedDate` anotados.
* Adiciona `@GeneratedValue` suporte de anotação para geração de ID automática para campos de ID do `String` tipo.
* Adiciona suporte à configuração de vários bancos de dados para contas de Azure Cosmos DB único com vários bancos de dados e várias contas de Azure Cosmos DB com vários bancos de dados.
* Adiciona suporte para `@Version` anotação em qualquer campo de cadeia de caracteres.
* Atualiza os tipos de retorno da API de sincronização para `Iterable` tipos em vez de `List` .
* Expõe `CosmosClientBuilder` da Azure Cosmos DB SDK como Spring Bean para a `@Configuration` classe.
* Atualizações `CosmosConfig` para conter métricas de consulta e implementação do processador de diagnóstico de resposta.
* Adiciona suporte para retornar o `Optional` tipo de dados para consultas de resultado único.
#### <a name="renames"></a>Renomeia
* `CosmosDbFactory` para o `CosmosFactory` .
* `CosmosDBConfig` para o `CosmosConfig` .
* `CosmosDBAccessException` para o `CosmosAccessException` .
* `Document` anotação para `Container` anotação.
* `DocumentIndexingPolicy` anotação para `CosmosIndexingPolicy` anotação.
* `DocumentQuery` para o `CosmosQuery` .
* aplicativo. Properties sinalizador `populateQueryMetrics` para `queryMetricsEnabled` .
#### <a name="key-bug-fixes"></a>Correções de bug de chave
* Agendamento da tarefa de log de diagnóstico para `Parallel` threads para evitar O bloqueio de threads de e/s de sub-rede.
* Corrige o bloqueio otimista na operação de exclusão.
* Corrige o problema com a cláusula de saída de consultas de escape `IN` .
* Corrige o problema permitindo o `long` tipo de dados para `@Id` .
* Corrige o problema permitindo `boolean` , `long` , `int` e `double` como tipos de dados para a `@PartitionKey` anotação.
* Correções `IgnoreCase` e `AllIgnoreCase` palavras-chave para consultas de Ignorar maiúsculas e minúsculas.
* Remove o valor de unidade de solicitação padrão de 4.000 quando contêineres são criados automaticamente.

## <a name="faq"></a>Perguntas frequentes
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre o [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).

Saiba mais sobre o [Spring Framework](https://spring.io/projects/spring-framework).

Saiba mais sobre o [Spring boot](https://spring.io/projects/spring-boot).

Saiba mais sobre o [Spring data](https://spring.io/projects/spring-data).