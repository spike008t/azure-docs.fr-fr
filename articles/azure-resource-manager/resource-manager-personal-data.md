---
title: Données personnelles Azure Resource Manager | Microsoft Docs
description: Découvrez comment gérer les données personnelles associées aux opérations Azure Resource Manager.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: db4e1b8705b879fd5716763869462bafdf1f905c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66128469"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Gérer les données personnelles associées à Azure Resource Manager

Pour éviter d’exposer des informations sensibles, supprimez toutes les informations personnelles que vous avez éventuellement fournies dans les déploiements, les groupes de ressources ou les balises. Azure Resource Manager propose des opérations qui vous permettent de gérer les données personnelles que vous avez éventuellement fournies dans les déploiements, les groupes de ressources ou les balises.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Supprimer les données personnelles de l’historique du déploiement

Pour les déploiements, Resource Manager conserve les valeurs des paramètres et les messages d’état dans l’historique du déploiement. Ces valeurs sont conservées jusqu'à ce que vous supprimiez le déploiement de l’historique. Pour voir si vous avez fourni des données personnelles dans ces valeurs, consultez la liste des déploiements. Si vous trouvez des données personnelles, supprimez les déploiements de l’historique.

Pour afficher la liste des **déploiements** dans l’historique, utilisez :

* [Filtrer par groupe de ressources](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az group deployment list](/cli/azure/group/deployment#az-group-deployment-list)

Pour supprimer des **déploiements** de l’historique, utilisez :

* [Supprimer](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az group deployment delete](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Supprimer les données personnelles des noms de groupes de ressources

Le nom du groupe de ressources persiste jusqu'à ce que vous supprimiez le groupe de ressources. Pour voir si vous avez fourni des données personnelles dans les noms, consultez la liste des groupes de ressources. Si vous trouvez des données personnelles, [déplacez les ressources](resource-group-move-resources.md) vers un nouveau groupe de ressources puis supprimez le groupe de ressources dont le nom contient des données personnelles.

Pour répertorier les **groupes de ressources**, utilisez :

* [Liste](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [az group list](/cli/azure/group#az-group-list)

Pour supprimer des **groupes de ressources**, utilisez :

* [Supprimer](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Supprimer des données personnelles dans des balises

Les noms et les valeurs des balises persistent jusqu'à ce que vous supprimez ou modifiez la balise. Pour voir si vous avez fourni des données personnelles dans les balises, consultez la liste des balises. Si vous trouvez des données personnelles, supprimez les balises.

Pour afficher la liste des **balises**, utilisez :

* [Liste](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [az tag list](/cli/azure/tag#az-tag-list)

Pour supprimer des **balises**, utilisez :

* [Supprimer](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [az tag delete](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Étapes suivantes
* Pour une présentation d’Azure Resource Manager, consultez [Présentation de Resource Manager](resource-group-overview.md).