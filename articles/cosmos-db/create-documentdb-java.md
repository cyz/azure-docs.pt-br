﻿---
title: 'Azure Cosmos DB: compilar um aplicativo com Java e com a API do DocumentDB | Microsoft Docs'
description: "Apresenta um exemplo de código Java que pode ser usado para se conectar à API do DocumentDB do Azure Cosmos DB e consultá-la"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 06/27/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: c178646f0ec10cb08e90c1eda544a2488782187f
ms.contentlocale: pt-br
ms.lasthandoff: 06/28/2017


---
<a id="azure-cosmos-db-build-a-documentdb-api-app-with-java-and-the-azure-portal" class="xliff"></a>

# BD Cosmos do Azure: compilar um aplicativo de API do DocumentDB com Java e com o Portal do Azure

O Azure Cosmos DB é um serviço de banco de dados  multimodelo, globalmente distribuído da Microsoft. É possível criar e consultar rapidamente documentos, chave/valor e bancos de dados do gráfico. Todos se beneficiam de recursos de escala horizontal e distribuição global no núcleo do Azure Cosmos DB. 

Este início rápido demonstra como criar uma conta do Azure Cosmos DB, um banco de dados de documento e uma coleção usando o Portal do Azure. Em seguida, você compilará e executará um aplicativo de console compilado na [API do Java do DocumentDB](documentdb-sdk-java.md).

<a id="prerequisites" class="xliff"></a>

## Pré-requisitos

* Antes que possa executar esta amostra, você deverá ter os seguintes pré-requisitos:
   * JDK 1.7 + (execute `apt-get install default-jdk` se você não tiver o JDK)
   * Maven (execute `apt-get install maven` se você não tiver o Maven)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-database-account" class="xliff"></a>

## Crie uma conta de banco de dados

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="add-a-collection" class="xliff"></a>

## Adicionar uma coleção

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="clone-the-sample-application" class="xliff"></a>

## Clonar o aplicativo de exemplo

Agora, vamos clonar um aplicativo de API do DocumentDB do GitHub, defina a cadeia de conexão e execute-o. Você verá como é fácil trabalhar usando dados de forma programática. 

1. Abra uma janela de terminal do Git, como git bash, e `CD` para um diretório de trabalho.  

2. Execute o comando a seguir para clonar o repositório de exemplo. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

<a id="review-the-code" class="xliff"></a>

## Examine o código

Façamos uma rápida revisão do que está acontecendo no aplicativo. Abra o arquivo `Program.java` e encontre essas linhas de código que criam os recursos do BD Cosmos do Azure. 

* O `DocumentClient` é inicializado.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* Um novo banco de dados é criado.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* Uma nova coleção é criada.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    // DocumentDB collections can be reserved with throughput
    // specified in request units/second. 1 RU is a normalized
    // request equivalent to the read of a 1KB document. Here we
    // create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Alguns documentos são criados.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* Uma consulta SQL no JSON é executada.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

<a id="update-your-connection-string" class="xliff"></a>

## Atualizar sua cadeia de conexão

Agora, volte ao portal do Azure para obter informações sobre a cadeia de conexão e copiá-las para o aplicativo.

1. No [Portal do Azure](http://portal.azure.com/), na sua conta do Azure Cosmos DB, no painel de navegação esquerdo, clique em **Chaves** e, em seguida, clique em **Chaves de leitura/gravação**. Você usará os botões de cópia no lado direito da tela para copiar o URI e a Chave Primária para o arquivo `Program.java` na próxima etapa.

    ![Exibir e copiar uma chave de acesso no Portal do Azure, folha Chaves](./media/create-documentdb-dotnet/keys.png)

2. Abra o arquivo `Program.java`. 

3. Copie o valor do URI do portal (usando o botão de cópia) e transforme-o no valor do ponto de extremidade para o construtor DocumentClient em `Program.java`. 

    `"https://FILLME.documents.azure.com"`

4. Em seguida, copie o valor de CHAVE PRIMÁRIA no portal e substitua o segundo parâmetro "FILL ME" com a chave no construtor do DocumentClient em 'Program.java'. Agora, você atualizou o aplicativo com todas as informações necessárias para se comunicar com o Azure Cosmos DB. 
    
<a id="run-the-app" class="xliff"></a>

## Execute o aplicativo

1. Execute `mvn package` em um terminal para instalar os pacotes necessários do Java.

2. Execute `mvn exec:java -D exec.mainClass=GetStarted.Program` em um terminal para iniciar o aplicativo Java.

Agora, é possível voltar ao Data Explorer e ver a consulta, modificar e trabalhar com esses novos dados. 

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## Examinar SLAs no Portal do Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Limpar recursos

Se você não continuar usando este aplicativo, exclua todos os recursos criados por esse início rápido no portal do Azure com as seguintes etapas:

1. No menu à esquerda no Portal do Azure, clique em **Grupos de recursos** e depois clique no nome do recurso criado. 
2. Em sua página de grupo de recursos, clique em **Excluir**, digite o nome do recurso para excluir na caixa de texto e depois clique em **Excluir**.

<a id="next-steps" class="xliff"></a>

## Próximas etapas

Neste início rápido, você aprendeu como criar uma conta do Azure Cosmos DB, como criar uma coleção usando o Data Explorer e como executar um aplicativo. Agora, é possível importar outros dados para sua conta do Cosmos DB. 

> [!div class="nextstepaction"]
> [Importar dados no BD Cosmos do Azure](import-data.md)



