---
title: Exemple de script Azure CLI - Créer une application web utilisant le service SignalR et l’authentification GitHub
description: Exemple de script Azure CLI - Créer une application web utilisant le service SignalR et l’authentification GitHub
author: sffamily
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.date: 04/22/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: 84020448019867744d08806acbbd47adbc1a83e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66128369"
---
# <a name="create-a-web-app-that-uses-signalr-service-and-github-authentication"></a>Créer une application web utilisant le service SignalR et l’authentification GitHub

Cet exemple de script crée une nouvelle ressource de service Azure SignalR, utilisée pour transmettre les mises à jour en temps réel de contenus aux clients. Ce script ajoute également une application web et un plan App Service pour héberger une application web ASP.NET Core qui utilise le service SignalR. L’application web est configurée avec les paramètres d’application établissant la connexion avec la nouvelle ressource de service SignalR, et l’authentification avec [GitHub](https://developer.github.com/v3/guides/basics-of-authentication/). L’application web est également configurée pour utiliser une source de déploiement du référentiel git local.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser l’interface de ligne de commande localement, vous devez exécuter Azure CLI version 2.0 ou une version ultérieure pour poursuivre la procédure décrite dans cet article. Exécutez `az --version` pour trouver la version. Si vous devez effectuer une installation ou une mise à niveau, consultez [Installer Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exemple de script

Ce script utilise l’extension *signalr* pour Azure CLI. Exécutez la commande suivante afin d’installer l’extension *signalr* pour l’interface Azure CLI avant d’utiliser cet exemple de script :

```azurecli-interactive
az extension add -n signalr
```

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-with-app-service-github-oauth/create-signalr-with-app-service-github-oauth.sh "Create a new SignalR Service and Web App configured to use SignalR, GitHub OAuth, and local Git repository deployment source.")]

Consignez le nom généré pour le nouveau groupe de ressources. Il est affiché dans la sortie. Vous utiliserez ce nom de groupe de ressources au moment de la suppression de l’ensemble des ressources du groupe.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Chaque commande du tableau renvoie à une documentation spécifique. Ce script utilise les commandes suivantes :

| Commande | Notes |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az signalr create](/cli/azure/ext/signalr/signalr#ext-signalr-az-signalr-create) | Crée une ressource Azure SignalR Service. |
| [az signalr key list](/cli/azure/ext/signalr/signalr/key#ext-signalr-az-signalr-key-list) | Répertorie les clés qui seront utilisées par votre application lors de la transmission des mises à jour en temps réel de contenus avec SignalR. |
| [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) | Crée un plan Azure App Service pour l’hébergement des applications web. |
| [az webapp create](/cli/azure/webapp#az-webapp-create) | Crée une application web Azure utilisant le plan d’hébergement App Service. |
| [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) | Ajoute de nouveaux paramètres pour l’application web. Ces paramètres d’application sont utilisés pour stocker la chaîne de connexion SignalR et les secrets d’application GitHub OAuth. |
| [az webapp deployment user set](/cli/azure/webapp/deployment/user#az-webapp-deployment-user-set) | Met à jour les informations d’identification du déploiement. |
| [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#az-webapp-deployment-source-config-local-git) | Récupère une URL pour un point de terminaison de référentiel Git à cloner et à établir comme instance de réception pour le déploiement d’application web. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).

Des exemples supplémentaires de script de l’interface CLI du service Azure SignalR sont disponibles dans la [documentation sur le service Azure SignalR](../signalr-reference-cli.md).
