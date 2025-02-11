---
title: 'Azure Active Directory Domain Services : Administrer un domaine managé | Microsoft Docs'
description: Administrer les domaines gérés par les services de domaine Azure Active Directory
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: d4fdbc75-3e6b-4e20-8494-5dcc3bf2220a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 31221b17059bb7754e24ce1139e8cb2ec22cf7a7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66245863"
---
# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Administrer un domaine géré par les services de domaine Azure Active Directory
Cet article indique comment administrer un domaine géré par les services de domaine Azure Active Directory (AD).

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Avant de commencer
Pour effectuer les tâches décrites dans cet article, vous avez besoin de ce qui suit :

1. Un **abonnement Azure**valide.
2. Un **répertoire Azure AD** , synchronisé avec un répertoire local ou un répertoire cloud uniquement.
3. **services de domaine Azure AD** , qui doivent être activés pour le répertoire Azure AD. Si ce n’est déjà fait, suivez l’ensemble des tâches décrites dans le [Guide de mise en route](create-instance.md).
4. Une **machine virtuelle jointe au domaine** , qui vous permet d’administrer le domaine géré par les services de domaine Azure AD. Si vous ne disposez pas de cette machine, suivez toutes les tâches décrites dans l’article intitulé [Joindre une machine virtuelle Windows Server à un domaine géré](active-directory-ds-admin-guide-join-windows-vm.md).
5. Vous devez disposer des informations d’identification d’un **compte d’utilisateur appartenant au groupe « AAD DC Administrators »** dans votre répertoire, pour administrer votre domaine géré.

<br>

## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Tâches d’administration pouvant être effectuées sur un domaine géré
Les membres du groupe « AAD DC Administrators » bénéficient de privilèges leur permettant d’effectuer les tâches suivantes sur le domaine managé :

* joindre des ordinateurs au domaine géré ;
* configurer le GPO intégré pour les conteneurs « Ordinateurs AADDC » et « Utilisateurs AADDC » au sein du domaine géré ;
* administrer le DNS sur le domaine géré ;
* créer et administrer des unités d’organisation (UO) personnalisées dans le domaine géré.
* obtenir un accès d’administrateur aux ordinateurs joints au domaine géré ;

## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Privilèges d’administrateur dont vous ne disposez pas concernant un domaine géré
Le domaine est géré par Microsoft, y compris les activités telles que les mises à jour correctives, le monitoring et l’exécution des sauvegardes. Il est verrouillé ; vous ne disposez pas de privilèges permettant d’effectuer certaines tâches d’administration sur le domaine. Voici quelques exemples de tâches que vous ne pouvez pas réaliser.

* Vous ne disposez pas des privilèges Administrateur de domaine ou d’entreprise sur le domaine managé.
* Vous ne pouvez pas étendre le schéma du domaine managé.
* Vous ne pouvez pas vous connecter aux contrôleurs du domaine managé à l’aide du Bureau à distance.
* Vous ne pouvez pas ajouter de contrôleurs de domaine au domaine managé.

## <a name="task-1---create-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Tâche 1 : créer une machine virtuelle Windows Server jointe au domaine afin d’administrer le domaine managé à distance
Vous pouvez administrer les domaines gérés par les services de domaine Azure Active Directory (AD) par l’intermédiaire des outils d’administration familiers d’Active Directory, par exemple le Centre d’administration Active Directory (ADAC, Active Directory Administrative Center) ou AD PowerShell. Les administrateurs clients ne disposent pas des privilèges permettant la connexion aux contrôleurs de domaine sur le domaine géré, via le Bureau à distance. Les membres du groupe « AAD DC Administrators » peuvent administrer les domaines managés à distance, à l’aide des outils d’administration AD d’un ordinateur client/Windows Server joint au domaine managé. Les outils d’administration AD peuvent être installés en tant que composants des Outils d’administration de serveur distant (fonction en option) sur les machines clientes et Windows Server jointes au domaine géré.

