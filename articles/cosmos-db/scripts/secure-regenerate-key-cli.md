---
title: "Script da CLI do Azure – regenerar chave de conta do BD Cosmos do Azure | Microsoft Docs"
description: "Exemplo de script da CLI do Azure – regenerar uma chave de conta do BD Cosmos do Azure"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 1a0ff3f8b8fb3eaf398d9fa925ef027b2481d47a
ms.contentlocale: pt-br
ms.lasthandoff: 07/18/2017

---

# <a name="regenerate-an-azure-cosmos-db-account-key-using-the-azure-cli"></a>Regenere uma chave de conta do BD Cosmos do Azure usando a CLI do Azure

Este exemplo regenera qualquer tipo de chave de conta do BD Cosmos do Azure usando a CLI do Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI localmente, este tópico exigirá que você esteja executando a CLI do Azure versão 2.0 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[principal](../../../cli_scripts/cosmosdb/secure-cosmosdb-regenerate-keys/secure-cosmosdb-regenerate-keys.sh?highlight=27-31 "Regenerar chaves de conta do BD Cosmos do Azure")]

## <a name="clean-up-deployment"></a>Limpar implantação

Após executar o exemplo de script, o comando a seguir pode ser usado para remover o grupo de recursos e todos os recursos associados a ele.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os seguintes comandos. Cada comando na tabela redireciona para a documentação específica do comando.

| Command | Observações |
|---|---|
| [az group create](/cli/azure/group#create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#create) | Atualiza uma conta do BD Cosmos do Azure. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb/regenerate-key) | Regenera chaves de conta do BD Cosmos do Azure. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#delete) | Exclui um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Exemplos adicionais de scripts da CLI do Banco de Dados Cosmos do Azure podem ser encontrados na [Documentação da CLI do Banco de Dados Cosmos do Azure](../cli-samples.md).

