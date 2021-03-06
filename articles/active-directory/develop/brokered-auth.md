---
title: Autenticação orientada no Android | Azure
titlesuffix: Microsoft identity platform
description: Uma visão geral da autenticação orientada & autorização para Android na plataforma de identidade da Microsoft
services: active-directory
author: shoatman
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/25/2020
ms.author: shoatman
ms.custom: aaddev
ms.reviewer: shoatman, hahamil, brianmel
ms.openlocfilehash: 9042318d29b9a7fc8c2064bdf845d6f0d5a4f3e8
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88853866"
---
# <a name="brokered-authentication-in-android"></a>Autenticação orientada no Android

Você deve usar um dos agentes de autenticação da Microsoft para participar do SSO (logon único) em todo o dispositivo e para atender às políticas de acesso condicional organizacional. A integração com um agente oferece os seguintes benefícios:

- Logon único do dispositivo
- Acesso condicional para:
  - Proteção de Aplicativo do Intune
  - Registro de dispositivo (Workplace Join)
  - Gerenciamento do Dispositivo Móvel
- Gerenciamento de conta em todo o dispositivo
  -  por meio do Android Contamanager & configurações de conta
  - "Conta de trabalho"-tipo de conta personalizada

No Android, o Microsoft Authentication Broker é um componente que está incluído com [Microsoft Authenticator aplicativo](https://play.google.com/store/apps/details?id=com.azure.authenticator) e [portal da empresa do Intune](https://play.google.com/store/apps/details?id=com.microsoft.windowsintune.companyportal)

> [!TIP]
> Somente um aplicativo que hospeda o agente estará ativo como o agente de cada vez. Qual aplicativo está ativo como um agente é determinado pela ordem de instalação no dispositivo. O primeiro a ser instalado, ou o último presente no dispositivo, torna-se o agente ativo.

O diagrama a seguir ilustra a relação entre seu aplicativo, a MSAL (biblioteca de autenticação da Microsoft) e os agentes de autenticação da Microsoft.

![Diagrama de implantação do agente](./media/brokered-auth/brokered-deployment-diagram.png)

## <a name="installing-apps-that-host-a-broker"></a>Instalando aplicativos que hospedam um agente

Os aplicativos de Hospedagem de agente podem ser instalados pelo proprietário do dispositivo de sua loja de aplicativos (normalmente Google Play Store) a qualquer momento. No entanto, algumas APIs (recursos) são protegidas por políticas de acesso condicional que exigem que os dispositivos sejam:

- Registrado (ingressado no local de trabalho) e/ou
- Registrado no gerenciamento de dispositivos ou
- Registrado no Proteção de Aplicativo do Intune

Se um dispositivo ainda não tiver um aplicativo de agente instalado, o MSAL instruirá o usuário a instalar um assim que o aplicativo tentar obter um token interativamente. O aplicativo precisará conduzir o usuário pelas etapas para tornar o dispositivo compatível com a política necessária.

## <a name="effects-of-installing-and-uninstalling-a-broker"></a>Efeitos da instalação e desinstalação de um agente

### <a name="when-a-broker-is-installed"></a>Quando um agente é instalado

Quando um agente é instalado em um dispositivo, todas as solicitações de token interativo subsequentes (chamadas para `acquireToken()` ) são tratadas pelo agente em vez de localmente por MSAL. Qualquer estado de SSO disponível anteriormente para MSAL não está disponível para o agente. Como resultado, o usuário precisará autenticar novamente ou selecionar uma conta da lista de contas existente conhecida pelo dispositivo.

A instalação de um agente não exige que o usuário entre novamente. Somente quando o usuário precisar resolver um `MsalUiRequiredException` , a próxima solicitação vai para o agente. `MsalUiRequiredException` é lançada por vários motivos e precisa ser resolvida interativamente. Estes são alguns motivos comuns:

- O usuário alterou a senha associada à sua conta.
- A conta do usuário não atende mais a uma política de acesso condicional.
- O usuário revogou seu consentimento para que o aplicativo seja associado à sua conta.

### <a name="when-a-broker-is-uninstalled"></a>Quando um agente é desinstalado

Se houver apenas um aplicativo de Hospedagem de agente instalado e ele for removido, o usuário precisará entrar novamente. A desinstalação do agente ativo remove a conta e os tokens associados do dispositivo.

Se o Portal da Empresa do Intune estiver instalado e estiver operando como o agente ativo, e Microsoft Authenticator também estiver instalado, se o Portal da Empresa do Intune (Active Broker) for desinstalado, o usuário precisará entrar novamente. Depois que eles entrarem novamente, o aplicativo Microsoft Authenticator se tornará o agente ativo.

## <a name="integrating-with-a-broker"></a>Integração com um agente

### <a name="generating-a-redirect-uri-for-a-broker"></a>Gerando um URI de redirecionamento para um agente

Você deve registrar um URI de redirecionamento que seja compatível com o agente. O URI de redirecionamento para o agente precisa incluir o nome do pacote do aplicativo, bem como a representação codificada em base64 da assinatura do seu aplicativo.

O formato do URI de redirecionamento é: `msauth://<yourpackagename>/<base64urlencodedsignature>`

Gere a assinatura codificada da URL base64 usando as chaves de assinatura do aplicativo. Aqui estão alguns comandos de exemplo que usam suas chaves de assinatura de depuração:

#### <a name="macos"></a>macOS

```bash
keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64
```

#### <a name="windows"></a>Windows

```powershell
keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64
```

Consulte [assinar seu aplicativo](https://developer.android.com/studio/publish/app-signing) para obter informações sobre como assinar seu aplicativo.

> [!IMPORTANT]
> Use sua chave de assinatura de produção para a versão de produção do seu aplicativo.

### <a name="configure-msal-to-use-a-broker"></a>Configurar o MSAL para usar um agente

Para usar um agente em seu aplicativo, você deve atestar que configurou o redirecionamento do agente. Por exemplo, inclua o URI de redirecionamento habilitado para o agente – e indique que você o registrou, incluindo o seguinte no arquivo de configuração do MSAL:

```javascript
"redirect_uri" : "<yourbrokerredirecturi>",
"broker_redirect_uri_registered": true
```

> [!TIP]
> O novo portal do Azure interface do usuário de registro de aplicativo ajuda a gerar o URI de redirecionamento do agente. Se você registrou seu aplicativo usando a experiência mais antiga ou fez isso usando o portal de registro de aplicativo da Microsoft, talvez seja necessário gerar o URI de redirecionamento e atualizar a lista de URIs de redirecionamento no portal manualmente.

### <a name="broker-related-exceptions"></a>Exceções relacionadas ao agente

O MSAL se comunica com o agente de duas maneiras:

- Serviço ligado do agente
- Conta do androidmanager

O MSAL primeiro usa o serviço ligado do agente, pois chamar esse serviço não requer nenhuma permissão do Android. Se a associação ao serviço ligado falhar, o MSAL usará a API de conta do Androidmanager. MSAL faz isso apenas se seu aplicativo já tiver recebido a `"READ_CONTACTS"` permissão.

Se você receber um `MsalClientException` código de erro com `"BROKER_BIND_FAILURE"` , haverá duas opções:

- Peça ao usuário para desabilitar a otimização de energia para o aplicativo Microsoft Authenticator e o Portal da Empresa do Intune.
- Peça ao usuário para conceder a `"READ_CONTACTS"` permissão

## <a name="verifying-broker-integration"></a>Verificando a integração do Broker

Talvez não esteja imediatamente claro que a integração do agente está funcionando, mas você pode usar as seguintes etapas para verificar:

1. Em seu dispositivo Android, conclua uma solicitação usando o agente.
1. Nas configurações em seu dispositivo Android, procure uma conta recém-criada correspondente à conta com a qual você se autenticou. A conta deve ser do tipo *conta de trabalho*.

Você poderá remover a conta das configurações se quiser repetir o teste.
