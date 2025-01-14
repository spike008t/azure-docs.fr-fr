---
title: Comprendre la conservation des données dans votre environnement Azure Time Series Insights | Microsoft Docs
description: Cet article décrit deux paramètres qui contrôlent la conservation des données dans votre environnement Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: e3336df30873b40d2b8a464d1f866b524f76776d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236997"
---
# <a name="understand-data-retention-in-azure-time-series-insights"></a>Comprendre la conservation des données dans Azure Time Series Insights

Cet article décrit deux paramètres qui affectent la conservation des données dans votre environnement Azure Time Series Insights.

## <a name="video"></a>Vidéo

### <a name="the-following-video-summarizes-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>La vidéo suivante résume la rétention des données Time Series Insights et comment il planifier.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Chacun de vos environnements Azure Time Series a un paramètre qui contrôle **durée de conservation des données**. La valeur est comprise entre 1 et 400 jours. Les données sont supprimées en fonction de la capacité de stockage d’environnement ou de la durée de rétention, le premier prévalant.

En outre, votre environnement Azure Time Series a un **comportement de limite de stockage dépassée** paramètre. Il contrôle en entrée et vider le comportement lorsque la capacité maximale d’un environnement est atteinte. Il existe deux comportements proposées lors de sa configuration :

- **Vidage des données anciennes** (par défaut)  
- **Suspendre l’entrée**

> [!NOTE]
> Par défaut, lorsque vous créez un nouvel environnement, la conservation est configurée sur **Vidage des données anciennes**. Ce paramètre peut être activé ou désactivé en fonction des besoins après l’heure de création à l’aide du portail Azure, sur le **configurer** page de l’environnement Time Series Insights.

Pour plus d’informations sur la commutation des comportements de conservation, consultez [Configuration de la conservation dans Time Series Insights](time-series-insights-how-to-configure-retention.md).

Comparez le comportement de conservation des données :

## <a name="purge-old-data"></a>Vidage des données anciennes

- Ce comportement est le comportement par défaut pour les environnements Time Series Insights.  
- Ce comportement est préférable lorsque les utilisateurs souhaitent toujours voir leur *données les plus récentes* dans leur environnement Time Series Insights.
- Ce comportement *vide* les données une fois que les limites de l’environnement (durée de conservation, taille ou nombre, selon ce qui se présente en premier) sont atteintes. Par défaut, la conservation est définie sur 30 jours.
- Les données ingérées les plus anciennes sont vidées en premier (approche FIFO).

### <a name="example-one"></a>Exemple 1

Prenons l’exemple d’un environnement configuré avec le comportement de conservation **Poursuivre l’entrée et vider les données anciennes** :

**Durée de conservation des données** est définie sur 400 jours. La **Capacité** est définie sur l’unité S1, avec une capacité totale de 30 Go.   Supposons que les données entrantes atteignent en moyenne 500 Mo par jour. Cet environnement ne peut conserver que 60 jours de données étant donné le taux de données entrantes, la capacité maximale étant atteinte après 60 jours. Les données entrantes s’accumulent comme suit : 500 Mo chaque jour x 60 jours = 30 Go.

Le 61ème jour, l’environnement affiche les données les plus récentes, mais vide les données plus anciennes, plues de 60 jours. Le vidage fait de la place pour les nouvelles données entrantes, afin qu’elles puissent toujours être explorées. Si l’utilisateur souhaite conserver les données plus longtemps, ils peut augmenter la taille de l’environnement en ajoutant des unités supplémentaires ou il peut envoyer (push) moins de données.  

### <a name="example-two"></a>Le deuxième exemple

Prenons l’exemple d’un environnement configuré également avec le comportement de conservation **Poursuivre l’entrée et vider les données anciennes**. Dans cet exemple, la **Durée de conservation des données** est définie sur une valeur inférieure de 180 jours. La **Capacité** est définie sur l’unité S1, avec une capacité totale de 30 Go. Pour stocker des données durant les 180 jours complets, l’entrée quotidienne ne peut pas dépasser 0,166 Go (166 Mo) par jour.  

