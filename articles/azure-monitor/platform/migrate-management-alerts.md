---
title: Migrer les alertes Azure pour les événements de gestion vers les alertes de journal d’activité
description: Les alertes pour les événements de gestion seront supprimées le 1er octobre. Préparez-vous en migrant les alertes existantes.
author: rboucher
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/14/2017
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 78519dad85739b6e4d760bc34719837956638f48
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388716"
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Migrer les alertes Azure pour les événements de gestion vers les alertes de journal d’activité

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!WARNING]
> Alertes sur des événements de gestion seront désactivées sur ou après octobre 1,2017. Utilisez les instructions ci-dessous pour déterminer si vous avez ce type d’alertes et migrez-les le cas échéant.

## <a name="what-is-changing"></a>Changements

Azure Monitor (anciennement Azure Insights) proposait une fonctionnalité permettant de générer une alerte à partir d’événements de gestion et de transmettre des notifications à des URL du webhook ou des adresses e-mail. Vous avez peut-être créé une de ces alertes d’une des manières suivantes :
* Dans le portail Azure pour certains types de ressources, sous Surveillance -> Alertes -> Ajouter une alerte, où « Alerte pour » est défini sur « Événements » »
* En exécutant l’applet de commande Add-AzLogAlertRule PowerShell
* En utilisant directement [l’API REST d’alerte](https://docs.microsoft.com/rest/api/monitor/alertrules) avec odata.type = "ManagementEventRuleCondition" et dataSource.odata.type = "RuleManagementEventDataSource"
 
Le script PowerShell suivant retourne une liste de toutes les alertes sur les événements de gestion figurant dans votre abonnement, ainsi que les conditions définies pour chaque alerte.

```powershell
Connect-AzAccount
$alerts = $null
foreach ($rg in Get-AzResourceGroup ) {
  $alerts += Get-AzAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Si vous n’avez aucune alerte sur des événements de gestion, l’applet de commande PowerShell ci-dessus générera une série de messages d’avertissement similaire à celle-ci :

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Ces messages d’avertissement peuvent être ignorés. Si vous avez des alertes sur des événements de gestion, le résultat de cette applet de commande PowerShell ressemblera à ceci :

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Chaque alerte est séparée par une ligne en pointillés, et les détails incluent l’ID de ressource de l’alerte et la règle spécifique en cours de surveillance.

Cette fonctionnalité a été migrée vers [Alertes Journal d’activité Azure Monitor](../../azure-monitor/platform/activity-log-alerts.md). Ces nouvelles alertes permettent de définir une condition pour les événements du journal d’activité et de recevoir une notification lorsqu’un nouvel événement correspond à cette condition. Elles apportent également plusieurs améliorations au niveau des alertes sur des événements de gestion :
* Vous pouvez réutiliser votre groupe de destinataires de notification (« actions ») dans plusieurs alertes à l’aide de [groupes d’actions](../../azure-monitor/platform/action-groups.md), ce qui permet de choisir plus facilement les destinataires d’une alerte.
* Grâce aux groupes d’actions, vous pouvez recevoir une notification directement par SMS sur votre téléphone.
* Vous pouvez [créer des alertes de journal d’activité à l’aide de modèles Resource Manager](../../azure-monitor/platform/alerts-activity-log.md).
* Vous pouvez créer des conditions offrant plus de souplesse et de complexité afin de répondre à vos besoins spécifiques.
* Les notifications sont remises plus rapidement.
 
## <a name="how-to-migrate"></a>Migration
 
Voici les différentes méthodes pour créer une nouvelle alerte de journal d’activité :
* Suivez [notre guide sur la création d’une alerte dans le portail Azure](../../azure-monitor/platform/activity-log-alerts.md)
* Découvrez comment [créer une alerte à l’aide d’un modèle Resource Manager](../../azure-monitor/platform/alerts-activity-log.md)
 
Les alertes pour des événements de gestion que vous avez créées précédemment ne seront pas automatiquement migrées vers des alertes de journal d’activité. Vous devez utiliser le script PowerShell précédent pour répertorier les alertes sur des événements de gestion que vous avez actuellement configurées, puis les recréer manuellement en tant qu’alertes de journal d’activité. Ces opérations doivent être effectuées avant le 1er octobre. Après cette date, les alertes sur des événements de gestion n’apparaîtront plus dans votre abonnement Azure. Autres types d’alertes Azure, y compris les alertes de métrique Azure Monitor, les alertes Application Insights et les alertes de recherche dans les journaux ne sont pas affectés par cette modification. Si vous avez des questions, publiez-les dans les commentaires ci-dessous.


## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur les [journaux d’activité](../../azure-monitor/platform/activity-logs-overview.md)
* Configurer les [alertes de journal d’activité par le biais du portail Azure](../../azure-monitor/platform/activity-log-alerts.md)
* Configurer les [alertes de journal d’activité au moyen de Resource Manager](../../azure-monitor/platform/alerts-activity-log.md)
* Consulter le [schéma de webhook d’alerte de journal d’activité](../../azure-monitor/platform/activity-log-alerts-webhook.md)
* Apprenez-en plus sur les [notifications de service](../../azure-monitor/platform/service-notifications.md)
* En savoir plus sur les [groupes d’actions](../../azure-monitor/platform/action-groups.md)

