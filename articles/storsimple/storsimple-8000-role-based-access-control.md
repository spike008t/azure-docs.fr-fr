---
title: Utiliser le contrôle d’accès en fonction du rôle (RBAC) pour StorSimple | Microsoft Docs
description: Décrit comment utiliser le contrôle d’accès en fonction du rôle (RBAC) Azure dans le contexte de StorSimple.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: a79753a897a62e194a759c23a9c0acc45c5f36c1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159099"
---
# <a name="role-based-access-control-for-storsimple"></a>Contrôle d’accès en fonction du rôle pour StorSimple

Cet article fournit une brève description de l’utilisation du contrôle d’accès en fonction du rôle pour votre appareil StorSimple. RBAC offre une gestion fine des accès pour Azure. Utilisez RBAC pour accorder seulement les accès strictement nécessaires aux utilisateurs StorSimple pour effectuer leurs tâches, au lieu de donner à tout le monde un accès illimité. Pour plus d’informations sur les concepts de base de la gestion des accès dans Azure, consultez [Bien démarrer avec le contrôle d’accès en fonction du rôle dans le portail Azure](../role-based-access-control/overview.md).

Cet article s’applique aux appareils de la série StorSimple 8000 exécutant Update 3.0 ou ultérieur dans le portail Azure.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-storsimple"></a>Rôles RBAC pour StorSimple

RBAC peut être affecté en fonction des rôles. Les rôles garantissent certains niveaux d’autorisation en fonction des ressources disponibles dans l’environnement. Les utilisateurs StorSimple peuvent choisir entre deux types de rôles : intégrés ou personnalisés.

* **Rôles intégrés** : les rôles intégrés sont propriétaire, contributeur, lecteur ou administrateur des accès utilisateurs. Pour plus d’informations, consultez [Rôles intégrés pour le contrôle d’accès en fonction du rôle Azure](../role-based-access-control/built-in-roles.md).

* **Rôles personnalisés** : si les rôles intégrés ne répondent pas à vos besoins, vous pouvez créer des rôles RBAC personnalisés pour StorSimple. Pour créer un rôle RBAC personnalisé, commencez avec un rôle intégré, modifiez-le, puis réimportez-le dans l’environnement. Le téléchargement et le chargement du rôle sont gérés avec PowerShell ou Azure CLI. Pour plus d’informations, consultez [Créer des rôles personnalisés pour le contrôle d’accès en fonction du rôle](../role-based-access-control/custom-roles.md).

Pour voir les différents rôles disponibles pour un utilisateur d’un appareil StorSimple dans le portail Azure, accédez à votre service StorSimple Device Manager, puis à **Contrôle d’accès (IAM) > Rôles**.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>Créer un rôle personnalisé pour l’administrateur d’infrastructure StorSimple

Dans l’exemple suivant, nous commençons avec le rôle intégré **Lecteur**, qui permet aux utilisateurs de voir toutes les étendues de ressources, mais pas de les modifier ou d’en créer de nouvelles. Nous étendons ensuite ce rôle pour créer un rôle d’administrateur d’infrastructure StorSimple personnalisé. Ce rôle est attribué aux utilisateurs qui peuvent gérer l’infrastructure pour les appareils StorSimple.

1. Exécutez Windows PowerShell en tant qu’administrateur.

2. Connectez-vous à Azure.

    `Connect-AzAccount`

3. Exportez le rôle Lecteur sous la forme d’un modèle JSON sur votre ordinateur.

    ```powershell
    Get-AzRoleDefinition -Name "Reader"

    Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. Ouvrez le fichier JSON dans Visual Studio. Vous voyez qu’un rôle RBAC classique comprend trois sections principales : **Actions**, **NotActions** et **AssignableScopes**.

    Dans la section **Action**, toutes les opérations autorisées pour ce rôle sont répertoriées. Chaque action est affectée à partir d’un fournisseur de ressources. Pour un administrateur d’infrastructure StorSimple, utilisez le fournisseur de ressources `Microsoft.StorSimple`.

    Utilisez PowerShell pour voir tous les fournisseurs de ressources disponibles et inscrits dans votre abonnement.

    `Get-AzResourceProvider`

    Vous pouvez aussi utiliser des applets de commande PowerShell pour gérer les fournisseurs de ressources.

    Dans la section **NotActions**, toutes les actions restreintes pour un rôle RBAC particulier sont répertoriées. Dans cet exemple, aucune action n’est restreinte.
    
    Dans la section **AssignableScopes**, les ID d’abonnement sont répertoriés. Vérifiez que le rôle RBAC contient l’ID d’abonnement explicite où il est utilisé. Si l’ID de l’abonnement approprié n’est pas spécifié, vous n’êtes pas autorisé à importer le rôle dans votre abonnement.

    Prenez en compte ces considérations quand vous modifiez le fichier.

    ```json
    {
        "Name":  "StorSimple Infrastructure Admin",
        "Id":  "<guid>",
        "IsCustom":  true,
        "Description":  "Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install, download etc.",
        "Actions":  [
                        "Microsoft.StorSimple/managers/alerts/read",
                        "Microsoft.StorSimple/managers/devices/volumeContainers/read",
                        "Microsoft.StorSimple/managers/devices/jobs/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/write",
                        "Microsoft.StorSimple/managers/clearAlerts/action",
                        "Microsoft.StorSimple/managers/devices/networkSettings/read",
                        "Microsoft.StorSimple/managers/devices/publishSupportPackage/action",
                        "Microsoft.StorSimple/managers/devices/scanForUpdates/action",
                        "Microsoft.StorSimple/managers/devices/metrics/read"

                    ],
        "NotActions":  [

                    ],
        "AssignableScopes":  [
                                "/subscriptions/<subscription_ID>/"
                            ]
    }
    ```

6. Réimportez le rôle RBAC personnalisé dans l’environnement.

    `New-AzRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Ce rôle doit maintenant apparaître dans la liste des rôles dans le panneau **Contrôle d’accès**.

