---
title: Conectividade SSL/TLS-banco de dados do Azure para MariaDB
description: Informações para configurar o Banco de Dados do Azure para MariaDB e aplicativos associados a fim de usar as conexões SSL adequadamente
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: a108459985f235f0280354ef7b4fa0cb181f5dda
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90054238"
---
# <a name="ssltls-connectivity-in-azure-database-for-mariadb"></a>Conectividade SSL/TLS no banco de dados do Azure para MariaDB
O Banco de Dados do Azure para MariaDB dá suporte à conexão de seu servidor de banco de dados com aplicativos cliente usando o protocolo SSL. Impor conexões SSL entre seu servidor de banco de dados e os aplicativos cliente ajuda a proteger contra ataques de "intermediários" criptografando o fluxo de dados entre o servidor e seu aplicativo.

> [!IMPORTANT] 
> O certificado raiz SSL está definido para expirar a partir de 26 de outubro, 2020 (10/26/2020). Atualize seu aplicativo para usar o [novo certificado](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem). Para saber mais, confira [atualizações de certificado planejadas](concepts-certificate-rotation.md)

## <a name="default-settings"></a>Configurações padrão
Por padrão, o serviço de banco de dados deve ser configurado para exigir conexões SSL ao se conectar ao MariaDB.  Recomendamos evitar desabilitar a opção SSL sempre que possível.

Ao provisionar um novo servidor de Banco de Dados do Azure para MariaDB por meio do portal do Azure e da CLI do Azure, a imposição de conexões SSL está habilitada por padrão.

Em alguns casos, os aplicativos exigem um arquivo de certificado local gerado por meio de um arquivo de certificado de AC (autoridade de certificação) confiável para se conectar com segurança. Atualmente, os clientes podem **usar apenas** o certificado predefinido para se conectar a um banco de dados do Azure para o servidor MariaDB que está localizado em https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem . 

Da mesma forma, os links a seguir apontam para os certificados para servidores em nuvens soberanas: [Azure governamental](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem), [Azure China](https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem)e [Azure Alemanha](https://www.d-trust.net/cgi-bin/D-TRUST_Root_Class_3_CA_2_2009.crt).

Cadeias de conexão para várias linguagens de programação são mostradas no Portal do Azure. Essas cadeias de conexão incluem os parâmetros SSL necessários para conectar-se ao banco de dados. No Portal do Azure, selecione seu servidor. No cabeçalho **Configurações**, selecione as **Cadeias de conexão**. O parâmetro SSL varia de acordo com o conector, por exemplo "ssl=true" ou "sslmode=require" ou "sslmode=required" e outras variações.

Para saber como habilitar ou desabilitar a conexão SSL durante o desenvolvimento de aplicativos, consulte [Como configurar o SSL](howto-configure-ssl.md).

## <a name="tls-enforcement-in-azure-database-for-mariadb"></a>Imposição de TLS no banco de dados do Azure para MariaDB

O banco de dados do Azure para MariaDB dá suporte à criptografia para clientes que se conectam ao servidor de banco de dados usando o protocolo TLS. O TLS é um protocolo padrão do setor que garante conexões de rede seguras entre o servidor de banco de dados e os aplicativos cliente, permitindo que você atenda aos requisitos de conformidade.

### <a name="tls-settings"></a>Configurações de protocolo TLS

O banco de dados do Azure para MariaDB fornece a capacidade de impor a versão de TLS para as conexões de cliente. Para impor a versão de TLS, use a configuração de opção de **versão mínima do TLS** . Os valores a seguir são permitidos para esta configuração de opção:

|  Configuração de TLS mínima             | Versão do TLS do cliente com suporte                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (padrão) | Não é necessário TLS                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 e superior         |
| TLS1_1                           | TLS 1,1, TLS 1,2 e superior              |
| TLS1_2                           | TLS versão 1,2 e superior                  |


Por exemplo, definir o valor da versão de configuração de TLS mínimo como TLS 1,0 significa que o servidor permitirá conexões de clientes usando TLS 1,0, 1,1 e 1.2 +. Como alternativa, definir isso como 1,2 significa que você só permite conexões de clientes que usam o TLS 1.2 + e todas as conexões com TLS 1,0 e TLS 1,1 serão rejeitadas.

> [!Note] 
> Por padrão, o banco de dados do Azure para MariaDB não impõe uma versão mínima do TLS (a configuração `TLSEnforcementDisabled` ).
>
> Depois de aplicar uma versão mínima do TLS, não será possível desabilitar mais tarde a imposição mínima da versão.

Para saber como definir a configuração de TLS para o banco de dados do Azure para MariaDB, consulte [como definir a configuração de TLS](howto-tls-configurations.md).

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre [regras de firewall de servidor](concepts-firewall-rules.md)
- Saiba como [Configurar o SSL](howto-configure-ssl.md)
- Saiba como [Configurar o TLS](howto-tls-configurations.md)
