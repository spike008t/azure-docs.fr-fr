---
title: Surveiller Azure Batch | Microsoft Docs
description: Découvrez les services de surveillance Azure, les métriques, les journaux de diagnostic et d’autres fonctionnalités de surveillance d’Azure Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: danlep
ms.openlocfilehash: 29ac86ed5c744d37150b0f1b2db17f60306fe77e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2018
---
# <a name="monitor-batch-solutions"></a>Solutions Monitor Batch

Azure et le service Batch fournissent une gamme de services, d’outils et d’API permettant de surveiller vos solutions Batch. Cet article de présentation vous aide à choisir une approche de surveillance correspondant à vos besoins.

Pour une vue d’ensemble des composants et services Azure disponibles permettant de surveiller les ressources Azure, consultez [Surveillance des applications et ressources Azure](../monitoring-and-diagnostics/monitoring-overview.md).

## <a name="subscription-level-monitoring"></a>Surveillance au niveau de l’abonnement

Au niveau de l’abonnement, qui inclut les comptes Batch, le [journal d’activité Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) collecte les données des événements opérationnels dans [plusieurs catégories](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md#categories-in-the-activity-log).

Plus précisément, pour les comptes Batch, le journal d’activité collecte les événements liés à la gestion des clés et à la création ou la suppression de comptes.

L’une des méthodes pour récupérer des événements dans votre journal d’activité consiste à utiliser le portail Azure. Cliquez sur **Tous les services** > **Journal d’activité**. Vous pouvez également rechercher des événements à l’aide d’Azure CLI, des applets de commande PowerShell ou de l’API REST Azure Monitor. Vous pouvez également exporter le journal d’activité ou configurer les [alertes de journal d’activité](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md).

## <a name="batch-account-level-monitoring"></a>Surveillance au niveau du compte Batch

Surveillez chaque compte Batch à l’aide des fonctionnalités d’[Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure Monitor collecte des [métriques](../monitoring-and-diagnostics/monitoring-overview-metrics.md) et éventuellement des [journaux de diagnostic](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) pour les ressources au niveau du compte Batch, comme les pools, les travaux et les tâches. Collectez et utilisez ces données manuellement ou par programmation pour surveiller les activités dans votre compte Batch et diagnostiquer les problèmes. Pour plus d’informations, consultez [Métriques, alertes et journaux Batch pour l’évaluation de diagnostic et la surveillance](batch-diagnostics.md).
 
> [!NOTE]
> Les métriques sont disponibles par défaut dans votre compte Batch sans configuration supplémentaire, et elles ont un historique roulant de 30 jours. Vous devez activer la journalisation des diagnostics pour un compte Batch. Le stockage ou le traitement des données du journal de diagnostic peut induire des frais supplémentaires. 

## <a name="batch-resource-monitoring"></a>Surveillance des ressources Batch

Dans vos applications Batch, utilisez les API Batch pour surveiller ou interroger l’état de vos ressources, notamment les travaux, les tâches, les nœuds et les pools. Par exemple : 

* [Nombre des tâches par état](batch-get-task-counts.md)
* [Créer des requêtes pour répertorier les ressources Batch efficacement](batch-efficient-list-queries.md)
* [Créer des dépendances de tâches pour exécuter des tâches qui dépendent d’autres tâches](batch-task-dependencies.md)
* Utiliser une [tâche du gestionnaire de travaux](/rest/api/batchservice/job/add#jobmanagertask)
* Surveiller l’[état des tâches](/rest/api/batchservice/task/list#taskstate)
* Surveiller l’[état des nœuds](/rest/api/batchservice/computenode/list#computenodestate)
* Surveiller l’[état des pools](/rest/api/batchservice/pool/get#poolstate)
* Surveiller l’[utilisation des pools dans le compte](/rest/api/batchservice/pool/listusagemetrics)
* [Compter les nœuds de pool par état](/rest/api/batchservice/account/listpoolnodecounts)

## <a name="vm-performance-counters-and-application-monitoring"></a>Compteurs de performances des machines virtuelles et surveillance des applications

* [Application Insights](../application-insights/app-insights-overview.md) est un service Azure que vous pouvez utiliser pour surveiller par programmation la disponibilité, les performances et l’utilisation de vos travaux et tâches Batch. Accédez facilement aux compteurs de performances à partir de nœuds de calcul (machines virtuelles) et personnalisez les informations des tâches à partir des machines virtuelles. 

  Par exemple, consultez [Monitor and debug a Batch .NET application with Application Insights (Surveiller et déboguer une application .NET Batch avec Application Insights)](monitor-application-insights.md) et l’[exemple de code](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights) fourni avec.

  > [!NOTE]
  > L’utilisation d’Application Insights peut induire des frais supplémentaires. Consultez les [options de tarification](https://azure.microsoft.com/pricing/details/application-insights/). 
  >

* [BatchLabs](https://github.com/Azure/BatchLabs) est un outil client autonome, gratuit et doté de nombreuses fonctionnalités, aidant à créer, déboguer et surveiller les applications Azure Batch. Téléchargez un [package d’installation](https://azure.github.io/BatchLabs/) pour Mac, Linux ou Windows. Le cas échéant, configurez votre solution Batch pour [afficher des données Application Insights](https://github.com/Azure/batch-insights) comme des compteurs de performances de machines virtuelles dans BatchLabs.


## <a name="next-steps"></a>Étapes suivantes

* Découvrez les [outils et API Batch](batch-apis-tools.md) disponibles pour créer des solutions Batch.
* En savoir plus sur les [journaux de diagnostic](batch-diagnostics.md) avec Batch.