---
title: Présentation de l’API Table d’Azure Cosmos DB
description: Découvrez comment utiliser Azure Cosmos DB pour stocker et interroger d’immenses volumes de données clé-valeur avec une faible latence en utilisant les API Tables Azure.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 05/20/2019
ms.author: sngun
ms.openlocfilehash: bd86b230d801f5fff8a9fb0de85f9f3025527382
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65953454"
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Présentation d’Azure Cosmos DB : API de table

[Azure Cosmos DB](introduction.md) fournit l’API de table aux applications qui sont écrites pour le stockage de table Azure et qui ont besoin de fonctionnalités Premium comme :

* [Une distribution mondiale clé en main](distribute-data-globally.md).
* [Un débit dédié](partition-data.md) partout dans le monde.
* Des latences de quelques millisecondes au 99e centile.
* Une haute disponibilité garantie.
* [Une indexation secondaire automatique](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Les applications écrites pour le stockage de table Azure peuvent migrer vers Azure Cosmos DB à l’aide de l’API de table sans aucune modification de code, et tirer parti des fonctionnalités Premium. L’API de table a des kits de développement logiciel (SDK) pour .NET, Java, Python et Node.js.

> [!IMPORTANT]
> Le kit de développement logiciel (SDK) .NET Framework [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) est en mode maintenance et sera bientôt déprécié. Mettez à niveau vers la nouvelle bibliothèque .NET Standard [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) pour continuer à bénéficier des dernières fonctionnalités prises en charge par l’API Table.

## <a name="table-offerings"></a>Offres de table
Si vous utilisez actuellement le stockage de table Azure, vous bénéficiez des avantages suivants en passant à l’API Table d’Azure Cosmos DB :

| | Stockage de tables Azure | API Table d’Azure Cosmos DB |
| --- | --- | --- |
| Latence | Rapide, mais aucune limite supérieure sur la latence. | Une latence de quelques millisecondes pour les lectures et écritures, appuyée par des lectures avec une latence de < 10 ms et des écritures avec une latence de < 15 ms au 99e centile, à n’importe quelle échelle, n’importe où dans le monde. |
| Débit | Modèle de débit variable. Les tables ont une limite d’évolutivité de 20 000 opérations/s. | Hautement évolutif avec un [débit dédié réservé par table](request-units.md), qui est appuyé par des contrats de niveau de service. Les comptes n’ont aucune limite supérieure sur le débit, et prennent en charge > 10 millions d’opérations/s par table. |
| Diffusion mondiale | Une région unique avec une région de lecture secondaire en option pour la haute disponibilité. Vous ne pouvez pas lancer le basculement. | [Une distribution mondiale clé en main](distribute-data-globally.md) de 1 à plus de 30 régions. Prise en charge des [basculements automatiques et manuels](high-availability.md) à tout moment, partout dans le monde. |
| Indexation | Index primaire uniquement sur PartitionKey et RowKey. Pas d’index secondaire. | Indexation automatique et complète de toutes les propriétés, aucune gestion des index. |
| Requête | L’exécution des requêtes utilise un index de clé primaire, et effectue une recherche dans le cas contraire. | Les requêtes peuvent tirer parti de l’indexation automatique de propriétés pour des temps de requête rapides. |
| Cohérence | Forte au sein de la région primaire. Éventuelle au sein de la région secondaire. | [Cinq niveaux de cohérence bien définis](consistency-levels.md) pour compenser la disponibilité, la latence, le débit ou la cohérence en fonction des besoins de votre application. |
| Tarifs | Optimisation pour le stockage. | Optimisation pour le débit. |
| Contrats SLA | Disponibilité de 99,99 %. | Un contrat SLA avec une disponibilité à 99,99 % pour tous les comptes à région unique et à plusieurs régions avec cohérence souple, ainsi qu’une disponibilité de lecture à 99,999 % pour tous les comptes de base de données à plusieurs régions [Contrats SLA complets à la pointe du secteur](https://azure.microsoft.com/support/legal/sla/cosmos-db/) sur la disponibilité générale. |

## <a name="get-started"></a>Prise en main

Créez un compte Azure Cosmos DB dans le [portail Azure](https://portal.azure.com). Continuez avec notre [démarrage rapide pour l’API de table à l’aide de .NET](create-table-dotnet.md). 

> [!IMPORTANT]
> Si vous avez créé un compte d’API Table dans la préversion, créez un [nouveau compte d’API Table](create-table-dotnet.md#create-a-database-account) pour utiliser les Kits de développement logiciels (SDK) mis à la disposition générale pour l’API Table.
>

## <a name="next-steps"></a>Étapes suivantes

Voici quelques conseils pour vous aider à démarrer :
* [Azure Cosmos DB : Créer une application .NET à l’aide de l’API de table](create-table-dotnet.md)
* [Développer avec l’API de table dans .NET](tutorial-develop-table-dotnet.md)
* [Interroger des données de table avec l’API de table](tutorial-query-table.md)
* [Comment configurer la distribution mondiale Azure Cosmos DB à l’aide de l’API de table](tutorial-global-distribution-table.md)
* [API .NET Table Azure Cosmos DB](table-sdk-dotnet.md)
* [API Java Table Azure Cosmos DB](table-sdk-java.md)
* [API Node.js Table Azure Cosmos DB](table-sdk-nodejs.md)
* [Kit de développement logiciel (SDK) de table Azure Cosmos DB pour Python](table-sdk-python.md)

