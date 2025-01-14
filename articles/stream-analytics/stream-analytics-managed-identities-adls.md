---
title: Authentifier des travaux Azure Stream Analytique à la sortie d’Azure Data Lake Storage Gen1
description: Cet article explique comment utiliser des identités managées afin d’authentifier votre tâche Azure Stream Analytics pour la sortie Azure Data Lake Storage Gen1.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2019
ms.custom: seodec18
ms.openlocfilehash: 695591fedfacb34742335a6e9d6ca32a9c77eb7e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66148540"
---
# <a name="authenticate-stream-analytics-to-azure-data-lake-storage-gen1-using-managed-identities"></a>Authentifier Analytique Stream pour Azure Data Lake Storage Gen1 à l’aide d’identités gérées

Azure Stream Analytics prend en charge l’authentification des identités managées avec la sortie Azure Data Lake Storage (ADLS) Gen1. L’identité est une application managée inscrite auprès d’Azure Active Directory. Elle représente un travail Stream Analytics spécifique et peut servir à l’authentification auprès d’une ressource cible. Les identités managées n’ont pas les limitations des méthodes d’authentification basée sur l’utilisateur, comme la réauthentification obligatoire après un changement de mot de passe ou l’expiration du jeton d’utilisateur tous les 90 jours. De plus, les identités managées facilitent l’automatisation des déploiements de travaux Stream Analytics dont la sortie est générée dans Azure Data Lake Storage Gen1.

Cet article présente trois façons d'activer une identité managée pour un travail Azure Stream Analytics dont la sortie est générée dans Azure Data Lake Storage Gen1 à l'aide du portail Azure, d'un déploiement de modèle Azure Resource Manager et d'Azure Stream Analytics Tools pour Visual Studio.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-portal"></a>Portail Azure

