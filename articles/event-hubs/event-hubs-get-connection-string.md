---
title: Obtenir une chaîne de connexion - Azure Event Hubs | Microsoft Docs
description: Cet article fournit des instructions pour obtenir une chaîne de connexion utilisable par les clients pour se connecter à Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: edd197fb6d578df064c67a422767e3e70a0c8142
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66158902"
---
# <a name="get-an-event-hubs-connection-string"></a>Obtenir une chaîne de connexion Event Hubs

Pour utiliser Event Hubs, vous devez créer un espace de noms Event Hubs. Un espace de noms est un conteneur d’étendue pour plusieurs hubs d’événements ou rubriques Kafka. Cet espace de noms vous donne une valeur unique de [nom de domaine complet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Une fois l’espace de noms créé, vous pouvez obtenir la chaîne de connexion requise pour communiquer avec Event Hubs.

La chaîne de connexion pour Azure Event Hubs se compose des éléments suivants :

* FQDN = nom de domaine complet de l’espace de noms EventHubs que vous avez créé (cela inclut l’espace de noms EventHubs suivi de servicebus.windows.net)
* SharedAccessKeyName = le nom que vous avez choisi pour les clés de signature d’accès partagé de votre application
* SharedAccessKey = la valeur de la clé générée.

Le modèle de chaîne de connexion ressemble à ce qui suit :
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Un exemple de chaîne de connexion peut se présenter comme `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

Cet article vous guide dans les différentes façons d’obtenir la chaîne de connexion.

## <a name="get-connection-string-from-the-portal"></a>Obtenir la chaîne de connexion dans le portail
1. Connectez-vous au [portail Azure](https://portal.azure.com). 
2. Dans le menu de navigation de gauche, sélectionnez **Tous les services**. 
3. Sélectionnez **Event Hubs** dans la section **Analytics**. 
4. Dans la liste de hubs d’événements, sélectionnez votre Event Hub.
6. Sur la page **Espace de noms Event Hubs**, cliquez sur **Stratégies d’accès partagé** dans le menu de gauche.

    ![Élément de menu des stratégies d’accès partagées](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)
7. Sélectionnez une **stratégie d’accès partagé** dans la liste des stratégies. Celle par défaut s’appelle : **RootManageSharedAccessPolicy**. Vous pouvez ajouter une stratégie avec les autorisations appropriées (lecture, écriture) et utiliser cette stratégie. 

    ![Stratégies d’accès partagé Event Hubs](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)
8. Cliquez sur le bouton **copier** à côté du champ **Clé primaire de la chaîne de connexion**. 

    ![Obtenir la chaîne de connexion Event Hubs](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Obtention de la chaîne de connexion avec Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Vous pouvez utiliser [Get-AzEventHubNamespaceKey](/powershell/module/az.eventhub/get-azeventhubkey) pour obtenir la chaîne de connexion pour la stratégie ou le nom de règle spécifié, comme indiqué ci-dessous :

```azurepowershell-interactive
Get-AzEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

## <a name="getting-the-connection-string-with-azure-cli"></a>Obtention de la chaîne de connexion avec Azure CLI
Vous pouvez utiliser ce qui suit pour obtenir la chaîne de connexion de l’espace de noms :

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Pour plus d’informations sur les commandes Azure CLI pour Event Hubs, consultez [Azure CLI pour Event Hubs](/cli/azure/eventhubs).

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez en apprendre plus sur Event Hubs en consultant les liens suivants :

* [Vue d’ensemble d’Event Hubs](event-hubs-what-is-event-hubs.md)
* [Créer un concentrateur d’événements](event-hubs-create.md)
