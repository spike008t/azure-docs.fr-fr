---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 5bde217601d27129e044b64d90184727ea717950
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132878"
---
Azure Resource Manager vous permet de définir des paramètres pour les valeurs que vous voulez spécifier lorsque le modèle est déployé. Ce modèle inclut une section appelée Paramètres, qui contient toutes les valeurs de paramètres.
Vous devez définir un paramètre pour les valeurs qui varient selon le projet que vous déployez, ou de l’environnement dans lequel vous effectuez le déploiement. Ne définissez pas de paramètres pour les valeurs qui restent identiques. Chaque valeur de paramètre est utilisée dans le modèle pour définir les ressources déployées. 

Lorsque vous définissez les paramètres, utilisez le champ **allowedValues** pour spécifier les valeurs qu'un utilisateur peut fournir au cours du déploiement. Utilisez le champ **defaultValue** pour affecter une valeur au paramètre, si aucune valeur n'est fournie pendant le déploiement.

Nous allons décrire chaque paramètre du modèle.

### <a name="sitename"></a>siteName
Le nom de l'application web que vous souhaitez créer.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Le nom du plan App Service à utiliser pour héberger l'application web.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>référence (SKU)
Le niveau de tarification du plan d'hébergement.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Le modèle définit les valeurs autorisées pour ce paramètre et affecte une valeur par défaut (S1) si aucune valeur n’est spécifiée.

### <a name="workersize"></a>workerSize
La taille d'instance du plan d'hébergement (petite, moyenne ou grande).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Le modèle définit les valeurs autorisées pour ce paramètre (0, 1 ou 2) et affecte une valeur par défaut (0) si aucune valeur n'est spécifiée. Les valeurs correspondent à une taille petite, moyenne et grande.

