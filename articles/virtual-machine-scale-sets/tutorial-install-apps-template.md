---
title: Didacticiel -Installer des applications dans un groupe identique avec des modèles Azure | Microsoft Docs
description: Découvrez comment utiliser des modèles Azure Resource Manager pour installer des applications dans des groupes identiques de machines virtuelles avec l’extension de script personnalisé
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b388e0aaec29c5b9a01e0b0a306f5486f6215092
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-install-applications-in-virtual-machine-scale-sets-with-an-azure-template"></a>Didacticiel : Installer des applications dans des groupes identiques de machines virtuelles avec un modèle Azure
Pour exécuter des applications sur des instances de machine virtuelle d’un groupe identique, vous devez d’abord installer les composants d’application et les fichiers requis. Dans un didacticiel précédent, vous avez appris à créer et utiliser une image personnalisée de machine virtuelle pour déployer vos instances de machine virtuelle. Cette image personnalisée comprenait l’installation et la configuration manuelles d’applications. Vous pouvez également automatiser l’installation des applications pour un groupe identique après le déploiement de chaque instance de machine virtuelle, ou mettre à jour une application déjà exécutée dans un groupe identique. Ce didacticiel vous montre comment effectuer les opérations suivantes :

> [!div class="checklist"]
> * Installer automatiquement des applications dans votre groupe identique
> * Utilisez l’extension de script personnalisé Azure
> * Mettre à jour une application en cours d’exécution dans un groupe identique

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser l’interface CLI localement, vous devez exécuter Azure CLI version 2.0.29 ou une version ultérieure pour poursuivre la procédure décrite dans ce didacticiel. Exécutez `az --version` pour trouver la version. Si vous devez installer ou mettre à niveau, consultez [Installation d’Azure CLI 2.0]( /cli/azure/install-azure-cli).


## <a name="what-is-the-azure-custom-script-extension"></a>Qu’est-ce que l’extension de script personnalisé Azure ?
L’extension de script personnalisé télécharge et exécute des scripts sur des machines virtuelles Azure. Cette extension est utile pour la configuration post-déploiement, l’installation de logiciels ou toute autre tâche de configuration ou de gestion. Des scripts peuvent être téléchargés à partir de Stockage Azure ou de GitHub, ou fournis dans le portail Azure lors de l’exécution de l’extension.

L’extension de script personnalisé s’intègre aux modèles Azure Resource Manager et peut être utilisée à l’aide de l’interface CLI 2.0, de Azure PowerShell, du portail Azure ou de l’API REST. Pour plus d’informations, consultez [Vue d’ensemble de l’extension de script personnalisé](../virtual-machines/linux/extensions-customscript.md).

Pour voir l’extension de script personnalisé en action, créez un groupe identique qui installe le serveur web NGINX et qui affiche le nom d’hôte de l’instance de machine virtuelle du groupe identique. La définition suivante de l’extension de script personnalisé télécharge un exemple de script à partir de GitHub, installe les packages requis, puis écrit le nom d’hôte de l’instance de machine virtuelle sur une page HTML de base.


## <a name="create-custom-script-extension-definition"></a>Créer une définition de l’extension de script personnalisé
Lorsque vous définissez un groupe identique de machines virtuelles avec un modèle Azure, le fournisseur de ressources *Microsoft.Compute/virtualMachineScaleSets* peut inclure une section sur les extensions. Le *extensionsProfile* détaille ce qui est appliqué aux instances de machine virtuelle dans un groupe identique. Pour utiliser l’extension de script personnalisé, vous spécifiez un éditeur de *Microsoft.Azure.Extensions* et un type de *CustomScript*.

La propriété *fileUris* est utilisée pour définir les packages ou les scripts d’installation sources. Pour démarrer le processus d’installation, les scripts nécessaires sont définis dans *commandToExecute*. L’exemple suivant définit un exemple de script à partir de GitHub qui installe et configure le serveur web NGINX :

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "AppInstall",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx.sh"
          ],
          "commandToExecute": "bash automate_nginx.sh"
        }
      }
    }
  ]
}
```

Pour obtenir l’exemple complet d’un modèle Azure qui déploie un groupe identique et l’extension de script personnalisé, consultez [https://github.com/Azure-Samples/compute-automation-configurations/blob/master/scale_sets/azuredeploy.json](https://github.com/Azure-Samples/compute-automation-configurations/blob/master/scale_sets/azuredeploy.json). Ce modèle exemple est utilisé dans la section suivante.


## <a name="create-a-scale-set"></a>Créer un groupe identique
Nous allons utiliser le modèle exemple pour créer un groupe identique et appliquer l’extension de script personnalisé. Tout d’abord, créez un groupe de ressources avec la commande [az group create](/cli/azure/group#az_group_create). L’exemple suivant crée un groupe de ressources nommé *myResourceGroup* à l’emplacement *eastus* :

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Créez à présent un groupe identique de machines virtuelles avec [az group deployment create](/cli/azure/group/deployment#az_group_deployment_create). Lorsque vous y êtes invité, fournissez votre propre nom d’utilisateur et mot de passe utilisés comme informations d’identification pour chaque instance de machine virtuelle :

```azurecli-interactive
az group deployment create \
  --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/scale_sets/azuredeploy.json
