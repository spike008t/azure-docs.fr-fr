---
title: Guide pratique pour utiliser redis-cli avec Cache Azure pour Redis | Microsoft Docs
description: Découvrez comment utiliser redis-cli avec le Cache Azure pour Redis.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: yegu
ms.openlocfilehash: 318d02f5da816ae8fe2fe199b9c87b3748d5d1fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66133006"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-cache-for-redis"></a>Utiliser l’outil en ligne de commande Redis avec le Cache Azure pour Redis

*redis-cli.exe* est un outil en ligne de commande populaire pour interagir avec un Cache Azure pour Redis en tant que client. Cet outil est également utilisable avec le Cache Azure pour Redis.

Si vous souhaitez utiliser l’outil pour des plateformes Windows, vous pouvez télécharger les [outils en ligne de commande Redis pour Windows](https://github.com/MSOpenTech/redis/releases/). 

Si vous souhaitez exécuter l’outil en ligne de commande sur une autre plateforme, téléchargez le Cache Azure pour Redis à partir de l’adresse [https://redis.io/download](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Collecter les informations d’accès au cache

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Vous pouvez collecter les informations nécessaires pour accéder au cache à l’aide de trois méthodes :

1. Azure CLI, en utilisant [az redis list-keys](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Azure PowerShell, en utilisant [Get-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-AzRedisCacheKey)
3. En utilisant le portail Azure

Dans cette section, vous extrairez les clés du portail Azure.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Activer l’accès pour redis-cli.exe

Avec le Cache Azure pour Redis, seul le port SSL (6380) est activé par défaut. L’outil en ligne de commande `redis-cli.exe` ne prend pas en charge SSL. Vous avez deux options de configuration pour l’utiliser :

1. [Activer le port non-SSL (6379)](cache-configure.md#access-ports) - **Cette configuration n’est pas recommandée**, car, dans cette configuration, les clés d’accès sont envoyées via TCP en texte clair. Cette modification peut compromettre l’accès à votre cache. Le seul scénario dans lequel vous pouvez envisager cette configuration est quand vous accédez simplement à un cache de test.

2. Télécharger et installer [stunnel](https://www.stunnel.org/downloads.html).

    Exécutez **stunnel GUI Start** pour démarrer le serveur.

    Dans la barre des tâches, cliquez avec le bouton droit sur l’icône du serveur stunnel, puis cliquez sur **Show Log Window** (Afficher la fenêtre de journal).

    Dans le menu stunnel Log Window (Fenêtre de journal stunnel), cliquez sur **Configuration** > **Edit Configuration** (Modifier la Configuration) pour ouvrir le fichier de configuration actuel.

    Ajoutez l’entrée suivante pour *redis-cli.exe* sous la section **Service definitions** (Définitions de service). Remplacez `yourcachename` par le nom de votre cache. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Enregistrez et fermez le fichier de configuration. 
  
    Dans le menu stunnel Log Window (Fenêtre de journal stunnel), cliquez sur **Configuration** > **Reload Configuration** (Recharger la configuration).


## <a name="connect-using-the-redis-command-line-tool"></a>Se connecter à l’aide de l’outil en ligne de commande Redis

Quand vous utilisez stunnel, exécutez *redis-cli.exe* et passez uniquement vos *port* et *clé d’accès* (principale ou secondaire) pour vous connecter au cache.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![stunnel avec redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Si vous utilisez un cache de test avec le port non-SSL **non sécurisé**, exécutez `redis-cli.exe` et passez vos *nom d’hôte*, *port* et *clé d’accès* (principale ou secondaire) pour vous connecter au cache de test.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![stunnel avec redis-cli](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur l’utilisation de la [console Redis](cache-configure.md#redis-console) pour émettre des commandes.

