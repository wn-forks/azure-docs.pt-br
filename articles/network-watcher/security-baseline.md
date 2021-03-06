---
title: Linha de base de segurança do Azure para observador de rede
description: Linha de base de segurança do Azure para observador de rede
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 28b3bc5adfc3c2e83de658947193b6046a455c32
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89231496"
---
# <a name="azure-security-baseline-for-network-watcher"></a>Linha de base de segurança do Azure para observador de rede

A linha de base de segurança do Azure para o observador de rede contém recomendações que o ajudarão a melhorar a postura de segurança de sua implantação.

A linha de base para esse serviço é extraída do [Azure Security Benchmark versão 1.0](https://docs.microsoft.com/azure/security/benchmarks/overview), que fornece recomendações sobre como proteger suas soluções de nuvem no Azure com nossas diretrizes de melhores práticas.

Para obter mais informações, confira a [Visão geral sobre linhas de base de segurança do Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Segurança de rede

*Para saber mais, confira [Controle de segurança: Segurança de rede](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: proteger os recursos do Azure em redes virtuais

**Orientação**: não aplicável; O observador de rede tem a capacidade de monitorar a conexão entre um ponto de extremidade e uma máquina virtual, mas não pode ser protegido por uma rede virtual, um grupo de segurança de rede ou um firewall do Azure.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: monitorar e registrar a configuração e o tráfego de redes virtuais, sub-redes e NICs

**Orientação**: não aplicável; O observador de rede do Azure tem a capacidade de analisar logs de fluxo e fornecer visualizações avançadas de dados; no entanto, o observador de rede em si não gera logs de fluxo ou pacotes capturable.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="13-protect-critical-web-applications"></a>1.3: proteger aplicativos Web críticos

**Diretriz**: Não aplicável; essa recomendação destina-se a aplicativos Web em execução no Serviço de Aplicativo do Azure ou recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Rejeitar comunicações com endereços IP maliciosos conhecidos

**Orientação**: não aplicável; O observador de rede do Azure não é um recurso com potencial exposição a redes públicas que precisam ser protegidos contra tráfego mal-intencionado.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="15-record-network-packets"></a>1,5: gravar pacotes de rede

**Orientação**: não aplicável; O observador de rede do Azure tem a capacidade de analisar logs de fluxo e fornecer visualizações avançadas de dados; no entanto, o observador de rede em si não gera logs de fluxo ou pacotes capturable.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: implantar os sistemas de detecção de intrusão/prevenção de invasão baseado em rede (IDS/IPS)

**Orientação**: não aplicável; O observador de rede do Azure não é um recurso com potencial exposição a redes públicas que precisam ser protegidos contra tráfego mal-intencionado.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="17-manage-traffic-to-web-applications"></a>1.7: gerenciar o tráfego para aplicativos Web

**Diretriz**: Não aplicável; essa recomendação destina-se a aplicativos Web em execução no Serviço de Aplicativo do Azure ou recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Minimizar a complexidade e a sobrecarga administrativa de regras de segurança de rede

**Orientação**: não aplicável; O observador de rede do Azure tem a capacidade de analisar logs de fluxo e fornecer visualizações avançadas de dados; no entanto, o observador de rede não é um ponto de extremidade no qual você pode filtrar ou permitir o tráfego com marcas de serviço ou grupos de segurança de rede.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: manter configurações de segurança padrão para dispositivos de rede

**Orientação**: definir e implementar configurações de segurança padrão para o observador de rede do Azure com o Azure Policy. Use Azure Policy aliases no namespace "Microsoft. Network" para criar políticas personalizadas para auditar ou impor a configuração de rede de suas instâncias do observador de rede. Você também pode fazer uso de definições de política internas, como:

Implantar o observador de rede quando redes virtuais são criadas

O Observador de Rede deve ser habilitado

* [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Como criar uma política personalizada com aliases de política](https://docs.microsoft.com/azure/governance/policy/tutorials/create-custom-policy-definition)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="110-document-traffic-configuration-rules"></a>1.10: documentar regras de configuração de tráfego

**Orientação**: não aplicável; enquanto o observador de rede do Azure dá suporte a marcas, o próprio observador de rede não controla o fluxo do tráfego de rede.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: usar ferramentas automatizadas para monitorar as configurações de recursos de rede e detectar alterações

**Orientação**: Use o log de atividades do Azure para monitorar as alterações feitas no observador de rede do Azure. Você pode criar alertas no Azure Monitor que serão disparados quando ocorrerem alterações.

* [Como exibir e recuperar eventos do log de atividades do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Como criar alertas no Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

## <a name="logging-and-monitoring"></a>Log e monitoramento

*Para saber mais, confira [Controle de segurança: Registro em log e monitoramento](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Usar fontes de sincronização de tempo aprovadas

**Orientação**: não aplicável; A Microsoft mantém a fonte de tempo usada para recursos do Azure, como o observador de rede do Azure, para carimbos de data/hora nos logs.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="22-configure-central-security-log-management"></a>2.2: configurar o gerenciamento central de log de segurança

**Orientação**: Use o log de atividades do Azure para monitorar as configurações e detectar alterações para as instâncias do observador de rede do Azure. Além do plano de controle (por exemplo, portal do Azure), o observador de rede em si não gera logs relacionados ao tráfego de rede. O observador de rede fornece ferramentas para monitorar, diagnosticar, exibir métricas e habilitar ou desabilitar logs de recursos em uma rede virtual do Azure.

* [Como exibir e recuperar eventos do log de atividades do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Entender o observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: habilitar o registro em log de auditoria para recursos do Azure

**Orientação**: Use o log de atividades do Azure para monitorar as configurações e detectar alterações para as instâncias do observador de rede do Azure. Além do plano de controle (por exemplo, portal do Azure), o observador de rede em si não gera logs de auditoria. O observador de rede fornece ferramentas para monitorar, diagnosticar, exibir métricas e habilitar ou desabilitar logs de recursos em uma rede virtual do Azure.

* [Como exibir e recuperar eventos do log de atividades do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Entender o observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: coletar logs de segurança de sistemas operacionais

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="25-configure-security-log-storage-retention"></a>2.5: Configurar a retenção de armazenamento do log de segurança

**Diretrizes**: em Azure monitor, defina o período de retenção de log para espaços de trabalho de log Analytics associados ao observador de rede do Azure de acordo com os regulamentos de conformidade da sua organização.

* [Como definir parâmetros de retenção de log](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="26-monitor-and-review-logs"></a>2.6: monitorar e revisar logs

**Orientação**: Use o log de atividades do Azure para monitorar as configurações e detectar alterações para as instâncias do observador de rede do Azure. Além do plano de controle (por exemplo, portal do Azure), o observador de rede em si não gera logs relacionados ao tráfego de rede. O observador de rede fornece ferramentas para monitorar, diagnosticar, exibir métricas e habilitar ou desabilitar logs de recursos em uma rede virtual do Azure.

* [Como exibir e recuperar eventos do log de atividades do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Entender o observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: habilitar alertas para atividades anômalas

**Orientação**: você pode configurar o para receber alertas com base nos logs de atividade relacionados ao observador de rede do Azure. Azure Monitor permite que você configure um alerta para enviar uma notificação por email, chamar um webhook ou invocar um aplicativo lógico do Azure.

* [Como gerenciar alertas na central de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="28-centralize-anti-malware-logging"></a>2.8: centralizar o registro em log de antimalware

**Orientação**: não aplicável; O observador de rede do Azure não processa nem produz logs relacionados a anti-malware.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="29-enable-dns-query-logging"></a>2.9: Habilitar o registro em log de consultas DNS

**Orientação**: não aplicável; O observador de rede do Azure não processa nem produz logs relacionados ao DNS.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="210-enable-command-line-audit-logging"></a>2.10: Habilitar o registro em log de auditoria de linha de comando

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

## <a name="identity-and-access-control"></a>Identidade e controle de acesso

*Para saber mais, confira [Controle de segurança: Identidade e controle de acesso](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: Manter um inventário de contas administrativas

**Diretrizes**: Mantenha um inventário das contas de usuário que têm acesso administrativo ao plano de controle (por exemplo, portal do Azure) do observador de rede do Azure. Para usar os recursos do observador de rede, a conta que você faz logon no Azure, deve ser atribuída às funções internas de proprietário, colaborador ou colaborador de rede ou atribuída a uma função personalizada que é atribuída às ações listadas para recursos específicos do observador de rede.

Você pode usar o painel IAM (controle de acesso e identidade) no portal do Azure para sua assinatura para configurar o controle de acesso baseado em função do Azure (RBAC do Azure). As funções são aplicadas a usuários, grupos, entidades de serviço e identidades gerenciadas no Active Directory.

* [Entender o RBAC do Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

* [As permissões de controle de acesso baseadas em função são necessárias para usar os recursos do Observador de Rede.](https://docs.microsoft.com/azure/network-watcher/required-rbac-permissions)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="32-change-default-passwords-where-applicable"></a>3.2: alterar senhas padrão quando aplicável

**Diretriz**: O Azure AD não tem o conceito de senhas padrão. Outros recursos do Azure que exigem uma senha forçam a criação de uma senha com requisitos de complexidade e um comprimento mínimo de senha, que difere dependendo do serviço. Você é responsável por aplicativos de terceiros e serviços do Marketplace que podem usar senhas padrão.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Usar contas administrativas dedicadas

**Diretriz**: Crie procedimentos operacionais padrão em relação ao uso de contas administrativas dedicadas. Use a identidade e o gerenciamento de acesso da Central de Segurança do Azure para monitorar a quantidade de contas administrativas.

Além disso, para ajudá-lo a controlar contas administrativas dedicadas, você pode seguir as recomendações da Central de Segurança do Azure ou as políticas internas do Azure, como:
- Deve haver mais de um proprietário atribuído à sua assinatura
- As contas preteridas com permissões de proprietário devem ser removidas de sua assinatura
- As contas externas com permissões de proprietário devem ser removidas de sua assinatura

* [Como usar a Central de Segurança do Azure para monitorar a identidade e o acesso (versão prévia)](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Como usar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: usar o SSO (logon único) com o Azure Active Directory

**Orientação**: não aplicável; o SSO (logon único) adiciona segurança e conveniência quando os usuários entram em aplicativos personalizados no Azure Active Directory (AD). O acesso ao observador de rede do Azure já está integrado com o Azure Active Directory e é acessado por meio do portal do Azure, bem como da API REST do Azure Resource Manager.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: usar a autenticação multifator para todos os acessos baseados no Azure Active Directory

**Diretrizes**: habilite Azure Active Directory autenticação multifator e siga as recomendações de gerenciamento de acesso e identidade da central de segurança do Azure.

* [Como habilitar a MFA no Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Como monitorar identidade e acesso na Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: usar computadores dedicados (estações de trabalho com acesso privilegiado) para todas as tarefas administrativas

**Orientação**: Use uma estação de trabalho de acesso privilegiado (Paw) com a autenticação multifator do Azure (MFA) habilitada para fazer logon e configurar seus recursos relacionados ao sentinela do Azure.

* [Estações de trabalho com acesso privilegiado](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Planejar uma implantação da Autenticação Multifator do Azure baseada em nuvem](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: registrar em log e alertar sobre atividades suspeitas de contas administrativas

**Diretriz**: Use o Privileged Identity Management (PIM) do Azure Active Directory (AD) para geração de logs e alertas quando atividades suspeitas ou inseguras ocorrem no ambiente.

Além disso, use as detecções de risco do Azure Active Directory para ver alertas e relatórios sobre o comportamento do usuário suspeito.

* [Como implantar o Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Entenda as detecções de risco do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Gerenciar recursos do Azure provenientes somente de locais aprovados

**Diretriz**: Use localizações nomeadas de acesso condicional para permitir o acesso ao portal do Azure somente a agrupamentos lógicos de intervalos de endereços IP ou de países/regiões específicos.

* [Como configurar localizações nomeadas no Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="39-use-azure-active-directory"></a>3.9: Use o Azure Active Directory Domain Services

**Diretrizes**: Use Azure Active Directory (AD do Azure) como o sistema de autenticação e autorização central para suas instâncias do Azure Sentinel. O Azure AD protege os dados usando criptografia forte para dados em repouso e em trânsito. O Azure Active Directory também inclui sais, hashes e armazena com segurança as credenciais do usuário.

* [Como criar e configurar uma instância do Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: revisar e reconciliar regularmente o acesso do usuário

**Diretrizes**: o Azure Active Directory (AD) fornece logs para ajudá-lo a descobrir contas obsoletas. Além disso, use as revisões de acesso de identidade do Azure para gerenciar com eficiência as associações de grupo, o acesso aos aplicativos empresariais e as atribuições de função. O acesso de usuários pode ser examinado regularmente para garantir que somente os usuários corretos tenham acesso contínuo.

* [Entender os relatórios do Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Como usar as revisões de acesso de identidade do Azure](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: monitorar tentativas de acessar credenciais desativadas

**Diretrizes**: Use o Azure Active Directory (AD) como o sistema de autenticação e autorização central para o observador de rede do Azure. O Azure AD protege os dados usando criptografia forte para dados em repouso e em trânsito. O Azure Active Directory também inclui sais, hashes e armazena com segurança as credenciais do usuário.

Você tem acesso à atividade de entrada do Azure AD, às fontes de log de eventos de auditoria e de risco, que permitem a integração com o Azure Sentinel ou um SIEM de terceiros.

Você pode simplificar esse processo criando configurações de diagnóstico para contas de usuário do Azure AD e enviando logs de auditoria e logs de entrada para um espaço de trabalho Log Analytics. Você pode configurar os alertas de log desejados no Log Analytics.

* [Como integrar os logs de atividades do Azure ao Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Como integrar o Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: alertar sobre o desvio de comportamento de logon na conta

**Diretrizes**: para o desvio do comportamento de logon da conta no plano de controle (por exemplo, portal do Azure), use Azure ad Identity Protection e recursos de detecção de risco para configurar respostas automatizadas para ações suspeitas detectadas relacionadas a identidades de usuário. Você também pode ingerir dados no Azure Sentinel para uma investigação mais aprofundada.

* [Como exibir o logon arriscado do Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Como configurar e habilitar políticas de risco de proteção de identidade](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Como integrar o Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: fornecer à Microsoft acesso a dados relevantes do cliente durante cenários de suporte

**Orientação**: não aplicável; Sistema de Proteção de Dados do Cliente não é aplicável ao observador de rede do Azure.

* [Lista de serviços suportados do Sistema de Proteção de Dados do Cliente](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

## <a name="data-protection"></a>Proteção de dados

*Para saber mais, confira [Controle de segurança: Proteção de dados](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Manter um inventário de informações confidenciais

**Diretriz**: Use marcas para ajudar a controlar os recursos do Azure que armazenam ou processam informações confidenciais.

* [Como criar e usar marcas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: isolar sistemas que armazenam ou processam informações confidenciais

**Diretriz**: implemente assinaturas e/ou grupos de gerenciamento separados para desenvolvimento, teste e produção.

* [Como criar assinaturas adicionais do Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Como criar Grupos de Gerenciamento](https://docs.microsoft.com/azure/governance/management-groups/create)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: monitorar e bloquear a transferência não autorizada de informações confidenciais

**Diretrizes**: a Microsoft gerencia a infraestrutura subjacente para o observador de rede do Azure e recursos relacionados e implementou controles estritos para evitar a perda ou a exposição dos dados do cliente.

* [Entender a proteção de dados do cliente no Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Microsoft

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: criptografar todas as informações confidenciais em trânsito

**Orientação**: se você estiver usando o gateway de VPN do Azure para criar uma conexão segura entre sua rede local e suas redes virtuais do Azure, verifique se o gateway de rede local está configurado com os parâmetros de criptografia e de comunicação IPSec compatíveis. Qualquer configuração incorreta resultará em perda de conectividade entre a rede local e o Azure.

* [Parâmetros de IPSec com suporte para o gateway de VPN do Azure](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity)

* [Como configurar uma conexão site a site no portal do Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

**Monitoramento da Central de Segurança do Azure**: Não disponível no momento

**Responsabilidade**: Cliente

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: usar uma ferramenta de descoberta ativa para identificar dados confidenciais

**Orientação**: não aplicável; O observador de rede do Azure não mantém nenhum dado do cliente.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: usar o RBAC do Azure para controlar o acesso a recursos

**Orientação**: você pode usar o painel iam (controle de acesso e identidade) no portal do Azure para sua assinatura para configurar o controle de acesso baseado em função do Azure (RBAC do Azure). As funções são aplicadas a usuários, grupos, entidades de serviço e identidades gerenciadas no Active Directory. Você pode usar funções internas ou funções personalizadas para indivíduos e grupos.

Para usar os recursos do observador de rede, a conta que você faz logon no Azure, deve ser atribuída às funções internas de proprietário, colaborador ou colaborador de rede ou atribuída a uma função personalizada que é atribuída às ações listadas para recursos específicos do observador de rede.

* [Como configurar o RBAC do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

* [Entender as permissões do RBAC do Azure no observador de rede](https://docs.microsoft.com/azure/network-watcher/required-rbac-permissions)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: usar a prevenção contra perda de dados baseada em host para impor controle de acesso

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação. A Microsoft gerencia a infraestrutura subjacente para o observador de rede do Azure e implementou controles estritos para evitar a perda ou a exposição dos dados do cliente.

* [Proteção de dados do cliente do Microsoft Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Criptografar informações confidenciais em repouso

**Orientação**: não aplicável; O observador de rede do Azure não mantém nenhum dado do cliente. O observador de rede armazena logs e outras informações no armazenamento do Azure, onde os dados são criptografados em repouso.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: registrar e alertar sobre alterações em recursos críticos do Azure

**Diretrizes**: Use Azure monitor com o log de atividades do Azure para criar alertas para quando as alterações ocorrerem no observador de rede do Azure e outros recursos críticos ou relacionados.

* [Como criar alertas para eventos do log de atividades do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

## <a name="vulnerability-management"></a>Gerenciamento de vulnerabilidades

*Para saber mais, confira [Controle de segurança: Gerenciamento de vulnerabilidades](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Executar ferramentas automatizadas de verificação de vulnerabilidade

**Orientação**: não aplicável; A Microsoft executa o gerenciamento de vulnerabilidades nos sistemas subjacentes que dão suporte ao observador de rede do Azure.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Implantar solução automatizada de gerenciamento de patch de sistema operacional

**Diretriz**: Não aplicável. Esta recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: implantar solução de gerenciamento de patch automatizado para títulos de software de terceiros

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Comparar verificações de vulnerabilidade consecutivas

**Orientação**: não aplicável; A Microsoft executa o gerenciamento de vulnerabilidades nos sistemas subjacentes que dão suporte ao observador de rede do Azure.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Usar um processo de avaliação de risco para priorizar a correção das vulnerabilidades descobertas

**Orientação**: não aplicável; A Microsoft executa o gerenciamento de vulnerabilidades nos sistemas subjacentes que dão suporte ao observador de rede do Azure.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

## <a name="inventory-and-asset-management"></a>Inventário e gerenciamento de ativos

*Para saber mais, confira [Controle de segurança: Inventário e gerenciamento de ativos](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: usar solução de descoberta de ativos automatizada

**Diretriz**: Use o Azure Resource Graph para consultar/descobrir todos os recursos (como computação, armazenamento, rede, portas, protocolos etc.) em suas assinaturas. Configure permissões apropriadas (leitura) no seu locatário e enumere todas as assinaturas do Azure, bem como os recursos em suas assinaturas.

Embora os recursos clássicos do Azure possam ser descobertos por meio do grafo de recursos, é altamente recomendável que você crie e use Azure Resource Manager recursos no futuro.

* [Como criar consultas com o Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Como exibir suas assinaturas do Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Entender o RBAC do Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="62-maintain-asset-metadata"></a>6.2: Manter metadados de ativo

**Diretriz**: Aplique marcas aos recursos do Azure, fornecendo metadados para organizá-los logicamente em uma taxonomia.

* [Como criar e usar marcas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Excluir recursos do Azure não autorizados

**Diretriz**: Use marcação, grupos de gerenciamento e assinaturas separadas, sempre que apropriado, para organizar e rastrear recursos do Azure. Reconcilie o inventário regularmente e garanta que os recursos não autorizados sejam excluídos da assinatura em tempo hábil.

Além disso, use Azure Policy para colocar restrições no tipo de recursos que podem ser criados em assinaturas do cliente usando as seguintes definições de política interna:
- Tipos de recursos não permitidos
- Tipos de recursos permitidos

* [Como criar assinaturas adicionais do Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Como criar grupos de gerenciamento](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Como criar e usar marcas](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: definir e manter um inventário de recursos aprovados do Azure

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: monitorar recursos do Azure não aprovados

**Diretrizes**: Use Azure Policy para colocar restrições no tipo de recursos que podem ser criados em suas assinaturas.

Use o grafo de recursos do Azure para consultar e descobrir recursos em suas assinaturas. Verifique se todos os recursos do Azure presentes no ambiente foram aprovados.

* [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Como criar consultas com o Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: monitorar aplicativos de software não aprovados nos recursos de computação

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Remover recursos e aplicativos de software não aprovados do Azure

**Diretriz**: Não aplicável. Esta recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="68-use-only-approved-applications"></a>6.8: Usar somente aplicativos aprovados

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="69-use-only-approved-azure-services"></a>6.9: Usar somente serviços do Azure aprovados

**Diretriz**: use o Azure Policy para colocar restrições nos tipos de recursos que podem ser criados em assinaturas do cliente usando as seguintes definições de política interna:
- Tipos de recursos não permitidos
- Tipos de recursos permitidos

* [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Como negar um tipo de recurso específico com o Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: manter um inventário de títulos de software aprovados

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: limitar a capacidade dos usuários de interagir com Azure Resource Manager

**Diretriz**: Configure o acesso condicional do Azure para limitar a capacidade dos usuários de interagir com o Azure Resource Manager configurando "Bloquear acesso" para o aplicativo de “Gerenciamento do Microsoft Azure”.

* [Como configurar o acesso condicional para bloquear o acesso ao Azure Resource Manager](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: limitar a capacidade dos usuários de executar scripts nos recursos de computação

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Separar física ou logicamente os aplicativos de alto risco

**Diretriz**: Não aplicável; essa recomendação destina-se a aplicativos Web em execução no Serviço de Aplicativo do Azure ou recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

## <a name="secure-configuration"></a>Configuração segura

*Para saber mais, confira [Controle de segurança: Configuração segura](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Estabelecer configurações seguras para todos os recursos do Azure

**Orientação**: definir e implementar configurações de segurança padrão para o observador de rede do Azure com o Azure Policy. Use Azure Policy aliases no namespace "Microsoft. Network" para criar políticas personalizadas para auditar ou impor a configuração de rede de suas instâncias do observador de rede. Você também pode fazer uso de definições de política internas, como:

* [Implantar o observador de rede quando redes virtuais são criadas](https://github.com/Azure/azure-policy/blob/master/samples/built-in-policy/deploy-network-watcher-in-vnet-regions/README.md)

* [Consulte também: como configurar e gerenciar Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Como criar uma política personalizada com aliases de política](https://docs.microsoft.com/azure/governance/policy/tutorials/create-custom-policy-definition)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: estabelecer configurações seguras de sistema operacional

**Diretriz**: não aplicável; essa diretriz destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Manter configurações seguras de recursos do Azure

**Diretriz**: use o Azure Policy [negar] e [implantar se não existir] para impor configurações seguras em seus recursos do Azure.

* [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Compreender os efeitos do Azure Policy](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: manter configurações seguras de sistema operacional

**Diretriz**: não aplicável; essa diretriz destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Armazenar configuração de recursos do Azure com segurança

**Orientação**: se estiver usando definições de Azure Policy personalizadas, use o Azure DevOps ou Azure Repos para armazenar e gerenciar seu código com segurança.

* [Como armazenar código no Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Documentação do Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: armazenar imagens personalizadas do sistema operacional com segurança

**Diretriz**: não aplicável; essa diretriz destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: implantar as ferramentas de gerenciamento de configuração para recursos do Azure

**Orientação**: definir e implementar configurações de segurança padrão para o observador de rede do Azure com o Azure Policy. Use Azure Policy aliases no namespace "Microsoft. Network" para criar políticas personalizadas para auditar ou impor a configuração de rede de suas instâncias do observador de rede. Você também pode fazer uso de definições de política internas, como:

* [Implantar o observador de rede quando redes virtuais são criadas](https://github.com/Azure/azure-policy/blob/master/samples/built-in-policy/deploy-network-watcher-in-vnet-regions/README.md)

Veja também:

* [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Como criar uma política personalizada com aliases de política](https://docs.microsoft.com/azure/governance/policy/tutorials/create-custom-policy-definition)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: implantar as ferramentas de gerenciamento de configuração para sistemas operacionais

**Diretriz**: não aplicável; essa diretriz destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: implementar o monitoramento automatizado de configuração para recursos do Azure

**Orientação**: Use definições de Azure Policy internas, bem como aliases de Azure Policy no namespace "Microsoft. Network" para criar definições de Azure Policy personalizadas para alertar, auditar e impor configurações do sistema. Use Azure Policy [auditoria], [negar] e [implantar se não existir] para impor automaticamente as configurações para os recursos do Azure.

* [Como configurar e gerenciar o Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: implementar monitoramento automatizado de configuração para sistemas operacionais

**Diretriz**: não aplicável; essa diretriz destina-se a recursos de computação.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="711-manage-azure-secrets-securely"></a>7.11: Gerenciar segredos do Azure com segurança

**Orientação**: não aplicável; Não há senhas, segredos ou chaves associadas ao observador de rede do Azure.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: gerenciar identidades de maneira segura e automática

**Orientação**: não aplicável; O observador de rede do Azure não faz uso de identidades gerenciadas.

* [Serviços do Azure que dão suporte a identidades gerenciadas](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: eliminar a exposição involuntária de credenciais

**Diretriz**: implemente o verificador de credenciais para identificar credenciais no código. O verificador de credenciais também encorajará a migração de credenciais descobertas para locais mais seguros, como o Azure Key Vault.

* [Como configurar o verificador de credenciais](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="malware-defense"></a>Defesa contra malware

*Para saber mais, confira [Controle de segurança: Defesa contra malware](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Usar software antimalware gerenciado centralmente

**Diretriz**: não aplicável; essa diretriz destina-se a recursos de computação. O antimalware da Microsoft está habilitado no host subjacente que dá suporte aos serviços do Azure (por exemplo, Azure App Service), no entanto, ele não é executado no conteúdo do cliente.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: arquivos de pré-verificação a serem carregados para recursos não computados do Azure

**Diretrizes**: Não aplicável. O observador de rede não funciona em dados carregados pelo usuário.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Garantir que o software antimalware e as assinaturas sejam atualizados

**Diretriz**: não aplicável; essa recomendação destina-se a recursos de computação. O antimalware da Microsoft está habilitado no host subjacente que dá suporte aos serviços do Azure, no entanto, ele não é executado no conteúdo do cliente.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

## <a name="data-recovery"></a>Recuperação de dados

*Para saber mais, confira [Controle de segurança: Recuperação de dados](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Garantir backups automatizados regulares

**Orientação**: não aplicável; O observador de rede do Azure não mantém nenhum dado do cliente.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: executar backups completos do sistema e fazer backup de qualquer chave gerenciada pelo cliente

**Orientação**: não aplicável; O observador de rede do Azure não mantém nenhum dado do cliente.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: validar todos os backups, incluindo chaves gerenciadas pelo cliente

**Orientação**: não aplicável; O observador de rede do Azure não mantém nenhum dado do cliente.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: garantir a proteção de backups e chaves gerenciadas pelo cliente

**Orientação**: não aplicável; O observador de rede do Azure não mantém nenhum dado do cliente.

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Não aplicável

## <a name="incident-response"></a>Resposta a incidentes

*Para saber mais, confira [Controle de segurança: Resposta a incidentes](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10.1: Criar um guia de resposta a incidentes

**Diretriz**: crie um guia de resposta a incidentes para sua organização. Verifique se há planos de resposta a incidentes escritos que definem todas as funções de pessoal, bem como as fases de tratamento/gerenciamento de incidentes, desde a detecção até a revisão após o incidente.

* [Como configurar a automação do fluxo de trabalho na central de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Orientação sobre como criar seu processo de resposta a incidentes de segurança](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Anatomia de um incidente do Microsoft Security Response Center](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [O cliente também pode aproveitar o guia de tratamento de incidentes de segurança do computador da NIST para ajudar na criação de seu próprio plano de resposta a incidentes](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: criar um procedimento de pontuação e priorização de incidentes

**Diretriz**: a Central de Segurança atribui uma severidade a cada alerta para ajudar você a priorizar quais alertas devem ser investigados primeiro. A severidade se baseia na confiança que a Central de Segurança tem na constatação ou na análise usada para emitir o alerta, bem como no nível de confiança de que houve uma ação mal-intencionada por trás da atividade que levou ao alerta.

Além disso, marque claramente as assinaturas (por exemplo, produção, não produção) e crie um sistema de nomenclatura para identificar e categorizar claramente os recursos do Azure.

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="103-test-security-response-procedures"></a>10.3: testar procedimentos de resposta de segurança

**Diretriz**: conduza exercícios para testar os recursos de resposta a incidentes de seus sistemas em uma cadência regular. Identifique pontos fracos e lacunas e revise o plano conforme necessário.

* [Veja a publicação do NIST: Guia para testar, treinar e exercitar programas para planos de TI e recursos](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: fornecer detalhes de contato do incidente de segurança e configurar notificações de alerta para incidentes de segurança

**Diretriz**: as informações de contato do incidente serão usadas pela Microsoft para contatá-lo se o MSRC (Microsoft Security Response Center) descobrir que os dados do cliente foram acessados por uma pessoa não autorizada ou ilegal. Examine os incidentes após o fato para garantir que os problemas sejam resolvidos.

* [Como definir o contato de segurança da Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Monitoramento da Central de Segurança do Azure**: Sim

**Responsabilidade**: Cliente

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: incorporar alertas de segurança em seu sistema de resposta a incidentes

**Diretriz**: exporte os alertas e recomendações da Central de Segurança do Azure usando o recurso de exportação contínua. A exportação contínua permite exportar alertas e recomendações de forma manual ou contínua. Você pode usar o conector de dados da Central de Segurança do Azure para transmitir os alertas do Sentinel.

* [Como configurar a exportação contínua](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Como transmitir alertas para o Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: automatizar a resposta a alertas de segurança

**Diretrizes**: Use o recurso de automação de fluxo de trabalho na Central de Segurança do Azure para disparar automaticamente respostas por meio de "Aplicativos Lógicos" em alertas de segurança e recomendações.

* [Como configurar a automação de fluxo de trabalho e os Aplicativos Lógicos](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Cliente

## <a name="penetration-tests-and-red-team-exercises"></a>Testes de penetração e exercícios de Red Team

*Para saber mais, confira [Controle de segurança: Testes de penetração e exercícios de Red Team](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: realize testes de penetração regulares de seus recursos do Azure e garanta a correção de todas as descobertas de segurança críticas

**Diretrizes**: * [Siga as regras de envolvimento da Microsoft para garantir que os testes de penetração não violem as políticas da Microsoft](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Você pode encontrar mais informações sobre a estratégia da Microsoft e a execução de Red Team e testes de penetração de sites ao vivo em infraestrutura, serviços e aplicativos de nuvem gerenciados pela Microsoft, aqui](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Monitoramento da Central de Segurança do Azure**: Não aplicável

**Responsabilidade**: Compartilhado

## <a name="next-steps"></a>Próximas etapas

- Confira o [Azure Security Benchmark](https://docs.microsoft.com/azure/security/benchmarks/overview)
- Saiba mais sobre a [Linhas de base de segurança do Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
