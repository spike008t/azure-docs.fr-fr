---
title: Fichier Include
description: Fichier Include
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/28/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: e81b8fb06240d566e46ca0b45a0910e014dee329
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119022"
---
Le contrôle d’accès en fonction du rôle est une stratégie de sécurité basée sur l’héritage permettant de gérer les accès, les autorisations et les rôles. Les rôles descendants héritent des autorisations des rôles parents. Les autorisations peuvent également être attribuées sans être héritées d’un rôle parent. Elle peuvent aussi être attribuées pour personnaliser un rôle, si besoin.

Par exemple, l’administrateur de l’espace peut avoir besoin d’un accès global pour exécuter toutes les opérations de cet espace. L’accès comprend tous les nœuds en dessous de l’espace ou dans l’espace. Le programme d’installation de l’appareil peut nécessiter uniquement les autorisations *lire* et *mettre à jour* pour les appareils et les capteurs.

Dans tous les cas, les rôles doivent être octroyés *avec précision et sans aller au-delà de l’accès nécessaire* pour remplir leurs tâches, selon le principe des privilèges minimum. Conformément à ce principe, l’identité est accordée *uniquement* :

* L’accès nécessaire pour effectuer son travail.
* Un rôle approprié et limité à son travail.

>[!IMPORTANT]
> Suivez toujours le principe des privilèges minimum.

Deux autres pratiques importantes à suivre avec le contrôle d’accès en fonction du rôle :

> [!div class="checklist"]
> * Auditez régulièrement les attributions de rôles pour vérifier que chaque rôle dispose des autorisations appropriées.
> * Les attributions et les rôles doivent être modifiés lorsque les utilisateurs en changent.