---
title: Solucionar problemas do Apache Oozie no Azure HDInsight
description: Solucionar problemas de determinados erros do Apache Oozie no Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.openlocfilehash: fb795a9d7100019b2b1820c592f87025b77f5878
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045851"
---
# <a name="troubleshoot-apache-oozie-in-azure-hdinsight"></a>Solucionar problemas do Apache Oozie no Azure HDInsight

Com a interface do usuário do Apache Oozie, você pode exibir logs do Oozie. A IU do Oozie também contém links para os logs de JobTracker das tarefas de MapReduce que foram iniciadas pelo fluxo de trabalho. O padrão para solução de problemas deve ser:

1. Exiba o trabalho na interface do usuário da Web do Oozie.

2. Se houver um erro ou falha para uma ação específica, selecione a ação para ver se o campo **mensagem de erro** fornece mais informações sobre a falha.

3. Se estiver disponível, use a URL da ação para exibir mais detalhes, como logs do JobTracker, sobre ela.

Estes são os erros específicos que você pode chegar e como resolvê-los.

## <a name="ja009-cant-initialize-cluster"></a>JA009: não é possível inicializar o cluster

### <a name="issue"></a>Problema

O status do trabalho é alterado para **SUSPENSO**. Os detalhes do trabalho mostram o status de `RunHiveScript` como **START_MANUAL**. Selecionar a ação exibirá a seguinte mensagem de erro:

```output
JA009: Cannot initialize Cluster. Please check your configuration for map
```

### <a name="cause"></a>Causa

Os endereços do Armazenamento de Blobs do Azure usados no arquivo **job.xml** não contêm o contêiner de armazenamento ou o nome da conta de armazenamento. O formato de endereço do armazenamento de blobs deve ser `wasbs://containername@storageaccountname.blob.core.windows.net`.

### <a name="resolution"></a>Resolução

Altere os endereços de armazenamento de blobs que o trabalho usa.

---

## <a name="ja002-oozie-isnt-allowed-to-impersonate-ltusergt"></a>JA002: Oozie não pode representar &lt; usuário&gt;

### <a name="issue"></a>Problema

O status do trabalho é alterado para **SUSPENSO**. Os detalhes do trabalho mostram o status de `RunHiveScript` como **START_MANUAL**. Se você selecionar a ação, a seguinte mensagem de erro será exibida:

```output
JA002: User: oozie is not allowed to impersonate <USER>
```

### <a name="cause"></a>Causa

As configurações de permissão atuais não permitem ao Oozie representar a conta de usuário especificada.

### <a name="resolution"></a>Resolução

Oozie pode representar usuários no **`users`** grupo. Use o `groups USERNAME` para ver os grupos do qual a conta de usuário é membro. Se o usuário não for um membro do **`users`** grupo, use o seguinte comando para adicionar o usuário ao grupo:

```bash
sudo adduser USERNAME users
```

> [!NOTE]  
> Pode levar vários minutos antes de o HDInsight reconhecer que o usuário foi adicionado ao grupo.

---

## <a name="launcher-error-sqoop"></a>ERRO do Iniciador (Sqoop)

### <a name="issue"></a>Problema

O status do trabalho é alterado para **ENCERRADO**. Os detalhes do trabalho mostram o status `RunSqoopExport` como **ERRO**. Se você selecionar a ação, a seguinte mensagem de erro será exibida:

```output
Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]
```

### <a name="cause"></a>Causa

O Sqoop não conseguiu carregar o driver do banco de dados necessário para acessar o banco de dados.

### <a name="resolution"></a>Resolução

Ao usar o Sqoop em um trabalho do Oozie, você deve incluir o driver de banco de dados com os outros recursos, como o workflow.xml, usados pelo trabalho. Além disso, referencie o arquivo morto que contém o driver de banco de dados na seção `<sqoop>...</sqoop>` do workflow.xml.

Por exemplo, para que o exemplo de trabalho [use fluxos](hdinsight-use-oozie-linux-mac.md)de trabalhos Oozie do Hadoop, você usaria as seguintes etapas:

1. Copie o `mssql-jdbc-7.0.0.jre8.jar` arquivo para o diretório **/Tutorials/useoozie** :

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre8.jar /tutorials/useoozie/mssql-jdbc-7.0.0.jre8.jar
    ```

2. Modifique o `workflow.xml` para adicionar o seguinte XML a uma nova linha acima `</sqoop>`:

    ```xml
    <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
    ```

## <a name="next-steps"></a>Próximas etapas

Se você não encontrou seu problema ou não conseguiu resolver seu problema, visite um dos seguintes canais para obter mais suporte:

* Obtenha respostas de especialistas do Azure por meio do [Suporte da Comunidade do Azure](https://azure.microsoft.com/support/community/).

* Conecte-se com [@AzureSupport](https://twitter.com/azuresupport), a conta oficial do Microsoft Azure para melhorar a experiência do cliente. Como se conectar à comunidade do Azure para os recursos certos: respostas, suporte e especialistas.

* Se precisar de mais ajuda, poderá enviar uma solicitação de suporte do [portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecione **Suporte** na barra de menus ou abra o hub **Ajuda + suporte**. Para obter informações mais detalhadas, consulte [Como criar uma solicitação de Suporte do Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). O acesso ao Gerenciamento de assinaturas e ao suporte de cobrança está incluído na sua assinatura do Microsoft Azure, e o suporte técnico é fornecido por meio de um dos [Planos de suporte do Azure](https://azure.microsoft.com/support/plans/).
