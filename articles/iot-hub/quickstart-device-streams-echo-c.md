---
title: Communiquer avec une application d’appareil dans C par le biais de flux d’appareil IoT Hub (préversion) | Microsoft Docs
description: Dans ce guide de démarrage rapide, vous allez exécuter une application côté service C qui communique avec un appareil IoT par le biais d’un flux d’appareil.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: f5e6128c1ecceda181f92b2d81e9ac06effbfce2
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834064"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Démarrage rapide : Communiquer avec une application d’appareil dans C par le biais de flux d’appareil IoT Hub (préversion)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IoT Hub prend actuellement en charge les flux d’appareil en tant que [fonctionnalité d’évaluation](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Les [flux d’appareil IoT Hub](iot-hub-device-streams-overview.md) permettent aux applications de service et d’appareil de communiquer de manière sécurisée à travers des pare-feux. Dans la préversion publique, le SDK C prend uniquement en charge les flux d’appareil côté appareil. Par conséquent, le présent guide de démarrage rapide couvre uniquement les instructions permettant d’exécuter l’application côté appareil. Vous devez exécuter une application côté service complémentaire, disponible dans les guides de démarrage rapide suivants :
 
   * [Communication avec des applications d’appareils en C# via les flux d'appareils IoT Hub](./quickstart-device-streams-echo-csharp.md)

   * [Communication avec des applications d’appareils en C# via les flux d'appareils IoT Hub](./quickstart-device-streams-echo-nodejs.md).

L’application C côté appareil utilisée dans ce guide de démarrage rapide présente les fonctionnalités suivantes :

* Établissez un flux d’appareil vers un appareil IoT.

* Recevez des données envoyées à partir du côté service et renvoyez-les.

Le code illustre le processus de lancement d’un flux d’appareil ainsi que la façon de l’utiliser pour envoyer et recevoir des données.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites"></a>Prérequis

* Pour le moment, la préversion des flux d’appareil est uniquement prise en charge pour les hubs IoT créés dans les régions suivantes :

  * **USA Centre**

  * **EUAP USA Centre**

* Installez [Visual Studio 2017](https://www.visualstudio.com/vs/) avec la charge de travail [« Développement Desktop en C++ »](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) activée.

* Installez la dernière version de [Git](https://git-scm.com/download/).

* Exécutez la commande suivante afin d’ajouter l’extension Microsoft Azure IoT pour Azure CLI à votre instance Cloud Shell. L’extension IoT ajoute des commandes IoT Hub, IoT Edge et IoT Device Provisioning Service (DPS) à Azure CLI.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Préparer l’environnement de développement

Pour ce guide de démarrage rapide, vous devez utiliser le [kit SDK Azure IoT device SDK for C](iot-hub-device-sdk-c-intro.md). Vous allez préparer un environnement de développement pour cloner et générer le [SDK C Azure IoT](https://github.com/Azure/azure-iot-sdk-c) à partir de GitHub. Le kit SDK sur GitHub comprend l’exemple de code utilisé dans ce guide de démarrage rapide.

1. Téléchargez le [système de génération CMake](https://cmake.org/download/).

    Il est important que les composants requis Visual Studio (Visual Studio et la charge de travail « Développement Desktop en C++ ») soient installés sur votre machine, **avant** de commencer l’installation de l’élément `CMake`. Une fois les composants requis en place et le téléchargement effectué, installez le système de génération de CMake.

2. Ouvrez une invite de commandes ou l’interpréteur de commandes Git Bash. Exécutez la commande suivante pour cloner le référentiel GitHub du [Kit de développement logiciel (SDK) Azure IoT pour C](https://github.com/Azure/azure-iot-sdk-c) :

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Cette opération peut prendre plusieurs minutes.

3. Créez un sous-répertoire `cmake` dans le répertoire racine du référentiel Git et accédez à ce dossier.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Exécutez les commandes suivantes à partir du répertoire `cmake` pour générer une version du kit SDK propre à votre plateforme cliente de développement.

   * Dans Linux :

      ```bash
      cmake ..
      make -j
      ```

   * Dans Windows, exécutez les commandes suivantes à l’invite de commandes développeur pour Visual Studio 2015 ou 2017. Une solution Visual Studio pour l’appareil simulé est générée dans le répertoire `cmake`.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>Créer un hub IoT

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Inscrire un appareil

Un appareil doit être inscrit dans votre hub IoT pour pouvoir se connecter. Dans cette section, vous allez utiliser Azure Cloud Shell avec l’[extension IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) pour inscrire un appareil simulé.

1. Exécutez les commandes suivantes dans Azure Cloud Shell pour créer l’identité d’appareil.

   **YourIoTHubName** : Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

   **MyDevice** : il s’agit du nom donné à l’appareil inscrit. Utilisez MyDevice comme indiqué. Si vous choisissez un autre nom pour votre appareil, vous devez également utiliser ce nom pour l’ensemble de cet article et mettre à jour le nom de l’appareil dans les exemples d’application avant de les exécuter.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Exécutez les commandes suivantes dans Azure Cloud Shell pour obtenir la *chaîne de connexion* à l’appareil que vous venez d’inscrire :

   **YourIoTHubName** : Remplacez l’espace réservé ci-dessous par le nom que vous avez choisi pour votre hub IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Notez la chaîne de connexion de l’appareil, qui ressemble à l’exemple suivant :

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Vous utiliserez cette valeur plus loin dans ce démarrage rapide.

## <a name="communicate-between-device-and-service-via-device-streams"></a>Communiquer entre appareil et service par le biais de flux d’appareil

Dans cette section, vous exécutez l’application côté appareil et l’application côté service et communiquez entre les deux.

### <a name="run-the-device-side-application"></a>Exécuter l’application côté appareil

Pour exécuter l’application côté appareil, vous avez besoin d’effectuer les étapes suivantes :

1. Fournissez les informations d’identification de votre appareil en modifiant le fichier source `iothub_client_c2d_streaming_sample.c` dans le dossier `iothub_client/samples/iothub_client_c2d_streaming_sample` et en indiquant la chaîne de connexion de votre appareil.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[device connection string]";
   ```

2. Compilez le code comme suit :

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

3. Exécutez le programme compilé :

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>Exécuter l’application côté service

Comme mentionné précédemment, le kit de développement logiciel (SDK) C IoT Hub prend uniquement en charge des flux d’appareil côté appareil. Pour générer et exécuter l’application côté service, suivez les étapes disponibles dans l’un des guides de démarrage rapide suivants :

* [Communiquer avec une application d’appareil en C# par le biais de flux d'appareil IoT Hub](./quickstart-device-streams-echo-csharp.md)

* [Communiquer avec une application d’appareil dans Node.js par le biais de flux d'appareil IoT Hub](./quickstart-device-streams-echo-nodejs.md).

## <a name="clean-up-resources"></a>Supprimer des ressources

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez configuré un hub IoT, inscrit un appareil, établi un flux d’appareil entre une application C sur l’appareil et une autre application côté service, puis utilisé le flux pour échanger des données entre les applications.

Utilisez les liens ci-dessous pour en savoir plus sur les flux d’appareils :

> [!div class="nextstepaction"]
> [Vue d’ensemble des flux d’appareil](./iot-hub-device-streams-overview.md)