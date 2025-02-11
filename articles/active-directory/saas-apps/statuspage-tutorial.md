---
title: "Didacticiel : Intégration d'Azure Active Directory à StatusPage | Microsoft Docs"
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et StatusPage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: 49f77da5843bc2438ea7f82475b4faf753b66f09
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65866800"
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Didacticiel : Intégration d'Azure Active Directory à StatusPage

Dans ce didacticiel, vous allez apprendre à intégrer StatusPage à Azure Active Directory (Azure AD).
L’intégration de StatusPage dans Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à StatusPage.
* Vous pouvez permettre aux utilisateurs de se connecter automatiquement à StatusPage (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec StatusPage, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/)
* Abonnement StatusPage pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* StatusPage prend en charge l’authentification unique lancée par le **fournisseur d’identité**

## <a name="adding-statuspage-from-the-gallery"></a>Ajout de StatusPage à partir de la galerie

Pour configurer l’intégration de StatusPage avec Azure AD, vous devez ajouter StatusPage à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter StatusPage à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **StatusPage**, sélectionnez **StatusPage** dans le panneau de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![StatusPage dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec StatusPage à l’aide d’un utilisateur de test appelé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre l’utilisateur Azure AD et l’utilisateur StatusPage associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec StatusPage, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique StatusPage](#configure-statuspage-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test StatusPage](#create-statuspage-test-user)** pour avoir un équivalent de Britta Simon dans StatusPage lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec StatusPage, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **StatusPage**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Sur la page **Configurer l’authentification unique avec SAML**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL StatusPage](common/idp-intiated.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant :
    
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/`|
    | `https://<subdomain>.statuspage.io/`|

    b. Dans la zone de texte **URL de réponse**, tapez une URL au format suivant :
    
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume`|
    | `https://<subdomain>.statuspage.io/sso/saml/consume`|

    > [!NOTE]
    > Contactez l’équipe de support StatusPage à l’adresse [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io)pour demander les métadonnées nécessaires à la configuration de l’authentification unique. 
    >
    > a. À partir des métadonnées, copiez la valeur de l’émetteur et collez-la dans la zone de texte **Identificateur** .
    >
    > b. À partir des métadonnées, copiez l’URL de réponse et collez-la dans la zone de texte **URL de réponse** .

5. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le **Certificat (Base64)** en fonction des options définies par rapport à vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/certificatebase64.png)

6. Dans la section **Configurer StatusPage**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-statuspage-single-sign-on"></a>Configurer l’authentification unique StatusPage

1. Dans une autre fenêtre de navigateur, connectez-vous à votre site d’entreprise StatusPage en tant qu’administrateur.

1. Dans la barre d’outils principale, cliquez sur **Manage Account**(Gérer le compte).

    ![Configurer l'authentification unique](./media/statuspage-tutorial/tutorial_statuspage_06.png)

1. Cliquez sur l’onglet **Single Sign-on** (Authentification unique).

    ![Configurer l'authentification unique](./media/statuspage-tutorial/tutorial_statuspage_07.png)

1. Sur la page de configuration de l’authentification unique, procédez comme suit :

    ![Configurer l'authentification unique](./media/statuspage-tutorial/tutorial_statuspage_08.png)

    ![Configurer l'authentification unique](./media/statuspage-tutorial/tutorial_statuspage_09.png)

    a. Dans la zone de texte **SSO Target URL** (URL cible d’authentification unique), collez la valeur **URL de connexion**, que vous avez copiée à partir du portail Azure.

    b. Ouvrez le certificat que vous avez téléchargé dans le Bloc-notes, copiez son contenu, puis collez-le dans la zone de texte **Certificat** .

    c. Cliquez sur **ENREGISTRER LA CONFIGURATION**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **BrittaSimon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez `brittasimon@yourcompanydomain.extension`  
    Par exemple, BrittaSimon@contoso.com

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à StatusPage.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **StatusPage**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **StatusPage**.

    ![Lien StatusPage dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-statuspage-test-user"></a>Créer un utilisateur de test StatusPage

L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans StatusPage.

StatusPage prend en charge l’approvisionnement juste-à-temps. Vous l’avez déjà activé dans [Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on).

**Pour créer un utilisateur appelé Britta Simon dans StatusPage, procédez comme suit :**

1. Connectez-vous à votre site d’entreprise StatusPage en tant qu’administrateur.

1. Dans le menu situé en haut, cliquez sur **Manage Account**.

    ![Configurer l'authentification unique](./media/statuspage-tutorial/tutorial_statuspage_06.png)

1. Cliquez sur l’onglet **Team Members** (Membres de l’équipe).
  
    ![Création d’un utilisateur de test Azure AD](./media/statuspage-tutorial/tutorial_statuspage_10.png) 

1. Cliquez sur l’onglet **ADD TEAM MEMBER** (Ajouter un membre d’équipe).
  
    ![Création d’un utilisateur de test Azure AD](./media/statuspage-tutorial/tutorial_statuspage_11.png) 

1. Entrez l'adresse e-mail, le prénom et le nom de l'utilisateur valide que vous souhaitez approvisionner dans les zones de texte correspondantes, à savoir, **Email Address** (Adresse e-mail), **First Name** (Prénom) et **Surname** (Nom). 

    ![Création d’un utilisateur de test Azure AD](./media/statuspage-tutorial/tutorial_statuspage_12.png) 

1. Pour **Role**, choisissez **Client Administrator**.

1. Cliquez sur **CREATE ACCOUNT** (Créer un compte).

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Le fait de cliquer sur la vignette StatusPage dans le panneau d’accès doit vous connecter automatiquement à l’application StatusPage pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
