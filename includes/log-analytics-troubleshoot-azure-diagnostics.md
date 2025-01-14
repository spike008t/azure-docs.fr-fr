---
author: mgoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 11/09/2018
ms.author: magoedte
ms.openlocfilehash: 6890c71ac7c265d46cc77751786fea4d0b228588
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66129698"
---
### <a name="troubleshoot-azure-diagnostics"></a>Résoudre les problèmes de Diagnostics Azure

Si vous recevez le message d’erreur suivant, le fournisseur de ressources Microsoft.insights n’est pas enregistré :

`Failed to update diagnostics for 'resource'. {"code":"Forbidden","message":"Please register the subscription 'subscription id' with Microsoft.Insights."}`

Pour enregistrer le fournisseur de ressources, effectuez les opérations suivantes dans le portail Azure :

1.  Dans le volet de navigation à gauche, cliquez sur *Abonnements*
2.  Sélectionnez l’abonnement identifié dans le message d’erreur
3.  Cliquez sur *Fournisseurs de ressources*
4.  Recherchez le fournisseur *Microsoft.insights*
5.  Cliquez sur le lien *Enregistrer*

![Enregistrez le fournisseur de ressources microsoft.insights](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Une fois le fournisseur de ressources *Microsoft.insights* enregistré, tentez à nouveau de configurer les diagnostics.


Dans PowerShell, si vous recevez le message d’erreur suivant, vous devez mettre à jour votre version de PowerShell :

`Set-AzDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Mettre à jour votre version d’Azure PowerShell, suivez les instructions de la [installer Azure PowerShell](/powershell/azure/install-az-ps) article.
