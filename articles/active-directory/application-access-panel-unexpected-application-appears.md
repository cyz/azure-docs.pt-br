---
title: Como aplicativos aparecem no painel de acesso | Microsoft Docs
description: "Solucionar problemas relacionados a por que um aplicativo está aparecendo no Painel de Acesso"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: 2429ebef69aaddf28d10cd77bf4ce9072ea71476
ms.contentlocale: pt-br
ms.lasthandoff: 04/11/2017

---

<a id="how-applications-appear-on-the-access-panel" class="xliff"></a>

# Como aplicativos aparecem no painel de acesso

O Painel de Acesso é um portal baseado na Web que permite a um usuário com uma conta corporativa ou de estudante no Azure AD (Azure Active Directory) exibir e iniciar aplicativos baseados em nuvem aos quais o administrador do Azure AD concedeu acesso. Esses aplicativos são configurados em nome do usuário no portal do Azure AD. O administrador pode provisionar o aplicativo diretamente para o usuário ou para um grupo de que o usuário faz parte, o que faz com que o aplicativo apareça no Painel de Acesso do usuário.

<a id="general-issues-to-check-first" class="xliff"></a>

## Problemas gerais a serem verificados primeiro

-   Se um aplicativo tiver acabado de ser removido de um usuário ou de um grupo de que o usuário faz parte, tente entrar e sair novamente do Painel de Acesso do usuário após alguns minutos para ver se o aplicativo foi removido.

-   Se uma licença tiver acabado de ser removida de um usuário ou de um grupo de que o usuário faz parte, isso poderá levar um longo período, dependendo do tamanho e da complexidade do grupo. Espere por mais algum tempo antes de entrar no Painel de Acesso.

<a id="problems-related-to-assigning-applications-to-users" class="xliff"></a>

## Problemas relacionados à atribuição de aplicativos para usuários

Um usuário pode estar vendo um aplicativo em seu Painel de Acesso por ter sido atribuído a ele anteriormente. Abaixo, são descritas algumas formas de verificar:

-   [Verificar se um usuário está atribuído ao aplicativo](#check-if-a-user-is-assigned-to-the-application)

-   [Verificar se um usuário está sob uma licença relacionada ao aplicativo](#check-if-a-user-is-under-a-license-related-to-the-application)


<a id="check-if-a-user-is-assigned-to-the-application" class="xliff"></a>

### Verificar se um usuário está atribuído ao aplicativo

Para verificar se um usuário está atribuído ao aplicativo, siga as etapas abaixo:

1.  Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global.**

2.  Abra a **Extensão do Azure Active Directory** clicando em **Mais serviços** na parte inferior do menu de navegação esquerdo principal.

3.  Digite **"Azure Active Directory**" na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4.  Clique em **Aplicativos Empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em **Todos os Aplicativos** para exibir uma lista com todos os seus aplicativos.

6.  **Pesquise** pelo nome do aplicativo em questão.

7.  Clique em **Usuários e grupos**.

8.  Verifique se o usuário está atribuído ao aplicativo.

  * Se quiser remover o usuário do aplicativo, **clique na linha** do usuário e selecione **excluir**.

<a id="check-if-a-user-is-under-a-license-related-to-the-application" class="xliff"></a>

### Verificar se um usuário está sob uma licença relacionada ao aplicativo

Para verificar as licenças atribuídas a um usuário, siga as etapas abaixo:

1.  Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global.**

2.  Abra a **Extensão do Azure Active Directory** clicando em **Mais serviços** na parte inferior do menu de navegação esquerdo principal.

3.  Digite **"Azure Active Directory**" na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4.  Clique em **Usuários e grupos** no menu de navegação.

5.  Clique em **Todos os usuários**.

6.  **Pesquise** pelo usuário no qual você está interessado e **clique na linha** para selecionar.

7.  Clique em **Licenças** para ver quais licenças estão atribuídas ao usuário.

   * Se o usuário estiver atribuído a uma licença do Office, isso permitirá que aplicativos internos do Office apareçam no Painel de Acesso do usuário.

<a id="problems-related-to-assigning-applications-to-groups" class="xliff"></a>

## Problemas relacionados à atribuição de aplicativos a grupos

Um usuário pode estar vendo um aplicativo em seu Painel de Acesso por fazer parte de um grupo ao qual o aplicativo foi atribuído. Abaixo, são descritas algumas formas de verificar:

-   [Verificar as associações de grupo de um usuário](#check-a-users-group-memberships)

-   [Verificar se um usuário é membro de um grupo atribuído a uma licença](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

<a id="check-a-users-group-memberships" class="xliff"></a>

### Verificar as associações de grupo de um usuário

Para verificar as associações de grupo, siga as etapas abaixo:

1.  Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global.**

2.  Abra a **Extensão do Azure Active Directory** clicando em **Mais serviços** na parte inferior do menu de navegação esquerdo principal.

3.  Digite **"Azure Active Directory**" na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4.  Clique em **Usuários e grupos** no menu de navegação.

5.  Clique em **Todos os usuários**.

6.  **Pesquise** pelo usuário no qual você está interessado e **clique na linha** para selecionar.

7.  Clique em **Grupos.**

8.  Verifique se o usuário faz parte de um Grupo atribuído ao aplicativo.

   * Se quiser remover o usuário do grupo, **clique na linha** do grupo e selecione excluir.

<a id="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license" class="xliff"></a>

### Verificar se um usuário é membro de um grupo atribuído a uma licença

1.  Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global.**

2.  Abra a **Extensão do Azure Active Directory** clicando em **Mais serviços** na parte inferior do menu de navegação esquerdo principal.

3.  Digite **"Azure Active Directory**" na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4.  Clique em **Usuários e grupos** no menu de navegação.

5.  Clique em **Todos os usuários**.

6.  **Pesquise** pelo usuário no qual você está interessado e **clique na linha** para selecionar.

7.  Clique em **Grupos.**

8.  Clique na linha de um grupo específico.

9.  Clique em **Licenças** para ver quais licenças o grupo atribuiu a ele.

  * Se o grupo for atribuído a uma licença do Office, isso poderá permitir que determinados aplicativos internos do Office apareçam no Painel de Acesso do usuário.


<a id="if-these-troubleshooting-steps-do-not-the-resolve-the-issue" class="xliff"></a>

## Se essas etapas de solução de problemas não resolverem o problema

Abra um tíquete de suporte com as informações a seguir, se estiverem disponíveis:

-   ID de erro de correlação

-   UPN (endereço de email do usuário)

-   ID do locatário

-   Tipo de navegador

-   Fuso horário e hora/período em que o erro ocorre

-   Rastreamentos do Fiddler

<a id="next-steps" class="xliff"></a>

## Próximas etapas
[Gerenciando aplicativos com o Azure Active Directory](active-directory-enable-sso-scenario.md)

