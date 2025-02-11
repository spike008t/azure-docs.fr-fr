---
title: Outils d’exploration et de visualisation de données - Azure | Microsoft Docs
description: Outils d’exploration et de visualisation de données pour la machine virtuelle DSVM
keywords: outils de science des données, machine virtuelle science des données, outils pour la science des données, science des données linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 693be80e493a0ba259d147f432dc9d6c07ba876d
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427518"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Outils d’exploration et de visualisation de données sur la machine virtuelle DSVM

Une étape clé de la science des données consiste à comprendre les données. Les outils de visualisation et d’exploration de données aident à accélérer la compréhension des données. Voici quelques outils fournis sur la machine virtuelle DSVM qui facilitent cette étape clé. 

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Qu’est-ce que c’est ?   | Moteur de requête SQL open source sur le Big Data    |
| Versions DSVM prises en charge      | Windows, Linux  |
| Comment est-il configuré / installé sur la machine virtuelle DSVM ?      |  Installé dans `/dsvm/tools/drill*` en mode incorporé uniquement   |
| Utilisations classiques      |  Exploration de données in situ sans nécessiter d’extraction, de transformation et de chargement. Interroger différents formats et sources de données, notamment CSV, JSON, les tables relationnelles et Hadoop     |
| Comment l’utiliser/l’exécuter ?      | Raccourci du Bureau  <br/> [Prendre en main Drill en 10 minutes](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| Outils connexes sur la machine virtuelle DSVM      |   Rattle, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Qu’est-ce que c’est ?   |  Weka est une collection d’algorithmes d’apprentissage automatique pour les tâches d’exploration de données. Les algorithmes peuvent être appliqués directement à un jeu de données ou être appelés à partir de votre propre code Java. Weka contient des outils pour le prétraitement des données, la classification, la régression, le clustering, les règles d’association et la visualisation. |
| Éditions DSVM prises en charge     | Windows, Linux     |
| Utilisations classiques      | Outil ML général     |
| Comment l’utiliser/l’exécuter ?      | Sur Windows, recherchez Weka dans le menu Démarrer. Sur Linux, connectez-vous avec X2Go, puis accédez à Applications -> développement -> Weka. |
| Liens vers des exemples      | [Exemples Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Outils connexes sur la machine virtuelle DSVM      |LightGBM, Rattle, Xgboost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| Qu’est-ce que c’est ?   |   Une interface graphique utilisateur pour l’exploration de données à l’aide de R   |
| Éditions DSVM prises en charge     | Windows, Linux     |
| Utilisations classiques      | Outil général d’interface graphique utilisateur d’exploration de données pour R    |
| Comment l’utiliser/l’exécuter ?      | Outil d’interface utilisateur. Sur Windows, démarrez une invite de commandes, exécutez R puis, dans R, exécutez `rattle()`. Sur Linux, connectez-vous avec X2Go, démarrez un terminal, exécutez R puis, dans R, exécutez `rattle()`. |
| Liens vers des exemples      | [Rattle](https://togaware.com/onepager/) |
| Outils connexes sur la machine virtuelle DSVM      |LightGBM, Weka, Xgboost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Qu’est-ce que c’est ?   | Outil de décisionnel et de visualisation interactive des données    |
| Versions DSVM prises en charge      | Windows  |
| Utilisations classiques      |  Visualisation des données et création de tableaux de bord   |
| Comment l’utiliser/l’exécuter ?      | Raccourci sur le Bureau (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Outils connexes sur la machine virtuelle DSVM      |   Visual Studio 2019, Visual Studio Code, Juno      |

