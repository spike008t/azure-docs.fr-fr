---
title: Modèle d'application web mutualisée | Microsoft Docs
description: Trouvez des présentations et des modèles de conception architecturaux qui décrivent comment implémenter une application web mutualisée dans Azure.
services: ''
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: ''
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 92a0caedca34756228dbf57ec9099fd2ece3d84e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66225972"
---
# <a name="multitenant-applications-in-azure"></a>Applications mutualisées dans Azure
Une application mutualisée est une ressource partagée qui autorise des utilisateurs distincts, appelés « locataires », à afficher l'application comme s'il s'agissait de la leur. Un scénario typique qui se prête à une application mutualisée est celui dans lequel tous les utilisateurs de l'application peuvent personnaliser leur expérience utilisateur, tout en ayant les mêmes besoins professionnels de base. Exemples d'applications mutualisées : Office 365, Outlook.com et visualstudio.com.

Du point de vue du fournisseur d'application, les avantages d'une architecture mutualisée touchent principalement à l'efficacité opérationnelle et à la réduction des coûts. Une version de votre application peut répondre aux besoins de plusieurs locataires/clients, autorisant ainsi la consolidation des tâches d'administration système telles que la surveillance, le réglage des performances, les mises à jour logicielles et les sauvegardes de données.

Voici une liste des objectifs et exigences les plus significatifs du point de vue d'un fournisseur.

* **Approvisionnement**: Vous devez être en mesure d’approvisionner de nouveaux locataires pour l’application.  Pour les applications mutualisées incluant un grand nombre de locataires, il est généralement nécessaire d'automatiser ce processus en activant l'approvisionnement libre-service.
* **La facilité de gestion**: Vous devez être en mesure de mettre à niveau de l’application et effectuer d’autres tâches de maintenance lorsque plusieurs clients l’utilisent.
* **Supervision** : Vous devez être en mesure de surveiller l’application à tout moment pour identifier les problèmes et à les résoudre. Cela inclut la surveillance de la façon dont chaque locataire utilise l'application.

Une application mutualisée correctement mise en œuvre offre les avantages suivants aux utilisateurs.

* **Isolation** : Les activités de locataires individuels n’affectent pas l’utilisation de l’application par d’autres clients. Les locataires ne peuvent pas accéder aux données des uns et des autres. Le locataire a ainsi l’impression d’avoir une utilisation exclusive de l’application.
* **Disponibilité** : Locataires individuels veulent que l’application soit constamment disponible, peut-être avec des garanties définies dans un contrat SLA. À nouveau, les activités des autres locataires ne doivent pas affecter la disponibilité de l'application.
* **Scalabilité** : L’application s’adapte à répondre à la demande de locataires individuels. La présence et les actions des autres locataires ne doivent pas affecter les performances de l'application.
* **Coûts** : Les coûts sont inférieurs à ceux de l’exécution d’une application à locataire unique, dédiée, car une architecture mutualisée permet le partage des ressources.
* **Possibilités de personnalisation**. possibilité de personnaliser l'application d'un locataire individuel de diverses façons telles qu'en ajoutant ou en supprimant des fonctionnalités, en changeant les couleurs et les logos, ou même en ajoutant son propre code ou script.

En bref, bien qu’il existe de nombreuses considérations que vous devez prendre en compte pour fournir un service hautement évolutif, il existe également un nombre d’objectifs et exigences qui sont communes à nombreuses applications mutualisées. Certains peuvent ne pas concerner des scénarios spécifiques, et l'importance d'objectifs et d'exigences individuels peut différer pour chaque scénario. En tant que fournisseur de l’application multilocataire, vous aurez également objectifs et exigences tels que du locataire objectifs et exigences, rentabilité, facturation, plusieurs niveaux de service, l’approvisionnement, l’analyse de la facilité de gestion et automatisation de la réunion.

