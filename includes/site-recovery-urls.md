---
title: Fichier Include
description: Fichier Include
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/12/2018
ms.author: raynew
ms.openlocfilehash: f4cd9cad3813378fcdc3f06e8ab1c28eced4f93c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66116016"
---
Nom | URL commerciale | URL du gouvernement | Description
--- | --- | --- | ---
Azure Active Directory | ``login.microsoftonline.com`` | ``login.microsoftonline.us`` | Azure Active Directory utilise ces adresses pour le contrôle d’accès et la gestion des identités. 
Sauvegarde | ``*.backup.windowsazure.com`` | ``*.backup.windowsazure.us`` | Élément utilisé pour la coordination et le transfert des données de réplication.
Réplication | ``*.hypervrecoverymanager.windowsazure.com`` | ``*.hypervrecoverymanager.windowsazure.us``  | Élément utilisé pour la coordination et l’administration des opérations de gestion de la réplication.
Stockage | ``*.blob.core.windows.net`` | ``*.blob.core.usgovcloudapi.net``  | Élément utilisé pour l’accès au compte de stockage qui stocke les données répliquées.
Données de télémétrie (facultatif) | ``dc.services.visualstudio.com`` | ``dc.services.visualstudio.com`` | Adresses utilisées pour la télémétrie.
Synchronisation temporelle | ``time.windows.com`` | ``time.nist.gov`` | Adresses utilisées pour vérifier la synchronisation horaire entre l’horloge système et l’heure globale lors de tous les déploiements.


