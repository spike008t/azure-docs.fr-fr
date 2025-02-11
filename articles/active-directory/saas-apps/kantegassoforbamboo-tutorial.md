---
title: 'Didacticiel : Intégration d’Azure Active Directory avec Kantega SSO pour Bamboo | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Kantega SSO pour Bamboo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e238b574-9e9b-43b7-ab98-d2a87ff89d48
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 34b5a5901122b60c20a33e3564dca0eb5273afa3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64711990"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bamboo"></a>Didacticiel : Intégration d’Azure Active Directory avec Kantega SSO pour Bamboo

Dans ce didacticiel, vous allez apprendre à intégrer Kantega SSO pour Bamboo avec Azure Active Directory (Azure AD).
L’intégration de Kantega SSO pour Bamboo avec Azure AD offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler qui a accès à Kantega SSO pour Bamboo.
* Vous pouvez permettre à vos utilisateurs d’être connectés automatiquement à Kantega SSO pour Bamboo (via l’authentification unique) avec leur compte Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec Kantega SSO pour Bamboo, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/)
* Un abonnement Kantega SSO pour Bamboo pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Kantega SSO pour Bamboo prend en charge l’authentification unique lancée par le **fournisseur de services et le fournisseur d’identité**.

## <a name="adding-kantega-sso-for-bamboo-from-the-gallery"></a>Ajout de Kantega SSO pour Bamboo à partir de la galerie

Pour configurer l’intégration de Kantega SSO pour Bamboo dans Azure AD, vous devez ajouter Kantega SSO pour Bamboo à partir de la galerie à la liste des applications SaaS managées.

