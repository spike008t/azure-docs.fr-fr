---
title: Partage des ressources inter-origines (CORS) dans Azure Cosmos DB
description: Cet article décrit comment configurer le partage des ressources inter-origines (CORS) dans Azure Cosmos DB à l’aide du portail Azure et des modèles Azure Resource Manager.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dech
ms.openlocfilehash: 1269c4c2405e9b906b63c8a29c0de1ac217da1d7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241890"
---
# <a name="configure-cross-origin-resource-sharing-cors"></a>Configuration du partage des ressources cross-origin (CORS) 

CORS (Cross Origin Resource Sharing) est une fonctionnalité HTTP qui permet à une application web exécutée dans un domaine d’accéder aux ressources d’un autre domaine. Les navigateurs web implémentent une restriction de sécurité, appelée stratégie de même origine, qui empêche une page web d’appeler des API dans un domaine différent. CORS, cependant, constitue un moyen sûr permettant d’autoriser le domaine d’origine à appeler des API dans un autre domaine. L’API SQL de base dans Azure Cosmos DB prend désormais en charge le partage des ressources inter-origines à l’aide de l’en-tête « allowedOrigins ». Après avoir activé la prise en charge CORS pour votre compte Azure Cosmos, seules les requêtes authentifiées sont évaluées pour déterminer si elles sont autorisées selon les règles que vous avez spécifiées.

Vous pouvez configurer le paramètre CORS à partir du portail Azure ou d’un modèle Azure Resource Manager. Dans Azure Cosmos DB, l’API SQL de base prend en charge une bibliothèque JavaScript qui fonctionne dans Node.js et dans les environnements de navigateur. Cette bibliothèque peut désormais tirer parti de la prise en charge de CORS lorsque le mode passerelle est utilisé. Cette fonctionnalité ne nécessite aucune configuration côté client. Avec la prise en charge de CORS, les ressources provenant d’un navigateur peuvent accéder directement à Azure Cosmos DB via la [bibliothèque JavaScript](https://www.npmjs.com/package/@azure/cosmos), ou directement à partir de l’[API REST](https://docs.microsoft.com/rest/api/cosmos-db/) pour les opérations simples. 

## <a name="enable-cors-support-from-azure-portal"></a>Activer la prise en charge CORS à partir du portail Azure

Utilisez les étapes suivantes pour activer le partage des ressources inter-origines par le biais du portail Azure :

1. Accédez à votre compte Azure Cosmos DB. Ouvrez le panneau **CORS**.

2. Spécifiez une liste, séparée par des virgules, des origines autorisées à effectuer des appels inter-origines à votre compte Azure Cosmos DB. Par exemple, `https://www.mydomain.com`, `https://mydomain.com`, `https://api.mydomain.com`. Vous pouvez également utiliser un caractère générique « \* » pour autoriser toutes les origines, puis sélectionnez **Envoyer**. 

   > [!NOTE]
   > Pour le moment, il n’est pas possible d’utiliser des caractères génériques dans un nom de domaine. Par exemple, le format `https://*.mydomain.net` n’est pas encore pris en charge. 
   
   ![Activer le partage des ressources inter-origines à l’aide du portail Azure](./media/how-to-configure-cross-origin-resource-sharing/enable-cross-origin-resource-sharing-using-azure-portal.png)
 
## <a name="enable-cors-support-from-resource-manager-template"></a>Activer la prise en charge de CORS à partir du modèle Resource Manager

Pour activer CORS en utilisant un modèle Resource Manager, ajoutez la section « cors » avec la propriété « allowedOrigins » à un modèle existant. Le code JSON suivant est un exemple de modèle qui crée un compte Azure Cosmos avec la fonctionnalité CORS activée.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "name": "test",
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "apiVersion": "2015-04-08",
            "location": "East US 2",
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": {
                    "defaultConsistencyLevel": "Session",
                    "maxIntervalInSeconds": 5,
                    "maxStalenessPrefix": 100
                },
                "locations": [
                    {
                        "id": "test-eastus2",
                        "failoverPriority": 0,
                        "locationName": "East US 2"
                    }
                ],
                "cors": [
                    {
                        "allowedOrigins": "*"
                    }
                ]
            },
            "dependsOn": [
            ]
        }
    ]
}
```

## <a name="using-the-azure-cosmos-db-javascript-library-from-a-browser"></a>Utilisation de la bibliothèque JavaScript Azure Cosmos DB à partir d’un navigateur

Actuellement, la bibliothèque JavaScript Azure Cosmos DB n’a que la version CommonJS de la bibliothèque fournie avec son package. Pour utiliser cette bibliothèque depuis le navigateur, vous devez utiliser un outil, tel que Rollup ou Webpack, et créer une bibliothèque compatible avec ce navigateur. Certaines bibliothèques Node.js doivent avoir des simulations de navigateur pour cela. L’exemple suivant est un fichier de configuration webpack qui contient les paramètres de simulation nécessaires.

```javascript
const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  node: {
    net: "mock",
    tls: "mock"
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist")
  }
};
```
 
Voici un [exemple de code](https://github.com/christopheranderson/cosmos-browser-sample) qui utilise TypeScript et Webpack avec la bibliothèque du kit SDK JavaScript Azure Cosmos DB pour créer une application Todo qui envoie des mises à jour en temps réel lorsque de nouveaux éléments sont créés.
Une bonne pratique consiste à ne pas utiliser la clé primaire pour communiquer avec Azure Cosmos DB depuis le navigateur. Préférez l’utilisation de jetons de ressource pour communiquer. Pour plus d’informations sur les jetons de ressource, consultez l’article [Sécurisation de l’accès aux données Azure Cosmos DB](secure-access-to-data.md#resource-tokens).

## <a name="next-steps"></a>Étapes suivantes

Pour connaître les autres moyens de sécuriser votre compte Azure Cosmos, consultez les articles suivants :

* [Configurer un pare-feu pour Azure Cosmos DB](how-to-configure-firewall.md)

* [Configurer l’accès à votre compte Azure Cosmos DB à partir d’un réseau virtuel et d’un sous-réseau](how-to-configure-vnet-service-endpoint.md)
    

