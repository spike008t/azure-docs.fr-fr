---
title: 'Script PowerShell : Copier des données en local dans Azure avec Data Factory | Microsoft Docs'
description: Ce script PowerShell copie des données depuis une base de données SQL Server locale vers un stockage Blob Azure.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2017
ms.author: jingwang
ms.openlocfilehash: 1218408bee067d6edb274fdcbf4fae62b8245a01
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66160664"
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-from-on-premises-to-azure"></a>Utiliser PowerShell pour créer un pipeline de fabrique de données afin de copier des données en local dans Azure

Cet exemple de script PowerShell crée dans Azure Data Factory un pipeline qui copie les données d’une base de données SQL Server locale vers un stockage Blob Azure.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Conditions préalables

- **SQL Server**. Dans cet exemple, vous allez utiliser une base de données SQL Server locale comme magasin de données **source**.
- **Compte Stockage Azure**. Dans cet exemple, le stockage Blob Azure est utilisé comme magasin de données de **destination/réception**. Si vous n’avez pas de compte de stockage Azure, consultez l’article [Créer un compte de stockage](../../storage/common/storage-quickstart-create-account.md) pour découvrir comment en créer un.
- **Runtime d’intégration auto-hébergé**. Téléchargez le fichier MSI à partir du [centre de téléchargement](https://www.microsoft.com/download/details.aspx?id=39717) et exécutez-le pour installer un runtime d’intégration auto-hébergé sur votre ordinateur.  

### <a name="create-sample-database-in-sql-server"></a>Créer un exemple de base de données dans SQL Server
1. Dans la base de données SQL Server locale, créez une table nommée **emp** en utilisant le script SQL suivant : 

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Insérez des exemples de données dans la table :

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```

## <a name="sample-script"></a>Exemple de script

> [!IMPORTANT]
> Ce script crée des fichiers JSON qui définissent des entités Data Factory (service lié, jeu de données et pipeline) sur votre disque dur dans le dossier c:\.

```powershell
$resourceGroupName = "<Resource group name>"
$dataFactoryName = "<Data factory name>" # must be globally unique
$storageAccountName = "<Az.Storage account name>"
$storageAccountKey = "<Az.Storage account key>"
$sqlServerName = "<SQL server name>"
$sqlDatabaseName = "SQL Server database name"
$sqlTableName = "emp" # create the emp table if it does not already exist in your database with ID, FirstName, and LastName columns of type String. 
$sqlUserName = "<SQL Authentication - user name>"
$sqlPassword = "<SQL Authentication - user password>"
$blobFolderPath = "<Azure blob container name>/<Azure blob folder name>"
$integrationRuntimeName = "<Self-hosted integration runtime name"
$pipelineName = "SqlServerToBlobPipeline"
$dataFactoryRegion = "China East"

# Create a resource group
New-AzResourceGroup -Name $resourceGroupName -Location $dataFactoryRegion

# create a data factory
$df = Set-AzDataFactory -ResourceGroupName $resourceGroupName -Name $dataFactoryName -Location $dataFactoryRegion

# create a self-hosted integration runtime
Set-AzDataFactoryIntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName

# get the authorization key from the created integration runtime in the cloud
Get-AzDataFactoryIntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json

# IMPORTANT: Install self-hosted integration runtime on your machine and use one of the keys to register the IR installed on your machine with the cloud service

# create an Az.Storage linked service

## JSON definition of the linked service. 
$storageLinkedServiceDefinition = @"
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey",
                "type": "SecureString"
            }
        }
    }
}
"@

## IMPORTANT: stores the JSON definition in a file that will be used by the Set-AzDataFactoryLinkedService command. 
$storageLinkedServiceDefinition | Out-File c:\AzureStorageLinkedService.json

## Creates a linked service in the data factory
Set-AzDataFactoryLinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -File c:\AzureStorageLinkedService.json

# create an on-premises SQL Server linked service

## JSON definition of the linked service. 
$sqlServerLinkedServiceDefinition = @"
{
   "properties": {
     "type": "SqlServer",
     "typeProperties": {
         "connectionString": {
             "type": "SecureString",
            "value": "Server=$sqlServerName;Database=$sqlDatabaseName;User ID=$sqlUserName;Password=$sqlPassword;Timeout=60"
         }
     },
     "connectVia": {
       "type": "integrationRuntimeReference",
       "referenceName": "$integrationRuntimeName"
     }
 },
 "name": "SqlServerLinkedService"
}
"@

## IMPORTANT: stores the JSON definition in a file that will be used by the Set-AzDataFactoryLinkedService command. 
$sqlServerLinkedServiceDefinition | Out-File c:\SqlServerLinkedService.json

## Encrypt SQL Server credentials 
New-AzDataFactoryLinkedServiceEncryptCredential -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File "c:\SqlServerLinkedService.json" > c:\EncryptedSqlServerLinkedService.json

# Create a SQL Server linked service
Set-AzDataFactoryLinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName "EncryptedSqlServerLinkedService" -File "c:\EncryptedSqlServerLinkedService.json"


# Create a source dataset for source SQL Server Database

## JSON definition of the dataset
$sourceSqlServerDatasetDefiniton = @"
{
   "properties": {
        "type": "SqlServerTable",
        "typeProperties": {
            "tableName": "$sqlTableName"
        },
        "structure": [
             {
                "name": "ID",
                "type": "String"
            },
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
        ],
        "linkedServiceName": {
            "referenceName": "EncryptedSqlServerLinkedService",
            "type": "LinkedServiceReference"
        }
    },
    "name": "SqlServerDataset"
}
"@

