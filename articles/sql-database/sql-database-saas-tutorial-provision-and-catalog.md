---
title: "Provisionar novos locatários em um aplicativo multilocatário que usa o Banco de Dados SQL do Azure | Microsoft Docs"
description: "Saiba como provisionar e catalogar novos locatários no aplicativo SaaS Wingtip"
keywords: tutorial do banco de dados SQL
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: sstein
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 658c316d8d9d14ce11dbb92188afbf0e68c00493
ms.contentlocale: pt-br
ms.lasthandoff: 07/28/2017

---
# <a name="provision-new-tenants-and-register-them-in-the-catalog"></a>Provisionar novos locatários e registrá-los no catálogo

Neste tutorial, você provisionará novos locatários no aplicativo de SaaS do Wingtip. Crie locatários provisionando bancos de dados de locatários e registrando-os no catálogo. O *catálogo* é um banco de dados que mantém o mapeamento entre os locatários de um aplicativo SaaS e seus dados.

Use esses scripts para explorar os padrões de provisionamento e catálogo usados e como é a implementação do registro de novos locatários no catálogo. O catálogo desempenha uma função importante no direcionamento das solicitações de aplicativo para o banco de dados correto.

Neste tutorial, você aprenderá a:

> [!div class="checklist"]

> * Provisionar um novo único locatário
> * Provisionar um lote de locatários adicionais
> * Intervir nos detalhes do provisionamento de locatários e do registro deles no catálogo


Para concluir este tutorial, verifique se todos os pré-requisitos a seguir são atendidos:

* O aplicativo SaaS Wingtip é implantado. Para implantar em menos de cinco minutos, confira [Implantar e explorar o aplicativo de SaaS do Wingtip](sql-database-saas-tutorial.md)
* O Azure PowerShell está instalado. Para obter detalhes, consulte [Introdução ao Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-catalog-pattern"></a>Introdução ao padrão de catálogo de SaaS

Em um aplicativo de SaaS multilocatário com suporte de banco de dados, é importante saber o local em que estão armazenadas as informações de cada locatário. No padrão de catálogo de SaaS, um banco de dados de catálogo é usado para manter o mapeamento entre locatários e o local em que os respectivos dados estão armazenados. O aplicativo SaaS Wingtip usa uma arquitetura de locatário único por banco de dados, mas o padrão básico de armazenamento do mapeamento de locatário para banco de dados em um catálogo aplica-se tanto ao uso de um banco de dados multilocatário quanto ao de locatário único.

A cada locatário é atribuída uma chave que distingue seus dados no catálogo. No aplicativo de SaaS do Wingtip, a chave é formada por um hash do nome do locatário. Esse padrão permite que a parte do nome do locatário da URL do aplicativo seja usada para construir a chave e recuperar a conexão de um locatário específico. Outros esquemas de ID podem ser usados sem afetar o padrão geral.

O catálogo no aplicativo é implementado usando a tecnologia de Gerenciamento de Fragmentos na [EDCL (Biblioteca de cliente do banco de dados elásticos)](sql-database-elastic-database-client-library.md). A EDCL é responsável por criar e gerenciar um *catálogo* com suporte de banco de dados em que um *mapa de fragmentos* é mantido. O catálogo contém o mapeamento entre chaves (locatários) e seus fragmentos (bancos de dados).

> [!IMPORTANT]
> Os dados de mapeamento estão acessíveis no banco de dados de catálogo, mas *não os edite*! Edite os dados de mapeamento somente com o uso de APIs da Biblioteca de Cliente do Banco de Dados Elástico. Manipular diretamente os dados de mapeamento gera o risco de corrupção do catálogo e não há suporte para isso.



## <a name="get-the-wingtip-application-scripts"></a>Obter os scripts do aplicativo Wingtip

Os scripts de SaaS do Wingtip e o código-fonte do aplicativo estão disponíveis no repositório GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). [Etapas para baixar os scripts do SaaS Wingtip](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="provision-one-new-tenant"></a>Provisionar um novo locatário

Se você já criou um locatário no [primeiro tutorial de SaaS do Wingtip](sql-database-saas-tutorial.md), siga direto para a próxima seção, em que você [provisiona um lote de locatários](#provision-a-batch-of-tenants).

Execute o script *Demo-ProvisionAndCatalog* para criar um locatário rapidamente e registrá-lo no catálogo:

1. Abra o **Demo-ProvisionAndCatalog.ps1** no ISE do PowerShell e defina os seguintes valores:
   * **$TenantName** = o nome do novo local do evento (por exemplo, *Bushwillow Blues*).
   * **$VenueType** = um dos tipos predefinidos de local: blues, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer.
   * **$DemoScenario** = 1, deixe isso definido como _1_ para *Provisionar um único locatário*.

1. Pressione **F5** e execute o script.

Depois que o script for concluído, o novo locatário será provisionado e seu aplicativo *Eventos* será aberto no navegador:

![Novo locatário](./media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Provisionar um lote de locatários

Este exercício provisiona um lote de locatários adicionais. É recomendável provisionar um lote de locatários antes de concluir os outros tutoriais de SaaS do Wingtip, de forma que haja mais do que apenas alguns bancos de dados com os quais trabalhar.

1. Abra ...\\Módulos de Aprendizado\\Utilitários\\*Demo-ProvisionAndCatalog.ps1* no *ISE do PowerShell* e altere o parâmetro *$DemoScenario* para 3:
   * **$DemoScenario** = **3**, altere para **3** para *Provisionar um lote de locatários*.
1. Pressione **F5** e execute o script.

O script implanta um lote de locatários adicionais. Ele usa um [modelo do Azure Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md) que controla o lote e, em seguida, delega o provisionamento de cada banco de dados a um modelo vinculado. O uso de modelos dessa maneira permite que Azure Resource Manager seja o agente do processo de provisionamento do seu script. Os modelos provisionam bancos de dados em paralelo nos locais em que isso for possível e lidam com repetições, se necessário, otimizando o processo geral. O script é idempotente; portanto, se ele falhar ou parar por qualquer motivo, execute-o novamente.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Verificar se o lote de locatários foi implantado com êxito

* Abra o servidor *tenants1* navegando até sua lista de servidores no [Portal do Azure](https://portal.azure.com), clique em **Bancos de Dados SQL** e verifique se o lote de 17 bancos de dados adicionais está na lista:

   ![lista de banco de dados](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)


## <a name="provision-and-catalog-details"></a>Detalhes do provisionamento e do catálogo

Para compreender melhor como o aplicativo Wingtip implementa o provisionamento do novo locatário, execute o script *Demo-ProvisionAndCatalog* novamente e provisione mais um locatário. Desta vez, adicione um ponto de interrupção e percorra o fluxo de trabalho:

1. Abra ...\\Learning Modules\Utilities\_Demo-ProvisionAndCatalog.ps1_ e defina os seguintes parâmetros:
   * **$TenantName** = os nomes de locatário devem ser exclusivos; portanto, defina-os com um nome diferente dos locatários existentes (por exemplo, *Humberto Gomes*).
   * **$VenueType** = usar um dos tipos predefinidos de local (por exemplo, *judo*).
   * **$DemoScenario** = **1**, defina como **1** para *Provisionar um único locatário*.

1. Adicione um ponto de interrupção, colocando o cursor em qualquer local na linha a seguir: *New-Tenant `* e pressione **F9**.

   ![ponto de interrupção](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Para executar o script, pressione **F5**. Quando o ponto de interrupção for alcançado, pressione **F11** para intervir. Rastreie a execução do script usando as opções de menu Depurar – **F10** e **F11** para passar por cima ou intervir nas funções chamadas. Para saber mais sobre como depurar scripts do PowerShell, confira [Dicas sobre como trabalhar e depurar scripts do PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

### <a name="examine-the-provision-and-catalog-implementation-in-detail-by-stepping-through-the-script"></a>Examinar a implementação do catálogo e o provisionamento em detalhes ao percorrer o script

Os script provisiona e cataloga novos locatários seguindo as etapas a seguir:

1. **Importar o módulo SubscriptionManagement.psm1** que contém funções para entrar no Azure e selecionar a assinatura do Azure com a qual você está trabalhando.
1. **Importar o módulo CatalogAndDatabaseManagement.psm1** que fornece um catálogo e uma abstração em nível de locatário em relação às funções de [Gerenciamento de Fragmentos](sql-database-elastic-scale-shard-map-management.md). Este é um módulo importante que encapsula a maior parte do padrão do catálogo e vale a pena explorar.
1. **Obter detalhes de configuração**. Intervenha em *Get-Configuration* (com **F11**) e veja como a configuração do aplicativo é especificada. Os nomes de recursos e outros valores específicos do aplicativo são definidos aqui, mas não altere nenhum desses valores até que você esteja familiarizado com os scripts.
1. **Obter o objeto de catálogo**. Intervenha em *Get-Catalog* para ver como o catálogo é inicializado usando as funções de Gerenciamento de Fragmentos que são importadas do **AzureShardManagement.psm1**. O catálogo é composto dos seguintes objetos:
   * O $catalogServerFullyQualifiedName é criado usando o tronco padrão além do seu nome de Usuário: _catalog-\<usuário\>.database.windows.net_.
   * O $catalogDatabaseName é recuperado da configuração: *tenantcatalog*.
   * O objeto $shardMapManager é inicializado no banco de dados do catálogo.
   * O objeto $shardMap é inicializado por meio do mapa do fragmentos do *tenantcatalog* no banco de dados de catálogo.
   Um objeto de catálogo é composto e retornado e usado no script de nível superior.
1. **Calcular a nova chave de locatário**. Uma função de hash é usada para criar a chave de locatário com base no nome do locatário.
1. **Verificar se a chave de locatário já existe**. O catálogo é verificado para garantir que a chave está disponível.
1. **O banco de dados do locatário é provisionado com New-TenantDatabase.** Use **F11** para intervir e ver como o banco de dados é provisionado usando um modelo do Resource Manager.

O nome do banco de dados é construído com base no nome do locatário para deixar claro qual fragmento pertence a qual locatário. (Outras estratégias de nomeação de banco de dados podem se facilmente usadas.)

Um modelo do Resource Manager é usado para criar um banco de dados de locatário com a cópia de um banco de dados *final (gold)* (basetenantdb) no servidor de catálogo.  Uma abordagem alternativa seria criar um banco de dados vazio e, em seguida, inicializá-lo com a importação de um bacpac.

O modelo do Resource Manager está na pasta ...\\Módulos de aprendizado\\Comum\\: *tenantdatabasecopytemplate.json*

Depois que o banco de dados do locatário é criado, ele é, em seguida, inicializado com o nome do local (locatário) e o tipo de local. Outra inicialização também poderia ser feita aqui.

O banco de dados de locatário é registrado no catálogo com *Add-TenantDatabaseToCatalog* usando a chave do locatário. Use **F11** para ver os detalhes:

* O banco de dados de catálogo é adicionado ao mapa de fragmentos (a lista de bancos de dados conhecidos).
* O mapeamento que vincula o valor da chave (locatário) ao fragmento (banco de dados) é criado.
* Metadados adicionais sobre o locatário são adicionados.

Depois que o provisionamento for concluído, a execução retornará ao script original *Demo-ProvisionAndCatalog* e a página **Eventos** do novo locatário será aberta no navegador:

   ![events](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant2.png)


## <a name="other-provisioning-patterns"></a>Outros padrões de provisionamento

Outros padrões de provisionamento não mencionados nesse tutorial incluem:

**Pré-provisionamento de bancos de dados.** O padrão de pré-provisionamento explora o fato de que os bancos de dados de um pool elástico não adicionam custo extra. A cobrança é pelo pool elástico, não pelos bancos de dados e os bancos de dados ociosos não consomem nenhum recurso. Ao pré-provisionar bancos de dados em um pool e alocá-los quando necessário, o tempo de integração do locatário poderá ser reduzido consideravelmente. O número de bancos de dados pré-provisionados pode ser ajustado conforme necessário para manter um buffer adequado para a taxa de provisionamento esperada.

**Provisionamento automático.** No padrão de provisionamento automático, um serviço de provisionamento dedicado é usado para provisionar servidores, pools e bancos de dados automaticamente, conforme necessário – incluindo o pré-provisionamento de bancos de dados em pools elásticos, se desejado. E se os bancos de dados forem encerrados e excluídos, as lacunas nos pools elásticos poderão ser preenchidas pelo serviço de provisionamento conforme desejado. Esse serviço poderá ser simples ou complexo – por exemplo, manipulação do provisionamento em várias áreas geográficas e poderá configurar a replicação geográfica automaticamente, caso essa estratégia esteja sendo usada para a recuperação de desastre. Com o padrão de provisionamento automático, um script ou aplicativo cliente poderia enviar uma solicitação de provisionamento para uma fila para ser processada pelo serviço de provisionamento e, em seguida, pesquisaria o serviço para determinar a conclusão. Se o pré-provisionamento fosse usado, as solicitações seriam manipuladas rapidamente com o serviço gerenciando o provisionamento de um banco de dados de substituição em execução em segundo plano.



## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a:

> [!div class="checklist"]

> * Provisionar um novo único locatário
> * Provisionar um lote de locatários adicionais
> * Intervir nos detalhes do provisionamento de locatários e do registro deles no catálogo

Experimente o [Tutorial de monitoramento de desempenho](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Tutoriais adicionais que aproveitam o aplicativo de SaaS do Wingtip](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Biblioteca de cliente do banco de dados elástico](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Como depurar scripts no ISE do Windows PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

