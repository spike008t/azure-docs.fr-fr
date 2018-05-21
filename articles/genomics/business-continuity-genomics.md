---
title: Continuité d’activité - Microsoft Genomics | Microsoft Docs
titleSuffix: Azure
description: Découvrez comment Microsoft Genomics prend en charge la continuité d’activité.
keywords: continuité d’activité, récupération d’urgence
services: microsoft-genomics
author: grhuynh
manager: jhubbard
editor: jasonwhowell
ms.author: grhuynh
ms.service: microsoft-genomics
ms.workload: genomics
ms.topic: article
ms.date: 04/06/2018
ms.openlocfilehash: cb3825cb89aff386c4f7c3f3b0d771d73fe637b1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>Vue d’ensemble de la continuité d’activité avec Microsoft Genomics
Cette vue d’ensemble décrit les fonctionnalités de Microsoft Genomics en matière de continuité d’activité et de récupération d’urgence. En savoir plus sur les options de récupération après des événements d’interruption, comme la panne d’une région Azure, pouvant entraîner des pertes de données. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Fonctionnalités Microsoft Genomics prenant en charge la continuité d’activité 
Cela est rare, mais un centre de données Azure peut subir une panne, susceptible d’entraîner le cas échéant une interruption d’activité de quelques minutes à quelques heures. Lorsqu’une panne se produit, toutes les tâches en cours d’exécution dans le centre de données sont mises en échec, et le service Microsoft ne redirige pas automatiquement les tâches vers une région secondaire. 

* Vous pouvez attendre que votre centre de données redevienne disponible une fois la panne réparée. Si la panne est courte, le service Microsoft Genomics détecte automatiquement les tâches mises en échec et le workflow est automatiquement redémarré.

* Une autre option consiste à proactivement envoyer le workflow dans une autre région de données. Microsoft Genomics déploie les instances dans plusieurs [régions Azure](https://azure.microsoft.com/regions/services/), et chaque instance de région est indépendante. Si l’une des instances Microsoft Genomics subit une panne locale régionale, les autres régions exécutant des instances de Microsoft Genomics continuent à traiter les tâches. Ce transfert vers une autre région, sous le contrôle de l’utilisateur, est disponible à tout moment.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>Basculer manuellement les workflows Microsoft Genomics vers une autre région
Dans l’éventualité d’une panne régionale du centre de données, vous pouvez décider d’envoyer les workflows Microsoft Genomics vers une région secondaire, en fonction de vos exigences individuelles de souveraineté et de continuité d’activité. Pour basculer manuellement les workflows Microsoft Genomics, il vous faut utiliser un compte Genomics d’une région différente et soumettre la tâche avec des informations d’identification spécifiques à la région Genomics et du compte de stockage.

Plus précisément, vous devez :
* créer un compte Genomics dans une région secondaire, à l’aide du portail Azure ; 
* migrer vos données d’entrée vers un compte de stockage de la région secondaire, et définir un dossier de sortie dans la région secondaire ;
* soumettre le workflow dans la région secondaire.

Lorsque la région d’origine est restaurée, le service Microsoft Genomics ne migre pas les données de la région secondaire vers la région d’origine. Vous pouvez choisir de déplacer les fichiers d’entrée et de sortie de la région secondaire vers la région d’origine.  Si vous faites ce choix, cette opération s’effectue en dehors du service Genomics et l’ensemble des frais associés aux mouvements des données relèvent de votre responsabilité. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>Préparation à une situation de panne régionale
Si vous vous inquiétez de la rapidité de la récupération en cas de panne du centre de données, il existe une procédure vous permettant de réduire le délai de soumission manuelle de vos workflows Microsoft Genomics vers une région secondaire :

* Identifiez une région secondaire appropriée et créez proactivement un compte Genomics dans cette région.
* Dupliquez vos données dans les régions primaire et secondaire afin de les rendre immédiatement disponibles dans la région secondaire. Cette opération peut être effectuée manuellement ou à l’aide de la fonctionnalité de [stockage géo-redondant](https://docs.microsoft.com/azure/storage/common/storage-redundancy) disponible dans Stockage Azure. 

## <a name="next-steps"></a>Étapes suivantes
Dans cet article, vous avez découvert les options dédiées à la continuité d’activité et à la récupération d’urgence en lien avec l’utilisation du service Microsoft Genomics. Pour plus d’informations sur la continuité d’activité et la récupération d’urgence au sein d’Azure en général, consultez la [documentation technique sur la résilience Azure.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region) 