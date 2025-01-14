---
title: Utilisation du service Azure Import/Export pour transférer des données dans des objets blob Azure | Microsoft Docs
description: Découvrez comment créer des tâches d’importation et d’exportation dans le portail Azure pour transférer des données vers et à partir d’objets blob Azure.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/31/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 68f62a6945f3b651781414e3194104b6d2e6295c
ms.sourcegitcommit: ec7b0bf593645c0d1ef401a3350f162e02c7e9b8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455808"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>Utiliser le service Azure Import/Export pour transférer des données dans le Stockage Blob Azure

Cet article fournit des instructions détaillées sur l’utilisation du service Azure Import/Export pour importer de manière sécurisée de grandes quantités de données dans le Stockage Blob Azure. Pour importer des données dans des objets blob Azure, le service vous demande d’expédier à un centre de données Azure des lecteurs de disque chiffrés contenant vos données.  

## <a name="prerequisites"></a>Conditions préalables

Avant de créer une tâche d’importation pour transférer des données dans le Stockage Blob Azure, passez soigneusement en revue et complétez la liste suivante des prérequis pour ce service. Vous devez respecter les consignes suivantes :

- Avoir un abonnement Azure actif utilisable avec le service Import/Export
- Avoir au moins un compte de stockage Azure avec un conteneur de stockage. Consultez la liste des [Comptes de stockage et types de stockage pris en charge pour le service Import/Export](storage-import-export-requirements.md). 
    - Pour plus d'informations sur la création d'un compte de stockage, consultez la page [Création d'un compte de stockage](storage-quickstart-create-account.md). 
    - Pour plus d’informations sur le conteneur de stockage, accédez à [Créer un conteneur de stockage](../blobs/storage-quickstart-blobs-portal.md#create-a-container).
- Avoir un nombre suffisant de disques de [Types pris en charge](storage-import-export-requirements.md#supported-disks). 
- Avoir un système Windows exécutant une [Version de système d’exploitation prise en charge](storage-import-export-requirements.md#supported-operating-systems). 
- Activez BitLocker sur le système Windows. Consultez [Comment activer BitLocker](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
- [Téléchargez la version 1 de WAImportExport](https://aka.ms/waiev1) sur le système Windows. Décompressez le package dans le dossier par défaut : `waimportexportv1`. Par exemple : `C:\WaImportExportV1`.
- Dotez-vous d’un compte FedEx/DHL. Si vous souhaitez utiliser un opérateur autre que FedEx/DHL, contactez l’équipe des opérations de zone de données Azure à `adbops@microsoft.com`.  
    - Le compte doit être valide, doit avoir un solde et doit offrir des fonctionnalités de réexpédition.
    - Générez un numéro de suivi pour le travail d’exportation.
    - Chaque travail doit avoir un numéro de suivi distinct. Plusieurs travaux portant le même numéro de suivi ne sont pas pris en charge.
    - Si vous n’avez pas de compte de transporteur, accédez à :
        - [Créer un compte FedEX](https://www.fedex.com/en-us/create-account.html), ou 
        - [Créer un compte DHL](http://www.dhl-usa.com/en/express/shipping/open_account.html).

## <a name="step-1-prepare-the-drives"></a>Étape 1 : Préparer les lecteurs

Cette étape génère un fichier journal. Le fichier journal stocke les informations de base comme le numéro de série du lecteur, la clé de chiffrement et les détails du compte de stockage. 

Effectuez les étapes suivantes pour préparer les lecteurs.

1.  Connectez vos lecteurs de disque au système Windows via des connecteurs SATA.
1.  Créez un seul volume NTFS sur chaque lecteur. Attribuez une lettre de lecteur au volume. N’utilisez pas de points de montage.
2.  Activez le chiffrement BitLocker sur le volume NTFS. Si vous utilisez un système Windows Server, suivez les instructions de [How to enable BitLocker sur Windows Server 2012 R2](https://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/) (Comment activer BitLocker sur Windows Server 2012 R2).
3.  Copiez des données sur le volume chiffré. Utilisez la fonction glisser-déplacer, Robocopy ou n’importe quel outil de copie de ce type.
4.  Ouvrez une fenêtre PowerShell ou de ligne de commande avec des privilèges d’administrateur. Pour changer le répertoire du dossier décompressé, exécutez la commande suivante :
    
    `cd C:\WaImportExportV1`
5.  Pour obtenir la clé BitLocker du lecteur, exécutez la commande suivante :
    
    `manage-bde -protectors -get <DriveLetter>:`
6.  Pour préparer le disque, exécutez la commande suivante. **Selon la taille des données, l’opération peut durer plusieurs heures, voire plusieurs jours.** 

    ```
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session#<session number> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /blobtype:<BlockBlob or PageBlob> /skipwrite /enablecontentmd5 
    ```
    Un fichier journal est créé dans le même dossier où vous avez exécuté l’outil. Deux autres fichiers sont également créés : un fichier *.xml* (dossier où vous exécutez l’outil) et un fichier *drive-manifest.xml* (dossier où se trouvent les données).
    
    Les paramètres utilisés sont décrits dans le tableau suivant :

    |Option  |Description  |
    |---------|---------|
    |/j:     |Nom du fichier journal, avec l’extension .jrn. Un fichier journal est généré par lecteur. Nous vous recommandons d’utiliser le numéro de série du disque comme nom de fichier journal.         |
    |/id:     |ID de la session. Utilisez un numéro de session unique pour chaque instance de la commande.      |
    |/t:     |Lettre de lecteur du disque à expédier. Exemple : lecteur `D`.         |
    |/bk:     |Clé BitLocker du lecteur. Son mot de passe numérique à partir de la sortie de `manage-bde -protectors -get D:`      |
    |/srcdir:     |Lettre de lecteur du disque à expédier suivie de `:\`. Par exemple : `D:\`.         |
    |/dstdir:     |Nom du conteneur de destination dans le Stockage Azure.         |
    |/blobtype:     |Cette option spécifie le type d’objets BLOB que vous souhaitez importer les données. Objets BLOB de blocs, il s’agit de `BlockBlob` et objets BLOB de pages, il est `PagaBlob`.         |
    |/skipwrite:     |Option qui spécifie qu’aucune nouvelle donnée ne doit être copiée et que les données existantes sur le disque doivent être préparées.          |
    |/enablecontentmd5:     |L’option lors de l’option est activée, permet de s’assurer que MD5 est calculé lors du téléchargement d’objets BLOB de blocs vers Azure.          |
7. Répétez l’étape précédente pour chaque disque à expédier. Un fichier journal avec le nom fourni est créé pour chaque exécution de la ligne de commande.
    
    > [!IMPORTANT]
    > - En plus du fichier journal, un fichier `<Journal file name>_DriveInfo_<Drive serial ID>.xml` est également créé dans le même dossier où se trouve l’outil. Le fichier .xml est utilisé à la place du fichier journal quand vous créez une tâche si le fichier journal est trop volumineux. 

## <a name="step-2-create-an-import-job"></a>Étape 2 : Créer une tâche d’importation

Effectuez les étapes suivantes pour créer une tâche d’importation dans le portail Azure.

1. Connectez-vous sur https://portal.azure.com/.
2. Accédez à **Tous les services > Stockage > Tâches d’importation/exportation**. 
    
    ![Accéder à Tâches d’importation/exportation](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Cliquez sur **Créer une tâche d’importation/exportation**.

    ![Cliquez sur Créer une tâche d’importation/exportation](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. Dans **De base** :

   - Sélectionnez **Importer dans Azure**.
   - Indiquez un nom décrivant le travail d’importation. Utilisez le nom pour suivre la progression de vos tâches.
       - Le nom ne peut contenir que des lettres minuscules, des chiffres et des traits d’union.
       - Le nom doit commencer par une lettre et ne doit pas contenir d’espaces.
   - Sélectionnez un abonnement.
   - Entrez ou sélectionnez un groupe de ressources.  

     ![Créer une tâche d’importation - Étape 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. Dans **Détails de la tâche** :

    - Chargez les fichiers journaux du lecteur que vous avez obtenus à l’étape de préparation de lecteur. Si vous avez utilisé `waimportexport.exe version1`, chargez un fichier pour chaque lecteur préparé. Si la taille du fichier journal dépasse 2 Mo, vous pouvez utiliser `<Journal file name>_DriveInfo_<Drive serial ID>.xml` également créé avec le fichier journal. 
    - Sélectionnez le compte de stockage de destination des données. 
    - L’emplacement de remise est automatiquement rempli en fonction de la région du compte de stockage sélectionné.
   
   ![Créer une tâche d’importation - Étape 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. Dans **Informations de réexpédition** :

   - Sélectionnez le transporteur dans la liste déroulante. Si vous souhaitez utiliser un opérateur autre que FedEx/DHL, choisissez une option existante dans la liste déroulante. Opérations de boîte de données Azure contact de l’équipe à `adbops@microsoft.com` avec les informations concernant le transporteur que vous prévoyez d’utiliser.
   - Entrez un numéro de compte de transporteur valide que vous avez créé pour ce transporteur. Microsoft utilise ce compte pour renvoyer les lecteurs une fois la tâche d’importation terminée. Si vous n’avez pas de numéro de compte, créez un compte de transporteur [FedEx](https://www.fedex.com/us/oadr/) ou [DHL](https://www.dhl.com/).
   - Indiquez le nom d’un contact, le numéro de téléphone, l’e-mail, l’adresse, la ville, le code postal, l’état/la province et le pays/la région, puis vérifiez que ces informations sont complètes et valides. 
        
       > [!TIP] 
       > Au lieu de spécifier une adresse de messagerie pour un seul utilisateur, fournissez une adresse de groupe. Cela garantit que vous recevrez des notifications même si un administrateur s’en va.

     ![Créer une tâche d'importation - Étape 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)
   
5. Dans le **Récapitulatif** :

   - Passez en revue les informations de tâche dans le récapitulatif. Notez le nom de la tâche et l’adresse du centre de données Azure pour réexpédier les disques à Azure. Ces informations sont utilisées par la suite sur l’étiquette d’expédition.
   - Cliquez sur **OK** pour créer la tâche d’importation.

     ![Créer une tâche d’importation - Étape 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives"></a>Étape 3 : Expédier les disques 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]


## <a name="step-4-update-the-job-with-tracking-information"></a>Étape 4 : Mettre à jour la tâche avec les informations de suivi

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>Étape 5 : Vérifier le chargement des données dans Azure

Surveillez le travail jusqu’à son achèvement. Une fois le travail terminé, vérifiez que vos données ont été chargées sur Azure. Ne supprimez les données locales qu’après avoir vérifié que le chargement a réussi.

## <a name="next-steps"></a>Étapes suivantes

* [Voir l’état de la tâche et des disques](storage-import-export-view-drive-status.md)
* [Passer en revue les exigences d’importation/exportation](storage-import-export-requirements.md)


