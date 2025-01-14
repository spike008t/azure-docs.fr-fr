---
title: Configurer les paramètres des groupes avec PowerShell - Azure Active Directory | Microsoft Docs
description: Comment gérer les paramètres des groupes à l’aide des applets de commande Azure Active Directory ?
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/26/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c9b07e7524488d0336a55af6e1d5f36af59a870
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729820"
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Configuration des paramètres de groupe avec les applets de commande Azure Active Directory
Cet article contient des instructions concernant l’utilisation des applets de commande PowerShell Azure Active Directory (Azure AD) pour créer et mettre à jour des groupes. Ce contenu s’applique uniquement aux groupes Office 365 (parfois appelés groupes unifiés). 

> [!IMPORTANT]
> Certains paramètres nécessitent une licence Azure Active Directory Premium P1. Pour plus d’informations, consultez le tableau [Paramètres de modèle](#template-settings).

Pour plus d’informations sur la façon d’empêcher les utilisateurs non-administrateurs de créer des groupes de sécurité, définissez  `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` tel que décrit dans [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Les paramètres des groupes Office 365 sont configurés à l’aide d’un objet Settings et d’un objet SettingsTemplate. Au départ, vous ne voyez aucun objet Paramètres dans votre répertoire, car votre répertoire est configuré avec les paramètres par défaut. Pour changer les paramètres par défaut, vous devez créer un objet de paramètres en utilisant un modèle de paramètres. Les modèles de paramètres sont définis par Microsoft. Il existe différents modèles de paramètres. Pour configurer les paramètres du groupe Office 365 pour votre répertoire, utiliserez le modèle nommé « Group.Unified ». Pour configurer les paramètres du groupe Office 365 sur un seul groupe, utilisez le modèle nommé « Group.Unified.Guest ». Ce modèle est utilisé pour gérer l’accès invité à un groupe Office 365. 

Les applets de commande font partie du module Azure Active Directory PowerShell V2. Pour obtenir des instructions sur le téléchargement et l’installation du module sur votre ordinateur, reportez-vous à l’article [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/azuread/). Vous pouvez installer la version 2 du module depuis [la galerie PowerShell](https://www.powershellgallery.com/packages/AzureAD/).



## <a name="create-settings-at-the-directory-level"></a>Créer des paramètres au niveau du répertoire
Les étapes ci-après permettent de créer des paramètres au niveau du répertoire qui s’appliquent à tous les groupes Office 365 du répertoire. L’applet de commande Get-AzureADDirectorySettingTemplate est disponible uniquement dans le [module de la préversion d’Azure AD PowerShell pour Graph](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

1. Dans les applets de commande DirectorySettings, vous devez spécifier l’ID du modèle SettingsTemplate que vous voulez utiliser. Si vous ne connaissez pas cet ID, cette applet de commande renvoie la liste de tous les modèles de paramètres :
  
   ```powershell
   Get-AzureADDirectorySettingTemplate
   ```
   Cet appel d’applet de commande retourne tous les modèles disponibles :
  
   ```powershell
   Id                                   DisplayName         Description
   --                                   -----------         -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
   16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
   ```
2. Pour ajouter une URL d’instructions d’utilisation, vous devez d’abord obtenir l’objet SettingsTemplate qui définit la valeur de l’URL d’instructions d’utilisation ; autrement dit, le modèle Group.Unified :
  
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
   ```
3. Ensuite, créez un nouvel objet Settings basé sur ce modèle :
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Puis mettez à jour la valeur des instructions d’utilisation :
  
   ```powershell
   $Setting["UsageGuidelinesUrl"] = "https://guideline.example.com"
   ```  
5. Appliquez ensuite le paramètre :
  
   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
6. Vous pouvez lire les valeurs à l’aide de :

  ```powershell
   $Setting.Values
   ```  
## <a name="update-settings-at-the-directory-level"></a>Mettre à jour des paramètres au niveau du répertoire
Pour mettre à jour la valeur pour UsageGuideLinesUrl dans la définition d’un modèle, modifiez l’URL à l’étape 4 ci-dessus, puis effectuez l’étape 5 pour définir la nouvelle valeur.

Pour supprimer la valeur de UsageGuideLinesUrl, modifiez l’URL pour être une chaîne vide à l’aide d’étape 4 ci-dessus :

 ```powershell
   $Setting["UsageGuidelinesUrl"] = ""
   ```  
Puis procédez étape 5 pour définir la nouvelle valeur.

## <a name="template-settings"></a>Paramètres de modèle
Voici les paramètres définis dans l’objet SettingsTemplate Group.Unified. Sauf indication contraire, ces fonctionnalités nécessitent une licence Azure Active Directory Premium P1. 

| **Paramètre** | **Description** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Tapez : Boolean<li>Valeur par défaut : True |Indicateur spécifiant si la création de groupes Office 365 est autorisée dans le répertoire par les utilisateurs non administrateurs. Ce paramètre ne nécessite pas une licence Azure Active Directory Premium P1.|
|  <ul><li>GroupCreationAllowedGroupId<li>Tapez : Chaîne<li>Valeur par défaut : “” |GUID du groupe de sécurité pour lequel les membres sont autorisés à créer des groupes Office 365 même lorsque EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Tapez : String<li>Valeur par défaut : “” |Lien vers les instructions d’utilisation du groupe. |
|  <ul><li>ClassificationDescriptions<li>Tapez : Chaîne<li>Valeur par défaut : “” | Liste séparée par des virgules des descriptions de classification. La valeur de ClassificationDescriptions est uniquement valide au format suivant :<br>$setting[“ClassificationDescriptions”] ="Classification:Description,Classification:Description"<br>où Classification correspondent aux chaînes dans le ClassificationList.|
|  <ul><li>DefaultClassification<li>Tapez : Chaîne<li>Valeur par défaut : “” | Classification qui doit être utilisée en tant que classement par défaut pour un groupe si aucune classification n’a été spécifiée.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Tapez : String<li>Valeur par défaut : “” | Chaîne d’une longueur maximale de 64 caractères qui définit la convention d’affectation de noms configurée pour les groupes Office 365. Pour plus d’informations, consultez [Appliquer une stratégie de nommage pour les groupes Office 365 dans Azure Active Directory (préversion)](groups-naming-policy.md). |
| <ul><li>CustomBlockedWordsList<li>Tapez : String<li>Valeur par défaut : “” | Chaîne d’expressions séparées par des virgules que les utilisateurs ne seront pas autorisés à employer dans les noms ou alias de groupe. Pour plus d’informations, consultez [Appliquer une stratégie de nommage pour les groupes Office 365 dans Azure Active Directory (préversion)](groups-naming-policy.md). |
| <ul><li>EnableMSStandardBlockedWords<li>Tapez : Boolean<li>Valeur par défaut : false | Ne pas utiliser
|  <ul><li>AllowGuestsToBeGroupOwner<li>Tapez : Boolean<li>Valeur par défaut : False | Valeur booléenne indiquant si un utilisateur invité peut être ou non un propriétaire de groupes. |
|  <ul><li>AllowGuestsToAccessGroups<li>Tapez : Boolean<li>Valeur par défaut : True | Valeur booléenne indiquant si un utilisateur invité peut avoir ou non accès au contenu des groupes Office 365.  Ce paramètre ne nécessite pas une licence Azure Active Directory Premium P1.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Tapez : Chaîne<li>Valeur par défaut : “” | URL d’un lien vers les instructions d’utilisation de l’invité. |
|  <ul><li>AllowToAddGuests<li>Tapez : Boolean<li>Valeur par défaut : True | Une valeur booléenne indiquant si l’utilisateur est autorisé ou non à ajouter des invités à ce répertoire.|
|  <ul><li>ClassificationList<li>Tapez : Chaîne<li>Valeur par défaut : “” |Liste de valeurs de classification valides séparées par des virgules qui peuvent être appliquées à des groupes Office 365. |

## <a name="example-configure-guest-policy-for-groups-at-the-directory-level"></a>Exemple : Configurer la stratégie de l’invité pour les groupes au niveau du répertoire
1. Obtenir tous les modèles de paramètre :
  ```powershell
   Get-AzureADDirectorySettingTemplate
   ```
2. Pour définir la stratégie de l’invité pour les groupes au niveau du répertoire, vous avez besoin d’un modèle Group.Unified
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
   ```
3. Ensuite, créez un nouvel objet Settings basé sur ce modèle :
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Puis mettez à jour le paramètre de AllowToAddGuests
   ```powershell
   $Setting["AllowToAddGuests"] = $False
   ```  
5. Appliquez ensuite le paramètre :
  
   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
6. Vous pouvez lire les valeurs à l’aide de :

  ```powershell
   $Setting.Values
   ```   

## <a name="read-settings-at-the-directory-level"></a>Lire les paramètres au niveau du répertoire

Si vous connaissez le nom du paramètre à récupérer, vous pouvez utiliser l’applet de commande ci-dessous pour récupérer la valeur des paramètres actuelle. Dans cet exemple, nous récupérons la valeur d’un paramètre nommé « UsageGuidelinesUrl ». 

  ```powershell
  (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
  ```
Les étapes suivantes permettent de lire les paramètres au niveau du répertoire qui s’appliquent à tous les groupes Office du répertoire.

1. Lisez tous les paramètres du répertoire existant :
   ```powershell
   Get-AzureADDirectorySetting -All $True
   ```
   Cette applet de commande retourne une liste de tous les paramètres du répertoire :
   ```powershell
   Id                                   DisplayName   TemplateId                           Values
   --                                   -----------   ----------                           ------
   c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
   ```

2. Lisez tous les paramètres d’un groupe spécifique :
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
   ```

3. Lire toutes les valeurs de paramètres de répertoire d’un objet de paramètres de répertoire spécifique, à l’aide du GUID de l’ID de paramètres :
   ```powershell
   (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
   ```
   Cette applet de commande retourne les noms et les valeurs dans cet objet de paramètres pour ce groupe spécifique :
   ```powershell
   Name                          Value
   ----                          -----
   ClassificationDescriptions
   DefaultClassification
   PrefixSuffixNamingRequirement
   CustomBlockedWordsList        
   AllowGuestsToBeGroupOwner     False 
   AllowGuestsToAccessGroups     True
   GuestUsageGuidelinesUrl
   GroupCreationAllowedGroupId
   AllowToAddGuests              True
   UsageGuidelinesUrl            https://guideline.example.com
   ClassificationList
   EnableGroupCreation           True
   ```

## <a name="remove-settings-at-the-directory-level"></a>Supprimer des paramètres au niveau du répertoire
Les étapes suivantes permettent de supprimer les paramètres au niveau du répertoire qui s’appliquent à tous les groupes Office du répertoire.
  ```powershell
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="update-settings-for-a-specific-group"></a>Mettre à jour les paramètres d’un groupe spécifique

1. Recherchez le modèle de paramètres nommé « Groups.Unified.Guest »
   ```powershell
   Get-AzureADDirectorySettingTemplate
  
   Id                                   DisplayName            Description
   --                                   -----------            -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
   ```
2. Récupérez l’objet de modèle pour le modèle Groups.Unified.Guest :
   ```powershell
   $Template1 = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
   ```
3. Créez un nouvel objet settings à partir du modèle :
   ```powershell
   $SettingCopy = $Template1.CreateDirectorySetting()
   ```

4. Définissez le paramètre sur la valeur requise :
   ```powershell
   $SettingCopy["AllowToAddGuests"]=$False
   ```
5. Créez un nouveau paramètre pour le groupe requis dans le répertoire :
   ```powershell
   New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $SettingCopy
   ```
6. Pour vérifier les paramètres, exécutez la commande suivante :
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups | fl Values
   ```

## <a name="cmdlet-syntax-reference"></a>Informations de référence sur la syntaxe des applets de commande
Vous trouverez plus d’informations sur Azure Active Directory PowerShell dans la page dédiée aux [applets de commande Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Documentation supplémentaire

* [Gestion de l’accès aux ressources avec les groupes Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Intégration des identités locales dans Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
