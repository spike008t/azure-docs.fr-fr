---
title: 'Didacticiel : Intégration d’Azure Active Directory à Dome9 Arc | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Dome9 Arc.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/31/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 86112c6d1c720787af80a9846b5c94ec59895ecb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65862149"
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Didacticiel : Intégration d’Azure Active Directory à Dome9 Arc

Dans ce didacticiel, vous allez apprendre à intégrer Dome9 Arc à Azure Active Directory (Azure AD).
L’intégration de Dome9 Arc à Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à Dome9 Arc.
* Vous pouvez permettre aux utilisateurs de se connecter automatiquement à Dome9 (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Dome9 Arc, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Un abonnement Dome9 Arc pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Dome9 prend en charge l’authentification unique lancée par le **fournisseur de services** et le **fournisseur d’identité**

## <a name="adding-dome9-arc-from-the-gallery"></a>Ajout de Dome9 Arc à partir de la galerie

Pour configurer l’intégration de Dome9 Arc à Azure AD, vous devez ajouter Dome9 Arc depuis la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Dome9 Arc à partir de la galerie, effectuez les étapes suivantes :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **Dome9 Arc**, sélectionnez **Dome9 Arc** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![Dome9 Arc dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Dome9 Arc avec un utilisateur de test appelé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur Dome9 Arc associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Dome9 Arc, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique Dome9 Arc ](#configure-dome9-arc-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer utilisateur de test Dome9 Arc](#create-dome9-arc-test-user)** pour avoir un équivalent de Britta Simon dans Dome9 Arc lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec Dome9 Arc, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **Dome9 Arc**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. À la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode initié par **IDP**, suivez les étapes ci-dessous :

    ![Informations d’authentification unique dans Dome9 Arc Domain and URLs (Domaine et URL Dome9 Arc)](common/idp-intiated.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://secure.dome9.com/`

    b. Dans la zone de texte **URL de réponse**, tapez une URL au format suivant : `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Vous allez sélectionner le nom de votre société dans le portail d’administration dome9, comme expliqué plus loin dans le didacticiel.

5. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Dome9 Arc Domain and URLs (Domaine et URL Dome9 Arc)](common/metadata-upload-additional-signon.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de réponse et l’URL de connexion réelles. Pour obtenir ces valeurs, contactez [l’équipe de support technique de Dome9 Arc](mailto:support@dome9.com). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

6. L’application Dome9 Arc attend les assertions SAML dans un format spécifique. Configurez les revendications suivantes pour cette application. Vous pouvez gérer les valeurs de ces attributs à partir de la section **Attributs utilisateur** sur la page d’intégration des applications. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur le bouton **Modifier** pour ouvrir la boîte de dialogue **Attributs utilisateur**.

    ![image](common/edit-attribute.png)

7. Dans la section **Revendications des utilisateurs** de la boîte de dialogue **Attributs utilisateur**, modifiez les revendications en utilisant l’icône **Modifier** ou ajoutez des revendications en utilisant l’option **Ajouter une nouvelle revendication** pour configurer l’attribut de jeton SAML comme sur l’image ci-dessus et procédez comme suit : 

    | Nom |  Attribut source|
    | ---------------| --------------- |
    | memberof | user.assignedroles |

    a. Cliquez sur le bouton **Ajouter une nouvelle revendication** pour ouvrir la boîte de dialogue **Gérer les revendications des utilisateurs**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Dans la zone de texte **Attribut**, indiquez le nom d’attribut pour cette ligne.

    c. Laissez le champ **Espace de noms** vide.

    d. Sélectionnez Source comme **Attribut**.

    e. Dans la liste **Attribut de la source**, tapez la valeur d’attribut indiquée pour cette ligne.

    f. Cliquez sur **OK**.

    g. Cliquez sur **Enregistrer**.

8. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le **Certificat (Base64)** en fonction des options définies par rapport à vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/certificatebase64.png)

9. Dans la section **Configurer Dome9 Arc**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-dome9-arc-single-sign-on"></a>Configurer l’authentification unique Dome9 Arc

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre site d’entreprise Dome9 Arc en tant qu’administrateur.

2. Cliquez sur les **paramètres du profil** dans l’angle supérieur droit puis cliquez sur les **paramètres du compte**. 

    ![Configuration de Dome9 Arc](./media/dome9arc-tutorial/configure1.png)

3. Accédez à **SSO**, puis cliquez sur **ACTIVER**.

    ![Configuration de Dome9 Arc](./media/dome9arc-tutorial/configure2.png)

4. Dans la section de la configuration de l’authentification unique, effectuez les étapes suivantes :

    ![Configuration de Dome9 Arc](./media/dome9arc-tutorial/configure3.png)

    a. Entrez le nom de la société dans la zone de texte **Account ID** (ID de compte). Cette valeur sera utilisée dans l’URL de réponse mentionnée dans la section de l’URL du portail Azure.

    b. Dans la zone de texte **Issuer** (Émetteur), collez la valeur **Identificateur Azure AD** que vous avez copiée à partir du portail Azure.

    c. Dans la zone de texte **Idp endpoint url** (URL du point de terminaison IDP), collez la valeur **URL de connexion** que vous avez copiée dans le portail Azure.

    d. Ouvrez le certificat codé en base64 que vous avez téléchargé dans le Bloc-notes, copiez son contenu dans le Presse-papiers, puis collez-le dans la zone de texte **X.509 certificate** (Certificat X.509).

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
  
    b. Dans le champ **Nom d’utilisateur**, tapez **brittasimon\@domainedevotreentreprise.extension**.  
    Par exemple, BrittaSimon@contoso.com

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Dome9 Arc.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **Dome9 Arc**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Dome9 Arc**.

    ![Lien Dome9 Arc dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-dome9-arc-test-user"></a>Créer un utilisateur de test Dome9 Arc

Pour permettre aux utilisateurs Azure AD de se connecter à Dome9 Arc, vous devez les provisionner dans Dome9 Arc. Dome9 Arc prend en charge le provisionnement juste-à-temps, mais pour que cela fonctionne correctement, l’utilisateur doit sélectionner un **rôle** particulier et se l’assigner.

   >[!Note]
   >Pour la création de **rôle** et autres détails, contactez [l’équipe de support client de Dome9 Arc](https://dome9.com/about/contact-us/).

**Pour provisionner un compte d’utilisateur manuellement, effectuez les étapes suivantes :**

1. Connectez-vous au site d’entreprise Dome9 Arc en tant qu’administrateur.

2. Cliquez sur **Users & Roles** (Utilisateurs et rôles), puis cliquez sur **Users** (utilisateurs).

    ![Ajouter un employé](./media/dome9arc-tutorial/user1.png)

3. Cliquez sur**ADD USER** (Ajouter un utilisateur).

    ![Ajouter un employé](./media/dome9arc-tutorial/user2.png)

4. Dans la section **Create User** , procédez comme suit :

    ![Ajouter un employé](./media/dome9arc-tutorial/user3.png)

    a. Dans la zone de texte **Email**, entrez l’adresse e-mail de l’utilisateur, par exemple, Brittasimon@contoso.com.

    b. Dans la zone de texte **First Name**, entrez le prénom de l’utilisateur, par exemple Britta.

    c. Dans la zone de texte **Last Name**, entrez le nom de l’utilisateur, par exemple Simon.

    d. Définissez **SSO User** (SSO utilisateur) sur **On** (Activé).

    e. Cliquez sur **CREATE** (Créer).

### <a name="test-single-sign-on"></a>Tester l’authentification unique 

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette Dome9 Arc dans le volet d’accès, vous devez être connecté automatiquement à l’application Dome9 Arc pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

