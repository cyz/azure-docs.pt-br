---
title: "Tutorial: integração do Azure Active Directory com o ThirdLight | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o ThirdLight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: ee7710cfea3a13907c0cc940a98c875bf83607a9
ms.contentlocale: pt-br
ms.lasthandoff: 06/20/2017


---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Tutorial: integração do Azure Active Directory com o ThirdLight

Neste tutorial, você aprenderá como integrar o ThirdLight com o Azure AD (Azure Active Directory).

A integração do ThirdLight com o Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao ThirdLight
- Você pode habilitar os usuários a entrarem automaticamente no ThirdLight (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o ThirdLight, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do ThirdLight

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o ThirdLight da Galeria
2. Configurar e testar o logon único do AD do Azure

## <a name="adding-thirdlight-from-the-gallery"></a>Adicionando o ThirdLight da Galeria
Para configurar a integração do ThirdLight ao Azure AD, você precisará adicionar o ThirdLight da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o ThirdLight da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![Aplicativos][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![Aplicativos][3]

4. Na caixa de pesquisa, digite **ThirdLight**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_search.png)

5. No painel de resultados, selecione **ThirdLight** e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o ThirdLight, com base em uma usuária de teste chamada "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do ThirdLight é equivalente a um usuário no Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no ThirdLight.

Essa relação de vínculo é estabelecida por meio da atribuição do valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** no ThirdLight.

Para configurar e testar o logon único do Azure AD com o ThirdLight, você precisará concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criação de um usuário de teste do ThirdLight](#creating-a-thirdlight-test-user)** – para ter um equivalente de Brenda Fernandes no ThirdLight que seja vinculado à representação de usuário do Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** : para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no Portal do Azure e configura o logon único em seu aplicativo ThirdLight.

**Para configurar o logon único do Azure AD com o ThirdLight, realize as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **ThirdLight**, clique em **Logon único**.

    ![Configurar Logon Único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar Logon Único](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_samlbase.png)

3. Na seção **URLs e Domínio do ThirdLight**, realize as seguintes etapas:

    ![Configurar Logon Único](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<subdomain>.thirdlight.com/`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<subdomain>.thirdlight.com/saml/sp`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Contate a [equipe de suporte ao cliente do ThirdLight](https://www.thirdlight.com/support) para obter esses valores. 
 
4. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo XML em seu computador.

    ![Configurar Logon Único](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_certificate.png) 

5. Clique no botão **Salvar** .

    ![Configurar Logon Único](./media/active-directory-saas-thirdlight-tutorial/tutorial_general_400.png)

6. Em outra janela do navegador da Web, faça logon em seu site de empresa ThirdLight como um administrador.

7. Vá para **Configuração \> Administração do Sistema** e clique em **SAML2**.
   
    ![Administração do Sistema](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "Administração do Sistema")

8. Na seção de configuração do SAML2, execute as seguintes etapas:
   
    ![Logon Único do SAML](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "Logon Único do SAML")   

     a. Selecione **Habilitar Logon Único do SAML2**.
 
     b. Como **Fonte de metadados do IdP**, selecione **Carregar Metadados do IdP do XML**.
 
     c. Abra o arquivo de metadados baixado, copie o conteúdo e cole-o na caixa de texto **XML de Metadados do IdP** . 
     
     d. Clique em **Salvar configurações do SAML2**.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-thirdlight-test-user"></a>Criação de um usuário de teste do ThirdLight

Para permitir que os usuários do Azure AD façam logon no ThirdLight, eles devem ser provisionados no ThirdLight.  
No caso do ThirdLight, o provisionamento é uma tarefa manual.

**Para provisionar uma conta de usuário, execute as seguintes etapas:**

1. Faça logon em seu site de empresa do **ThirdLight** como um administrador.

2. Vá para a guia **Usuários** .

3. Selecione **Usuários e Grupos**.

4. Clique no botão **Adicionar novo Usuário** .

5. Digite os valores de **Nome de usuário, Nome ou Descrição, Email, Escolher uma Predefinição ou Grupo de Novos Membros** de uma conta válida do AAD que você deseja provisionar.

6. Clique em **Criar**.

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do Thirdlight ou as APIs fornecidas pelo Thirdlight para provisionar as contas de usuário do AAD. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você habilita a Brenda Fernandes a usar o logon único do Azure através da concessão de acesso ao ThirdLight.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao ThirdLight, realize as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **ThirdLight**.

    ![Configurar Logon Único](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco ThirdLight no Painel de Acesso, você deve ser conectado automaticamente ao aplicativo ThirdLight.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_203.png


