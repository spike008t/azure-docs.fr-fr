---
title: Utiliser une identité managée affectée par le système de machine virtuelle Windows pour accéder au Stockage Azure à l’aide d’informations d’identification SAP
description: Tutoriel montrant comment utiliser une identité managée affectée par le système de machine virtuelle Windows pour accéder au Stockage Azure en utilisant des informations d’identification SAP au lieu d’une clé d’accès au compte de stockage.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23ec4d2a67beb9b5f903aa0b7f03196b47db3f78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66226465"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-storage-via-a-sas-credential"></a>Tutoriel : Utiliser une identité managée affectée par le système de machine virtuelle Windows pour accéder au Stockage Azure avec des informations d’identification SAP

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Ce tutoriel vous indique comment utiliser une identité affectée par le système pour une machine virtuelle Windows afin d’obtenir des informations d’identification SAP (signature d’accès partagé) de stockage. Plus précisément, des [informations d’identification SAP de service](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Une SAP de service offre la possibilité d’accorder un accès limité à des objets dans un compte de stockage, pendant une durée limitée et pour un service spécifique (en l’occurrence, le service BLOB), sans exposer de clé d’accès de compte. Vous pouvez utiliser des informations d’identification SAP comme d’habitude lors de l’exécution d’opérations de stockage, par exemple, lors de l’utilisation du SDK Stockage Azure. Ce didacticiel montre le chargement et le téléchargement d’un objet blob à l’aide de Stockage Azure PowerShell. Vous apprendrez à :

> [!div class="checklist"]
> * Créez un compte de stockage.
> * Autoriser votre machine virtuelle à accéder à la SAP d’un compte stockage dans le Gestionnaire des ressources 
> * Obtenir un jeton d’accès à l’aide de l’identité de votre machine virtuelle et l’utiliser pour récupérer la SAP à partir du Gestionnaire des ressources 

## <a name="prerequisites"></a>Conditions préalables

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-a-storage-account"></a>Créez un compte de stockage. 

Si vous n’en avez pas déjà un, vous allez maintenant créer un compte de stockage. Vous pouvez également ignorer cette étape, et autoriser votre identité managée affectée par le système de la machine virtuelle à accéder aux informations d’identification SAP d’un compte de stockage existant. 

1. Cliquez sur le bouton **+/Créer un service** dans l’angle supérieur gauche du portail Azure.
2. Cliquez sur **Stockage**, puis **Compte de stockage**, et un nouveau panneau « Créer un compte de stockage » s’affiche.
3. Saisissez un nom pour le compte de stockage, vous l’utiliserez ultérieurement.  
4. **Modèle de déploiement** et **Type de compte** doivent être respectivement définis sur « Gestionnaire de ressources » et « Usage général ». 
5. Assurez-vous que les champs **Abonnement** et **Groupe de ressources** correspondent à ceux que vous avez spécifiés lorsque vous avez créé votre machine virtuelle à l’étape précédente.
6. Cliquez sur **Créer**.

    ![Créer un nouveau compte de stockage](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Création d’un conteneur d’objets blob dans le compte de stockage

Plus tard, nous chargerons et téléchargerons un fichier vers le nouveau compte de stockage. Étant donné que les fichiers nécessitent un stockage d’objets blob, nous devons créer un conteneur d’objets blob dans lequel stocker le fichier.

1. Revenez à votre compte de stockage nouvellement créé.
2. Cliquez sur le lien **Conteneurs** dans le panneau de gauche, sous « Service BLOB ».
3. Cliquez sur **+ Conteneur** en haut de la page et un panneau « Nouveau conteneur » apparait.
4. Nommez le conteneur, sélectionnez un niveau d’accès, puis cliquez sur **OK**. Le nom spécifié sera utilisé plus loin dans le didacticiel. 

    ![Créer un conteneur de stockage](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-system-assigned-managed-identity-access-to-use-a-storage-sas"></a>Autoriser votre identité managée affectée par le système de machine virtuelle à utiliser une SAP de stockage 

Le stockage Azure ne prend pas en charge l’authentification Azure AD en mode natif.  Toutefois, vous pouvez utiliser une identité managée pour récupérer une SAP de stockage à partir du Gestionnaire des ressources, puis utiliser cette SAP pour accéder au stockage.  Dans cette étape, vous autorisez votre identité managée affectée par le système de machine virtuelle à accéder à la SAP de votre compte de stockage.   

1. Revenez à votre compte de stockage nouvellement créé.   
2. Cliquez sur le lien **(IAM) de contrôle d’accès** dans le panneau de gauche.  
3. Cliquez sur **+ Ajouter une attribution de rôle** en haut de la page pour ajouter une nouvelle attribution de rôle à votre machine virtuelle.
4. Définissez **Rôle** sur « Contributeur de comptes de stockage » sur le côté droit de la page.  
5. Dans la liste déroulante suivante, définissez **Attribuer l’accès à** sur la ressource « Machine virtuelle ».  
6. Ensuite, assurez-vous que l’abonnement approprié est répertorié dans la liste déroulante **Abonnement**, puis définissez **Groupe de ressources** sur « Tous les groupes de ressources ».  
7. Enfin, sous **Sélectionner**, choisissez votre machine virtuelle Windows dans la liste déroulante, puis cliquez sur **Enregistrer**. 

    ![Texte de remplacement d’image](./media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Obtenir un jeton d’accès à l’aide de l’identité de la machine virtuelle et l’utiliser pour appeler Azure Resource Manager 

Pour la suite de ce didacticiel, nous allons utiliser la machine virtuelle que nous avons créée précédemment.

Dans cette partie, vous devez utiliser les applets de commande PowerShell d’Azure Resource Manager.  Si vous ne l’avez pas installé, [téléchargez la dernière version](https://docs.microsoft.com/powershell/azure/overview) avant de continuer.

1. Dans le portail Azure, accédez à **Machines virtuelles**, accédez à votre machine virtuelle Windows, puis, en haut de la page **Vue d’ensemble**, cliquez sur **Se connecter**.
2. Entrez le **Nom d’utilisateur** et le **Mot de passe** que vous avez ajoutés lorsque vous avez créé la machine virtuelle Windows. 
3. Maintenant que vous avez créé une **Connexion au Bureau à distance** avec la machine virtuelle, ouvrez PowerShell dans la session à distance. 
4. À l’aide d’Invoke-WebRequest de Powershell, envoyez une requête à l’identité managée locale pour le point de terminaison de ressources Azure afin d’obtenir un jeton d’accès pour Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > La valeur du paramètre « ressource » doit correspondre exactement à ce qui est attendu par Azure AD. Lorsque vous utilisez l’ID de ressource Azure Resource Manager, vous devez inclure la barre oblique de fin à l’URI.
    
    Ensuite, extrayez l’élément « Content » qui est stocké sous forme de chaîne au format JSON (JavaScript Objet Notation) dans l’objet $response. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Ensuite, extrayez le jeton d’accès de la réponse.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Obtenir des informations d’identification SAP d’Azure Resource Manager pour effectuer des appels de stockage 

Maintenant, utilisez PowerShell pour appeler le Gestionnaire des ressources à l’aide du jeton d’accès récupéré dans la section précédente, afin de créer des informations d’identification SAP. Une fois en possession des informations d’identification SAP, nous pouvons appeler des opérations de stockage.

Pour cette demande, nous allons utiliser les paramètres de requête HTTP suivants pour créer les informations d’identification SAP :

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Ces paramètres sont inclus dans le corps POST de la requête pour les informations d’identification SAP. Pour plus d’informations concernant les paramètres pour la création d’informations d’identification SAP, voir [Afficher la référence REST de SAP de service](/rest/api/storagerp/storageaccounts/listservicesas).

Tout d’abord, convertissez les paramètres au format JSON, puis appelez le point de terminaison `listServiceSas` du stockage pour créer les informations d’identification SAP :

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> L’URL respecte la casse, par conséquent, assurez-vous que la casse est identique à celle utilisée précédemment lorsque vous avez nommé le groupe de ressources, et que la majuscule « G » est correcte dans « resourceGroup. » 

Nous pouvons à présent extraire les informations d’identification SAP de la réponse :

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

Si vous inspectez les informations d’identification SAP, vous devez voir quelque chose comme ceci :

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Nous créons ensuite un fichier appelé « test.txt ». Ensuite, utilisez les informations d’identification SAP pour vous authentifier à l’aide de l’applet de commande `New-AzStorageContent`, charger le fichier dans notre conteneur d’objets blob, puis télécharger le fichier.

```bash
echo "This is a test text file." > test.txt
```

Veillez à installer au préalable les applets de commande de Stockage Azure à l’aide de la commande `Install-Module Azure.Storage`. Vous pouvez charger l’objet blob que vous venez de créer à l’aide de l’applet de commande PowerShell `Set-AzStorageBlobContent`:

```powershell
$ctx = New-AzStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Réponse :

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Vous pouvez également télécharger l’objet blob que vous venez de charger, à l’aide de l’applet de commande PowerShell `Get-AzStorageBlobContent` :

```powershell
Get-AzStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Réponse :

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez appris à utiliser une identité managée affectée par le système de machine virtuelle Windows pour accéder au Stockage Azure à l’aide d’informations d’identification SAP.  Pour en savoir plus sur le Stockage Azure SAP, consultez :

> [!div class="nextstepaction"]
>[Utilisation des signatures d’accès partagé (SAP)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)


