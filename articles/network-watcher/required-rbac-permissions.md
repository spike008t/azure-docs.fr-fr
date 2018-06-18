---
title: Autorisations obligatoires pour utiliser les fonctionnalités d’Azure Network Watcher | Microsoft Docs
description: Découvrez les autorisations de contrôle d’accès en fonction du rôle Azure qui sont nécessaires pour utiliser les fonctionnalités de Network Watcher.
services: network-watcher
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jdial
ms.openlocfilehash: 09f3a1e1d9c6796cb55ae8f0ab18bf8e1b3fa198
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077754"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Autorisations de contrôle d’accès en fonction du rôle obligatoires pour utiliser les fonctionnalités de Network Watcher

Le contrôle d’accès en fonction du rôle (RBAC) Azure vous permet d’affecter aux membres de votre organisation uniquement les actions dont ils ont besoin pour remplir leur fonction. Pour utiliser les fonctionnalités de Network Watcher, le compte avec lequel vous vous connectez à Azure doit être affecté aux rôles intégrés [Propriétaire](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [Contributeur](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor) ou [Contributeur de réseaux](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor), ou à un [rôle personnalisé](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) auquel sont affectées les actions listées pour chaque fonctionnalité de Network Watcher dans les sections qui suivent. Pour en savoir plus sur les fonctionnalités de Network Watcher, consultez [Qu’est-ce que Network Watcher](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Network Watcher

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Obtenir un observateur réseau                                          |
| Microsoft.Network/networkWatchers/write                             | Créer ou mettre à jour un observateur réseau                             |
| Microsoft.Network/networkWatchers/delete                            | Supprimer un observateur réseau                                       |

## <a name="nsg-flow-logs"></a>Journaux de flux NSG

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Configurer un journal de flux                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Interroger l’état d’un journal de flux                                    |

## <a name="connection-troubleshoot"></a>Résoudre les problèmes de connexion

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Interroger les résultats d’un test de résolution de problèmes de connexion                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Exécuter un test de résolution de problèmes de connexion                             |

## <a name="connection-monitor"></a>Moniteur de connexion

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Démarrer un moniteur de connexion                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Arrêter un moniteur de connexion                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Interroger un moniteur de connexion                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Obtenir un moniteur de connexion                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Créer un moniteur de connexion                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Supprimer un moniteur de connexion                                    |

## <a name="packet-capture"></a>Capture de paquet

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Interroger l’état d’une capture de paquets                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Arrêter une capture de paquets                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Obtenir une capture de paquets                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Créer une capture de paquets                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Supprimer une capture de paquets                                        |

## <a name="ip-flow-verify"></a>Vérification du flux IP

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Vérifier un flux IP                                              |

## <a name="next-hop"></a>Tronçon suivant

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Obtenir le tronçon suivant à partir d’une machine virtuelle                                     |

## <a name="network-security-group-view"></a>Vue du groupe de sécurité réseau

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Afficher les groupes de sécurité                                           |

## <a name="topology"></a>Topologie

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Obtenir la topologie                                                   |

## <a name="reachability-report"></a>Rapport d’accessibilité

| Action                                                              | NOM                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Obtenir un rapport d’accessibilité Azure                               |

## <a name="additional-actions"></a>Actions supplémentaires

Les fonctionnalités de Network Watcher nécessitent également les actions suivantes :

- Microsoft.Storage/Read
- Microsoft.Authorization/Read
- Microsoft.Resources/subscriptions/resourceGroups/Read
- Microsoft.Storage/storageAccounts/listServiceSas/Action
- Microsoft.Storage/storageAccounts/listAccountSas/Action
- Microsoft.Storage/storageAccounts/listKeys/Action
- Microsoft.Compute/virtualMachines/Read
- Microsoft.Compute/virtualMachines/Write
- Microsoft.Compute/virtualMachineScaleSets/Read
- Microsoft.Compute/virtualMachineScaleSets/Write
- Microsoft.Insights/alertRules/*
- Microsoft.Support/*