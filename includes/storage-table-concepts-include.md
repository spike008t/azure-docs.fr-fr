---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 042aedf1a043cd89d74ff099554642d38a3c7dd3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120675"
---
## <a name="what-is-table-storage"></a>Qu’est-ce qu’un stockage de table ?
Le stockage de table Azure permet de stocker de grandes quantités de données structurées. Il s’agit d’une banque de données NoSQL qui accepte les appels authentifiés provenant de l’intérieur et de l’extérieur du cloud Azure. Les tables Azure sont idéales pour le stockage des données structurées non relationnelles. Voici quelques utilisations courantes du stockage de table :

* Stockage des téraoctets de données structurées capables de servir des applications Web
* Stockage des jeux de données ne nécessitant pas de jonctions complexes, de clés étrangères ou de procédures stockées, et pouvant être dénormalisés pour un accès rapide
* Interrogation rapide des données par requête à l’aide d’un index cluster
* Accès aux données avec le protocole OData et les quêtes LINQ avec les bibliothèques WCF Data Service .NET

Vous pouvez utiliser le stockage de table pour stocker et interroger de grands ensembles de données non relationnelles structurées. Vos tables évoluent en même temps que la demande.

## <a name="table-storage-concepts"></a>Concepts de stockage de table
Le stockage de table est composé des éléments suivants :

![Diagramme des composants du stockage de table][Table1]

* **Format d’URL** : les comptes de stockage Table Azure utilisent ce format : `http://<storage account>.table.core.windows.net/<table>`

  Les comptes de l’API Table d’Azure Cosmos DB utilisent ce format : `http://<storage account>.table.cosmosdb.azure.com/<table>`  

  Vous pouvez traiter les tables Azure directement à l’aide de cette adresse avec le protocole OData. Pour plus d’informations, consultez [OData.org][OData.org].
* **Comptes :** Tous les accès à Azure Storage passent par un compte de stockage. Pour plus d’informations sur la capacité du compte de stockage, consultez la page [Objectifs de performance et évolutivité du stockage Azure](../articles/storage/common/storage-scalability-targets.md) . 

    Tous les accès à Azure Cosmos DB passent par un compte d’API de table. Consultez [Créer un compte d’API de Table](../articles/cosmos-db/create-table-dotnet.md#create-a-database-account) pour plus d’informations sur la création d’un compte d’API de table.
* **Table**: une table est une collection d’entités. Les tables n’appliquent pas de schéma sur les entités, ce qui signifie qu’une seule table peut contenir des entités ayant différents ensembles de propriétés.  
* **Entité** : une entité est un ensemble de propriétés similaire à une ligne de base de données. Une entité dans le stockage Azure peut avoir une taille maximale d’1 Mo. Une entité dans le stockage Azure Cosmos DB peut avoir une taille maximale de 2 Mo.
* **Propriétés** : une propriété est une paire nom-valeur. Chaque entité peut inclure jusqu’à 252 propriétés pour stocker des données. Chaque entité dispose également de trois propriétés système qui spécifient une clé de partition, une clé de ligne et un horodatage. Les entités ayant la même clé de partition peuvent être interrogées plus rapidement et insérées ou mises à jour dans des opérations atomiques. La clé de ligne d'une entité est son identificateur unique à l'intérieur d'une partition.

Pour plus de détails sur l’affectation de noms à des tables et les propriétés, consultez la rubrique [Présentation du modèle de données du service de Table (en anglais)](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
