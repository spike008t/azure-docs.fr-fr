---
title: Fichier Include
description: Fichier Include
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: a11b291ab89dc9f8159e00e1f2304706f041068e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120157"
---
## <a name="test-your-code"></a>Test de votre code

Pour exécuter votre projet, sélectionnez **F5** dans Visual Studio. Votre application **MainWindow** s’affiche, comme illustré ici :

![Tester votre application](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

La première fois que vous exécutez l’application et que vous sélectionnez le bouton **Call Microsoft Graph API** (Appeler l’API Microsoft Graph), vous êtes invité à vous connecter. Utilisez un compte Azure Active Directory (compte professionnel ou scolaire) ou un compte Microsoft (live.com, outlook.com) pour tester votre application.

![Se connecter à l’application](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Accorder les droits d’accès à l’application

La première fois que vous vous connectez à votre application, vous êtes également invité à autoriser l’application à accéder à votre profil et vous connecter, comme illustré ici :

![Donner votre accord pour l’accès par l’application](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Afficher les résultats de l’application

Une fois connecté, vous devez voir les informations de profil utilisateur qui sont retournées par l’appel à l’API Microsoft Graph. Les résultats s’affichent dans la zone **API Call Results** (Résultats de l’appel à l’API). Les informations de base concernant le jeton qui a été acquis via l’appel à `AcquireTokenInteractive` ou `AcquireTokenSilent` doivent s’afficher dans la zone **Token Info** (Informations sur le jeton). Les résultats contiennent les propriétés suivantes :

|Propriété  |Format  |Description |
|---------|---------|---------|

|**Nom d’utilisateur**  |<span>user@domain.com</span> |Le nom d’utilisateur qui est utilisé pour identifier l’utilisateur.| |**Expiration du jeton** |Date/heure | Date/heure à laquelle le jeton expire. MSAL repousse la date d’expiration en renouvelant le jeton de façon appropriée.|


<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Informations supplémentaires sur les étendues et les autorisations déléguées

L’API Microsoft Graph nécessite l’étendue *user.read* pour lire le profil d’un utilisateur. Par défaut, cette étendue est automatiquement ajoutée à toutes les applications inscrites dans le portail d’inscription de l’application. D’autres API pour Microsoft Graph ainsi que des API personnalisées pour votre serveur principal peuvent nécessiter des étendues supplémentaires. L’API Microsoft Graph nécessite l’étendue *Calendars.Read* pour répertorier les calendriers de l’utilisateur.

Pour accéder aux calendriers de l’utilisateur dans le contexte d’une application, ajoutez l’autorisation déléguée *Calendars.Read* aux informations d’inscription de l’application. Ajoutez ensuite l’étendue *Calendars.Read* à l’appel `acquireTokenSilent`.

>[!NOTE]
>L’utilisateur peut être invité à donner des consentements supplémentaires à mesure que vous augmentez le nombre d’étendues.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
