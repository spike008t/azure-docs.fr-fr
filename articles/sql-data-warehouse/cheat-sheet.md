---
title: "Aide-mémoire pour Azure SQL Data Warehouse | Microsoft Docs"
description: "Découvrez des liens et les bonnes pratiques à suivre pour créer rapidement vos solutions Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: acomet
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 12/14/2017
ms.author: acomet
ms.openlocfilehash: 2d17385ff255ddf7b85baa81600a2af60d015540
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Aide-mémoire pour Azure SQL Data Warehouse
Cet article est destiné à vous aider à concevoir une solution Data Warehouse adaptée à la plupart des cas d’usage. Cet aide-mémoire vous sera utile dans votre parcours de création d’un entrepôt de données de premier ordre, mais nous vous recommandons fortement de découvrir plus en détail chacune des étapes décrites. Tout d’abord, nous vous recommandons de lire cet article intéressant qui explique ce que SQL Data Warehouse **[est et n’est pas]**.

Le diagramme simplifié suivant indique les étapes à suivre au commencement de la conception d’un entrepôt de données SQL Data Warehouse.

![Diagramme simplifié]

## <a name="queries-and-operations-across-tables"></a>Requêtes et opérations entre les tables

Il est très important de déterminer à l’avance quelles seront les principales requêtes et opérations effectuées dans votre entrepôt de données. En effet, vous devez organiser l’architecture de votre entrepôt de données en priorité pour ces opérations. Voici des exemples d’opérations courantes :
* Joindre une ou deux tables de faits à des tables de dimension, filtrer ces tables sur une période spécifique et ajouter les résultats dans un mini-Data Warehouse
* Effectuer des mises à jour minimes ou majeures dans vos tables de faits de ventes
* Ajouter uniquement des données dans vos tables

Le fait de bien déterminer le type des opérations vous aide à optimiser la conception de vos tables.

## <a name="data-migration"></a>Migration de données

Nous vous recommandons de commencer par charger vos données **[dans Azure Data Lake Store]** ou le Stockage Blob Azure. Ensuite, utilisez Polybase pour charger vos données dans une table de mise en lots dans SQL Data Warehouse. Nous vous recommandons la configuration suivante :

| Conception | Recommandation |
|:--- |:--- |
| Distribution | Tourniquet |
| Indexation | Segment de mémoire |
| Partitionnement | Aucun |
| Classe de ressources | largerc ou xlargerc |

Découvrez plus en détail la **[migration des données] et le [chargement des données]**, notamment des **[conseils plus approfondis] sur le chargement**. 

## <a name="distributed-or-replicated-tables"></a>Tables répliquées ou distribuées

Nous vous recommandons les stratégies suivantes, en fonction des caractéristiques de vos tables :

| Type | Usage recommandé | À utiliser avec précaution dans ces cas|
|:--- |:--- |:--- |
| Répliquée | • Tables de dimension de petite taille dans un schéma en étoile, avec moins de 2 Go de stockage après compression (compression par 5 environ) |• Table faisant l’objet de nombreuses transactions d’écriture (par exemple, insertion, upsert, suppression, mise à jour)<br></br>• Changement fréquent du provisionnement d’unités DWU (Data Warehouse Units)<br></br>• Table contenant de nombreuses colonnes, dont seulement deux ou trois sont utilisées<br></br>• Index utilisé sur une table répliquée |
| Tourniquet (par défaut) | • Table temporaire/de mise en lots<br></br> • Absence de clé de jointure évidente ou de colonne candidate appropriée |• Lenteur d’exécution due au déplacement des données |
| Hachage | • Tables de faits<br></br>• Tables de dimension de grande taille |• Mise à jour impossible de la clé de distribution |

