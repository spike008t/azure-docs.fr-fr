---
title: Introduction à Azure Red Hat OpenShift | Microsoft Docs
description: Découvrez les fonctionnalités et les avantages de Microsoft Azure Red Hat OpenShift pour déployer et gérer des applications basées sur des conteneurs.
services: container-service
author: jimzim
ms.author: jzim
ms.service: container-service
manager: jeconnoc
ms.topic: overview
ms.date: 05/08/2019
ms.custom: mvc
ms.openlocfilehash: 7cabedaec1190437aa9f225397afa8871cb06e88
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306371"
---
# <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Le service Microsoft *Azure Red Hat OpenShift* vous permet de déployer des clusters [OpenShift](https://www.openshift.com/) entièrement gérés.

Azure Red Hat OpenShift est une extension de [Kubernetes](https://kubernetes.io/). L’exécution de conteneurs en production avec Kubernetes nécessite des outils et des ressources supplémentaires, tels que le Registre d’image, la gestion du stockage, les solutions de mise en réseau ainsi que les outils de journalisation et de surveillance, dont les versions doivent être gérées et qui doivent être testés ensemble. La création d’applications basées sur des conteneurs requiert encore plus de travail d’intégration avec des intergiciels, des infrastructures, des bases de données et des outils CI/CD. Azure Red Hat OpenShift combine tout cela dans une seule plateforme, facilitant les opérations des équipes informatiques tout en donnant aux équipes d’application ce dont elles ont besoin pour exécuter.

Azure Red Hat OpenShift est conçue, exploitée et prise en charge conjointement par Red Hat et par Microsoft pour fournir une expérience de support intégré. Il n’y a aucune machine virtuelle à faire fonctionner et aucune mise à jour corrective n’est nécessaire. Les nœuds master, d’infrastructure et d’applications sont corrigés, mis à jour et surveillés en votre nom par Red Hat et par Microsoft. Vos clusters Azure Red Hat OpenShift sont déployés dans votre abonnement Azure et inclus sur votre facture Azure.

Vous pouvez choisir vos propres solutions de Registre, de mise en réseau, de stockage et CI/CD, ou utiliser les solutions intégrées pour la gestion automatisée du code source, les builds de conteneurs et d’applications, les déploiements, la mise à l’échelle, la gestion de l’intégrité et bien plus encore. Azure Red Hat OpenShift fournit une expérience d’authentification intégrée via Azure Active Directory.

Pour commencer, suivez le tutoriel [Créer un cluster Azure Red Hat OpenShift](tutorial-create-cluster.md).

## <a name="access-security-and-monitoring"></a>Accès, sécurité et surveillance

Pour améliorer la sécurité et la gestion, Azure Red Hat OpenShift vous permet d’intégrer Azure Active Directory (Azure AD) et d’utiliser le contrôle d’accès en fonction du rôle Kubernetes (RBAC). Vous pouvez également superviser l’intégrité de votre cluster et de vos ressources.

## <a name="cluster-and-node"></a>Cluster et nœud

Les nœuds Azure Red Hat OpenShift s’exécutent sur des machines virtuelles Azure. Vous pouvez connecter le stockage à des nœuds et des pods, mettre à niveau les composants du cluster et utiliser des GPU.

## <a name="virtual-networks-and-ingress"></a>Réseaux virtuels et entrée

Vous pouvez connecter un cluster Azure Red Hat OpenShift à un réseau virtuel existant via le peering. Dans cette configuration, les pods peuvent se connecter à d’autres services dans un réseau virtuel appairé et à des réseaux locaux via [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) ou des connexions VPN site à site (S2S).

Consultez [Connecter le réseau virtuel d’un cluster à un réseau virtuel existant](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) pour plus d’informations.

## <a name="kubernetes-certification"></a>Certification Kubernetes

Le service Azure Red Hat OpenShift a été certifié conforme à Kubernetes par CNCF.

## <a name="next-steps"></a>Étapes suivantes

Découvrez les conditions préalables pour Azure Red Hat OpenShift :

> [!div class="nextstepaction"]
> [Configurer votre environnement de développement](howto-setup-environment.md)