---
title: 'Tutorial: Integração do Active Directory do Azure com o Vibe HCM | Microsoft Docs'
description: Saiba como configurar o logon único entre o Microsoft Azure Active Directory e o Vibe HCM.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: 8908b411155cbe3d4b2f3677d2305b2872574014
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88531966"
---
# <a name="tutorial-azure-active-directory-integration-with-vibe-hcm"></a>Tutorial: Integração do Microsoft Azure Active Directory com o Vibe HCM

Neste tutorial, você aprenderá a integrar o Vibe HCM ao Microsoft Azure Active Directory.
A integração do Vibe HCM ao Azure Active Directory oferece os seguintes benefícios:

* Você pode controlar no Azure Active Directory quem terá acesso ao Vibe HCM.
* Você pode permitir que os usuários sejam conectados automaticamente ao Vibe HCM (Logon Único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Prerequisites

Para configurar a integração do Azure Active Directory com o Vibe HCM, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Assinatura habilitada para logon único no Vibe HCM

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Vibe HCM dá suporte a SSO iniciado por **SP** e **IDP**

## <a name="adding-vibe-hcm-from-the-gallery"></a>Adicionar o Vibe HCM a partir da galeria

Para configurar a integração do Vibe HCM ao Azure Active Directory, você precisará adicionar o Vibe HCM da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o do Vibe HCM da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite  **Vibe HCM**, selecione  **Vibe HCM** no painel de resultados e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Vibe HCM na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Vibe HCM, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do e Vibe HCM.

Para configurar e testar o logon único do Azure Active Directory com o Vibe HCM, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o Logon Único do Vibe HCM](#configure-vibe-hcm-single-sign-on)** – para definir as configurações de Logon Único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar usuário de teste do Vibe HCM](#create-vibe-hcm-test-user)** – para ter um equivalente de Brenda Fernandes no Vibe HCM vinculado à representação do usuário no Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Vibe HCM, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração do aplicativo **Vibe HCM**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração Básica de SAML**, se você desejar configurar o aplicativo no modo iniciado pelo **IDP**, o usuário não precisará executar nenhuma etapa, pois o aplicativo já estará pré-integrado ao Azure.

    ![Informações de logon único de Domínio e URLs do HCM](common/preintegrated.png)

5. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    ![Informações de logon único de Domínio e URLs do HCM](common/metadata-upload-additional-signon.png)

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://<companyName>.vibehcm.com/portal.jsp`

    > [!NOTE]
    > O valor não é real. Atualize o valor com a URL de Logon real. Contate a [equipe de suporte do Cliente do Vibe HCM](mailto:support@vibehcm.com) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

6. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique no botão copiar para copiar **URL de metadados de federação de aplicativos** e salve-a no computador.

    ![O link de download do Certificado](common/copy-metadataurl.png)

### <a name="configure-vibe-hcm-single-sign-on"></a>Configurar o Logon Único do Vibe HCM

Para configurar o logon único no lado do **Vibe HCM**, é necessário enviar a **URL de metadados de federação do aplicativo** para a [equipe de suporte do Vibe HCM](mailto:support@vibehcm.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD 

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite brittasimon@yourcompanydomain.extension. Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Vibe HCM.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Vibe HCM**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Vibe HCM**.

    ![O link do Vibe HCM na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-vibe-hcm-test-user"></a>Criar usuário de teste de Vibe HCM

Nesta seção, você criará uma usuária chamado Brenda Fernandes no Vibe HCM. Trabalhar com a [equipe de suporte de Vibe HCM ](mailto:support@vibehcm.com) para adicionar os usuários à plataforma de Vibe HCM. Os usuários devem ser criados e ativados antes de usar o logon único.

### <a name="test-single-sign-on"></a>Testar logon único 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Vibe HCM no Painel de Acesso, você deverá ser conectado automaticamente ao Vibe HCM no qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

