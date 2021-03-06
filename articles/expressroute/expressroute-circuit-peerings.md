---
title: "Circuitos do Azure ExpressRoute e domínios de roteamento | Microsoft Docs"
description: "Esta página apresenta uma visão geral dos circuitos da Rota Expressa e dos domínios de roteamento."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6f0c5d8e-cc60-4a04-8641-2c211bda93d9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 3742fb37016db5e6230bc2b7ce11f0270d9d58b3
ms.contentlocale: pt-br
ms.lasthandoff: 08/01/2017

---
# <a name="expressroute-circuits-and-routing-domains"></a>Circuitos da Rota Expressa e domínios de roteamento
 Você deve solicitar um *circuito da Rota Expressa* para conectar sua infraestrutura local à Microsoft por meio de um provedor de conectividade. A figura abaixo fornece uma representação lógica de conectividade entre sua WAN e a Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>Circuitos da Rota Expressa
Um *circuito da Rota Expressa* representa uma conexão lógica entre a infraestrutura local e os serviços de nuvem da Microsoft por meio de um provedor de conectividade. É possível solicitar vários circuitos da Rota Expressa. Os circuitos podem estar na mesma região ou em regiões diferentes, bem como podem ser conectados aos seus locais por meio de diferentes provedores de conectividade. 

Os circuitos da Rota Expressa não são mapeados para entidades físicas. Um circuito é identificado exclusivamente por um GUID padrão chamado de chave de serviço (s-key). A chave de serviço é a única informação trocada entre a Microsoft, o provedor de conectividade e você. A chave-s não é um segredo para fins de segurança. Há um mapeamento de 1:1 entre um circuito da Rota Expressa e a chave-s.

Um circuito da Rota Expressa pode ter até três emparelhamentos independentes: público do Azure, privado do Azure e da Microsoft. Cada emparelhamento é um par de sessões BGP independente, cada uma delas configurada de modo redundante para alta disponibilidade. Há um mapeamento de 1:N (1 <= N <= 3) entre um circuito da Rota Expressa e os domínios de roteamento. Um circuito da Rota Expressa pode ter qualquer tipo de emparelhamento (um, dois ou os três) habilitado por circuito da Rota Expressa.

Cada circuito tem uma largura de banda fixa (50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 10 Gbps) e é mapeado para um provedor de conectividade e um local de emparelhamento. A largura de banda selecionada é compartilhada entre todos os emparelhamentos para o circuito. 

### <a name="quotas-limits-and-limitations"></a>Cotas, limites e limitações
Cotas e limites padrão aplicam-se a todos os circuitos da Rota Expressa. Consulte a página [Limites, cotas e restrições de serviço e assinatura do Azure](../azure-subscription-service-limits.md) para obter informações atualizadas sobre cotas.

## <a name="expressroute-routing-domains"></a>Domínios de roteamento da Rota Expressa
Um circuito da Rota Expressa tem vários domínios de roteamento associados a ele: público do Azure, privado do Azure e Microsoft. Cada um dos domínios de roteamento é configurado de modo idêntico em um par de roteadores (na configuração ativo-ativo ou de compartilhamento de carga) para alta disponibilidade. Os serviços do Azure estão categorizados como *público do Azure* e *privado do Azure* para representar os esquemas de endereçamento IP.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="private-peering"></a>Emparelhamento privado
Os serviços de computação do Azure, isto é, máquinas virtuais (IaaS) e serviços de nuvem (PaaS), implantados em uma rede virtual podem ser conectados por meio do domínio de emparelhamento privado. O domínio de emparelhamento privado é considerado uma extensão confiável de sua rede principal para o Microsoft Azure. Você pode configurar a conectividade bidirecional entre sua rede principal e as redes virtuais (VNets) do Azure. Esse emparelhamento permite a você se conectar a máquinas virtuais e serviços de nuvem diretamente em seus endereços IP privados.  

Você pode conectar mais de uma rede virtual ao domínio de emparelhamento privado. Examine a [Página de perguntas Frequentes](expressroute-faqs.md) para obter informações sobre limites e limitações. Você pode visitar a página [Limites, cotas e restrições de serviço e assinatura do Azure](../azure-subscription-service-limits.md) para obter informações atualizadas sobre limites.  Consulte a página [Roteamento](expressroute-routing.md) para obter informações detalhadas sobre a configuração de roteamento.

### <a name="public-peering"></a>Emparelhamento público
Serviços como o Armazenamento do Azure, Sites e Bancos de dados SQL são oferecidos em endereços IP públicos. Você pode se conectar de modo privado a serviços hospedados em endereços IP públicos (incluindo VIPs de seus serviços de nuvem) por meio do domínio de roteamento de emparelhamento público. É possível conectar o domínio de emparelhamento público à sua DMZ e a todos os serviços do Azure em seus endereços IP públicos de sua WAN sem precisar se conectar pela Internet. 

