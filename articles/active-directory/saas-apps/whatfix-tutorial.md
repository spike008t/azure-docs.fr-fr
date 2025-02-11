---
title: "Didacticiel : Intégration d'Azure Active Directory à Whatfix | Microsoft Docs"
description: Apprenez à configurer l'authentification unique entre Azure Active Directory et Whatfix.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: f4272f1e-7639-4bdd-9a86-e7208099da6c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9fa088aa331621409103997755b2344f2dd70c64
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65992942"
---
# <a name="tutorial-integrate-whatfix-with-azure-active-directory"></a>Didacticiel : Intégrer Whatfix à Azure Active Directory

Dans ce didacticiel, vous allez apprendre à intégrer Whatfix à Azure Active Directory (Azure AD). Lorsque vous intégrez Whatfix à Azure AD, vous pouvez :

* contrôler qui a accès à Whatfix ;
* permettre à vos utilisateurs de se connecter automatiquement à Whatfix avec leur compte Azure AD ;
* gérer vos comptes à un emplacement central : le portail Azure.

Pour en savoir plus sur l'intégration des applications SaaS à Azure AD, consultez [Qu'est-ce que l'accès aux applications et l'authentification unique auprès d'Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous n'en avez pas, vous pouvez bénéficier d'un essai gratuit d'un mois en cliquant [ici](https://azure.microsoft.com/pricing/free-trial/).
* Un abonnement Whatfix pour lequel l'authentification unique (SSO) est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous allez configurer et tester l'authentification unique Azure AD dans un environnement de test. Whatfix prend en charge l'authentification unique initiée par le **fournisseur de services et le fournisseur d'identité**

## <a name="adding-whatfix-from-the-gallery"></a>Ajout de Whatfix à partir de la galerie

Pour configurer l'intégration de Whatfix à Azure AD, vous devez ajouter Whatfix à votre liste d'applications SaaS managées à partir de la galerie.

1. Connectez-vous au [portail Azure](https://portal.azure.com) avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d'entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, entrez **Whatfix**  dans la zone de recherche.
1. Sélectionnez **Whatfix**  dans le volet de résultats, puis ajoutez l'application. Patientez quelques secondes, jusqu'à ce que l'application soit ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Configurez et testez l'authentification unique (SSO) Azure AD auprès de Whatfix à l'aide d'un utilisateur de test nommé **Britta Simon**. Pour que l'authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l'utilisateur Whatfix associé.

Pour configurer et tester l'authentification unique (SSO) Azure AD auprès de Whatfix, suivez les indications des sections ci-après :

1. **[Configurer l'authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d'utiliser cette fonctionnalité.
2. **[Configurer l'authentification unique Whatfix](#configure-whatfix-sso)** pour configurer les paramètres de l'authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test Whatfix](#create-whatfix-test-user)** pour avoir dans Whatfix un équivalent de Britta Simon lié à la représentation Azure AD associée.
6. **[Tester l'authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.


### <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Procédez comme suit pour activer l'authentification unique Azure AD sur le portail Azure.

1. Sur le [portail Azure](https://portal.azure.com/), accédez à la page d'intégration de l'application **Whatfix**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Sur la page **Sélectionner une méthode d'authentification unique**, sélectionnez **SAML**.
1. Sur la page **Configurer l'authentification unique avec SAML**, cliquez sur l'icône de modification/stylo de la **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, si vous souhaitez configurer l'application en mode Initié par le **fournisseur d'identité**, entrez les valeurs pour le champ suivant :

    1. Cliquez sur **Définir des URL supplémentaires**.
    1. Dans la zone de texte **État de relais**, entrez l'URL correspondante spécifiée par le client.
    
    > [!NOTE]
    > Contactez l'[équipe du support technique Whatfix ](https://support.whatfix.com) pour obtenir la valeur de l'URL État de relais.

1. Cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes si vous souhaitez configurer l’application en mode lancé par le **fournisseur de services** :

    Dans la zone de texte **URL de connexion**, tapez l’URL : `https://whatfix.com`

1. Sur la page **Configurer l'authentification unique avec SAML**, accédez à la section **Certificat de signature SAML** et cliquez sur le bouton Copier pour copier l'**URL des métadonnées de fédération d'application**.

    ![Lien Téléchargement de certificat](common/copy-metadataurl.png)

### <a name="configure-whatfix-sso"></a>Configurer l'authentification unique Whatfix

Pour configurer l'authentification unique côté **Whatfix**, vous devez envoyer l'**URL des métadonnées de fédération de l'application** à l'[équipe du support technique Whatfix](https://support.whatfix.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

Dans cette section, vous allez créer un utilisateur de test appelé Britta Simon sur le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, **Utilisateurs**, puis **Tous les utilisateurs**.
1. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.
1. Dans les propriétés **Utilisateur**, procédez comme suit :
   1. Dans le champ **Nom**, entrez `Britta Simon`.  
   1. Dans le champ **Nom de l'utilisateur**, entrez username@companydomain.extension. Par exemple : `BrittaSimon@contoso.com`.
   1. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.
   1. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l'authentification unique Azure en lui accordant l'accès à Whatfix.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Whatfix**.
1. Sur la page de vue d'ensemble de l'application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.

   ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Lien Ajouter un utilisateur](common/add-assign-user.png)

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** à partir de la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l'écran.
1. Si vous attendez une valeur de rôle dans l'assertion SAML, accédez à la boîte de dialogue **Sélectionner un rôle**, puis sélectionnez le rôle approprié pour l'utilisateur dans la liste et cliquez sur le bouton **Sélectionner** en bas de l'écran.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-whatfix-test-user"></a>Créer un utilisateur de test Whatfix

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Whatfix. Collaborez avec l' [équipe du support technique Whatfix](https://support.whatfix.com) pour ajouter des utilisateurs sur la plateforme Whatfix. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

### <a name="test-sso"></a>Tester l’authentification unique (SSO)

Le fait de cliquer sur la vignette Whatfix dans le volet d'accès doit vous connecter automatiquement à l'application Whatfix pour laquelle vous avez configuré l'authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de tutoriels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
