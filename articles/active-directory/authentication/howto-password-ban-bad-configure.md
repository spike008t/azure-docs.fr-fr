---
title: Comment exclure des mots de passe faibles dans Azure AD - Azure Active Directory
description: Interdire les mots de passe faibles dans votre environnement via des mots de passe interdits dynamiquement dans Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28201e09a4025c0c8820abc6836a5923e48eb885
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742292"
---
# <a name="configuring-the-custom-banned-password-list"></a>Configuration de la liste de mots de passe interdits personnalisée

Nombre d’organisations estiment que leurs utilisateurs créent des mots de passe à l’aide des mots locaux courants comme un établissement scolaire, une équipe de sport ou une personne célèbre, ce qui les rend faciles à deviner. La liste de mots de passe interdits personnalisée de Microsoft permet aux entreprises d’ajouter des chaînes à évaluer et bloquer, en plus de la liste de mots de passe interdits globale, lorsque des utilisateurs et administrateurs tentent de modifier ou de réinitialiser un mot de passe.

## <a name="add-to-the-custom-list"></a>Ajouter à la liste personnalisée

La configuration de la liste de mots de passe interdits personnalisée requiert une licence Azure Active Directory Premium P1 ou P2. Pour plus d’informations sur les licences Azure Active Directory, consultez le [page de tarification d’Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

1. Se connecter à la [Azure portal](https://portal.azure.com) et accédez à **Azure Active Directory**, **méthodes d’authentification**, puis **protection de mot de passe**.
1. Définissez l’option **Enforce custom list** (Appliquer la liste personnalisée) sur **Oui**.
1. Ajouter des chaînes à la **liste de mots de passe interdits personnalisée**, à raison d’une chaîne par ligne
   * La liste de mots de passe interdits personnalisé peut contenir jusqu’à 1 000 mots.
   * La liste de mots de passe interdits personnalisée respecte la casse.
   * La liste de mots de passe interdits personnalisée prend en compte la substitution de caractères courante.
      * Exemple : « o » et « 0 » ou « a » et « \@ »
   * La longueur de chaîne minimale est de quatre caractères et la valeur maximale est de 16 caractères.
1. Lorsque vous avez ajouté toutes les chaînes, cliquez sur **Enregistrer**.

> [!NOTE]
> L’application de la mise à jour de la liste de mots de passe interdits personnalisée peut prendre plusieurs heures.

![Modifier la liste de mots de passe interdits personnalisée sous Méthodes d’authentification dans le portail Azure](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Fonctionnement

Chaque fois qu’un utilisateur ou administrateur réinitialise ou modifie un mot de passe Azure AD, celui-ci transite par la liste de mots de passe interdits pour confirmer qu’il ne figure pas dans une liste. Cette vérification est incluse dans les mots de passe définis ou modifiés à l’aide d’Azure AD.

## <a name="what-do-users-see"></a>Ce que voient les utilisateurs

Lorsqu’un utilisateur tente de réinitialiser un mot de passe pour le remplacer par un mot de passe interdit, le message d’erreur suivant s’affiche :

Malheureusement, votre mot de passe contient un mot, une expression ou un modèle qui rend votre mot de passe facile à deviner. Réessayez avec un autre mot de passe.

## <a name="next-steps"></a>Étapes suivantes

[Vue d’ensemble conceptuelle des listes de mots de passe interdits](concept-password-ban-bad.md)

[Vue d’ensemble conceptuelle de la protection de mot de passe Azure AD](concept-password-ban-bad-on-premises.md)

[Activer l’intégration locale aux listes de mots de passe interdits](howto-password-ban-bad-on-premises.md)
