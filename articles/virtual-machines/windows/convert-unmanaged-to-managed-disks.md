---
title: "Converter uma máquina virtual do Windows de discos não gerenciados para Managed Disks – Azure Managed Disks | Microsoft Docs"
description: "Como converter uma VM do Windows de discos não gerenciados em Managed Disks usando o PowerShell no modelo de implantação do Resource Manager"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: cynthn
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: 53681c58ca1eff394d6a3db2d6a026845ac03df1
ms.contentlocale: pt-br
ms.lasthandoff: 08/05/2017

---

# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Converter uma máquina virtual do Windows de discos não gerenciados em Managed Disks

Se você tiver VMs (máquinas virtuais) do Windows existentes que usam discos não gerenciados, será possível converter as VMs para usar Managed Disks por meio do serviço [Azure Managed Disks](../../storage/storage-managed-disks-overview.md). Esse processo converte o disco do sistema operacional e os discos de dados anexados.

Este artigo mostra como converter VMs usando o Azure PowerShell. Se você precisa instalá-lo ou atualizá-lo, confira [Instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Antes de começar


* Revisão [Planejar a migração para os Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Converter VMs de instância única
Esta seção aborda como converter suas VMs de instância única do Azure de discos não gerenciados em discos gerenciados. (Se suas VMs estão em uma conjunto de disponibilidade, consulte a próxima seção.) 

1. Desaloque a VM usando o cmdlet [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). O seguinte exemplo desaloca a VM `myVM` no grupo de recursos chamado `myResourceGroup`: 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Converter a VM em Managed Disks usando o cmdlet [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk). O processo a seguir converte a VM anterior incluindo o disco do sistema operacional e eventuais discos de dados:

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. Inicie a VM após a conversão em Managed Disks usando [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm). O exemplo a seguir reinicia a VM anterior:

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a>Converter VMs em um conjunto de disponibilidade

Se as VMs que você deseja converter em discos gerenciados estão em um conjunto de disponibilidade, converta primeiro o conjunto de disponibilidade em um conjunto de disponibilidade gerenciado.

1. Converter o conjunto de disponibilidade usando o cmdlet [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset). O exemplo a seguir atualiza o conjunto de disponibilidade nomeado `myAvailabilitySet` no grupo de recursos nomeado`myResourceGroup`:

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Se a região em que o conjunto de disponibilidade está localizado tem apenas 2 domínios de falha gerenciados, mas o número de domínios de falha não gerenciado é 3, este comando mostra um erro semelhante a "O total de domínio de falha especificado é 3 e deve estar no intervalo de 1 a 2". Para resolver o erro, atualize o domínio de falha para 2 e atualize `Sku` para `Aligned` da seguinte maneira:

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Desaloque e converta as VMs no conjunto de disponibilidade. O script a seguir desaloca cada VM usando o cmdlet [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm), converte-a usando [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) e a reinicia usando [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):

  ```powershell
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  }
  ```


## <a name="convert-standard-managed-disks-to-premium"></a>Converter Managed Disks Standard para Premium
Depois de converter sua VM em Managed Disks, você pode alternar entre os tipos de armazenamento. Você também pode ter uma combinação de discos que usam o armazenamento Standard e Premium. 

No exemplo a seguir, mostramos como alternar do armazenamento Standard para o Premium. Para usar Managed Disks Premium, sua VM deve usar um [tamanho da VM](sizes.md) que dá suporte a armazenamento Premium. Este exemplo também pode mudar para um tamanho que dá suporte a armazenamento Premium.

```powershell
$rgName = 'myResourceGroup'
$vmName = 'YourVM'
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change the VM size to a size that supports premium storage
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType PremiumLRS
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="troubleshooting"></a>Solucionar problemas

Se houver um erro durante a conversão ou se uma VM estiver em um estado de falha devido a problemas em uma conversão anterior, execute o cmdlet `ConvertTo-AzureRmVMManagedDisk` novamente. Normalmente, uma repetição simples desbloqueia a situação.


## <a name="next-steps"></a>Próximas etapas

Faça uma cópia somente leitura de uma VM usando [instantâneos](snapshot-copy-managed-disk.md).


