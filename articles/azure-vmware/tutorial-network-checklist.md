---
title: 'Tutorial: Lista de verificação de rede'
description: Pré-requisitos do Requisito de Rede e detalhes sobre conectividade de rede e portas de rede
ms.topic: tutorial
ms.date: 08/21/2020
ms.openlocfilehash: aba5d7767e420b3ade6238621487884e44fbb6e2
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750414"
---
# <a name="networking-checklist-for-azure-vmware-solution"></a>Lista de verificação de rede da Solução VMware no Azure 

A solução Azure VMware oferece um ambiente de nuvem privada VMware acessível a usuários e aplicativos de ambientes ou recursos locais e baseados no Azure. A conectividade é fornecida por meio de serviços de rede como o Azure ExpressRoute e conexões VPN e exigirá alguns intervalos de endereços de rede e portas de firewall específicos para habilitar os serviços. Veja neste artigo as informações necessárias para configurar corretamente a rede e trabalhar com a Solução VMware no Azure.

Neste tutorial, você aprenderá sobre:

> [!div class="checklist"]
> * Requisitos de conectividade e rede
> * DHCP na Solução VMware no Azure

## <a name="virtual--network-and-expressroute-circuit--considerations"></a>Considerações sobre a rede virtual e o circuito ExpressRoute
Quando você cria uma conexão em uma rede virtual na sua assinatura, o circuito ExpressRoute se estabelece por meio do emparelhamento e usa uma chave de autorização e uma ID de emparelhamento solicitada no portal do Azure. O emparelhamento é uma conexão particular e individual entre a nuvem privada e a rede virtual.

> [!NOTE] 
> O circuito ExpressRoute não faz parte de uma implantação de nuvem privada. O circuito ExpressRoute local está além do escopo deste documento. Para ter conectividade local com a nuvem privada, use um dos circuitos ExpressRoute existentes ou compre um no portal do Azure.

Ao implantar uma nuvem privada, você recebe endereços IP para o vCenter e o NSX-T Manager. Para acessar essas interfaces de gerenciamento, é preciso criar mais recursos em uma rede virtual na sua assinatura. Veja nos tutoriais os procedimentos para criar esses recursos e estabelecer o emparelhamento privado do ExpressRoute.

A rede lógica da nuvem privada vem com um NSX-T pré-provisionado. Um gateway de camada 0 e outro de camada 1 são pré-provisionados para você. É possível criar um segmento e anexá-lo ao gateway de camada 1 existente ou ao novo gateway de camada 1 definido por você. Os componentes da rede lógica NSX-T oferecem conectividade Leste-Oeste entre cargas de trabalho e conectividade Norte-Sul com a Internet e os serviços do Azure.

## <a name="routing-and-subnet-considerations"></a>Considerações sobre o roteamento e a sub-rede
A nuvem privada da AVS é conectada à sua rede virtual do Azure usando uma conexão do Azure ExpressRoute. Essa conexão de baixa latência e alta largura de banda permite que você acesse os serviços em execução na sua assinatura do Azure de seu ambiente de nuvem privada. O roteamento é baseado em BGP (Border Gateway Protocol), provisionado automaticamente e habilitado por padrão para cada implantação de nuvem privada. 

As nuvens privadas da AVS exigem pelo menos um bloco de endereços de rede CIDR `/22` para sub-redes, mostrado abaixo. Essa rede complementa suas redes locais. O bloco de endereço não deve se sobrepor aos blocos de endereços usados em outras redes virtuais que estão em sua assinatura e redes locais. Dentro desse bloco de endereços, o gerenciamento, o provisionamento e as redes vMotion são provisionados automaticamente.

Exemplo de bloco de endereço de rede CIDR `/22`: `10.10.0.0/22`

As sub-redes:

| Uso de rede             | Sub-rede | Exemplo        |
| ------------------------- | ------ | -------------- |
| Gerenciamento de nuvem privada  | `/24`  | `10.10.0.0/24` |
| Rede do vMotion           | `/24`  | `10.10.1.0/24` |
| Cargas de trabalho de VM              | `/24`  | `10.10.2.0/24` |
| Emparelhamento do ExpressRoute      | `/24`  | `10.10.3.8/30` |


### <a name="network-ports-required-to-communicate-with-the-service"></a>Portas de rede necessárias para comunicação com o serviço