La première étape consiste à configurer une machine virtuelle Windows Server qui est jointe au domaine géré. Pour savoir comment procéder, consultez l’article [Joindre une machine virtuelle Windows Server à un domaine géré par les services de domaine Azure AD](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Administrer à distance le domaine géré à partir d’un ordinateur client (par exemple, Windows 10)
Les instructions contenues dans cet article utilisent une machine virtuelle Windows Server pour administrer le domaine géré par les services de domaine Azure AD. Toutefois, vous pouvez également choisir d’utiliser une machine virtuelle cliente Windows (par exemple, Windows 10) pour ce faire.

Vous pouvez [installer les Outils d’administration de serveur distant](https://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) sur une machine virtuelle cliente Windows en suivant les instructions de TechNet.

## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Tâche 2 : installer les Outils d’administration Active Directory sur la machine virtuelle
Pour installer les outils d’administration Active Directory sur la machine virtuelle jointe à un domaine, suivez les étapes ci-dessous. Pour en savoir plus sur [l’installation et l’utilisation des Outils d’administration de serveur distant](https://technet.microsoft.com/library/hh831501.aspx), voir TechNet.

1. Accédez au portail Azure. Cliquez sur **Toutes les ressources** dans le panneau gauche. Recherchez et cliquez sur la machine virtuelle que vous avez créée dans la tâche 1.
2. Cliquez sur le bouton **Connecter** sous l’onglet Vue d’ensemble. Un fichier de protocole Remote Desktop Protocol (.rdp) est créé et téléchargé.

    ![Se connecter à une machine virtuelle Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
3. Pour vous connecter à votre machine virtuelle, ouvrez le fichier RDP téléchargé. À l’invite, cliquez sur **Se connecter**. Utilisez les informations d’identification d’un utilisateur appartenant au groupe « AAD DC Administrators ». Par exemple, « bob@domainservicespreview.onmicrosoft.com ». Un avertissement de certificat peut s’afficher pendant le processus de connexion. Cliquez sur Oui ou Continuer pour poursuivre le processus de connexion.
4. Dans l’écran d’accueil, ouvrez **Gestionnaire de serveur**. Dans le volet central de la fenêtre Gestionnaire de serveur, cliquez sur l’option **Ajouter des rôles et fonctionnalités** .

    ![Lancer le gestionnaire de serveur sur la machine virtuelle](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)
5. Sur la page **Avant de commencer** de **l’Assistant Ajout de rôles et de fonctionnalités**, cliquez sur **Suivant**.

    ![Page Avant de commencer](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)
6. Sur la page **Type d’installation**, laissez l’option **Installation basée sur un rôle ou une fonctionnalité** sélectionnée et cliquez sur **Suivant**.

    ![Page Type d’installation](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)
7. Sur la page **Sélection du serveur**, choisissez la machine virtuelle actuelle dans le pool de serveurs, puis cliquez sur **Suivant**.

    ![Page Sélection du serveur](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)
8. Sur la page **Rôles de serveurs**, cliquez sur **Suivant**.
9. Sur la page **Fonctionnalités**, cliquez sur le nœud **Outils d’administration de serveur distant** pour le développer, puis sur le nœud **Outils d’administration de rôles**, pour le développer. Sélectionnez **Outils AD DS et AD LDS** dans la liste des outils d’administration de rôles.

    ![Page Fonctionnalités](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)
10. Dans la page **Confirmation**, cliquez sur **Installer** pour installer la fonctionnalité Outils AD DS et AD LDS sur la machine virtuelle. Une fois l’installation de la fonctionnalité terminée, cliquez sur **Fermer** afin de fermer l’Assistant **Ajout de rôles et de fonctionnalités**.

    ![Page Confirmation](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)

## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Tâche 3: se connecter au domaine géré et l’explorer
Vous pouvez à présent utiliser les outils d’administration AD Windows Server pour explorer et administrer le domaine managé.

> [!NOTE]
> Vous devez être membre du groupe « AAD DC Administrators » pour administrer le domaine géré.
>
>

1. Dans l’écran d’accueil, cliquez sur **Outils d’administration**. Les outils d’administration AD doivent apparaître comme installés sur la machine virtuelle.

    ![Outils d’administration installés sur le serveur](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)
2. Cliquez sur **Centre d’administration Active Directory**.

    ![Centre d’administration Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)
3. Pour explorer le domaine, cliquez sur son nom dans le volet gauche (par exemple, contoso100.com). Vous pouvez remarquer deux conteneurs appelés « Ordinateurs AADDC » et « Utilisateurs AADDC », respectivement.

    ![ADAC - Affichage du domaine](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)
4. Cliquez sur le conteneur appelé **Utilisateurs AADDC** pour afficher tous les utilisateurs et groupes qui appartiennent au domaine géré. Les comptes d’utilisateur et groupes de votre client Azure AD doivent apparaître dans ce conteneur. Notez dans cet exemple, un compte d’utilisateur pour l’utilisateur « bob » et un groupe appelé « AAD DC Administrators » disponibles dans ce conteneur.

    ![ADAC - Utilisateurs du domaine](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)
5. Cliquez sur le conteneur **Ordinateurs AADDC** pour afficher les ordinateurs joints à ce domaine géré. Vous devez voir apparaître une entrée pour la machine virtuelle en cours, qui est jointe au domaine. Les comptes d’ordinateurs de l’ensemble des ordinateurs joints au domaine géré par les services de domaine Azure AD sont stockés dans ce conteneur « Ordinateurs AADDC ».

    ![ADAC - Ordinateurs joints au domaine](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Contenu associé
* [Services de domaine Azure AD : guide de mise en route](create-instance.md)
* [Joindre une machine virtuelle Windows Server à un domaine géré par les services de domaine Azure AD](active-directory-ds-admin-guide-join-windows-vm.md)
* [Déployer les Outils d’administration de serveur distant](https://technet.microsoft.com/library/hh831501.aspx)
