---
title: Fichier Include
description: Fichier Include
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 975bb5ee194e7bd9538660878cbed20c943fcf52
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66151481"
---
1. Dans une nouvelle fenêtre du navigateur, connectez-vous au [portail Azure](https://portal.azure.com/).

2. Cliquez sur **Créer une ressource** > **Bases de données** > **Azure Cosmos DB**.
   
   ![Volet « Bases de données » du portail Azure](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. Dans la page **Créer un compte Azure Cosmos DB**, entrez les paramètres du nouveau compte Azure Cosmos DB. 

    Paramètre|Valeur|Description
    ---|---|---
    Abonnement|Votre abonnement|Sélectionnez l’abonnement Azure que vous souhaitez utiliser pour ce compte Azure Cosmos DB. 
    Groupe de ressources|Création<br><br>Entrez ensuite le même nom unique que celui fourni dans l’ID|Sélectionnez **Créer nouveau**. Entrez ensuite le nom du nouveau groupe de ressources pour votre compte. Pour rester simple, utilisez le même nom que celui de votre ID. 
    Nom du compte|Entrez un nom unique|Entrez un nom unique pour identifier votre compte Azure Cosmos DB. Étant donné que *documents.azure.com* est ajouté à l’ID que vous fournissez pour créer votre URI, utilisez un ID unique.<br><br>L’ID peut uniquement utiliser des lettres minuscules, des chiffres et le caractère de trait d’union (-). Il doit comprendre entre 3 et 31 caractères.
    API|Gremlin (graphique)|L’API détermine le type de compte à créer. Azure Cosmos DB fournit cinq API : Core(SQL) pour les bases de données orientées document, Gremlin pour les bases de données de graphe, MongoDB pour les bases de données de document, Table Azure et Cassandra. Actuellement, vous devez créer un compte distinct pour chaque API. <br><br>Sélectionnez **Gremlin (graphe)**, car dans ce guide de démarrage rapide, vous créez une table qui fonctionne avec l’API Gremlin. <br><br>[Découvrez plus d’informations sur l’API Graph](../articles/cosmos-db/graph-introduction.md).|
    Lieu|Sélectionner la région la plus proche de vos utilisateurs|Sélectionnez la zone géographique dans laquelle héberger votre compte Azure Cosmos DB. Utilisez l’emplacement le plus proche de vos utilisateurs, pour leur donner l’accès le plus rapide possible aux données.

    Sélectionnez **Vérifier + créer**. Vous pouvez ignorer les sections **Réseau** et **Balises**. 

    ![Panneau de nouveau compte pour Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-create-new-account.png)

4. La création du compte prend quelques minutes. Attendez que le portail affiche la page **Félicitations ! Votre compte Azure Cosmos DB a été créé**.

    ![Volet Notifications du portail Azure](./media/cosmos-db-create-dbaccount-graph/azure-cosmos-db-graph-created.png)