Lorsque le taux d’entrée quotidien de cet environnement dépasse 0,166 Go par jour, les données ne peuvent pas être stockées pendant 180 jours, étant donné que certaines données sont vidées. Considérez ce même environnement pendant un laps de temps occupé. Supposons que le taux d’entrée de l’environnement atteigne 0,189 Go en moyenne par jour. Durant ce laps de temps occupé, 158 jours environ de données sont conservés (30 Go/0,189 = 158,73 jours de conservation). Cette durée est inférieure à la durée de conservation des données souhaitée.

## <a name="pause-ingress"></a>Suspendre l’entrée

- Le **suspendre l’entrée** paramètre est conçu pour vous assurer de données ne sont pas vidées si les limites de taille et de nombre sont atteintes avant leur période de rétention.  
- **Suspendre l’entrée** offre plus de temps pour les utilisateurs augmenter la capacité de leur environnement avant que les données soient vidées en raison de la violation de la période de rétention
- Il vous aide à vous protéger contre la perte de données mais que vous pouvez créer une opportunité de perte de vos données les plus récentes si l’entrée est suspendue au-delà de la période de rétention de votre source d’événements.
- Toutefois, une fois que la capacité maximale d’un environnement est atteinte, l’environnement suspend l’entrée de données jusqu'à ce que les actions supplémentaires suivantes se produisent :

   - Vous augmentez la capacité maximale de l’environnement pour ajouter des unités d’échelle comme décrit dans [mise à l’échelle de votre environnement Time Series Insights](time-series-insights-how-to-scale-your-environment.md).
   - La période de rétention de données est atteinte et les données sont vidées, apportant l’environnement sous sa capacité maximale.

### <a name="example-three"></a>Exemple trois

Prenons l’exemple d’un environnement avec le comportement de conservation configuré sur **Suspendre l’entrée**. Dans cet exemple, la **Durée de conservation des données** est configurée sur 60 jours. **Capacité** est défini sur trois (3) les unités de S1. Supposons que cet environnement connaît une entrée de données de 2 Go par jour. Dans cet environnement, l’entrée est suspendue une fois la capacité maximale atteinte.

À ce stade, l’environnement affiche le même jeu de données jusqu'à ce que l’entrée reprenne ou jusqu'à ce que **poursuivre l’entrée** est activé (ce qui serait purger les données plus anciennes pour faire place aux nouvelles données).

Lorsque l’entrée reprend :

- Les données circulent dans l’ordre où elles ont été reçues par la source d’événements
- Les événements sont indexés en fonction de leur horodatage, sauf si vous avez dépassé les stratégies de conservation sur votre source d’événements. Pour plus d’informations sur la configuration de la conservation de la source d’événements, consultez [Forum Aux Questions (FAQ) sur Event Hubs](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Nous vous conseillons de définir des alertes pour éviter que l’entrée ne soit suspendue. La perte de données est possible, car la conservation par défaut est définie sur 1 jour pour les sources d’événements Azure. Par conséquent, une fois que l’entrée est suspendue, vous perdrez probablement les données les plus récentes, sauf si une action supplémentaire est effectuée. Vous devez augmenter la capacité ou définir le comportement sur **Vider les données anciennes** afin d’éviter la perte de données potentielle.

Dans les Event Hubs concernés, envisagez d’ajuster la propriété **Conservation des messages** afin de minimiser la perte de données lorsque l’entrée est suspendue dans Time Series Insights.

[![Rétention des messages Event hub.](media/time-series-insights-contepts-retention/event-hub-retention.png)](media/time-series-insights-contepts-retention/event-hub-retention.png#lightbox)

Si aucune propriété n’est configurées sur la source d’événement (`timeStampPropertyName`), Time Series Insights par défaut est l’horodatage d’arrivée dans le concentrateur d’événements en tant que l’axe des abscisses. Si `timeStampPropertyName` est configuré pour être autre chose, le recherche environnement configuré `timeStampPropertyName` dans le paquet de données lorsque les événements sont analysés.

Si vous devez mettre votre environnement à l’échelle pour prendre en charge une capacité supplémentaire ou pour augmenter la durée de conservation, consultez [Mise à l’échelle de votre environnement Time Series Insights](time-series-insights-how-to-scale-your-environment.md) pour plus d’informations.  

## <a name="next-steps"></a>Étapes suivantes

- Pour plus d’informations sur la configuration ou la modification des paramètres de rétention de données, passez en revue [configuration de la conservation dans Time Series Insights](time-series-insights-how-to-configure-retention.md).
