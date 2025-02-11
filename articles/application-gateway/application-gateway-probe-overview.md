---
title: Vue d’ensemble de l’analyse de l’intégrité pour Azure Application Gateway
description: En savoir plus sur les capacités d’analyse dans Azure Application Gateway
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
origin.date: 08/06/2018
ms.date: 04/16/2019
ms.author: v-junlch
ms.openlocfilehash: d0c425bcb9961fde9fb319991148c18c6a9ff57b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66135216"
---
# <a name="application-gateway-health-monitoring-overview"></a>Vue d’ensemble de l’analyse d’intégrité Application Gateway

Azure Application Gateway analyse par défaut l’intégrité de toutes les ressources de son pool principal et supprime automatiquement du pool les ressources considérées comme défectueuses. Application Gateway continue de surveiller les instances défaillantes et les réintroduit dans le pool principal intègre une fois qu’elles redeviennent disponibles et répondent aux sondes d’intégrité. La passerelle d’application envoie les sondes d’intégrité avec le même port que celui défini dans les paramètres HTTP du serveur principal. Cette configuration garantit que la sonde teste le même port que celui qu’utiliseraient les clients pour se connecter au serveur principal.

![exemple de sonde application gateway][1]

En plus d’utiliser la surveillance par sonde d’intégrité par défaut, vous pouvez aussi personnaliser la sonde d’intégrité pour répondre aux exigences de votre application. Dans cet article, nous nous intéressons aux sondes d’intégrité par défaut et personnalisées.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-health-probe"></a>Sonde d’intégrité par défaut

Une passerelle d’application configure automatiquement une sonde d’intégrité par défaut lorsque vous ne définissez pas de configuration de sonde personnalisée. Le comportement d’analyse par défaut consiste à lancer une requête HTTP aux adresses IP configurées pour le pool principal. En ce qui concerne les sondes par défaut, si les paramètres HTTP du serveur principal sont configurés pour HTTPS, la sonde utilise également HTTPS pour tester l’intégrité des serveurs principaux.