Pour plus d'informations sur les considérations supplémentaires d'une application mutualisée en matière de conception, consultez la page [Hébergement d'une application mutualisée dans Azure][Hosting a Multi-Tenant Application on Azure]. Pour plus d’informations sur les modèles d’architecture de données courants pour les applications de base de données SaaS (software as a service) multilocataires, consultez [Modèles de conception pour les applications SaaS multilocataires avec Azure SQL Database](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure offre de nombreuses fonctionnalités vous permettant de résoudre les problèmes clés rencontrés lors de la conception d'un système mutualisé.

**Isolement**

* Segmentation des locataires de site web en fonction des en-têtes d'hôte avec ou sans communication SSL
* Segmentation des locataires de site web en fonction des paramètres de requête
* Services Web dans les rôles de travail
  * Rôles de travail qui traitent généralement les données sur le serveur principal d’une application.
  * Rôles Web faisant généralement office d'interface frontale pour les applications.

**Stockage**

Gestion des données telles que les services de base de données SQL Azure ou le stockage Azure tels que le service de Table, qui fournit des services pour le stockage de grandes quantités de données non structurées et le service Blob, qui fournit des services pour stocker de grandes quantités de texte non structuré ou données binaires telles que la vidéo, audio et images.

* Sécurisation de données mutualisées dans la base de données SQL par locataire connexions SQL Server.
* Utilisation de Tables Azure pour les ressources de l’Application en spécifiant une stratégie d’accès de niveau conteneur, vous pouvez avoir la possibilité de modifier les autorisations sans avoir à émettre de nouvelles URL pour les ressources protégées avec des signatures d’accès partagé.
* Files d'attentes Azure pour les ressources d'application : les files d'attente Azure sont couramment utilisées pour effectuer le traitement pour le compte de locataires, mais elles peuvent également permettre de distribuer les tâches requises pour l'approvisionnement ou la gestion.
* Files d'attente Service Bus pour les ressources d'application qui transfèrent les tâches vers un service partagé. Vous pouvez utiliser une file d'attente unique dans laquelle chaque expéditeur locataire dispose uniquement d'autorisations (telles que dérivées de revendications émises par ACS) de transfert vers cette file d'attente, tandis que seuls les destinataires de ce service sont autorisés à extraire à partir de la file d'attente les données provenant de plusieurs locataires.

**Services de connexion et de sécurité**

* Azure Service Bus, infrastructure de messagerie située entre des applications et autorisant celles-ci à échanger des messages de façon souple, offrant ainsi une mise à l'échelle et une résilience améliorées.

**Services de mise en réseau**

Azure offre plusieurs services de mise en réseau prenant en charge l'authentification et améliorant la capacité de gestion de vos applications hébergées. Ces services sont les suivants :

* Azure Virtual Network vous permet d'approvisionner et de gérer des réseaux privés virtuels (VPN) dans Azure, ainsi que de lier ceux-ci de façon sécurisée avec l'infrastructure informatique locale.
* Virtual Network Traffic Manager vous permet de répartir le trafic entrant sur plusieurs services Azure hébergés, qu'ils s'exécutent sur le même centre de données ou sur différents centres de données à travers le monde.
* Azure Active Directory (Azure AD) est un service moderne, basé sur le protocole REST, qui offre des capacités de contrôle d'accès et de gestion des identités pour vos applications dans le cloud. À l’aide d’Azure AD pour les ressources de l’Application fournit un moyen simple d’authentifier et autoriser les utilisateurs d’accéder à vos applications et services web tout en autorisant les fonctionnalités d’authentification et d’autorisation factorisation hors de votre code.
* Azure Service Bus offre des capacités de messagerie et de flux de données sécurisées pour les applications distribuées et hybrides, telles que les communications entre les applications hébergées sur Azure et les applications et services locaux, sans nécessiter d'infrastructures de sécurité et de pare-feu complexes. À l’aide de Service Bus Relay pour les ressources de l’Application à accéder aux services qui sont exposées en tant que points de terminaison peut-être appartenir au locataire (par exemple, être hébergés en dehors du système, par exemple en local), ou ils peuvent être des services approvisionnés spécifiquement pour le locataire (parce que les données sensibles, spécifique au locataire y transitent).

**Approvisionnement de ressources**

Azure offre plusieurs façons d’approvisionner de nouveaux locataires pour l’application. Pour les applications mutualisées incluant un grand nombre de locataires, il est généralement nécessaire d'automatiser ce processus en activant l'approvisionnement libre-service.

* Les rôles de travail vous permettent d'approvisionner ou d'annuler l'approvisionnement de ressources par locataire (comme lorsqu'un nouveau locataire s'inscrit ou annule), de recueillir des mesures et de gérer la mise à l'échelle sur la base d'une planification spécifique ou en réponse au dépassement des seuils d'indicateurs de performance clé. Ce même rôle peut également être utilisé pour transmettre des mises à jour et des mises à niveau vers la solution.
* Les objets blob Azure peuvent être utilisés pour approvisionner des ressources de calcul ou de stockage pré-initialisées pour les nouveaux locataires tout en fournissant des stratégies d'accès de niveau conteneur visant à protéger les packages de service de calcul, les images de disque dur virtuel et d'autres ressources.
* Options pour l'approvisionnement de ressources de la base de données SQL pour un locataire :
  
  * DDL dans les scripts ou incorporé en tant que ressources dans les assemblys.
  * Packages DAC SQL Server 2008 R2 déployés par programme
  * Copie à partir d’une base de données de référence principale.
  * Utilisation de l'importation et de l'exportation de base de données pour approvisionner de nouvelles bases de données à partir d'un fichier

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
