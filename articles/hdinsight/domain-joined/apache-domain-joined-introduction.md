---
title: Introduction à la sécurité Apache Hadoop avec le Pack Sécurité Entreprise
description: Découvrez comment le Pack Sécurité Entreprise prend en charge les quatre piliers de la sécurité d’entreprise.
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: 0582fa8b26bee05e4d2948037cc39a71ed656fce
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243950"
---
# <a name="what-is-enterprise-security-package-in-azure-hdinsight"></a>Quel est le Package de sécurité d’entreprise dans Azure HDInsight

Auparavant, Azure HDInsight ne prenait en charge qu’un seul utilisateur : l’administrateur local. Cela fonctionnait à merveille pour les petites équipes d’application ou les services. Avec le gain en popularité des charges de travail Apache Hadoop dans le secteur de l’entreprise, les besoins en fonctionnalités de classe Entreprise, comme l’authentification basée sur Active Directory, la prise en charge multi-utilisateur et le contrôle d’accès en fonction du rôle sont devenus de plus en plus importants. 

Vous pouvez créer un cluster HDInsight avec le Pack Sécurité Entreprise (ESP) qui est joint à un domaine Active Directory. Vous pouvez ensuite configurer une liste d’employés à partir de l’entreprise qui peuvent s’authentifier via Azure Active Directory pour vous connecter au cluster HDInsight. Aucun à partir en dehors de l’entreprise peut se connecter ou accéder au cluster HDInsight. 

L’administrateur d’entreprise peut configurer le contrôle d’accès en fonction du rôle (RBAC) pour assurer la sécurité de Apache Hive avec [Apache Ranger](https://hortonworks.com/apache/ranger/). La configuration de RBAC limite l’accès des données au strict nécessaire. Enfin, l’administrateur peut auditer l’accès des données par les employés, et les modifications apportées aux stratégies de contrôle d’accès. L’administrateur peut ainsi atteindre un degré élevé de gouvernance des ressources de l’entreprise.

> [!NOTE]  
> Apache Oozie est maintenant activé sur les clusters ESP. Pour accéder à l’interface utilisateur web d’Oozie, les utilisateurs doivent activer le [tunneling](../hdinsight-linux-ambari-ssh-tunnel.md).

La sécurité d’entreprise est constituée de quatre piliers majeurs : la sécurité du périmètre, l’authentification, l’autorisation et le chiffrement.

![Avantages des clusters HDInsight ESP dans les quatre piliers de la sécurité d’entreprise](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

## <a name="perimeter-security"></a>Sécurité du périmètre
La sécurité du périmètre dans HDInsight est obtenue par le biais de réseaux virtuels et du service de passerelle VPN Azure. Un administrateur d’entreprise peut créer un cluster ESP à l’intérieur d’un réseau virtuel, et utiliser des groupes de sécurité réseau (règles de pare-feu) pour restreindre l’accès au réseau virtuel. Seules les adresses IP définies dans les règles de pare-feu entrantes sont en mesure de communiquer avec le cluster HDInsight. Cette configuration assure la sécurité du périmètre.

Le service de passerelle VPN permet d’obtenir un autre niveau de sécurité du périmètre. La passerelle agit en tant que première ligne de défense pour toutes les requêtes entrantes dans le cluster HDInsight. Elle accepte la requête, la valide et seulement après autorise la requête à passer sur les autres nœuds du cluster. De cette façon, la passerelle assure la sécurité du périmètre d’autres nœuds du nom et des données dans le cluster.

## <a name="authentication"></a>Authentication
Un administrateur d’entreprise peut créer un cluster HDInsight avec ESP dans un [réseau virtuel](https://azure.microsoft.com/services/virtual-network/). Tous les nœuds du cluster HDInsight sont joints au domaine que l’entreprise gère. Ceci s’effectue par l’intermédiaire d’[Azure Active Directory Domain Services](../../active-directory-domain-services/overview.md). 

Avec cette configuration, employés de l’entreprise peuvent se connecter aux nœuds du cluster à l’aide de leurs informations d’identification de domaine. Ils peuvent également utiliser leurs informations d’identification de domaine pour s’authentifier auprès d’autres points de terminaison approuvés comme les vues Ambari, ODBC, JDBC, PowerShell et les API REST afin d’interagir avec le cluster. L’administrateur dispose d’un contrôle total sur la limitation du nombre d’utilisateurs interagissant avec le cluster via ces points de terminaison.

## <a name="authorization"></a>Authorization
L’une des bonnes pratiques que la plupart des entreprises appliquent consiste à s’assurer que chaque employé n’a pas accès à l’intégralité des ressources de l’entreprise. Parallèlement, l’administrateur peut définir des stratégies de contrôle d’accès en fonction du rôle pour les ressources du cluster. 

Par exemple, l’administrateur peut configurer [Apache Ranger](https://hortonworks.com/apache/ranger/) pour définir des stratégies de contrôle d’accès pour Hive. Cette fonctionnalité garantit que les employés peuvent accéder uniquement aux données nécessaires à leur travail. L’accès SSH au cluster est aussi limité exclusivement à l’administrateur.

## <a name="auditing"></a>Audit
L’audit de tous les accès aux ressources du cluster, et aux données, est nécessaire pour effectuer le suivi des accès non autorisés ou non intentionnels des ressources. Il est tout aussi important de protéger les ressources du cluster HDInsight des utilisateurs non autorisés, et de sécuriser les données. 

L’administrateur peut afficher et signaler tout accès aux données et aux ressources du cluster HDInsight. L’administrateur peut également voir et signaler toutes les modifications des stratégies de contrôle d’accès créées dans les points de terminaison pris en charge par Apache Ranger. 

Un cluster HDInsight avec ESP utilise l’interface utilisateur familière d’Apache Ranger pour rechercher les journaux d’audit. Sur le backend, Ranger utilise [Apache Solr](https://hortonworks.com/apache/solr/) pour le stockage et la recherche des journaux d’activité.

## <a name="encryption"></a>Chiffrement
La protection des données est essentielle pour se conformer aux exigences de l’organisation en matière de conformité et de sécurité. Parallèlement à la restriction de l’accès aux données des employés non autorisés, vous devez procéder à leur chiffrement. 

Stockage Blob Azure et Azure Data Lake Storage Gen1/Gen2, les deux magasins de données pour les clusters HDInsight, prennent en charge de façon transparente côté serveur le [chiffrement des données](../../storage/common/storage-service-encryption.md) au repos. Les clusters HDInsight sécurisés fonctionnent sans problème avec cette fonctionnalité de chiffrement des données au repos côté serveur.

## <a name="next-steps"></a>Étapes suivantes

* [Planifier des clusters HDInsight avec ESP](apache-domain-joined-architecture.md)
* [Configurer des clusters HDInsight avec ESP](apache-domain-joined-configure.md)
* [Gérer des clusters HDInsight avec ESP](apache-domain-joined-manage.md)
* [Configurer des stratégies Apache Hive pour des clusters HDInsight avec ESP](apache-domain-joined-run-hive.md)
* [Utiliser SSH avec HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)

