---
title: "Didacticiel : Intégration d'Azure Active Directory à Salesforce Sandbox | Microsoft Docs"
description: Découvrez comment configurer une authentification unique entre Azure Active Directory et Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/17/2019
ms.author: jeedes
ms.openlocfilehash: b2940f3eee3112fe1c6d57cc92157c573ecad109
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65904278"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Didacticiel : Intégration d'Azure Active Directory à Salesforce Sandbox

Dans ce didacticiel, vous allez apprendre à intégrer Salesforce Sandbox avec Azure Active Directory (Azure AD).

Les bacs à sable (sandbox) vous permettent de créer plusieurs copies de votre organisation dans des environnements distincts à des fins diverses, notamment le développement, le test et la formation, sans compromettre les données ou les applications de votre organisation de production Salesforce.
Pour plus d’informations, consultez la page de [présentation Salesforce des bacs à sable](https://help.salesforce.com/articleView?id=create_test_instance.htm&language=en_us&type=5).

L’intégration de Salesforce Sandbox avec Azure AD vous offre les avantages suivants :

* Vous pouvez contrôler dans Azure AD qui a accès à Salesforce Sandbox.
* Vous pouvez permettre à vos utilisateurs de se connecter automatiquement à Salesforce Sandbox (par le biais de l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec Salesforce Sandbox, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/)
* Abonnement Salesforce Sandbox pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Salesforce Sandbox prend en charge l’authentification unique initiée par le **fournisseur de services et le fournisseur d’identité**
* Salesforce Sandbox prend en charge l’attribution d’utilisateurs **juste-à-temps**
* Salesforce Sandbox prend en charge l’attribution d’utilisateurs [**automatique**](salesforce-sandbox-provisioning-tutorial.md)

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Ajout de Salesforce Sandbox à partir de la galerie

Pour configurer l’intégration de Salesforce Sandbox avec Azure AD, vous devez ajouter Salesforce Sandbox à partir de la galerie à votre liste d’applications SaaS managées.

**Pour ajouter Salesforce Sandbox à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **Salesforce Sandbox**, sélectionnez **Salesforce Sandbox** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Salesforce Sandbox dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Salesforce Sandbox sur un utilisateur de test nommé **Britta Simon**.

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Salesforce Sandbox équivalent à l’utilisateur dans Azure AD. En d’autres termes, une relation entre l’utilisateur Azure AD et l’utilisateur Salesforce Sandbox associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Salesforce Sandbox, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique Salesforce Sandbox](#configure-salesforce-sandbox-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test Salesforce Sandbox](#create-salesforce-sandbox-test-user)** pour avoir un équivalent de Britta Simon dans Salesforce Sandbox lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec Salesforce Sandbox, procédez comme suit :

1. Dans le [Portail Azure](https://portal.azure.com/), sur la page d’intégration de l’application **Salesforce Sandbox**, cliquez sur **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, si vous avez un **fichier de métadonnées de fournisseur de services** et voulez une configuration en mode initié par le **fournisseur d’identité** (IDP), effectuez les étapes suivantes :

    a. Cliquez sur **Charger un fichier de métadonnées**.

    ![Charger le fichier de métadonnées](common/upload-metadata.png)

    b. Cliquez sur le **logo du dossier** pour sélectionner le fichier de métadonnées, puis cliquez sur **Charger**.

    ![choisir le fichier de métadonnées](common/browse-upload-metadata.png)

    > [!NOTE]
    > Vous obtenez le fichier de métadonnées du fournisseur de services à partir du portail d’administration Salesforce Sandbox, décrit plus loin dans le tutoriel.

    c. Une fois que le fichier de métadonnées est correctement chargé, la valeur **URL de réponse** est renseignée automatiquement dans la zone de texte **URL de réponse**.

    ![image](common/both-replyurl.png)

    > [!Note]
    > Si la valeur **URL de réponse** n’est pas automatiquement renseignée, renseignez-la manuellement en fonction de vos besoins.

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML des métadonnées** en fonction des options définies, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer Salesforce Sandbox**, copiez la ou les URL appropriées, selon vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-salesforce-sandbox-single-sign-on"></a>Configurer l’authentification unique Salesforce Sandbox

1. Ouvrez un nouvel onglet dans votre navigateur et connectez-vous à votre compte d’administrateur Salesforce Sandbox.

2. Cliquez sur **Setup** sous **l’icône de paramètres** en haut à droite de la page.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/configure1.png)

3. Dans le volet de navigation de gauche, accédez à **SETTINGS** (PARAMÈTRES), puis cliquez sur **Identity** (Identité) pour développer la section associée. Puis cliquez sur **Paramètres de l’authentification unique**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

4. Sur la page **Paramètres de l’authentification unique**, cliquez sur le bouton **Modifier**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/configure3.png)

5. Sélectionnez **SAML activé**, puis cliquez sur **Enregistrer**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

6. Pour configurer vos paramètres d’authentification unique SAML, cliquez sur **Nouveau à partir d’un fichier de métadonnées**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

7. Cliquez sur **Sélectionner le fichier** pour charger le fichier XML de métadonnées que vous avez téléchargé à partir du Portail Azure, puis cliquez sur **Créer**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/xmlchoose.png)

