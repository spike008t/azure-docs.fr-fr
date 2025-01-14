---
title: Prise en main de Mobile Apps à l’aide de Xamarin.Forms
description: Suivez ce didacticiel pour commencer à utiliser Azure Mobile Apps pour le développement Xamarin.Forms
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: b0719f6ac2f99f9e665b1265665752dd53ccbaf0
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242662"
---
# <a name="create-a-xamarinforms-app-with-azure"></a>Créer une application Xamarin.Forms avec Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

Ce didacticiel vous montre comment ajouter un service principal cloud à une application Xamarin.Forms en utilisant la fonctionnalité Azure Mobile Apps d’Azure App Service en tant que back end. Vous allez créer un serveur principal d’applications mobiles et une simple application Xamarin.Forms de liste de tâches qui stocke les données d’application dans Azure.

Vous devez suivre ce didacticiel avant de pouvoir suivre tous les autres didacticiels Mobile Apps pour les applications Xamarin.Forms.

## <a name="prerequisites"></a>Conditions préalables

Pour réaliser ce didacticiel, vous avez besoin des éléments suivants :

* Un compte Azure actif. Si vous n'avez pas de compte, vous pouvez vous inscrire pour une évaluation d'Azure et obtenir jusqu'à 10 applications mobiles gratuites que vous pourrez conserver après l'expiration de votre période d'évaluation. Pour plus d’informations, consultez la page [Version d’évaluation gratuite d’Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Tools pour Xamarin, dans Visual Studio 2017 ou version ultérieure, ou Visual Studio pour Mac. Consultez la [page d’installation de Xamarin][Install Xamarin] pour obtenir des instructions.

* (facultatif) Pour générer une application iOS, un Mac avec Xcode 9.0 ou une version ultérieure est requis. Visual Studio pour Mac peut être utilisé pour développer des applications iOS, ou Visual Studio 2017 ou versions ultérieures peut être utilisé (tant que le Mac est disponible sur le réseau).

## <a name="create-a-new-mobile-apps-back-end"></a>Créer un back end Mobile Apps
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Créer une connexion de base de données et de configurer le projet client et serveur
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinforms-solution"></a>Exécuter la solution Xamarin.Forms

Visual Studio Tools pour Xamarin est requis pour ouvrir la solution, consultez les [instructions d’installation de Xamarin][Install Xamarin]. Si les outils sont déjà installés, suivez ces étapes pour télécharger et ouvrir la solution :

### <a name="visual-studio"></a>Visual Studio

