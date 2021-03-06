---
title: Gerenciar o tráfego para aplicativos multilocatários usando o portal
titleSuffix: Azure Application Gateway
description: Este artigo fornece orientação sobre como configurar os aplicativos Web do serviço Azure App como membros no pool de back-end em um gateway de aplicativo novo ou existente.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/09/2020
ms.author: absha
ms.openlocfilehash: dbaad0f6639d65d88da6847886d3aa3d39b93e82
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90563746"
---
# <a name="configure-app-service-with-application-gateway"></a>Configurar Serviço de Aplicativo com Gateway de Aplicativo

Como o serviço de aplicativo é um serviço multilocatário em vez de uma implantação dedicada, ele usa o cabeçalho de host na solicitação de entrada para resolver a solicitação para o ponto de extremidade correto do serviço de aplicativo. Normalmente, o nome DNS do aplicativo que, por sua vez, é o nome DNS associado ao gateway de aplicativo que o serviço de aplicativo, é diferente do nome de domínio do serviço de aplicativo de back-end. Portanto, o cabeçalho de host na solicitação original recebida pelo gateway de aplicativo não é o mesmo que o nome de host do serviço de back-end. Por isso, a menos que o cabeçalho de host na solicitação do gateway de aplicativo para o back-end seja alterado para o nome de host do serviço de back-end, os back-ends de vários locatários não poderão resolver a solicitação para o ponto de extremidade correto.

O gateway de aplicativo fornece uma opção chamada `Pick host name from backend address` que substitui o cabeçalho de host na solicitação pelo nome de host do back-end quando a solicitação é roteada do gateway de aplicativo para o back-end. Esse recurso habilita o suporte para back-ends de vários locatários, como o serviço de aplicativo do Azure e o gerenciamento de API. 

Neste artigo, você aprenderá como:

- Criar um pool de back-end e adicionar um serviço de aplicativo a ele
- Criar configurações de HTTP e investigação personalizada com opções "escolher nome do host" habilitadas

## <a name="prerequisites"></a>Pré-requisitos

