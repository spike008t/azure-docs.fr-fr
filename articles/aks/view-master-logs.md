---
title: Afficher les journaux d’activité de contrôleur Azure Kubernetes Service (AKS)
description: Découvrez comment activer et afficher les journaux d’activité relatifs au nœud principal Kubernetes dans Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/03/2019
ms.author: iainfou
ms.openlocfilehash: 256101cce5588f56a8094a7a9a98e5fe69e6ec73
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497250"
---
# <a name="enable-and-review-kubernetes-master-node-logs-in-azure-kubernetes-service-aks"></a>Activer et consulter les journaux d’activité du nœud principal Kubernetes dans Azure Kubernetes Service (AKS)

Avec Azure Kubernetes Service (AKS), les composants principaux tels que *kube-apiserver* et *kube-controller-manager* sont fournis sous la forme d’un service administré. Vous créez et gérez les nœuds qui exécutent le *kubelet* et le runtime de conteneurs, et vous déployez vos applications par le biais du serveur d’API de l’environnement Kubernetes managés. Pour faciliter la résolution des problèmes de votre application et de vos services, vous pouvez avoir besoin d’afficher les journaux d’activité générés par ces composants principaux. Cet article vous indique comment utiliser Azure Monitor pour activer et interroger les journaux d’activité provenant des composants principaux Kubernetes.

## <a name="before-you-begin"></a>Avant de commencer

Cet article requiert l’exécution d’un cluster AKS existant dans votre compte Azure. Si vous ne disposez pas encore d’un cluster AKS, créez-en un à l’aide de [l’interface de ligne de commande Azure (Azure CLI)][cli-quickstart] ou du [Portail Azure][portal-quickstart]. Les journaux d’activité Azure Monitor fonctionne avec des clusters AKS sur lesquels le contrôle d’accès en fonction du rôle (RBAC) est activé ou non.

## <a name="enable-diagnostics-logs"></a>Activer les journaux de diagnostic

Pour faciliter la collecte et l’analyse des données provenant de plusieurs sources, les journaux d’activité Azure Monitor fournissent un langage de requêtes et un moteur d’analyse vous permettant d’obtenir des insights sur votre environnement. Un espace de travail est utilisé pour assembler et analyser les données, et peut s’intégrer à d’autres services Azure tels qu’Application Insights et Security Center. Si vous souhaitez analyser les journaux d’activité à l’aide d’une autre plateforme, vous pouvez plutôt choisir d’envoyer les journaux de diagnostic à un compte de stockage ou à un Event Hub Azure. Pour plus d’informations, consultez [Présentation des journaux d’activité Azure Monitor][log-analytics-overview].

Journaux d’Azure Monitor sont activées et gérées dans le portail Azure. Pour activer la collecte des journaux relatifs aux composants principaux Kubernetes dans votre cluster AKS, ouvrez le Portail Azure dans un navigateur web et procédez comme suit :

1. Sélectionnez le groupe de ressources de votre cluster AKS, par exemple *myResourceGroup*. Ne sélectionnez pas le groupe de ressources qui contient vos ressources de cluster AKS individuelles, comme *MC_myResourceGroup_myAKSCluster_eastus*.
1. Sur le côté gauche, choisissez **Paramètres de diagnostic**.
1. Sélectionnez votre cluster AKS, tel que *myAKSCluster*, puis choisissez à **ajouter le paramètre de diagnostic**.
1. Entrez un nom, par exemple *myAKSClusterLogs*, puis sélectionnez l’option pour **Envoyer à Log Analytics**.
1. Sélectionnez un espace de travail existant ou créez-en un. Si vous créez un espace de travail, fournissez un nom d’espace de travail, un groupe de ressources et un emplacement.
1. Dans la liste des journaux d’activité disponibles, sélectionnez les journaux d’activité que vous souhaitez activer. Journaux courants incluent le *kube-apiserver*, *kube-contrôleur-manager*, et *kube-planificateur*. Vous pouvez activer des journaux d’activité supplémentaires, tels que *kube-audit* et *cluster-autoscaler*. Vous pourrez réaccéder à cet emplacement et modifier les journaux d’activité collectés une fois les espaces de travail Log Analytics activés.
1. Lorsque vous avez terminé, sélectionnez **Enregistrer** pour activer la collecte des journaux d’activité sélectionnés.

> [!NOTE]
> AKS ne capture les journaux d’audit que pour les clusters créés ou mis à niveau après l’activation d’un indicateur de fonctionnalité sur votre abonnement. Pour enregistrer l'indicateur de fonctionnalité *AKSAuditLog*, utilisez la commande [az feature register][az-feature-register], comme indiqué dans l'exemple suivant :
>
> `az feature register --name AKSAuditLog --namespace Microsoft.ContainerService`
>
> Attendez que l'état affiche *Enregistré*. Vous pouvez vérifier l'état de l'enregistrement à l'aide de la commande [az feature list][az-feature-list] :
>
> `az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAuditLog')].{Name:name,State:properties.state}"`
>
> Lorsque vous êtes prêt, actualisez l'enregistrement du fournisseur de ressources AKS à l'aide de la commande [az provider register][az-provider-register] :
>
> `az provider register --namespace Microsoft.ContainerService`

