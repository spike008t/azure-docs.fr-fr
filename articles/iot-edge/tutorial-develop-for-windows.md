---
title: Développer un module pour les appareils Windows - Azure IoT Edge | Microsoft Docs
description: Ce tutoriel vous aide à configurer votre machine de développement et vos ressources cloud afin de développer des modules IoT Edge à l’aide de conteneurs Windows pour des appareils Windows
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/20/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 891b64b8e31266360d718255dcd8e8a1f9fb597c
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306576"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>Didacticiel : Développer des modules IoT Edge pour les appareils Windows

Utilisez Visual Studio pour développer et déployer du code sur des appareils Windows exécutant IoT Edge.

Dans le guide de démarrage rapide, vous avez créé un appareil IoT Edge à l’aide d’une machine virtuelle Windows et déployé un module prédéfini à partir de la Place de marché Azure. Ce tutoriel vous montre comment développer et déployer votre propre code sur un appareil IoT Edge. Il constitue un bon préambule à tous les autres tutoriels, qui abordent plus en détail des langages de programmation ou des services Azure spécifiques. 

Ce tutoriel utilise l’exemple de déploiement d’un **module C sur un appareil Windows**. Nous avons choisi cet exemple pour sa simplicité ; vous pouvez donc étudier les outils de développement sans vous préoccuper de savoir si les bibliothèques adéquates sont installées. Après avoir assimilé les concepts de développement, vous pouvez choisir un langage ou un service Azure spécifique pour vous plonger dans les détails. 

Ce tutoriel vous montre comment effectuer les opérations suivantes :

> [!div class="checklist"]
> * Configurer votre machine de développement
> * Utiliser les outils IoT Edge pour Visual Studio afin de créer un projet
> * Générer votre projet sous forme de conteneur et le stocker dans un registre de conteneurs Azure
> * Déployer votre code sur un appareil IoT Edge 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>Concepts clés

Ce tutoriel présente le développement d’un module IoT Edge. Un *module IoT Edge*, ou parfois simplement *module* dans sa forme abrégée, est un conteneur qui comporte du code exécutable. Vous pouvez déployer un ou plusieurs modules sur un appareil IoT Edge. Les modules effectuent des tâches spécifiques comme l’ingestion de données provenant de capteurs, la réalisation d’opérations d’analytique ou de nettoyage de données ou l’envoi de messages à un hub IoT. Pour plus d’informations, consultez [Présentation des modules Azure IoT Edge](iot-edge-modules.md).

Lors du développement d’un module IoT Edge, il est important de comprendre la différence entre la machine de développement et l’appareil IoT Edge cible où le module sera finalement déployé. Le conteneur que vous générez pour le stockage du code du module doit correspondre au système d’exploitation (OS) de l’*appareil cible*. Pour le développement de conteneur Windows, ce concept est plus simple, car les conteneurs Windows s’exécutent uniquement sur les systèmes d’exploitation Windows. Mais vous pourriez, par exemple, utiliser votre machine de développement Windows pour générer des modules pour les appareils Linux IoT Edge. Dans ce scénario, vous seriez obligé de vous assurer que votre machine de développement exécute des conteneurs Linux. À mesure que vous parcourez ce tutoriel, gardez à l’esprit la différence entre *système d’exploitation de la machine de développement* et *système d’exploitation du conteneur*.

Ce tutoriel cible les appareils Windows exécutant IoT Edge. Les appareils IoT Edge Windows utilisent des conteneurs Windows. Visual Studio étant recommandé pour développer pour les appareils Windows, nous allons l’utiliser dans le cadre de ce tutoriel. Vous pouvez aussi bien utiliser Visual Studio Code, bien qu’il existe des différences de prise en charge entre les deux outils.