**Conseils :**
* Choisissez initialement une distribution par tourniquet (round robin), mais envisagez ensuite de passer à une stratégie de distribution par hachage pour tirer pleinement parti d’une architecture de traitement massivement parallèle
* Assurez-vous que les clés de hachage communes ont toutes le même format de données
* N’effectuez pas de distribution sur le format varchar
* Les tables de dimension ayant une clé de hachage commune avec une table de faits soumise à de fréquentes opérations de jointure peuvent faire l’objet d’une distribution par hachage
* Utilisez *[sys.dm_pdw_nodes_db_partition_stats]* pour analyser l’asymétrie dans les données
* Utilisez *[sys.dm_pdw_request_steps]* pour analyser les mouvements de données subséquents aux requêtes, et surveiller la durée des opérations de diffusion et de lecture aléatoire. Ces informations sont utiles pour la revue de votre stratégie de distribution.

Découvrez plus en détail les **[tables répliquées] et les [tables distribuées]**.

## <a name="indexing-your-table"></a>Indexation de votre table

L’indexation est un **excellent moyen** pour lire rapidement des tables. Vous pouvez utiliser un ensemble unique de technologies selon vos besoins :

| Type | Usage recommandé | À utiliser avec précaution dans ces cas|
|:--- |:--- |:--- |
| Segment de mémoire | • Table de mise en lots/temporaire<br></br>• Tables de petite taille avec des recherches réduites |• Analyse complète de la table à chaque recherche |
| Index cluster | • Table contenant jusqu’à 100 millions de lignes<br></br>• Tables volumineuses (plus de 100 millions de lignes) où seulement une ou deux colonnes sont souvent utilisées |• Index utilisé sur une table répliquée<br></br>• Requêtes complexes avec plusieurs opérations de jointure et de regroupement<br></br>• Consommation de mémoire à cause des mises à jour sur les colonnes indexées |
| Index columnstore cluster (par défaut) | • Tables volumineuses (plus de 100 millions de lignes) | • Index utilisé sur une table répliquée<br></br>• Nombre élevé de mises à jour effectuées dans la table<br></br>• Partitionnement excessif de la table : les groupes de lignes ne sont pas répartis entre les différents nœuds et partitions de la distribution |

**Conseils :**
* En plus d’un index cluster, vous pouvez ajouter un index non cluster à une colonne souvent utilisée comme filtre. 
* Gérez avec attention la mémoire pour une table avec un index columnstore cluster. Quand vous chargez des données, l’utilisateur (ou la requête) doit pouvoir disposer d’une large classe de ressources. Ainsi, vous empêchez la suppression et la création de nombreux petits groupes de lignes compressés
* Le niveau Calcul optimisé peut avoir un index columnstore cluster
* Avec un index columnstore cluster, vous pouvez observer une exécution moins rapide en raison de la faible compression de vos groupes de lignes. Dans ce cas, essayez de regénérer ou de réorganiser votre index columnstore cluster. Prévoyez au moins 100 000 lignes par groupe de lignes compressé. L’idéal est d’avoir un million de lignes dans un groupe de lignes.
* Selon la fréquence et la taille du chargement incrémentiel, il peut être utile d’automatiser la réorganisation ou la regénération de vos index. Un nettoyage est toujours conseillé.
* Soyez méthodique pour supprimer un groupe de lignes : de quelle taille sont les groupes de lignes ouverts ? Quelle quantité de données prévoyez-vous de charger dans les jours à venir ?

Découvrez plus en détail les **[index]**.

## <a name="partitioning"></a>Partitionnement
Vous pouvez partitionner votre table quand vous avez des tables de faits volumineuses contenant plus d’un milliard de lignes. Dans 99 % des cas, la clé de partition doit être basée sur la date. Veillez à ne pas effectuer de partitionnement excessif, en particulier si vous utilisez un index columnstore cluster.
Avec les tables de mise en lots qui nécessitent ETL, le partitionnement peut être bénéfique. Il simplifie la gestion du cycle de vie des données.
Là-aussi, n’effectuez pas de partitionnement excessif, notamment avec un index columnstore cluster.

Découvrez plus en détail les **[partitions]**.

## <a name="incremental-load"></a>Chargement incrémentiel

Tout d’abord, assurez-vous d’allouer des classes de ressources suffisamment grandes pour le chargement de vos données. Nous vous recommandons d’utiliser Polybase et ADF V2 pour automatiser vos pipelines ETL dans SQL Data Warehouse.

