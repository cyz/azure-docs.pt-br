---
title: "Exemplo de script da CLI do Azure – Copiar (mover) discos gerenciados para a mesma assinatura ou outra assinatura | Microsoft Docs"
description: "Exemplo de script da CLI do Azure – Copiar (mover) discos gerenciados para a mesma assinatura ou outra assinatura"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.translationtype: Human Translation
ms.sourcegitcommit: be747170a0d8a7a6defd790a3f8a122c4d397671
ms.openlocfilehash: 90228d66150cf961557b6dbeed11308932677d83
ms.contentlocale: pt-br
ms.lasthandoff: 05/23/2017

---

# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Copiar um disco gerenciado para a mesma assinatura ou outra assinatura com a CLI

Esse script copia um disco gerenciado para a mesma assinatura ou outra assinatura, mas na mesma região. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli[main](../../../cli_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copiar disco gerenciado")]


## <a name="script-explanation"></a>Explicação sobre o script

Esse script usa os comandos a seguir para criar um novo disco gerenciado na assinatura de destino usando a ID do disco gerenciado de origem. Cada comando na tabela redireciona para a documentação específica do comando.

| Command | Observações |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#show) | Obtém todas as propriedades de um disco gerenciado usando o nome e as propriedades do grupo de recursos do disco gerenciado. A propriedade de ID é usada para copiar o disco gerenciado para assinatura diferente.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | Copia um disco gerenciado criando um novo disco gerenciado em uma assinatura diferente usando a Id e o nome do disco gerenciado pai.  |

## <a name="next-steps"></a>Próximas etapas

[Criar uma máquina virtual com base em um disco gerenciado](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Os exemplos adicionais de script da CLI de máquina virtual e discos gerenciados podem ser encontrados na [documentação da VM Linux do Azure](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