8. Sur la page **Paramètres d’authentification unique SAML**, les champs se renseignent automatiquement et cliquez sur Enregistrer.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/salesforcexml.png)

9. Dans la page **Paramètres de l’authentification unique**, cliquez sur le bouton **Télécharger les métadonnées** pour télécharger le fichier de métadonnées du fournisseur de services. Utilisez ce fichier dans la section **Configuration SAML de base** dans le portail Azure pour configurer les URL nécessaires comme expliqué ci-dessus.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/configure4.png)

10. Si vous souhaitez configurer l’application en mode initié par le **fournisseur de service**, vous devez respecter les prérequis suivants :

    a. Vous devez disposer d’un domaine vérifié.

    b. Vous devez configurer et activer votre domaine sur Salesforce Sandbox. La procédure à suivre est décrite dans la suite de ce didacticiel.

    c. Dans le portail Azure, dans la section **Configuration SAML de base**, cliquez sur **Définir des URL supplémentaires** et effectuez l’étape suivante :
  
    ![Informations d’authentification unique dans Domaine et URL Salesforce Sandbox](common/both-signonurl.png)

    Dans la zone de texte **URL de connexion**, tapez la valeur au format suivant : `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Cette valeur doit être copiée à partir du portail Salesforce Sandbox une fois que vous avez activé le domaine.

11. Dans la section **Certificat de signature SAML**, cliquez sur **XML de métadonnées de fédération**, puis enregistrez le fichier xml sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

12. Ouvrez un nouvel onglet dans votre navigateur et connectez-vous à votre compte d’administrateur Salesforce Sandbox.

13. Cliquez sur **Setup** sous **l’icône de paramètres** en haut à droite de la page.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/configure1.png)

14. Dans le volet de navigation de gauche, accédez à **SETTINGS** (PARAMÈTRES), puis cliquez sur **Identity** (Identité) pour développer la section associée. Puis cliquez sur **Paramètres de l’authentification unique**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

15. Sur la page **Paramètres de l’authentification unique**, cliquez sur le bouton **Modifier**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/configure3.png)

16. Sélectionnez **SAML activé**, puis cliquez sur **Enregistrer**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

17. Pour configurer vos paramètres d’authentification unique SAML, cliquez sur **Nouveau à partir d’un fichier de métadonnées**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

18. Cliquez sur **Choisir un fichier** pour charger le fichier XML de métadonnées, puis sur **Créer**.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/xmlchoose.png)

19. Sur la page **Paramètres d'authentification unique SAML**, les champs sont automatiquement renseignés. Entrez le nom de la configuration (par exemple : *SPSSOWAAD_Test*) dans la zone de texte **Nom** et cliquez sur Enregistrer.

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

20. Pour activer votre domaine sur Salesforce Sandbox, procédez comme suit :

    > [!NOTE]
    > Avant d’activer le domaine, vous devez créer le même sur Salesforce Sandbox. Pour plus d’informations, voir [Définition de votre nom de domaine](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Une fois le domaine créé, assurez-vous qu’il est configuré correctement.

21. Dans le volet de navigation de gauche de Salesforce Sandbox, cliquez sur **Company Settings** (Paramètres de l’entreprise) pour développer la section associée, puis cliquez sur **My Domain** (Mon domaine).

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

22. Dans la section **Authentication Configuration** (Configuration de l’authentification), cliquez sur **Authentication Policy** (Stratégie d’authentification).

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

23. Dans la section **Authentication Configuration** (Configuration de l’authentification), pour le champ **Authentication Service** (Service d’authentification), sélectionnez le nom du paramètre d’authentification unique SAML que vous avez défini lors de la configuration SSO dans Salesforce Sandbox, puis cliquez sur **Save** (Enregistrer).

    ![Configurer l'authentification unique](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **BrittaSimon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez `brittasimon@yourcompanydomain.extension`. Par exemple : BrittaSimon@contoso.com.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Salesforce Sandbox.

1. Dans le Portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **Salesforce Sandbox**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Salesforce Sandbox**.

    ![Lien Salesforce Sandbox dans la liste des applications.](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-salesforce-sandbox-test-user"></a>Créer un utilisateur de test Salesforce Sandbox

Dans cette section, un utilisateur nommé Britta Simon est créé dans Salesforce Sandbox. Salesforce Sandbox prend en charge l’approvisionnement juste-à-temps, option activée par défaut. Vous n’avez aucune opération à effectuer dans cette section. Si un utilisateur n’existe pas dans Salesforce Sandbox, un nouveau est créé lorsque vous tentez d’accéder à Salesforce Sandbox. Salesforce Sandbox prend également en charge l’attribution automatique d’utilisateurs, vous trouverez [ici](salesforce-sandbox-provisioning-tutorial.md) plus d’informations sur la façon de configurer l’attribution automatique d’utilisateurs.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Le fait de cliquer sur la vignette Salesforce Sandbox dans le panneau d’accès doit vous connecter automatiquement à l’application Salesforce Sandbox pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

* [Configurer l’approvisionnement de l’utilisateur](salesforce-sandbox-provisioning-tutorial.md)
