---
title: Fichier Include
description: Fichier Include
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: c70e2c166bee14ac58ee88bd18baf0cc61702767
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66137301"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Dans Cloud Shell, créez un groupe de ressources avec la commande [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create). L’exemple suivant crée un groupe de ressources nommé *myResourceGroup* à l’emplacement *Europe Ouest*. Pour afficher tous les emplacements pris en charge pour App Service sous Linux au niveau **De base**, exécutez la commande [`az appservice list-locations --sku B1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Vous créez généralement votre groupe de ressources et les ressources dans une région proche de chez vous. 

Une fois la commande terminée, une sortie JSON affiche les propriétés du groupe de ressources.