Le tableau suivant liste les scénarios de développement pris en charge pour des **conteneurs Windows** dans Visual Studio Code et Visual Studio.

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Services Azure** | Azure Functions <br> Azure Stream Analytics |   |
| **Langues** | C# (débogage non pris en charge) | C <br> C# |
| **Plus d’informations** | [Azure IoT Edge pour Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Azure IoT Edge Tools pour Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools), [Azure IoT Edge Tools pour Visual Studio 2019](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

Ce tutoriel décrit les étapes de développement pour Visual Studio 2019. Si vous préférez utiliser Visual Studio Code, reportez-vous aux instructions dans [Utiliser Visual Studio Code pour développer et déboguer des modules pour Azure IoT Edge](how-to-vs-code-develop-module.md). Si vous utilisez Visual Studio 2017 (version 15.7 ou supérieure), téléchargez et installez [Azure IoT Edge Tools pour Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools).

## <a name="prerequisites"></a>Prérequis

Une machine de développement :

* Windows 10 avec la mise à jour 1809 ou une mise à jour plus récente.
* Vous pouvez utiliser votre propre ordinateur ou une machine virtuelle, selon vos préférences de développement.
* Installez [Git](https://git-scm.com/). 
* Installez le SDK C Azure IoT pour Windows x64 par le biais de vcpkg :

   ```powershell
   git clone https://github.com/Microsoft/vcpkg
   cd vcpkg
   .\bootstrap-vcpkg.bat
   .\vcpkg install azure-iot-sdk-c:x64-windows
   .\vcpkg --triplet x64-windows integrate install
   ```

<!--vcpkg only required for C development-->

Un appareil Azure IoT Edge sur Windows :

* Nous vous déconseillons d’exécuter IoT Edge sur votre machine de développement ; utilisez un appareil distinct à la place. Cette distinction entre machine de développement et appareil IoT Edge reflète plus fidèlement un scénario de déploiement réel et aide à assimiler les différents concepts.
* Si vous ne disposez pas d’un deuxième appareil, suivez l’article de démarrage rapide permettant de créer un appareil IoT Edge dans Azure avec une [machine virtuelle Windows](quickstart.md).

Ressources cloud :

* Un [hub IoT](../iot-hub/iot-hub-create-through-portal.md) de niveau gratuit ou standard dans Azure. 

## <a name="install-container-engine"></a>Installer le moteur de conteneur

Les modules IoT Edge étant empaquetés en tant que conteneurs, vous avez besoin d’un moteur de conteneur sur votre machine de développement pour générer et gérer les conteneurs. Nous vous recommandons d’utiliser Docker Desktop pour le développement en raison de ses nombreuses fonctionnalités et de sa popularité comme moteur de conteneur. Avec Docker Desktop sur un ordinateur Windows, vous pouvez basculer entre les conteneurs Linux et Windows, et ainsi aisément développer des modules pour différents types d’appareils IoT Edge. 

Utilisez la documentation Docker pour effectuer les différentes procédures d’installation sur votre machine de développement : 

* [Installer Docker Desktop pour Windows](https://docs.docker.com/docker-for-windows/install/)

  * Quand vous installez Docker Desktop pour Windows, vous êtes invité à indiquer si vous souhaitez utiliser des conteneurs Linux ou Windows. Pour ce tutoriel, utilisez des **conteneurs Windows**. Pour plus d’informations, consultez [Switch between Windows and Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).


## <a name="set-up-visual-studio-and-tools"></a>Configurer Visual Studio et les outils

Utilisez les extensions IoT pour Visual Studio 2019 pour développer des modules IoT Edge. Ces extensions fournissent des modèles de projet, automatisent la création du manifeste de déploiement et vous permettent de superviser et de gérer des appareils IoT Edge. Dans cette section, vous installez Visual Studio et l’extension IoT Edge, puis configurez votre compte Azure pour gérer les ressources IoT Hub à partir de Visual Studio. 

1. Si vous ne disposez pas de Visual Studio sur votre machine de développement, [installez Visual Studio 2019](https://docs.microsoft.com/visualstudio/install/install-visual-studio) avec les charges de travail suivantes : 

   * Développement Azure
   * Développement Desktop en C++
   * Développement multiplateforme .NET Core

1. Si vous disposez déjà de Visual Studio 2019 sur votre machine de développement. Suivez les étapes indiquées dans [Modifier Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) pour ajouter les charges de travail nécessaires si vous ne les avez pas déjà.

2. Téléchargez et installez l’extension [Azure IoT Edge Tools](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) pour Visual Studio 2019. 

3. Une fois les installations terminées, ouvrez Visual Studio 2019 et sélectionnez **Continuer sans code**.

4. Sélectionnez **Affichage** > **Cloud Explorer**. 

5. Sélectionnez l’icône de profil dans Cloud Explorer et connectez-vous à votre compte Azure si vous n’êtes pas déjà connecté. 

6. Une fois que vous êtes connecté, vos abonnements Azure sont listés. Sélectionnez les abonnements auxquels vous souhaitez accéder via Cloud Explorer, puis sélectionnez **Appliquer**. 

7. Développez successivement votre abonnement, **IoT Hubs** et votre hub IoT. Vous devez voir une liste de vos appareils IoT et pouvez utiliser cet explorateur pour les gérer. 

   ![Accéder aux ressources IoT Hub dans Cloud Explorer](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Créer un projet de module

L’extension Azure IoT Edge Tools fournit des modèles de projet pour tous les langages de module IoT Edge pris en charge dans Visual Studio. Ces modèles ont tous les fichiers et tout le code dont vous avez besoin pour déployer un module opérationnel afin de tester IoT Edge. Vous pouvez également vous servir de ces modèles comme point de départ et les personnaliser avec votre propre logique métier. 

1. Sélectionnez **Fichier** > **Nouveau** > **Projet...**

2. Dans la fenêtre du nouveau projet, 2. Dans la fenêtre Nouveau projet, recherchez **IoT Edge**, puis sélectionnez le projet **Azure IoT Edge (Windows amd64)** . Cliquez sur **Suivant**. 

   ![Créer un projet Azure IoT Edge](./media/tutorial-develop-for-windows/new-project.png)

3. Dans la fenêtre Configurer votre nouveau projet, renommez le projet et la solution à l’aide d’un nom descriptif comme **CTutorialApp**. Cliquez sur **Créer** pour créer le projet.

   ![Configurer un nouveau projet Azure IoT Edge](./media/tutorial-develop-for-windows/configure-project.png)
 

4. Dans l’application et la fenêtre du module IoT Edge, configurez votre projet à l’aide des valeurs suivantes : 

   | Champ | Valeur |
   | ----- | ----- |

   | Sélectionnez un modèle | Sélectionnez **Module C**. | | Nom de projet du module | Acceptez les valeurs par défaut **IoTEdgeModule1**. | | Référentiel d’images Docker | Un référentiel d’images comprend le nom de votre registre de conteneurs et celui de votre image conteneur. Votre image conteneur est préremplie avec le nom du projet de module. Remplacez **localhost:5000** par la valeur de serveur de connexion de votre registre de conteneurs Azure. Vous pouvez récupérer le serveur de connexion à partir de la page Vue d’ensemble de votre registre de conteneurs dans le Portail Azure. <br><br> Le dépôt d’images final ressemble à ceci : \<nom_registre\>.azurecr.io/iotedgemodule1. |

   ![Configurer votre projet pour l’appareil cible, le type de module et le registre de conteneurs](./media/tutorial-develop-for-windows/add-application-and-module.png)

5. Sélectionnez **OK** pour enregistrer vos modifications. 

Une fois votre nouveau projet chargé dans la fenêtre Visual Studio, prenez un moment pour vous familiariser avec les fichiers qu’elle a créés : 

* Un projet IoT Edge appelé **AzureIoTEdgeApp1.Windows.Amd64**.
    * Le dossier **Modules** contient des pointeurs vers les modules inclus dans le projet. Dans ce cas, ce doit simplement être IoTEdgeModule1. 
    * Le fichier **deployment.template.json** est un modèle qui vous aide à créer un manifeste de déploiement. Un *manifeste de déploiement* est un fichier qui définit exactement les modules à déployer sur un appareil, leur configuration et la façon dont ils peuvent communiquer entre eux et avec le cloud. 
* Un projet de module IoT Edge appelé **IoTEdgeModule1**.
    * Le fichier **main.c** contient le code du module C par défaut qui est fourni avec le modèle de projet. Le module par défaut prend des données issues d’une source en guise d’entrée et les transmet au hub IoT. 
    * Le fichier **module.json** contient les détails du module, notamment le dépôt d’images complet, la version de l’image et le fichier Dockerfile à utiliser pour chaque plateforme prise en charge.

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>Fournir vos informations d’identification de registre à l’agent IoT Edge

Le runtime IoT Edge a besoin des informations d’identification de registre pour extraire vos images de conteneur sur l’appareil IoT Edge. Ajoutez ces informations d’identification au modèle de déploiement. 

1. Ouvrez le fichier **deployment.template.json**.

2. Recherchez la propriété **registryCredentials** dans les propriétés $edgeAgent souhaitées. 

3. Mettez à jour la propriété avec vos informations d’identification, en respectant le format suivant : 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }

4. Save the deployment.template.json file. 

### Review the sample code

The solution template that you created includes sample code for an IoT Edge module. This sample module simply receives messages and then passes them on. The pipeline functionality demonstrates an important concept in IoT Edge, which is how modules communicate with each other.

Each module can have multiple *input* and *output* queues declared in their code. The IoT Edge hub running on the device routes messages from the output of one module into the input of one or more modules. The specific language for declaring inputs and outputs varies between languages, but the concept is the same across all modules. For more information about routing between modules, see [Declare routes](module-composition.md#declare-routes).

1. In the **main.c** file, find the **SetupCallbacksForModule** function.

2. This function sets up an input queue to receive incoming messages. It calls the C SDK module client function [SetInputMessageCallback](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-setinputmessagecallback). Review this function and see that it initializes an input queue called **input1**. 

   ![Find the input name in the SetInputMessageCallback constructor](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. Next, find the **InputQueue1Callback** function.

4. This function processes received messages and sets up an output queue to pass them along. It calls the C SDK module client function [SendEventToOutputAsync](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-sendeventtooutputasync). Review this function and see that it initializes an output queue called **output1**. 

   ![Find the output name in the SendEventToOutputAsync constructor](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. Open the **deployment.template.json** file.

6. Find the **modules** property of the $edgeAgent desired properties. 

   There should be two modules listed here. The first is **tempSensor**, which is included in all the templates by default to provide simulated temperature data that you can use to test your modules. The second is the **IotEdgeModule1** module that you created as part of this project.

   This modules property declares which modules should be included in the deployment to your device or devices. 

7. Find the **routes** property of the $edgeHub desired properties. 

   One of the functions if the IoT Edge hub module is to route messages between all the modules in a deployment. Review the values in the routes property. The first route, **IotEdgeModule1ToIoTHub**, uses a wildcard character (**\***) to include any message coming from any output queue in the IoTEdgeModule1 module. These messages go into *$upstream*, which is a reserved name that indicates IoT Hub. The second route, **sensorToIotEdgeModule1**, takes messages coming from the tempSensor module and routes them to the *input1* input queue of the IotEdgeModule1 module. 

   ![Review routes in deployment.template.json](./media/tutorial-develop-for-windows/deployment-routes.png)


## Build and push your solution

You've reviewed the module code and the deployment template to understand some key deployment concepts. Now, you're ready to build the IotEdgeModule1 container image and push it to your container registry. With the IoT tools extension for Visual Studio, this step also generates the deployment manifest based on the information in the template file and the module information from the solution files. 

### Sign in to Docker

Provide your container registry credentials to Docker on your development machine so that it can push your container image to be stored in the registry. 

1. Open PowerShell or a command prompt.

2. Sign in to Docker with the Azure container registry credentials that you saved after creating the registry. 

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   Il se peut que vous receviez un avertissement de sécurité recommandant d’utiliser `--password-stdin`. Bien qu’il s’agisse de la bonne pratique recommandée pour les scénarios de production, elle n’est pas pertinente pour ce tutoriel. Pour plus d’informations, consultez les informations de référence sur [docker login](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin).

### <a name="build-and-push"></a>Générer et envoyer (push)

Votre machine de développement a désormais accès à votre registre de conteneurs ainsi que vos appareils IoT Edge. Il est temps de transformer le code du projet en image conteneur. 

1. Cliquez avec le bouton droit sur le dossier de projet **AzureIotEdgeApp1.Windows.Amd64**, puis sélectionnez **Générer et envoyer (push) les modules IoT Edge**. 

   ![Générer et envoyer (push) les modules IoT Edge](./media/tutorial-develop-for-windows/build-and-push-modules.png)

   La commande de génération et d’envoi (push) déclenche trois opérations. Tout d’abord, elle crée un dossier dans la solution appelé **config** contenant les manifestes de déploiement en entier. Il est généré à partir des informations dans le modèle de déploiement et d’autres fichiers de solution. Ensuite, elle exécute `docker build` pour générer l’image de conteneur basée sur le fichier docker correspondant à votre architecture cible. Puis, elle exécute `docker push` pour envoyer (push) le dépôt d’images vers votre registre de conteneurs. 

   Ce processus peut prendre plusieurs minutes la première fois, mais il est plus rapide la prochaine fois que vous exécutez les commandes. 

2. Ouvrez le fichier **deployment.windows-amd64.json** dans le dossier config créé. (Il est possible que le dossier config n’apparaisse pas dans l’Explorateur de solutions dans Visual Studio. Si tel est le cas, sélectionnez l’icône **Afficher tous les fichiers** dans la barre des tâches de l’Explorateur de solutions.)

3. Recherchez le paramètre **image** de la section IotEdgeModule1. Notez que l’image contient le dépôt d’images complet ; l’étiquette du nom, de la version et de l’architecture provient du fichier module.json.

4. Ouvrez le fichier **module.json** dans le dossier IotEdgeModule1. 

5. Changez le numéro de version de l’image de module. (La version, pas $schema-version.) Par exemple, incrémentez le numéro de version de correctif en le définissant sur **0.0.2** comme si nous avions apporté une petite correction dans le code du module. 

   >[!TIP]
   >Les versions de module activent la gestion de version, ce qui vous permet de tester les modifications sur un petit ensemble d’appareils avant de déployer des mises à jour en production. Si vous n’incrémentez pas la version du module avant d’effectuer les opérations de génération et d’envoi (push), vous remplacez le dépôt dans votre registre de conteneurs. 

6. Enregistrez les modifications apportées au fichier module.json.

7. Recliquez avec le bouton droit sur le dossier de projet **AzureIotEdgeApp1.Windows.Amd64**, puis resélectionnez **Générer et envoyer (push) les modules IoT Edge**. 

8. Rouvrez le fichier **deployment.windows-amd64.json**. Notez qu’aucun fichier n’a été créé quand vous avez réexécuté la commande de génération et d’envoi (push). Au lieu de cela, le même fichier a été mis à jour pour refléter les modifications. L’image IotEdgeModule1 pointe désormais vers la version 0.0.2 du conteneur. Cette modification dans le manifeste de déploiement permet d’indiquer à l’appareil IoT Edge qu’il existe une nouvelle version d’un module à extraire. 

9. Pour vérifier davantage ce qu’a fait la commande de génération et d’envoi (push), accédez au [portail Azure](https://portal.azure.com), puis à votre registre de conteneurs. 

10. Dans votre registre de conteneurs, sélectionnez **Dépôts**, puis **iotedgemodule1**. Vérifiez que les deux versions de l’image ont été envoyées (push) au registre.

    ![Afficher les deux versions de l’image dans le registre de conteneurs](./media/tutorial-develop-for-windows/view-repository-versions.png)

### <a name="troubleshoot"></a>Résolution des problèmes

Si vous rencontrez des erreurs lors de la génération et de l’envoi (push) de votre image de module, elles sont souvent liées à la configuration de Docker sur votre machine de développement. Pour passer en revue votre configuration, effectuez les vérifications suivantes : 

* Avez-vous exécuté la commande `docker login` en utilisant les informations d’identification que vous avez copiées à partir de votre registre de conteneurs ? Ces informations d’identification sont différentes de celles que vous utilisez pour vous connecter à Azure. 
* Votre référentiel de conteneurs est-il correct ? A-t-il les noms de registre de conteneur et de module appropriés ? Ouvrez le fichier **module.json** dans le dossier IotEdgeModule1 pour vérifier. La valeur du dépôt doit ressembler à **\<nom_registre\>.azurecr.io/iotedgemodule1**. 
* Si vous avez utilisé un autre nom que **IotEdgeModule1** pour votre module, ce nom est-il cohérent dans l’ensemble de la solution ?
* Votre machine exécute-t-elle le même type de conteneurs que celui que vous générez ? Ce tutoriel concernant les appareils IoT Edge Windows, vos fichiers Visual Studio doivent avoir l’extension **windows-amd64** et Docker Desktop doit exécuter des conteneurs Windows. 

## <a name="deploy-modules-to-device"></a>Déployer des modules sur un appareil

Vous avez vérifié que les images de conteneur générées sont stockées dans votre registre de conteneurs. Il est donc temps de les déployer sur un appareil. Vérifiez que votre appareil IoT Edge est opérationnel. 

1. Ouvrez Cloud Explorer dans Visual Studio et développez les détails de votre hub IoT. 

2. Sélectionnez le nom de l’appareil sur lequel vous souhaitez effectuer l’opération de déploiement. Dans la liste **Actions**, sélectionnez **Créer le déploiement**.

   ![Créer un déploiement pour un seul appareil](./media/tutorial-develop-for-windows/create-deployment.png)


3. Dans l’Explorateur de fichiers, accédez au dossier config de votre projet et sélectionnez le fichier **deployment.windows-amd64.json**. Ce fichier se trouve souvent à l’emplacement suivant : `C:\Users\<username>\source\repos\AzureIotEdgeApp1\AzureIotEdgeApp1.Windows.Amd64\config\deployment.windows-amd64.json`

   N’utilisez pas le fichier deployment.template.json, qui ne contient pas les valeurs d’image de module complètes. 

4. Développez les détails de votre appareil IoT Edge dans Cloud Explorer pour voir les modules sur votre appareil.

5. Utilisez le bouton **Actualiser** pour mettre à jour l’état de l’appareil afin de voir que les modules IotEdgeModule1 et tempSensor sont déployés sur votre appareil. 


   ![Afficher les modules s’exécutant sur votre appareil IoT Edge](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>Afficher les messages provenant de l’appareil

Le code IotEdgeModule1 reçoit des messages via sa file d’attente d’entrée et les transmet à sa file d’attente de sortie. Le manifeste de déploiement a déclaré des routes qui ont passé les messages depuis tempSensor vers IotEdgeModule1, puis vers le hub IoT. Les outils Azure IoT Edge pour Visual Studio vous permettent de voir les messages à mesure qu’ils arrivent au hub IoT en provenance de vos différents appareils. 

1. Dans Visual Studio Cloud Explorer, sélectionnez le nom de l’appareil IoT Edge sur lequel vous avez effectué l’opération de déploiement. 

2. Dans le menu **Actions**, sélectionnez **Start Monitoring Built-in Event Endpoint** (Démarrer la supervision du point de terminaison d’événements intégré).

3. Regardez la section **Sortie** dans Visual Studio pour voir les messages arrivant à votre hub IoT. 

   Le démarrage des deux modules peut prendre plusieurs minutes. Le runtime IoT Edge doit recevoir son nouveau manifeste de déploiement, extraire les images de module à partir du runtime du conteneur, puis démarrer chaque nouveau module. Si vous 

   ![Afficher les messages appareil-à-cloud entrants](./media/tutorial-develop-for-windows/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Afficher les modifications sur l’appareil

Si vous souhaitez voir ce qui se passe sur votre appareil lui-même, utilisez les commandes de cette section pour inspecter le runtime IoT Edge et les modules en cours d’exécution sur votre appareil. 

Les commandes de cette section concernent votre appareil IoT Edge, pas votre machine de développement. Si vous utilisez une machine virtuelle pour votre appareil IoT Edge, connectez-vous-y maintenant. Dans Azure, accédez à la page de vue d’ensemble de la machine virtuelle et sélectionnez **Se connecter** pour accéder à la connexion Bureau à distance. Sur l’appareil, ouvrez une commande ou une fenêtre PowerShell pour exécuter les commandes `iotedge`.

* Affichez tous les modules déployés sur votre appareil et vérifiez leur état :

   ```cmd
   iotedge list
   ```

   Vous devez voir quatre modules : les deux modules du runtime IoT Edge, tempSensor et IotEdgeModule1. Les quatre doivent être listés comme étant en cours d’exécution.

* Examinez les journaux d’activité d’un module spécifique :

   ```cmd
   iotedge logs <module name>
   ```

   Les modules IoT Edge respectent la casse. 

   Les journaux d’activité de SamplModule et IotEdgeModule1 doivent afficher les messages qu’ils traitent. Le module edgeAgent étant responsable du démarrage des autres modules, ses journaux d’activité contiennent des informations sur l’implémentation du manifeste de déploiement. Si un module n’est pas listé ou n’est pas en cours d’exécution, les journaux d’activité edgeAgent sont susceptibles de contenir des erreurs. Le module edgeHub est responsable des communications entre les modules et le hub IoT. Si les modules sont opérationnels, mais que les messages n’atteignent pas votre hub IoT, les journaux d’activité edgeHub sont susceptibles de contenir des erreurs. 

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez configuré Visual Studio 2019 sur votre machine de développement et déployé votre premier module IoT Edge à partir de celle-ci. Les concepts de base n’ayant plus de secret pour vous, essayez d’ajouter des fonctionnalités à un module afin qu’il puisse analyser les données transitant par son intermédiaire. Choisissez votre langage préféré : 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)