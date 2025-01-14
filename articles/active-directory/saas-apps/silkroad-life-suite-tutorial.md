---
title: 'Didacticiel : Intégration d’Azure Active Directory à SilkRoad Life Suite | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et SilkRoad Life Suite.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: bcad6232de7fa257b58fe6d84f2c2ff794b64589
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65902297"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Didacticiel : Intégration d’Azure Active Directory à SilkRoad Life Suite

Dans ce didacticiel, vous allez apprendre à intégrer SilkRoad Life Suite à Azure Active Directory (Azure AD).
L’intégration de SilkRoad Life Suite à Azure AD offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à SilkRoad Life Suite.
* Vous pouvez permettre à vos utilisateurs de se connecter automatiquement à SilkRoad Life Suite (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec SilkRoad Life Suite, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/)
* Un abonnement SilkRoad Life Suite pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* SilkRoad Life Suite prend en charge l’authentification unique initiée par le **fournisseur de services**

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Ajout de SilkRoad Life Suite à partir de la galerie

Pour configurer l’intégration de SilkRoad Life Suite dans Azure AD, vous devez ajouter SilkRoad Life Suite, disponible dans la galerie, à votre liste d’applications SaaS gérées.

**Pour ajouter SilkRoad Life Suite à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **SilkRoad Life Suite**, sélectionnez **SilkRoad Life Suite** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![SilkRoad Life Suite dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec SilkRoad Life Suite, avec un utilisateur de test appelé **Britta Simon**.
Pour que l’authentification unique fonctionne, vous devez établir une relation entre l’utilisateur Azure AD et l’utilisateur SilkRoad Life Suite associé.

Pour configurer et tester l’authentification unique Azure AD avec SilkRoad Life Suite, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique SilkRoad Life Suite](#configure-silkroad-life-suite-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test SilkRoad Life Suite](#create-silkroad-life-suite-test-user)** pour avoir un équivalent de Britta Simon dans SilkRoad Life Suite qui soit lié à la représentation de l’utilisateur dans Azure AD.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec SilkRoad Life Suite, effectuez les étapes suivantes :

1. Sur le [portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **SilkRoad Life Suite**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, si vous disposez d’un **fichier de métadonnées du fournisseur de services**, suivez les étapes ci-dessous :

    > [!NOTE]
    > Le **fichier de métadonnées du fournisseur de services** est expliqué plus loin dans ce tutoriel.

    a. Cliquez sur **Charger un fichier de métadonnées**.

    ![image](common/upload-metadata.png)

    b. Cliquez sur le **logo du dossier** pour sélectionner le fichier de métadonnées, puis cliquez sur **Charger**.

    ![image](common/browse-upload-metadata.png)

    c. Une fois le fichier de métadonnées chargé, les valeurs **Identificateur** et **URL de réponse** sont automatiquement renseignées dans la section Configuration SAML de base :

    ![image](common/sp-identifier-reply.png)

    > [!Note]
    > Si les valeurs **Identificateur** et **URL de réponse** ne sont pas automatiquement renseignées, renseignez-les manuellement en fonction de vos besoins.

    d. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<subdomain>.silkroad-eng.com/Authentication/`.

5. Dans la section **Configuration SAML de base**, si vous ne disposez pas d’un **fichier de métadonnées du fournisseur de services**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL SilkRoad Life Suite](common/sp-identifier-reply.png)

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<subdomain>.silkroad-eng.com/Authentication/`.

    b. Dans la zone de texte **Identificateur**, tapez une URL au format suivant :

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP`|
    | `https://<subdomain>.silkroad.com/Authentication/SP`|

    c. Dans la zone de texte **URL de réponse**, tapez une URL au format suivant :

    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/`|
    | `https://<subdomain>.silkroad.com/Authentication/`|

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion, l’identificateur et l’URL de réponse réels. Pour obtenir ces valeurs, contactez [l’équipe de support technique SilkRoad Life Suite](https://www.silkroad.com/locations/). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

6. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies en fonction de vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

7. Dans la section **Configurer SilkRoad Life Suite**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-silkroad-life-suite-single-sign-on"></a>Configurer l’authentification unique SilkRoad Life Suite

1. Connectez-vous à votre site d’entreprise SilkRoad en tant qu’administrateur.

    > [!NOTE]
    > Pour obtenir l’accès à l’application d’authentification SilkRoad Life Suite pour configurer la fédération avec Microsoft Azure AD, contactez le support technique SilkRoad ou votre représentant SilkRoad Services.

1. Accédez à **Service Provider** (Fournisseur de services), puis cliquez sur **Federation Details** (Informations sur la fédération).

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_06.png)

1. Cliquez sur **Download Federation Metadata**(Télécharger les métadonnées de fédération), puis enregistrez le fichier de métadonnées sur votre ordinateur. Dans la section **Configuration SAML de base** du portail Azure, utilisez les métadonnées de fédération téléchargées pour le **fichier de métadonnées de fournisseur de services**.

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_07.png)

1. Dans votre application **SilkRoad**, cliquez sur **Authentication Sources** (Sources d’authentification).

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_08.png) 

