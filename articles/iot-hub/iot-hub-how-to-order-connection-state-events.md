---
title: Commander des événements de connexion d’appareils depuis Azure IoT Hub à l’aide d’Azure Cosmos DB | Microsoft Docs
description: Cet article décrit comment commander et enregistrer des événements de connexion d’appareils à partir d’Azure IoT Hub à l’aide d’Azure Cosmos DB pour conserver le dernier état de connexion
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: asrastog
ms.openlocfilehash: f4baab6e0909144efc613572207e7f24c4b4fe1f
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743269"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Commander des événements de connexion d’appareils depuis Azure IoT Hub à l’aide d’Azure Cosmos DB

Azure Event Grid vous permet de créer des applications basées sur des événements et d’intégrer facilement des événements IoT dans vos solutions métier. Cet article vous guide au travers d’une configuration pouvant être utilisée pour suivre et stocker le dernier état de connexion d’un appareil dans Cosmos DB. Nous utiliserons le numéro de séquence disponible dans les événements de connexion et de déconnexion d’appareils et nous stockerons l’état le plus récent dans Cosmos DB. Nous allons utiliser une procédure stockée, qui est une logique d’application exécutée sur une collection dans Cosmos DB.

Le numéro de séquence est la représentation d’un nombre hexadécimal sous la forme d’une chaîne. Vous pouvez utiliser la comparaison de chaînes pour identifier le plus grand nombre. Si vous convertissez la chaîne en hexadécimal, vous obtenez un nombre de 256 bits. Le numéro de séquence est strictement croissante, et le dernier événement aura un nombre plus élevé que les autres événements. Cela est utile en cas de connexion et de déconnexion fréquentes d’appareils, et si vous souhaitez vous assurer que seul le dernier événement est utilisé pour déclencher une action en aval, étant donné que Azure Event Grid ne prend pas en charge la commande d’événements.

## <a name="prerequisites"></a>Conditions préalables

