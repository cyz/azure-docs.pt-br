---
title: Como montar um compartilhamento de arquivos do Azure e acessar o compartilhamento no Windows | Microsoft Docs
description: Como montar um compartilhamento de arquivos do Azure e acessar o compartilhamento no Windows.
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: e911e787cd1e29b2bbeaa648869c50245f2dd9ba
ms.contentlocale: pt-br
ms.lasthandoff: 07/22/2017

---

# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>Como montar um compartilhamento de arquivos do Azure e acessar o compartilhamento no Windows
O [Armazenamento de arquivos do Azure](storage-dotnet-how-to-use-files.md) é o sistema de arquivos de nuvem de fácil acesso da Microsoft. Os compartilhamentos de arquivos do Azure podem ser montados no Windows e no Windows Server. Este artigo mostra três maneiras diferentes de montagem de compartilhamento de arquivos do Azure no Windows: com a IU do explorador de arquivos, por meio do PowerShell e por meio do Prompt de comando. 

Para montar um compartilhamento de Arquivos do Azure fora da região na qual o Azure está hospedado, como no local ou em uma região diferente do Azure, o sistema operacional deve dar suporte a SMB 3.0. 

O compartilhamento de Arquivos do Azure pode ser montado no computador Windows do local ou na VM do Azure, dependendo da versão do sistema operacional. A tabela abaixo ilustra 

| Versão do Windows        | Versão do SMB |Montável na VM do Azure|Montável no local|
|------------------------|-------------|---------------------|---------------------|
| Windows 7              | SMB 2.1     | Sim                 | Não                  |
| Windows Server 2008 R2 | SMB 2.1     | Sim                 | Não                  |
| Windows 8              | SMB 3.0     | Sim                 | Sim                 |
| Windows Server 2012    | SMB 3.0     | Sim                 | Sim                 |
| Windows Server 2012 R2 | SMB 3.0     | Sim                 | Sim                 |
| Windows 10             | SMB 3.0     | Sim                 | Sim                 |

> [!Note]  
> É sempre recomendável obter o KB mais recente para a sua versão do Windows.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Pré-requisitos para montar o compartilhamento de arquivos do Azure com o Windows 
* **Nome da conta de armazenamento**: para montar um compartilhamento de arquivos do Azure, você precisará do nome da conta de armazenamento.

* **Chave de conta de armazenamento**: para montar um compartilhamento de arquivos do Azure, você precisará da chave de armazenamento primária (ou secundária). Atualmente, as chaves SAS não têm suporte para montagem.

* **Certifique-se de que a porta 445 esteja aberta**: o armazenamento de arquivos do Azure usa o protocolo SMB. O SMB se comunica pela porta TCP 445: confira se o firewall não está bloqueando as portas TCP 445 do computador cliente.

## <a name="mount-the-azure-file-share-with-file-explorer"></a>Como montar o compartilhamento de arquivos do Azure com o explorador de arquivos
> [!Note]  
> Observe que as instruções a seguir são mostradas no Windows 10 e podem diferir ligeiramente em versões mais antigas. 

1. **Abra o Explorador de arquivos**: você pode fazer isso pelo Menu Iniciar ou pressionando o atalho Win + E.

2. **Navegue até o item "Este Computador" no lado esquerdo da janela. Isso alterará os menus disponíveis na faixa de opções. No menu do computador, selecione "Mapear unidade de rede"**.
    
    ![Uma captura de tela do menu suspenso "Mapear unidade de rede"](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Copie o caminho UNC do painel "Conectar" no portal do Azure**: você encontrará uma descrição detalhada de como localizar essas informações [aqui](storage-file-how-to-use-files-portal.md#connect-to-file-share).

    ![O caminho UNC no painel de conexão de armazenamento de arquivos do Azure](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Selecione a letra da unidade e digite o caminho UNC.** 
    
    ![Uma captura de tela da caixa de diálogo "Mapear unidade de rede"](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Use o nome da conta de armazenamento anexado com `Azure\` como o nome de usuário e uma chave de conta de armazenamento como a senha.**
    
    ![Uma captura de tela da caixa de diálogo de credenciais de rede](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Use o compartilhamento de arquivos do Azure como quiser**.
    
    ![O compartilhamento de arquivos do Azure já está montado](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Quando quiser desmontar (ou desconectar) o compartilhamento de arquivos do Azure, clique com o botão direito na entrada para o compartilhamento em "Locais de rede" no Explorador de Arquivos e selecione "Desconectar"**.

## <a name="mount-the-azure-file-share-with-powershell"></a>Como montar o compartilhamento de arquivos do Azure com o PowerShell
1. **Use o comando a seguir para montar o compartilhamento de arquivos do Azure**: não se esqueça de substituir `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` com as informações apropriadas.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Use o compartilhamento de arquivos do Azure como quiser**.

3. **Quando tiver terminado, desmonte o compartilhamento de arquivos do Azure usando o seguinte comando**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> Você pode usar o parâmetro `-Persist` no `New-PSDrive` para tornar o compartilhamento de arquivos do Azure visível para o restante do sistema operacional enquanto montado.

## <a name="mount-the-azure-file-share-with-command-prompt"></a>Como montar o compartilhamento de arquivos do Azure com o Prompt de comando
1. **Use o comando a seguir para montar o compartilhamento de arquivos do Azure**: não se esqueça de substituir `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` com as informações apropriadas.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Use o compartilhamento de arquivos do Azure como quiser**.

3. **Quando tiver terminado, desmonte o compartilhamento de arquivos do Azure usando o seguinte comando**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Você pode configurar o compartilhamento de arquivos do Azure para se reconectar automaticamente na reinicialização por manter as credenciais do Windows. O comando a seguir manterá as credenciais:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Próximas etapas
Consulte estes links para obter mais informações sobre o armazenamento de arquivo do Azure.

* [Perguntas frequentes](storage-files-faq.md)
* [Solução de problemas](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Artigos e vídeos conceituais
* [Armazenamento de Arquivos do Azure: um sistema de arquivos SMB de nuvem ininterrupto para Windows e Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Como utilizar o Armazenamento de Arquivos do Azure com Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Suporte de ferramentas para o armazenamento de Arquivos do Azure
* [Usando o PowerShell do Azure com o Armazenamento do Azure](storage-powershell-guide-full.md)
* [Como usar o AzCopy com o Armazenamento do Microsoft Azure](storage-use-azcopy.md)
* [Usando a CLI do Azure com o Armazenamento do Azure](storage-azure-cli.md#create-and-manage-file-shares)
* [Solução de problemas do armazenamento de Arquivos do Azure](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Postagens no blog
* [O Armazenamento de arquivos do Azure agora está disponível ao público geral](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Por dentro do Armazenamento de Arquivos do Azure](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Apresentando o serviço de arquivo do Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Como migrar dados para o arquivo do Azure ](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referência
* [Referência à Biblioteca de Cliente de Armazenamento para .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referência à API REST do serviço de arquivos](http://msdn.microsoft.com/library/azure/dn167006.aspx)