![Afficher les rôles RBAC](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Pour plus d’informations, accédez à [Rôles personnalisés](../role-based-access-control/custom-roles.md).

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>Exemple de sortie pour la création d’un rôle personnalisé via PowerShell

```powershell
Connect-AzAccount
```

```Output
Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :
```

```powershell
Get-AzRoleDefinition -Name "Reader"
```

```Output
Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}
```

```powershell
Get-AzRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
New-AzRoleDefinition -InputFile "C:\ssrbaccustom.json"
```

```Output
Name             : StorSimple Infrastructure Admin
Id               : <tenant_ID>
IsCustom         : True
Description      : Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install,
                   download etc.
Actions          : {Microsoft.StorSimple/managers/alerts/read,
                   Microsoft.StorSimple/managers/devices/volumeContainers/read,
                   Microsoft.StorSimple/managers/devices/jobs/read,
                   Microsoft.StorSimple/managers/devices/alertSettings/read...}
NotActions       : {}
AssignableScopes : {/subscriptions/<subscription_ID>/}
```

## <a name="add-users-to-the-custom-role"></a>Ajouter des utilisateurs au rôle personnalisé

Vous accordez l’accès à partir de la ressource, du groupe de ressources ou de l’abonnement qui correspond à l’étendue de l’attribution du rôle. Quand vous accordez un accès, gardez à l’esprit que l’enfant hérite de l’accès accordé au niveau du nœud parent. Pour plus d’informations, accédez à [Contrôle d’accès en fonction du rôle](../role-based-access-control/overview.md).

1. Accédez à **Contrôle d’accès (IAM)**. Cliquez sur **+ Ajouter** dans le panneau Contrôle d’accès.

    ![Ajouter un accès au rôle RBAC](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Sélectionnez le rôle que vous souhaitez attribuer : dans ce cas, il s’agit de **Administrateur d’infrastructure StorSimple**.

3. Sélectionnez l’utilisateur, le groupe ou l’application de votre répertoire auquel vous voulez accorder l’accès. Dans le répertoire, vous pouvez rechercher des noms d’affichage, des adresses de messagerie et des identificateurs d’objet.

4. Sélectionnez **Enregistrer** pour créer l’attribution.

    ![Ajouter des autorisations au rôle RBAC](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Une notification **Ajout d’utilisateur** suit la progression. Une fois que l’utilisateur a été ajouté, la liste des utilisateurs dans Contrôle d’accès est mise à jour.

## <a name="view-permissions-for-the-custom-role"></a>Afficher les autorisations pour le rôle personnalisé

Une fois ce rôle créé, vous pouvez afficher les autorisations associées à ce rôle dans le portail Azure.

1. Pour afficher les autorisations associées à ce rôle, accédez à **Contrôle d’accès (IAM) > Rôles > Administrateur d’infrastructure StorSimple**. La liste des utilisateurs de ce rôle s’affiche.

2. Sélectionnez un utilisateur Administrateur d’infrastructure StorSimple et cliquez sur **Autorisations**.

    ![Afficher les autorisations pour le rôle Administrateur d’infrastructure StorSimple](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. Les autorisations associées à ce rôle sont affichées.

    ![Afficher les utilisateurs pour le rôle Administrateur d’infrastructure StorSimple](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [attribuer des rôles personnalisés à des utilisateurs internes et externes](../role-based-access-control/role-assignments-external-users.md).