* Un compte Azure actif. Si vous n’en avez pas, vous pouvez créer un [compte gratuit](https://azure.microsoft.com/pricing/free-trial/).

* Un compte d’API SQL Azure Cosmos DB actif. Si vous n’en avez pas encore créé un, consultez [Créer un compte de base de données](../cosmos-db/create-sql-api-dotnet.md#create-an-azure-cosmos-db-account) pour obtenir une procédure pas à pas.

* Une collection dans votre base de données. Consultez [Ajouter une collection](../cosmos-db/create-sql-api-dotnet.md#add-a-database-and-a-collection) pour une procédure pas à pas. Lorsque vous créez votre collection, utilisez `/id` pour la clé de partition.

* Un Hub IoT dans Azure. Si vous n’en avez pas encore créé un, consultez [Prise en main d’IoT Hub](iot-hub-csharp-csharp-getstarted.md) pour obtenir une procédure pas à pas.

## <a name="create-a-stored-procedure"></a>Créer une procédure stockée

Tout d’abord, créez une procédure stockée et configurez-la pour exécuter une logique qui compare les numéros de séquence d’événements entrants et qui enregistre le dernier événement par appareil dans la base de données.

1. Dans votre API SQL Cosmos DB, sélectionnez **Explorateur de données** > **Éléments** > **Nouvelle procédure stockée**.

   ![Créer une procédure stockée](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Entrez **LatestDeviceConnectionState** pour l’ID de la procédure stockée et collez le texte suivant dans le **corps de la procédure stockée**. Notez que ce code doit remplacer tout code existant dans le corps de la procédure stockée. Ce code tient à jour une ligne par ID d’appareil et enregistre le dernier état de connexion de cet ID d’appareil en identifiant le numéro de séquence le plus élevé.

    ```javascript
    // SAMPLE STORED PROCEDURE
    function UpdateDevice(deviceId, moduleId, hubName, connectionState, connectionStateUpdatedTime, sequenceNumber) {
      var collection = getContext().getCollection();
      var response = {};

      var docLink = getDocumentLink(deviceId, moduleId);

      var isAccepted = collection.readDocument(docLink, function(err, doc) {
        if (err) {
          console.log('Cannot find device ' + docLink + ' - ');
          createDocument();
        } else {
          console.log('Document Found - ');
          replaceDocument(doc);
        }
      });

      function replaceDocument(document) {
        console.log(
          'Old Seq :' +
            document.sequenceNumber +
            ' New Seq: ' +
            sequenceNumber +
            ' - '
        );
        if (sequenceNumber > document.sequenceNumber) {
          document.connectionState = connectionState;
          document.connectionStateUpdatedTime = connectionStateUpdatedTime;
          document.sequenceNumber = sequenceNumber;

          console.log('replace doc - ');

          isAccepted = collection.replaceDocument(docLink, document, function(
            err,
            updated
          ) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(updated);
            }
          });
        } else {
          getContext()
            .getResponse()
            .setBody('Old Event - current: ' + document.sequenceNumber + ' Incoming: ' + sequenceNumber);
        }
      }
      function createDocument() {
        document = {
          id: deviceId + '-' + moduleId,
          deviceId: deviceId,
          moduleId: moduleId,
          hubName: hubName,
          connectionState: connectionState,
          connectionStateUpdatedTime: connectionStateUpdatedTime,
          sequenceNumber: sequenceNumber
        };
        console.log('Add new device - ' + collection.getAltLink());
        isAccepted = collection.createDocument(
          collection.getAltLink(),
          document,
          function(err, doc) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(doc);
            }
          }
        );
      }

      function getDocumentLink(deviceId, moduleId) {
        return collection.getAltLink() + '/docs/' + deviceId + '-' + moduleId;
      }
    }
    ```

3. Enregistrer la procédure stockée :

    ![enregistrer la procédure stockée](./media/iot-hub-how-to-order-connection-state-events/save-stored-procedure.png)

## <a name="create-a-logic-app"></a>Créer une application logique

Tout d’abord, créez une application logique et ajoutez un déclencheur Event Grid, qui surveille le groupe de ressources de votre machine virtuelle.

### <a name="create-a-logic-app-resource"></a>Créer une ressource d’application logique

1. Dans le [Azure portal](https://portal.azure.com), sélectionnez **+ créer une ressource**, sélectionnez **intégration** , puis **application logique**.

   ![Créer une application logique](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Donnez à votre application logique un nom qui est unique dans votre abonnement, puis sélectionnez les mêmes abonnement, groupe de ressources et emplacement que votre hub IoT.

   ![Nouvelle application logique](./media/iot-hub-how-to-order-connection-state-events/new-logic-app.png)

3. Sélectionnez **Créer** pour créer l’application logique.

   Vous venez de créer une ressource Azure pour votre application logique. Une fois qu’Azure a déployé votre application logique, le Concepteur d’applications logiques vous propose des modèles courants pour faciliter vos premiers pas.

   > [!NOTE]
   > Pour rechercher et ouvrir votre application logique, sélectionnez **groupes de ressources** et sélectionnez le groupe de ressources que vous utilisez pour cette procédure. Puis sélectionnez votre nouvelle application logique. Cette opération ouvre le Concepteur d’application logique.

4. Dans le Concepteur d’application logique, défilez vers la droite jusqu'à ce que vous voyiez déclencheurs courants. Sous **modèles**, choisissez **application logique vide** qui vous pouvez de générer votre application logique à partir de zéro.

### <a name="select-a-trigger"></a>Sélectionner un déclencheur

Un déclencheur désigne un événement spécifique qui démarre votre application logique. Pour ce didacticiel, le déclencheur qui lance le flux de travail reçoit une demande via HTTP.

1. Dans les connecteurs et les déclencheurs la barre de recherche, tapez **HTTP** et appuyez sur ENTRÉE.

2. Sélectionnez **Requête - Lors de la réception d’une requête HTTP** comme déclencheur.

   ![Sélectionner le déclencheur de requête HTTP](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. Sélectionnez **Utiliser l’exemple de charge utile pour générer le schéma**.

   ![Utiliser l’exemple de charge utile pour générer un schéma](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

4. Collez l’exemple de code JSON suivant dans la zone de texte, puis sélectionnez **Terminé** :

   ```json
   [{
    "id": "fbfd8ee1-cf78-74c6-dbcf-e1c58638ccbd",
    "topic":
      "/SUBSCRIPTIONS/DEMO5CDD-8DAB-4CF4-9B2F-C22E8A755472/RESOURCEGROUPS/EGTESTRG/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/MYIOTHUB",
    "subject": "devices/Demo-Device-1",
    "eventType": "Microsoft.Devices.DeviceConnected",
    "eventTime": "2018-07-03T23:20:11.6921933+00:00",
    "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
      "hubName": "MYIOTHUB",
      "deviceId": "48e44e11-1437-4907-83b1-4a8d7e89859e",
      "moduleId": ""
    },
    "dataVersion": "1",
    "metadataVersion": "1"
   }]
   ```

   ![Exemple de charge utile JSON de collage](./media/iot-hub-how-to-order-connection-state-events/paste-sample-payload.png)

5. Vous pouvez recevoir une notification contextuelle indiquant **N’oubliez pas d’inclure un en-tête Content-Type défini sur application/JSON dans votre demande**. Vous pouvez ignorer cette suggestion et passer à la section suivante.

### <a name="create-a-condition"></a>Créer une condition

Dans votre workflow d’application logique, les conditions contribuent à exécuter des actions spécifiques après avoir passé la condition en question. Une fois que la condition est remplie, une action souhaitée peut être définie. Pour ce didacticiel, la condition est de vérifier si l’eventType est appareil connecté ou appareil déconnecté. L’action sera l’exécution de la procédure stockée dans votre base de données.

1. Sélectionnez **+ nouvelle étape** puis **intégrés**, puis recherchez et sélectionnez **Condition**. Cliquez sur dans **choisir une valeur** et une fenêtre contextuelle montrant le contenu dynamique, les champs qui peuvent être sélectionnés. Renseignez les champs comme indiqué ci-dessous pour exécuter cette méthode uniquement pour les événements d’appareil connecté et déconnecté de l’appareil :

   * Choisissez une valeur : **eventType** --Sélectionnez cette option à partir des champs dans le contenu dynamique qui s’affichent lorsque vous cliquez sur ce champ.
   * Modification » est égale à » pour **se termine par**.
   * Choisissez une valeur : **nected**.

     ![Remplir une condition](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

2. Dans le **Si true** boîte de dialogue, cliquez sur **ajouter une action**.
  
   ![Ajouter une action si True](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

3. Recherchez Cosmos DB et sélectionnez **Azure Cosmos DB - exécuter la procédure stockée**

   ![Rechercher CosmosDB](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

4. Renseignez **cosmosdb-connexion** pour le **nom de la connexion** et sélectionnez l’entrée dans la table, puis sélectionnez **créer**. Vous voyez la **exécuter la procédure stockée** Panneau de configuration. Entrez les valeurs pour les champs :

   **ID de base de données**: ToDoList

   **ID de collection**: Éléments

   **ID de la procédure stockée**: LatestDeviceConnectionState

5. Sélectionnez **ajouter un nouveau paramètre**. Dans la liste déroulante qui s’affiche, cochez les cases en regard **clé de Partition** et **paramètres pour la procédure stockée**, puis cliquez sur n’importe où sur l’écran ; il ajoute un champ de valeur de clé de partition et un champ pour paramètres de la procédure stockée.

   ![remplir une action d’application logique](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Maintenant il est recommandé d’entrer la valeur de clé de partition et les paramètres comme indiqué ci-dessous. Veillez à placer dans les crochets et les guillemets doubles comme indiqué. Vous devrez sur **ajouter du contenu dynamique** pour obtenir les valeurs valides que vous pouvez utiliser ici.

   ![remplir une action d’application logique](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure-2.png)

7. En haut du volet intitulée **pour chaque**, sous **sélectionner une sortie des étapes précédentes**, vérifier qu’elle **corps** est sélectionné.

   ![remplir l’application logique pour chaque](./media/iot-hub-how-to-order-connection-state-events/logicapp-foreach-body.png)

8. Enregistrez votre application logique.

### <a name="copy-the-http-url"></a>Copier l’URL HTTP

Avant de quitter le Concepteur d’applications logiques, copiez l’URL qu’écoute votre application logique dans l’attente d’un déclencheur. Cette URL vous permet de configurer Event Grid.

1. Développez la zone de configuration de déclencheur **Lors de la réception d’une demande HTTP** en cliquant dessus.

2. Copiez la valeur du champ **URL HTTP POST** en sélectionnant le bouton de copie en regard de celui-ci.

   ![Copier l’URL HTTP POST](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Enregistrez cette URL afin de pouvoir y faire référence dans la section suivante.

## <a name="configure-subscription-for-iot-hub-events"></a>Configurer l’abonnement pour les événements IoT Hub

Dans cette section, vous configurez votre hub IoT pour publier des événements à mesure qu’ils se produisent.

1. Accédez à votre hub IoT dans le portail Azure.

2. Sélectionnez **Événements**.

   ![Ouvrir les détails Event Grid](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. Sélectionnez **+ abonnement aux événements**.

   ![Créer un abonnement aux événements](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Renseignez **détails de l’abonnement événement**: fournissez un nom descriptif et sélectionnez **Schéma Event Grid**.

5. Renseignez le **Types d’événements** champs. Dans la liste déroulante, sélectionnez uniquement **appareil connecté** et **périphérique déconnecté** à partir du menu. Cliquez sur n’importe où sur l’écran pour fermer la liste et enregistrer vos sélections.

   ![Définir les types d’événements à rechercher](./media/iot-hub-how-to-order-connection-state-events/set-event-types.png)

6. Pour **détails du point de terminaison**, sélectionnez le Type de point de terminaison **Webhook** et cliquez sur le point de terminaison sélectionnez et collez l’URL que vous avez copiée à partir de votre application logique et confirmer la sélection.

   ![sélectionner une url de point de terminaison](./media/iot-hub-how-to-order-connection-state-events/endpoint-select.png)

7. Le formulaire doit maintenant ressembler à l’exemple suivant :

   ![Exemple de formulaire d’abonnement aux événements](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

   Sélectionnez **Créer** pour enregistrer l’abonnement aux événements.

## <a name="observe-events"></a>Observez des événements

Maintenant que votre abonnement aux événements est configuré, nous allons le tester en vous connectant à un appareil.

### <a name="register-a-device-in-iot-hub"></a>Inscrire un appareil dans un IoT Hub

1. À partir de votre hub IoT, sélectionnez **Appareils IoT**.

2. Sélectionnez **+ ajouter** en haut du volet.

3. Pour **ID d’appareil**, entrez `Demo-Device-1`.

4. Sélectionnez **Enregistrer**.

5. Vous pouvez ajouter plusieurs appareils avec différents ID d’appareil.

   ![Appareils ajoutés à un hub](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Recliquez sur l’appareil ; maintenant les touches et les chaînes de connexion seront renseignés. Copiez la **Chaîne de connexion - clé primaire** pour l’utiliser ultérieurement.

   ![ConnectionString pour appareil](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Démarrer le simulateur Raspberry Pi

Nous allons utiliser le simulateur web Raspberry Pi pour simuler la connexion de l’appareil.

[Démarrer le simulateur Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Exécuter un exemple d’application sur le simulateur web Raspberry Pi

Cela déclenchera un événement d’appareil connecté.

1. Dans la zone de codage, remplacez l’espace réservé à la ligne 15 par votre chaîne de connexion d’appareil Azure IoT Hub que vous avez enregistré à la fin de la section précédente.

   ![Collez dans la chaîne de connexion d’appareil](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Exécutez l’application en sélectionnant **exécuter**.

Vous consultez quelque chose de similaire à la sortie suivante qui affiche les données de capteur et les messages qui sont envoyés à votre IoT hub.

   ![Exécution de l'application](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Cliquez sur **Arrêter** pour arrêter le simulateur et déclencher un événement **d’appareil déconnecté**.

Vous avez exécuté un exemple d’application pour collecter des données de capteur et les envoyer à votre IoT Hub.

### <a name="observe-events-in-cosmos-db"></a>Observez des événements dans Cosmos DB

Vous pouvez voir des résultats de la procédure stockée exécutée dans votre document Cosmos DB. Voici à quoi ils ressemblent. Chaque ligne contient le dernier état de connexion de chaque appareil.

   ![Résultat du guide](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Utilisation de l’interface de ligne de commande Microsoft Azure

Au lieu d’utiliser le [portail Azure](https://portal.azure.com), vous pouvez effectuer les étapes IoT Hub à l’aide de l’interface Azure CLI. Pour plus d’informations, consultez les pages de l’interface de ligne de commande Azure consacrées à la [création d’un abonnement aux événements](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) et à la [création d’un appareil IoT](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create).

## <a name="clean-up-resources"></a>Supprimer des ressources

Ce didacticiel utilise des ressources qui peuvent entraîner des frais sur votre abonnement Azure. Lorsque vous avez terminé le tester le didacticiel et vos résultats, désactiver ou supprimer des ressources que vous ne souhaitez pas conserver.

Pour ne pas perdre le travail effectué sur votre application logique, désactivez-la au lieu de la supprimer.

1. Accédez à votre application logique.

2. Dans le panneau **Vue d’ensemble**, sélectionnez **Supprimer** ou **Désactiver**.

    Chaque abonnement peut avoir un hub IoT gratuit. Si vous avez créé un hub gratuit pour ce didacticiel, vous n’avez pas besoin de le supprimer pour éviter les frais.

3. Accédez à votre hub IoT.

4. Dans le panneau **Vue d’ensemble**, sélectionnez **Supprimer**.

    Même si vous conservez votre hub IoT, vous pouvez souhaiter supprimer l’abonnement aux événements que vous avez créé.

5. Dans votre hub IoT, sélectionnez **Grille d’événement**.

6. Sélectionnez l’abonnement aux événements que vous souhaitez supprimer.

7. Sélectionnez **Supprimer**.

Pour supprimer un compte Azure Cosmos DB du portail Azure, cliquez avec le bouton droit sur le nom du compte et cliquez sur **Supprimer le compte**. Consultez les instructions détaillées pour la [suppression d’un compte Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/manage-account).

## <a name="next-steps"></a>Étapes suivantes

* Découvrez-en plus sur la [réaction aux événements IoT Hub en utilisant Event Grid pour déclencher des actions](../iot-hub/iot-hub-event-grid.md)

* [Suivre le didacticiel d’événements IoT Hub](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* Découvrez ce que vous pouvez faire d’autre avec [Event Grid](../event-grid/overview.md)