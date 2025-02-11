---
title: Créer un pool d’hôte de Windows Virtual Desktop Preview avec PowerShell - Azure
description: Comment créer un pool d’hôte dans la version préliminaire de bureau virtuel Windows avec les applets de commande PowerShell.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/06/2019
ms.author: helohr
ms.openlocfilehash: a58e059e800b13d01ba8e50880bd75077d4418ae
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65833965"
---
# <a name="create-a-host-pool-with-powershell"></a>Créer un pool d’hôtes avec PowerShell

Les pools d’hôtes sont une collection d’une ou de plusieurs machines virtuelles identiques dans des environnements de locataires Windows Virtual Desktop Preview. Chaque pool d’hôtes peut contenir un groupe d’applications avec lequel les utilisateurs peuvent interagir comme ils le feraient sur un ordinateur de bureau physique.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Utilisez votre client PowerShell pour créer un pool de l’hôte

Tout d’abord, si vous ne l’avez pas déjà fait, [téléchargez et importez le module PowerShell Windows Virtual Desktop](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) à utiliser dans votre session PowerShell.

Exécutez l’applet de commande suivante pour vous connecter à l’environnement de bureau virtuel Windows

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Ensuite, exécutez cette applet de commande pour créer un nouveau pool d’hôte dans votre client de bureau virtuel Windows :

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Exécutez l’applet de commande suivante pour créer un jeton d’inscription pour autoriser un hôte de session rejoignent le pool de l’hôte et l’enregistrer dans un nouveau fichier sur votre ordinateur local. Vous pouvez spécifier la durée pendant laquelle le jeton d’inscription n’est valide en utilisant le paramètre - ExpirationHours.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token > <PathToRegFile>
```

Après cela, exécutez la commande suivante pour ajouter des utilisateurs Azure Active Directory au groupe d’application de bureau par défaut pour le pool de l’hôte.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

Le **Add-RdsAppGroupUser** applet de commande ne prend pas en charge l’ajout de groupes de sécurité et ajoute uniquement un seul utilisateur à la fois pour le groupe de l’application. Si vous souhaitez ajouter plusieurs utilisateurs à l’app group, réexécutez l’applet de commande avec les noms de principal d’utilisateur approprié.

Exécutez l’applet de commande suivante pour exporter le jeton d’inscription à une variable, vous utiliserez plus tard dans [inscrire les machines virtuelles au pool hôte bureau virtuel Windows](#register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Créer des machines virtuelles pour le pool de l’hôte

Vous pouvez maintenant créer une machine virtuelle Azure qui peut être jointe à votre pool d’hôte de bureau virtuel Windows.

Vous pouvez créer une machine virtuelle de plusieurs façons :

- [Créer une machine virtuelle à partir d’une image de galerie Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Créer une machine virtuelle à partir d’une image managée](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Créer une machine virtuelle à partir d’une image non managée](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-preview-agent-installations"></a>Préparer les ordinateurs virtuels pour les installations d’agent Windows Virtual Desktop Preview

Vous devez effectuer les opérations suivantes pour préparer vos machines virtuelles avant de pouvoir installer les agents de bureau virtuel Windows et inscrire les machines virtuelles à votre pool d’hôte de bureau virtuel Windows :

- Vous devez la machine de jonction de domaine. Ainsi, les utilisateurs entrants de bureau virtuel Windows à être mappées à partir de leur compte Azure Active Directory à leur compte Active Directory et a été autorisé à accéder à la machine virtuelle.
- Vous devez installer le rôle hôte de Session de bureau à distance (RDSH) si la machine virtuelle exécute un système d’exploitation de Windows Server. Le rôle RDSH permet aux agents bureau virtuel Windows à l’installation.

Pour correctement jonction de domaine, procédez comme suit sur chaque machine virtuelle :

1. [Se connecter à la machine virtuelle](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) avec les informations d’identification que vous avez fourni lors de la création de la machine virtuelle.
2. Sur la machine virtuelle, lancez **le panneau de configuration** et sélectionnez **système**.
3. Sélectionnez **nom de l’ordinateur**, sélectionnez **modifier les paramètres**, puis sélectionnez **modification...**
4. Sélectionnez **domaine** , puis entrez le domaine Active Directory sur le réseau virtuel.
5. S’authentifier avec un compte de domaine qui dispose de privilèges pour les machines de jonction de domaine.

    >[!NOTE]
    > Si vous rejoignez vos machines virtuelles dans un environnement Azure AD Domain Services, assurez-vous que votre utilisateur de jonction de domaine est également un membre de la [groupe AAD DC Administrators](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-admingroup#task-3-configure-administrative-group).

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool"></a>Inscrire les machines virtuelles au pool hôte Windows Virtual Desktop Preview

Enregistrer les ordinateurs virtuels à un pool d’hôte de bureau virtuel Windows est aussi simple que l’installation des agents de bureau virtuel Windows.

Pour inscrire les agents de bureau virtuel de Windows, procédez comme suit sur chaque machine virtuelle :

1. [Se connecter à la machine virtuelle](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) avec les informations d’identification que vous avez fourni lors de la création de la machine virtuelle.
2. Téléchargez et installez l’Agent de bureau virtuel Windows.
   - Téléchargez le [Agent bureau virtuel Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv).
   - Cliquez sur le programme d’installation téléchargé, sélectionnez **propriétés**, sélectionnez **Unblock**, puis sélectionnez **OK**. Cela permettra à votre système d’approuver le programme d’installation.
   - Exécutez le programme d’installation. Lorsque le programme d’installation vous demande le jeton d’inscription, entrez la valeur que vous avez obtenu à partir de la **RdsRegistrationInfo d’exportation** applet de commande.
3. Téléchargez et installez le Windows Virtual Desktop Agent du chargeur de démarrage.
   - Téléchargez le [chargeur de démarrage de l’Agent de bureau virtuel Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH).
   - Cliquez sur le programme d’installation téléchargé, sélectionnez **propriétés**, sélectionnez **Unblock**, puis sélectionnez **OK**. Cela permettra à votre système d’approuver le programme d’installation.
   - Exécutez le programme d’installation.

>[!IMPORTANT]
>Pour contribuer à sécuriser votre environnement Windows Virtual Desktop dans Azure, nous vous recommandons de ne pas ouvrir le port entrant 3389 sur vos machines virtuelles. Windows Virtual Desktop ne nécessite pas l’ouverture du port entrant 3389 pour permettre aux utilisateurs d’accéder aux machines virtuelles du pool hôte. Si vous devez ouvrir le port 3389 pour résoudre des problèmes, nous vous recommandons d’utiliser un [accès à la machine virtuelle juste-à-temps](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez créé un pool de l’hôte, vous pouvez le remplir avec RemoteApps. Pour en savoir plus sur la façon de gérer des applications dans Windows Virtual Desktop, consultez le tutoriel Gérer les groupes d’applications.

> [!div class="nextstepaction"]
> [Gérer les groupes d’applications](./manage-app-groups.md)