Exemple : Vous configurez votre passerelle d’application de manière à utiliser les serveurs principaux A, B et C, qui recevront le trafic réseau HTTP sur le port 80. Les contrôles de défaillance par défaut testent les trois serveurs toutes les 30 secondes pour obtenir une réponse HTTP correcte. Le [code d’état](https://msdn.microsoft.com/library/aa287675.aspx) d’une réponse HTTP correcte est compris entre 200 et 399.

Si l’analyse de la sonde par défaut échoue pour le serveur A, la passerelle d’application le retire de son pool principal et le trafic réseau cesse de passer par ce serveur. La sonde par défaut continue de contrôler le serveur A toutes les 30 secondes. Dès que le serveur A répond avec succès à une requête de la sonde d’intégrité par défaut, il est réintroduit dans le pool principal en tant que serveur intègre et le trafic vers celui-ci reprend.

### <a name="probe-matching"></a>Correspondance des sondes

Par défaut, une réponse HTTP (S) avec le code d’état compris entre 200 et 399 est considéré comme saine. Les sondes d’intégrité personnalisées prennent également en charge deux critères de correspondance. Les critères de correspondance peuvent être utilisés pour éventuellement modifier l’interprétation par défaut de ce qui constitue une réponse intègre.

Les éléments suivants sont des critères de correspondance : 

- **Correspondance de code d’état de la réponse HTTP** : le critère de correspondance de la sonde pour l’acceptation du code de réponse http spécifié par l’utilisateur ou des plages de codes de réponse. Les codes d’état de réponse séparées par des virgules individuelles ou une plage de codes d’état sont pris en charge.
- **Correspondance du corps de la réponse HTTP** : critère de correspondance de la sonde qui examine le corps de la réponse HTTP et correspond à une chaîne spécifiée par l’utilisateur. La correspondance ne tient compte que de la présence d’une chaîne spécifiée par l’utilisateur dans le corps de la réponse et n’est pas une correspondance d’expression régulière complète.

Les critères de correspondance peuvent être spécifiés à l’aide de la cmdlet `New-AzApplicationGatewayProbeHealthResponseMatch`.

Exemple :

```azurepowershell
$match = New-AzApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Une fois les critères de correspondance spécifiés, ils peuvent être joints à la configuration de la sonde à l’aide d’un `-Match` paramètre dans PowerShell.

### <a name="default-health-probe-settings"></a>Paramètres de sonde d’intégrité par défaut

| Propriétés de la sonde | Valeur | Description  |
| --- | --- | --- |
| URL de la sonde |http://127.0.0.1:\<port\>/ |Chemin d'accès de l'URL |
| Interval |30 |Durée de l’attente, en secondes, avant l’envoi de la sonde d’intégrité suivante.|
| Délai d’attente |30 |Durée de l’attente, en secondes, de la passerelle d’application pour une réponse de la sonde avant que la sonde ne soit déclarée comme défectueuse. Si une sonde renvoie un état intègre, le serveur principal correspondant est immédiatement marqué comme étant intègre.|
| Seuil de défaillance sur le plan de l'intégrité |3 |Détermine le nombre de sondes à envoyer en cas d’échec de la sonde d’intégrité standard. Ces sondes d’intégrité supplémentaires sont envoyées de façon rapprochée pour déterminer rapidement l’intégrité du serveur principal et ne tiennent pas compte de l’intervalle d’analyse. Le serveur principal est marqué comme étant défectueux après que le nombre d’échecs consécutifs a atteint le seuil de défaillance. |

> [!NOTE]
> Le port est le même que celui utilisé par les paramètres HTTP du serveur principal.

La sonde par défaut examine uniquement http :\//127.0.0.1 :\<port\> pour déterminer l’état d’intégrité. Si vous devez configurer la sonde d’intégrité de sorte qu’elle accède à une URL personnalisée ou modifier d’autres paramètres, vous devez utiliser des sondes personnalisées.

### <a name="probe-intervals"></a>Intervalles d'analyse

Toutes les instances d’Application Gateway sondent le serveur principal indépendamment des autres. La même configuration de sonde s’applique à chaque instance d’Application Gateway. Par exemple, si la configuration de sonde consiste à envoyer des sondes d’intégrité toutes les 30 secondes, et si Application Gateway a deux instances, les deux instances envoient la sonde d’intégrité toutes les 30 secondes.

S’il existe plusieurs processus d’écoute, chacun d’entre eux analyse le serveur principal indépendamment des autres. Par exemple, s’il existe deux processus d’écoute pointant vers le même pool principal sur deux ports distincts (configurés par deux paramètres HTTP du serveur principal), chaque processus d’écoute analyse le même serveur principal indépendamment. Dans ce cas, il existe deux sondes provenant de chaque instance d’Application Gateway pour les deux processus d’écoute. S’il existe deux instances d’Application Gateway dans ce scénario, la machine virtuelle principale voit quatre sondes par intervalle d’analyse configuré.

## <a name="custom-health-probe"></a>Sonde d’intégrité personnalisée

Les sondes personnalisées vous permettent d’avoir un contrôle plus précis de l’analyse de l’intégrité. En utilisant des sondes personnalisées, vous pouvez configurer l’intervalle d’analyse, l’URL et le chemin à tester et le nombre de réponses en échec autorisé avant que l’instance de pool principal soit marquée comme étant défectueuse.

### <a name="custom-health-probe-settings"></a>Paramètres de sonde d’intégrité personnalisée

Le tableau suivant fournit des définitions pour les propriétés d’une sonde d’intégrité personnalisée.

| Propriétés de la sonde | Description  |
| --- | --- |
| Nom |Nom de la sonde. Ce nom est utilisé pour désigner la sonde dans les paramètres HTTP du serveur principal. |
| Protocol |Protocole utilisé pour envoyer la sonde. La sonde utilise le protocole défini dans les paramètres HTTP du serveur principal |
| Host |Nom d’hôte pour l’envoi de la sonde. S’applique uniquement lorsque plusieurs sites sont configurés sur Application Gateway, sinon utilisez '127.0.0.1'. Cette valeur est différente du nom d’hôte de la machine virtuelle. |
| path |Chemin relatif de la sonde. Le chemin valide commence par « / ». |
| Interval |Intervalle d’analyse en secondes. Cette valeur est l’intervalle de temps qui s’écoule entre deux analyses consécutives. |
| Délai d’attente |Délai d’expiration de l’analyse en secondes. Si aucune réponse valide n’est reçue dans le délai imparti, la sonde est marquée comme étant en échec.  |
| Seuil de défaillance sur le plan de l'intégrité |Nombre de tentatives d’analyse Le serveur principal est marqué comme étant défectueux après que le nombre d’échecs consécutifs a atteint le seuil de défaillance. |

> [!IMPORTANT]
> Si Application Gateway est configuré pour un seul site, le nom d’hôte par défaut doit être spécifié sous la forme « 127.0.0.1 », sauf s’il est configuré d’une autre manière dans la sonde personnalisée.
> Pour référence, une sonde personnalisée est envoyée à \<protocole\>://\<hôte\>:\<port\>\<chemin d’accès\>. Le port utilisé est identique à celui défini dans les paramètres HTTP du serveur principal.

## <a name="nsg-considerations"></a>Considérations pour un groupe de sécurité réseau

Si le sous-réseau Application Gateway comporte un groupe de sécurité réseau (NSG), vous devez ouvrir la plage de ports 65503-65534 sur ce sous-réseau pour permettre l’arrivée du trafic entrant. Ces ports sont requis pour permettre à l’API relative à l’intégrité du serveur principal de fonctionner correctement.

En outre, la connectivité Internet sortante ne peut pas être bloquée, et le trafic entrant provenant de la balise AzureLoadBalancer doit être autorisé.

## <a name="next-steps"></a>Étapes suivantes
Après vous être familiarisé avec l’analyse d’intégrité Application Gateway, vous pouvez configurer une [sonde d’intégrité personnalisée](application-gateway-create-probe-portal.md) dans le portail Azure ou une [sonde d’intégrité personnalisée](application-gateway-create-probe-ps.md) à l’aide de PowerShell et du modèle de déploiement Azure Resource Manager.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png

<!-- Update_Description: wording update -->