1. Commencez par créer un travail Stream Analytics ou par ouvrir un travail existant dans le portail Azure. Dans la barre de menus située sur le côté gauche de l’écran, sélectionnez **msi** situé sous **configurer**.

   ![Configurer l’identité de l’Analytique Stream gérés](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Sélectionnez **attribué par le système de l’utilisation des identités gérées** à partir de la fenêtre qui apparaît à droite. Cliquez sur **enregistrer** à un principal de service pour l’identité de la tâche Analytique de Stream dans Azure Active Directory. Le cycle de vie de la nouvelle identité est géré par Azure. Quand le travail Stream Analytics est supprimé, l’identité associée (autrement dit, le principal de service) est également automatiquement supprimée par Azure.

   Une fois que la configuration est enregistrée, l’ID objet (l’OID) du principal de service s’affiche en tant qu’ID de principal, comme ci-dessous :

   ![ID du principal de service Stream Analytics](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   Le principal de service a le même nom que le travail Stream Analytics. Par exemple, si le nom de votre travail est **MyASAJob**, le nom du principal de service créé est également **MyASAJob**.

3. Dans la fenêtre de propriétés de sortie du récepteur de sortie ADLS Gen1, cliquez sur le mode d’authentification liste déroulante et sélectionnez ** msi **.

4. Renseignez le reste des propriétés. Pour en savoir plus sur la création d’une sortie ADLS, consultez [Créer une sortie Data Lake Storage avec Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md). Quand vous avez terminé, cliquez sur **Enregistrer**.

   ![Configurer Azure Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. Accédez à la page Vue d’ensemble de votre sortie ADLS Gen1 et cliquez sur **Explorateur de données**.

   ![Configurer Data Lake Storage - Vue d’ensemble](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Dans le volet Explorateur de données, sélectionnez **Accès** et cliquez sur **Ajouter** dans le volet Accès.

   ![Configurer Data Lake Storage - Accès](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. Dans la zone de texte du volet **Sélectionner un utilisateur ou un groupe**, tapez le nom du principal de service. N’oubliez pas que le nom du principal de service est également le nom du travail Stream Analytics correspondant. Quand vous commencez à taper le nom du principal, celui-ci s’affiche sous la zone de texte. Choisissez le nom du principal de service souhaité en cliquant sur **Sélectionner**.

   ![Sélectionner un nom de principal de service](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. Dans le volet **Autorisations**, sélectionnez les autorisations **Écriture** et **Exécution**, et assignez-les à **Ce dossier et tous les enfants**. Cliquez ensuite sur **OK**.

   ![Sélectionner les autorisations d’écriture et d’exécution](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. Le principal de service est répertorié sous **Autorisations assignées** dans le volet **Accès** comme illustré ci-dessous. Vous pouvez maintenant revenir en arrière et lancer votre travail Stream Analytics.

   ![Liste d’accès Stream Analytics dans le portail](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Pour en savoir plus sur les autorisations de système de fichiers pour Data Lake Storage Gen1, consultez [Contrôle d’accès dans Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

## <a name="stream-analytics-tools-for-visual-studio"></a>Azure Stream Analytics Tools pour Visual Studio

1. Dans JobConfig.json, définissez **Utiliser l’identité affectée par le système** sur **True**.

   ![Identités managées dans la configuration du travail Stream Analytics](./media/stream-analytics-managed-identities-adls/adls-mi-jobconfig-vs.png)

2. Dans la fenêtre de propriétés de sortie du récepteur de sortie ADLS Gen1, cliquez sur le mode d’authentification liste déroulante et sélectionnez ** msi **.

   ![Identités managées générées par ADLS](./media/stream-analytics-managed-identities-adls/adls-mi-output-vs.png)

3. Renseignez le reste des propriétés et cliquez sur **Enregistrer**.

4. Cliquez sur **Envoyer sur Azure** dans l’éditeur de requête.

   Quand vous envoyez le travail, les outils effectuent deux choses :

   * Ils créent automatiquement un principal de service pour l’identité associée au travail Stream Analytics dans Azure Active Directory. Le cycle de vie de la nouvelle identité est géré par Azure. Quand le travail Stream Analytics est supprimé, l’identité associée (autrement dit, le principal de service) est également automatiquement supprimée par Azure.

   * Ils définissent automatiquement les autorisations **Écrire** et **Exécuter** pour le chemin de préfixe ADLS Gen1 utilisé dans le travail, et l’attribuent à ce dossier et à tous ses enfants.

5. Vous pouvez générer les modèles Resource Manager avec la propriété suivante à l’aide du [package Nuget Stream Analytics CI.CD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/) version 1.5.0 ou ultérieure sur une machine de build (en dehors de Visual Studio). Suivez les étapes de déploiement du modèle Resource Manager dans la section suivante pour obtenir le principal de service et lui accorder l’accès via PowerShell.

## <a name="resource-manager-template-deployment"></a>Déploiement de modèle Resource Manager

1. Vous pouvez créer une ressource *Microsoft.StreamAnalytics/streamingjobs* avec une identité managée en incluant la propriété suivante dans la section ressources de votre modèle Resource Manager :

    ```json
    "Identity": {
      "Type": "SystemAssigned",
    },
    ```

   Cette propriété indique à Azure Resource Manager de créer et manager l’identité de votre travail Azure Stream Analytics.

   **Exemple de travail**
   
   ```json
   {
     "Name": "AsaJobWithIdentity",
     "Type": "Microsoft.StreamAnalytics/streamingjobs",
     "Location": "West US",
     "Identity": {
       "Type": "SystemAssigned",
     },
     "properties": {
       "sku": {
         "name": "standard"
       },
       "outputs": [
         {
           "name": "string",
           "properties":{
             "datasource": {
               "type": "Microsoft.DataLake/Accounts",
               "properties": {
                 "accountName": "myDataLakeAccountName",
                 "filePathPrefix": "cluster1/logs/{date}/{time}",
                 "dateFormat": "YYYY/MM/DD",
                 "timeFormat": "HH",
                 "authenticationMode": "Msi"
             }
           }
         }
       }
     }
   }
   ```
  
   **Exemple de réponse du travail**

   ```json
   {
    "Name": "mySAJob",
    "Type": "Microsoft.StreamAnalytics/streamingjobs",
    "Location": "West US",
    "Identity": {
      "Type": "SystemAssigned",
        "principalId": "GUID",
        "tenantId": "GUID",
      },
      "properties": {
        "sku": {
          "name": "standard"
        },
     }
   }
   ```

   Prenez note de l’ID du principal indiqué dans la réponse du travail pour accorder l’accès à la ressource ADLS requise.

   La valeur **ID du locataire** est l’ID du locataire Azure Active Directory où le principal de service est créé. Le principal de service est créé dans le locataire Azure approuvé par l’abonnement.

   Le **Type** indique le type de l’identité managée, comme expliqué dans les types d’identités managées. Seul le type Affecté par le système est pris en charge.

2. Fournissez l’accès au principal de service à l’aide de PowerShell. Pour donner accès au principal de service avec PowerShell, exécutez la commande suivante :

   ```powershell
   Set-AzDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   La valeur **PrincipalId** correspond à l’ID objet du principal de service, qui s’affiche sur le portail une fois que le principal de service est créé. Si vous avez créé le travail à l’aide d’un déploiement de modèle Resource Manager, l’ID objet est indiqué dans la propriété Identity contenue dans la réponse du travail.

   **Exemple**

   ```powershell
   PS > Set-AzDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Pour en savoir plus sur la commande PowerShell ci-dessus, reportez-vous à la [Set-AzDataLakeStoreItemAclEntry](/powershell/module/az.datalakestore/set-azdatalakestoreitemaclentry) documentation.

## <a name="limitations"></a>Limites
Cette fonctionnalité ne prend en charge les éléments suivants :

1. **Accès de l’architecture mutualisé**: Le principal du Service créé pour un travail Stream Analytique donné résident sur le client Azure Active Directory sur lequel le travail a été créé et ne peut pas être utilisé sur une ressource qui réside sur un autre locataire Azure Active Directory. Par conséquent, vous pouvez uniquement utiliser MSI sur les ressources ADLS Gen 1 qui se trouvent dans le même locataire Azure Active Directory que votre travail Azure Stream Analytique. 

2. **[Utilisateur de l’identité affectée](../active-directory/managed-identities-azure-resources/overview.md)**: n’est pas pris en charge. Cela signifie que l’utilisateur n’est pas en mesure d’entrer leur propres principal de service à utiliser par leur travail Analytique de Stream. Le principal du service est généré par Azure Stream Analytique.

## <a name="next-steps"></a>Étapes suivantes

* [Créer une sortie Data Lake Store avec Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md)
* [Tester des requêtes Stream Analytics localement avec Visual Studio](stream-analytics-vs-tools-local-run.md)
* [Tester des données actives localement à l’aide d’Azure Stream Analytics Tools pour Visual Studio](stream-analytics-live-data-local-testing.md) 