## IMPORTANT: store the JSON definition in a file that will be used by the Set-AzDataFactoryDataset command. 
$sourceSqlServerDatasetDefiniton | Out-File c:\SqlServerDataset.json

# Create an Azure Blob dataset in the data factory
Set-AzDataFactoryDataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File "c:\SqlServerDataset.json"

# Create a dataset for sink Azure Blob Storage

## JSON definition of the dataset
$sinkBlobDatasetDefiniton = @"
{
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "$blobFolderPath",
            "format": {
                "type": "TextFormat"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    },
    "name": "AzureBlobDataset"
}
"@

## IMPORTANT: store the JSON definition in a file that will be used by the Set-AzDataFactoryDataset command. 
$sinkBlobDatasetDefiniton | Out-File c:\AzureBlobDataset.json

## Create the Azure Blob dataset
Set-AzDataFactoryDataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File "c:\AzureBlobDataset.json"


# Create a pipeline in the data factory

## JSON definition of the pipeline
$pipelineDefinition = @"
{
   "name": "$pipelineName",
    "properties": {
        "activities": [       
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource"
                    },
                    "sink": {
                        "type":"BlobSink"
                    }
                },
                "name": "CopySqlServerToAzureBlobActivity",
                "inputs": [
                    {
                        "referenceName": "SqlServerDataset",
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "AzureBlobDataset",
                        "type": "DatasetReference"
                    }
                ]
            }
        ]
    }
}
"@

## IMPORTANT: store the JSON definition in a file that will be used by the Set-AzDataFactoryPipeline command. 
$pipelineDefinition | Out-File c:\SqlServerToBlobPipeline.json

## Create a pipeline in the data factory
Set-AzDataFactoryPipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "$pipelineName" -File "c:\SqlServerToBlobPipeline.json"


# start the pipeline run
$runId = Invoke-AzDataFactoryPipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName

# Check the pipeline run status until it finishes the copy operation
while ($True) {
    $result = Get-AzDataFactoryActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

    if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
        Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
        Start-Sleep -Seconds 30
    }
    else {
        Write-Host "Pipeline $pipelineName run finished. Result:" -foregroundcolor "Yellow"
        $result
        break
    }
}

# Get the activity run details 
    $result = Get-AzDataFactoryActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName `
        -PipelineRunId $runId `
        -RunStartedAfter (Get-Date).AddMinutes(-10) `
        -RunStartedBefore (Get-Date).AddMinutes(10) `
        -ErrorAction Stop

    $result

    if ($result.Status -eq "Succeeded") {`
        $result.Output -join "`r`n"`
    }`
    else {`
        $result.Error -join "`r`n"`
    }

# To remove the data factory from the resource gorup
# Remove-AzDataFactory -Name $dataFactoryName -ResourceGroupName $resourceGroupName
# 
# To remove the whole resource group
# Remove-AzResourceGroup  -Name $resourceGroupName
```


## <a name="clean-up-deployment"></a>Nettoyer le déploiement

Après avoir exécuté l’exemple de script, vous pouvez utiliser la commande suivante pour supprimer le groupe de ressources et toutes les ressources associées :

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
Pour supprimer la fabrique de données du groupe de ressources, exécutez la commande suivante : 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes : 

| Commande | Notes |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Créer une fabrique de données. |
| [New-AzDataFactoryV2LinkedServiceEncryptCredential](/powershell/module/az.datafactory/new-Azdatafactoryv2linkedserviceencryptedcredential) | Chiffre les informations d’identification dans un service lié et génère une nouvelle définition de service lié à l’aide des informations d’identification chiffrées. 
| [Set-AzDataFactoryV2LinkedService](/powershell/module/az.datafactory/Set-Azdatafactoryv2linkedservice) | Crée un service lié dans la fabrique de données. Un service lié rattache une banque de données ou une ressource de calcul à une fabrique de données. |
| [Set-AzDataFactoryV2Dataset](/powershell/module/az.datafactory/Set-Azdatafactoryv2dataset) | Crée un jeu de données dans la fabrique de données. Un jeu de données représente les entrées/sorties pour une activité dans un pipeline. | 
| [Set-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Set-Azdatafactoryv2pipeline) | Crée un pipeline dans la fabrique de données. Un pipeline contient une ou plusieurs activités effectuant une opération donnée. Dans ce pipeline, l’activité de copie a pour effet de copier les données d’un emplacement vers un autre dans le Stockage Blob Azure. |
| [Invoke-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Invoke-Azdatafactoryv2pipeline) | Crée une exécution pour le pipeline. En d’autres termes, exécute le pipeline. |
| [Get-AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-Azdatafactoryv2activityrun) | Obtient les détails de l’exécution de l’activité dans le pipeline. 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Supprime un groupe de ressources, y compris toutes les ressources imbriquées. |
|||

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Azure PowerShell, consultez la [documentation Azure PowerShell](https://docs.microsoft.com/powershell/).

Des exemples supplémentaires de scripts PowerShell pour Azure Data Factory sont à votre disposition dans [Exemples PowerShell pour Azure Data Factory](../samples-powershell.md).
