---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 48a3326dbe0e9eed4a5490e720248555586d189c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66118264"
---
### <a name="to-take-a-backup"></a>Pour effectuer une sauvegarde

1. Accédez à votre service StorSimple Device Manager. Sélectionnez votre appareil et cliquez dessus dans la liste tabulaire des appareils, puis cliquez sur **Tous les paramètres**. Dans le panneau **Paramètres**, accédez à **Paramètres > Gérer > Stratégie de sauvegarde**.

    ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu1.png)

2. Dans le panneau **Stratégie de sauvegarde**, cliquez sur **+ Ajouter une stratégie**.

    ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu2.png)

3. Spécifiez un nom comprenant entre 3 et 150 caractères pour votre stratégie de sauvegarde dans le panneau **Créer une stratégie de sauvegarde**.

4. Sélectionnez les volumes à sauvegarder. Si vous sélectionnez plusieurs volumes, ceux-ci sont regroupés pour créer une sauvegarde afin d’assurer la cohérence des données.

    ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu4.png)

5. Dans le panneau **Ajouter la première planification** :

    1. Sélectionnez le type de sauvegarde. Pour une restauration plus rapide, sélectionnez Instantané **local**. Pour la résilience des données, sélectionnez Instantané **cloud**.
    2. Spécifiez la fréquence de sauvegarde en minutes, heures, jours ou semaines.
    3. Sélectionnez une durée de conservation. La durée de conservation dépend de la fréquence de sauvegarde. Par exemple, si vous sélectionnez une stratégie quotidienne, vous pouvez spécifier une durée de conservation en semaines, alors que pour une stratégie mensuelle, la durée de conservation est en mois.
    4. Sélectionnez l’heure et la date de début de la stratégie de sauvegarde.
    5. Cliquez sur **OK** pour créer la stratégie de sauvegarde.

        ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. Cliquez sur **Créer** pour démarrer la création de la stratégie de sauvegarde. Un message s’affiche une fois la stratégie de sauvegarde créée. La liste des stratégies de sauvegarde est également mise à jour.
      
      ![Add-backup-policy](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      Vous disposez maintenant d’une stratégie de sauvegarde, qui crée des sauvegardes planifiées des données de volume.




