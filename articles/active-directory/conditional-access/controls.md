---
title: Que sont les contrôles d’accès dans l’accès conditionnel Azure Active Directory ? | Microsoft Docs
description: Découvrez comment fonctionnent les contrôles d’accès dans l’accès conditionnel Azure Active Directory.
services: active-directory
keywords: accès conditionnel aux applications, accès conditionnel à Azure AD, accès sécurisé aux ressources d’entreprise, stratégies d’accès conditionnel
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: acc30bc18921b79be3b5e5b2ae340eab29dd5bcf
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66305758"
---
# <a name="what-are-access-controls-in-azure-active-directory-conditional-access"></a>Que sont les contrôles d’accès dans l’accès conditionnel Azure Active Directory ?

Avec l’[accès conditionnel Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), vous pouvez contrôler la façon dont les utilisateurs autorisés accèdent à vos applications cloud. Dans une stratégie d’accès conditionnel, vous définissez la réponse (« faire ») sur la raison du déclenchement de votre stratégie (« quand cela se produit »).

![Contrôle](./media/controls/10.png)

Dans le contexte de l’accès conditionnel,

- « **Quand cela se produit** » est une **condition**

- « **Faire** » est un **contrôle d’accès**

Une stratégie d’accès conditionnel combine une instruction de condition à des contrôles.

![Contrôle](./media/controls/61.png)

Chaque contrôle est soit une exigence qui doit être remplie par la personne ou le système qui se connecte, soit une restriction sur ce que l’utilisateur peut faire après la connexion.

Il existe deux types de contrôles :

- **Contrôle d’octroi** - pour réguler l’accès

- **Contrôles de session** - pour restreindre l’accès au sein d’une session

Cette rubrique explique les divers contrôles qui sont disponibles dans l’accès conditionnel Azure AD. 

## <a name="grant-controls"></a>Contrôles d’octroi

Avec les contrôles d’octroi, vous pouvez bloquer l’accès complètement ou autoriser l’accès avec des exigences supplémentaires en sélectionnant les contrôles de votre choix. Pour de nombreux contrôles, vous pouvez exiger ce qui suit :

- Tous les contrôles sélectionnés doivent être satisfaits (*AND*).
- Un seul contrôle sélectionné doit être satisfait (*OR*).

![Contrôle](./media/controls/18.png)

### <a name="multi-factor-authentication"></a>Authentification multifacteur

Vous pouvez utiliser ce contrôle pour exiger une authentification multifacteur pour accéder à l’application cloud spécifiée. Ce contrôle prend en charge les fournisseurs multifacteur suivants :

- Azure Multi-Factor Authentication

- Un fournisseur d’authentification multifacteur local combiné avec les services de fédération Active Directory (AD FS).

L’authentification multifacteur contribue à empêcher tout accès à vos ressources par un utilisateur non autorisé qui peut avoir obtenu l’accès aux informations d’identification principales d’un utilisateur valide.

### <a name="compliant-device"></a>Conformité de l’appareil

Vous pouvez configurer des stratégies d’accès conditionnel au niveau de l’appareil. L’objectif d’une stratégie d’accès conditionnel appareil par appareil est de n’accorder l’accès aux applications cloud sélectionnées qu’aux [appareils managés](require-managed-devices.md). Exiger qu’un appareil soit marqué comme conforme est une option dont vous disposez pour limiter l’accès aux appareils managés. Un appareil peut être marqué comme conforme par Intune (pour n’importe quel système d’exploitation d’appareil), ou par votre système MDM tiers pour les appareils Windows 10. Les systèmes MDM tiers pour les systèmes d’exploitation autres que Windows 10 ne sont pas pris en charge. 

Votre appareil doit être inscrit auprès d’Azure AD avant de pouvoir être marqué comme conforme. Pour inscrire un appareil, vous disposez de trois options : 

