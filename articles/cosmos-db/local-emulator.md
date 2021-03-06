---
title: Desenvolver localmente com o emulador do Azure Cosmos
description: Usando o Emulador do Azure Cosmos, você pode desenvolver e testar seu aplicativo no local gratuitamente, sem criar uma assinatura do Azure.
ms.service: cosmos-db
ms.topic: how-to
author: markjbrown
ms.author: mjbrown
ms.date: 08/19/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: ece2fdf5c75decb9a2139b973ad4bbb3f0803a0b
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89011167"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Usar o Emulador do Azure Cosmos para desenvolvimento e teste locais

O emulador do Azure Cosmos DB fornece um ambiente local que emula o serviço do Azure Cosmos para fins de desenvolvimento. Com o Emulador do Azure Cosmos, você pode desenvolver e testar seu aplicativo localmente sem criar uma assinatura Azure ou incorrer em custos. Quando estiver satisfeito com o funcionamento de seu aplicativo no Emulador do Azure Cosmos, você poderá passar a usar uma conta do Azure Cosmos na nuvem.

Você pode desenvolver usando o emulador do Azure Cosmos com as contas de [SQL](local-emulator.md#sql-api), [Cassandra](local-emulator.md#cassandra-api), [MongoDB](local-emulator.md#azure-cosmos-dbs-api-for-mongodb), [Gremlin](local-emulator.md#gremlin-api), e [API](local-emulator.md#table-api) de tabela. No entanto, neste momento o modo de exibição do Data Explorer no emulador é totalmente compatível com clientes para API do SQL somente. 

## <a name="how-the-emulator-works"></a>Como o emulador funciona

O emulador do Azure Cosmos fornece uma emulação de alta fidelidade do serviço do Azure Cosmos DB. Ele dá suporte a uma funcionalidade idêntica ao Azure Cosmos DB, incluindo suporte para criar e consultar dados, provisionar e dimensionar contêineres, bem como executar procedimentos armazenados e gatilhos. Desenvolva e teste aplicativos usando o emulador do Azure Cosmos e implante-os no Azure em escala global, apenas fazendo uma única alteração de configuração no ponto de extremidade de conexão do Azure Cosmos DB.

Embora a emulação do serviço do Azure Cosmos DB seja fiel, a implementação do emulador é diferente do serviço. Por exemplo, o emulador usa componentes padrão do SO, como sistema de arquivos local para persistência e pilha do protocolo HTTPS para conectividade. A funcionalidade que depende da infraestrutura do Azure, como replicação global, latência de único dígito em milissegundos para leituras/gravações e níveis de consistência ajustáveis, não está disponível.

Também é possível migrar dados entre o emulador do Azure Cosmos e o serviço Azure Cosmos usando a [Ferramenta de Migração de Dados do Microsoft Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool).

Você pode executar o emulador do Azure Cosmos no contêiner do Docker do Windows, confira o [Docker Hub](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/) quanto ao comando docker pull e [GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker) para o `Dockerfile` e mais informações.

## <a name="differences-between-the-emulator-and-the-service"></a>Diferenças entre o emulador e o serviço

Como o Emulador do Azure Cosmos fornece um ambiente emulado em execução em uma estação de trabalho do desenvolvedor local, há algumas diferenças na funcionalidade entre o emulador e a conta do Azure Cosmos na nuvem:

* Neste momento, o Data Explorer no emulador é totalmente compatível com clientes para a API do SQL. A exibição do Gerenciador de Dados e operações para APIs do Azure Cosmos DB, como o MongoDB, tabela, Graph e APIs de Cassandra não têm suporte total.
* O Emulador do Azure Cosmos dá suporte apenas uma única conta fixa e uma chave mestra conhecida. A regeneração de chave não é possível no emulador do Cosmos do Azure, mas a chave padrão pode ser alterada usando a opção da linha de comando.
* O emulador Cosmos do Azure dá suporte a uma conta do Azure Cosmos no modo de [taxa de transferência provisionado](set-throughput.md) ; Atualmente, ele não dá suporte a uma conta do Azure Cosmos no modo sem [servidor](serverless.md) .
* O emulador do Azure Cosmos não é um serviço escalonável e não dará suporte a um grande número de contêineres.
* O emulador do Azure Cosmos não oferta diferentes [níveis de consistência do Azure Cosmos DB](consistency-levels.md).
* O emulador do Azure Cosmos não oferece [replicação de várias regiões](distribute-data-globally.md).
* Como a sua cópia do emulador do Azure Cosmos pode não ser atualizada com as alterações mais recentes com o serviço do Azure Cosmos DB, você deve consultar o [planejador de capacidade do Azure Cosmos DB](https://www.documentdb.com/capacityplanner) para estimar as necessidades de taxa de transferência (RUs) de seu aplicativo.
* Ao usar o emulador do Cosmos do Azure, por padrão, você pode criar até 25 contêineres de tamanho fixo (suportado apenas usando os SDKs do Azure Cosmos DB) ou os 5 contêineres ilimitados usando o emulador do Azure Cosmos. Para saber mais sobre como alterar esse valor, veja [Definição do valor de PartitionCount](#set-partitioncount).
* O emulador dá suporte ao tamanho de propriedade de ID máx de 254 caracteres.

## <a name="system-requirements"></a>Requisitos do sistema

O emulador do Azure Cosmos DB tem os seguintes requisitos de hardware e software:

* Requisitos de software
  * Windows Server 2012 R2, Windows Server 2016 ou Windows 10
  * Sistema operacional de 64 bits
* Requisitos mínimos de hardware
  * 2 GB de RAM
  * Espaço disponível no disco rígido de 10 GB

## <a name="installation"></a>Instalação

Você pode baixar e instalar o Emulador do Azure Cosmos no [Centro de Download da Microsoft](https://aka.ms/cosmosdb-emulator) ou você pode executar o emulador no Docker CE for Windows. Para obter instruções sobre como usar o emulador no Docker for Windows, confira [Em execução no Docker](#running-on-docker).

> [!NOTE]
> Para instalar, configurar e executar o emulador do Azure Cosmos DB, você deve ter privilégios administrativos no computador. O emulador criará ou adicionará um certificado e também definirá as regras de firewall para executar seus serviços. Portanto, é necessário para o emulador ser capaz de executar essas operações.

## <a name="running-on-windows"></a>Em execução no Windows

Para iniciar o emulador do Azure Cosmos, selecione o botão Iniciar ou pressione a tecla Windows. Comece a digitar **emulador do Azure Cosmos** e selecione o emulador na lista de aplicativos.

:::image type="content" source="./media/local-emulator/database-local-emulator-start.png" alt-text="Selecione o botão Iniciar ou pressione a tecla Windows, comece a digitar Emulador do Azure Cosmos e selecione o emulador na lista de aplicativos":::

Quando o emulador estiver em execução, você verá um ícone na área de notificação da barra de tarefas do Windows. 

:::image type="content" source="./media/local-emulator/database-local-emulator-taskbar.png" alt-text="Notificação na barra de tarefas do emulador local do Azure Cosmos DB":::

O Emulador do Azure Cosmos, por padrão, é executado no computador local ("localhost") escutando na porta 8081.

O emulador do Azure Cosmos DB é instalado em `C:\Program Files\Azure Cosmos DB Emulator` por padrão. Você também pode iniciar e parar o emulador pela linha de comando. Para saber mais, veja a [referência da ferramenta de linha de comando](#command-line).

## <a name="start-data-explorer"></a>Iniciar o Data Explorer

Quando o emulador do Azure Cosmos é iniciado, ele abre automaticamente o Data Explorer do Azure Cosmos no seu navegador. O endereço é exibido como `https://localhost:8081/_explorer/index.html`. Se você fechar o Explorer e quiser reabri-lo mais tarde, é possível abrir a URL no navegador ou iniciá-lo no Emulador do Azure Cosmos no Ícone de Bandeja do Windows, como mostrado abaixo.

:::image type="content" source="./media/local-emulator/database-local-emulator-data-explorer-launcher.png" alt-text="Iniciador do Data Explorer do emulador local do Azure Cosmos":::

## <a name="checking-for-updates"></a>Procurando atualizações

O Data Explorer indica se há uma nova atualização disponível para download.

> [!NOTE]
> Não há garantias de que os dados criados em uma versão do emulador do Azure Cosmos (consulte %LOCALAPPDATA%\CosmosDBEmulator ou configurações opcionais do caminho de dados) possam ser acessados em outras versões. Se você precisar persistir seus dados por longo prazo, é recomendável armazená-los em uma conta do Azure Cosmos e não no Emulador do Azure Cosmos.

## <a name="authenticating-requests"></a>Autenticar solicitações

Assim como no Azure Cosmos DB na nuvem, cada solicitação feita no emulador do Azure Cosmos deve ser autenticada. O Emulador do Azure Cosmos dá suporte a uma única conta fixa e a uma chave de autenticação conhecida para autenticação de chave mestra. Essa conta e a chave são as únicas credenciais permitidas para uso com o Emulador do Azure Cosmos. Eles são:

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> A chave mestra com suporte do Emulador do Azure Cosmos destina-se ao uso somente com o emulador. Você não pode usar sua conta do Microsoft Azure Cosmos DB de produção e a chave com o emulador do Azure Cosmos.

> [!NOTE]
> Se você tiver iniciado o emulador com a opção /Key, use a chave gerada em vez de `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`. Para saber mais sobre a opção /Keym veja a [referência da ferramenta da linha de comando](#command-line).

Assim como o Azure Cosmos DB, o emulador do Azure Cosmos dá suporte apenas à comunicação segura por TLS.

## <a name="running-on-a-local-network"></a>Em execução em uma rede local

Você pode executar o emulador em uma rede local. Para habilitar o acesso à rede, especifique a opção `/AllowNetworkAccess` na [linha de comando](#command-line-syntax), o que também requer que você especifique `/Key=key_string` ou `/KeyFile=file_name`. Você pode usar `/GenKeyFile=file_name` para gerar um arquivo com uma chave aleatória antecipadamente. Em seguida, você pode passá-lo para `/KeyFile=file_name` ou `/Key=contents_of_file`.

Para habilitar o acesso de rede pela primeira vez, o usuário deve desligar o emulador e excluir o diretório de dados do emulador (%LOCALAPPDATA%\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Desenvolver com o emulador

### <a name="sql-api"></a>API do SQL

Depois que o emulador do Azure Cosmos estiver em execução na área de trabalho, você poderá usar qualquer [SDK do Azure Cosmos DB](sql-api-sdk-dotnet-standard.md) com suporte ou a [API REST do Azure Cosmos DB](/rest/api/cosmos-db/) para interagir com o emulador. O Emulador do Azure Cosmos também inclui um Data Explorer interno que permite criar contêineres para a API do SQL ou Cosmos DB para API do Mongo DB, bem como exibir e editar itens sem precisar escrever nenhum código.

```csharp
// Connect to the Azure Cosmos Emulator running locally
CosmosClient client = new CosmosClient(
   "https://localhost:8081", 
    "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>API do Azure Cosmos DB para MongoDB

Depois que o Emulador do Azure Cosmos estiver em execução na sua área de trabalho, você poderá usar a [API para MongoDB do Azure Cosmos DB](mongodb-introduction.md) para interagir com o emulador. Inicie o emulador no prompt de comando como administrador com "/EnableMongoDbEndpoint". Em seguida, use a seguinte cadeia de conexão para se conectar à conta da API do MongoDB:

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>API de Tabela

Depois que o emulador do Azure Cosmos estiver em execução na sua área de trabalho, você poderá usar qualquer SDK da API da Tabela do Azure Cosmos DB com suporte ou o [SDK da API de Tabela do Azure Cosmos DB](table-storage-how-to-use-dotnet.md) para interagir com o emulador. Inicie o emulador de prompt de comando como administrador com "/EnableTableEndpoint". Em seguida, execute o código a seguir para conectar-se à conta de API da tabela:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>API Cassandra

Inicie o emulador em um prompt de comando do administrador com "/EnableCassandraEndpoint". Como alternativa, você também pode definir a variável de ambiente `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true`.

* [Instalar o Python 2.7](https://www.python.org/downloads/release/python-2716/)

* [Instalar Cassandra CLI/CQLSH](https://cassandra.apache.org/download/)

* Execute os seguintes comandos em uma janela de prompt de comando regular:

  ```bash
  set Path=c:\Python27;%Path%
  cd /d C:\sdk\apache-cassandra-3.11.3\bin
  set SSL_VERSION=TLSv1_2
  set SSL_VALIDATE=false
  cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
  ```

* No shell CQLSH, execute os comandos a seguir para se conectar ao ponto de extremidade do Cassandra:

  ```bash
  CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
  DESCRIBE keyspaces;
  USE mykeyspace;
  CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
  INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
  SELECT * from table1;
  EXIT
  ```

### <a name="gremlin-api"></a>API do Gremlin

Inicie o emulador em um prompt de comando do administrador com "/EnableGremlinEndpoint". Como alternativa, você também pode definir a variável de ambiente `AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`

* [Instale o apache-tinkerpop-gremlin-console-3.3.4](https://archive.apache.org/dist/tinkerpop/3.3.4).

* No Data Explorer do emulador, crie um banco de dados "db1" e uma coleção "coll1"; para a chave de partição, escolha "/name"

* Execute os seguintes comandos em uma janela de prompt de comando regular:

  ```bash
  cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
  copy /y conf\remote.yaml conf\remote-localcompute.yaml
  notepad.exe conf\remote-localcompute.yaml
    hosts: [localhost]
    port: 8901
    username: /dbs/db1/colls/coll1
    password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
    connectionPool: {
    enableSsl: false}
    serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
    config: { serializeResultToString: true  }}

  bin\gremlin.bat
  ```

* No shell Gremlin, execute os comandos a seguir para se conectar ao ponto de extremidade do Gremlin:

  ```bash
  :remote connect tinkerpop.server conf/remote-localcompute.yaml
  :remote console
  :> g.V()
  :> g.addV('person1').property(id, '1').property('name', 'somename1')
  :> g.addV('person2').property(id, '2').property('name', 'somename2')
  :> g.V()
  ```

## <a name="export-the-tlsssl-certificate"></a>Exportar o certificado TLS/SSL

As linguagens e o runtime .NET usam o Repositório de Certificados do Windows para a conexão segura com o emulador local do Azure Cosmos DB. Outras linguagens têm seu próprio método de gerenciar e usar certificados. O Java usa seu próprio [repositório de certificados](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) enquanto o Python usa [wrappers de soquete](https://docs.python.org/2/library/ssl.html).

Para obter um certificado para uso com linguagens e runtimes que não se integram ao Repositório de Certificados do Windows, você precisará exportá-lo usando o Gerenciador de Certificados do Windows. Você pode iniciá-lo executando certlm.msc ou seguir as instruções passo a passo em [Exportar os certificados do emulador do Azure Cosmos](./local-emulator-export-ssl-certificates.md). Depois que o gerenciador de certificados estiver em execução, abra os Certificados Pessoais, conforme mostrado abaixo, e exporte o certificado com o nome amigável "DocumentDBEmulatorCertificate" como um arquivo X.509 codificado em BASE-64 (.cer).

:::image type="content" source="./media/local-emulator/database-local-emulator-ssl_certificate.png" alt-text="Certificado TLS/SSL do emulador local do Azure Cosmos DB":::

O certificado X.509 pode ser importado no repositório de certificados Java seguindo as instruções em [Adicionando um certificado ao repositório de certificados de AC do Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store). Depois que o certificado é importado para o repositório de certificados, os clientes para API do SQL e do Azure Cosmos DB para MongoDB podem se conectar ao Emulador do Azure Cosmos.

Ao se conectar ao emulador de SDKs Python e Node.js, a verificação TLS é desabilitada.

## <a name="command-line-tool-reference"></a><a id="command-line"></a>Referência da ferramenta de linha de comando
No local da instalação, você pode usar a linha de comando para iniciar e interromper o emulador, configurar opções e executar outras operações.

### <a name="command-line-syntax"></a>Sintaxe da linha de comando

```cmd
Microsoft.Azure.Cosmos.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/EnableMongoDbEndpoint] [/?]
```

Para exibir a lista de opções, digite `Microsoft.Azure.Cosmos.Emulator.exe /?` no prompt de comando.

|**Opção** | **Descrição** | **Comando**| **Argumentos**|
|---|---|---|---|
|[No arguments] | Inicia o Emulador do Azure Cosmos com as configurações padrão. |Microsoft.Azure.Cosmos.Emulator.exe| |
|[Ajuda] |Exibe a lista de argumentos de linha de comando com suporte.|Microsoft.Azure.Cosmos.Emulator.exe /? | |
| GetStatus |Obtém o status do Emulador do Azure Cosmos. O status é indicado pelo código de saída: 1 = Iniciando, 2 = Em execução, 3 = Parado. Um código de saída negativo indica que ocorreu um erro. Nenhum outro resultado é produzido. | Microsoft.Azure.Cosmos.Emulator.exe /GetStatus| |
| Shutdown| Desliga o Emulador do Azure Cosmos.| Microsoft.Azure.Cosmos.Emulator.exe /Shutdown | |
|DataPath | Especifica o caminho no qual armazenar os arquivos de dados. O valor padrão é %LocalAppdata%\CosmosDBEmulator. | Microsoft.Azure.Cosmos.Emulator.exe /DataPath=\<datapath\> | \<datapath\>: um caminho acessível |
|Porta | Especifica o número da porta a ser usada para o emulador. O valor padrão é de 8081. |Microsoft.Azure.Cosmos.Emulator.exe /Port=\<port\> | \<port\>: número da porta único |
| ComputePort | Especificado o número da porta a ser usada para o serviço de Gateway de interoperabilidade de computação. A porta de investigação de ponto de extremidade HTTP do Gateway é calculada como ComputePort + 79. Portanto, ComputePort e ComputePort + 79 deve estar aberta e disponível. O valor padrão é 8900. | Microsoft.Azure.Cosmos.Emulator.exe /ComputePort=\<computeport\> | \<computeport\>: número da porta único |
| EnableMongoDbEndpoint=3.2 | Habilita a API MongoDB 3.2 | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.2 | |
| EnableMongoDbEndpoint=3.6 | Habilita a API MongoDB 3.6 | Microsoft.Azure.Cosmos.Emulator.exe /EnableMongoDbEndpoint=3.6 | |
| MongoPort | Especifica o número da porta a ser usada para API de compatibilidade do MongoDB. O valor padrão é de 10255. |Microsoft.Azure.Cosmos.Emulator.exe /MongoPort=\<mongoport\>|\<mongoport\>: número da porta único|
| EnableCassandraEndpoint | Sobre a API do Cassandra | Microsoft.Azure.Cosmos.Emulator.exe /EnableCassandraEndpoint | |
| CassandraPort | Especifica o número da porta a ser usada para o ponto de extremidade Cassandra. O valor padrão é 10350. | Microsoft.Azure.Cosmos.Emulator.exe /CassandraPort=\<cassandraport\> | \<cassandraport\>: número da porta único |
| EnableGremlinEndpoint | Habilitar a API do Gremlin | Microsoft.Azure.Cosmos.Emulator.exe /EnableGremlinEndpoint | |
| GremlinPort | Número da porta a ser usado para o ponto de extremidade do Gremlin. O valor padrão é: 8901. | Microsoft.Azure.Cosmos.Emulator.exe /GremlinPort=\<port\> | \<port\>: número da porta único |
|EnableTableEndpoint | Permite a API de Tabela do Azure | Microsoft.Azure.Cosmos.Emulator.exe /EnableTableEndpoint | |
|TablePort | O número da porta a ser usado para o ponto de extremidade da Tabela do Azure. O valor padrão é 8902. | Microsoft.Azure.Cosmos.Emulator.exe /TablePort=\<port\> | \<port\>: número da porta único|
| KeyFile | A chave de autorização de leitura do arquivo especificado. Use a opção de /GenKeyFile para gerar um keyfile | Microsoft.Azure.Cosmos.Emulator.exe /KeyFile=\<file_name\> | \<file_name\>: Caminho para o compartilhamento o arquivo |
| ResetDataPath | Remove recursivamente todos os arquivos no caminho especificado. Se você não especificar um caminho, o padrão será %LOCALAPPDATA%\CosmosDbEmulator | Microsoft.Azure.Cosmos.Emulator.exe /ResetDataPath=\<path> | \<path\>: Caminho do arquivo  |
| StartTraces  |  Iniciar os logs de rastreamento de depuração de coleta usando LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe /StartTraces | |
| StopTraces     | Parar os logs de rastreamento de depuração de coleta usando LOGMAN. | Microsoft.Azure.Cosmos.Emulator.exe /StopTraces  | |
| StartWprTraces  |  Comece a coletar logs de rastreamento de depuração usando a ferramenta de Gravação de Desempenho do Windows. | Microsoft.Azure.Cosmos.Emulator.exe /StartWprTraces | |
| StopWprTraces     | Pare de coletar logs de rastreamento de depuração usando a ferramenta de Gravação de Desempenho do Windows. | Microsoft.Azure.Cosmos.Emulator.exe /StopWprTraces  | |
|FailOnSslCertificateNameMismatch | Por padrão o emulador regenera o certificado TLS/SSL autoassinado dele, se o SAN do certificado não inclui o nome de domínio do host do Emulador, o endereço do IPv4 local, “localhost” e “127.0.0.1”. Com essa opção, o emulador falhará na inicialização em vez disso. Em seguida, você deve usar a opção /GenCert para criar e instalar um certificado TLS/SSL autoassinado. | Microsoft.Azure.Cosmos.Emulator.exe /FailOnSslCertificateNameMismatch  | |
| GenCert | Gerar e instalar um novo certificado TLS/SSL autoassinado. opcionalmente, incluindo uma lista separada por vírgulas de nomes DNS adicionais para acessar o emulador pela rede. | Microsoft.Azure.Cosmos.Emulator.exe /GenCert=\<dns-names\> |\<dns-names\>: lista opcional separada por vírgula de nomes DNS adicionais  |
| DirectPorts |Especifica as portas a serem usadas para conectividade direta. Os padrões são 10251,10252,10253,10254. | Microsoft.Azure.Cosmos.Emulator.exe /DirectPorts:\<directports\> | \<directports\>: lista de 4 portas delimitada por vírgula |
| Chave |Chave de autorização para o emulador. A chave deve ser a codificação de base 64 de um vetor de 64 bytes. | Microsoft.Azure.Cosmos.Emulator.exe /Key:\<key\> | \<key\>: A chave deve ser a codificação base-64 de um vetor de 64 bytes|
| EnableRateLimiting | Especifica que o comportamento de limitação da taxa de solicitação está habilitado. |Microsoft.Azure.Cosmos.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |Especifica que o comportamento de limitação da taxa de solicitação está desabilitado. |Microsoft.Azure.Cosmos.Emulator.exe /DisableRateLimiting | |
| NoUI | Não mostra o emulador de interface do usuário. | Microsoft.Azure.Cosmos.Emulator.exe /NoUI | |
| NoExplorer | Não mostra o Data Explorer na inicialização. |Microsoft.Azure.Cosmos.Emulator.exe /NoExplorer | | 
| PartitionCount | Especifica o número máximo de contêineres particionados. Veja [Alterar o número de contêineres](#set-partitioncount) para saber mais. | Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=\<partitioncount\> | \<partitioncount\>: Número máximo permitido de contêineres de partição única. O valor padrão é 25. O máximo permitido é 250.|
| DefaultPartitionCount| Especifica o número padrão de partições para um contêiner particionado. | Microsoft.Azure.Cosmos.Emulator.exe /DefaultPartitionCount=\<defaultpartitioncount\> | \<defaultpartitioncount\> O valor padrão é 25.|
| AllowNetworkAccess | Permite o acesso para o emulador através de uma rede. Também é necessário passar /Key=\<key_string\> ou /KeyFile=\<file_name\> para habilitar o acesso à rede. | Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=\<key_string\> ou Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /KeyFile=\<file_name\>| |
| NoFirewall | Não ajuste as regras de firewall quando a opção /AllowNetworkAccess for usada. |Microsoft.Azure.Cosmos.Emulator.exe /NoFirewall | |
| GenKeyFile | Gere uma nova chave de autorização e salve no arquivo especificado. A chave gerada pode ser usada com as opções /Key ou /KeyFile. | Microsoft.Azure.Cosmos.Emulator.exe /GenKeyFile=\<path to key file\> | |
| Consistência | Defina o nível de consistência padrão da conta. | Microsoft.Azure.Cosmos.Emulator.exe /Consistency=\<consistency\> | \<consistency\>: O valor deve ser um dos seguintes [níveis de consistência](consistency-levels.md): Session, Strong, Eventual ou BoundedStaleness. O valor padrão é Sessão. |
| ? | Mostre a mensagem de ajuda.| | |

## <a name="change-the-number-of-containers"></a><a id="set-partitioncount"></a>Altere o número de contêineres

Por padrão, você pode criar até 25 contêineres de tamanho fixo (suportado apenas usando os SDKs do Azure Cosmos DB) ou os 5 contêineres ilimitados usando o emulador do Azure Cosmos. Ao modificar o valor do **PartitionCount**, que você pode criar até 250 contêineres de tamanho fixo ou 50 contêineres ilimitados ou qualquer combinação dos dois que não excedam 250 contêineres de tamanho fixo (em que um contêiner ilimitado = 5 de tamanho fixo contêineres). No entanto, não é recomendado configurar o emulador para executar com mais de 200 contêineres de tamanho fixo. Por causa da sobrecarga que isso adiciona a operações de E/S de disco, que resulta em tempo limite imprevisível ao usar o ponto de extremidade de APIs.

Se você tentar criar contêiner depois que a contagem de partição atual tiver sido excedida, o emulador lançará uma exceção de ServiceUnavailable, com a mensagem de erro a seguir.

"Desculpe, estamos tendo alta demanda nesta região e não podemos atender sua solicitação neste momento. Estamos trabalhando continuamente para trazer cada vez mais capacidade online e incentivamos você a tentar novamente.
ActivityId: 12345678-1234-1234-1234-123456789abc"

Para alterar o número de contêineres disponíveis para o emulador do Azure Cosmos DB, faça o seguinte:

1. Exclua todos os dados locais do emulador do Azure Cosmos clicando com o botão direito do mouse no ícone **Emulador do Azure Cosmos DB** na bandeja do sistema e clicando em **Redefinir Dados…** .
2. Excluir todos os dados de emulador desta pasta `%LOCALAPPDATA%\CosmosDBEmulator`.
3. Saia de todas as instâncias abertas clicando com o botão direito do mouse no ícone do **Emulador do Azure Cosmos DB** na bandeja do sistema e clicando em **Sair**. Pode levar um minuto para que todas as instâncias saiam.
4. Instale a versão mais recente do [emulador Azure Cosmos](https://aka.ms/cosmosdb-emulator).
5. Inicie o emulador com o sinalizador PartitionCount definindo um valor <= 250. Por exemplo: `C:\Program Files\Azure Cosmos DB Emulator> Microsoft.Azure.Cosmos.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>Controlar o emulador

O emulador vem com um módulo PowerShell para iniciar, parar, desinstalar e recuperar o status do serviço. Execute o seguinte cmdlet para usar o módulo do PowerShell:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

ou coloque o diretório `PSModules` no seu `PSModulesPath` e importe-o conforme mostrado no comando a seguir:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Aqui é apresentado um resumo dos comandos para controlar o emulador do PowerShell:

### `Get-CosmosDbEmulatorStatus`

**Sintaxe**

`Get-CosmosDbEmulatorStatus`

**Comentários**

Retorna um destes valores ServiceControllerStatus: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running ou ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

**Sintaxe**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**Comentários**

Inicia o emulador. Por padrão, o comando aguarda até que o emulador esteja pronto para aceitar solicitações. Use a opção -NoWait, se quiser que o cmdlet seja retornado assim que ele iniciar o emulador.

### `Stop-CosmosDbEmulator`

**Sintaxe**

 `Stop-CosmosDbEmulator [-NoWait]`

**Comentários**

Parada do emulador. Por padrão, esse comando aguarda até que o emulador seja completamente desligado. Use a opção -NoWait, se quiser que o cmdlet seja retornado assim que o emulador iniciar o desligamento.

### `Uninstall-CosmosDbEmulator`

**Sintaxe**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**Comentários**

Desinstala o emulador e, opcionalmente, remove o conteúdo completo do $env:LOCALAPPDATA\CosmosDbEmulator.
O cmdlet garante que o emulador seja parado antes de desinstalá-lo.

## <a name="running-on-docker"></a>Em execução no Docker

O emulador do Azure Cosmos pode ser executado no Docker CE for Windows. O emulador não funciona no Docker for Oracle Linux.

Uma vez que você tenha o [Docker para Windows](https://www.docker.com/docker-windows) instalado, mude para contêineres do Windows clicando com o botão direito do mouse no ícone do Docker na barra de ferramentas e selecionando **Mudar para contêineres do Windows**.

Em seguida, efetue pull da imagem do emulador do Hub do Docker, executando o comando a seguir do seu shell favorito.

```bash
docker pull mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```
Para iniciar a imagem, execute os comandos a seguir.

Na linha de comando:
```cmd

md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
```

> [!NOTE]
> Se você vir um erro de conflito de porta (a porta especificada já está em uso) ao executar o comando de execução do docker, você poderá passar uma porta personalizada alterando os números de porta. Por exemplo, você pode alterar "-p 8081:8081" para "-p 443:8081"

Do PowerShell:
```powershell

md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator

```

A resposta tem aparência semelhante à seguinte:

```
Starting emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
```

Agora use o ponto de extremidade e a chave mestra da resposta no seu cliente e importe o certificado TLS/SSL no seu host. Para importar o certificado TLS/SSL, faça o seguinte em um prompt de comando do administrador:

Na linha de comando:

```cmd
cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
powershell .\importcert.ps1
```

Do PowerShell:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
.\importcert.ps1
```

Fechar o shell interativo depois de o emulador ter sido iniciado desligará o contêiner do emulador.

Para abrir o Data Explorer, navegue até a URL a seguir no seu navegador. O ponto de extremidade do emulador é fornecido na mensagem de resposta mostrada acima.

**https\://** \<emulator endpoint provided in response> **/_explorer/index.html**

Se você tiver um aplicativo cliente .NET em execução em um contêiner do Docker do Linux e estiver executando o emulador do Azure Cosmos em um computador host, siga a seção abaixo para o Linux importar o certificado para o contêiner do Docker do Linux.

## <a name="running-on-mac-or-linux"></a>Executar no Mac ou Linux<a id="mac"></a>

Atualmente, o emulador do Cosmos somente pode ser executado no Windows. Os usuários que executam o Mac ou Linux podem executar o emulador em uma máquina virtual do Windows hospedada em um hipervisor, como paralelos ou VirtualBox. Abaixo estão as etapas para habilitá-lo.

Na VM do Windows, execute o comando a seguir e anote o endereço IPv4.

```cmd
ipconfig.exe
```

No seu aplicativo, você precisa alterar o URI usado como o Ponto de extremidade para usar o endereço IPv4 retornado por `ipconfig.exe` em vez de `localhost`.

A próxima etapa, na VM do Windows, inicie o emulador do Cosmos na linha de comando usando as opções a seguir.

```cmd
Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

Por fim, precisamos resolver o processo de confiança de certificado entre o aplicativo em execução no ambiente Linux ou Mac e o emulador. Nós temos duas opções:

1. Desabilite a validação SSL no aplicativo:

# <a name="net-standard-21"></a>[.NET Standard 2.1 +](#tab/ssl-netstd21)

   Para qualquer aplicativo em execução em uma estrutura compatível com o .NET Standard 2,1 ou posterior, podemos aproveitar o `CosmosClientOptions.HttpClientFactory` :

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/HttpClientFactory/Program.cs?name=DisableSSLNETStandard21)]

# <a name="net-standard-20"></a>[.NET Standard 2.0](#tab/ssl-netstd20)

   Para qualquer aplicativo em execução em uma estrutura compatível com o .NET Standard 2,0, podemos aproveitar o `CosmosClientOptions.HttpClientFactory` :

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/HttpClientFactory/Program.cs?name=DisableSSLNETStandard20)]

# <a name="nodejs"></a>[Node.js](#tab/ssl-nodejs)

   Para Node.js aplicativos, você pode modificar o `package.json` arquivo para definir o `NODE_TLS_REJECT_UNAUTHORIZED` ao iniciar o aplicativo:

   ```json
   "start": NODE_TLS_REJECT_UNAUTHORIZED=0 node app.js
   ```

--- 

> [!NOTE]
> A desabilitação da validação SSL só é recomendada para fins de desenvolvimento e não deve ser feita durante a execução em um ambiente de produção.

2. Importe o certificado de AC do emulador para o ambiente Linux ou Mac:

### <a name="linux"></a>Linux

Se você estiver trabalhando no Linux, o .NET será retransmitido no OpenSSL para realizar a validação:

1. [Exporte o certificado em formato PFX](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-tlsssl-certificate) (o PFX está disponível ao optar por exportar a chave privada). 

1. Copie esse arquivo PFX para seu ambiente Linux.

1. Converta o arquivo PFX em um arquivo CRT

   ```bash
   openssl pkcs12 -in YourPFX.pfx -clcerts -nokeys -out YourCTR.crt
   ```

1. Copie o arquivo CRT para a pasta que contém certificados personalizados em sua distribuição do Linux. Normalmente, em distribuições do Debian, ela está localizada em `/usr/local/share/ca-certificates/`.

   ```bash
   cp YourCTR.crt /usr/local/share/ca-certificates/
   ```

1. Atualize os certificados de AC, que atualizarão a pasta `/etc/ssl/certs/`.

   ```bash
   update-ca-certificates
   ```

### <a name="macos"></a>macOS

Use as seguintes etapas se estiver trabalhando no Mac:

1. [Exporte o certificado em formato PFX](./local-emulator-export-ssl-certificates.md#how-to-export-the-azure-cosmos-db-tlsssl-certificate) (o PFX está disponível ao optar por exportar a chave privada).

1. Copie esse arquivo PFX para seu ambiente Mac.

1. Abra o aplicativo de *Acesso do Conjunto de Chaves* e importe o arquivo PFX.

1. Abra a lista de certificados e identifique aquele com o nome `localhost`.

1. Abra o menu de contexto desse item específico, selecione *Obter Item* e, na opção *Confiar* > *Ao usar este certificado*, selecione *Sempre Confiar*. 

   :::image type="content" source="./media/local-emulator/mac-trust-certificate.png" alt-text="Abra o menu de contexto desse item específico, selecione Obter Item e, na opção Confiar – Ao usar este certificado, selecione Sempre Confiar":::

Após seguir essas etapas, seu ambiente confiará no certificado usado pelo Emulador ao se conectar ao endereço IP exposto por `/AllowNetworkAccess`.

## <a name="troubleshooting"></a>Solução de problemas

Use as dicas a seguir para ajudar a solucionar problemas encontrados com o emulador do Azure Cosmos:

- Se você tiver instalado uma nova versão do emulador e se houver erros, redefina os dados. Você pode redefinir seus dados clicando com o botão direito do mouse no ícone emulador do Azure Cosmos DB na bandeja do sistema e clicando em Redefinir Dados... Se isso não resolver os erros, você pode desinstalar o emulador e as versões mais antigas do emulador se encontradas, remova o diretório "C:\Program files\Azure Cosmos DB Emulator" e reinstale o emulador. Veja [Desinstalar o emulador local](#uninstall) para obter instruções.

- Se o Emulador do Azure Cosmos falhar, colete os arquivos de despejo da pasta '%LOCALAPPDATA%\CrashDumps', compacte-os e abra um tíquete de suporte no [portal do Azure](https://portal.azure.com).

- Se você enfrentar falhas em `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe`, isso pode ser um sintoma de onde os contadores de desempenho estão em um estado corrompido. Normalmente, executar o seguinte comando em um prompt de comando do administrador corrige o problema:

  ```cmd
  lodctr /R
   ```

- Se encontrar um problema de conectividade, [colete os arquivos de rastreamento](#trace-files), compacte-os e abra um tíquete de suporte no [portal do Azure](https://portal.azure.com).

- Se você receber uma mensagem de **Serviço Indisponível**, o emulador poderá estar falhando em inicializar a pilha de rede. Verifique se você tem o cliente seguro Pulse ou o cliente de redes Juniper instalado, uma vez que seus drivers de filtro de rede podem causar o problema. Desinstalar os drivers de filtro de rede de terceiros geralmente corrige o problema. Como alternativa, inicie o emulador com /DisableRIO, que mudará a comunicação de rede do emulador para Winsock regular. 

- Se você encontrar **"proibido", "mensagem": "a solicitação está sendo feita com um protocolo de criptografia proibido em trânsito ou codificação. Verificar a configuração de protocolo mínimo permitido por SSL/TLS da conta...** problemas de conectividade, isso pode ser causado por alterações globais no sistema operacional (por exemplo, Insider Preview versão 20170) ou as configurações do navegador que habilitam o TLS 1,3 como padrão. Pode ocorrer um erro semelhante ao usar o SDK para executar uma solicitação no emulador Cosmos, como **Microsoft.Azure.Documents.DocumentClientException: a solicitação está sendo feita com uma criptografia proibida no protocolo de trânsito ou na codificação. Verifique a configuração de protocolo mínimo permitido por SSL/TLS**. Isso é esperado no momento, pois o emulador Cosmos só aceita e funciona com o protocolo TLS 1.2. A solução recomendada é alterar as configurações e usar o padrão para TLS 1,2; por exemplo, no Gerenciador do IIS, navegue até "sites"-> "sites padrão" e localize as "ligações do site" para a porta 8081 e edite-as para desabilitar o TLS 1,3. Uma operação semelhante pode ser realizada para o navegador da Web por meio das opções de "Configurações".

- Enquanto o emulador é executado, se o computador entrar em modo de suspensão ou executar atualizações do sistema operacional, talvez você veja uma mensagem **O serviço não disponível no momento**. Reinicie os dados do emulador clicando com o botão direito do mouse no ícone que aparece na bandeja de notificação do Windows e selecione **Redefinir dados**.

### <a name="collect-trace-files"></a><a id="trace-files"></a>Coletar arquivos de rastreamento

Para coletar rastreamentos de depuração, execute os seguintes comandos em um prompt de comando administrativo:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `Microsoft.Azure.Cosmos.Emulator.exe /shutdown`. Confira a bandeja do sistema para ter certeza de que o programa foi desligado. Isso pode levar um minuto. Você também pode simplesmente clicar em **Sair** na interface do usuário do emulador do Azure Cosmos.
3. `Microsoft.Azure.Cosmos.Emulator.exe /startwprtraces`
4. `Microsoft.Azure.Cosmos.Emulator.exe`
5. Reproduza o problema. Se o Data Explorer não está funcionando, você só precisa aguardar que o navegador abra por alguns segundos para capturar o erro.
6. `Microsoft.Azure.Cosmos.Emulator.exe /stopwprtraces`
7. Navegue até `%ProgramFiles%\Azure Cosmos DB Emulator` e localize o arquivo docdbemulator_000001.etl.
8. Abra um tíquete de suporte no [portal do Azure](https://portal.azure.com) e inclua o arquivo .etl com as etapas de reprodução.

### <a name="uninstall-the-local-emulator"></a><a id="uninstall"></a>Desinstalar o emulador local

1. Saia de todas as instâncias abertas do emulador local clicando com o botão direito do mouse no ícone do Emulador do Azure Cosmos na bandeja do sistema e clicando em Sair. Pode levar um minuto para que todas as instâncias saiam.
2. Na caixa de pesquisa do Windows, digite **Aplicativos e recursos** e clique no resultado de **Aplicativos e recursos (Configurações do sistema)** .
3. Na lista de aplicativos, vá para **Emulador do Azure Cosmos DB**, selecione-o, clique em **Desinstalar** e então confirme e clique em **Desinstalar** novamente.
4. Após a desinstalação do aplicativo, navegue até `%LOCALAPPDATA%\CosmosDBEmulator` e exclua a pasta.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como usar o emulador local para o desenvolvimento local gratuito. Agora, você pode seguir para o próximo tutorial e aprender a exportar certificados TLS/SSL do emulador.

> [!div class="nextstepaction"]
> [Exportar os certificados do Emulador do Azure Cosmos DB](local-emulator-export-ssl-certificates.md)
