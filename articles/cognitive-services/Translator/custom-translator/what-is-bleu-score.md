---
title: Qu’est-ce qu’un score BLEU ? - Custom Translator
titleSuffix: Azure Cognitive Services
description: BLEU est une mesure des différences entre une traduction automatique et une ou plusieurs traductions de référence créées par l’homme pour une même phrase source. L’algorithme BLEU compare les phrases consécutives de la traduction automatique avec les phrases consécutives qu’il trouve dans la traduction de référence, et il compte le nombre de correspondances de manière pondérée.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-pawal
ms.openlocfilehash: 08be79dde62a1d624fa59816cf855e702245b622
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66385164"
---
# <a name="what-is-a-bleu-score"></a>Qu’est-ce qu’un score BLEU ?

[BLEU (Bilingual Evaluation Understudy)](https://en.wikipedia.org/wiki/BLEU) est une mesure des différences entre une traduction automatique et une ou plusieurs traductions de référence créées par l’homme pour une même phrase source.

## <a name="scoring-process"></a>Processus de scoring

L’algorithme BLEU compare les phrases consécutives de la traduction automatique avec les phrases consécutives qu’il trouve dans la traduction de référence, et il compte le nombre de correspondances de manière pondérée. Ces correspondances sont indépendantes de la position. Un degré de concordance plus élevé indique un degré de similitude plus élevé avec la traduction de référence, et un score plus élevé. L’intelligibilité et l’exactitude grammaticale ne sont pas prises en compte.

## <a name="how-bleu-works"></a>Comment la technologie BLEU fonctionne-t-elle ?

La force de la technologie BLEU est qu’elle correspond bien au jugement humain en faisant la moyenne des erreurs de jugement des phrases individuelles sur un corpus d’essai, plutôt que de tenter d’établir un jugement humain exact pour chaque phrase.

Une discussion plus approfondie des scores BLEU est disponible [ici](https://youtu.be/-UqDljMymMg).

Les résultats de la technologie BLEU dépendent fortement de l’étendue de votre domaine, de la cohérence des données de test avec les données d’entraînement et de réglage, et de la quantité de données dont vous disposez pour l’entraînement. Si vos modèles ont été entraînés sur un domaine étroit et que vos données d’apprentissage correspondent à celles de vos tests, vous pouvez vous attendre à un score BLEU élevé.

>[!NOTE]
>Une comparaison entre les scores BLEU n’est justifiable que lorsque les résultats BLEU sont comparés au même ensemble de tests, à la même paire de langues et au même moteur de traduction machine. Un score BLEU d’une autre série de tests est forcément différent.
