---
title: En savoir plus sur Microsoft Authentication Library (MSAL) | Azure
description: Microsoft Authentication Library (MSAL) permet aux développeurs d’applications d’acquérir des jetons afin d’appeler des API web sécurisées. Ces API web peuvent être Microsoft Graph, d’autres APIS Microsoft, des API web de tiers ou vos propres API web. MSAL prend en charge plusieurs architectures et plateformes d’application.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bd3e7d47b6e3083af6f388a5cd750da240a76b6
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392886"
---
# <a name="overview-of-microsoft-authentication-library-msal"></a>Vue d’ensemble de Microsoft Authentication Library (MSAL)
Microsoft Authentication Library (MSAL) permet aux développeurs d’acquérir des [jetons](developer-glossary.md#security-token) à partir du point de terminaison de la plateforme d’identités Microsoft afin d’accéder aux API web sécurisées. Ces API web peuvent être Microsoft Graph, d’autres APIS Microsoft, des API web de tiers ou vos propres API web. MSAL est disponible pour .NET, JavaScript, Android et iOS, qui prennent en charge de nombreuses architectures et plateformes d’application différentes.

MSAL vous permet d’obtenir des jetons différentes manières, avec une API cohérente pour plusieurs plateformes. MSAL offre les avantages suivants :

* Ne nécessite pas d’utiliser directement des bibliothèques ou du code OAuth pour le protocole dans votre application.
* Acquiert des jetons pour le compte d’un utilisateur ou d’une application (si applicable à la plateforme).
* Gère un cache de jeton et actualise les jetons pour vous quand ils sont sur le point d’expirer. Vous n’avez pas besoin de gérer l’expiration du jeton.
* Vous aide à spécifier le public auquel vous souhaitez que votre application se connecte (votre organisation, plusieurs organisations, comptes professionnels, scolaires et personnels Microsoft, identités sociales avec Azure AD B2C, utilisateurs dans des clouds souverains et nationaux).
* Vous permet de configurer votre application à partir de fichiers de configuration.
* Vous permet de dépanner votre application en exposant des exceptions, une journalisation et une télémétrie exploitables.

## <a name="application-types-and-scenarios"></a>Types d’application et scénarios
MSAL permet d’acquérir un jeton à partir de plusieurs types d’applications : applications web, API web, applications monopages (JavaScript), applications mobiles et natives, démons et applications côté serveur. 

MSAL peut être utilisé dans de nombreux scénarios d’application, dont les suivants :

* [Applications monopages (JavaScript)](scenario-spa-overview.md) 
* [Application web connectant des utilisateurs](scenario-web-app-sign-user-overview.md)
* [Application web connectant un utilisateur et appelant une API web pour le compte de l’utilisateur](scenario-web-app-call-api-overview.md)
* [Protection d’une API web afin que seuls des utilisateurs authentifiés puissent y accéder](scenario-protected-web-api-overview.md)
* [API web appelant une autre API web en aval pour le compte de l’utilisateur connecté](scenario-web-api-call-api-overview.md)
* [Application de bureau appelant une API web pour le compte de l’utilisateur connecté](scenario-desktop-overview.md)
* [Application mobile appelant une API web pour le compte de l’utilisateur qui s’est connecté de manière interactive](scenario-mobile-overview.md).
* [Application démon de bureau/service appelant une API web pour son propre compte](scenario-daemon-overview.md)

## <a name="languages-and-frameworks"></a>Langages et infrastructures

| Bibliothèque | Plateformes et infrastructures prises en charge|
| --- | --- | 
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/>[MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)| .NET framework, .NET Core, Xamarin Android, Xamarin iOS, plateforme Windows universelle|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/>[MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)| Infrastructures JavaScript/TypeScript telles que AngularJS, Ember.js ou Durandal.js|
| ![MSAL pour Android](media/sample-v2-code/logo_Android.png) <br/>[MSAL pour Android (préversion)](https://github.com/AzureAD/microsoft-authentication-library-for-android)|Android|
| ![MSAL pour iOS](media/sample-v2-code/logo_iOS.png) <br/>[MSAL.Objective-C (préversion)](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|iOS|

## <a name="differences-between-adal-and-msal"></a>Différences entre les bibliothèques ADAL et MSAL
La Bibliothèque d’authentification Active Directory (ADAL) s’intègre avec le point de terminaison d’Azure AD pour les développeurs (v1.0), tandis que la bibliothèque MSAL s’intègre avec le point de terminaison de la plateforme d’identités Microsoft (v2.0). Le point de terminaison v1.0 prend en charge les comptes de travail, mais pas les comptes personnels. Le point de terminaison v2.0 est l’unification de comptes personnels et professionnels Microsoft dans un système d’authentification unique. En outre, avec MSAL, vous pouvez également obtenir des authentifications pour Azure AD B2C.

Pour plus d’informations, lisez la documentation sur la [migration d’ADAL.NET vers MSAL.NET](msal-net-migration.md) et la [migration d’ADAL.js vers MSAL.js](msal-compare-msal-js-and-adal-js.md).

            