1. Cliquez sur **Add Authentication Source**(Ajouter une source d’authentification).

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_09.png)

1. Dans la section **Add Authentication Source** (Ajouter une source d’authentification), procédez comme suit :

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_10.png)
  
    a. Sous **Option 2 - Metadata File** (Option 2 - Fichier de métadonnées), cliquez sur **Browse** (Parcourir) pour charger le fichier de métadonnées que vous avez téléchargé à partir du portail Azure.
  
    b. Cliquez sur **Create Identity Provider using File Data**.

1. Dans la section **Authentication Sources** (Sources d’authentification), cliquez sur **Edit** (Modifier).

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_11.png)

1. Dans la boîte de dialogue **Edit Authentication Source** (Modifier la source d’authentification), procédez comme suit :

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_12.png)

    a. Pour l’option **Enabled**, sélectionnez **Yes**.

    b. Dans la zone de texte **EntityId**, collez la valeur de l’**identificateur Azure AD** que vous avez copiée dans le portail Azure.

    c. Dans la zone de texte **IdP Description** (Description IdP), entrez une description de votre configuration (par exemple : *Azure AD SSO*).

    d. Dans la zone de texte **Metadata File** (Fichier de métadonnées), chargez le fichier de **métadonnées** que vous avez téléchargé à partir du portail Azure.
  
    e. Dans la zone de texte **IdP Name** (Nom IdP), entrez un nom spécifique de votre configuration (par exemple : *Azure SP*).
  
    f. Dans la zone de texte **Logout Service URL** (URL de service de déconnexion), collez l’**URL de déconnexion** que vous avez copiée dans le portail Azure.

    g. Dans la zone de texte **Sign-on service URL** (URL du service de connexion), collez l’**URL de connexion** que vous avez copiée à partir du portail Azure.

    h. Cliquez sur **Enregistrer**.

1. Désactivez toutes les autres sources d’authentification.

    ![Authentification unique Azure AD](./media/silkroad-life-suite-tutorial/tutorial_silkroad_13.png)

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

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à SilkRoad Life Suite.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **SilkRoad Life Suite**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **SilkRoad Life Suite**.

    ![Lien SilkRoad Life Suite dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-silkroad-life-suite-test-user"></a>Créer un utilisateur de test SilkRoad Life Suite

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans SilkRoad Life Suite. Collaborez avec [l’équipe de support technique SilkRoad Life Suite](https://www.silkroad.com/locations/) pour ajouter des utilisateurs dans la plateforme SilkRoad Life Suite. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette SilkRoad Life Suite dans le volet d’accès, vous êtes connecté automatiquement à l’application SilkRoad Life Suite pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
