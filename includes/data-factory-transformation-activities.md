---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: eabfca64a4bdf1f72375575161111ddaff55eba3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66123443"
---
Azure Data Factory prend en charge les activités de transformation suivantes, qui peuvent être ajoutées à des pipelines, soit individuellement soit de façon chaînée avec une autre activité.

| Activités de transformation des données | Environnement de calcul |
|:--- |:--- |
| [Hive](../articles/data-factory/v1/data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](../articles/data-factory/v1/data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](../articles/data-factory/v1/data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Diffusion en continu Hadoop](../articles/data-factory/v1/data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Spark](../articles/data-factory/v1/data-factory-spark.md) | HDInsight [Hadoop] |
| [Activités Machine Learning : exécution par lot et ressource de mise à jour](../articles/data-factory/v1/data-factory-azure-ml-batch-execution-activity.md) |Microsoft Azure |
| [Procédure stockée](../articles/data-factory/v1/data-factory-stored-proc-activity.md) |SQL Azure, Azure SQL Data Warehouse ou SQL Server |
| [Langage U-SQL du service Analytique Data Lake](../articles/data-factory/v1/data-factory-usql-activity.md) |Service Analytique Azure Data Lake |
| [DotNet](../articles/data-factory/v1/data-factory-use-custom-activities.md) |HDInsight [Hadoop] ou Azure Batch |

> [!NOTE]
> Vous pouvez utiliser l'activité MapReduce pour exécuter des programmes Spark sur votre cluster HDInsight Spark. Consultez la page [Appeler des programmes Spark à partir d’Azure Data Factory](../articles/data-factory/v1/data-factory-spark.md) pour plus d’informations.
> Vous pouvez créer une activité personnalisée pour exécuter des scripts R sur votre cluster HDInsight si R est installé. Consultez la page [Exécuter des scripts R avec Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).
> 
> 

