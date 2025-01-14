---
title: Créer une machine virtuelle Windows avec l’applet de commande New-AzVM simplifiée dans Azure Cloud Shell | Microsoft Docs
description: Apprenez rapidement à créer des machines virtuelles Windows avec l’applet de commande New-AzVM simplifiée dans Azure Cloud Shell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: cdf7385b3686c5c3f91e66c67ab5f0758935b39f
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730139"
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azvm-cmdlet-in-cloud-shell"></a>Créer une machine virtuelle Windows avec l’applet de commande New-AzVM simplifiée dans Cloud Shell 

L’applet de commande [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) a ajouté un jeu de paramètres simplifié pour la création d’une nouvelle machine virtuelle à l’aide de PowerShell. Cette rubrique explique comment utiliser PowerShell dans Azure Cloud Shell, avec la dernière version de l’applet de commande New-AzureVM préinstallée, pour créer une machine virtuelle. Nous allons utiliser un jeu de paramètres simplifié qui crée automatiquement toutes les ressources nécessaires à l’aide de valeurs par défaut pertinentes. 

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]


## <a name="create-the-vm"></a>Création de la machine virtuelle

Vous pouvez utiliser l’applet de commande [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) pour créer une machine virtuelle avec les valeurs par défaut, ce qui implique l’utilisation de l’image Windows Server 2016 Datacenter de la Place de marché Azure. Vous pouvez utiliser New-AzVM avec le paramètre **-Name** uniquement, et l’applet de commande utilisera cette valeur pour tous les noms de ressources. Dans cet exemple, nous définissons le paramètre **-Name** en tant que *myVM*. 

Vérifiez que **PowerShell** est sélectionné dans Cloud Shell et tapez :

```azurepowershell-interactive
New-AzVm -Name myVM
```

Vous êtes invité à créer un nom d’utilisateur et un mot de passe pour la machine virtuelle, qui seront utilisés quand vous vous connecterez à la machine virtuelle plus loin dans cette rubrique. Le mot de passe doit compter 12 à 123 caractères et comprendre trois des quatre caractères suivants : une minuscule, une majuscule, un chiffre et un caractère spécial.

La création de la machine virtuelle et des ressources associées prend une minute. Quand vous avez terminé, vous pouvez voir toutes les ressources qui ont été créées avec l’applet de commande [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource).

```azurepowershell-interactive
Get-AzResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Connexion à la machine virtuelle

Une fois le déploiement terminé, créez une connexion Bureau à distance avec la machine virtuelle.

Utilisez la commande [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) pour renvoyer l’adresse IP publique de la machine virtuelle. Notez cette adresse IP.

```azurepowershell-interactive
Get-AzPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Sur votre ordinateur local, ouvrez une invite de commandes et utilisez la commande **mstsc** pour démarrer une session Bureau à distance avec votre nouvelle machine virtuelle. Remplacez &lt;publicIPAddress&gt; par l’adresse IP de votre machine virtuelle. Lorsque vous y êtes invité, entrez le nom d’utilisateur et le mot de passe que vous aviez fournis pour votre machine virtuelle lors de sa création.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Spécifier des noms de ressources différents

Vous pouvez également fournir des noms de ressources plus descriptifs et continuer à les créer automatiquement. Dans l’exemple suivant, nous avons nommé plusieurs ressources pour la nouvelle machine virtuelle, y compris un nouveau groupe de ressources.

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Supprimer des ressources

Quand vous n’en avez plus besoin, vous pouvez utiliser la commande [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) pour supprimer le groupe de ressources, la machine virtuelle et toutes les ressources associées.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myVM
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Étapes suivantes

Dans cette rubrique, vous avez déployé une machine virtuelle simple avec New-AzVM et vous l’avez connectée via RDP. Pour en savoir plus sur les machines virtuelles Azure, suivez le didacticiel pour les machines virtuelles Windows.

> [!div class="nextstepaction"]
> [Didacticiels sur les machines virtuelles Azure Windows](./tutorial-manage-vm.md)