Si vous avez beaucoup de mises à jour dans vos données d’historique, nous vous recommandons de supprimer d’abord les données concernées. Vous pouvez ensuite insérer en bloc les nouvelles données. Cette méthode en deux étapes est plus efficace.

## <a name="maintain-statistics"></a>Mettre à jour les statistiques
Les statistiques automatiques seront bientôt mises à la disposition générale. En attendant, vous devez continuer à mettre à jour manuellement les statistiques dans SQL Data Warehouse. Il est important de mettre à jour les statistiques dès que des modifications **significatives** ont été apportées à vos données. Cela permet d’optimiser davantage vos plans de requête. Si vous trouvez que la mise à jour de toutes vos statistiques prend trop de temps, vous pouvez sélectionner moins de colonnes contenant des statistiques. Vous pouvez également définir la fréquence des mises à jour. Par exemple, vous pouvez mettre à jour des colonnes de date, où de nouvelles valeurs peuvent être ajoutées de façon quotidienne. Pour obtenir des performances optimales, effectuez des statistiques sur les colonnes utilisées dans les jointures, celles utilisées dans la clause WHERE et celles figurant dans GROUP BY.

Découvrez plus en détail les **[statistiques]**.

## <a name="resource-class"></a>Classe de ressources
SQL Data Warehouse utilise des groupes de ressources pour allouer de la mémoire aux requêtes. Si vous avez besoin de davantage de mémoire pour accélérer les requêtes ou le chargement, allouez des classes de ressources plus grandes. L’utilisation de classes de ressources plus grandes impacte toutefois la concurrence. C’est un point à prendre en compte avant de déplacer tous vos utilisateurs dans une classe de ressources de plus grande taille.

Si vous remarquez que les requêtes prennent trop de temps, vérifiez que vos utilisateurs ne se trouvent pas dans des grandes classes de ressources. Les grandes classes de ressources consomment beaucoup d’emplacements de concurrence. Elles peuvent aussi entraîner la mise en file d’attente d’autres requêtes.

Enfin, si vous utilisez le niveau Calcul optimisé, chaque classe de ressources obtient 2,5 fois plus de mémoire que pour le niveau Élastique optimisé.

Découvrez plus en détail comment utiliser les **[classes de ressources et la concurrence]**.

## <a name="lower-your-cost"></a>Réduire vos coûts
Une fonctionnalité clé de SQL Data Warehouse est la possibilité de suspension lorsque vous ne l’utilisez pas, ce qui permet d’arrêter la facturation des ressources de calcul. Une autre fonctionnalité clé est la possibilité de mettre à l’échelle les ressources. La suspension et la mise à l’échelle peuvent s’effectuer via le portail Azure ou des commandes PowerShell.

Vous pouvez maintenant effectuer une mise à l’échelle automatique à tout moment à l’aide d’Azure Functions :

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-you-architecture-for-performance"></a>Optimiser votre architecture pour améliorer les performances

Nous vous recommandons d’envisager SQL Database et Azure Analysis Services dans une architecture Hub and Spokes. Cette solution peut fournir l’isolation de la charge de travail entre les différents groupes d’utilisateurs, tout en rendant possible l’utilisation de certaines fonctionnalités de sécurité avancées offertes par SQL Database et Azure Analysis Services. C’est également un moyen de fournir une concurrence illimitée à vos utilisateurs.

Découvrez plus en détail les **[architectures standard utilisant SQL Data Warehouse]**.

Déployez en un seul clic vos spokes dans les bases de données SQL Database à partir de SQL Data Warehouse :

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Diagramme simplifié]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[chargement des données]:./design-elt-data-loading.md
[conseils plus approfondis]: ./guidance-for-loading-data.md
[index]:./sql-data-warehouse-tables-index.md
[partitions]:./sql-data-warehouse-tables-partition.md
[statistiques]:./sql-data-warehouse-tables-statistics.md
[classes de ressources et la concurrence]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[architectures standard utilisant SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[est et n’est pas]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[migration des données]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[tables répliquées]:https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[tables distribuées]:https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[dans Azure Data Lake Store]: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql