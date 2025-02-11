---
title: 'Didacticiel : Intégration d’Azure Active Directory à Autotask Workplace | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Autotask Workplace.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00e838dd3df417bd0b26167d5f316225520e7263
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "65859029"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Didacticiel : Intégration d’Azure Active Directory à Autotask Workplace

Dans ce didacticiel, vous allez apprendre à intégrer Autotask Workplace à Azure Active Directory (Azure AD).
L’intégration d’Autotask Workplace à Azure AD vous offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à Autotask Workplace.
* Vous pouvez permettre aux utilisateurs de se connecter automatiquement à Autotask Workplace (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Autotask Workplace, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Un abonnement Autotask Workplace pour lequel l’authentification unique est activée
* Un abonnement Autotask Workplace pour lequel l’authentification unique est activée
* Vous devez être un administrateur ou un super administrateur dans Workplace.
* Vous devez disposer d’un compte d’administrateur dans Azure AD.
* Les utilisateurs qui font appel à cette fonctionnalité doivent disposer de comptes dans Workplace et Azure AD, et l’adresse de messagerie associée à ces deux comptes doit être la même.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Autotask Workplace prend en charge l’authentification unique lancée par le **fournisseur de services et le fournisseur d’identité**

## <a name="adding-autotask-workplace-from-the-gallery"></a>Ajout d’Autotask Workplace à partir de la galerie

Pour configurer l’intégration d’Autotask Workplace à Azure AD, vous devez ajouter Autotask Workplace à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Autotask Workplace à partir de la galerie, effectuez les étapes suivantes :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **Autotask Workplace**, sélectionnez **Autotask Workplace** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![Autotask Workplace dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD auprès d’Autotask Workplace, sur un utilisateur de test nommé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur Autotask Workplace associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Autotask Workplace, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique Autotask Workplace](#configure-autotask-workplace-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test Autotask Workplace](#create-autotask-workplace-test-user)** pour avoir un équivalent de Britta Simon dans Autotask Workplace, associé à sa représentation dans Azure AD.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD auprès d’Autotask Workplace, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **Autotask Workplace**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. À la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode initié par **IDP**, suivez les étapes ci-dessous :

    ![Informations d’authentification unique dans Domaine et URL Autotask Workplace](common/idp-intiated.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. Dans la zone de texte **URL de réponse**, tapez une URL au format suivant : `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

5. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL Autotask Workplace](common/metadata-upload-additional-signon.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<subdomain>.awp.autotask.net/loginsso`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur, l’URL de réponse et l’URL de connexion réels. Pour obtenir ces valeurs, contactez [l’équipe du support Autotask Workplace](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

6. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies en fonction de vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

7. Dans la section **Configurer Autotask Workplace**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-autotask-workplace-single-sign-on"></a>Configurer l’authentification unique Autotask Workplace

1. Dans une autre fenêtre de navigateur web, connectez-vous à Workplace Online à l’aide des informations d’identification administrateur.

    > [!Note]
    > Lorsque vous configurez le fournisseur d’identité, vous devez indiquer un sous-domaine. Pour confirmer le sous-domaine, connectez-vous à Workplace Online. Une fois connecté, notez le sous-domaine dans l’URL. Le sous-domaine est la partie entre le « https:// » et «.awp.autotask.net/ » et doit être us, eu, ca ou au.

2. Accédez à **Configuration** > **Authentification unique** et procédez comme suit :

    ![Configuration de l’authentification unique Autotask](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)

    a. Sélectionnez l’option **Fichier XML de métadonnées** et chargez le fichier **XML de métadonnées de fédération** téléchargé à partir du portail Azure.

    b. Cliquez sur **ACTIVER SSO**.

    ![Configuration de l’approbation d’authentification unique Autotask](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Cochez la case **I confirm this information is correct and I trust this IdP** (Je confirme l’exactitude de ces informations et je fais confiance à ce fournisseur d’identité).

    d. Cliquez sur **APPROUVER**.

> [!Note]
> Si vous avez besoin d’aide lors de la configuration d’Autotask Workplace, consultez [cette page](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) pour obtenir de l’aide avec votre compte Workplace.

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

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Autotask Workplace.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis sélectionnez **Autotask Workplace**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Autotask Workplace**.

    ![Lien Autotask Workplace dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-autotask-workplace-test-user"></a>Créer un utilisateur de test Autotask Workplace

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Autotask Workplace. Pour ajouter des utilisateurs dans la plateforme Autotask Workplace, contactez [l’équipe du support Autotask Workplace](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm).

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette Autotask Workplace dans le volet d’accès, vous devez vous connecter automatiquement à l’application Autotask Workplace pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