- [Appareils inscrits sur Azure AD](../devices/overview.md#azure-ad-registered-devices)
- [Appareils joints Azure AD](../devices/overview.md#azure-ad-joined-devices)  
- [Appareils avec jonction Azure AD Hybride](../devices/overview.md#hybrid-azure-ad-joined-devices)

Pour plus d’informations, consultez le [guide pratique pour exiger des appareils managés et accéder aux applications cloud avec l’accès conditionnel](require-managed-devices.md).

### <a name="hybrid-azure-ad-joined-device"></a>Appareil joint Azure AD hybride

Pour configurer des stratégies d’accès conditionnel au niveau de l’appareil, vous pouvez également exiger un appareil joint Azure AD hybride. Cette exigence fait référence aux tablettes professionnelles, ordinateurs portables et postes de travail Windows joints à une instance sur site d’Active Directory. Si cette option est sélectionnée, votre stratégie d’accès conditionnel accorde l’accès aux tentatives avec des appareils joints à votre Active Directory local et votre Azure Active Directory.  

Pour plus d’informations, consultez [Configurer des stratégies d’accès conditionnel au niveau de l’appareil Azure Active Directory](require-managed-devices.md).

### <a name="approved-client-app"></a>Application cliente approuvée

Étant donné que vos employés utilisent des appareils mobiles pour les tâches personnelles et professionnelles, vous pouvez souhaiter avoir la possibilité de protéger les données d’entreprise accessibles à l’aide d’appareils même si ce n’est pas vous qui les gérez.
Vous pouvez utiliser des [stratégies de protection des applications Intune](https://docs.microsoft.com/intune/app-protection-policy) pour mieux protéger les données de votre entreprise, quelle que soit votre solution de gestion des appareils mobiles (MDM).

Avec les applications clientes approuvées, vous pouvez exiger qu’une application cliente qui tente d’accéder à vos applications cloud prenne en charge des [stratégies de protection des applications Intune](https://docs.microsoft.com/intune/app-protection-policy). Par exemple, vous pouvez restreindre l’accès à Exchange Online à l’application Outlook. Une stratégie d’accès conditionnel qui requiert des applications clientes approuvées est aussi appelée [stratégie d’accès conditionnel basé sur les applications](app-based-conditional-access.md). Pour obtenir la liste des applications clientes approuvées prises en charge, consultez [Spécification d’application cliente approuvée](technical-reference.md#approved-client-app-requirement).

### <a name="app-protection-policy-preview"></a>Stratégie de protection des applications (version préliminaire)

Étant donné que vos employés utilisent des appareils mobiles pour les tâches personnelles et professionnelles, vous pouvez souhaiter avoir la possibilité de protéger les données d’entreprise accessibles à l’aide d’appareils même si ce n’est pas vous qui les gérez.
Vous pouvez utiliser des [stratégies de protection des applications Intune](https://docs.microsoft.com/intune/app-protection-policy) pour mieux protéger les données de votre entreprise, quelle que soit votre solution de gestion des appareils mobiles (MDM).

Avec la stratégie de protection d’application, vous pouvez limiter l’accès aux applications clientes qui ont signalé à Azure AD a ayant reçu [stratégies Intune app protection](https://docs.microsoft.com/intune/app-protection-policy). Par exemple, vous pouvez restreindre l’accès à Exchange Online à l’application Outlook qui a une stratégie de protection d’application Intune. Une stratégie d’accès conditionnel qui requiert la stratégie de protection des applications est également appelé [stratégie accès conditionnel basé sur la protection des applications](app-protection-based-conditional-access.md). 

Votre appareil doit être inscrit à Azure AD avant d’une application peut être marquée en tant que stratégie protégé.

Pour obtenir la liste de stratégie pris en charge les applications clientes protégé, consultez [exigence de stratégie de protection application](technical-reference.md#app-protection-policy-requirement).


### <a name="terms-of-use"></a>Conditions d’utilisation

Vous pouvez demander à un utilisateur dans votre locataire de donner son consentement pour les conditions d’utilisation avant de lui accorder l’accès à une ressource. En tant qu’administrateur, vous pouvez configurer et personnaliser les conditions d’utilisation en chargeant un document PDF. Si un utilisateur est soumis à ce contrôle, il est autorisé à accéder à une application uniquement s’il a accepté les conditions d’utilisation.

### <a name="custom-controls-preview"></a>Contrôles personnalisés (préversion)

Vous pouvez ajouter des contrôles personnalisés dans l’accès conditionnel qui redirigent les utilisateurs vers un service compatible pour satisfaire à d’autres exigences en dehors d’Azure Active Directory. Cela vous permet d’utiliser certains fournisseurs de vérification et l’authentification multifacteur externes pour appliquer des exigences d’accès conditionnel. Pour satisfaire à ce contrôle, le navigateur de l’utilisateur est redirigé vers le service externe, effectue les opérations d’authentification et de vérification requises, puis est redirigé vers Azure Active Directory. Si l’utilisateur a été correctement authentifié ou vérifié, il continue dans le flux d’accès conditionnel. 

## <a name="custom-controls"></a>Contrôles personnalisés

Les contrôles personnalisés sont une fonctionnalité de l’édition Premium P1 d’Azure Active Directory. Quand vous utilisez les contrôles personnalisés, les utilisateurs sont redirigés vers un service compatible pour satisfaire à d’autres exigences en dehors d’Azure Active Directory. Pour satisfaire à ce contrôle, le navigateur de l’utilisateur est redirigé vers le service externe, effectue les opérations d’authentification et de vérification requises, puis est redirigé vers Azure Active Directory. Azure Active Directory vérifie la réponse. Si l’utilisateur a été correctement authentifié ou vérifié, il continue dans le flux d’accès conditionnel.

Ces contrôles permettent l’utilisation de certains services externes ou personnalisés comme contrôles d’accès conditionnel. Ils étendent généralement les fonctionnalités de l’accès conditionnel.

Les fournisseurs suivants offrent un service compatible :

- [Duo Security](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [Identité Ping](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- RSA
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

Pour plus d’informations sur ces services, contactez directement les fournisseurs.

### <a name="creating-custom-controls"></a>Création de contrôles personnalisés

Pour créer un contrôle personnalisé, commencez par contacter le fournisseur que vous souhaitez utiliser. Les fournisseurs autres que Microsoft ont chacun leurs propres processus et exigences que vous devez suivre pour vous inscrire, vous abonner ou devenir membre du service, et pour demander à intégrer l’accès conditionnel. Le fournisseur vous fournit alors un bloc de données au format JSON. Ces données permettent au fournisseur et à l’accès conditionnel de fonctionner ensemble pour votre locataire, créent le contrôle et définissent de quelle façon l’accès conditionnel indique si vos utilisateurs ont réussi la vérification auprès du fournisseur.

Contrôles personnalisés ne peut pas être utilisés avec l’automatisation du service Identity Protection exigeant une authentification multifacteur ou à élever les rôles dans Privileged Identity Manager (PIM).

Copiez les données JSON et collez-les dans la zone de texte correspondante. Ne modifiez pas les données JSON, à moins d’être parfaitement sûr de la modification que vous apportez. Une modification inappropriée peut rompre la liaison entre le fournisseur et Microsoft, et potentiellement verrouiller l’accès à votre compte et aux comptes de vos utilisateurs.

L’option pour créer un contrôle personnalisé se trouve dans la section **Gérer** de la page **Accès conditionnel**.

![Contrôle](./media/controls/82.png)

Cliquez sur **Nouveau contrôle personnalisé** pour ouvrir un panneau contenant une zone de texte réservée aux données JSON de votre contrôle.  

![Contrôle](./media/controls/81.png)

### <a name="deleting-custom-controls"></a>Suppression de contrôles personnalisés

Pour supprimer un contrôle personnalisé, vous devez d’abord vous assurer qu’il n’est pas utilisé dans une stratégie d’accès conditionnel. Quand vous avez terminé :

1. Accédez à la liste de contrôles personnalisés

2. Cliquez sur...  

3. Sélectionnez **Supprimer**.

### <a name="editing-custom-controls"></a>Modification des contrôles personnalisés

Pour modifier un contrôle personnalisé, vous devez supprimer le contrôle existant et créer un autre contrôle avec les nouvelles informations.

## <a name="session-controls"></a>Contrôles de session

Les contrôles de session permettent de limiter l’expérience dans une application cloud. Les contrôles de session sont appliqués par les applications cloud et s’appuient sur des informations supplémentaires fournies par Azure AD à l’application concernant la session.

![Contrôle](./media/controls/31.png)

### <a name="use-app-enforced-restrictions"></a>Utiliser les restrictions appliquées par l’application

Vous pouvez utiliser ce contrôle pour exiger qu’Azure AD transmette les informations d’appareil aux applications cloud sélectionnées. Les informations d’appareil permettent aux applications cloud de savoir si une connexion est établie à partir d’un appareil conforme ou joint au domaine. Ce contrôle prend uniquement en charge SharePoint Online et Exchange Online comme applications cloud sélectionnées. Lorsqu’elle est sélectionnée, l’application cloud utilise les informations d’appareil pour fournir aux utilisateurs une expérience limitée ou complète en fonction de l’état de l’appareil.

Pour plus d'informations, consultez les rubriques suivantes :

- [Activation d’un accès limité avec SharePoint Online](https://aka.ms/spolimitedaccessdocs)

- [Activation d’un accès limité avec Exchange Online](https://aka.ms/owalimitedaccess)

## <a name="next-steps"></a>Étapes suivantes

- Pour savoir comment configurer une stratégie d’accès conditionnel, consultez [Exiger une authentification multifacteur (MFA) pour des applications spécifiques disposant d’un accès conditionnel Azure Active Directory](app-based-mfa.md).

- Si vous êtes prêt à configurer des stratégies d’accès conditionnel pour votre environnement, consultez les [Meilleures pratiques pour l’accès conditionnel dans Azure Active Directory](best-practices.md).
