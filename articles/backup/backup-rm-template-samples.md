---
title: Modèles Azure Resource Manager pour la Sauvegarde Azure
description: Exemples Azure Backup PowerShell
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: sample
ms.date: 01/31/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bed2c002cacdac1e47852e6fa3181885af6733d2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236798"
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Modèles Azure Resource Manager pour la Sauvegarde Azure

Le tableau suivant inclut des liens vers des modèles Azure Resource Manager à utiliser avec les coffres Recovery Services et les fonctionnalités de Sauvegarde Azure. Pour découvrir la syntaxe et les propriétés JSON, consultez [Types de ressources Microsoft.RecoveryServices](/azure/templates/microsoft.recoveryservices/allversions).

|   |   |
|---|---|
|**Coffre Recovery Services** | |
| [Créer un coffre Recovery Services](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Créez un coffre Recovery Services. Le coffre peut être utilisé pour la Sauvegarde Azure et Azure Site Recovery. |
|**Sauvegarde de machines virtuelles**| |
| [Sauvegarde des machines virtuelles Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Utilisez le coffre Recovery Services existant et la stratégie de sauvegarde pour sauvegarder les machines virtuelles Azure Resource Manager dans le même groupe de ressources.|
| [Sauvegarde des machines virtuelles IaaS dans le coffre Recovery Services](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Modèle pour sauvegarder les machines virtuelles classiques et Azure Resource Manager. |
| [Création d’une stratégie de sauvegarde hebdomadaire pour les machines virtuelles IaaS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | Le modèle crée un coffre Recovery Services et une stratégie de sauvegarde hebdomadaire utilisée pour sauvegarder des machines virtuelles classiques et Azure Resource Manager.|
| [Création d’une stratégie de sauvegarde quotidienne pour les machines virtuelles IaaS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | Le modèle crée un coffre Recovery Services et une stratégie de sauvegarde quotidienne utilisée pour sauvegarder des machines virtuelles classiques et Azure Resource Manager.|
| [Déploiement d’une machine virtuelle Windows Server avec la fonctionnalité de sauvegarde activée](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | Le modèle crée une machine virtuelle Windows Server et un coffre Recovery Services avec la stratégie de sauvegarde par défaut activée.|
|**Surveillance des travaux de sauvegarde** |  |
| [Utiliser les journaux d’activité Azure Monitor avec Sauvegarde Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | Le modèle déploie les journaux d’activité Azure Monitor avec la Sauvegarde Azure, ce qui vous permet de superviser les travaux de sauvegarde et de restauration, ainsi que les alertes de sauvegarde et le stockage cloud utilisés dans vos coffres Recovery Services.|  
|**Sauvegarder SQL Server sur une machine virtuelle Azure** |  |
| [Sauvegarder SQL Server sur une machine virtuelle Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup) | Ce modèle crée un coffre Recovery Services et une stratégie de sauvegarde spécifique aux charges de travail. Il inscrit la machine virtuelle auprès du service Sauvegarde Azure et configure la protection sur cette machine virtuelle. Ceci fonctionne uniquement pour les images de la galerie SQL. |
|   |   |