| Fonte | Destino | Protocolo | Porta | Descrição  | 
| ------ | ----------- | :------: | :---:| ------------ | 
| Servidor DNS da nuvem privada | Servidor DNS local | UDP | 53 | Solicitações de encaminhamento de cliente DNS do PC vCenter para qualquer consulta de DNS local (verifique a seção DNS abaixo) |  
| Servidor DNS local   | Servidor DNS da nuvem privada | UDP | 53 | Cliente DNS – solicitações de encaminhamento de serviços locais para servidores DNS da nuvem privada (verifique a seção DNS abaixo) |  
| Rede local  | vCenter Server de nuvem privada  | TCP (HTTP)  | 80 | O vCenter Server requer a porta 80 para conexões HTTP diretas. A porta 80 redireciona as solicitações para a porta HTTPS 443. Esse redirecionamento ajuda a usar `http://server` em vez de `https://server`.  <br><br>WS-Management (também requer que a porta 443 esteja aberta) <br><br>Se você usar um banco de dados Microsoft SQL personalizado e não o pacote de banco de dados do SQL Server 2008 no vCenter Server, a porta 80 será usada pelo SQL Reporting Services. Quando você instala o vCenter Server, o instalador solicita que você altere a porta HTTP para o vCenter Server. Altere a porta HTTP do vCenter Server para um valor personalizado para garantir uma instalação bem-sucedida. O IIS (Serviços de Informações da Internet) da Microsoft também usa a porta 80. Confira Conflito entre vCenter Server e o IIS para a porta 80. |  
| Rede de gerenciamento de nuvem privada | Active Directory local  | TCP  | 389 | Essa porta deve estar aberta no local e em todas as instâncias remotas do vCenter Server. Essa porta é o número da porta LDAP para os serviços de diretório para o grupo do vCenter Server. O sistema vCenter Server precisará associar-se à porta 389, mesmo se você não estiver unindo essa instância de vCenter Server a um grupo Linked Mode. Se outro serviço estiver em execução nessa porta, talvez seja preferível removê-lo ou alterar sua porta para uma porta diferente. Você pode executar o serviço LDAP em qualquer porta de 1025 a 65535.  Se essa instância estiver atuando como o Microsoft Windows Active Directory, altere o número da porta de 389 para uma porta disponível de 1025 até 65535. Essa porta é opcional para configurar o AD local como uma fonte de identidade no vCenter da nuvem privada. |  
| Rede local  | vCenter Server de nuvem privada  | TCP(HTTPS)  | 443 | Essa porta permite que você acesse o vCenter por meio da rede local. A porta padrão que o sistema vCenter Server usa para escutar conexões do cliente vSphere. Para permitir que o sistema de vCenter Server receba dados do cliente vSphere, abra a porta 443 no firewall. O sistema de vCenter Server também usa a porta 443 para monitorar a transferência de dados de clientes SDK. Essa porta também é usada para os seguintes serviços: WS-Management (também requer que a porta 80 esteja aberta). Acesso de cliente vSphere ao vSphere Update Manager. Conexões de cliente de gerenciamento de rede de terceiros para vCenter Server. Acesso de clientes de gerenciamento de rede de terceiros a hosts. |  
| Navegador da Web  | Gerenciador de Nuvem Híbrida  | TCP(HTTPS) | 9443 | Interface de gerenciamento de dispositivo virtual do Gerenciador de Nuvem Híbrida para configuração do sistema do Gerenciador de Nuvem Híbrida. |
| Rede de administrador  | Gerenciador de Nuvem Híbrida | SSH | 22 | Acesso SSH do administrador ao Gerenciador de Nuvem Híbrida. |
| HCM | Gateway de nuvem | TCP(HTTPS) | 8123 | Envie instruções de serviço de replicação baseadas em host para o Gateway de Nuvem Híbrida. |
| HCM | Gateway de nuvem | HTTP TCP(HTTPS) | 9443 | Envie instruções de gerenciamento para o Gateway de Nuvem Híbrida local usando a API REST. |
| Gateway de Nuvem | L2C | TCP(HTTPS) | 443 | Envie instruções de gerenciamento do Gateway de Nuvem para L2C quando L2C usar o mesmo caminho que o Gateway de Nuvem Híbrida. |
| Gateway de Nuvem | Hosts ESXi | TCP | 80.902 | Gerenciamento e implantação de OVF. |
| Gateway de Nuvem (local)| Gateway de Nuvem (remoto) | UDP | 4500 | Necessário para IPSEC<br>   Protocolo IKE (IKEv2) para encapsular cargas de trabalho para o túnel bidirecional. Também há suporte para a NAT-T (conversão de endereços de rede – travessia). |
| Gateway de Nuvem (local) | Gateway de Nuvem (remoto)  | UDP | 500 | Necessário para IPSEC<br> Protocolo IKE (ISAKMP) para o túnel bidirecional. |
| Rede vCenter local | Rede de gerenciamento de nuvem privada | TCP | 8000 |  vMotion de VMs do vCenter local para o vCenter de Nuvem Privada   |     

## <a name="dhcp-and-dns-resolution-considerations"></a>Considerações sobre a resolução de DHCP e DNS
Os aplicativos e as cargas de trabalho em execução em um ambiente de nuvem privada exigem a resolução de nomes e os serviços DHCP para pesquisa e atribuição de endereço IP. Uma infraestrutura apropriada de DHCP e DNS é necessária para fornecer esses serviços. Você pode configurar uma máquina virtual para fornecer esses serviços em seu ambiente de nuvem privada.  

É recomendável usar o serviço DHCP interno ao NSX ou usando um servidor DHCP local na nuvem privada em vez de rotear o tráfego DHCP de difusão pela WAN de volta para o local.


## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

> [!div class="checklist"]
> * Requisitos de conectividade e rede
> * DHCP na Solução VMware no Azure

Quando a rede adequada estiver funcionando, siga para o próximo tutorial para aprender a criar uma nuvem privada da Solução VMware no Azure.

> [!div class="nextstepaction"]
> [Tutorial: Criar uma nuvem privada da Solução VMware no Azure](tutorial-create-private-cloud.md)
