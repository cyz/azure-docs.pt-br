---
title: Navegar e selecionar as imagens da VM do Windows | Microsoft Docs
description: "Saiba como determinar o editor, a oferta e o SKU para imagens ao criar uma máquina virtual do Windows com o modelo de implantação do Gerenciador de Recursos."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: danlep
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 28bb214570fcca94c5ceb6071c4851b81ec00c8d
ms.contentlocale: pt-br
ms.lasthandoff: 04/27/2017

---
<a id="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell" class="xliff"></a>

# Navegar e selecionar imagens da máquina virtual do Windows no Azure com o PowerShell
Este tópico descreve como localizar editores de imagens de VM, ofertas, SKUs e versões de cada local no qual você pode implantar. Para dar um exemplo, algumas das imagens mais usadas da VM do Windows são:

<a id="table-of-commonly-used-windows-images" class="xliff"></a>

## Tabela das imagens do Windows mais usadas
| PublisherName | Oferta | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-DW |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-OLTP |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008-R2-SP1 |
| MicrosoftWindowsServer |WindowsServer |Windows-Server-Technical-Preview |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |

<a id="find-azure-images-with-powershell" class="xliff"></a>

## Encontrar imagens do Azure com o PowerShell
> [!NOTE]
> Instalar e configurar o [Azure PowerShell mais recente](/powershell/azure/overview). Se você estiver usando módulos do Azure PowerShell inferiores a 1.0, ainda usa os comandos a seguir, mas deve primeiro `Switch-AzureMode AzureResourceManager`. 
> 
> 

Ao criar uma nova máquina virtual com o Gerenciador de Recursos do Azure, em alguns casos é necessário especificar uma imagem com a combinação das propriedades de imagem a seguir:

* Editor
* Oferta
* SKU

Por exemplo, esses valores são necessários para o cmdlet `Set-AzureRMVMSourceImage` do PowerShell ou com um arquivo de modelo de grupo de recursos no qual você deve especificar o tipo de máquina virtual a ser criado.

Se você precisar determinar esses valores, você pode navegar pelas imagens para determinar esses valores:

1. Liste os editores de imagem.
2. Para um determinado editor, liste suas ofertas.
3. Para uma determinada oferta, liste seus SKUs.

Primeiro, lista os editores com os seguintes comandos:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Preencha o nome do editor escolhido e execute os seguintes comandos:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Preencha o nome da oferta escolhida e execute os seguintes comandos:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Na exibição do comando `Get-AzureRMVMImageSku` , você tem todas as informações de que precisa para especificar a imagem para uma nova máquina virtual.

O exemplo a seguir mostra um exemplo completo:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Para o editor "MicrosoftWindowsServer":

```powershell
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

Para a oferta de "Windows Server":

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Nesta lista, copie o nome da SKU escolhida e você terá todas as informações do cmdlet `Set-AzureRMVMSourceImage` do PowerShell ou de um modelo de grupo de recursos.

<a id="next-steps" class="xliff"></a>

## Próximas etapas
Agora você pode escolher exatamente a imagem que deseja usar. Para criar uma máquina virtual rapidamente usando as informações de imagem, que você acabou de encontrar, ou para usar um modelo com as informações dessa imagem, veja [Criar uma VM do Windows usando o Resource Manager e o PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

