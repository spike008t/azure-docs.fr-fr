---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: d5af557a62f4bd35c242d334c28a38c3d632f7cf
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66161167"
---
1. [Se connecter à l’interface PowerShell](#connect-to-the-powershell-interface).
2. Utilisez le `Get-HcsApplianceInfo` pour obtenir les informations de votre appareil.

    L’exemple suivant illustre l’utilisation de cette applet de commande :

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Voici un tableau récapitulant les informations sur les périphérique important :
    
    | Paramètre                             | Description                                                                                                                                                   |   |
    |--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    | FriendlyName                   | Le nom convivial de l’appareil, tel que configuré via l’interface utilisateur web locale pendant le déploiement de l’appareil. Le nom convivial par défaut est le numéro de série de l’appareil.  |   |
    | SerialNumber                   | Le numéro de série du périphérique est un numéro unique attribué en usine.                                                                             |   |
    | Modèle                          | Le modèle pour votre appareil Edge de zone de données ou de la passerelle de zone de données. Le modèle est virtuel pour la passerelle de données boîte et physiques pour Edge de zone de données.                   |   |
    | FriendlySoftwareVersion        | Chaîne conviviale qui correspond à la version du logiciel de l’appareil. Pour un système exécutant la version préliminaire, la version conviviale du logiciel serait 1902 de Edge de zone de données. |   |
    | HcsVersion                     | Version du logiciel HCS exécutée sur votre appareil. Par exemple, la version du logiciel HCS correspondant aux données boîte Edge 1902 est 1.4.771.324.            |   |
    | LocalCapacityInMb              | La capacité totale locale de l’appareil en mégabits.                                                                                                        |   |
    | IsRegistered                   | Cette valeur indique si votre appareil est activé avec le service.                                                                                         |   |


