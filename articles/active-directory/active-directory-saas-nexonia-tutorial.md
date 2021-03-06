---
title: "Tutorial: Integração do Azure Active Directory ao Nexonia | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o Nexonia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a93b771a-9bc3-444a-bdc0-457f8bb7e780
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/1/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 1370fa64c2ddc25d3121c567ceea4828b1e50921
ms.contentlocale: pt-br
ms.lasthandoff: 05/02/2017


---
# <a name="tutorial-azure-active-directory-integration-with-nexonia"></a>Tutorial: Integração do Azure Active Directory ao Nexonia

Neste tutorial, você aprenderá a integrar o Nexonia ao Azure AD (Azure Active Directory).

A integração do Nexonia ao Azure AD oferece os seguintes benefícios:

- No Azure AD, você pode controlar quem tem acesso ao Nexonia
- Você pode permitir que seus usuários faça logon automaticamente no Nexonia (logon único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Se você quiser saber mais detalhes sobre a integração de aplicativos SaaS com o Azure AD, consulte. [O que é o acesso a aplicativos e logon único com o Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Nexonia, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Nexonia habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Nexonia da galeria
2. Configurar e testar o logon único do AD do Azure

## <a name="adding-nexonia-from-the-gallery"></a>Adicionar Nexonia da galeria
Para configurar a integração do Nexonia ao Azure AD, você precisará adicionar o Nexonia da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Nexonia da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![Aplicativos][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![Aplicativos][3]

4. Na caixa de pesquisa, digite **Nexonia**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_search.png)

5. No painel de resultados, selecione **Nexonia** e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o Nexonia, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Nexonia é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no Nexonia.

No Nexonia, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Nexonia, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criar um usuário de teste do Nexonia](#creating-a-nexonia-test-user)** – para ter um equivalente de Brenda Fernandes no Nexonia que esteja vinculado à representação do usuário no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** : para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único em seu aplicativo Nexonia.

>[!Note]
>Se você tiver problemas na integração, consulte este [link](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) para um guia de solução de problemas. Se você ainda não encontrou a solução, acione a solicitação de suporte do Portal do Azure.

**Para configurar o logon único do Azure AD com o Nexonia, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **Nexonia**, clique em **Logon único**.

    ![Configurar Logon Único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar Logon Único](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_samlbase.png)

3. Na seção **URLs e Domínio do Nexonia**, execute as seguintes etapas:

    ![Configurar Logon Único](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_url.png)

    Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`

    > [!NOTE] 
    > Esse valor não é real. Atualize esse valor com a URL de Resposta real. Para obter esse valor, entre em contato com a [equipe de suporte do Nexonia](https://nexonia.zendesk.com/hc/requests/new). 


4. Na seção **Certificado de Autenticação SAML**, clique em **Certificado (Base64)** e, em seguida, salve o arquivo do certificado em seu computador.

    ![Configurar Logon Único](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_certificate.png) 

5. Clique no botão **Salvar** .

    ![Configurar Logon Único](./media/active-directory-saas-nexonia-tutorial/tutorial_general_400.png)

6. Na seção **Configuração do Nexonia**, clique em **Configurar o Nexonia** para abrir a janela **Configurar logon**. Copie a **URL de saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção de Referência Rápida.**

    ![Configurar Logon Único](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_configure.png) 

7. Para que o SSO seja configurado para seu aplicativo, entre em contato com a [equipe de suporte do Nexonia](https://nexonia.zendesk.com/hc/requests/new) e forneça o seguinte:

    • O **certificado**

    • A **ID da entidade SAML**

    • A **URL do Serviço de Logon Único SAML**

    • A **URL de Saída**

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 


### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-nexonia-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-nexonia-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-nexonia-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-nexonia-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-nexonia-test-user"></a>Criar um usuário de teste do Nexonia

Nesta seção, você criará um usuário chamado Brenda Fernandes no Nexonia. Trabalhe com a [equipe de suporte do Nexonia](https://nexonia.zendesk.com/hc/requests/new) para adicionar os usuários na plataforma do Nexonia. Os usuários devem ser criados e ativados antes de usar o logon único.


### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, ao conceder acesso ao Nexonia a Brenda Fernandes, você permitirá que ela use o logon único do Azure.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Nexonia, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Nexonia**.

    ![Configurar Logon Único](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Nexonia no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo Nexonia.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_203.png


