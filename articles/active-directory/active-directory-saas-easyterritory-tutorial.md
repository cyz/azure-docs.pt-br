---
title: "Tutorial: Integração do Azure Active Directory ao EasyTerritory | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o EasyTerritory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d29b362d-e986-4f67-8ff2-e158e49353aa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 64131f85cceb463bdd91cec40c2f272ece773691
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-easyterritory"></a>Tutorial: integração do Azure Active Directory ao EasyTerritory

Neste tutorial, você aprenderá como integrar o EasyTerritory ao Azure Active Directory (Azure AD).

A integração do EasyTerritory ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao EasyTerritory
- Você pode permitir que os usuários se conectem automaticamente ao SSO (logon único) do EasyTerritory com suas contas do Azure AD
- Você pode gerenciar suas contas em um único local - o portal de Gerenciamento do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao EasyTerritory, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do EasyTerritory habilitada para SSO (logon único)

>[!NOTE]
>Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.
>
>

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Caso você não tenha um ambiente de avaliação do Azure AD, poderá obter uma avaliação [de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testa o SSO do Azure AD em um ambiente de teste. 

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o EasyTerritory da galeria
2. Configurar e testar o SSO do Azure AD

## <a name="adding-easyterritory-from-the-gallery"></a>Adicionar o EasyTerritory da galeria
Para configurar a integração do EasyTerritory ao Azure AD, você precisa adicionar o EasyTerritory da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o EasyTerritory da galeria, execute as seguintes etapas:**

1. No **[Portal de Gerenciamento do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![Aplicativos][2]
    
3. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicativos][3]

4. Na caixa de pesquisa, digite **EasyTerritory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_001.png)

5. No painel de resultados, selecione **EasyTerritory** e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_0001.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD
Nesta seção, você configura e testa o SSO do Azure AD com o EasyTerritory, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o SSO funcione, o Azure AD precisa saber qual usuário do EasyTerritory é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do EasyTerritory.

Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD ao valor do **Nome de Usuário** no EasyTerritory.

Para configurar e testar o SSO do Azure AD com o EasyTerritory, você precisa concluir os seguintes blocos de construção:

1. **[Configurar logon único do Azure AD](#configuring-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criação de um usuário de teste do EasyTerritory](#creating-an-easyterritory-test-user)** – para ter um equivalente de Britta Simon no EasyTerritory que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** : para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal de Gerenciamento do Azure e configura o SSO no aplicativo EasyTerritory.

**Para configurar o SSO do Azure AD com o EasyTerritory, realize as seguintes etapas:**

1. No Portal de Gerenciamento do Azure, na página de integração de aplicativos do **EasyTerritory**, clique em **Logon único**.

    ![Configurar Logon Único][4]

2. Na caixa de diálogo **Logon único**, como **Modo**, selecione **Logon baseado em SAML** para habilitar o SSO.
 
    ![Configurar Logon Único](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_01.png)

3. Na seção **Domínio e URLs do EasyTerritory**, se você quiser configurar o aplicativo em **Modo iniciado pelo IDP**, execute as seguintes etapas:

    ![Configurar Logon Único](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_02.png)
   1. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://apps.easyterritory.com/<tenant id>/DEV/`
   2. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://apps.easyterritory.com/<tenant id>/DEV/AuthServices/Acs`
    
4. Se você quiser configurar o aplicativo em **Modo iniciado pelo SP**, na seção **Domínio e URLs do EasyTerritory**, execute as seguintes etapas:
    
    ![Configurar Logon Único](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_03.png)
  1. Clique na opção **Mostrar configurações de URL avançadas**.
  2. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<company name>.easyterritory.com/`

    >[!NOTE] 
    >Esses não são os valores reais. Você precisa atualizar esses valores com a URL de Entrada, o Identificador e a URL de Resposta reais. Entre em contato com a [equipe de suporte do EasyTerritory](mailto:sales@easyterritory.com) para obter esses valores.

5. Na seção **Certificado de Autenticação SAML**, clique em **Criar novo certificado**.

    ![Configurar Logon Único](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_04.png)     

6. Sobre o **criar um novo certificado** caixa de diálogo, clique no ícone de calendário e selecione um **data de expiração**. Em seguida, clique no botão **Salvar**.

    ![Configurar o logon único](./media/active-directory-saas-easyterritory-tutorial/tutorial_general_300.png)

7. Sobre o **certificado de autenticação SAML** seção, selecione **ativar o novo certificado** e clique em **salvar** botão.

    ![Configurar Logon Único](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_05.png)

8. No pop-up **certificado de substituição** janela, clique em **OK**.

    ![Configurar o logon único](./media/active-directory-saas-easyterritory-tutorial/tutorial_general_400.png)

9. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar Logon Único](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_06.png) 

10. Para que o SSO seja configurado para o aplicativo, entre em contato com a [equipe de suporte do EasyTerritory](mailto:sales@easyterritory.com) e forneça os **metadados** baixados. 

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD
O objetivo desta seção é criar um usuário de teste no portal de Gerenciamento do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal de Gerenciamento do Azure**, no painel navegação à esquerda, clique em **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_01.png) 

2. Vá para **usuários e grupos** e clique em **todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo clique **adicionar** para abrir o **usuário** caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_04.png) 
 1. Na caixa de texto **Nome**, digite **Brenda Fernandes**.
 2. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.
 3. Selecione **Mostrar senha** e anote o valor de **senha**.
 4. Clique em **Criar**. 

### <a name="create-an-easyterritory-test-user"></a>Criar um usuário de teste do EasyTerritory

Nesta seção, você criará uma usuária chamada Britta Simon no EasyTerritory. Trabalhe com a [equipe de suporte do EasyTerritory](mailto:sales@easyterritory.com) para adicionar os usuários na plataforma do EasyTerritory.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o SSO do Azure concedendo-lhe acesso ao EasyTerritory.

![Atribuir usuário][200] 

**Para atribuir Britta Simon ao EasyTerritory, execute as seguintes etapas:**

1. No Portal de Gerenciamento do Azure, abra a exibição de aplicativos e, em seguida, navegue até o modo de exibição de diretório, vá para **Aplicativos empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir Usuário][201] 

2. Na lista de aplicativos, escolha **EasyTerritory**.

    ![Configurar Logon Único](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_50.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    

### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de SSO do Azure AD usando o Painel de Acesso.

Ao clicar no bloco EasyTerritory no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo EasyTerritory.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_203.png
