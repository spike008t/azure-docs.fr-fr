---
title: Réplicas en lecture dans Azure Database pour PostgreSQL - serveur unique
description: Cet article décrit la fonctionnalité de réplica en lecture dans Azure Database pour PostgreSQL - serveur unique.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 75a3c8a9912fe9ace70e411983996167da755128
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734650"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>Réplicas en lecture dans Azure Database pour PostgreSQL - serveur unique

La fonctionnalité de réplica en lecture vous permet de répliquer les données d’un serveur Azure Database pour PostgreSQL sur un serveur en lecture seule. Vous pouvez répliquer à partir du serveur maître à jusqu'à cinq réplicas. Les réplicas sont mis à jour de manière asynchrone à l’aide de la technologie de réplication native du moteur PostgreSQL.

> [!IMPORTANT]
> Vous pouvez créer un réplica en lecture dans la même région que votre serveur principal, ou dans n’importe quelle autre région Azure de votre choix. La réplication entre les régions est actuellement en version préliminaire publique.

Les réplicas sont de nouveaux serveurs que vous gérez de manière similaire aux serveurs Azure Database pour PostgreSQL classiques. Pour chaque réplica en lecture, vous êtes facturé en fonction de la capacité de calcul provisionnée dans les vCores et du stockage provisionné en Go/mois.