**Pour ajouter Kantega SSO pour Bamboo à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, entrez **Kantega SSO pour Bamboo**, sélectionnez **Kantega SSO pour Bamboo** dans le panneau des résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Kantega SSO pour Bamboo dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Kantega SSO pour Bamboo sur un utilisateur de test nommé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur associé dans Kantega SSO pour Bamboo doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Kantega SSO pour Bamboo, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique Kantega SSO pour Bamboo](#configure-kantega-sso-for-bamboo-single-sign-on)** pour définir les paramètres d’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test Kantega SSO pour Bamboo](#create-kantega-sso-for-bamboo-test-user)** pour avoir un équivalent de Britta Simon dans Kantega SSO pour Bamboo, qui soit lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec Kantega SSO pour Bamboo, procédez comme suit :

1. Sur le [portail Microsoft Azure](https://portal.azure.com/), dans la page d’intégration de l’application **Kantega SSO pour Bamboo**, cliquez sur **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode lancé par le **fournisseur d’identité**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL Kantega SSO pour Bamboo](common/idp-intiated.png)

    a. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. Dans la zone de texte **URL de réponse**, tapez une URL au format suivant : `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL Kantega SSO pour Bamboo](common/metadata-upload-additional-signon.png)

    Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur, l’URL de réponse et l’URL de connexion réels. Ces valeurs sont reçues durant la configuration du plug-in Bamboo qui est décrite plus loin dans le tutoriel.

6. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

7. Dans la section **Configurer Kantega SSO pour Bamboo**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-kantega-sso-for-bamboo-single-sign-on"></a>Configurer l’authentification unique Kantega SSO pour Bamboo

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre serveur local Bamboo en tant qu’administrateur.

1. Pointez sur le roue dentée, puis cliquez sur **Modules complémentaires**.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon1.png)

1. Sous l’onglet Modules complémentaires, cliquez sur **Find new add-ons** (Trouver de nouveaux modules complémentaires). Recherchez **Kantega SSO pour Bamboo (SAML & Kerberos)** , puis cliquez sur le bouton **Installer** pour installer le nouveau plug-in SAML.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon2.png)

1. L’installation du plug-in démarre.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon21.png)

1. Une fois l’installation terminée. Cliquez sur **Fermer**.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon33.png)

1. Cliquez sur **Gérer**.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon34.png)

1. Cliquez sur **Configurer** pour configurer le nouveau plug-in.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon3.png)

1. Dans la section **SAML**. Dans le menu déroulant **Ajouter le fournisseur d’identité**, sélectionnez **Azure Active Directory (Azure AD)** .

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon4.png)

1. Sélectionnez le niveau d’abonnement **De base**.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon5.png)

1. Dans la section **Propriétés de l’application**, procédez comme suit :

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon6.png)

    a. Copiez la valeur de **URI ID d’application** et utilisez-la en tant que **Identificateur, URL de réponse et URL de connexion** dans la section **Configuration SAML de base** du portail Azure.

    b. Cliquez sur **Suivant**.

1. Dans la section **Metadata import** (Importation des métadonnées), procédez comme suit :

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon7.png)

    a. Sélectionnez **Metadata file on my computer** (Fichier de métadonnées sur mon ordinateur), puis chargez le fichier de métadonnées que vous avez téléchargé à partir du portail Azure.

    b. Cliquez sur **Suivant**.

1. Dans la section **Name and SSO location** (Nom et emplacement de l’authentification unique), procédez comme suit :

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon8.png)

    a. Ajoutez le nom du fournisseur d’identité dans la zone de texte **Identity provider name** (Nom du fournisseur d’identité) (par exemple, Azure AD).

    b. Cliquez sur **Suivant**.

1. Vérifiez le certificat de signature, puis cliquez sur **Suivant**.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon9.png)

1. Dans la section **Bamboo user accounts** (Comptes d’utilisateur Bamboo), procédez comme suit :

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon10.png)

    a. Sélectionnez **Create users in Bamboo's internal Directory if needed** (Créer des utilisateurs dans l’annuaire interne de Bamboo si nécessaire) et entrez le nom de groupe approprié pour les utilisateurs (peut être plusieurs groupes séparés par des virgules).

    b. Cliquez sur **Suivant**.

1. Cliquez sur **Terminer**.

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon11.png)

1. Dans la section **Known domains for Azure AD** (Domaines connus pour Azure AD), procédez comme suit :

    ![Configurer l'authentification unique](./media/kantegassoforbamboo-tutorial/addon12.png)

    a. Sélectionnez **Known domains** (Domaines connus) dans le volet gauche de la page.

    b. Entrez le nom de domaine dans la zone de texte **Known domains** (Domaines connus).

    c. Cliquez sur **Enregistrer**.

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

Dans cette section, vous permettez à Britta Simon d’utiliser l’authentification unique Azure en lui accordant l’accès à Kantega SSO pour Bamboo.

1. Dans le portail Microsoft Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **Kantega SSO pour Bamboo**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Kantega SSO pour Bamboo**.

    ![Lien Kantega SSO pour Bamboo dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-kantega-sso-for-bamboo-test-user"></a>Créer un utilisateur de test Kantega SSO pour Bamboo

Pour permettre aux utilisateurs Azure AD de se connecter à Bamboo, vous devez les approvisionner dans Bamboo. Dans Kantega SSO pour Bamboo, cet approvisionnement est une tâche manuelle.

**Pour approvisionner un compte d’utilisateur, procédez comme suit :**

1. Connectez-vous à votre serveur local Bamboo en tant qu’administrateur.

1. Pointez sur la roue dentée, puis cliquez sur **Gestion des utilisateurs**.

    ![Ajouter un employé](./media/kantegassoforbamboo-tutorial/user1.png)

1. Cliquez sur **Utilisateurs**. Sous la section **Add User** (Ajouter un utilisateur), procédez comme suit :

    ![Ajouter un employé](./media/kantegassoforbamboo-tutorial/user2.png)

    a. Dans la zone de texte **Username** (Nom d’utilisateur), tapez l’e-mail d’un utilisateur, par exemple, Brittasimon@contoso.com.

    b. Dans la zone de texte **Password** (Mot de passe), tapez le mot de passe de l’utilisateur.

    c. Dans la zone de texte **Confirm Password** (Confirmer le mot de passe), entrez à nouveau le mot de passe de l’utilisateur.

    d. Dans la zone de texte **Full Name** (Nom complet), tapez le nom complet d’un utilisateur, par exemple, Britta Simon.

    e. Dans la zone de texte **Email** (E-mail), tapez l’adresse e-mail d’un utilisateur, par exemple, Brittasimon@contoso.com.

    f. Cliquez sur **Enregistrer**.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Lorsque vous cliquez sur la mosaïque Kantega SSO pour Bamboo dans le volet d’accès, vous devez être connecté automatiquement à l’application Kantega SSO pour Bamboo pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
