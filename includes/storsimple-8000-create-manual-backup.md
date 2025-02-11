---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 4fc92931979aa367bdead435c3d6fd758d66a397
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66118318"
---
#### <a name="to-create-a-manual-backup"></a>Création d’une sauvegarde manuelle

1. Accédez à votre service StorSimple Device Manager et cliquez sur **Appareils**. Dans la liste tabulaire des appareils, sélectionnez votre appareil. Accédez à **Paramètres > Gérer > Stratégies de sauvegarde**.

2. Le panneau **Stratégies de sauvegarde** répertorie toutes les stratégies de sauvegarde au format tabulaire, y compris la stratégie du volume que vous souhaitez sauvegarder. Sélectionnez la stratégie associée au volume que vous souhaitez sauvegarder et cliquez avec le bouton droit pour appeler le menu contextuel. Sélectionnez **Back up now** (Sauvegarder maintenant) dans la liste déroulante.

    ![Création d’une sauvegarde manuelle](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. Dans le panneau **Back up now** (Sauvegarder maintenant), procédez comme suit :

    1. Dans la liste déroulante, sélectionnez le **Type d'instantané** qui convient : instantané **Local** ou instantané **Cloud**. Sélectionnez une capture instantanée locale pour les restaurations ou sauvegardes rapides et une capture instantanée cloud pour la résilience des données.

        ![Création d’une sauvegarde manuelle](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. Cliquez sur **OK** pour lancer un travail de création d’une capture instantanée. Vous verrez une notification en haut de la page lorsque le travail aura été créé avec succès.

        ![Création d’une sauvegarde manuelle](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. Pour surveiller le travail, cliquez sur la notification. Vous serez redirigé vers le panneau **Travaux** où vous pouvez afficher la progression du travail.


5. Une fois le travail de sauvegarde terminé, accédez à l’onglet **Catalogue de sauvegarde** .

6. Définissez les sélections de filtre pour l’appareil approprié, la stratégie de sauvegarde et la plage horaire. La sauvegarde doit apparaître dans la liste des jeux de sauvegarde qui s’affiche dans le catalogue.

