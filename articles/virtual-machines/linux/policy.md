---
title: "Impor a segurança com políticas em VMs do Linux no Azure | Microsoft Docs"
description: "Como aplicar uma política a uma Máquina Virtual Windows Linux do Azure Resource Manager"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: singhkay
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: c1ad80a56627695f3594f4d9b60cd623fa9bcce3
ms.contentlocale: pt-br
ms.lasthandoff: 06/30/2017


---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Aplicar políticas a VMs Linux com o Azure Resource Manager
Usando políticas, uma organização pode impor várias convenções e regras em toda a empresa. A imposição do comportamento desejado pode ajudar a reduzir o risco e contribui para o sucesso da organização. Neste artigo, descrevemos como você pode usar as políticas do Azure Resource Manager para definir o comportamento desejado das Máquinas Virtuais de sua organização.

Para obter uma introdução às políticas, consulte [Usar a política para gerenciar recursos e controlar o acesso](../../azure-resource-manager/resource-manager-policy.md).

## <a name="define-policy-for-permitted-virtual-machines"></a>Definir a política de Máquinas Virtuais permitidas
Para garantir que as máquinas virtuais de sua organização são compatíveis com um aplicativo, você pode restringir os sistemas operacionais permitidos. No exemplo de política a seguir, você permite que apenas Máquinas Virtuais Ubuntu 14.04.2-LTS sejam criadas.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Use um curinga para modificar a política anterior a fim de permitir qualquer imagem do Ubuntu LTS: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Para obter informações sobre campos de política, consulte [Aliases de política](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="define-policy-for-using-managed-disks"></a>Definir a política para uso de discos gerenciados

Para exigir o uso de discos gerenciados, use a seguinte política:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a>Próximas etapas
* Depois de definir uma regra de política (conforme mostrado nos exemplos anteriores), você precisará criar a definição de política e atribuí-la a um escopo. O escopo pode ser uma assinatura, grupo de recursos ou recurso. Para atribuir políticas por meio do portal, consulte [Usar o portal do Azure para atribuir e gerenciar políticas de recurso](../../azure-resource-manager/resource-manager-policy-portal.md). Para atribuir políticas por meio da API REST, do PowerShell ou da CLI do Azure, consulte [Atribuir e gerenciar políticas por meio de script](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Para ver uma introdução às políticas de recurso, confira [Visão geral da política de recurso](../../azure-resource-manager/resource-manager-policy.md).
* Para obter orientação sobre como as empresas podem usar o Resource Manager para gerenciar assinaturas de forma eficaz, consulte [Azure enterprise scaffold – controle de assinatura prescritivas](../../azure-resource-manager/resource-manager-subscription-governance.md).

