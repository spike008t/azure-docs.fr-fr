---
title: Fichier Include
description: Fichier Include
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 9ffb02fce41e8805dfccf1dfd6e982cf107039ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127547"
---
| **Ressource** | **Limite par défaut** | **Limite maximale** |
| --- | --- | --- |
| Comptes Batch Azure par région et par abonnement | 1-3 |50 |
| Cœurs dédiés par compte Batch | 10-100 | N/A<sup>1</sup> |
| Cœurs à priorité basse par compte Batch | 10-100 | N/A<sup>2</sup> |
| Travaux et planifications de travaux actifs<sup>3</sup> par compte Batch | 100-300 | 1,000<sup>4</sup> |
| Pools par compte Batch | 20-100 | 500<sup>4</sup> |

> [!NOTE]
> Les limites par défaut varient selon le type d’abonnement que vous utilisez pour créer un compte Batch. Les quotas de cœurs affichés correspondent aux comptes Batch en mode service Batch. [Affichez les quotas dans votre compte Batch](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup>le nombre de cœurs dédiés par compte Batch peut être augmenté, mais le nombre maximal n’est pas spécifié. Pour discuter des possibilités d’augmentation, contactez le Support Azure.

<sup>2</sup>le nombre de cœurs de faible priorité par compte Batch peut être augmenté, mais le nombre maximal n’est pas spécifié. Pour discuter des possibilités d’augmentation, contactez le Support Azure.

<sup>3</sup>terminé et planifications de travaux ne sont pas limitées.

<sup>4</sup>pour demander une augmentation au-delà de cette limite, contactez le Support Azure.
