---
title: Sécurité de la base de données – Azure Cosmos DB
description: Découvrez comment Azure Cosmos DB garantit la protection de la base de données et la sécurité de vos données.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: b82f7fed2407da86c036237a2de10363c4706d67
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241129"
---
# <a name="security-in-azure-cosmos-db---overview"></a>Indexation dans Azure Cosmos DB - Vue d’ensemble

Cet article décrit les meilleures pratiques en matière de sécurité de la base de données et les principales fonctionnalités d’Azure Cosmos DB qui vous aident à prévenir et détecter les violations de la base de données et à y répondre.

## <a name="whats-new-in-azure-cosmos-db-security"></a>Nouveautés de la sécurité Azure Cosmos DB

Le chiffrement au repos est désormais disponible pour les documents et les sauvegardes stockés dans Azure Cosmos DB dans toutes les régions Azure. Un chiffrement au repos est automatiquement appliqué aux clients nouveaux et existants dans ces régions. Vous n’avez pas à configurer quoi que ce soit. Vous bénéficiez des mêmes performances remarquables en matière de temps de latence, de débit, de disponibilité et de fonctionnalité qu’auparavant, avec l’avantage de savoir que vos données sont en lieu sûr et sécurisée grâce à un chiffrement au repos.

## <a name="how-do-i-secure-my-database"></a>Comment sécuriser ma base de données

