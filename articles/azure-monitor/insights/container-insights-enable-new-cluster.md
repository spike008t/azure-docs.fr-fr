---
title: Surveiller un cluster Azure Kubernetes Service (AKS) | Microsoft Docs
description: Découvrez comment activer la surveillance pour un cluster Azure Kubernetes Service (AKS) avec Azure Monitor pour l’abonnement de conteneurs.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: d73ab2d5cca4f20f954a0b0e972111d3f395c3c8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65077529"
---
# <a name="enable-monitoring-of-a-new-azure-kubernetes-service-aks-cluster"></a>Activer la surveillance d’un nouveau cluster Azure Kubernetes Service (AKS)

Cet article décrit comment configurer Azure Monitor pour les conteneurs pour surveiller le cluster Kubernetes géré hébergé sur [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) que vous préparez à déployer dans votre abonnement.

Vous pouvez activer la surveillance d’un cluster AKS à l’aide d’une des méthodes prises en charge :

* Azure CLI
* Terraform

## <a name="enable-using-azure-cli"></a>Activer à l’aide d’Azure CLI

Pour activer la supervision d’un nouveau cluster AKS créé avec Azure CLI, suivez l’étape de l’article de démarrage rapide sous la section [Créer un cluster AKS](../../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Si vous avez choisi d’utiliser Azure CLI, vous devez d’abord l’installer et l’utiliser localement. Vous devez exécuter Azure CLI version 2.0.59 ou version ultérieure. Pour identifier votre version, exécutez `az --version`. Si vous devez installer ou mettre à niveau Azure CLI, consultez [Installer Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

## <a name="enable-using-terraform"></a>Activer à l’aide de Terraform

Si vous souhaitez [déployer un nouveau cluster AKS à l’aide de Terraform](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md), vous devez spécifier les arguments nécessaires dans le profil [pour créer un espace de travail Log Analytics](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html), si vous n’en spécifiez pas un existant. 

>[!NOTE]
>Si vous choisissez d’utiliser Terraform, vous devez exécuter le fournisseur de gestion des ressources Azure Terraform version 1.17.0 ou ultérieure.

Pour ajouter Azure Monitor pour les conteneurs à l’espace de travail, consultez [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) et renseignez le profil en incluant [**addon_profile**](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) et en spécifiant **oms_agent**. 

Une fois la surveillance activée et toutes les tâches de configuration terminées correctement, vous pouvez contrôler les performances de votre cluster de deux manières :

* Directement dans le cluster AKS en sélectionnant **Intégrité** dans le volet gauche.
* En cliquant sur la vignette **Conteneurs Monitor Insights** dans la page du cluster AKS pour le cluster sélectionné. Dans Azure Monitor, dans le volet gauche, sélectionnez **Intégrité**. 

  ![Options de sélection d’Azure Monitor pour les conteneurs dans AKS](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

Une fois que vous avez activé la surveillance, 15 minutes peuvent s’écouler avant que vous puissiez voir les métriques d’intégrité du cluster. 

## <a name="verify-agent-and-solution-deployment"></a>Vérifier le déploiement de l’agent et de la solution
Avec la version *06072018* de l’agent (ou ultérieure), vous pouvez vérifier que l’agent et la solution ont été déployés correctement. Avec les versions antérieures de l’agent, vous pouvez uniquement vérifier le déploiement de l’agent.

### <a name="agent-version-06072018-or-later"></a>Agent version 06072018 ou ultérieure
Pour vérifier que l’agent a été correctement déployé, exécutez la commande suivante : 

```
kubectl get ds omsagent --namespace=kube-system
```

La sortie doit ressembler à la suivante, qui indique que l’agent a été correctement déployé :

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Pour vérifier le déploiement de la solution, exécutez la commande suivante :

```
kubectl get deployment omsagent-rs -n=kube-system
```

La sortie doit ressembler à la suivante, qui indique que l’agent a été correctement déployé :

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Versions de l’agent antérieures à la version 06072018

Pour vérifier que la version de l’agent Log Analytics antérieure à la version *06072018* a été correctement déployée, exécutez la commande suivante :  

```
kubectl get ds omsagent --namespace=kube-system
```

La sortie doit ressembler à la suivante, qui indique que l’agent a été correctement déployé :  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>Afficher la configuration avec l’interface de ligne de commande
Utilisez la commande `aks show` pour obtenir des détails comme l’activation ou non de la solution, le resourceID de l’espace de travail Log Analytics, et un résumé détaillé du cluster.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Au bout de quelques minutes, la commande se termine et renvoie des informations formatées JSON sur la solution.  Les résultats de la commande doivent afficher le profil de module complémentaire de supervision et ressemblent à l’exemple de sortie suivant :

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="next-steps"></a>Étapes suivantes

* Si vous rencontrez des problèmes en tentant d’intégrer la solution, consultez le [guide de résolution des problèmes](container-insights-troubleshoot.md)

* Vous pouvez activer la fonctionnalité de supervision pour collecter des métriques d’intégrité pour les nœuds et pods du cluster AKS et les consulter dans le portail Azure. Pour savoir comment utiliser Azure Monitor pour les conteneurs, consultez l’article [Connaître l’état d’Azure Kubernetes Service](container-insights-analyze.md).