- Gateway de aplicativo: se você não tiver um gateway de aplicativo existente, consulte como [criar um gateway de aplicativo](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)
- Serviço de aplicativo: se você não tiver um serviço de aplicativo existente, consulte a [documentação do serviço de aplicativo](https://docs.microsoft.com/azure/app-service/).

## <a name="add-app-service-as-backend-pool"></a>Adicionar serviço de aplicativo como pool de back-end

1. No portal do Azure, abra o modo de exibição de configuração do seu gateway de aplicativo.

2. Em **pools de back-end**, clique em **Adicionar** para criar um novo pool de back-end.

3. Forneça um nome adequado para o pool de back-end. 

4. Em **destinos**, clique na lista suspensa e escolha **serviços de aplicativos** como a opção.

5. Uma lista suspensa logo abaixo da lista suspensa **destinos**  aparecerá, que conterá uma List dos serviços de aplicativo. Nessa lista suspensa, escolha o serviço de aplicativo que você deseja adicionar como um membro do pool de back-end e clique em Adicionar.

   ![Back-end do serviço de aplicativo](./media/configure-web-app-portal/backendpool.png)
   
   > [!NOTE]
   > A lista suspensa preencherá apenas os serviços de aplicativos que estão na mesma assinatura que o seu gateway de aplicativo. Se você quiser usar um serviço de aplicativo que esteja em uma assinatura diferente daquela em que o gateway de aplicativo está, em vez de escolher **serviços de aplicativos** na lista suspensa **destinos** , escolha **endereço IP ou** opção de nome de host e insira o nome do host (exemplo. azurewebsites.net) do serviço de aplicativo.

## <a name="create-http-settings-for-app-service"></a>Criar configurações de HTTP para o serviço de aplicativo

1. Em **configurações de http**, clique em **Adicionar** para criar uma nova configuração de http.

2. Insira um nome para a configuração de HTTP e você pode habilitar ou desabilitar a afinidade baseada em cookie de acordo com seu requisito.

3. Escolha o protocolo como HTTP ou HTTPS de acordo com seu caso de uso. 

   > [!NOTE]
   > Se você selecionar HTTPS, não será necessário carregar nenhum certificado de autenticação ou certificado raiz confiável para permitir o back-end do serviço de aplicativo, pois o serviço de aplicativo é um serviço do Azure confiável.

4. Marque a caixa para **uso do serviço de aplicativo** . Observe que os comutadores  `Create a probe with pick host name from backend address` e `Pick host name from backend address` serão habilitados automaticamente.`Pick host name from backend address` substituirá o cabeçalho de host na solicitação pelo nome do host do back-end quando a solicitação for roteada do gateway de aplicativo para o back-end.  

   `Create a probe with pick host name from backend address` criará automaticamente uma investigação de integridade e a associará a essa configuração de HTTP. Você não precisa criar nenhuma outra investigação de integridade para essa configuração de HTTP. Você pode verificar se uma nova investigação com o nome foi <HTTP Setting name> <Unique GUID> adicionada na lista de investigações de integridade e se ela já tem a opção `Pick host name from backend http settings enabled` .

   Se você já tiver uma ou mais configurações HTTP que estão sendo usadas para o serviço de aplicativo e se essas configurações de HTTP usarem o mesmo protocolo que o que você está usando no que você está criando, em vez da `Create a probe with pick host name from backend address` opção, você obterá uma lista suspensa para selecionar uma das investigações personalizadas. Isso ocorre porque, como já existe uma configuração HTTP com o serviço de aplicativo, por isso, também existe uma investigação de integridade que tem a opção `Pick host name from backend http settings enabled` . Escolha essa investigação personalizada na lista suspensa.

5. Clique em **OK** para criar a configuração de http.

   ![Captura de tela mostra o painel de configuração adicionar H T T P com usar para o serviço de aplicativo e OK selecionado.](./media/configure-web-app-portal/http-setting1.png)

   ![Captura de tela mostra uma investigação de integridade com selecionar nome de host configurações de http de back-end selecionado.](./media/configure-web-app-portal/http-setting2.png)



## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Criar regra para ligar o ouvinte, o pool de back-end e a configuração de HTTP

1. Em **regras**, clique em **básico** para criar uma nova regra básica.

2. Forneça um nome adequado e selecione o ouvinte que irá aceitar as solicitações de entrada para o serviço de aplicativo.

3. Na lista suspensa **pool de back-end** , escolha o pool de back-end que você criou acima.

4. Na lista suspensa **configuração de http** , escolha a configuração http que você criou acima.

5. Clique em **OK** para salvar esta regra.

   ![Captura de tela mostra o painel Adicionar regra básica com o ouvinte, o pool de back-end e a configuração de H t P realçada.](./media/configure-web-app-portal/rule.png)

## <a name="additional-configuration-in-case-of-redirection-to-app-services-relative-path"></a>Configuração adicional no caso de redirecionamento para o caminho relativo do serviço de aplicativo

Quando o serviço de aplicativo envia uma resposta de redirecionamento para o cliente para redirecionar para seu caminho relativo (por exemplo, um redirecionamento de contoso.azurewebsites.net/path1 para contoso.azurewebsites.net/path2), ele usa o mesmo nome de host no cabeçalho de local de sua resposta como aquele na solicitação recebida do gateway de aplicativo. Portanto, o cliente fará a solicitação diretamente para contoso.azurewebsites.net/path2 em vez de passar pelo gateway de aplicativo (contoso.com/path2). Ignorar o gateway de aplicativo não é desejável.

Se, em seu caso de uso, houver cenários em que o serviço de aplicativo precisará enviar uma resposta de redirecionamento para o cliente, execute as [etapas adicionais para regravar o cabeçalho de local](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration).

## <a name="restrict-access"></a>Restringir acesso

Os aplicativos web implantados nesses exemplos usam endereços IP públicos que podem ser acessados diretamente da internet. Isso ajuda com solução de problemas quando você estiver aprendendo sobre um novo recurso e tentar novas coisas. Mas se você pretende implantar um recurso na produção, você desejará adicionar mais restrições.

Uma forma de você restringir o acesso a seus aplicativos web de uma maneira é usar [restrições de IP estático do Serviço de Aplicativo do Azure](../app-service/app-service-ip-restrictions.md). Por exemplo, você pode restringir o aplicativo web para que ele só recebe tráfego de gateway de aplicativo. Use o recurso de restrição de IP de serviço de aplicativo para listar o VIP do gateway de aplicativo como o único endereço com o acesso.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o serviço de aplicativo e outros suporte a vários locatários com o gateway de aplicativo, consulte [suporte a serviços multilocatários com o gateway de aplicativo](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).
