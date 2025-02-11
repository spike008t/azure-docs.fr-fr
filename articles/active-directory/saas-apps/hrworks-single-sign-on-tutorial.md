---
title: 'Didacticiel : Intégration d’Azure Active Directory à HRworks Single Sign-On | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et HRworks Single Sign-On.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c4c5d434-3f8a-411e-83a5-c3d5276ddc0a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: c0c326525f5a551ddb00a709ed0a36a68a1343db
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66356481"
---
# <a name="tutorial-azure-active-directory-integration-with-hrworks-single-sign-on"></a>Didacticiel : Intégration d’Azure Active Directory à HRworks Single Sign-On | Microsoft Docs

Dans ce tutoriel, vous allez apprendre à intégrer HRworks Single Sign-On à Azure Active Directory (Azure AD).
L’intégration de HRworks Single Sign-On à Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à HRworks Single Sign-On.
* Vous pouvez permettre aux utilisateurs de se connecter automatiquement à HRworks Single Sign-On (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à HRworks Single Sign-On, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Abonnement HRworks Single Sign-On pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* HRworks Single Sign-On prend en charge l’authentification unique lancée par le **fournisseur de services**

## <a name="adding-hrworks-single-sign-on-from-the-gallery"></a>Ajout de HRworks Single Sign-On à partir de la galerie

Pour configurer l’intégration de HRworks Single Sign-On à Azure AD, vous devez ajouter HRworks Single Sign-On à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter HRworks Single Sign-On à partir de la galerie, effectuez les étapes suivantes :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **HRworks Single Sign-On**, sélectionnez **HRworks Single Sign-On** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![HRworks Single Sign-On dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec HRworks Single Sign-On, à l’aide d’un utilisateur de test appelé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur HRworks Single Sign-On associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec HRworks Single Sign-On, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique HRworks Single Sign-On](#configure-hrworks-single-sign-on-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test HRworks Single Sign-On](#create-hrworks-single-sign-on-test-user)** pour avoir un équivalent de Britta Simon dans HRworks Single Sign-On lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec HRworks Single Sign-On, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **HRworks Single Sign-On**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL HRworks Single Sign-On](common/sp-signonurl.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://login.hrworks.de/?companyId=<companyId>&directssologin=true`

    > [!NOTE]
    > Cette valeur n’est pas la valeur réelle. Mettez à jour la valeur avec l’URL de connexion réelle. Pour obtenir cette valeur, contactez l’[équipe du support technique HRworks Single Sign-On](mailto:support@hrworks.de). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies en fonction de vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer HRworks Single Sign-On**, copiez la ou les URL appropriées, selon vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-hrworks-single-sign-on-single-sign-on"></a>Configurer l’authentification unique HRworks Single Sign-On

1. Dans une autre fenêtre de navigateur web, connectez-vous à HRworks Single Sign-On en tant qu’administrateur.

2. Cliquez sur **Administrator** > **Basics** > **Security** > **Single Sign-on** (Authentification unique) sur le côté gauche de la barre de menus, puis effectuez les étapes suivantes :

    ![Configurer l’authentification unique](./media/hrworks-single-sign-on-tutorial/configure01.png)

    a. Cochez la case **Use Single Sign-on** (Utiliser l’authentification unique).

    b. Sélectionnez **XML Metadata** (Métadonnées XML) comme **méthode d’entrée des métadonnées**.

    c. Sélectionnez **Individual NameID identifier** (Identificateur NameID individuel) comme **valeur pour NameID**.

    d. Dans le Bloc-notes, ouvrez le XML des métadonnées que vous avez téléchargé à partir du portail Azure, copiez son contenu, puis collez-le dans la zone de texte **Metadata** (Métadonnées).

    e. Cliquez sur **Enregistrer**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD 

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **BrittaSimon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez le nom d’utilisateur, tel que BrittaSimon@contoso.com.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à HRworks Single Sign-On.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **HRworks Single Sign-On**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **HRworks Single Sign-On**.

    ![Lien HRworks Single Sign-On dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-hrworks-single-sign-on-test-user"></a>Créer un utilisateur de test HRworks Single Sign-On

Pour permettre aux utilisateurs Azure AD de se connecter à HRworks Single Sign-On, ils doivent y être provisionnés. Dans HRworks Single Sign-On, le provisionnement est une tâche manuelle.

**Pour approvisionner un compte d’utilisateur, procédez comme suit :**

1. Connectez-vous à HRworks Single Sign-On en tant qu’administrateur.

2. Cliquez sur **Administrator** > **Persons** > **Persons** > **New person** (Nouvelle personne) sur le côté gauche de la barre de menus.

     ![Configurer l'authentification unique](./media/hrworks-single-sign-on-tutorial/configure02.png)

3. Dans le menu contextuel, cliquez sur **Next** (Suivant).

    ![Configurer l'authentification unique](./media/hrworks-single-sign-on-tutorial/configure03.png)

4. Dans la fenêtre contextuelle **Create new person with country for legal terms** (Créer une personne avec un pays pour les conditions juridiques), remplissez les détails respectifs, tels que **First name**(Prénom) et **Last name** (Nom), puis cliquez sur **Create**.
    
    ![Configurer l'authentification unique](./media/hrworks-single-sign-on-tutorial/configure04.png)

### <a name="test-single-sign-on"></a>Tester l’authentification unique 

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette HRworks Single Sign-On dans le volet d’accès, vous devez être connecté automatiquement à l’application HRworks Single Sign-On pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

