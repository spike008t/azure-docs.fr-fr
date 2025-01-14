---
title: À propos des passerelles de réseau virtuel ExpressRoute - Azure | Microsoft Docs
description: En savoir plus sur les passerelles de réseau virtuel pour ExpressRoute. Cet article comprend des informations sur les types et les références SKU de passerelle.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 18615cf737eedcd188fd59d2aa98482210b9333a
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991585"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>Passerelle de réseau virtuel ExpressRoute et FastPath
Pour connecter votre réseau virtuel Azure et votre réseau local via ExpressRoute, vous devez d’abord créer une passerelle de réseau virtuel. Une passerelle de réseau virtuel a deux objectifs : exchange IP itinéraires entre les réseaux et acheminer le trafic réseau. Cet article explique les types de passerelle, les références SKU de passerelle et les performances estimé par référence (SKU). Cet article explique également ExpressRoute [chemin d’accès rapide](#fastpath), une fonctionnalité qui permet le trafic réseau à partir de votre réseau local de contourner la passerelle de réseau virtuel pour améliorer les performances.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Types de passerelle

Lorsque vous créez une passerelle de réseau virtuel, vous devez spécifier plusieurs paramètres. L’un des paramètres requis, « Type Passerelle », spécifie si la passerelle est utilisée pour le trafic ExpressRoute, ou le trafic VPN. Les types de passerelles sont les suivants:

* **Vpn** : Pour envoyer le trafic chiffré sur l’Internet public, vous utilisez le type de passerelle « Vpn ». Il s’agit alors d’une passerelle VPN. Les connexions site à site, point à site et réseau virtuel à réseau virtuel utilisent toutes une passerelle VPN.

* **ExpressRoute** : Pour envoyer le trafic réseau sur une connexion privée, vous utilisez le type de passerelle « ExpressRoute ». Ce type de passerelle est également appelé « passerelle ExpressRoute » et est utilisé lors de la configuration d'ExpressRoute.

Chaque réseau virtuel ne peut posséder qu’une seule passerelle de réseau virtuel par type de passerelle. Par exemple, une passerelle de réseau virtuel peut utiliser le type de passerelle VPN et une autre le type de passerelle ExpressRoute.

## <a name="gwsku"></a>SKU de passerelle
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Si vous souhaitez mettre à niveau votre passerelle vers une référence (SKU) de passerelle plus puissante, dans la plupart des cas, vous pouvez utiliser l’applet de commande PowerShell de « Resize-AzVirtualNetworkGateway ». Cela fonctionne pour les mises à niveau vers les références (SKU) Standard HighPerformance. Toutefois, pour mettre à niveau vers la référence (SKU) UltraPerformance, vous devez recréer la passerelle. La recréation d’une passerelle entraîne un temps d’arrêt.

### <a name="aggthroughput"></a>Performances estimées par référence (SKU) de passerelle
Le tableau ci-dessous présente les types de passerelle et les performances estimées. Cette table s’applique aux modèles de déploiement classique et Resource Manager.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Les performances de l’application dépendent de plusieurs facteurs, tels que la latence de bout en bout et le nombre de flux de trafic que l’application ouvre. Les numéros indiqués dans le tableau représentent la limite supérieure que l’application peut théoriquement atteindre dans un environnement idéal.
>
>

### <a name="zrgw"></a>Références SKU de passerelles redondantes interzone

Vous pouvez également déployer des passerelles ExpressRoute dans des zones de disponibilité Azure. Cela les sépare physiquement et logiquement dans différentes zones de disponibilité, tout en protégeant votre connectivité de réseau local à Azure des échecs au niveau de la zone.

![Passerelle ExpressRoute redondante interzone](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Les passerelles redondantes interzone utilisent de nouvelles références SKU spécifiques de passerelle pour la passerelle ExpressRoute.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Les nouvelles références SKU de passerelle prennent également en charge les autres options de déploiement pour mieux répondre à vos besoins. Lorsque vous créez une passerelle de réseau virtuel avec les nouvelles références SKU de passerelle, vous avez également la possibilité de déployer la passerelle dans une zone spécifique. Il s’agit alors d’une passerelle zonale. Lorsque vous déployez une passerelle zonale, les deux instances de la passerelle sont déployées dans la même zone de disponibilité.

## <a name="fastpath"></a>FastPath
Passerelle de réseau virtuel ExpressRoute est conçue pour échanger des itinéraires de réseau et acheminer le trafic réseau. Chemin d’accès rapide est conçu pour améliorer les performances de chemin d’accès des données entre votre réseau local et votre réseau virtuel. Lorsque l’option est activée, chemin d’accès rapide envoie le trafic réseau directement aux machines virtuelles dans le réseau virtuel, en ignorant la passerelle. 

Chemin d’accès rapide est disponible sur [ExpressRoute Direct](expressroute-erdirect-about.md) uniquement. En d’autres termes, vous pouvez activer cette fonctionnalité uniquement vous [connecter votre réseau virtuel](expressroute-howto-linkvnet-arm.md) à un circuit ExpressRoute créé sur un port Direct d’ExpressRoute. Chemin d’accès rapide nécessite toujours une passerelle de réseau virtuel doivent être créés pour échanger des itinéraires entre le réseau virtuel et le réseau local. La passerelle de réseau virtuel doit être très hautes performances ou ErGw3AZ.

Chemin d’accès rapide ne prend pas en charge les fonctionnalités suivantes :
* UDR sur le sous-réseau de passerelle : Si vous appliquez un UDR au sous-réseau de passerelle de votre réseau virtuel, le trafic réseau à partir de votre réseau local continueront à être envoyées à la passerelle de réseau virtuel.
* L’homologation de réseau virtuel : Si vous disposez d’autres réseaux virtuels homologués avec celui qui est connecté à ExpressRoute, le trafic réseau à partir de votre réseau local pour les autres réseaux virtuels (par exemple, les soi-disant « Spoke » réseaux virtuels) continueront à être envoyées sur le réseau virtuel passerelle. La solution de contournement consiste à connecter tous les réseaux virtuels au circuit ExpressRoute directement.

## <a name="resources"></a>API REST et applets de commande PowerShell
Pour accéder à des ressources techniques supplémentaires et connaître les exigences spécifiques en matière de syntaxe lors de l’utilisation d’API REST et d’applets de commande PowerShell pour les configurations de passerelles de réseau virtuel, consultez les pages suivantes :

| **Classique** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [API REST](https://msdn.microsoft.com/library/jj154113.aspx) |[API REST](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur les configurations de connexion disponibles, consultez la rubrique [Vue d’ensemble d’ExpressRoute](expressroute-introduction.md) .

Consultez [Créer une passerelle de réseau virtuel pour ExpressRoute](expressroute-howto-add-gateway-resource-manager.md) pour plus d’informations sur la création des passerelles ExpressRoute.

Pour plus d'informations sur la configuration des passerelles redondantes interzone, consultez [Créer une passerelle de réseau virtuel redondante interzone](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md).

Consultez [réseau virtuel pour ExpressRoute à liaison](expressroute-howto-linkvnet-arm.md) pour plus d’informations sur l’activation de chemin d’accès rapide. 
