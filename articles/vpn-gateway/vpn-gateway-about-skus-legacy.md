---
title: SKUs de gateway de rede virtual herdadas do Azure | Microsoft Docs
description: SKUs de gateway de rede virtual anteriores.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 01ef2f0309848be5a891749178196e60fff2930e
ms.contentlocale: pt-br
ms.lasthandoff: 08/03/2017

---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Trabalhar com SKUs de gateway de rede virtual (SKUs herdadas)

Este artigo contém informações sobre as SKUs de gateway de rede virtual herdadas (antigas). As SKUs herdadas ainda funcionam em ambos os modelos de implantação de gateways de VPN que já foram criados. Os gateways de VPN clássicos continuam a usar as SKUs herdadas, para os gateways existentes e para os novos gateways. Ao criar novos gateways de VPN do Resource Manager, use as novas SKUs de gateway. Para saber mais sobre os novo SKUs, veja [Sobre o Gateway de VPN](vpn-gateway-about-vpngateways.md).

## <a name="gateway-skus"></a>SKUs de gateway

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="estimated-aggregate-throughput-by-sku"></a>Taxa de transferência agregada estimada por SKU

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="supported-configurations-by-sku-and-vpn-type"></a>Configurações com suporte pelo tipo SKU e VPN

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Redimensionar um gateway (alterar uma SKU de gateway)

Você pode redimensionar um SKU de gateway dentro da mesma família de SKUs. Por exemplo, se você tiver uma SKU Standard, poderá redimensionar para uma SKU HighPerformance. Você não pode redimensionar seus gateways de VPN entre as SKUs antigas e as novas famílias de SKU. Por exemplo, você não pode ir de uma SKU padrão para uma SKU VpnGw2. 

Para redimensionar uma SKU de gateway para o modelo de implantação clássico, use o seguinte comando:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

Para redimensionar uma SKU de gateway para o modelo de implantação do Resource Manager, use o seguinte comando:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Migrar para as novas SKUs de gateway

Se você estiver trabalhando com o modelo de implantação do Resource Manager, poderá migrar para as novas SKUs do gateway. Se você estiver trabalhando com o modelo de implantação clássico, não poderá migrar para as SKUs novas e, em vez disso, deverá continuar a usar as SKUs herdadas.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre as novas SKUs do Gateway, consulte [SKUs de Gateway](vpn-gateway-about-vpngateways.md#gwsku).

Para saber mais sobre definições de configuração, veja [Sobre definições de configuração do Gateway de VPN](vpn-gateway-about-vpn-gateway-settings.md).
