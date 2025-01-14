---
title: Stratégies personnalisées Azure Active Directory B2C | Microsoft Docs
description: En savoir plus sur les stratégies personnalisées Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2938ae075bbd4c38b686ca6654bede678f876857
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509798"
---
# <a name="custom-policies-in-azure-active-directory-b2c"></a>Stratégies personnalisées dans Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Les stratégies personnalisées sont des fichiers de configuration qui définissent le comportement de votre client Azure Active Directory (Azure AD) B2C. Les flux d’utilisateur sont prédéfinis dans le portail Azure AD B2C pour les tâches d’identité les plus courantes. Les stratégies personnalisées peuvent être entièrement modifiées par un développeur d’identité pour effectuer de nombreuses tâches différentes.

## <a name="comparing-user-flows-and-custom-policies"></a>Comparaison des flux d’utilisateur et des stratégies personnalisées

| | Flux d’utilisateurs | Stratégies personnalisées |
|-|-------------------|-----------------|
| Utilisateurs cibles | Tous les développeurs d’applications avec ou sans expertise de l’identité | Professionnels de l’identité, intégrateurs système, consultants et équipes d’identité internes. Ils sont à l’aise avec les flux OpenIDConnect et comprennent les fournisseurs d’identité et l’authentification basée sur les revendications. |
| Mode de configuration | Portail Azure avec interface utilisateur (UI) conviviale | Modification directe des fichiers XML, puis chargement sur le portail Azure |
| Personnalisation de l’interface utilisateur | Personnalisation de l’interface utilisateur complète avec HTML, CSS et JavaScript.<br><br>Prise en charge multilingue avec chaînes personnalisées | Identique |
| Personnalisation des attributs | Attributs standard et personnalisés | Identique |
| Gestion des jetons et des sessions | Options de jetons personnalisés et de sessions multiples | Identique |
| Identity Providers | Prédéfinis fournisseur local ou social et la plupart des fournisseurs d’identité OIDC, telles que la fédération avec les clients Azure Active Directory. | OIDC basé sur les normes, OAUTH et SAML  L’authentification est également possible à l’aide de l’intégration avec les API REST. |
| Tâches d’identité | Inscription ou connexion avec des comptes locaux et de nombreux comptes sociaux<br><br>Réinitialisation de mot de passe en libre service<br><br>Modification de profil<br><br>Authentification multifacteur<br><br>Personnalisation des jetons et des sessions<br><br>Flux de jetons d’accès | Réalisation des mêmes tâches que les flux d’utilisateur à l’aide de fournisseurs d’identité personnalisés ou utilisation des étendues personnalisées<br><br>Provisionnement d’un compte d’utilisateur dans un autre système au moment de l’inscription<br><br>Envoi d’un e-mail de bienvenue avec votre propre fournisseur de service de messagerie<br><br>Utilisation d’un magasin d’utilisateurs en dehors d’Azure AD B2C<br><br>Validation des informations fournies par l’utilisateur avec un système approuvé à l’aide d’une API |

## <a name="policy-files"></a>Fichiers de stratégie

Les trois types suivants de fichiers de stratégie sont utilisés :

- **Fichier de base** -contient la plupart des définitions. Il est recommandé d’apporter aussi peu de modifications que possible à ce fichier pour faciliter la résolution des problèmes et la maintenance à long terme de vos stratégies.
- **Fichier d’extensions** - contient les modifications de configuration propres à votre client.
- **Fichier de la partie de confiance (RP)** - le seul fichier centré sur les tâches appelé directement par l’application ou le service (la partie de confiance). Chaque tâche unique nécessite sa propre partie de confiance et, en fonction des exigences de personnalisation, le nombre peut être « le nombre total d’applications multiplié par le nombre total de cas d’utilisation ».

Les flux d’utilisateur dans Azure AD B2C suivent le modèle à trois fichiers décrit ci-dessus, mais le développeur ne voit que le fichier RP, tandis que le portail Azure modifie en arrière-plan le fichier d’extensions.

## <a name="custom-policy-core-concepts"></a>Concepts fondamentaux des stratégies personnalisées

Le service de gestion des accès et des identités clients d’Azure inclut :

- Un annuaire d’utilisateurs accessible via Microsoft Graph, qui contient des données utilisateur pour les comptes locaux et les comptes fédérés.
- L’accès à l’**infrastructure d’expérience d’identité**, qui orchestre les relations de confiance entre les utilisateurs et les entités et transmet les revendications des uns aux autres pour mener à bien une tâche de gestion d’identité ou d’accès. 
- Un service d’émission de jeton de sécurité (STS), qui émet des jetons d’ID, des jetons d’actualisation et des jetons d’accès (ainsi que les assertions SAML équivalentes) et les valide afin de protéger les ressources.

Azure AD B2C interagit de façon séquentielle avec les fournisseurs d’identité, les utilisateurs, d’autres systèmes et l’annuaire d’utilisateurs locaux pour effectuer une tâche d’identité. Par exemple, connecter un utilisateur, inscrire un nouvel utilisateur ou réinitialiser un mot de passe. L’infrastructure d’expérience d’identité et une stratégie (également appelée « parcours utilisateur » ou « stratégie d’infrastructure de confiance ») établissent la confiance mutuelle et définissent explicitement les acteurs, les actions, les protocoles et la séquence d’étapes à effectuer.

L’infrastructure d’expérience d’identité est une plateforme Azure cloud, entièrement configurable et pilotée par des stratégies, qui orchestre les relations de confiance entre les entités dans des formats de protocoles standard, notamment OpenIDConnect, OAuth, SAML, WSFed, ainsi que quelques protocoles non standard (par exemple, des échanges de revendications intersystèmes basés sur l’API REST). L’infrastructure crée des expériences conviviales en marque blanche qui prennent en charge HTML et CSS.

Une stratégie personnalisée est représentée par un ou plusieurs fichiers au format XML qui se font mutuellement référence dans une chaîne hiérarchique. Les éléments XML définissent notamment les éléments suivants : schéma de revendications, transformations de revendications, définitions de contenu, fournisseurs de revendications, profils techniques et étapes d’orchestration du parcours utilisateur. Une stratégie personnalisée est accessible sous la forme d’un ou plusieurs fichiers XML qui sont exécutés par l’infrastructure d’expérience d’identité lorsqu’ils sont appelés par une partie de confiance. Les développeurs qui configurent des stratégies personnalisées doivent définir les relations de confiance dans leurs moindres détails pour inclure les points de terminaison de métadonnées et les définitions exactes des échanges de revendications, et configurer les secrets, les clés et les certificats selon les besoins de chaque fournisseur d’identité.

### <a name="inheritance-model"></a>Modèle d’héritage

Lorsqu’une application appelle le fichier de stratégie RP, l’infrastructure d’expérience d’identité dans Azure AD B2C ajoute tous les éléments à partir du fichier de base, du fichier d’extensions, puis du fichier de stratégie RP pour assembler la stratégie en vigueur.  Les éléments du même type et du même nom dans le fichier RP remplacent ceux figurant dans les extensions, et les extensions remplacent la base.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Bien démarrer avec les stratégies personnalisées](active-directory-b2c-get-started-custom.md)
