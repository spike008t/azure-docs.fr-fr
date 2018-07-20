---
title: Ressources évolutives Azure SQL Database | Microsoft Docs
description: Cet article explique comment mettre à l'échelle votre base de données en ajoutant ou en supprimant des ressources allouées.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/07/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: f55ce511f6ba90c27e149ac90bbd2c8aa0b3c742
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37923437"
---
# <a name="scale-database-resources"></a>Mettre à l'échelle des ressources de base de données

Azure SQL Database vous permet d’ajouter de façon dynamique plus de ressources à votre base de données, en un temps d’arrêt minimal.

## <a name="overview"></a>Vue d'ensemble

Lorsque la demande ciblant votre application s’accroît de quelques appareils et clients à plusieurs millions, Azure SQL Database se met à l’échelle immédiatement avec un temps d’arrêt minimal. L’extensibilité est une des caractéristiques les plus importantes de PaaS qui vous permet d’ajouter plus de ressources de façon dynamique à votre service, si besoin. Azure SQL Database vous permet de modifier en toute simplicité vos ressources (alimentation processeur, mémoire, débit E/S et stockage) allouées à vos bases de données.  
Vous pouvez limiter les problèmes de performances dus à une utilisation croissante de votre application qui ne peuvent être résolus en utilisant des méthodes de réécriture de requête ou d’indexation. L’ajout de ressources vous permet de réagir rapidement lorsqu’une base de données atteint la limite de ressources actuelles et a besoin de plus de puissance pour gérer les charges entrantes. Azure SQL Database vous permet aussi de diminuer la taille des ressources lorsqu’elles ne sont pas nécessaires afin de réduire les coûts.
Vous n’avez pas à vous inquiéter de l’achat de matériel et du changement de l’infrastructure sous-jacente. La mise à l'échelle de base de données peut être effectuée en toute simplicité via le portail Azure à l’aide d’un curseur.

![Performances de SQL Database](media/sql-database-scalability/scale-performance.svg)

Azure SQL Database propose un [modèle d’achat DTU](sql-database-service-tiers-dtu.md) ou le [modèle d’achat vCore (préversion)](sql-database-service-tiers-vcore.md). 
-   Le [modèle d’achat DTU](sql-database-service-tiers-dtu.md) offre un mélange de ressources de calcul, de mémoire et d’E/S dans trois niveaux de service pour prendre en charge les charges de travail de base de données, aussi bien légères qu’importantes : De base, Standard et Premium. Les niveaux de performance de chaque niveau fournissent une combinaison différente de ces ressources, à laquelle vous pouvez ajouter d’autres ressources de stockage.
-   Le [modèle d’achat vCore](sql-database-service-tiers-vcore.md) (préversion) vous permet de choisir le nombre de vCores, la quantité de mémoire et de stockage, ainsi que la vitesse de stockage.
Vous pouvez créer votre première application sur une seule petite base de données pour un coût mensuel modique et modifier le niveau de service manuellement ou automatiquement à tout moment pour répondre aux besoins de votre solution. Vous pouvez ajuster les performances sans perturber le fonctionnement de votre application, ni l’expérience de vos clients. L’évolutivité dynamique permet à votre base de données de répondre en toute transparence aux besoins en ressources qui évoluent sans cesse et de payer uniquement les ressources dont vous avez besoin, lorsque vous en avez besoin.


> [!NOTE]
> La scalabilité dynamique est différente de la mise à l’échelle automatique. La mise à l’échelle survient lorsqu’un service se met à l’échelle automatiquement en fonction de critères, tandis que l’extensibilité dynamique permet la mise à l’échelle manuelle sans temps d’arrêt.
>


Une instance unique d’Azure SQL Database prend en charge l’extensibilité dynamique, mais pas la mise à l’échelle automatique. Pour plus expérience plus *automatique*, envisagez d’utiliser des pools élastiques, ce qui permet aux bases de données de partager des ressources dans un pool en fonction de leurs besoins individuels.
Toutefois, il existe des scripts qui peuvent aider à automatiser l’extensibilité pour une instance unique d’Azure SQL Database. Pour obtenir un exemple, consultez la rubrique [Utiliser PowerShell pour surveiller et mettre à l’échelle une base de données SQL](scripts/sql-database-monitor-and-scale-database-powershell.md).


Ces trois possibilités d’Azure SQL Database offrent des capacités à mettre à l'échelle vos bases de données de façon dynamique :
-   Dans [Azure SQL Single Database](sql-database-single-database-scale.md), vous pouvez utiliser des modèles [DTU](sql-database-dtu-resource-limits-single-databases.md) ou [vCore](sql-database-vcore-resource-limits-single-databases.md) pour définir le volume maximal de ressources assignées à chaque base de données.
-   [Azure SQL Managed Instance](sql-database-managed-instance.md) utilise le modèle [vCores](/azure/sql-database/sql-database-managed-instance#vcore-based-purchasing-model-preview) et vous permet de définir le nombre maximum de cœurs de processeur et de stockage alloué à votre instance. Toutes les bases de données au sein de l’instance partageront les ressources allouées à l’instance.
-   Les [pools élastiques SQL Azure](sql-database-elastic-pool-scale.md) vous permettent de définir la limite de ressources maximum par groupe de bases de données dans le pool.

## <a name="alternative-scale-methods"></a>Autres méthodes de mise à l’échelle
La mise à l'échelle des ressources reste la façon la plus facile et efficace pour améliorer les performances de votre base de données sans changer le code de la base de données ou de l’application.
Dans certains cas, même les niveaux et les optimisations de performances les plus élevés pourraient ne pas pouvoir gérer votre charge de travail de façon économique et avec succès. Dans ces cas, vous disposez d’autres options pour mettre à l'échelle votre base de données :
-   L’[échelle lecture](sql-database-read-scale-out.md) est une fonctionnalité disponible qui vous offre un réplica en lecture seule de vos données, sur lequel vous pouvez exécutez des requêtes en lecture seule exigeantes telles que les rapports. Le réplica en lecture seule gère votre charge de travail en lecture seule sans affecter l’utilisation des ressources sur votre base de données primaire.
-   Le [partitionnement de base de données](sql-database-elastic-scale-introduction.md) est un ensemble de techniques qui vous permet de diviser vos données en plusieurs bases de données pour les mettre à l'échelle indépendamment.

## <a name="next-steps"></a>Étapes suivantes
- Pour en savoir plus sur l’amélioration des performances d’une base de données en modifiant son code, consultez [Trouver et appliquer des recommandations de performances](sql-database-advisor-portal.md).
- Pour en savoir sur l’optimisation de votre base de données par une intelligence intégrée, consultez [Ajustement automatique](sql-database-automatic-tuning.md).
- Pour en savoir plus l’échelle lecture dans le service Azure SQL Database, consultez [Utiliser des réplicas en lecture seule pour équilibrer la charge de requêtes en lecture seule](sql-database-read-scale-out.md).
- Pour en savoir plus sur le partitionnement de base de données, consultez la rubrique [Montée en charge avec Azure SQL Database](sql-database-elastic-scale-introduction.md).
