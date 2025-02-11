---
title: 'Didacticiel : Intégration d’Azure Active Directory à OrgChart Now | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et OrgChart Now.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 50a1522f-81de-4d14-9b6b-dd27bb1338a4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: cc2bbd0c1220a37de640bde6294eb096b25e5398
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65870499"
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Didacticiel : Intégration d’Azure Active Directory à OrgChart Now

Dans ce didacticiel, vous allez apprendre à intégrer OrgChart Now dans Azure Active Directory (Azure AD).
L’intégration d’OrgChart Now dans Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à OrgChart Now.
* Vous pouvez permettre aux utilisateurs de se connecter automatiquement à OrgChart Now (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD dans OrgChart Now, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Un abonnement OrgChart Now pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* OrgChart Now prend en charge l’authentification unique lancée par **le fournisseur de services** et le **fournisseur d’identité**

## <a name="adding-orgchart-now-from-the-gallery"></a>Ajout d’OrgChart Now à partir de la galerie

Pour configurer l’intégration d’OrgChart Now dans Azure AD, vous devez ajouter OrgChart Now à partir de la galerie dans votre liste d’applications SaaS gérées.

**Pour ajouter OrgChart Now à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **OrgChart Now**, sélectionnez **OrgChart Now** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![OrgChart Now dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD auprès d’OrgChart Now avec un utilisateur de test qui s’appelle **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre l’utilisateur Azure AD et l’utilisateur OrgChart Now associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec OrgChart Now, vous devez suivre les étapes ci-dessous :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique OrgChart Now](#configure-orgchart-now-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test OrgChart Now](#create-orgchart-now-test-user)** pour avoir un équivalent de Britta Simon dans OrgChart Now, lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD auprès d’OrgChart Now, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **OrgChart Now**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode lancé par le **fournisseur d’identité**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL OrgChart Now](common/idp-identifier.png)

    Dans la zone de texte **Identificateur**, tapez une URL :  `https://sso2.orgchartnow.com`

5. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

    ![image](common/both-preintegrated-signon.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`

    > [!NOTE]
    > `<YourEntityID>` est l’**identificateur Azure AD** copié à partir de la section **Configurer OrgChart Now**, décrite plus loin dans le tutoriel.

6. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

7. Dans la section **Configurer OrgChart Now**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-orgchart-now-single-sign-on"></a>Configurer l’authentification unique OrgChart Now

Pour configurer l’authentification unique côté **OrgChart Now**, vous devez envoyer le **XML des métadonnées de fédération** téléchargé et les URL copiées dans le portail Azure à l’[équipe du support technique OrgChart Now](mailto:ocnsupport@officeworksoftware.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD 

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **BrittaSimon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez **brittasimon@yourcompanydomain.extension**  
    Par exemple, BrittaSimon@contoso.com

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à OrgChart Now.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **OrgChart Now**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **OrgChart Now**.

    ![Lien OrgChart Now dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-orgchart-now-test-user"></a>Créer un utilisateur de test OrgChart Now

Pour permettre aux utilisateurs Azure AD de se connecter à OrgChart Now, vous devez les approvisionner dans OrgChart Now. 

1. OrgChart Now prend en charge l’approvisionnement juste-à-temps (option activée par défaut). Un utilisateur est créé lors d’une tentative d’accès à OrgChart Now (s’il n’existe pas déjà). La fonctionnalité d’approvisionnement d’utilisateur juste-à-temps crée un utilisateur **en lecture seule** lorsqu’une requête d’authentification unique provient d’un IDP reconnu et que l’adresse électronique dans l’assertion SAML ne se trouve pas dans la liste des utilisateurs. Pour cette fonctionnalité d’approvisionnement automatique, vous devez créer un groupe d’accès intitulé **Général** dans OrgChart Now. Suivez la procédure de création d’un groupe d’accès :

    a. Accédez à l’option **Gérer les groupes** après avoir cliqué sur l’**engrenage** dans l’angle supérieur droit de l’interface utilisateur.

    ![Groupes d’OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_manage.png)    

    b. Sélectionnez l’icône **Ajouter** et attribuez au groupe le nom **Général**, puis cliquez sur **OK**. 

    ![Ajout dans OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Sélectionnez le ou les dossiers auxquels les utilisateurs généraux ou en lecture seule doivent avoir accès :

    ![Dossiers d’OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. **Verrouillez** les dossiers afin que seuls les utilisateurs administrateurs puissent les modifier. Ensuite, appuyez sur **OK**.

    ![Verrouillage d’OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

2. Pour créer des utilisateurs **Admin** et des utilisateurs **en lecture/écriture**, vous devez créer manuellement un utilisateur afin d’obtenir l’accès à son niveau de privilège via l’authentification unique. Pour approvisionner un compte d’utilisateur, procédez comme suit :

    a. Connectez-vous à OrgChart Now en tant qu’administrateur de la sécurité.

    b.  Cliquez sur **Paramètres** sur l’angle supérieur droit, puis accédez à **Gérer les utilisateurs**.

    ![Paramètres d’OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Cliquez sur **Ajouter** et effectuez les opérations suivantes :

    ![Gestion d’OrgChart Now](./media/orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * Dans la zone de texte **ID utilisateur**, entrez l’ID d’utilisateur, par exemple **brittasimon\@contoso.com**.

    * Dans la zone de texte **Adresse e-mail**, tapez l’adresse e-mail de l’utilisateur, par exemple **brittasimon\@contoso.com**.

    * Cliquez sur **Add**.

### <a name="test-single-sign-on"></a>Tester l’authentification unique 

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette OrgChart Now dans le volet d’accès, vous devez être connecté automatiquement à l’application OrgChart Now pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

