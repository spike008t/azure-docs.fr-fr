---
title: Fichier Include
description: Fichier Include
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170856"
---
Les itinéraires système permettent la circulation automatique du trafic lié au déploiement, mais il existe des cas où il peut s’avérer utile d’acheminer les paquets au moyen d’une appliance virtuelle. Pour ce faire, créez des itinéraires personnalisés qui redirigent le prochain tronçon de paquets circulant vers un sous-réseau spécifique vers votre appliance virtuelle, et activer le transfert d’adresses IP pour la machine virtuelle exécutée en tant qu’appliance virtuelle.

Voici certaines situations dans lesquelles des équipements virtuels peuvent être utilisés :

* Surveillance du trafic avec un système de détection d’intrusion
* Contrôle du trafic avec un pare-feu

Pour plus d’informations sur les itinéraires définis par l’utilisateur et sur le transfert IP, consultez [Itinéraires définis par l’utilisateur et transfert IP](../articles/virtual-network/virtual-networks-udr-overview.md).

