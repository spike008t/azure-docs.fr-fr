---
title: Fichier Include
description: Fichier Include
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: include file
ms.openlocfilehash: 1f4a1e289f19e53bda1804a170c0c09d4cf081ec
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967621"
---
## <a name="setting-up-your-ios-application"></a>Configuration de votre application iOS

Cette section fournit des instructions détaillées permettant de créer un projet pour illustrer comment intégrer une application iOS (Swift) avec l’option *Se connecter avec Microsoft* pour pouvoir interroger des API web qui nécessitent un jeton.

> Vous préférez télécharger le projet XCode de cet exemple ? [Téléchargez un projet](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) et passez à [l’étape Configuration](#register-your-application) pour configurer l’exemple de code avant l’exécution.

## <a name="install-carthage-to-download-and-build-msal"></a>Installer Carthage pour télécharger et générer la bibliothèque MSAL

Le gestionnaire de package Carthage est utilisé pendant la période d’évaluation de la bibliothèque MSAL (Microsoft Authentication Library) : il s’intègre avec XCode tout en permettant à Microsoft de continuer à apporter des modifications à la bibliothèque.

- Téléchargez et installez la dernière version de Carthage [ici](https://github.com/Carthage/Carthage/releases "URL de téléchargement de Carthage").

## <a name="creating-your-application"></a>Création de votre application

1. Ouvrez Xcode et sélectionnez **Create a new Xcode project** (Créer un projet Xcode).
2. Sélectionnez **iOS > Single view Application** (iOS > Application à vue unique) et sélectionnez **Next** (Suivant).
3. Donnez un nom de produit, puis cliquez sur **Next** (Suivant).
4. Sélectionnez un dossier où créer votre application, puis cliquez sur *Créer*.

## <a name="build-the-msal-framework"></a>Générer l’infrastructure MSAL

Suivez les instructions ci-dessous pour extraire puis générer la version la plus récente des bibliothèques MSAL à l’aide de Carthage :

1. Ouvrez le terminal de l’interpréteur de commandes Bash et accédez au dossier racine de l’application.
2. Copiez le code ci-dessous et collez-le dans le terminal de l’interpréteur de commandes Bash pour créer un fichier « Cartfile » :

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Copiez et collez le code ci-dessous. Cette commande récupère les dépendances dans un dossier Carthage/Checkouts, puis génère la bibliothèque MSAL :
</li>
</ol>

```bash
carthage update
```

> Le processus ci-dessus sert à télécharger et à générer la bibliothèque MSAL. MSAL gère l’obtention, la mise en cache et l’actualisation des jetons d’utilisateur nécessaires pour accéder aux API protégées par la plateforme d’identités Microsoft.

## <a name="add-the-msal-framework-to-your-application"></a>Ajouter l’infrastructure MSAL à votre application

1. Dans Xcode, ouvrez l’onglet **General** (Général).
2. Accédez à la section **Bibliothèques et infrastructures liées** et sélectionnez **+**.
3. Sélectionnez **Add other...** (Ajouter autre...).
4. Sélectionnez **Carthage > Build > iOS > MSAL.framework**, puis **Open** (Ouvrir). `MSAL.framework` devrait être ajouté à la liste.
5. Cliquez sur l’onglet **Build Phases** (Phases de build), puis sélectionnez successivement l’icône **+** et **New Run Script Phase** (Nouvelle phase de script d’exécution).
6. Ajoutez le contenu suivant à la *zone de script* :

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Ajoutez le code suivant à <code>Input Files</code> en cliquant sur <code>+</code> :
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Création de l’interface utilisateur de votre application

Un fichier Main.storyboard doit être automatiquement créé dans le cadre de votre modèle de projet. Suivez les instructions ci-dessous pour créer l’interface utilisateur de l’application :

1.  Maintenez la touche Ctrl enfoncée et cliquez sur `Main.storyboard` pour faire apparaître le menu contextuel, puis cliquez sur `Open As` > `Source Code`.
2.  Remplacez le nœud `<scenes>` par le code ci-dessous :

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```
