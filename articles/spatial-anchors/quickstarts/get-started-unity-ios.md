---
title: 'Démarrage rapide : Créer une application iOS Unity avec Azure Spatial Anchors | Microsoft Docs'
description: Dans ce guide de démarrage rapide, vous allez apprendre à générer une application iOS avec Unity en utilisant Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 41189d4945ef22f8a587a51717b7dd6f40ac39ce
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64690544"
---
# <a name="quickstart-create-a-unity-ios-app-with-azure-spatial-anchors"></a>Démarrage rapide : Créer une application iOS Unity avec Azure Spatial Anchors

Ce guide de démarrage rapide explique comment créer une application iOS Unity à l’aide d’[Azure Spatial Anchors](../overview.md). Azure Spatial Anchors est un service de développement multiplateforme qui vous permet de créer des expériences de réalité mixte en utilisant des objets qui conservent leur emplacement sur les appareils. Quand vous aurez terminé, vous aurez une application iOS ARKit générée avec Unity capable d’enregistrer et de rappeler une ancre spatiale.

Vous découvrirez comment effectuer les actions suivantes :

> [!div class="checklist"]
> * Créer un compte Spatial Anchors
> * Préparer les paramètres de build Unity
> * Télécharger et importer le plug-in Unity ARKit
> * Configurer l’identificateur et la clé du compte Spatial Anchors
> * Exporter le projet Xcode
> * Déployer et exécuter sur un appareil iOS

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prérequis

Pour suivre ce guide de démarrage rapide, veillez à avoir :

- Une machine macOS dotée d’<a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a>, de <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10</a> et de <a href="https://cocoapods.org" target="_blank">CocoaPods</a>.
- Git installé via HomeBrew. Entrez la commande suivante sur une seule ligne du Terminal : `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Exécutez ensuite `brew install git`.
- Un appareil iOS <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">compatible ARKit</a> prêt pour le développement.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project-in-unity"></a>Ouvrir l’exemple de projet dans Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [iOS Unity Build Settings](../../../includes/spatial-anchors-unity-ios-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Configurer l’identificateur et la clé du compte

Dans le volet **Project**, accédez à `Assets/AzureSpatialAnchorsPlugin/Examples` et ouvrez le fichier de scène `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Enregistrez la scène en sélectionnant **File** -> **Save**.

## <a name="export-the-xcode-project"></a>Exporter le projet Xcode

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

[!INCLUDE [Configure Xcode](../../../includes/spatial-anchors-unity-ios-xcode.md)]

Suivez les instructions dans l’application pour placer et rappeler une ancre.

> [!NOTE]
> Pendant l’exécution de l’application, si vous ne voyez pas de caméra en arrière-plan (au lieu de cela, l’arrière-plan est vide, bleu ou présente d’autres textures), vous devez probablement réimporter les ressources dans Unity. Arrêtez l’application. Dans le menu supérieur d’Unity, choisissez **Assets -> Re-import all**. Ensuite, exécutez de nouveau l’application.

Dans Xcode, arrêtez l’application en appuyant sur **Stop**.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutoriel : Partager Spatial Anchors sur des appareils](../tutorials/tutorial-share-anchors-across-devices.md)
