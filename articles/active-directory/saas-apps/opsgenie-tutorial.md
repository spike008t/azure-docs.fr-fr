---
title: 'Didacticiel : Intégration d’Azure Active Directory à OpsGenie | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et OpsGenie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 98e4d1870c57c9718e2d4293157b21ead8ea44e1
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65987487"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Didacticiel : Intégration d’Azure Active Directory à OpsGenie

Dans ce didacticiel, vous allez apprendre à intégrer OpsGenie à Azure Active Directory (Azure AD).
L’intégration d’OpsGenie dans Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à OpsGenie.
* Vous pouvez autoriser les utilisateurs à se connecter automatiquement à OpsGenie (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à OpsGenie, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Abonnement OpsGenie pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* OpsGenie prend en charge l’authentification unique lancée par le **fournisseur de services**

## <a name="adding-opsgenie-from-the-gallery"></a>Ajout de OpsGenie à partir de la galerie

Pour configurer l’intégration de OpsGenie à Azure AD, vous devez ajouter OpsGenie à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter OpsGenie à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **OpsGenie**, sélectionnez **OpsGenie** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![OpsGenie dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec OpsGenie, avec un utilisateur de test appelé **B. Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur OpsGenie associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec OpsGenie, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique OpsGenie](#configure-opsgenie-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test OpsGenie](#create-opsgenie-test-user)** pour obtenir un équivalent de Britta Simon dans OpsGenie lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec OpsGenie, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **OpsGenie**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL OpsGenie](common/sp-signonurl.png)

    Dans la zone de texte **URL de connexion**, tapez une URL : `https://app.opsgenie.com/auth/login`

5. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur le bouton Copier pour copier l’**URL des métadonnées de fédération d’application**, puis enregistrez-la sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/copy-metadataurl.png)

6. Dans la section **Configurer OpsGenie**, copiez l’URL ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-opsgenie-single-sign-on"></a>Configurer l’authentification unique OpsGenie

1. Ouvrez une autre instance de navigateur, puis connectez-vous à OpsGenie en tant qu’administrateur.

2. Cliquez sur **Paramètres**, puis cliquez sur l’onglet **Authentification unique**.
   
    ![Authentification unique OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

3. Pour activer l’authentification unique, sélectionnez **Activé**.
   
    ![Paramètres OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

4. Dans la section **Fournisseur** cliquez sur l’onglet **Azure Active Directory**.
   
    ![Paramètres OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

5. Sur la page de boîte de dialogue Azure Active Directory, procédez comme suit :
   
    ![Paramètres OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. Dans la zone de texte **Point de terminaison SAML 2.0**, collez la valeur de l’**URL de connexion** que vous avez copiée à partir du Portail Azure.
    
    b. Dans la zone de texte **URL des métadonnées**, collez la valeur **URL des métadonnées de fédération de l’application** que vous avez copiée sur le portail Azure.
    
    c. Cliquez sur **Enregistrer les modifications**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD 

L’objectif de cette section est de créer un utilisateur de test appelé B. Simon dans le Portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **B. Simon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez **bsimon@yourcompanydomain.extension**  
    Par exemple, BSimon@contoso.com

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser B. Simon à utiliser l’authentification unique Azure en lui accordant l’accès à OpsGenie.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **OpsGenie**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **OpsGenie**.

    ![Lien OpsGenie dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-opsgenie-test-user"></a>Créer un utilisateur de test OpsGenie

L’objectif de cette section est de créer un utilisateur appelé B. Simon dans OpsGenie. 

1. Dans une fenêtre de navigateur web, connectez-vous à votre client OpsGenie en tant qu’administrateur.

2. Accédez à la liste Utilisateurs en cliquant sur **Utilisateurs** dans le volet gauche.
   
    ![Paramètres OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. Cliquez sur **Add User**.

4. Dans la boîte de dialogue **Ajouter un utilisateur**, procédez comme suit :
   
    ![Paramètres OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
    a. Dans la zone de texte **E-mail**, saisissez l’adresse e-mail de B. Simon utilisée dans Azure Active Directory.
   
    b. Dans la zone de texte **Nom complet**, tapez **B. Simon**.
   
    c. Cliquez sur **Enregistrer**. 

>[!NOTE]
>B. Simon reçoit un e-mail contenant des instructions pour configurer son profil.

### <a name="test-single-sign-on"></a>Tester l’authentification unique 

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette OpsGenie dans le volet d’accès, vous devez être connecté automatiquement à l’application OpsGenie pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

