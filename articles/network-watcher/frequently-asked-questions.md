---
title: Perguntas frequentes (FAQ) sobre o observador de rede do Azure | Microsoft Docs
description: Este artigo responde às perguntas frequentes sobre o serviço observador de rede do Azure.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: b48aab918b477f5c689a50ca476b0b1336642f0f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77471849"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Perguntas frequentes (FAQ) sobre o observador de rede do Azure
O serviço [observador de rede do Azure](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) fornece um conjunto de ferramentas para monitorar, diagnosticar, exibir métricas e habilitar ou desabilitar logs de recursos em uma rede virtual do Azure. Este artigo responde a perguntas comuns sobre o serviço.

## <a name="general"></a>Geral

### <a name="what-is-network-watcher"></a>O que é o Observador de Rede?
O observador de rede foi projetado para monitorar e reparar a integridade da rede de componentes IaaS (infraestrutura como serviço), que inclui máquinas virtuais, redes virtuais, gateways de aplicativo, balanceadores de carga e outros recursos em uma rede virtual do Azure. Não é uma solução para monitorar a infraestrutura PaaS (plataforma como serviço) ou obter a análise Web/móvel.

### <a name="what-tools-does-network-watcher-provide"></a>Quais ferramentas o observador de rede fornece?
O observador de rede fornece três conjuntos principais de recursos
* Monitoramento
  * A [exibição de topologia](https://docs.microsoft.com/azure/network-watcher/view-network-topology) mostra os recursos em sua rede virtual e as relações entre eles.
  * O [Monitor de conexão](https://docs.microsoft.com/azure/network-watcher/connection-monitor) permite monitorar a conectividade e a latência entre uma VM e outro recurso de rede.
  * O [Monitor de desempenho de rede](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor) permite que você monitore a conectividade e as latências entre arquiteturas de rede híbridas, circuitos de Expressroute e pontos de extremidade de serviço/aplicativo.  
* Diagnósticos
  * A [verificação de fluxo de IP](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) permite detectar problemas de filtragem de tráfego em um nível de VM.
  * O [próximo salto](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) ajuda a verificar as rotas de tráfego e detectar problemas de roteamento.
  * A [solução de problemas de conexão](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-portal) permite uma verificação única de conectividade e latência entre uma VM e outro recurso de rede.
  * A [captura de pacotes](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) permite capturar todo o tráfego em uma VM em sua rede virtual.
  * A [solução de problemas de VPN](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-overview) executa várias verificações de diagnóstico em seus gateways de VPN e conexões para ajudar a depurar problemas.
* Registro em log
  * Os [logs de fluxo do NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) permitem registrar todo o tráfego em seus [NSGs (grupos de segurança de rede)](https://docs.microsoft.com/azure/virtual-network/security-overview)
  * O [análise de tráfego](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) processa seus dados de log de fluxo do NSG, permitindo que você visualize, consulte, analise e entenda o tráfego de rede.


Para obter informações mais detalhadas, consulte a [página Visão geral do observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).


### <a name="how-does-network-watcher-pricing-work"></a>Como funciona o preço do observador de rede?
Visite a [página de preços](https://azure.microsoft.com/pricing/details/network-watcher/) dos componentes do observador de rede e seus preços.

### <a name="which-regions-is-network-watcher-supportedavailable-in"></a>Em quais regiões o observador de rede tem suporte/está disponível?
Você pode exibir a disponibilidade regional mais recente na [página de disponibilidade do serviço do Azure](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher)

### <a name="which-permissions-are-needed-to-use-network-watcher"></a>Quais permissões são necessárias para usar o observador de rede?
Consulte a lista de [permissões de RBAC necessárias para usar o observador de rede](https://docs.microsoft.com/azure/network-watcher/required-rbac-permissions). Para implantar recursos, você precisa de permissões de colaborador para o NetworkWatcherRG (veja abaixo).

### <a name="how-do-i-enable-network-watcher"></a>Como fazer a habilitação do Observador de Rede?
O serviço do observador de rede é [habilitado automaticamente](https://azure.microsoft.com/updates/azure-network-watcher-will-be-enabled-by-default-for-subscriptions-containing-virtual-networks/) para cada assinatura.

### <a name="what-is-the-network-watcher-deployment-model"></a>O que é o modelo de implantação do observador de rede?
O recurso pai do observador de rede é implantado com uma instância exclusiva em cada região. Formato de nomenclatura: NetworkWatcher_RegionName. Exemplo: NetworkWatcher_centralus é o recurso do observador de rede para a região "EUA Central".

### <a name="what-is-the-networkwatcherrg"></a>O que é o NetworkWatcherRG?
Os recursos do observador de rede estão localizados no grupo de recursos oculto **NetworkWatcherRG** que é criado automaticamente. Por exemplo, o recurso de logs de fluxo NSG é um recurso filho do observador de rede e está habilitado no NetworkWatcherRG.

### <a name="why-do-i-need-to-install-the-network-watcher-extension"></a>Por que preciso instalar a extensão do observador de rede? 
A extensão do observador de rede é necessária para qualquer recurso que precise gerar ou interceptar o tráfego de uma VM. 

### <a name="which-features-require-the-network-watcher-extension"></a>Quais recursos exigem a extensão do observador de rede?
Os recursos de captura de pacote, solução de problemas de conexão e monitor de conexão precisam que a extensão do observador de rede esteja presente.

### <a name="what-are-resource-limits-on-network-watcher"></a>O que são limites de recursos no observador de rede?
Consulte a página [limites de serviço](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#network-watcher-limits) para todos os limites.  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>Por que apenas uma instância do observador de rede é permitida por região? 
O observador de rede precisa ser habilitado apenas uma vez para uma assinatura para que seus recursos funcionem, esse não é um limite de serviço.

### <a name="how-can-i-manage-the-network-watcher-resource"></a>Como posso gerenciar o recurso do observador de rede? 
O recurso do observador de rede representa o serviço de back-end para o observador de rede e é totalmente gerenciado pelo Azure. Os clientes não precisam gerenciá-lo. Não há suporte para operações como mover no recurso. No entanto, [o recurso pode ser excluído](https://docs.microsoft.com/azure/network-watcher/network-watcher-create#delete-a-network-watcher-in-the-portal). 

## <a name="nsg-flow-logs"></a>Logs de fluxo do NSG

### <a name="what-does-nsg-flow-logs-do"></a>O que os logs de fluxo do NSG fazem?
Os recursos de rede do Azure podem ser combinados e gerenciados por meio [de NSGs (grupos de segurança de rede)](https://docs.microsoft.com/azure/virtual-network/security-overview). Os logs de fluxo do NSG permitem que você registre informações de fluxo de 5 tuplas sobre todo o tráfego por meio de seu NSGs. Os logs de fluxo brutos são gravados em uma conta de armazenamento do Azure de onde eles podem ser processados, analisados, consultados ou exportados conforme necessário.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-firewall"></a>Como fazer usar logs de fluxo NSG com uma conta de armazenamento por trás de um firewall?

Para usar uma conta de armazenamento por trás de um firewall, você precisa fornecer uma exceção para que os serviços confiáveis da Microsoft acessem sua conta de armazenamento:

* Navegue até a conta de armazenamento digitando o nome da conta de armazenamento na pesquisa global no portal ou na [página contas de armazenamento](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts)
* Na seção **CONFIGURAÇÕES**, selecione **Firewalls e redes virtuais**
* Em "permitir acesso de", selecione **redes selecionadas**. Em **exceções**, marque a caixa ao lado de **"permitir que os serviços confiáveis da Microsoft acessem esta conta de armazenamento"** 
* Se já estiver selecionada, nenhuma alteração será necessária.  
* Localize o NSG de destino na [página de visão geral dos logs de fluxo do NSG](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) e habilite os logs de fluxo do NSG com a conta de armazenamento acima selecionada.

Você pode verificar os logs de armazenamento após alguns minutos, você deve ver um carimbo de data/hora atualizado ou um novo arquivo JSON criado.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-service-endpoint"></a>Como fazer usar logs de fluxo NSG com uma conta de armazenamento por trás de um ponto de extremidade de serviço?

Os logs de fluxo NSG são compatíveis com pontos de extremidade de serviço sem a necessidade de nenhuma configuração extra. Consulte o [tutorial sobre como habilitar pontos de extremidade de serviço](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources#enable-a-service-endpoint) em sua rede virtual.


### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>Qual é a diferença entre as versões 1 & 2 dos logs de fluxo?
Os logs de fluxo versão 2 apresentam o conceito de *estado de fluxo* & armazena informações sobre bytes e pacotes transmitidos. [Leia mais](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview#log-file).

## <a name="next-steps"></a>Próximas etapas
 - Acesse nossa [página de visão geral da documentação](https://docs.microsoft.com/azure/network-watcher/) para ver alguns tutoriais para ajudá-lo a começar a usar o observador de rede.
