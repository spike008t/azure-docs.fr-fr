---
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 11/09/2018
ms.author: mbullwin
ms.openlocfilehash: 91141e314bf276d9138157c8a9d85d5262ac5907
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131029"
---
Collection des mesures personnalisées. Utilisez cette collection pour présenter des mesures nommées associées à l’élément de télémétrie. Les cas d’utilisation classiques sont :
- la taille de charge utile de télémétrie des dépendances ;
- le nombre d’éléments d’attente traités par la télémétrie des requêtes ;
- le temps que prend le client pour compléter l’étape dans l’Assistant pour l’état d’achèvement Télémétrie d’événement.

Vous pouvez interroger les [mesures personnalisées](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) dans l’analyse des applications :

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Des mesures personnalisées sont associées à l’élément de télémétrie auquel elles appartiennent. Elles sont soumises à un échantillonnage à l’aide de l’élément de télémétrie dans lequel ces mesures sont contenues. Pour effectuer le suivi d’une mesure dont la valeur est indépendante de tout autre type de données de télémétrie, utilisez [Télémétrie des mesures](../articles/azure-monitor/app/api-custom-events-metrics.md).

Longueur de clé maximale : 150
