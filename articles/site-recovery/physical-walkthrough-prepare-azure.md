---
title: "Preparar recursos do Azure para replicar os servidores físicos locais para o Azure usando o Azure Site Recovery | Microsoft Docs"
description: "Descreve o que você precisa em vigor no Azure antes de iniciar a replicação de servidores locais para o Azure, usando o serviço do Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 2b277f1fb96f347cc60d1395fafb7e2707883a77
ms.contentlocale: pt-br
ms.lasthandoff: 06/29/2017


---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a>Etapa 5: preparar os recursos do Azure para a replicação de servidor físico para o Azure


Use as instruções neste artigo para preparar recursos do Azure para que você possa replicar os servidores locais no Azure usando o serviço [Azure Site Recovery](site-recovery-overview.md).

Poste perguntas e comentários na parte inferior deste artigo ou no [Fórum de Serviços de Recuperação do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Antes de começar

Não se esqueça de ler os [pré-requisitos](physical-walkthrough-prerequisites.md).

## <a name="set-up-an-azure-account"></a>Configurar uma conta do Azure

- Obter uma [conta do Microsoft Azure](http://azure.microsoft.com/).
- Você pode começar com uma [avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/).
- Verifique as regiões suportadas do Site Recovery, em **Disponibilidade Geográfica** nos [Detalhes dos Preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
- Saiba mais sobre os [Preços do Site Recovery](site-recovery-faq.md#pricing) e obtenha os [Detalhes de preços](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Configurar uma rede do Azure

- Configure uma rede do Azure. As VMs do Azure são colocadas nessa rede quando são criadas após o failover.
- O Site Recovery no portal do Azure pode usar redes configuradas no [Gerenciador de Recursos](../resource-manager-deployment-model.md), ou no modo clássico.
- A rede deve estar na mesma região que o cofre dos Serviços de Recuperação.
- Saiba mais sobre os [preços de rede virtual](https://azure.microsoft.com/pricing/details/virtual-network/).
- Saiba mais sobre a [Conectividade de VM do Azure](physical-walkthrough-network.md) após o failover.


## <a name="set-up-an-azure-storage-account"></a>Configure uma conta de armazenamento do Azure

- O Site Recovery replica servidores locais para o armazenamento do Azure. As VMs do Azure são criadas a partir do armazenamento após o failover.
- [Configure uma conta de armazenamento do Azure](../storage/storage-create-storage-account.md#create-a-storage-account) para os dados replicados.
- O Site Recovery no portal do Azure pode usar contas de armazenamento configuradas no Gerenciador de Recursos, ou no modo clássico.
- A conta de armazenamento pode ser padrão ou [premium](../storage/storage-premium-storage.md).
- Se você configurar uma conta premium, também precisará de uma conta padrão adicional para dados de log.


## <a name="next-steps"></a>Próximas etapas

Vá para a [Etapa 6: configurar um cofre](physical-walkthrough-create-vault.md)

