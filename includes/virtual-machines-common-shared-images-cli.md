---
title: Fichier Include
description: Fichier Include
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 841027fe8d6b97e661faa038dc9381edbb3d4cd8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226024"
---
## <a name="before-you-begin"></a>Avant de commencer

Pour suivre l’exemple de cet article, vous devez avoir l’image managée d’une machine virtuelle généralisée. Pour plus d’informations, consultez [Tutoriel : Créer une image personnalisée d’une machine virtuelle Azure avec Azure CLI 2.0](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Si l’image managée contient un disque de données, la taille de disque de données ne peut pas être supérieur à 1 To.

## <a name="launch-azure-cloud-shell"></a>Lancement d’Azure Cloud Shell

Azure Cloud Shell est un interpréteur de commandes interactif et gratuit que vous pouvez utiliser pour exécuter les étapes de cet article. Il contient des outils Azure courants préinstallés et configurés pour être utilisés avec votre compte. 

Pour ouvrir Cloud Shell, sélectionnez simplement **Essayer** en haut à droite d’un bloc de code. Vous pouvez également lancer Cloud Shell dans un onglet distinct du navigateur en accédant à [https://shell.azure.com/bash](https://shell.azure.com/bash). Sélectionnez **Copier** pour copier les blocs de code, collez-les dans Cloud Shell, puis appuyez sur Entrée pour les exécuter.

Si vous préférez installer et utiliser l’interface CLI localement, consultez [installer Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-an-image-gallery"></a>Créer une galerie d’images 

Une galerie d’images est la ressource principale utilisée pour activer le partage d’image. Les caractères autorisés pour le nom de galerie sont les lettres majuscules ou minuscules, les chiffres et les points. Le nom de la galerie ne peut pas contenir de tirets.   Les noms de galerie doivent être uniques dans votre abonnement. 

Créez une galerie d’images à l’aide de [az sig create](/cli/azure/sig#az-sig-create). L’exemple suivant crée une galerie nommée *myGallery* dans *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Créer une définition d’image

Définitions d’image créer un regroupement logique des images. Ils sont utilisés pour gérer les informations sur les versions de l’image qui sont créés au sein de celles-ci. Noms de définition d’image peuvent être constitués de périodes, des chiffres, des points, des tirets et des lettres majuscules ou minuscules. Pour plus d’informations sur les valeurs que vous pouvez spécifier pour une définition de l’image, consultez [Image définitions](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

Créer une définition d’image initiale dans la galerie avec la commande [az sig image-definition create](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```


## <a name="create-an-image-version"></a>Créer une version d’image 

Créez des versions d’image en fonction de vos besoins avec la commande [az image gallery create-image-version](/cli/azure/sig/image-version#az-sig-image-version-create). L’ID de l’image managée sert de base de référence pour la création de la version d’image. Vous pouvez utiliser [az image list](/cli/azure/image?view#az-image-list) pour obtenir des informations sur les images qui se trouvent dans un groupe de ressources. 

Les caractères autorisés pour la version d’image sont les nombres et les points. Les nombres doivent être un entier 32 bits. Format : *MajorVersion*.*MinorVersion*.*Patch*.

Dans cet exemple, la version de notre image est *1.0.0* et nous allons créer 2 réplicas dans le *ouest des États-Unis* la région, 1 réplica dans le *South Central US* région et 1 réplica dans le *est des États-Unis 2* région à l’aide de stockage redondant.


```azurecli-interactive 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1=Standard_ZRS" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> Vous devez attendre que la version de l’image se termine complètement les cours de génération et de réplication avant de pouvoir utiliser la même image managée pour créer une autre version de l’image.
>
> Vous pouvez également stocker tous vos réplicas de version d’image dans [stockage redondant](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) en ajoutant `--storage-account-type standard_zrs` lorsque vous créez la version de l’image.
>

## <a name="share-the-gallery"></a>Partager la galerie

Nous recommandons que vous partagez avec d’autres utilisateurs au niveau de la galerie. Pour obtenir l’ID d’objet de votre galerie, utilisez [show de sig az](/cli/azure/sig#az-sig-show).

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

Utiliser l’ID d’objet en tant qu’étendue, ainsi qu’une adresse de messagerie et [créer d’attribution de rôle az](/cli/azure/role/assignment#az-role-assignment-create) à donner un accès utilisateur à la galerie d’images partagé.

```azurecli-interactive
az role assignment create --role "Reader" --assignee <email address> --scope <gallery ID>
```