L’exemple de portail capture d’écran suivante montre le *les paramètres de diagnostic* fenêtre :

![Activer l’espace de travail Log Analytics pour les journaux d’activité Azure Monitor dans un cluster AKS](media/view-master-logs/enable-oms-log-analytics.png)

## <a name="schedule-a-test-pod-on-the-aks-cluster"></a>Planifier un pod test sur le cluster AKS

Pour générer certains journaux d’activité, créez un pod dans votre cluster AKS. Vous pouvez utiliser l’exemple de manifeste YAML ci-après pour créer une instance NGINX de base. Créez un fichier nommé `nginx.yaml` dans un éditeur de votre choix, puis collez-y le contenu suivant :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    ports:
    - containerPort: 80
```

Créez le pod avec la commande [kubectl create][kubectl-create] et spécifiez votre fichier YAML, comme indiqué dans l’exemple suivant :

```
$ kubectl create -f nginx.yaml

pod/nginx created
```

## <a name="view-collected-logs"></a>Afficher les journaux d’activité collectés

L’activation et l’affichage des journaux de diagnostic dans l’espace de travail Log Analytics peuvent nécessiter quelques minutes. Dans le portail Azure, sélectionnez le groupe de ressources pour votre espace de travail Analytique de journal, tel que *myResourceGroup*, puis choisissez votre ressource d’analytique de journal, tel que *myAKSLogs*.

![Sélection de l’espace de travail Log Analytics pour votre cluster AKS](media/view-master-logs/select-log-analytics-workspace.png)

Sur le côté gauche, choisissez **Journaux d’activité**. Pour afficher le journal *kube-apiserver*, entrez la requête ci-après dans la zone de texte :

```
AzureDiagnostics
| where Category == "kube-apiserver"
| project log_s
```

L’application vous renvoie alors probablement un grand nombre de journaux d’activité concernant le serveur d’API. Pour réduire l’étendue des résultats de la requête de façon à n’afficher que les journaux d’activité relatifs au pod NGINX créé à l’étape précédente, ajoutez une instruction *where* supplémentaire afin de rechercher *pods/nginx*, comme indiqué dans l’exemple de requête suivant :

```
AzureDiagnostics
| where Category == "kube-apiserver"
| where log_s contains "pods/nginx"
| project log_s
```

Les journaux d’activité propres à votre pod NGINX s’affichent, comme illustré dans l’exemple de capture d’écran suivant :

![Résultats de la requête Log Analytics pour l’exemple de pod NGINX](media/view-master-logs/log-analytics-query-results.png)

Pour afficher des journaux d’activité supplémentaires, vous pouvez mettre à jour la requête en redéfinissant le nom *Catégorie* sur *kube-controller-manager* ou sur *kube-scheduler*, selon les autres journaux d’activité que vous activez. Vous pouvez ensuite utiliser des instructions *where* supplémentaires pour restreindre l’étendue de la requête aux événements qui vous intéressent.

Pour plus d’informations sur l’interrogation et le filtrage des données de journal d’activité, consultez l’article [Consulter ou analyser les données collectées avec la recherche dans les journaux d’activité de l’analytique des journaux d’activité][analyze-log-analytics].

## <a name="log-event-schema"></a>Schéma d’événement de journal

Pour faciliter votre analyse des données de journal, le tableau ci-après décrit en détail le schéma utilisé pour chaque événement :

| Nom du champ               | Description |
|--------------------------|-------------|
| *resourceId*             | Ressource Azure ayant généré le journal |
| *time*                   | Timestamp du chargement du journal |
| *category*               | Nom du conteneur/composant générant le journal |
| *operationName*          | Toujours *Microsoft.ContainerService/managedClusters/diagnosticLogs/Read* |
| *properties.log*         | Texte complet du journal provenant du composant |
| *properties.stream*      | *stderr* ou *stdout* |
| *properties.pod*         | Nom du pod d’où provient le journal |
| *properties.containerID* | ID du conteneur docker que provient de ce fichier journal |

## <a name="next-steps"></a>Étapes suivantes

Cet article vous a expliqué comment activer et consulter les journaux d’activité des composants principaux Kubernetes de votre cluster AKS. Si vous souhaitez approfondir la supervision et la résolution des problèmes, vous pouvez également [afficher les journaux d’activité Kubelet][kubelet-logs] et [activer l’accès au nœud SSH][aks-ssh].

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create

<!-- LINKS - internal -->
[cli-quickstart]: kubernetes-walkthrough.md
[portal-quickstart]: kubernetes-walkthrough-portal.md
[log-analytics-overview]: ../log-analytics/log-analytics-overview.md
[analyze-log-analytics]: ../azure-monitor/learn/tutorial-viewdata.md
[kubelet-logs]: kubelet-logs.md
[aks-ssh]: ssh.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