```

La création et la configuration des l’ensemble des ressources et des machines virtuelles du groupe identique prennent quelques minutes.

Chaque instance de machine virtuelle dans le groupe identique télécharge et exécute le script à partir de GitHub. Dans un exemple plus complexe, les composants et fichiers de plusieurs applications peuvent être installés. Si le groupe identique est mis à l’échelle, les nouvelles instances de machine virtuelle appliquent automatiquement la même définition de l’extension de script personnalisé et installent l’application requise.


## <a name="test-your-scale-set"></a>Tester votre groupe identique
Pour voir votre serveur web en action, obtenez l’adresse IP publique de votre équilibreur de charge avec [az network public-ip show](/cli/azure/network/public-ip#show). L’exemple suivant obtient l’adresse IP pour *myScaleSetPublicIP* qui a été créée dans le cadre du groupe identique :

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myScaleSetPublicIP \
  --query [ipAddress] \
  --output tsv
```

Saisissez l’adresse IP publique de l’équilibreur de charge dans un navigateur web. L’équilibreur de charge répartit le trafic vers l’une de vos instances de machine virtuelle, comme illustré dans l’exemple suivant :

![Page web de base dans NGINX](media/tutorial-install-apps-template/running-nginx.png)

Laissez le navigateur web ouvert afin que vous puissiez voir une version mise à jour à l’étape suivante.


## <a name="update-app-deployment"></a>Déploiement d’une mise à jour d’application
Tout au long du cycle de vie d’un groupe identique, vous devrez peut-être déployer une version mise à jour de votre application. Avec l’extension de script personnalisé, vous pouvez faire référence à un script de déploiement de mises à jour puis réappliquer l’extension pour votre groupe identique. Une fois le groupe identique créé à l’étape précédente, *upgradePolicy' a été défini sur *Automatique*. Ce paramètre permet aux instances de machine virtuelle dans le groupe identique de mettre automatiquement à jour et d’appliquer la dernière version de votre application.

Pour mettre à jour la définition de l’extension de script personnalisé, modifiez votre modèle pour faire référence à un nouveau script d’installation. Un nouveau nom de fichier doit être utilisé pour que l’extension de script personnalisé puisse reconnaître la modification. L’extension de script personnalisé n’examine pas le contenu du script pour déterminer les modifications. La définition suivante utilise un script d’installation mis à jour avec la mention *_v2* ajoutée à son nom :

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "AppInstall",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate_nginx_v2.sh"
          ],
          "commandToExecute": "bash automate_nginx_v2.sh"
        }
      }
    }
  ]
}
```

Appliquez la configuration de l’extension de script personnalisé aux instances de machine virtuelle dans votre groupe identique de nouveau avec [az group deployment create](/cli/azure/group/deployment#az_group_deployment_create). Ce modèle *azuredeployv2.json* est utilisé pour appliquer la version mise à jour de l’application. Dans la pratique, vous modifiez le modèle *azuredeploy.json* existant pour faire référence au script d’installation mis à jour, comme indiqué dans la section précédente. Lorsque vous y êtes invité, entrez le nom d’utilisateur et mot de passe utilisés lors de la création du groupe identique :

```azurecli-interactive
az group deployment create \
  --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/scale_sets/azuredeploy_v2.json
```

Toutes les instances de machine virtuelle dans le groupe identique sont automatiquement mises à jour avec la dernière version de la page web exemple. Pour afficher la version mise à jour, actualisez le site web dans votre navigateur :

![Page web mise à jour dans Nginx](media/tutorial-install-apps-template/running-nginx-updated.png)


## <a name="clean-up-resources"></a>Supprimer des ressources
Pour supprimer votre groupe identique et les ressources supplémentaires, supprimez le groupe de ressources et toutes ses ressources avec [az group delete](/cli/azure/group#az_group_delete). Le paramètre `--no-wait` retourne le contrôle à l’invite de commandes sans attendre que l’opération se termine. Le paramètre `--yes` confirme que vous souhaitez supprimer les ressources sans passer par une invite supplémentaire à cette fin.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez appris comment installer et mettre à jour automatiquement des applications dans votre groupe identique avec des modèles Azure :

> [!div class="checklist"]
> * Installer automatiquement des applications dans votre groupe identique
> * Utilisez l’extension de script personnalisé Azure
> * Mettre à jour une application en cours d’exécution dans un groupe identique

Passez au didacticiel suivant pour apprendre à mettre automatiquement à l’échelle votre groupe identique.

> [!div class="nextstepaction"]
> [Mettre automatiquement à l’échelle vos groupes identiques](tutorial-autoscale-template.md)