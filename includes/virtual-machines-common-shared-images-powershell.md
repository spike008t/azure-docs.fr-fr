---
title: Fichier Include
description: Fichier Include
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: bae66078a1bcb1d80f0798b1d501598fa785fb80
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241196"
---
## <a name="launch-azure-cloud-shell"></a>Lancement d’Azure Cloud Shell

Azure Cloud Shell est un interpréteur de commandes interactif et gratuit que vous pouvez utiliser pour exécuter les étapes de cet article. Il contient des outils Azure courants préinstallés et configurés pour être utilisés avec votre compte. 

Pour ouvrir Cloud Shell, sélectionnez simplement **Essayer** en haut à droite d’un bloc de code. Vous pouvez également lancer Cloud Shell dans un onglet distinct du navigateur en accédant à [https://shell.azure.com/powershell](https://shell.azure.com/powershell). Sélectionnez **Copier** pour copier les blocs de code, collez-les dans Cloud Shell, puis appuyez sur Entrée pour les exécuter.


## <a name="get-the-managed-image"></a>Obtenir l'image managée

Pour afficher une liste d’images disponibles dans un groupe de ressources, utilisez [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Une fois le nom de l'image et son groupe de ressources connus, vous pouvez réutiliser `Get-AzImage` pour obtenir l’objet image et le stocker dans une variable à des fins d'utilisation ultérieure. Cet exemple permet d'obtenir une image nommée *myImage* à partir du groupe de ressources « myResourceGroup » et l’assigne à la variable *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Créer une galerie d’images 

Une galerie d’images est la ressource principale utilisée pour activer le partage d’image. Les caractères autorisés pour le nom de galerie sont les lettres majuscules ou minuscules, les chiffres et les points. Le nom de la galerie ne peut pas contenir de tirets. Les noms de galerie doivent être uniques dans votre abonnement. 

Créez une galerie d’images à l’aide de [New-AzureRmGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). L’exemple suivant crée une galerie nommée *myGallery* dans le groupe de ressources *myGalleryRG*.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>Créer une définition d’image 

Définitions d’image créer un regroupement logique des images. Ils sont utilisés pour gérer les informations sur les versions de l’image qui sont créés au sein de celles-ci. Noms de définition d’image peuvent être constitués de périodes, des chiffres, des points, des tirets et des lettres majuscules ou minuscules. Pour plus d’informations sur les valeurs que vous pouvez spécifier pour une définition de l’image, consultez [Image définitions](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Créer la définition d’image à l’aide [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). Dans cet exemple, l’image de galerie est nommée *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```


## <a name="create-an-image-version"></a>Créer une version d’image

Créer une version de l’image à partir d’une image managée à l’aide [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). 

Les caractères autorisés pour la version d’image sont les nombres et les points. Les nombres doivent être un entier 32 bits. Format : *MajorVersion*.*MinorVersion*.*Patch*.

Dans cet exemple, la version d'image, *1.0.0*, elle est répliquée dans les deux centres de données *USA Centre-Ouest* et *USA Centre Sud*. Lors du choix des régions cibles pour la réplication, n’oubliez pas que vous devez également inclure le *source* région en tant que cible pour la réplication.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

La réplication de l'image dans toutes les régions cibles peut nécessiter un certain temps et nous avons donc créé une tâche pour en suivre la progression. Pour afficher la progression de la tâche, entrez `$job.State`.

```azurepowershell-interactive
$job.State
```

> [!NOTE]
> Vous devez attendre que la version de l’image se termine complètement les cours de génération et de réplication avant de pouvoir utiliser la même image managée pour créer une autre version de l’image. 
>
> Vous pouvez également stocker votre version de l’image dans [stockage redondant](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) en ajoutant `-StorageAccountType Standard_ZRS` lorsque vous créez la version de l’image.
>


## <a name="share-the-gallery"></a>Partager la galerie

Nous recommandons que vous partagez l’accès au niveau de la galerie d’image. Utiliser une adresse e-mail et le [Get-AzADUser](/powershell/module/az.resources/get-azaduser) applet de commande pour obtenir l’ID d’objet pour l’utilisateur, puis utilisez [New-AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment) pour leur donner accès à la galerie. Remplacez l’exemple d’e-mail, alinne_montes@contoso.com dans cet exemple, avec vos propres informations.

```azurepowershell-interactive
# Get the object ID for the user
$user = Get-AzADUser -StartsWith alinne_montes@contoso.com
# Grant access to the user for our gallery
New-AzRoleAssignment `
   -ObjectId $user.Id `
   -RoleDefinitionName Reader `
   -ResourceName $gallery.Name `
   -ResourceType Microsoft.Compute/galleries `
   -ResourceGroupName $resourceGroup.ResourceGroupName
```