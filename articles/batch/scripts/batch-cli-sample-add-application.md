---
title: Exemple de script Azure CLI - Ajout d’une application dans Batch | Microsoft Docs
description: Exemple de script Azure CLI - Ajout d’une application dans Batch
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: lahugh
ms.openlocfilehash: a2abcfeee6fe1a08e2b02ff87e75217daddcfad4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66127465"
---
# <a name="cli-example-add-an-application-to-an-azure-batch-account"></a>Exemple CLI : Ajouter une application à un compte Azure Batch

Ce script explique comment ajouter une application à utiliser avec une tâche ou un pool Azure Batch. Pour configurer une application à ajouter à votre compte Batch, placez votre fichier exécutable et toutes ses éventuelles dépendances dans un fichier .zip. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser l’interface de ligne de commande localement, vous devez exécuter Azure CLI version 2.0.20 ou une version ultérieure pour poursuivre la procédure décrite dans cet article. Exécutez `az --version` pour trouver la version. Si vous devez installer ou mettre à niveau, voir [Installer Azure CLI](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Exemple de script

[!code-azurecli-interactive[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement

Exécutez la commande suivante pour supprimer le groupe de ressources et toutes les ressources qui lui sont associées.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes.
Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Crée un compte de stockage. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Crée le compte Batch. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Effectue l’authentification par rapport au compte Batch spécifié pour renforcer les interactions avec la CLI.  |
| [az batch application create](/cli/azure/batch/application#az-batch-application-create) | Crée une application.  |
| [az batch application package create](/cli/azure/batch/application/package#az-batch-application-package-create) | Ajoute un package d’application à l’application spécifiée.  |
| [az batch application set](/cli/azure/batch/application#az-batch-application-set) | Met à jour les propriétés d’une application.  |
| [az group delete](/cli/azure/group#az-group-delete) | Supprime un groupe de ressources, y compris toutes les ressources imbriquées. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure).
