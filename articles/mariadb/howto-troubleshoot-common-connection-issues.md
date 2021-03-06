---
title: Solucionar problemas de conexão-banco de dados do Azure para MariaDB
description: Saiba como solucionar problemas de conexão com o banco de dados do Azure para MariaDB, incluindo erros transitórios que exigem novas tentativas, problemas de firewall e interrupções.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: troubleshooting
ms.date: 3/18/2020
ms.openlocfilehash: 521853fd361007159d7e497eb7e7c1f3d200731c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82101443"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-mariadb"></a>Solucionar problemas de conexão ao Banco de Dados do Azure para MariaDB

Os problemas de conexão podem ser causados por uma variedade de coisas, incluindo:

* Configurações de firewall
* Tempo limite da conexão
* Informações de logon incorretas
* Limite máximo atingido em alguns Banco de Dados do Azure para MariaDB
* Problemas com a infraestrutura do serviço
* Manutenção executada no serviço
* A alocação de computação do servidor é alterada pelo dimensionamento do número de vCores ou pela movimentação para outra camada de serviço

Geralmente, os problemas de conexão do Banco de Dados do Azure para MariaDB podem ser classificados da seguinte maneira:

* Erros transitórios (de curta duração ou intermitentes)
* Erros persistentes ou não transitórios (erros regularmente recorrentes)

## <a name="troubleshoot-transient-errors"></a>Solucionar problemas de erros transitórios

Quando a manutenção é executada, o sistema encontra um erro com o hardware ou software ou se você altera a camada de serviço ou vCores do seu servidor, ocorrem erros transitórios. O banco de dados do Azure para o serviço MariaDB possui alta disponibilidade integrada e foi projetado para atenuar esses tipos de problemas automaticamente. No entanto, seu aplicativo perde sua conexão ao servidor por um curto período de tempo de geralmente menos de 60 segundos, no máximo. Alguns eventos ocasionalmente podem levar mais tempo para serem corrigidos, como quando uma transação grande causa uma recuperação de execução longa.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Etapas para resolver problemas de conectividade temporários

1. Confira o [Painel de Serviços do Microsoft Azure](https://azure.microsoft.com/status) quanto a quaisquer interrupções conhecidas que tenham ocorrido durante o tempo em que o erro foi relatado pelo aplicativo.
2. Os aplicativos que se conectam a um serviço de nuvem, como o Banco de Dados do Azure para MariaDB, devem esperar erros temporários e implementar a lógica de repetição para lidar com esses erros, em vez de exibi-los como erros do aplicativo para os usuários. Analise [Tratamento de erros de conectividade transitórios para o Banco de Dados do Azure para MariaDB](concepts-connectivity.md) para melhores práticas e diretrizes de design para lidar com erros transitórios.
3. Conforme um servidor se aproxima dos limites de recursos, os erros podem parecer um problema de conectividade transitório. Veja [Limitações no Banco de Dados do Azure para MariaDB](concepts-limits.md).
4. Se problemas de conectividade continuarem, se a duração pela qual o aplicativo encontra o erro exceder 60 segundos ou se você vir várias ocorrências do erro em um determinado dia, envie uma solicitação de suporte do Azure selecionando **Obter Suporte** no site de [Suporte do Azure](https://azure.microsoft.com/support/options) .

## <a name="troubleshoot-persistent-errors"></a>Solucionar erros persistentes

Se o aplicativo persistentemente falhar ao se conectar ao Banco de Dados do Azure para MariaDB, isso geralmente indica um problema com um dos seguintes:

* Configuração do firewall: O Banco de Dados do Azure para MariaDB ou o firewall do lado do cliente está bloqueando as conexões.
* Reconfiguração da rede no lado do cliente: um novo endereço IP ou um servidor proxy foi adicionado.
* Erro do usuário: por exemplo, você pode ter parâmetros de conexão digitados, como o nome do servidor na cadeia de conexão ou um sufixo * \@ ServerName* ausente no nome de usuário.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Etapas para resolver os problemas de conectividade temporários

1. Configure [as regras de firewall](howto-manage-firewall-portal.md) para permitir o endereço IP do cliente. Para fins de testes temporários, configure uma regra de firewall usando 0.0.0.0 como o endereço IP inicial e usando 255.255.255.255 como o endereço IP final. Isso abrirá o servidor para todos os endereços IP. Se isso resolver seu problema de conectividade, remova essa regra e crie uma regra de firewall para um intervalo de endereçamento ou um endereço IP adequadamente limitado.
2. Em todos os firewalls entre o cliente e a Internet, certifique-se de que a porta 3306 esteja aberta para conexões de saída.
3. Verifique a cadeia de conexão e outras configurações de conexão. Analise [Como conectar aplicativos ao Banco de Dados do Azure para MariaDB](howto-connection-string.md).
4. Verifique a integridade do serviço no painel. Se você considerar que há uma interrupção regional, consulte [visão geral da continuidade de negócios com o banco de dados do Azure para MariaDB](concepts-business-continuity.md) para obter as etapas para recuperar em uma nova região.

## <a name="next-steps"></a>Próximas etapas

* [Manipulação de erros de conectividade transitórios para o Banco de Dados do Azure para MariaDB](concepts-connectivity.md)
