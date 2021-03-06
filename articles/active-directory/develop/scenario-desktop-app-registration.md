---
title: Registrar aplicativos da área de trabalho que chamam APIs da Web-plataforma Microsoft Identity | Azure
description: Saiba como criar um aplicativo de área de trabalho que chama APIs da Web (registro de aplicativo)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6796ac42a10d3b976b23f5af1418b1789011d61b
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89440942"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>Aplicativo de área de trabalho que chama APIs da Web: registro de aplicativo

Este artigo aborda as especificações de registro do aplicativo para um aplicativo de área de trabalho.

## <a name="supported-account-types"></a>Tipos de conta compatíveis

Os tipos de conta com suporte em um aplicativo de área de trabalho dependem da experiência que você deseja acender. Devido a essa relação, os tipos de conta com suporte dependem dos fluxos que você deseja usar.

### <a name="audience-for-interactive-token-acquisition"></a>Público-alvo para aquisição de token interativo

Se seu aplicativo de área de trabalho usa a autenticação interativa, você pode conectar usuários de qualquer [tipo de conta](quickstart-register-app.md).

### <a name="audience-for-desktop-app-silent-flows"></a>Público-alvo dos fluxos silenciosos do aplicativo para desktop

- Para usar a autenticação integrada do Windows ou um nome de usuário e uma senha, seu aplicativo precisa conectar usuários em seu próprio locatário, por exemplo, se você for um desenvolvedor de LOB (linha de negócios). Ou, em Azure Active Directory organizações, seu aplicativo precisará conectar usuários em seu próprio locatário se for um cenário de ISV. Esses fluxos de autenticação não têm suporte para contas pessoais da Microsoft.
- Se você quiser usar o fluxo de código do dispositivo, não poderá conectar usuários com suas contas pessoais da Microsoft ainda.
- Se você conectar usuários com identidades sociais que passam uma política e uma autoridade entre empresas (B2C), você só poderá usar a autenticação interativa e de nome de usuário-senha.

## <a name="redirect-uris"></a>URIs de redirecionamento

Os URIs de redirecionamento a serem usados em um aplicativo de área de trabalho dependem do fluxo que você deseja usar.

- Se você usar a autenticação interativa ou o fluxo de código do dispositivo, use `https://login.microsoftonline.com/common/oauth2/nativeclient` . Para obter essa configuração, selecione a URL correspondente na seção de **autenticação** do seu aplicativo.

  > [!IMPORTANT]
  > Hoje, o MSAL.NET usa outro URI de redirecionamento por padrão em aplicativos de área de trabalho que são executados no Windows ( `urn:ietf:wg:oauth:2.0:oob` ). No futuro, queremos alterar esse padrão, portanto, recomendamos que você use `https://login.microsoftonline.com/common/oauth2/nativeclient` .

- Se você criar um aplicativo de Objective-C ou Swift nativo para macOS, registre o URI de redirecionamento com base no identificador de pacote do seu aplicativo no seguinte formato: msauth. <your.app.bundle.id>://auth. Substitua <your.app.bundle.id> pelo identificador de pacote do seu aplicativo.
- Se seu aplicativo usar apenas a autenticação integrada do Windows ou um nome de usuário e uma senha, você não precisará registrar um URI de redirecionamento para seu aplicativo. Esses fluxos fazem uma viagem de ida e volta para o ponto de extremidade v 2.0 da plataforma de identidade da Microsoft. Seu aplicativo não será chamado de volta em nenhum URI específico.
- Para distinguir o fluxo de código do dispositivo, a autenticação integrada do Windows e um nome de usuário e uma senha de um fluxo de aplicativo cliente confidencial que não tem URIs de redirecionamento (o fluxo de credencial do cliente usado em aplicativos de daemon), você precisa expressar que seu aplicativo é um aplicativo cliente público. Para obter essa configuração, vá para a seção de **autenticação** do seu aplicativo. Na subseção **Configurações avançadas** , no tipo de **cliente padrão** parágrafo, selecione **Sim** para **tratar aplicativo como um cliente público**.

  ![Permitir cliente público](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>Permissões de API

Aplicativos de desktop chamam APIs para o usuário conectado. Eles precisam solicitar permissões delegadas. Eles não podem solicitar permissões de aplicativo, que são manipuladas apenas em [aplicativos de daemon](scenario-daemon-overview.md).

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Aplicativo de área de trabalho: configuração de aplicativo](scenario-desktop-app-configuration.md)
