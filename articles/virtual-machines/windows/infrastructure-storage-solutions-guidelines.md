---
title: "Soluções de armazenamento para VMs do Windows no Azure | Microsoft Docs"
description: "Saiba mais sobre as principais diretrizes de design e implementação referentes à implantação de soluções de armazenamento em serviços de infraestrutura do Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: c0fabf155d4feb6d88ef7d7e087cc1654f44978b
ms.contentlocale: pt-br
ms.lasthandoff: 06/28/2017

---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Diretrizes de infraestrutura de armazenamento do Azure para VMs Windows

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Este artigo destaca as noções básicas sobre as necessidades de armazenamento e considerações de design para atingir o desempenho de VM (máquina virtual) ideal.

## <a name="implementation-guidelines-for-storage"></a>Diretrizes de implementação de armazenamento
Decisões:

* Você pretende usar o Azure Managed Disks ou discos não gerenciados?
* Você precisa usar o armazenamento Standard ou Premium para sua carga de trabalho?
* Você precisa de distribuição de disco para criar discos maiores que 4TB?
* Você precisa da distribuição de discos para obter o desempenho de E/S ideal para sua carga de trabalho?
* De que conjunto de contas de armazenamento você precisa para hospedar a infraestrutura ou a carga de trabalho de TI?

Tarefas:

* Examinar as demandas de E/S dos aplicativos que serão implantados e planejar a quantidade e o tipo apropriados de contas de armazenamento.
* Crie o conjunto de contas de armazenamento usando a sua convenção de nomenclatura. Você pode usar o Azure PowerShell ou o Portal.

## <a name="storage"></a>Armazenamento
O Armazenamento do Azure é uma parte fundamental de implantação e gerenciamento de aplicativos e VMs (máquinas virtuais). O Armazenamento do Azure fornece serviços para armazenar dados de arquivo, dados não estruturados e mensagens, além de fazer parte da infraestrutura que dá suporte às VMs.

O [Azure Managed Disks](../../storage/storage-managed-disks-overview.md) lida com o armazenamento para você nos bastidores. Com discos não gerenciados, você cria contas de armazenamento para armazenar os discos (arquivos VHD) para as VMs do Azure. Ao aumentar, é necessário verificar se você criou contas de armazenamento adicionais para não exceder o limite de IOPS de armazenamento com um dos discos. Com o Managed Disks lidando com o armazenamento, não há mais os limites de conta de armazenamento (como 20.000 IOPS/conta). Também não é mais necessário copiar as imagens personalizadas (arquivos VHD) em várias contas de armazenamento. Você pode gerenciá-las em um local central, uma conta de armazenamento por região do Azure, e usá-las para criar centenas de VMs em uma assinatura. Recomendamos o uso do Managed Disks para novas implantações.

Há dois tipos de conta de armazenamento disponíveis para dar suporte às VMs:

* Contas de armazenamento Standard fornece acesso ao armazenamento de blobs (usado para armazenar discos da VM do Azure), armazenamento de tabelas, armazenamento de filas e armazenamento de arquivos.
* [armazenamento premium](../../storage/storage-premium-storage.md) dão suporte de disco de alto desempenho e baixa latência para cargas de trabalho com uso intensivo de E/S, como SQL Servers em um cluster AlwaysOn. Um armazenamento premium dá suporte no momento somente a discos de VM do Azure.

O Azure cria VMs com um disco do sistema operacional, um disco temporário e zero ou mais discos de dados opcionais. O disco do sistema operacional e os discos de dados são blobs de página do Azure, enquanto o disco temporário é armazenado localmente no nó em que a máquina reside. Tome cuidado ao projetar aplicativos para usar somente este disco temporário para dados não persistentes, pois a VM poderá ser migrada entre hosts durante um evento de manutenção. Todos os dados armazenados no disco temporário seriam perdidos.

Durabilidade e alta disponibilidade são fornecidas pelo ambiente de Armazenamento do Azure subjacente para garantir que seus dados permaneçam protegidos contra falhas de hardware ou de manutenção não planejadas. Ao projetar seu ambiente de Armazenamento do Azure, você pode optar por replicar o armazenamento da VM:

* localmente em um datacenter do Azure fornecido
* entre os datacenters do Azure dentro de uma determinada região
* entre os datacenters do Azure em regiões diferentes

Você pode ler [mais sobre as opções de replicação para alta disponibilidade](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Discos do sistema operacional e discos de dados possuem um tamanho máximo de 4TB. É possível usar os Espaços de Armazenamento no Windows Server 2012 ou posterior para ultrapassar esse limite por meio de pooling de discos de dados para apresentar volumes lógicos maiores que 4TB à sua VM.

Há alguns limites de escalabilidade ao projetar suas implantações do Armazenamento do Azure. Para obter mais informações, consulte [Assinatura do Microsoft Azure e limite de serviços, cotas e restrições](../../azure-subscription-service-limits.md#storage-limits). Consulte também [Metas de desempenho e escalabilidade do armazenamento do Azure](../../storage/storage-scalability-targets.md).

Para armazenamento de aplicativos, é possível armazenar dados de objeto não estruturados, como documentos, imagens, backups, dados de configuração, logs etc. usando o armazenamento de blobs. Em vez de seu aplicativo gravar em um disco virtual anexado à VM, o aplicativo poderá gravar diretamente no armazenamento de blobs do Azure. O armazenamento de blobs também oferece a opção de [camadas de armazenamento quentes e frias](../../storage/storage-blob-storage-tiers.md) dependendo de suas necessidades de disponibilidade e restrições de custo.

## <a name="striped-disks"></a>Discos distribuídos
Além de permitir que você crie discos com mais de 4TB, em muitos casos, o uso da distribuição para discos de dados pode melhorar o desempenho, permitindo que vários blobs façam o armazenamento de um único volume. Com a distribuição, a E/S necessária para gravar e ler dados de um único disco lógico continua em paralelo.

O Azure impõe limites no número de discos de dados e na quantidade de largura de banda disponíveis, dependendo do tamanho da VM. Para obter detalhes, consulte [Tamanhos das máquinas virtuais](sizes.md).

Se você estiver usando a distribuição de disco para os discos de dados do Azure, considere as seguintes diretrizes:

* Anexar o número máximo de discos de dados permitidos para o tamanho da VM.
* Use Espaços de Armazenamento.
* Evite usar opções de cache de disco de dados do Azure (política de cache = Nenhuma).

Para saber mais, consulte [Espaços de armazenamento: design para desempenho](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).

## <a name="multiple-storage-accounts"></a>Várias contas de armazenamento
Esta seção não se aplica ao [Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), pois você não cria contas de armazenamento separadas. 

Durante a criação do ambiente do Armazenamento do Azure para discos não gerenciados, você poderá usar várias contas de armazenamento conforme o número de VMs implantadas aumentar. Essa abordagem ajuda a distribuir a E/S em toda a infraestrutura subjacente do Armazenamento do Azure, para manter o desempenho ideal para suas VMs e aplicativos. Ao projetar os aplicativos que serão implantados, considere os requisitos de E/S que cada VM terá e faça um balanceamento dessas VMs entre as contas do Armazenamento do Azure. Tente evitar agrupar todas as VMs que exigem E/S alta em apenas uma ou duas contas de armazenamento.

Para saber mais sobre as funcionalidades de E/S das diferentes opções do Armazenamento do Azure e de alguns limites máximos recomendáveis, veja [Metas de desempenho e escalabilidade do armazenamento do Azure](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Próximas etapas
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


