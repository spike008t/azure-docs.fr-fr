---
title: Concepts de haute disponibilité dans Azure Database pour PostgreSQL - serveur unique
description: Cet article fournit des informations de la haute disponibilité lors de l’utilisation de base de données Azure pour PostgreSQL - serveur unique.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: f54c83099957b4d8795c4049be52d70e8a0e2a61
ms.sourcegitcommit: f4469b7bb1f380bf9dddaf14763b24b1b508d57c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "65073451"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql---single-server"></a>Concepts de haute disponibilité dans Azure Database pour PostgreSQL - serveur unique
Le service Azure Database pour PostgreSQL fournit un haut niveau de disponibilité garanti. Le contrat de niveau de service (SLA) est de 99,99 % selon la disponibilité générale. Il n’existe pratiquement aucun temps d’arrêt d’application lors de l’utilisation de ce service.

## <a name="high-availability"></a>Haute disponibilité
Le modèle de haute disponibilité (HA) repose sur des mécanismes de basculement intégrés lorsqu'une interruption se produit au niveau du nœud. Une interruption au niveau du nœud peut se produire en raison d’une défaillance matérielle ou en réponse à un déploiement de service.

À tout moment, les modifications apportées à un serveur de base de données Azure Database pour PostgreSQL se produisent dans le contexte d’une transaction. Les modifications sont enregistrées de manière synchrone dans Stockage Azure lorsque la transaction est validée. En cas d’interruption au niveau du nœud, le serveur de base de données crée automatiquement un nouveau nœud et associe le stockage de données au nouveau nœud. Toutes les connexions actives sont supprimées et aucune transaction séquentielle n’est validée.

## <a name="application-retry-logic-is-essential"></a>La logique de nouvelle tentative d’application est essentielle
Il est important que des applications de base de données PostgreSQL soient conçues pour détecter et relancer les connexions interrompues et transactions ayant échoué. Lorsque l’application lance une nouvelle tentative, la connexion de l’application est redirigée de façon transparente vers l’instance nouvellement créée, qui remplace l’instance qui a échoué.

En interne dans Azure, une passerelle est utilisée pour rediriger les connexions vers la nouvelle instance. Lors d’une interruption, tout le processus de basculement prend généralement quelques dizaines de secondes. Étant donné que la redirection est gérée en interne par la passerelle, la chaîne de connexion externe reste la même pour les applications clientes.

## <a name="scaling-up-or-down"></a>Augmentation ou diminution d’échelle
Comme pour le modèle de haute disponibilité, lorsqu’une base de données PostgreSQL Azure voit son échelle augmentée ou réduite, une nouvelle instance de serveur avec la taille spécifiée est créée. Le stockage de données existant est détaché de l’instance d’origine et attaché à la nouvelle instance.

Pendant l’opération de mise à l’échelle, une interruption se produit pour les connexions de base de données. Les applications clientes sont déconnectées, et les transactions non validées en cours sont annulées. Une fois que l’application cliente réessaie d’établir la connexion ou établit une nouvelle connexion, la passerelle dirige la connexion vers l’instance qui vient d’être dimensionnée. 

## <a name="next-steps"></a>Étapes suivantes
- En savoir plus sur le [traitement des erreurs de connectivité transitoires](concepts-connectivity.md)
- Apprendre à [répliquer vos données avec des réplicas en lecture](howto-read-replicas-portal.md)
