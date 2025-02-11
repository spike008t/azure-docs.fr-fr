---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 563849d3875ed0156d81770f58340633d90d515b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161278"
---
Voici les tailles des objets Azure qui peuvent être écrits. Assurez-vous que tous les fichiers chargés sont en conformité avec ces limites.

| Type d’objet Azure | Limite de chargement                                             |
|-------------------|-----------------------------------------------------------|
| Objet blob de blocs        | ~ 4,75 To                                                 |
| Objet blob de pages         | 1 To <br> Chaque fichier chargé au format d’objet blob de pages doit être de 512 octets alignés (un multiple entier), sinon le chargement échoue. <br> Les disques VHD et VHDX sont de 512 octets alignés. |
| Azure Files         | 1 To <br> Chaque fichier chargé au format d’objet blob de pages doit être de 512 octets alignés (un multiple entier), sinon le chargement échoue. <br> Les disques VHD et VHDX sont de 512 octets alignés. |

> [!IMPORTANT]
> La création de fichiers (quel que soit le type de stockage) est autorisée jusqu’à 5 To. Toutefois, si vous créez un fichier dont la taille est supérieure à la limite de chargement définie dans le tableau précédent, le fichier n’est pas chargé. Vous devez supprimer manuellement le fichier pour récupérer l’espace.