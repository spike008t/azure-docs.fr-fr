---
title: 'Didacticiel : Intégration d’Azure Active Directory à ClickUp Productivity Platform | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et ClickUp Productivity Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06853edd-e8da-4ad2-a4e6-5555d592cee5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: 2a14b251df0bedbcd27ab4e9bf1abe9a27ad50d6
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65950916"
---
# <a name="tutorial-azure-active-directory-integration-with-clickup-productivity-platform"></a>Didacticiel : Intégration d’Azure Active Directory à ClickUp Productivity Platform

Dans ce tutoriel, vous allez apprendre à intégrer ClickUp Productivity Platform à Azure Active Directory (Azure AD).
L’intégration de ClickUp Productivity Platform à Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à ClickUp Productivity Platform.
* Vous pouvez permettre à vos utilisateurs d’être automatiquement connectés à ClickUp Productivity Platform (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à ClickUp Productivity Platform, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Abonnement ClickUp Productivity Platform pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* ClickUp Productivity Platform prend en charge l’authentification unique initiée par le **fournisseur de services**

## <a name="adding-clickup-productivity-platform-from-the-gallery"></a>Ajout de ClickUp Productivity Platform depuis la galerie

Pour configurer l’intégration de ClickUp Productivity Platform à Azure AD, vous devez ajouter ClickUp Productivity Platform, disponible depuis la galerie, à votre liste d’applications SaaS managées.

**Pour ajouter ClickUp Productivity Platform à partir de la galerie, effectuez les étapes suivantes :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **ClickUp Productivity Platform**, sélectionnez **ClickUp Productivity Platform** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![ClickUp Productivity Platform dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD auprès de ClickUp Productivity Platform, à l’aide d’un utilisateur de test nommé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur ClickUp Productivity Platform associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD auprès de ClickUp Productivity Platform, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique de ClickUp Productivity Platform](#configure-clickup-productivity-platform-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer l’utilisateur de test ClickUp Productivity Platform](#create-clickup-productivity-platform-test-user)** pour avoir dans ClickUp Productivity Platform un équivalent de Britta Simon lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD auprès de ClickUp Productivity Platform, effectuez les étapes suivantes :

1. Dans la page d’intégration de l’application **ClickUp Productivity Platform** sur le [portail Azure](https://portal.azure.com/), sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    ![Informations d’authentification unique relatives au domaine et aux URL ClickUp Productivity Platform](common/sp-identifier.png)

    a. Dans la zone de texte **URL de connexion**, tapez une URL : `https://app.clickup.com/login/sso`

    b. Dans la zone de texte **Identificateur (ID d’entité)** , saisissez une URL au format suivant : `https://api.clickup.com/v1/team/<team_id>/microsoft`

    > [!NOTE]
    > La valeur de l'identificateur n'est pas réelle. Vous devez remplacer cette valeur par l’identificateur réel. La procédure à suivre est expliquée plus loin dans ce tutoriel.

5. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur le bouton Copier pour copier l’**URL des métadonnées de fédération d’application**, puis enregistrez-la sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/copy-metadataurl.png)

### <a name="configure-clickup-productivity-platform-single-sign-on"></a>Configurer l’authentification unique de ClickUp Productivity Platform

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre locataire ClickUp Productivity Platform en tant qu’administrateur.

2. Cliquez sur **User profile** (Profil utilisateur) et sélectionnez **Settings** (Paramètres).

    ![Configuration ClickUp Productivity Platform](./media/clickup-productivity-platform-tutorial/configure1.png)

3. Sélectionnez **Microsoft**, sous Single Sign-On (SSO) Provider (Fournisseur d’authentification unique (SSO)).

    ![Configuration ClickUp Productivity Platform](./media/clickup-productivity-platform-tutorial/configure2.png)

4. Dans la page **Configure Microsoft Single Sign On** (Configurer l’authentification unique de Microsoft), effectuez les étapes suivantes :

    ![Configuration ClickUp Productivity Platform](./media/clickup-productivity-platform-tutorial/configure3.png)

    a. Cliquez sur **Copy** (Copier) pour copier la valeur de l’ID d’entité et collez-la dans la zone de texte **Identificateur (ID d’entité)** de la section **Configuration SAML de base** du portail Azure.
    
    b. Dans la zone de texte **Azure Federation Metadata URL** (URL des métadonnées de fédération d’application Azure), collez la valeur de l’URL des métadonnées de fédération d’application que vous avez copiée à partir du portail Azure, puis cliquez sur **Save** (Enregistrer).

5. Pour terminer l’installation, cliquez sur **Authenticate With Microsoft to complete setup** (S’authentifier auprès de Microsoft pour terminer l’installation) et authentifiez-vous avec un compte Microsoft.

    ![Configuration ClickUp Productivity Platform](./media/clickup-productivity-platform-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **BrittaSimon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez **brittasimon\@domainedevotreentreprise.extension**.  
    Par exemple, BrittaSimon@contoso.com

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en accordant l’accès à ClickUp Productivity Platform.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **ClickUp Productivity Platform**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **ClickUp Productivity Platform**.

    ![Lien ClickUp Productivity Platform dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-clickup-productivity-platform-test-user"></a>Créer l’utilisateur de test de ClickUp Productivity Platform

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre locataire ClickUp Productivity Platform en tant qu’administrateur.

2. Cliquez sur **User profile** (Profil utilisateur) et sélectionnez **Users** (Utilisateurs).

    ![Configuration ClickUp Productivity Platform](./media/clickup-productivity-platform-tutorial/user1.png)

3. Entrez l’adresse e-mail de l’utilisateur dans la zone de texte et cliquez sur **Invite** (Inviter).

    ![Configuration ClickUp Productivity Platform](./media/clickup-productivity-platform-tutorial/user2.png)

    > [!NOTE]
    > L’utilisateur reçoit la notification et doit accepter l’invitation pour activer le compte.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Le fait de cliquer sur la vignette ClickUp Productivity Platform dans le volet d’accès doit vous connecter automatiquement à l’application ClickUp Productivity Platform pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

