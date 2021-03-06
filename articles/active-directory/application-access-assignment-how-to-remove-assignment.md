---
title: "Como remover o acesso de um usuário a um aplicativo | Microsoft Docs"
description: "Compreenda como remover o acesso de um usuário a um aplicativo"
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
ms.openlocfilehash: 8d4f2cec35a8edfec9b8830a077b8aa65ca0c229
ms.contentlocale: pt-br
ms.lasthandoff: 04/11/2017

---

<a id="how-to-remove-a-users-access-to-an-application" class="xliff"></a>

# Como remover o acesso de um usuário a um aplicativo

Este artigo o ajudará a compreender como remover o acesso de um usuário a um aplicativo.

<a id="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application" class="xliff"></a>

## Quero remover a atribuição de um usuário ou grupo específico a um aplicativo

Para remover uma atribuição de um usuário ou grupo a um aplicativo, siga as etapas listadas no artigo [Remover uma atribuição de usuário ou grupo de um aplicativo empresarial no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal).

.## Quero desabilitar todo o acesso a um aplicativo para todos os usuários

Para desabilitar todos os logons de usuário em um aplicativo, siga as etapas listadas no artigo [Desabilitar logons de usuário para um aplicativo empresarial no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal).

<a id="i-want-to-delete-an-application-entirely" class="xliff"></a>

## Quero excluir um aplicativo completamente

Para **excluir um aplicativo**, siga as instruções abaixo:

1.  Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global** ou **Coadministrador.**

2.  Abra a **Extensão do Azure Active Directory** clicando em **Mais serviços** na parte inferior do menu de navegação esquerdo principal.

3.  Digite **"Azure Active Directory**" na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4.  Clique em **Aplicativos Empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em **Todos os Aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se não vir o aplicativo desejado, use o controle **Filtro** na parte superior da **Lista com Todos os Aplicativos** e defina a opção **Mostrar** como **Todos os Aplicativos.**

6.  Selecione o aplicativo que deseja excluir.

7.  Após o aplicativo ser carregado, clique no ícone **Excluir** na folha **Visão Geral** superior do aplicativo.

<a id="i-want-to-disable-all-future-user-consent-operations-to-any-application" class="xliff"></a>

## Quero desabilitar todas as futuras operações de consentimento do usuário para qualquer aplicativo

Desabilitar o consentimento do usuário para todo o seu diretório impede que os usuários finais consintam com qualquer aplicativo. Os administradores ainda podem consentir em nome dos usuários. Para saber mais sobre o consentimento de aplicativos e por que você pode ou não querer fazer isso, leia [Entendendo o consentimento do usuário e do administrador](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

Para **desabilitar todas as futuras operações de consentimento do usuário no diretório inteiro**, siga as instruções abaixo:

1.  Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global.**

2.  Abra a **Extensão do Azure Active Directory** clicando em **Mais serviços** na parte inferior do menu de navegação esquerdo principal.

3.  Digite **"Azure Active Directory**" na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4.  Clique em **Usuários e grupos** no menu de navegação.

5.  Clique em **Configurações de usuário**.

6.  Desabilite todas as futuras operações de consentimento do usuário definindo o controle de alternância **Os usuários podem permitir que os aplicativos acessem seus dados** como **Não** e clique no botão **Salvar**.


<a id="next-steps" class="xliff"></a>

# Próximas etapas
[Gerenciamento do acesso aos aplicativos](active-directory-managing-access-to-apps.md)

