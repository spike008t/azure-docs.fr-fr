---
title: Questions fréquentes (FAQ) sur l’accélérateur de solution de surveillance à distance | Microsoft Docs
description: Questions fréquentes (FAQ) sur l’accélérateur de solution de surveillance à distance
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: a65f2a90aedf5aa50016a6c5e5019094f202bf55
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2018
---
# <a name="frequently-asked-questions-for-remote-monitoring-solution-accelerator"></a>Questions fréquentes (FAQ) sur l’accélérateur de solution de surveillance à distance

Voir aussi le [FAQ](iot-accelerators-faq.md) général.

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Combien coûte l’approvisionnement de la nouvelle solution de surveillance à distance ?

Le nouvel accélérateur de solution propose deux options de déploiement :

* Une option *de base* conçue pour les développeurs à la recherche d’un coût de développement moindre ou de clients souhaitant créer une démonstration ou une preuve de concept.
* Une option *standard* conçue pour les entreprises souhaitant déployer une infrastructure prête à l’emploi dans un environnement de production.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Comment maintenir des coûts faibles pendant le développement de ma solution ?

En plus des deux déploiements différenciés, la nouvelle solution de surveillance à distance dispose d’un paramètre pour activer ou désactiver tous les appareils simulés à la demande. La désactivation de la simulation réduit les données ingérées par la solution et, par conséquent, le coût global.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Quelle est la différence entre les options de déploiement de base et standard ? Comment choisir entre les deux options de déploiement ?

Chaque option de déploiement répond à des besoins différents. Le déploiement de base est conçu pour la prise en main et le développement d’une preuve de concept et de petits projets pilotes. Il fournit une architecture simplifiée avec le minimum de ressources nécessaires et à moindre coût. Le déploiement standard est conçu pour créer et personnaliser une solution prête à l’emploi dans un environnement de production et fournit les éléments nécessaires à cette fin. Pour la fiabilité et l’échelle, les microservices d’application sont générés en tant que conteneurs Docker et déployées à l’aide d’un orchestrateur (Kubernetes par défaut). L’orchestrateur est responsable du déploiement, de la mise à l’échelle et de la gestion de l’application. Vous devez choisir une option en fonction de vos besoins actuels. Vous pouvez utiliser l’une, l’autre ou une combinaison des deux selon la phase du projet.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Comment configurer un mappage dynamique sur le tableau de bord ?

Pour plus d’informations, consultez [Upgrade map key to see devices on a dynamic map (Mettre à niveau une clé de mappage pour voir les appareils sur un mappage dynamique)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="next-steps"></a>Étapes suivantes

Vous pouvez également explorer certaines des autres fonctionnalités des accélérateurs de solution IoT :

* [Explorer les fonctionnalités de l’accélérateur de solution de surveillance à distance](iot-accelerators-remote-monitoring-explore.md)
* [Présentation de l’accélérateur de solution de maintenance prédictive](../iot-suite/iot-suite-predictive-overview.md)
* [Présentation de l’accélérateur de solution Usine connectée](iot-accelerators-connected-factory-overview.md)
* [Sécurisation de l’Internet des objets de bout en bout](../iot-suite/securing-iot-ground-up.md)