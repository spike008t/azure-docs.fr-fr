---
title: Fichier Include
description: Fichier Include
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: d1e7fa1ed1649508f0d4923db8817d17ad556ca1
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66164587"
---
Lorsque vous déployez des ressources vers Azure, il est extrêmement simple de décider quels types de ressources déployer, leur emplacement et leur mode de configuration. Toutefois, cette flexibilité peut offrir plus d’options que vous ne souhaitez autoriser dans votre organisation. Si vous envisagez de déployer des ressources vers Azure, vous vous demandez peut-être :

* Comment respecter les obligations légales en matière de souveraineté des données dans certains pays/régions ?
* Comment contrôler les coûts ?
* Comment vérifier que quelqu'un ne modifie pas par inadvertance un système critique ?
* Comment effectuer le suivi des coûts des ressources et les facturer précisément ?

Cet article répond à ces questions. Plus précisément, vous :

> [!div class="checklist"]
> * assignez des utilisateurs aux rôles et les rôles à une étendue afin que les utilisateurs soient autorisés à effectuer des actions attendues, mais pas plus d’actions,
> * appliquez des stratégies qui définissent les conventions pour les ressources dans votre abonnement,
> * verrouillez les ressources importantes pour votre système,
> * balisez les ressources afin de les suivre selon la valeur qu’elles ont pour votre organisation.

Cet article se concentre sur les tâches effectuez pour implémenter la gouvernance. Pour une description plus détaillée des concepts, consultez [Gouvernance dans Azure](../articles/security/governance-in-azure.md). 
