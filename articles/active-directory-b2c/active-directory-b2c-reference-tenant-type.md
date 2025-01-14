---
title: Disponibilité régionale et résidence des données dans Azure Active Directory B2C | Microsoft Docs
description: Une rubrique sur les types de locataires Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 30f13a3b85e68babcaef62b9200a296105b3ce83
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509006"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C : Disponibilité des régions et résidence des données
La disponibilité régionale et la résidence des données sont deux concepts très différents qui ne s’appliquent pas à Azure Active Directory B2C de la même façon qu’à Azure. Cet article explique les différences entre ces deux concepts et compare la manière dont ils s’appliquent à Azure et Azure Active Directory B2C.

## <a name="summary"></a>Résumé
Azure Active Directory B2C est **généralement disponible dans le monde entier** avec l’option de **résidence des données aux États-Unis ou en Europe**.

## <a name="concepts"></a>Concepts
* La **disponibilité régionale** fait référence à l’endroit où se trouve le service pour utilisation.
* La **résidence des données** fait référence à l’endroit où sont stockées les données des utilisateurs.

## <a name="region-availability"></a>Disponibilité des régions
Azure Active Directory B2C est disponible dans le monde entier via le cloud public Azure. 

Cela diffère du modèle suivi par la plupart des autres services Azure qui associent la disponibilité à la résidence des données. C’est le cas par exemple dans la page [Produits disponibles par région](https://azure.microsoft.com/regions/services/) et la [calculatrice de tarification Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Résidence des données
Azure Active Directory B2C conserve les données des utilisateurs aux États-Unis ou en Europe.

La résidence des données est déterminée selon le pays/la région sélectionné lors de la [création d’un client Azure Active Directory B2C](active-directory-b2c-get-started.md).

![Capture d’écran d’un aperçu du client](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Les données des pays/régions suivants sont conservées aux États-Unis :

> États-Unis, Canada, Costa Rica, République dominicaine, Salvador, Guatemala, Mexique, Panama, Porto Rico et Trinité-et-Tobago

Les données des pays/régions suivants sont conservées en Europe :

> Algérie, Autriche, Azerbaïdjan, Bahreïn, Bélarus, Belgique, Bulgarie, Croatie, Chypre, République tchèque, Danemark, Égypte, Estonie, Finlande, France, Allemagne, Grèce, Hongrie, Islande, Irlande, Israël, Italie, Jordanie, Kazakhstan, Kenya, Koweït, la Lettonie, Liban, Liechtenstein, Lituanie, Luxembourg, Macédoine du Nord, Malte, Monténégro, Maroc, pays-bas, Nigeria, Norvège, Oman, Pakistan, Pologne, Portugal, Qatar, Roumanie, Russie, Arabie saoudite, Serbie, Slovaquie, Slovénie, Afrique du Sud, Espagne, Suède, Suisse, Tunisie, Turquie, Ukraine, Émirats Arabes Unis et Royaume-Uni.

Les autres pays/régions sont en cours d’ajout à cette liste.  Pour le moment, vous pouvez toujours utiliser Azure Active Directory B2C en choisissant l’un des pays ci-dessus.

> Afghanistan, Argentine, Australie, Brésil, Chili, Colombie, Équateur, RAS de Hong Kong, Inde, Indonésie, Irak, Japon, Corée, Malaisie, Nouvelle-Zélande, Paraguay, Pérou, Philippines, Singapour, Sri Lanka, Taïwan, Thaïlande, Uruguay et Venezuela.

## <a name="preview-tenant"></a>Client de la version préliminaire
Si vous avez créé un client B2C pendant la période d’évaluation d’Azure AD B2C, il est probable que votre **type de client** indique **Client de la version préliminaire**. Si c’est le cas, vous DEVEZ utiliser votre client uniquement à des fins de développement et de test, mais PAS pour les applications de production.

> [!IMPORTANT]
> Il n’existe aucun chemin de migration à partir d’un client B2C de la version préliminaire vers un client B2C de mise à l’échelle pour production. Notez qu’il existe des problèmes connus liés à la suppression d’un client B2C en version préliminaire et à la recréation d’un client B2C de mise à l’échelle pour production portant le même nom de domaine. Vous devez créer un client B2C de mise à l’échelle pour production portant un nom de domaine différent.


![Capture d’écran d’un aperçu du client](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