A conectividade é sempre iniciada por meio de sua WAN para serviços do Microsoft Azure. Os serviços do Microsoft Azure não poderão iniciar conexões a sua rede por meio desse domínio de roteamento. Após o emparelhamento público ser habilitado, você poderá se conectar a todos os serviços do Azure. Não permitimos que você escolha seletivamente os serviços para os quais podemos anunciar rotas. Você pode examinar a lista de prefixos que anunciamos por meio desse emparelhamento na página [Intervalos de IPs do datacenter do Microsoft Azure](http://www.microsoft.com/download/details.aspx?id=41653) . A página é atualizada semanalmente.

Você pode definir filtros de rota personalizados dentro da sua rede para consumir apenas as rotas que você precisa. Consulte a página [Roteamento](expressroute-routing.md) para obter informações detalhadas sobre a configuração de roteamento. 

Consulte a [página de perguntas frequentes](expressroute-faqs.md) para saber mais sobre serviços com suporte por meio do domínio de roteamento de emparelhamento público. 

### <a name="microsoft-peering"></a>Emparelhamento da Microsoft
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

A conectividade com todos os outros serviços online da Microsoft (como serviços do Office 365) será por emparelhamento da Microsoft. Habilitamos a conectividade bidirecional entre sua WAN e os serviços de nuvem da Microsoft por meio do domínio de roteamento de emparelhamento da Microsoft. Você deve se conectar aos serviços de nuvem da Microsoft somente por endereços IP públicos que pertençam a você ou ao seu provedor de conectividade e deve seguir todas as regras definidas. Consulte a página [Pré-requisitos da Rota Expressa](expressroute-prerequisites.md) para obter mais informações.

Consulte a [página de perguntas frequentes](expressroute-faqs.md) para obter mais informações sobre serviços com suporte, custos e detalhes de configuração. Consulte a página [Locais da Rota Expressa](expressroute-locations.md) para saber mais sobre a lista de provedores de conectividade que dão suporte ao emparelhamento da Microsoft.

## <a name="routing-domain-comparison"></a>Comparação de domínios de roteamento
A tabela abaixo compara os três domínios de roteamento.

|  | **Emparelhamento privado** | **Emparelhamento público** | **Emparelhamento da Microsoft** |
| --- | --- | --- | --- |
| **Número máximo de prefixos com suporte por emparelhamento** |4000 por padrão, 10.000 com a Rota Expressa Premium |200 |200 |
| **Intervalos de endereços IP com suporte** |Todos os endereços IPv4 válidos em sua WAN. |Os endereços IPv4 públicos pertencentes a você ou ao seu provedor de conectividade. |Os endereços IPv4 públicos pertencentes a você ou ao seu provedor de conectividade. |
| **Requisitos do número do AS** |Números públicos e privados do AS. Você deve possuir número público do AS, se você optar por usar um. |Números públicos e privados do AS. No entanto, você deve comprovar a propriedade de endereços IP públicos. |Números públicos e privados do AS. No entanto, você deve comprovar a propriedade de endereços IP públicos. |
| **Roteando endereços IP de interface** |RFC1918 e endereços IP públicos |Endereços IP públicos registrados para você em registros de roteamento. |Endereços IP públicos registrados para você em registros de roteamento. |
| **Suporte a Hash MD5** |Sim |Sim |Sim |

Você pode optar por habilitar um ou mais domínios de roteamento como parte do seu circuito ExpressRoute. Também é possível optar por ter todos os domínios de roteamento na mesma VPN se você desejar combiná-los em um único domínio de roteamento. Você também pode colocá-los em diferentes domínios de roteamento, da mesma forma que no diagrama. A configuração recomendada é conectar o emparelhamento privado diretamente à rede principal, enquanto os vínculos de emparelhamento público e da Microsoft são conectados à sua DMZ.

Se você optar por ter todas as três sessões de emparelhamento, você deve ter três pares de sessões BGP (um par para cada tipo de emparelhamento). Os pares de sessões BGP fornecem um link altamente disponível. Se estiver conectando por meio de provedores de conectividade da camada 2, você será responsável por configurar e gerenciar o roteamento. Saiba mais analisando os [fluxos de trabalho](expressroute-workflows.md) para configurar a Rota Expressa.

## <a name="next-steps"></a>Próximas etapas
* Encontrar um provedor de serviços. Consulte [Locais e provedores de serviços da Rota Expressa](expressroute-locations.md).
* Verifique se todos os pré-requisitos foram atendidos. Consulte [Pré-requisitos da Rota Expressa](expressroute-prerequisites.md).
* Configurar sua conexão da Rota Expressa.
  * [Criar e gerenciar circuitos de ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Configurar o roteamento (emparelhamento) para circuitos ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)