La sécurité des données est une responsabilité partagée entre vous, le client et le fournisseur de votre base de données. Selon le fournisseur de base de données que vous choisissez, la part de votre responsabilité peut varier. Si vous choisissez une solution locale, vous devez fournir tous les éléments de la protection de point de terminaison à la sécurité physique de votre matériel, ce qui n’est pas une tâche facile. Si vous choisissez un fournisseur de base de données cloud PaaS comme Azure Cosmos DB, vous réduisez considérablement votre niveau d’inquiétude. L’image suivante, tirée du livre blanc Microsoft [Shared Responsibilities for Cloud Computing](https://aka.ms/sharedresponsibility) (Responsabilités partagées en matière de cloud computing), montre en quoi votre responsabilité diminue en faisant appel à un fournisseur PaaS tel qu’Azure Cosmos DB.

![Responsabilités du client et du fournisseur de base de données](./media/database-security/nosql-database-security-responsibilities.png)

Le diagramme précédent présente les composants de sécurité cloud de haut niveau. Quels sont ceux auxquels vous devez être particulièrement attentif pour votre solution de base de données ? Et comment comparer les solutions entre elles ?

Nous vous recommandons d’utiliser la liste de vérification suivante qui répertorie les critères selon lesquels comparer les systèmes de base de données :

- Sécurité du réseau et paramètres de pare-feu
- Authentification des utilisateurs et contrôles utilisateur affinés
- Possibilité de répliquer des données globalement en cas de défaillances régionales
- Possibilité de basculer à partir d’un centre de données vers un autre
- Réplication locale des données dans un centre de données
- Sauvegardes automatiques des données
- Restauration des données supprimées à partir de sauvegardes
- Protection et isolement des données sensibles
- Surveillance des attaques
- Réponse aux attaques
- Possibilité de délimiter géographiquement les données pour respecter les restrictions de gouvernance des données
- Protection physique des serveurs dans les centres de données protégés
- Certifications

Bien que cela paraisse évident, de récents exemples de [violations de base de données à grande échelle](https://thehackernews.com/2017/01/mongodb-database-security.html) nous rappellent l’importance simple mais critique des exigences suivantes :

- Serveurs corrigés qui sont mis à jour
- Chiffrement HTTPS par défaut/SSL
- Comptes administratifs avec des mots de passe forts

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Comment Azure Cosmos DB sécurise-t-il ma base de données

Reprenons la liste précédente : combien de ces exigences de sécurité Azure Cosmos DB garantit-il ? Toutes, sans exception.

Examinons à présent chacune d’entre elles en détail.

|Exigence de sécurité|Approche de la sécurité d’Azure Cosmos DB|
|---|---|
|Sécurité du réseau|L’utilisation d’un pare-feu IP est la première couche de protection pour sécuriser votre base de données. Azure Cosmos DB prend en charge les contrôles d’accès basés sur une stratégie IP pour le pare-feu entrant. Les contrôles d’accès basés sur IP sont semblables aux règles de pare-feu utilisées par les systèmes de base de données traditionnels, mais ils rendent un compte de base de données Azure Cosmos DB accessible uniquement à partir d’un ensemble d’ordinateurs ou de services cloud approuvés. <br><br>Azure Cosmos DB vous permet d’activer une adresse IP spécifique (168.61.48.0), une plage d’adresses IP (168.61.48.0/8) et des combinaisons d’adresses et de plages IP. <br><br>Toutes les demandes provenant d’ordinateurs ne figurant pas dans cette liste sont bloquées par Azure Cosmos DB. Les demandes issues d’ordinateurs et de services cloud approuvés doivent ensuite terminer le processus d’authentification pour bénéficier d’un contrôle d’accès sur les ressources.<br><br>Pour plus d’informations, consultez [Prise en charge du pare-feu dans Azure Cosmos DB](firewall-support.md).|
|Authorization|Azure Cosmos DB utilise un code d’authentification de message basé sur le hachage (HMAC) pour l’autorisation. <br><br>Chaque demande est hachée à l’aide de la clé secrète du compte, et ce hachage encodé en base 64 suivant est envoyé avec chaque appel à Azure Cosmos DB. Afin de valider la demande, le service Azure Cosmos DB utilise la clé secrète et les propriétés appropriées pour générer un hachage dont il compare la valeur à celle de la demande. Si les deux valeurs correspondent, l’opération est autorisée et la demande est traitée. Dans le cas contraire, un échec d’autorisation est généré et la demande est rejetée.<br><br>Vous pouvez utiliser une [clé principale](secure-access-to-data.md#master-keys) ou un [jeton de ressource](secure-access-to-data.md#resource-tokens) autorisant un accès précis à une ressource telle qu’un document.<br><br>Pour en savoir plus, consultez [Sécurisation de l’accès aux données Azure Cosmos DB](secure-access-to-data.md).|
|Utilisateurs et autorisations|À l'aide de la clé principale du compte, vous pouvez créer des ressources utilisateur et des ressources d'autorisation par base de données. Dans une base de données, un jeton de ressource est associé à une autorisation et il détermine si l'utilisateur a accès (lecture-écriture, lecture seule ou aucun accès) à une ressource d'application de la base de données. Les ressources d’application comprennent les conteneurs, les documents, les pièces jointes, les procédures stockées, les déclencheurs et les fonctions définies par l’utilisateur. La jeton de ressource est ensuite utilisé lors de l’authentification pour autoriser ou refuser l’accès à la ressource.<br><br>Pour en savoir plus, consultez [Sécurisation de l’accès aux données Azure Cosmos DB](secure-access-to-data.md).|
|Intégration d’Active Directory (RBAC)| Vous pouvez également fournir ou restreindre l’accès au compte, base de données, conteneur et offres (débit) à l’aide du contrôle d’accès (IAM) dans le portail Azure Cosmos. IAM fournit un contrôle d’accès basé sur les rôles et s’intègre à Active Directory. Vous pouvez utiliser les rôles intégrés ou des rôles personnalisés pour les personnes et groupes. Consultez [intégration Active Directory](role-based-access-control.md) article pour plus d’informations.|
|Réplication mondiale|Azure Cosmos DB offre une diffusion mondiale clé en main, ce qui vous permet de répliquer vos données dans n’importe quel centre de données Azure dans le monde en un seul clic. La réplication mondiale vous permet une mise à l’échelle globale et fournit un accès à faible latence à vos données dans le monde entier.<br><br>En matière de sécurité, la réplication mondiale protège les données contre les défaillances régionales.<br><br>Pour en savoir plus, consultez [Distribution mondiale des données avec DocumentDB](distribute-data-globally.md).|
|Basculements régionaux|Si vous avez répliqué vos données dans plusieurs centres de données, Azure Cosmos DB bascule automatiquement vos opérations si un centre de données régional est indisponible. Vous pouvez créer une liste des régions de basculement prioritaires comportant les régions dans lesquelles vos données sont répliquées. <br><br>Pour en savoir plus, consultez [Basculements régionaux automatiques pour la continuité des activités dans Azure Cosmos DB](high-availability.md).|
|Réplication locale|Même dans un centre de données, Azure Cosmos DB réplique automatiquement les données pour garantir une haute disponibilité et vous donne la possibilité de choisir des [niveaux de cohérence](consistency-levels.md). Cette réplication garantit un 99,99 % [contrat SLA de disponibilité](https://azure.microsoft.com/support/legal/sla/cosmos-db) pour tous les comptes à région unique ou tous les comptes dans plusieurs régions avec cohérence souple, et de disponibilité sur tous les comptes de base de données dans plusieurs régions de lecture à 99,999 %.|
|Sauvegardes en ligne automatisées|Bases de données Azure Cosmos DB sont régulièrement sauvegardées et stockées dans un magasin redondant géo. <br><br>Pour en savoir plus, consultez [Sauvegarde et restauration en ligne automatiques avec Azure Cosmos DB](online-backup-and-restore.md).|
|Restauration de données supprimées|Les sauvegardes en ligne automatisées peuvent être utilisées pour récupérer des données que vous avez accidentellement supprimées jusqu’à environ 30 jours après l’événement. <br><br>Pour en savoir plus, consultez [Sauvegarde et restauration en ligne automatiques avec Azure Cosmos DB](online-backup-and-restore.md).|
|Protection et isolement des données sensibles|Toutes les données stockées dans les régions répertoriées dans la section Nouveautés sont désormais chiffrées au repos.<br><br>Les données personnelles et d’autres données confidentielles peuvent être isolées dans un conteneur spécifique, et un accès en lecture-écriture ou en lecture seule peut être restreint à des utilisateurs spécifiques.|
|Surveillance des attaques|À l’aide de [l’enregistrement d’audit et des journaux d’activité](logging.md), vous pouvez surveiller les activités normales et anormales de votre compte. Vous pouvez afficher les opérations qui ont été effectuées sur vos ressources, la personne qui a initié l’opération, le moment auquel l’opération a été réalisée, l’état de l’opération et bien plus encore, comme l’illustre la capture d’écran qui suit ce tableau.|
|Réponse aux attaques|Une fois que vous avez contacté le support Azure pour signaler une attaque potentielle, un processus de réponse aux incidents en 5 étapes est lancé. L’objectif de ce processus en 5 étapes est de restaurer aussi rapidement que possible dans des conditions normales les opérations et la sécurité du service suite à la détection d’un problème et au lancement d’une investigation.<br><br>Pour en savoir plus, consultez [Microsoft Azure Security Response in the Cloud](https://aka.ms/securityresponsepaper) (Réponse de Microsoft Azure en matière de sécurité dans le cloud).|
|Délimitation géographique|Azure Cosmos DB garantit la gouvernance des données dans les régions souveraines (par exemple, Allemagne, Chine, US Gov).|
|Installations protégées|Dans Azure Cosmos DB, les données sont stockées sur des disques SSD dans les centres de données protégées d’Azure.<br><br>Pour en savoir plus, consultez les [centres de données Microsoft globaux](https://www.microsoft.com/en-us/cloud-platform/global-datacenters).|
|Chiffrement HTTPS/SSL/TLS|Toutes les interactions client-service d’Azure Cosmos DB sont compatibles SSL/TLS 1.2, En outre, toutes les réplications centre de données intra centres de données ou entre différents sont chiffrement SSL/TLS 1.2.|
|Chiffrement au repos|Toutes les données stockées dans Azure Cosmos DB sont chiffrées au repos. Pour en savoir plus, consultez [Chiffrement de base de données Azure Cosmos DB au repos](./database-encryption-at-rest.md).|
|Serveurs corrigés|En tant que base de données NoSQL gérée, Azure Cosmos DB ne nécessite aucune gestion ou correction des serveurs. Tout est fait automatiquement.|
|Comptes administratifs avec des mots de passe forts|Il est difficile de croire que nous devions encore mentionner cette exigence, mais contrairement à certains de nos concurrents, il est impossible d’avoir un compte d’administrateur sans mot de passe dans Azure Cosmos DB.<br><br> La sécurité via SSL et l’authentification basée sur un secret HMAC sont intégrées par défaut.|
|Certifications de sécurité et de protection des données| Pour obtenir la liste actualisée des certifications, consultez l’ensemble [site de conformité Azure](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings) , ainsi que la dernière version [Document de conformité Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) avec toutes les certifications (recherchez Cosmos). Pour plus ciblée, lisez le billet le 25 avril 2018, consultez [Azure #CosmosDB : Sécurisé, privé et conforme qui inclut SOC 1/2 Type 2, HITRUST, la norme PCI DSS Level 1, ISO 27001, HIPAA, FedRAMP High et bien d’autres.

La capture d’écran suivante montre comment utiliser l’enregistrement d’audit et les journaux d’activité pour surveiller votre compte : ![Journaux d’activité pour Azure Cosmos DB](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clés principales et les jetons de ressource, consultez [sécurisation de l’accès aux données Azure Cosmos DB](secure-access-to-data.md).

Pour plus d’informations sur la journalisation d’audit, consultez [journalisation des diagnostics Azure Cosmos DB](logging.md).

Pour plus d’informations sur les certifications Microsoft, consultez [centre de confidentialité Azure](https://azure.microsoft.com/support/trust-center/).