---
title: Fichier Include
description: Fichier Include
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 04/09/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: eae4b1e919270413d4ca6627964fad9c7f5bd9bf
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151978"
---
1. Utilisez la `dps-keygen` utilitaire de ligne de commande pour générer une chaîne de connexion :

    Pour installer le [utilitaire du Générateur de clé](https://github.com/Azure/dps-keygen), exécutez la commande suivante :

    ```cmd/sh
    npm i -g dps-keygen
    ```

1. Pour générer une chaîne de connexion, exécutez la commande suivante en utilisant les informations de connexion notée précédemment :

    ```cmd/sh
    dps-keygen -di:<Device ID> -dk:<Primary or Secondary Key> -si:<Scope ID>
    ```

1. Copiez la chaîne de connexion à partir de la `dps-keygen` sortie à utiliser dans votre code de l’appareil.