---
title: Fichier Include
description: Fichier Include
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.openlocfilehash: 31ccf14c82f6248c74d6af932fe9e338d26d2747
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66157243"
---
Chaque ordinateur client que vous connectez à un réseau virtuel avec une connexion point à site doit avoir un certificat client installé. Ce certificat doit être généré à partir du certificat racine, puis installé sur chaque ordinateur client. Si vous n’installez pas de certificat client valide, l’authentification échoue lorsque le client essaie de se connecter au réseau virtuel.

Vous pouvez soit générer un certificat unique pour chaque client, soir utiliser le même certificat pour plusieurs clients. Le fait de générer des certificats clients uniques vous offre la possibilité de révoquer un seul certificat. Dans le cas contraire, si plusieurs clients utilisent le même certificat client pour s’authentifier et que vous révoquez ce dernier, vous devrez générer et installer de nouveaux certificats pour chaque client qui utilise ce certificat.

Vous pouvez générer des certificats clients à l’aide des méthodes suivantes :

- **Certificat d’entreprise :**

  - Si vous utilisez une solution de certificat d’entreprise, générez un certificat de client avec le format de valeur de nom commun *nom\@votredomaine.com*. Utilisez ce format au lieu du format *domain name\username*.
  - Assurez-vous que le certificat client repose sur un modèle de certificat utilisateur qui indique *Authentification client* comme premier élément dans la liste d’utilisateurs. Vérifiez le certificat en double-cliquant dessus et en affichant **Utilisation avancée de la clé** dans l’onglet **Détails**.

- **Certificat racine auto-signé :** Suivez la procédure décrite dans l’un des articles concernant les certificats P2S ci-dessous pour créer des certificats clients compatibles avec vos connexions P2S. Les procédures décrites dans les articles permettent de générer un certificat client compatible : 

  * [Instructions pour PowerShell sous Windows 10](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert) : Ces instructions requièrent Windows 10 et PowerShell pour générer des certificats. Les certificats qui sont générés peuvent être installés sur n’importe quel client P2S pris en charge.
  * [Instructions pour MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) : Utilisez MakeCert si vous n’avez pas accès à un ordinateur Windows 10 pour générer des certificats. Même si MakeCert est déconseillé, vous pouvez toujours l’utiliser pour générer des certificats. Vous pouvez installer les certificats générés sur n’importe quel client P2S pris en charge.
  * [Instructions Linux](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)

  Lorsque vous générez un certificat client à partir d’un certificat racine auto-signé, ce certificat est automatiquement installé sur l’ordinateur que vous avez utilisé pour le générer. Si vous souhaitez installer un certificat client sur un autre ordinateur client, exportez-le en tant que fichier .pfx, avec l’intégralité de la chaîne du certificat. Cette opération crée un fichier .pfx contenant les informations de certificat racine requises pour l’authentification du client. 

**Pour exporter le certificat**

Pour savoir comment exporter un certificat, consultez l’article [Générer et exporter des certificats pour les connexions de point à site à l’aide de PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).
