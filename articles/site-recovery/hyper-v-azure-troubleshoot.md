---
title: Résoudre les problèmes de réplication de Hyper-V sur Azure avec Azure Site Recovery | Microsoft Docs
description: Décrit comment résoudre les problèmes de réplication de Hyper-V sur Azure à l’aide d’Azure Site Recovery
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 04/09/2018
ms.author: rayne
ms.openlocfilehash: 95a33c80b1aeef7fbf8bea0ab760bbd66babdac8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="troubleshoot-hyper-v-to-azure-replication-and-failover"></a>Résoudre les problèmes de réplication et de basculement de Hyper-V sur Azure

Cet article décrit les problèmes courants que vous pouvez rencontrer lors de la réplication de machines virtuelles Hyper-V locales sur Azure avec [Azure Site Recovery](site-recovery-overview.md).

## <a name="enable-protection-issues"></a>Problèmes lors de l’activation de la protection

Si vous rencontrez des problèmes lorsque vous activez la protection des machines virtuelles Hyper-V, vérifiez les points suivants :

1. Vérifiez que vos hôtes et vos machines virtuelles Hyper-V répondent à la [configuration requise et aux conditions préalables](hyper-v-azure-support-matrix.md).
2. Si les serveurs Hyper-V sont situés dans des clouds System Center Virtual Machine Manager (VMM), vérifiez que vous avez préparé le [serveur VMM](hyper-v-prepare-on-premises-tutorial.md#prepare-vmm-optional).
3. Vérifiez que le service de gestion d’ordinateurs virtuels de Hyper-V est en cours d’exécution sur les hôtes Hyper-V.
4. Recherchez les problèmes dans le journal de Hyper-V-VMMS\Admin sur la machine virtuelle. Ce fichier journal se trouve dans **Journaux des applications et services** > **Microsoft** > **Windows**.
5. Sur la machine virtuelle invitée, vérifiez que WMI est activé et accessible.
  - [En savoir plus sur](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) le test de base de WMI.
  - [Résolvez les problèmes](https://aka.ms/WMiTshooting) de WMI.
  - [Résolvez les problèmes](https://technet.microsoft.com/library/ff406382.aspx#H22) des scripts et services de WMI.
5. Sur la machine virtuelle invitée, vérifiez que la version d’Integration Services en cours d’exécution est la dernière en date.
    - [Vérifiez](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) que vous avez la version la plus récente.
    - [Conservez](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#keep-integration-services-up-to-date) Integration Services à jour.
    
## <a name="replication-issues"></a>Problèmes de réplication

Pour résoudre les problèmes liés à la réplication initiale et continue, procédez comme suit :

1. Vérifiez que vous exécutez la [version la plus récente](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx) des services Azure Site Recovery.
2. Vérifiez si la réplication est suspendue :
  - Vérifiez l’état d’intégrité de la machine virtuelle dans la console du Gestionnaire Hyper-V.
  - S’il est critique, cliquez avec le bouton droit de la souris sur la machine virtuelle, puis sélectionnez **Réplication** > **Afficher l’intégrité de la réplication**.
  - Si la réplication est suspendue, cliquez sur **Reprendre la réplication**.
3. Vérifiez que les services nécessaires sont en cours d’exécution. Si tel n’est pas le cas, redémarrez-les.
    - Si vous répliquez Hyper-V sans VMM, vérifiez que ces services sont en cours d’exécution sur l’hôte Hyper-V :
        - Service Gestion d’ordinateurs virtuels
        - Service Microsoft Azure Recovery Services Agent
        - Service Microsoft Azure Site Recovery
        - Service de l’hôte du fournisseur WMI
    - Si vous effectuez une réplication avec VMM dans l’environnement, vérifiez que ces services sont en cours d’exécution :
        - Sur l’hôte Hyper-V, vérifiez que le service de gestion d’ordinateurs virtuels, Microsoft Azure Recovery Services Agent et le service de l’hôte du fournisseur WMI sont en cours d’exécution.
        - Sur le serveur VMM, assurez-vous que le service System Center Virtual Machine Manager est en cours d’exécution.
4. Vérifiez la connectivité entre le serveur Hyper-V et Azure. Pour ce faire, ouvrez le Gestionnaire des tâches sur l’hôte Hyper V. Dans l’onglet **Performances**, puis cliquez sur **Ouvrir le Moniteur de ressources**. Dans l’onglet **Réseau** > **Processess with Network Activity (Processus avec activité réseau)**, vérifiez si cbengine.exe envoie des volumes importants (Mo) de données.
5. Vérifiez si les hôtes Hyper-V peuvent se connecter à l’URL du blob de stockage Azure. Pour ce faire, sélectionnez et vérifiez **cbengine.exe**. Affichez les **connexions TCP** pour vérifier la connectivité de l’hôte au blob de stockage Azure.
6. Vérifiez les problèmes de performance, comme indiqué ci-dessous.
    
### <a name="performance-issues"></a>Problèmes de performance

Les limitations de la bande passante du réseau peuvent affecter la réplication. Résolvez les problèmes comme suit :

1. [Vérifiez](https://support.microsoft.com/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) si la bande passante est contrainte ou limitée dans votre environnement.
2. Exécuter le profileur [Planificateur de déploiement](hyper-v-deployment-planner-run.md).
3. Après avoir exécuté le profileur, suivez les recommandations en matière de [bande passante](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) et de [stockage](hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation).
4. Vérifiez les [limitations de l’activité des données](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits). Si vous constatez une activité importante des données sur une machine virtuelle, procédez comme suit :
  - Vérifiez si votre machine virtuelle est concernée par la resynchronisation.
  - Procédez comme [suit](https://blogs.technet.microsoft.com/virtualization/2014/02/02/hyper-v-replica-debugging-why-are-very-large-log-files-generated/) pour déterminer la raison d’une telle activité.
  - Elle peut survenir lorsque les fichiers journaux de HRL dépassent 50 % de l’espace disque disponible. Si tel est le cas, configurez davantage d’espace de stockage pour toutes les machines virtuelles présentant le problème.
  - Vérifiez que la réplication n’est pas suspendue. Si tel est le cas, elle continue de modifier le fichier hrl, ce qui peut contribuer à augmenter sa taille.
 

## <a name="critical-replication-state-issues"></a>Problèmes d’état de réplication critique

1. Pour vérifier l’intégrité de la réplication, connectez-vous à la console du Gestionnaire Hyper-V local, sélectionnez la machine virtuelle et vérifiez son intégrité.

    ![Intégrité de la réplication](media/hyper-v-azure-troubleshoot/replication-health1.png)
    

2. Cliquez sur **Afficher l’intégrité de la réplication** pour afficher les détails :

    - Si la réplication est suspendue, cliquez avec le bouton droit de la souris sur la machine virtuelle, puis sélectionnez **Réplication** > **Reprendre la réplication**.
    - Si une machine virtuelle sur un hôte Hyper-V configuré dans Azure Site Recovery est migrée vers un autre hôte Hyper-V du même cluster ou d’une machine autonome, sa réplication n’est pas affectée. Vérifiez simplement que le nouvel hôte Hyper-V répond à toutes les conditions préalables et qu’il est configuré dans Azure Site Recovery.

## <a name="app-consistent-snapshot-issues"></a>Problèmes de capture instantanée de cohérence des applications

Une capture instantanée de cohérence des applications est un instantané à un instant T des données des applications à l’intérieur de la machine virtuelle. Le service VSS s’assure que les applications sur la machine virtuelle sont dans un état cohérent au moment de la capture instantanée.  Cette section décrit certains problèmes courants que vous pouvez rencontrer.

### <a name="vss-failing-inside-the-vm"></a>Échec de VSS à l’intérieur de la machine virtuelle

1. Vérifiez que la version d’Integration Services installée et en cours d’exécution est la plus récente.  Vérifiez si une mise à jour est disponible en exécutant la commande suivante dans une invite PowerShell avec élévation de privilèges sur l’hôte Hyper-V : **get-vm | select Name, State, IntegrationServicesState**.
2. Vérifiez que les services VSS sont en cours d’exécution et intègres :
    - Pour ce faire, ouvrez une session sur la machine virtuelle invitée. Puis, ouvrez une invite de commandes administrateur et exécutez les commandes suivantes pour vérifier si tous les enregistreurs VSS sont intègres.
        - **Enregistreurs de liste Vssadmin**
        - **Clichés instantanés de liste Vssadmin**
        - **Fournisseurs de listes vssadmin**
    - Vérifiez la sortie. Si les enregistreurs sont dans un état d’échec, procédez comme suit :
        - Recherchez les dysfonctionnements de VSS dans le journal des événements d’application.
    - Essayez de redémarrer ces services associés à l’enregistreur en état d’échec :
        - Cliché instantané de volume
         - Fournisseur VSS d’Azure Site Recovery
    - Ensuite, attendez quelques heures pour voir si les captures instantanées de cohérence des applications sont bien générées.
    - En dernier recours, essayez de redémarrer la machine virtuelle. Cela peut résoudre les services qui ne répondent pas.
3. Vérifiez que vous n’avez pas de disques dynamiques dans la machine virtuelle. Les captures instantanées de cohérence des applications ne les prennent pas en charge. Vérifiez dans Gestion des disques (diskmgmt.msc).

    ![Disque dynamique](media/hyper-v-azure-troubleshoot/dynamic-disk.png)
    
4. Vérifiez que vous n’avez pas un disque iSCSI attaché à la machine virtuelle. Ceci n’est pas pris en charge.
5. Vérifiez que le service Sauvegarde Microsoft Azure est activé. Pour ce faire, sélectionnez **Paramètres Hyper-V** > **Integration Services**.
6. Vérifiez qu’il n’y a aucun conflit avec les applications qui prennent des captures instantanées VSS. Si plusieurs applications essaient de prendre des captures instantanées VSS en même temps, des conflits peuvent survenir. Par exemple, si une application Sauvegarde Microsoft Azure prend des captures instantanées VSS lorsque votre stratégie de réplication lui indique de le faire.   
7. Vérifiez si la machine virtuelle affiche un taux d’activité élevé :
    - Vous pouvez mesurer le taux quotidien de modification de données sur les machines virtuelles invitées, à l’aide des compteurs de performance de l’hôte Hyper-V. Pour ce faire, activez le compteur suivant. Cumulez un échantillon de cette valeur sur les disques de la machine virtuelle pendant 5 à 15 minutes, pour obtenir l’activité de la machine virtuelle.
        - Catégorie : « Dispositif de stockage virtuel Hyper-V »
        - Compteur : « Octets écrits/s »</br>
        - Ce taux d’activité des données va augmenter ou se maintenir à un niveau élevé, selon le niveau d’activité de la machine virtuelle ou de ses applications.
        - L’activité moyenne des données du disque source est de 2 Mo/s pour le stockage standard d’Azure Site Recovery. [En savoir plus](hyper-v-deployment-planner-analyze-report.md#azure-site-recovery-limits)
    - En outre, vous pouvez [vérifier les objectifs d’évolutivité du stockage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets.md#scalability-targets-for-a-storage-account).
8. Exécutez le [Planificateur de déploiement](hyper-v-deployment-planner-run.md).
9. Passez en revue les recommandations pour le [réseau](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input) et le [stockage](hyper-v-deployment-planner-analyze-report.md#recommendations-with-available-bandwidth-as-input).


### <a name="vss-failing-inside-the-hyper-v-host"></a>Échec de VSS à l’intérieur de l’hôte Hyper-V

1. Recherchez les erreurs et recommandations pour VSS dans les journaux d’événements :
    - Sur le serveur hôte Hyper-V, ouvrez le journal des événements de l’administrateur Hyper-V dans **Observateur d’événements** > **Journaux des applications et des services** > **Microsoft**  >  **Windows** > **Hyper-V** > **Admin**.
    - Vérifiez si des événements signalent des défaillances des captures instantanées de cohérence des applications.
    - Voici une erreur typique : « Hyper-V failed to generate VSS snapshot set for virtual machine 'XYZ': The writer experienced a non-transient error. Restarting the VSS service might resolve issues if the service is unresponsive. (Hyper-V n’a pas pu générer l’ensemble de captures instantanées VSS pour la machine virtuelle 'XYZ' : L’enregistreur a rencontré une erreur non transitoire. Le redémarrage du service VSS peut résoudre le problème si le service ne répond pas.) »

2. Pour générer des captures instantanées VSS pour la machine virtuelle, vérifiez que les services d’intégration de Hyper-V sont installés sur la machine virtuelle et que le service d’intégration de sauvegarde (VSS) est activé.
    - Assurez-vous que le service VSS/les démons des services d’intégration sont en cours d’exécution sur l’invité et qu’ils sont dans un état **OK**.
    - Pour le vérifier, ouvrez une session PowerShell avec élévation de privilèges sur l’hôte Hyper-V et exécutez la commande **et-VMIntegrationService -VMName<VMName>-Name VSS**. Vous pouvez également obtenir ces informations en vous connectant à la machine virtuelle invitée. [Plus d’informations](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services)
    - Assurez-vous que les services d’intégration Sauvegarde Microsoft Azure/VSS sur la machine virtuelle sont en cours d’exécution et intègres. Si tel n’est pas le cas, redémarrez ces services et le service Requête du service VSS Microsoft Hyper-V sur le serveur hôte Hyper-V.

### <a name="common-errors"></a>Erreurs courantes

**Code d’erreur** | **Message** | **Détails**
--- | --- | ---
**0x800700EA** | « Hyper-V failed to generate VSS snapshot set for virtual machine: More data is available. (Hyper-V n’a pas pu générer l’ensemble de captures instantanées VSS pour la machine virtuelle : d’autres données sont disponibles.) (0x800700EA). VSS snapshot set generation can fail if backup operation is in progress. (La génération du jeu de captures instantanées VSS peut échouer si l’opération de sauvegarde est en cours.)<br/><br/> Replication operation for virtual machine failed: More data is available. (L’opération de réplication de la machine virtuelle a échoué : d’autres données sont disponibles.) » | Vérifiez si votre machine virtuelle possède un disque dynamique activé. Ceci n’est pas pris en charge.
**0x80070032** | « Hyper-V Volume Shadow Copy Requestor failed to connect to virtual machine <./VMname> because the version does not match the version expected by Hyper-V (Requête du service VSS Microsoft Hyper-V n’a pas pu se connecter à la machine virtuelle <./nom_MV>, car la version ne correspond pas à la version attendue par Hyper-V) | Vérifiez si les dernières mises à jour de Windows sont installées.<br/><br/> [Mettez à niveau](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services.md#keep-integration-services-up-to-date) Integration Services vers la dernière version en date.



## <a name="collect-replication-logs"></a>Collecter les journaux de réplication

Tous les événements relatifs à la réplication Hyper-V sont consignés dans le journal Hyper-V-VMMS\Admin situé dans **Journaux des applications et des services** > **Microsoft** > **Windows**. En outre, vous pouvez activer un journal d’analyse pour le Service de gestion d’ordinateurs virtuels Hyper-V, comme suit :

1. Rendez les journaux d’analyse et de débogage consultables dans l’Observateur d’événements. Pour ce faire, dans l’Observateur d’événements, cliquez sur **Affichage** > **Afficher les journaux d’analyse et de débogage**. Le journal d’analyse s’affiche sous **Hyper-V-VMMS**.
2. Dans le volet **Actions**, cliquez sur **Activer le journal**. 

    ![Activer le journal](media/hyper-v-azure-troubleshoot/enable-log.png)
    
3. Une fois activé, le journal apparaît dans **Analyseur de performances** comme une **Session de suivi d’événements** sous **Ensembles de collecteurs de données**. 
4. Pour afficher les informations collectées, arrêtez la session de suivi en désactivant le journal. Ensuite, enregistrez le journal et rouvrez-le dans l’Observateur d’événements ou utilisez d’autres outils pour le convertir au format souhaité.


### <a name="event-log-locations"></a>Emplacements des journaux d’événements

**Journal des événements** | **Détails** |
--- | ---
**Journaux des applications et des services/Microsoft/VirtualMachineManager/Server/Admin** (serveur VMM) | Journaux pour résoudre les problèmes de VMM.
**Journaux des applications et des services/MicrosoftAzureRecoveryServices/Replication** (hôte Hyper-V) | Journaux pour résoudre les problèmes liés à Microsoft Azure Recovery Services Agent. 
**Journaux des applications et des services/Microsoft/Azure Site Recovery/Provider/Operational** (hôte Hyper-V)| Journaux pour résoudre les problèmes liés à Microsoft Azure Site Recovery Service.
**Journaux des applications et des services/Microsoft/Windows/Hyper-V-VMMS/Admin** (hôte Hyper-V) | Journaux pour résoudre les problèmes de gestion d’ordinateurs virtuels Hyper-V.

### <a name="log-collection-for-advanced-troubleshooting"></a>Collection de journaux pour la résolution de problèmes avancée

Ces outils peuvent aider à résoudre des problèmes plus complexes :

-   Pour VMM, collectez les journaux de récupération de site à l’aide de l’[outil SDP (Support Diagnostics Platform)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx).
-   Pour Hyper-V sans VMM, [téléchargez cet outil](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) et exécutez-le sur l’hôte Hyper-V afin de collecter les journaux.
