---
title: Utiliser l’explorateur Azure Cosmos DB pour gérer vos données
description: L’explorateur Azure Cosmos DB est une interface web autonome qui vous permet de voir et de gérer les données stockées dans Azure Cosmos DB.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dech
ms.openlocfilehash: 04cfdd1f96f83898710b6f292116f0afddc8df96
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237232"
---
# <a name="work-with-data-using-azure-cosmos-explorer"></a>Utiliser des données à l’aide d’Azure Cosmos Explorer 

L’explorateur Azure Cosmos DB est une interface web autonome qui vous permet de voir et de gérer les données stockées dans Azure Cosmos DB. L’explorateur Azure Cosmos DB est l’équivalent de l’onglet **Explorateur de données** qui se trouve dans le portail Azure lorsque vous créez un compte Azure Cosmos DB. Les principaux avantages de l’explorateur Azure Cosmos DB par rapport à l’Explorateur de données existant sont les suivants :

* Vous disposez d’une véritable vue plein écran pour consulter vos données, exécuter des requêtes, des procédures stockées, des déclencheurs et voir leurs résultats.  

* Vous pouvez fournir un accès temporaire ou permanent, en lecture-écriture ou en lecture seule, de votre compte de base de données et de ses collections, à d’autres utilisateurs qui n’ont pas accès à l’abonnement ou au portail Azure.  

* Vous pouvez partager les résultats de requête avec d’autres utilisateurs qui n’ont pas accès à l’abonnement ou au portail Azure.  

## <a name="access-azure-cosmos-db-explorer"></a>Accéder à l’explorateur Azure Cosmos DB

1. Connectez-vous au [portail Azure](https://portal.azure.com/). 

2. À partir de **Toutes les ressources**, recherchez et accédez à votre compte Azure Cosmos DB, sélectionnez Clés et copiez la **Chaîne de connexion principale**.  

3. Accédez à https://cosmos.azure.com/, collez la chaîne de connexion, puis sélectionnez **Connexion**. À l’aide de la chaîne de connexion, vous pouvez accéder à l’explorateur Azure Cosmos DB sans aucune limitation de durée.  

   Si vous souhaitez fournir à d’autres utilisateurs un accès temporaire à votre compte Azure Cosmos DB, vous pouvez le faire par le biais d’URL d’accès en lecture-écriture et en lecture. 

4. Ouvrez le panneau **Explorateur de données** et sélectionnez **Ouvrir en mode plein écran**. Dans la boîte de dialogue contextuelle, vous pouvez voir deux URL d’accès : **Lecture-écriture** et **Lecture**. Ces URL vous permettent de partager temporairement votre compte Azure Cosmos DB avec d’autres utilisateurs. L’accès au compte expire au bout de 24 heures, après quoi vous pouvez vous reconnecter au moyen d’une nouvelle URL d’accès, ou de la chaîne de connexion. 

   **Lecture-écriture** : lorsque vous partagez l’URL en lecture-écriture avec d’autres utilisateurs, ceux-ci peuvent voir et modifier les bases de données, collections, requêtes et d’autres ressources associées à ce compte particulier.

   **Lecture** : lorsque vous partagez l’URL en lecture seule avec d’autres utilisateurs, ceux-ci peuvent afficher les bases de données, collections, requêtes et d’autres ressources associées à ce compte particulier. Par exemple, si vous souhaitez partager les résultats d’une requête avec vos collègues qui n’ont pas accès au portail Azure ou à votre compte Azure Cosmos DB, vous pouvez leur fournir cette URL.

   Choisissez le type d’accès avec lequel vous souhaitez ouvrir le compte et cliquez sur **Ouvrir**. Une fois l’Explorateur ouvert, l’expérience est identique à celle que vous aviez avec l’onglet Explorateur de données dans le portail Azure.   

   ![Ouvrir l’explorateur Azure Cosmos DB](./media/data-explorer/open-data-explorer-with-access-url.png)

## <a name="known-issues"></a>Problèmes connus

Pour le moment, l’expérience **Ouvrir en mode plein écran** qui vous permet de partager un accès temporaire en lecture-écriture ou en lecture seule n’est pas encore prise en charge pour les comptes Azure Cosmos DB Gremlin et API Table. Vous pouvez toujours afficher vos comptes Gremlin et API Table en passant la chaîne de connexion à l’explorateur Azure Cosmos DB. 

## <a name="next-steps"></a>Étapes suivantes
Maintenant que vous avez découvert comment démarrer avec l’explorateur Azure Cosmos DB pour gérer vos données, vous pouvez :

* Commencer à définir des [requêtes](sql-api-query-reference.md) à l’aide de la syntaxe SQL, et faire de la [programmation côté serveur](stored-procedures-triggers-udfs.md) en vous servant de procédures stockées, d’UDF et de déclencheurs. 
