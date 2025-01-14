---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d6bad1ec584b28fee77bc654eba8a2d0c7b5df30
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66118298"
---
#### <a name="to-add-a-storage-account-credential-in-the-same-azure-subscription-as-the-storsimple-device-manager-service"></a>Pour ajouter une information d’identification de compte de stockage dans le même abonnement Azure que le service StorSimple Device Manager

1. Accédez à votre service StorSimple Device Manager. Dans la section **Configuration**, cliquez sur **Informations d’identification du compte de stockage**.

    ![Informations d’identification du compte de stockage](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct1.png)

2. Dans le panneau **Informations d’identification du compte de stockage**, cliquez sur **+ Ajouter**.

    ![Ajout des informations d’identification de compte de stockage](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct2.png)

3. Dans le panneau **Ajouter les informations d’identification d’un compte de stockage**, procédez comme suit :

    1. Lorsque vous ajoutez une information d’identification d’un compte de stockage dans le même abonnement Azure que votre service, vérifiez que **Actuel** est sélectionné.

    2. À partir de la liste déroulante **Compte de stockage**, sélectionnez un compte de stockage existant.

    3. En fonction du compte de stockage sélectionné, **l’emplacement** s’affiche (apparaît en grisé et ne peut pas être modifié ici).

    4. Sélectionnez **Activer le mode SSL** afin de créer un canal sécurisé pour la communication réseau entre votre appareil et le cloud. Désactivez **Activer SSL** uniquement si vous évoluez au sein d’un cloud privé.

        ![Panneau d’ajout des informations d’identification de compte de stockage](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct3.png)

    5. Cliquez sur **Ajouter** pour démarrer la création du travail pour les informations d’identification du compte de stockage. Un message s’affiche pour vous informer de la création des informations d’identification du compte de stockage.

        ![Notification de succès pour les informations d’identification du compte de stockage](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct5.png)

Les informations d’identification du compte de stockage nouvellement créées seront affichées dans la liste **Informations d’identification du compte de stockage**.

![Liste d’informations d’identification de compte de stockage](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct6.png)