Découvrez comment [créer et gérer des réplicas](howto-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Quand utiliser un réplica en lecture
La fonctionnalité de réplica en lecture contribue à améliorer les performances et la scalabilité des charges de travail nécessitant une lecture intensive. Les charges de travail de lecture peuvent être isolées sur les réplicas, tandis que les charges de travail d’écriture peuvent être dirigées vers le serveur maître.

Un scénario courant est d’avoir les charges de travail décisionnelles et analytiques qui utilisent le réplica en lecture comme source de données pour la création de rapports.

Dans la mesure où les réplicas sont en lecture seule, ils ne réduisent pas directement les charges relatives à la capacité d’écriture sur le serveur maître. Cette fonctionnalité ne cible pas les charges de travail nécessitant une écriture intensive.

La fonctionnalité de réplica en lecture utilise la réplication asynchrone PostgreSQL. La fonctionnalité n’est pas destinée aux scénarios de réplication synchrone. Il y aura un délai mesurable entre le maître et le réplica. Les données du réplica finissent par devenir cohérentes avec les données du serveur maître. Utilisez cette fonctionnalité pour les charges de travail pouvant s’adapter à ce délai.

Les réplicas en lecture peuvent améliorer votre plan de récupération d’urgence. Vous devez d’abord avoir un réplica dans une autre région Azure à partir du maître. En l’absence d’un incident de région, vous pouvez arrêter la réplication vers ce réplica et rediriger votre charge de travail vers elle. Arrêt de la réplication permet au réplica commencer à accepter les écritures, ainsi que lit. En savoir plus, consultez le [arrêter la réplication](#stop-replication) section. 

## <a name="create-a-replica"></a>Créer un réplica
Le serveur maître doit comporter le paramètre `azure.replication_support` avec la valeur **REPLICA**. Quand vous changez ce paramètre, un redémarrage du serveur est nécessaire pour que la modification soit prise en compte. (Le paramètre `azure.replication_support` s’applique uniquement aux niveaux à usage général et à mémoire optimisée).

Quand vous démarrez le workflow de création de réplica, un serveur Azure Database pour PostgreSQL vide est créé. Le nouveau serveur est rempli avec les données qui se trouvaient sur le serveur maître. Le temps de création dépend de la quantité de données présentes sur le serveur maître et du temps écoulé depuis la dernière sauvegarde complète hebdomadaire. Le temps nécessaire peut aller de quelques minutes à plusieurs heures.

Chaque réplica est activé pour le stockage [la croissance automatique](concepts-pricing-tiers.md#storage-auto-grow). La fonctionnalité de croissance automatique permet au réplica de suivre les données répliquées sur elle et empêcher une interruption dans la réplication a provoqué par des erreurs de stockage.

La fonctionnalité de réplica en lecture utilise la réplication physique PostgreSQL, et non la réplication logique. Le streaming de réplication à l’aide des slots de réplication est le mode de fonctionnement par défaut. Quand cela est nécessaire, la copie des journaux de transaction permet de rattraper le retard.

Découvrez comment [créer un réplica en lecture dans le portail Azure](howto-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Se connecter à un réplica
Quand vous créez un réplica, il n’hérite pas des règles de pare-feu ni du point de terminaison de service VNet (réseau virtuel) du serveur maître. Ces règles doivent être configurés indépendamment pour le réplica.

Le réplica hérite du compte Administrateur du serveur maître. Tous les comptes d’utilisateur sur le serveur maître sont répliqués sur les réplicas en lecture. Vous pouvez uniquement vous connecter à un réplica en lecture à l’aide des comptes d’utilisateur disponibles sur le serveur maître.

Vous pouvez vous connecter au réplica à l’aide de son nom d’hôte et d’un compte d’utilisateur valide, comme vous le faites sur un serveur Azure Database pour PostgreSQL classique. Pour un serveur nommé **mon réplica** avec le nom d’utilisateur administrateur **myadmin**, vous pouvez vous connecter au réplica à l’aide de psql :

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

À l’invite, entrez le mot de passe du compte d’utilisateur.

## <a name="monitor-replication"></a>Superviser la réplication
Azure Database pour PostgreSQL fournit deux mesures de surveillance de la réplication. Les deux mesures sont **Nb max. de décalage entre les réplicas** et **réplica Lag**. Pour savoir comment afficher ces mesures, consultez la **surveiller un réplica** section de la [lire l’article sur les procédures de réplica](howto-read-replicas-portal.md).

Le **Nb max. de décalage entre les réplicas** métrique affiche le décalage en octets entre le maître et le réplica de la plupart-en retard. Cette métrique est disponible sur le serveur maître uniquement.

Le **réplica Lag** métrique affiche l’heure dans la mesure où le dernier relus transaction. S’il n’existe aucune transaction sur votre serveur maître, la métrique reflète ce retard. Cette mesure est disponible pour les serveurs de réplication uniquement. Décalage de réplica est calculée à partir de la `pg_stat_wal_receiver` vue :

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

Définissez une alerte qui vous informe quand le retard du réplica atteint une valeur inacceptable pour votre charge de travail. 

Pour plus d’informations, interrogez directement le serveur maître pour connaître le délai de réplication en octets sur tous les réplicas.

Dans PostgreSQL version 10 :

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

Dans PostgreSQL version 9.6 et les versions antérieures :

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Si un serveur maître ou un réplica en lecture redémarre, le temps nécessaire pour redémarrer et rattraper le temps perdu est reflété par la métrique Retard du réplica.

## <a name="stop-replication"></a>Arrêter la réplication
Vous pouvez arrêter la réplication entre un serveur maître et un réplica. L’action d’arrêt provoque le redémarrage du réplica et la suppression de ses paramètres de réplication. Une fois la réplication arrêtée entre un serveur maître et un réplica en lecture, celui-ci devient un serveur autonome. Les données du serveur autonome sont les données disponibles sur le réplica au lancement de la commande d’arrêt de la réplication. Le serveur autonome ne se met pas au même niveau que le serveur maître.

> [!IMPORTANT]
> Le serveur autonome ne peut pas être retransformé en réplica.
> Avant d’arrêter la réplication sur un réplica en lecture, vérifiez que celui-ci contient toutes les données nécessaires.

Lorsque vous arrêtez la réplication, le réplica perd tous les liens avec son maître précédente et d’autres réplicas. Il n’existe aucun basculement automatique entre un maître et un réplica. 

Découvrez comment [arrêter la réplication sur un réplica](howto-read-replicas-portal.md).


## <a name="considerations"></a>Considérations

Cette section résume les considérations relatives à la fonctionnalité de réplica en lecture.

### <a name="prerequisites"></a>Conditions préalables
Avant de créer un réplica en lecture, vous devez affecter au paramètre `azure.replication_support` la valeur **REPLICA** sur le serveur maître. Quand vous changez ce paramètre, un redémarrage du serveur est nécessaire pour que la modification soit prise en compte. Le paramètre `azure.replication_support` s’applique uniquement aux niveaux à usage général et à mémoire optimisée.

### <a name="new-replicas"></a>Nouveaux réplicas
Un réplica en lecture est créé en tant que serveur Azure Database pour PostgreSQL. Un serveur existant ne peut pas être transformé en réplica. Vous ne pouvez pas créer un réplica d’un autre réplica en lecture.

### <a name="replica-configuration"></a>Configuration du réplica
Un réplica est créé à partir de la même configuration que celle du serveur maître. Une fois le réplica créé, vous pouvez changer plusieurs paramètres indépendamment du serveur maître : génération de calcul, vCores, stockage et période de conservation de la sauvegarde. Le niveau tarifaire peut également être changé indépendamment, sauf vers ou depuis le niveau De base.

> [!IMPORTANT]
> Avant de mettre à jour une configuration de serveur maître avec de nouvelles valeurs, mettez à jour la configuration du réplica avec des valeurs égales ou supérieures. Ainsi, vous avez la garantie que le réplica peut suivre les changements apportés au maître.

Avec PostgreSQL, la valeur du paramètre `max_connections` sur le réplica en lecture doit être supérieure ou égale à la valeur du serveur maître. Sinon, le réplica ne démarre pas. Dans Azure Database pour PostgreSQL, la valeur du paramètre `max_connections` est basée sur la référence (SKU). Pour plus d’informations, consultez [Limites d’Azure Database pour PostgreSQL](concepts-limits.md). 

Si vous tentez de mettre à jour les valeurs du serveur sans respecter les limites, vous recevez un message d’erreur.

### <a name="stopped-replicas"></a>Réplicas arrêtés
Si vous arrêtez la réplication entre un serveur maître et un réplica en lecture, le réplica redémarre pour permettre l’application du changement. Le réplica arrêté devient un serveur autonome qui accepte les lectures et les écritures. Le serveur autonome ne peut pas être retransformé en réplica.

### <a name="deleted-master-and-standalone-servers"></a>Serveurs maîtres et autonomes supprimés
Quand un serveur maître est supprimé, tous ses réplicas en lecture deviennent des serveurs autonomes. Les réplicas redémarrent pour permettre l’application de ce changement.

## <a name="next-steps"></a>Étapes suivantes
Découvrez comment [créer et gérer des réplicas en lecture dans le portail Azure](howto-read-replicas-portal.md).
