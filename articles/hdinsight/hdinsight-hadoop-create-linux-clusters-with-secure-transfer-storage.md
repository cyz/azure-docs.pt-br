---
title: "Criar um cluster Hadoop com contas de armazenamento para transferência segura no Azure HDInsight | Microsoft Docs"
description: "Saiba como criar clusters HDInsight com contas de armazenamento do Azure com transferência segura habilitada."
keywords: "guia de introdução do hadoop, hadoop linux, início rápido do hadoop, transferência segura, conta de armazenamento do azure"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: f89595c6721c00435e714368905ae48a542f8cb9
ms.contentlocale: pt-br
ms.lasthandoff: 08/03/2017

---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Criar um cluster Hadoop com contas de armazenamento para transferência segura no Azure HDInsight

O recurso [Transferência segura exigida](../storage/storage-require-secure-transfer.md) aprimora a segurança de sua conta de Armazenamento do Azure aplicando todas as solicitações à sua conta por meio de uma conexão segura. Esse recurso e o esquema wasbs só têm suporte da versão 3.6 ou mais recente do cluster HDInsight. 

>[!NOTE] 
> No momento, não há suporte para a criação de clusters com a conta de armazenamento com transferência segura habilitada usando o SDK do .NET. A solução alternativa é definir "wasbs" na propriedade "fs.defaultFS" na configuração do site central como parte de ClusterCreateParametersExtended.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial, você deverá ter o seguinte:

* **Assinatura do Azure**: para criar uma conta de avaliação gratuita de um mês, acesse [azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Uma conta de Armazenamento do Azure com transferência segura habilitada**. Para obter as instruções, consulte [Criar uma conta de armazenamento](../storage/storage-create-storage-account.md#create-a-storage-account) e [Exigir transferência segura](../storage/storage-require-secure-transfer.md).
* **Um contêiner de Blob na conta de armazenamento**. 
## <a name="create-cluster"></a>Criar cluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Nesta seção, você criará um cluster Hadoop no HDInsight usando um [modelo do Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md). O modelo está localizado em um [contêiner público](https://hditutorialdata.blob.core.windows.net/securetransfer/azuredeploy-new.json). Não é necessário ter experiência com o modelo do Resource Manager para seguir este tutorial. Para obter outros métodos de criação de cluster e Noções básicas sobre as propriedades usadas neste tutorial, confira [Criar clusters do HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Clique na imagem a seguir para entrar no Azure e abra o modelo do Resource Manager no Portal do Azure. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fsecuretransfer%2Fazuredeploy-new.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Siga as instruções para criar o cluster com as seguintes especificações: 

    - Especifique o HDInsight versão 3.6.  A versão padrão é a 3.5. É necessário ter a versão 3.6 ou mais recente.
    - Especifique uma conta de armazenamento com transferência segura habilitada.
    - Use um nome curto para a conta de armazenamento.
    - A conta de armazenamento e o contêiner de blob devem ser criados com antecedência. 

    Para obter as instruções, confira [Criar cluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 

Se você usar ação de script para fornecer seus próprios arquivos de configuração, use wasbs nas seguintes configurações:

- fs.defaultFS (site principal)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Adicionar outras contas de armazenamento

Há várias opções para adicionar outras contas de armazenamento com transferência segura habilitada:

- Modifique o modelo do Azure Resource Manager na última seção.
- Crie um cluster usando o [Portal do Azure](https://portal.azure.com) e especifique a conta de armazenamento vinculada.
- Use ações de script para adicionar outras contas de armazenamento com transferência segura habilitada a um cluster existente do HDInsight.  Para saber mais, confira [Adicionar outras contas de armazenamento ao HDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você aprendeu a criar um cluster do HDInsight e a habilitar a transferência segura para as contas de armazenamento.

Para saber mais sobre como analisar dados com o HDInsight, consulte os seguintes artigos:

* Para saber mais sobre como usar o Hive com HDInsight, inclusive como executar consultas de Hive do Visual Studio, consulte [usar o Hive com HDInsight][hdinsight-use-hive].
* Para saber mais sobre o Pig, uma linguagem usada para transformar dados, consulte [Usar o Pig com o HDInsight][hdinsight-use-pig].
* Para saber mais sobre o MapReduce, uma maneira de gravar programas que processam dados no Hadoop, consulte [Usar o MapReduce com HDInsight][hdinsight-use-mapreduce].
* Para saber mais sobre como usar as Ferramentas do HDInsight para o Visual Studio para analisar dados no HDInsight, consulte [Introdução às ferramentas do Hadoop do Visual Studio para o HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Para saber mais sobre como o HDInsight armazena dados, ou como inserir os dados no HDInsight, consulte os artigos a seguir:

* Para saber mais sobre como o HDInsight usa o Armazenamento do Azure, veja [Usar Armazenamento do Azure com o HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Para obter informações sobre como carregar arquivos no HDInsight, consulte [Carregar dados no HDInsight][hdinsight-upload-data].

Para saber mais sobre como criar ou gerenciar um cluster do HDInsight, consulte os artigos a seguir:

* Para saber mais sobre como gerenciar o cluster HDInsight baseado em Linux, consulte [Gerenciar clusters HDInsight usando o Ambari](hdinsight-hadoop-manage-ambari.md).
* Para saber mais sobre as opções que você pode selecionar ao criar um cluster HDInsight, confira [Como criar o HDInsight no Linux usando opções personalizadas](hdinsight-hadoop-provision-linux-clusters.md).
* Se você estiver familiarizado com o Linux e o Hadoop, mas quiser obter informações específicas sobre o Hadoop no HDInsight, consulte [Trabalhando com o HDInsight no Linux](hdinsight-hadoop-linux-information.md). Este artigo oferece informações como:
  
  * URLs para serviços hospedados no cluster, como Ambari e WebHCat
  * O local dos arquivos do Hadoop e exemplos no sistema de arquivos local
  * O uso do Azure Storage (WASB) em vez de HDFS como armazenamento de dados padrão

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md