1. Accédez au [portail Azure](https://portal.azure.com/).

2. Dans le panneau Paramètres de votre application mobile, cliquez sur **Quickstart** (sous Deployment) > **Xamarin.Forms**. À l’Étape 3, cliquez sur **Create a new app** si cette option n’est pas déjà sélectionnée.  Cliquez ensuite sur le bouton **Download** .

   Cette action télécharge un projet qui contient une application cliente connectée à votre application mobile. Enregistrez le fichier projet compressé sur votre ordinateur local et notez l'emplacement où vous l'avez enregistré.

3. Extrayez le projet que vous avez téléchargé, puis ouvrez-le dans Visual Studio.

4. Suivez les instructions ci-dessous pour exécuter les projets Android ou Windows, et, s’il existe un ordinateur Mac en réseau disponible, le projet iOS.

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

1. Accédez à la [Azure portal](https://portal.azure.com/) et accédez à l’application mobile que vous avez créé. Sur le `Overview` panneau, recherchez l’URL qui est le point de terminaison public pour votre application mobile. Exemple - sitename pour mon nom de l’application « test123 » sera https://test123.azurewebsites.net.

2. Ouvrez le fichier `Constants.cs` dans ce dossier - xamarin.forms/ZUMOAPPNAME. Le nom de l’application est `ZUMOAPPNAME`.

3. Dans `Constants.cs` classe, remplacez `ZUMOAPPURL` variable avec le point de terminaison public ci-dessus.

    `public static string ApplicationURL = @"ZUMOAPPURL";`

    devient

    `public static string ApplicationURL = @"https://test123.azurewebsites.net";`
    
4. Suivez les instructions ci-dessous pour exécuter les projets Android ou Windows, et, s’il existe un ordinateur Mac en réseau disponible, le projet iOS.

## <a name="optional-run-the-android-project"></a>(Facultatif) Exécuter le projet Android

Dans cette section, vous exécutez le projet Xamarin.Android. Vous pouvez ignorer cette section si vous n’utilisez pas d’appareils Android.

### <a name="visual-studio"></a>Visual Studio

1. Cliquez avec le bouton droit sur le projet Android (Droid), puis cliquez sur **Définir comme projet de démarrage**.

2. Dans le menu **Générer**, cliquez sur **Gestionnaire de configuration**.

3. Dans la boîte de dialogue **Gestionnaire de configuration**, cochez les cases **Générer** et **Déployer** situées à côté du projet Android, et assurez-vous que la case **Générer** du projet de code partagé est cochée.

4. Appuyez sur la touche **F5** ou le bouton **Démarrer** pour générer le projet et démarrer l’application dans l’émulateur Android.

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

1. Cliquez avec le bouton droit sur le projet Android, puis cliquez sur **Définir comme projet de démarrage**.

2. Pour générer le projet et démarrer l’application dans l’émulateur Android, cliquez sur le menu **Exécuter**, puis sur **Démarrer le débogage**.

Dans l’application, tapez un texte explicite, tel que *Découvrir Xamarin*, puis cliquez sur le bouton « + » ( **+** ).

![Application To-Do Android][11]

Cette action envoie une demande post vers le back end Mobile Apps qui est hébergé dans Azure. Les données de la requête sont insérées dans la table TodoItem. Les éléments stockés dans cette table sont renvoyés par le backend Mobile Apps et les données sont affichées dans la liste.

> [!NOTE]
> Le code qui vous permet d’accéder à votre back end Mobile Apps se trouve dans le fichier C# **TodoItemManager.cs** du projet de code partagé de la solution.
>

## <a name="optional-run-the-ios-project"></a>(Facultatif) Exécuter le projet iOS

Dans cette section, vous exécutez le projet Xamarin.iOS pour les appareils iOS. Vous pouvez ignorer cette section si vous n’utilisez pas d’appareils iOS.

### <a name="visual-studio"></a>Visual Studio

1. Cliquez avec le bouton droit sur le projet iOS, puis cliquez sur **Définir comme projet de démarrage**.

2. Dans le menu **Générer**, cliquez sur **Gestionnaire de configuration**.

3. Dans la boîte de dialogue **Gestionnaire de configuration**, cochez les cases **Générer** et **Déployer** situées à côté du projet iOS, et assurez-vous que la case **Générer** du projet de code partagé est cochée.

4. Pour générer le projet et démarrer l’application dans l’émulateur iPhone, appuyez sur la touche **F5**.

### <a name="visual-studio-for-mac"></a>Visual Studio pour Mac

1. Cliquez avec le bouton droit sur le projet iOS, puis cliquez sur **Définir comme projet de démarrage**.

2. Dans le menu **Exécuter**, cliquez sur **Démarrer le débogage** pour créer le projet et démarrer l’application dans l’émulateur iPhone.

Dans l’application, tapez un texte explicite, tel que *Découvrir Xamarin*, puis cliquez sur le bouton « + » ( **+** ).

![application To-Do iOS][10]

Cette action envoie une demande post vers le back end Mobile Apps qui est hébergé dans Azure. Les données de la requête sont insérées dans la table TodoItem. Les éléments stockés dans cette table sont renvoyés par le backend Mobile Apps et les données sont affichées dans la liste.

> [!NOTE]
> Vous trouverez le code qui vous permet d’accéder à votre back end Mobile Apps dans le fichier C# **TodoItemManager.cs** du projet de code partagé de la solution.
>

## <a name="optional-run-the-windows-project"></a>(Facultatif) Exécuter le projet Windows

Dans cette section, vous exécutez le projet UWP (plateforme Windows universelle) Xamarin.Forms pour les appareils Windows. Vous pouvez ignorer cette section si vous n’utilisez pas d’appareils Windows.

### <a name="visual-studio"></a>Visual Studio

1. Cliquez avec le bouton droit un projet UWP, puis cliquez sur **Définir comme projet de démarrage**.

2. Dans le menu **Générer**, cliquez sur **Gestionnaire de configuration**.

3. Dans la boîte de dialogue **Gestionnaire de configuration**, cochez les cases **Générer** et **Déployer** situées à côté du projet Windows que vous avez choisi, et assurez-vous que la case **Générer** du projet de code partagé est cochée.

4. Appuyez sur la touche **F5** ou le bouton **Démarrer** (qui doit indiquer **Ordinateur local**) pour générer le projet et démarrer l’application dans l’émulateur Android.

> [!NOTE]
> Le projet Windows ne peut pas être exécuté sur macOS.

Dans l’application, tapez un texte explicite, tel que *Découvrir Xamarin*, puis cliquez sur le bouton « + » ( **+** ).

Cette action envoie une demande post vers le back end Mobile Apps qui est hébergé dans Azure. Les données de la requête sont insérées dans la table TodoItem. Les éléments stockés dans cette table sont renvoyés par le backend Mobile Apps et les données sont affichées dans la liste.

![application To-Do UWP][12]

> [!NOTE]
> Vous trouverez le code qui vous permet d’accéder à votre back end Mobile Apps dans le fichier C# **TodoItemManager.cs** du projet de bibliothèque de classes portables de votre solution.
>

## <a name="troubleshooting"></a>Résolution de problèmes

Si vous avez des problèmes pour générer la solution, exécutez le gestionnaire de package NuGet et mettez à jour vers la dernière version de `Xamarin.Forms`, et dans le projet Android, mettez à jour les packages de support `Xamarin.Android`. Les projets de démarrage rapide n’incluent pas toujours les dernières versions.

Notez que tous les packages de support référencés dans votre projet Android doivent avoir la même version. Le [package NuGet Azure Mobile Apps](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) a une dépendance `Xamarin.Android.Support.CustomTabs` pour la plateforme Android, donc si votre projet utilise des packages de support plus récents vous devez installer ce package avec la version requise directement pour éviter les conflits.

<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png

<!-- URLs. -->
[Install Xamarin]: https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/