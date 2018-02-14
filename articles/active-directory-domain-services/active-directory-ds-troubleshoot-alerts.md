---
title: "Services de domaine Azure Active Directory : dépannage des alertes | Microsoft Docs"
description: "Dépannage des alertes pour les services de domaine Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: ergreenl
ms.openlocfilehash: b2e0edf3588f3b1db5f4b6641019be1ded9cb50e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services : dépannage des alertes
Cet article fournit des guides de dépannage pour les alertes que vous pouvez rencontrer sur votre domaine géré.


Choisissez les étapes de résolution qui correspondent au message ou ID d’erreur que vous rencontrez.

| **ID de l'alerte** | **Message d'alerte** | **Résolution :** |
| --- | --- | :--- |
| AADDS001 | *LDAP sécurisé sur Internet est activé pour le domaine géré. Toutefois, l’accès au port 636 n’est pas verrouillé à l’aide d’un groupe de sécurité réseau. Cela peut exposer les comptes d’utilisateur sur le domaine géré aux attaques par force brute.* | [Configuration de LDAP sécurisé incorrecte](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *L’annuaire Azure AD associé à votre domaine géré a peut-être été supprimé. Le domaine géré n’est plus dans une configuration prise en charge. Microsoft ne peut pas surveiller, gérer, mettre à jour et synchroniser votre domaine géré.* | [Répertoire manquant](#aadds100-missing-directory) |
| AADDS101 | *Les services de domaine Azure AD ne peuvent pas être activés dans un annuaire Azure AD B2C.* | [Azure AD B2C est en cours d’exécution dans ce répertoire](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Un principal de service requis pour que les services de domaine Azure AD fonctionnent correctement a été supprimé de votre annuaire Azure AD. Cette configuration affecte la capacité de Microsoft à surveiller, gérer, mettre à jour et synchroniser votre domaine géré.* | [Principal de service manquant](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *La plage d’adresses IP pour le réseau virtuel dans lequel vous avez activé les services de domaine Azure AD est dans une plage d’adresses IP publiques. Les services de domaine Azure AD doivent être activés dans un réseau virtuel avec une plage d’adresses IP privées. Cette configuration affecte la capacité de Microsoft à surveiller, gérer, mettre à jour et synchroniser votre domaine géré.* | [L’adresse est dans une plage d’adresses IP publiques](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft ne peut pas atteindre les contrôleurs de domaine pour ce domaine géré. Cela peut se produire si un groupe de sécurité réseau (NSG) configuré sur votre réseau virtuel bloque l’accès à un domaine géré. Une autre raison possible est s’il existe un itinéraire défini par l’utilisateur qui bloque le trafic entrant à partir d’Internet.* | [Erreur réseau](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100 : Répertoire manquant
**Message d'alerte :**

*L’annuaire Azure AD associé à votre domaine géré a peut-être été supprimé. Le domaine géré n’est plus dans une configuration prise en charge. Microsoft ne peut pas surveiller, gérer, mettre à jour et synchroniser votre domaine géré.*

**Correction :**

Cette erreur est généralement causée par un déplacement incorrect de votre abonnement Azure vers un nouvel annuaire Azure AD, et par la suppression de l’ancien répertoire Azure AD qui est toujours associé à des services de domaine Azure AD.

Cette erreur est irrécupérable. Pour résoudre ce problème, vous devez [supprimer votre domaine géré existant](active-directory-ds-disable-aadds.md) puis le recréer dans votre nouveau répertoire. Si vous avez des difficultés à le supprimer, contactez l’équipe produit des services de domaine Azure Active Directory [pour obtenir de l’aide](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101 : Azure AD B2C est en cours d’exécution dans ce répertoire
**Message d'alerte :**

*Les services de domaine Azure AD ne peuvent pas être activés dans un annuaire Azure AD B2C.*

**Correction :**

>[!NOTE]
>Pour pouvoir continuer à utiliser les services de domaine Azure AD, vous devez recréer votre instance de services de domaine Azure AD dans un répertoire hors Azure AD B2C.

Pour restaurer votre service, procédez comme suit :

1. [Supprimez votre domaine géré](active-directory-ds-disable-aadds.md) de votre annuaire Azure AD existant.
2. Créez un répertoire qui n’est pas un répertoire Azure AD B2C.
3. Suivez le guide de [Prise en main](active-directory-ds-getting-started.md) pour recréer un domaine géré.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103 : L’adresse est dans une plage d’adresses IP publiques

**Message d'alerte :**

*La plage d’adresses IP pour le réseau virtuel dans lequel vous avez activé les services de domaine Azure AD est dans une plage d’adresses IP publiques. Les services de domaine Azure AD doivent être activés dans un réseau virtuel avec une plage d’adresses IP privées. Cette configuration affecte la capacité de Microsoft à surveiller, gérer, mettre à jour et synchroniser votre domaine géré.*

**Correction :**

> [!NOTE]
> Pour résoudre ce problème, vous devez supprimer votre domaine géré existant et le créer de nouveau dans un réseau virtuel avec une plage d’adresses IP privées. Ce processus cause une interruption.

Avant de commencer, lisez la section **Espace d’adressage IPv4** de [cet article](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

1. [Supprimez votre domaine géré](active-directory-ds-disable-aadds.md) de votre annuaire.
2. Corrigez la plage d’adresses IP pour le sous-réseau
  1. Accédez à la [page Réseaux virtuels sur le portail Azure](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Sélectionnez le réseau virtuel que vous envisagez d’utiliser pour les services de domaine Azure AD.
  3. Cliquez sur **Espace d’adressage** sous Paramètres
  4. Mettez à jour la plage d’adresses en cliquant sur la plage d’adresses existante et en la modifiant ou en ajoutant une plage d’adresses supplémentaire. Assurez-vous que la nouvelle plage d’adresses est dans une plage d’adresses IP privées. Enregistrez vos modifications.
  5. Cliquez sur **Sous-réseaux** dans la navigation de gauche.
  6. Cliquez sur le sous-réseau que vous souhaitez modifier dans la table.
  7. Mettez à jour la plage d’adresses et enregistrez vos modifications.
3. Suivez [le guide Prise en main des services de domaine Azure AD](active-directory-ds-getting-started.md) pour recréer votre domaine géré. Veillez à sélectionner un réseau virtuel avec une plage d’adresses IP privées.
4. Pour joindre vos machines virtuelles à votre nouveau domaine, suivez [ce guide](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Vérifiez l’intégrité de votre domaine après deux heures pour vous assurer que vous avez effectué les étapes correctement.


## <a name="contact-us"></a>Nous contacter
Contactez l’équipe produit des Services de domaine Azure Active Directory pour [partager vos commentaires ou pour obtenir de l’aide](active-directory-ds-contact-us.md).