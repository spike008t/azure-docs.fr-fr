---
title: Créer une fonction déclenchée via HTTP dans Azure
description: Découvrez comment créer votre première fonction Python dans Azure à l’aide d’Azure CLI et d’Azure Functions Core Tools.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 88e8d543e8bc3c3ae07133ec333b33c4486c2684
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65864514"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Créer une fonction déclenchée via HTTP dans Azure

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

Cet article explique comment utiliser des outils en ligne de commande pour créer un projet Python s’exécutant dans Azure Functions. La fonction que vous créez est déclenchée par des requêtes HTTP. Enfin, vous publiez votre projet de sorte qu’il s’exécute en tant que [fonction serverless](functions-scale.md#consumption-plan) dans Azure.

Cet article est le premier des deux guides de démarrage rapide pour Azure Functions. Après avoir terminé cet article, vous allez [ajouter une liaison de sortie de file d’attente Stockage Azure](functions-add-output-binding-storage-queue-python.md) à votre fonction.

## <a name="prerequisites"></a>Prérequis

Avant de commencer, vous devez disposer des éléments suivants :

+ Installer [Python 3.6](https://www.python.org/downloads/).

+ [Azure Functions Core Tools](./functions-run-local.md#v2) version 2.6.666 ou ultérieure.

+ Installez [Azure CLI](/cli/azure/install-azure-cli) version 2.x ou ultérieure.

+ Un abonnement Azure actif.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Créer et activer un environnement virtuel

Pour pouvoir développer et tester localement des fonctions Python, vous devez travailler dans un environnement Python 3.6. Exécutez les commandes suivantes pour créer et activer un environnement virtuel nommé `.env`.

### <a name="bash-or-a-terminal-window"></a>Bash ou une fenêtre de terminal :

```bash
python3.6 -m venv .env
source .env/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell ou une invite de commandes Windows :

```powershell
py -3.6 -m venv .env
.env\scripts\activate
```

Les commandes restantes sont exécutées à l’intérieur de l’environnement virtuel.

## <a name="create-a-local-functions-project"></a>Créer un projet Functions local

Un projet Functions est l’équivalent d’une application de fonction dans Azure. Il peut contenir plusieurs fonctions qui partagent les mêmes configurations locale et d’hébergement.

Dans l’environnement virtuel, exécutez la commande suivante en choisissant **python** comme runtime de travail.

```command
func init MyFunctionProj
```

Un dossier nommé _MyFunctionProj_ est créé, qui contient les trois fichiers suivants :

* `local.settings.json` est utilisé pour stocker les paramètres d’application et les chaînes de connexion lors de l’exécution localement. Ce fichier n’est pas publié sur Azure.
* `requirements.txt` contient la liste des packages à installer lors de la publication sur Azure.
* `host.json` contient les options de configuration globale qui affectent toutes les fonctions d’une application de fonction. Ce fichier est publié sur Azure.

Accédez au nouveau dossier MyFunctionProj :

```command
cd MyFunctionProj
```

Ensuite, vous mettez à jour le fichier host.json pour activer les offres groupées d’extension.  

## <a name="reference-bindings"></a>Liaisons de référence

Les offres groupées d’extension facilitent l’ajout d’extensions de liaison par la suite. Elles éliminent également la nécessité d’installer le Kit de développement logiciel (SDK) .NET Core 2.x. Les offres groupées d’extension nécessitent Core Tools version 2.6.1071 ou ultérieure. 

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Maintenant, vous pouvez ajouter une fonction à votre projet.

## <a name="create-a-function"></a>Créer une fonction

Pour ajouter une fonction à votre projet, exécutez la commande suivante :

```command
func new
```

Choisissez le modèle **déclencheur HTTP**, tapez `HttpTrigger` comme nom de la fonction, puis appuyez sur Entrée.

Un sous-dossier nommé _HttpTrigger_ est créé, qui contient les fichiers suivants :

* **function.JSON** : fichier de configuration qui définit la fonction, le déclencheur et d’autres liaisons. L’examen de ce fichier révèle que la valeur de `scriptFile` pointe sur le fichier contenant la fonction, tandis que le déclencheur d’invocation et les liaisons sont définis dans le tableau `bindings`.

  Chaque liaison requiert une direction, un type et un nom unique. Le déclencheur HTTP comporte une liaison d’entrée de type [`httpTrigger`](functions-bindings-http-webhook.md#trigger) et une liaison de sortie de type [`http`](functions-bindings-http-webhook.md#output).

* **\_\_init\_\_.py** : fichier de script qui est votre fonction déclenchée via HTTP. Vous pouvez voir que ce script contient `main()` par défaut. Les données HTTP du déclencheur sont transmises à cette fonction à l’aide du paramètre de liaison nommé `req`. `req` est une instance de la [classe azure.functions.HttpRequest](/python/api/azure-functions/azure.functions.httprequest) définie dans function.json. 

    L’objet renvoyé, défini comme `$return` dans function.json, est une instance de la [classe azure.functions.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse). Pour en savoir plus, voir [Déclencheurs et liaisons HTTP Azure Functions](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>Exécuter la fonction localement

La commande suivante démarre l’application de fonction qui s’exécute localement à l’aide du même runtime Azure Functions que dans Azure.

```bash
func host start
```

Lorsque l’hôte Functions démarre, il écrit une sortie similaire à la suivante, que nous avons tronquée pour une meilleure lisibilité :

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Copiez l’URL de votre fonction `HttpTrigger` à partir de la sortie du runtime et collez-la dans la barre d’adresses de votre navigateur. Ajoutez la chaîne de requête `?name=<yourname>` à cette URL et exécutez la demande. Ce qui suit montre la réponse dans le navigateur à la requête GET retournée par la fonction locale :

![Tester localement dans le navigateur](./media/functions-create-first-function-python/function-test-local-browser.png)

Maintenant que vous avez exécuté votre fonction localement, vous pouvez créer l’application de fonction et d’autres ressources nécessaires dans Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Créer une application de fonction dans Azure

Une application de fonction fournit un environnement pour l’exécution du code de votre fonction. Elle vous permet de regrouper les fonctions en une unité logique pour faciliter la gestion, le déploiement et le partage des ressources.

Exécutez la commande suivante en spécifiant un nom d’application de fonction unique à la place de l’espace réservé `<APP_NAME>`, et le nom du compte de stockage pour `<STORAGE_NAME>`. `<APP_NAME>` représente également le domaine DNS par défaut pour l’application de fonction. Ce nom doit être unique parmi toutes les applications dans Azure.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

> [!NOTE]
> Les applications Linux et Windows ne peut pas être hébergées dans le même groupe de ressources. Si vous disposez d’un groupe de ressources existant nommé `myResourceGroup` avec une application de fonction Windows ou une application web, vous devez utiliser un autre groupe de ressources.

Vous êtes désormais prêt à publier votre projet de fonctions local sur l’application de fonction dans Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="next-steps"></a>Étapes suivantes

Vous avez créé un projet de fonctions Python avec une fonction déclenchée via HTTP, l’avez exécuté sur votre ordinateur local et l’avez déployé sur Azure. À présent, étendez votre fonction en effectuant un...

> [!div class="nextstepaction"]
> [Ajout de liaison de sortie de file d’attente Stockage Azure](functions-add-output-binding-storage-queue-python.md)
