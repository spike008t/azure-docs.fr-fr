---
title: 'Didacticiel : Intégration d’Azure Active Directory à Tableau Online | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Tableau Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/05/2019
ms.author: jeedes
ms.openlocfilehash: 352ad9473a1c1a9360ddceb720ff968f4e97e012
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65889263"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Didacticiel : Intégration d’Azure Active Directory dans Tableau Online

Dans ce didacticiel, vous allez apprendre à intégrer Tableau Online à Azure Active Directory (Azure AD).
L’intégration de Tableau Online à Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à Tableau Online.
* Vous pouvez permettre à vos utilisateurs d’être connectés automatiquement à Tableau Online (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Tableau Online, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/)
* Un abonnement Tableau Online pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Tableau Online prend en charge l’authentification unique lancée par le **fournisseur de services**

## <a name="adding-tableau-online-from-the-gallery"></a>Ajout de Tableau Online à partir de la galerie

Pour configurer l’intégration de Tableau Online à Azure AD, vous devez ajouter Tableau Online à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Tableau Online à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **Tableau Online**, sélectionnez **Tableau Online** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![Tableau Online dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Tableau Online à l’aide d’un utilisateur de test appelé **Britta Simon**.
Pour que l’authentification unique fonctionne, vous devez associer l’utilisateur Azure AD à l’utilisateur Tableau Online.

Pour configurer et tester l’authentification unique Azure AD avec Tableau Online, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique Tableau Online](#configure-tableau-online-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test Tableau Online](#create-tableau-online-test-user)** pour avoir un équivalent de Britta Simon dans Tableau Online lié à la représentation Azure AD de l’utilisateur.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec Tableau Online, procédez comme suit :

1. Dans le [portail Azure](https://portal.azure.com/), sur la page d’intégration de l’application **Tableau Online**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL Tableau Online](common/sp-identifier.png)

    a. Dans la zone de texte **URL de connexion**, tapez l’URL : `https://sso.online.tableau.com/public/sp/login?alias=<entityid>`

    b. Dans la zone de texte **Identificateur (ID d’entité)** , tapez l’URL suivante : `https://sso.online.tableau.com/public/sp/metadata?alias=<entityid>`

    > [!NOTE]
    > Vous obtiendrez la valeur `<entityid>` à partir de la section **Configurer Tableau Online** de ce didacticiel. La valeur d’ID d’entité sera la valeur **Identificateur Azure AD** figurant dans la section **Configurer Tableau Online**.

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer Tableau Online**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-tableau-online-single-sign-on"></a>Configurer l’authentification unique Tableau Online

1. Dans une autre fenêtre du navigateur, connectez-vous à votre application Tableau Online. Cliquez sur **Settings** (Paramètres), puis sur **Authentication** (Authentification).

    ![Configurer l'authentification unique](./media/tableauonline-tutorial/tutorial_tableauonline_09.png)

2. Pour activer SAML, dans la section **Types d’authentification**, cochez **Enable an additional authentication method**  (Activer une méthode d’authentification supplémentaire), puis cochez la case **SAML**.

    ![Configurer l'authentification unique](./media/tableauonline-tutorial/tutorial_tableauonline_12.png)

3. Faites défiler la page jusqu’à la section **Import metadata file into Tableau Online** (Importer le fichier de métadonnées dans Tableau Online).  Cliquez sur Browse et importez le fichier de métadonnées que vous avez téléchargé à partir d’Azure AD. Cliquez alors sur **Apply (Appliquer)** .

   ![Configurer l'authentification unique](./media/tableauonline-tutorial/tutorial_tableauonline_13.png)

4. Dans la section **Match assertions** (Faire correspondre les assertions), insérez les noms d’assertion du fournisseur d’identité correspondants pour **l’adresse e-mail**, le **prénom** et le **nom**. Pour obtenir ces informations à partir d’Azure AD : 
  
    a. Dans le portail Azure, accédez à la page d’intégration de l’application **Tableau Online**.

    b. Dans la section **Attributs et revendications de l’utilisateur**, cliquez sur l’icône de modification.

   ![Configurer l'authentification unique](./media/tableauonline-tutorial/attributesection.png)

    c. Copiez la valeur de l’espace de noms de ces attributs : prénom, adresse e-mail et nom de famille en procédant comme suit :

   ![Authentification unique Azure AD](./media/tableauonline-tutorial/tutorial_tableauonline_10.png)

    d. Cliquez sur la valeur **user.givenname**.

    e. Copiez la valeur à partir de la zone de texte **Espace de noms**.

    ![Configurer l'authentification unique](./media/tableauonline-tutorial/attributesection2.png)

    f. Pour copier les valeurs d’espace de noms pour l’adresse e-mail et le nom de famille, répétez les étapes ci-dessus.

    g. Basculez dans l’application Tableau Online, puis définissez la section **Attributs et revendications de l’utilisateur** comme suit :

    * Email (Adresse de messagerie) : **mail** ou **userPrincipalName**

    * First name (Prénom) : **givenName**

    * Last name (Nom) : **surname**

    ![Configurer l'authentification unique](./media/tableauonline-tutorial/tutorial_tableauonline_14.png)

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
    Par exemple, BrittaSimon\@contoso.com

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Tableau Online.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **Tableau Online**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Tableau Online**.

    ![Lien Tableau Online dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-tableau-online-test-user"></a>Créer un utilisateur de test Tableau Online

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Tableau Online.

1. Dans **Tableau Online**, cliquez sur **Paramètres**, puis sur la section **Authentification**. Faites défiler la page jusqu’à la section **Manage Users**. Cliquez sur **Ajouter des utilisateurs**, puis sur **Entrer les adresses de messagerie**.
  
    ![Création d’un utilisateur de test Azure AD](./media/tableauonline-tutorial/tutorial_tableauonline_15.png)

2. Sélectionnez **Add users for (SAML) authentication**  (Ajouter des utilisateurs pour l’authentification (SAML)). Dans la zone de texte **Enter email addresses**, ajoutez britta.simon\@contoso.com
  
    ![Création d’un utilisateur de test Azure AD](./media/tableauonline-tutorial/tutorial_tableauonline_11.png)

3. Cliquez sur **Add Users**.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Le fait de cliquer sur la vignette Tableau Online dans le panneau d’accès doit vous connecter automatiquement à l’application Tableau Online pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
