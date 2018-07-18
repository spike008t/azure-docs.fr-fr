---
title: Intégrer LUIS dans un bot à l’aide du Kit de développement logiciel (SDK) Bot Builder pour C# dans Azure | Microsoft Docs
description: Créer un bot intégré avec une application LUIS à l’aide de Bot Framework.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: v-geberr
ms.openlocfilehash: 273fca51d83f25e2657b51b68179e46f4295f92b
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263878"
---
# <a name="web-app-bot-using-the-luis-template-for-c"></a>Bot d’application web utilisant le modèle LUIS pour C#

Créer un bot conversationnel avec compréhension de langage intégrée.

## <a name="prerequisite"></a>Configuration requise

* [Application LUIS HomeAutomation](luis-get-started-create-app.md). Les intentions de cette application LUIS sont mappées vers les gestionnaires de boîtes de dialogue de l’outil. 

## <a name="luis-homeautomation-intents"></a>Intentions HomeAutomation de LUIS

| Intention | Exemple d’énoncé | Fonctionnalité du bot |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Allume les lumières. | Lorsque l’intention `HomeAutomation.TurnOn` de LUIS est détectée, le bot appelle le gestionnaire de boîte de dialogue `OnIntent`. Dans ce dialogue, vous appelez un service IoT pour allumer un appareil et indiquer à l’utilisateur qu’il a été allumé. |
| HomeAutomation.TurnOff | Éteins les lumières de la chambre. | Lorsque l’intention `HomeAutomation.TurnOff` de LUIS est détectée, le bot appelle le gestionnaire de boîte de dialogue `OffIntent`. Dans ce dialogue, vous appelez un service IoT pour éteindre un appareil et indiquer à l’utilisateur qu’il a été éteint. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Créer un bot Language Understanding avec Bot Service

1. Sur le [Portail Azure](https://portal.azure.com), sélectionnez **Créer une ressource** en haut à gauche dans le menu.

    ![Créer une ressource](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. Dans la zone de recherche, recherchez **Web App Bot**. 

    ![Créer une ressource](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Dans la fenêtre Web App Bot, cliquez sur **Créer**.

4. Dans **Bot Service**, fournissez les informations requises, puis cliquez sur **Créer**. Le service de bot et l’application LUIS sont alors déployés vers Azure. Si vous souhaitez utiliser la [préparation vocale](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), consultez les [conditions régionales](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) avant de créer votre bot. 
    * Dans **Nom de l’application**, entrez le nom de votre bot. Il sera utilisé comme sous-domaine lors du déploiement de votre bot sur le cloud (par exemple, mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Sélectionnez l’abonnement, le [groupe de ressources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), le plan App Service et [l’emplacement](https://azure.microsoft.com/regions/).
    * Sélectionnez le modèle **Language Understanding (C#)** pour le champ **Modèle de bot**.
    * Sélectionnez **l’emplacement de l’application LUIS**. L’application sera créée dans cette [région][LUIS].
    * Cochez la case de confirmation de la mention légale. Les conditions de la mention légale figurent sous la case à cocher.

    ![Service de robot](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Vérifiez que le service bot a été déployé.
    * Sélectionnez Notifications (l’icône représentant une cloche qui se trouve dans la partie supérieure du Portail Azure). La notification passe de **Déploiement lancé** à **Déploiement réussi**.
    * Cliquez alors sur **Accéder à la ressource** sur la notification **Déploiement réussi**.

> [!Note]
> Ce processus de création de bot d’application web a également créé pour vous une application LUIS. Il a effectué un apprentissage et a été publié pour vous. 

## <a name="try-the-default-bot"></a>Essayer le bot par défaut

Confirmez que le bot a été déployé en cochant la case **Notifications**. La notification passe de **Déploiement en cours…** à **Déploiement réussi**. Cliquez sur **Accéder à la ressource** pour ouvrir les ressources du bot.

Une fois le bot enregistré, cliquez sur **Tester dans la discussion web** pour ouvrir le volet de Discussion web. Tapez « Bonjour » dans la Discussion Web.

  ![Tester le bot dans la Discussion Web](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

Le bot répond : « Vous vous trouvez dans l’intention Salutations. Vous avez dit : bonjour ».  Cette réponse confirme que le bot a reçu votre message et l’a transmis à une application LUIS par défaut qu’il a créée. Celle-ci a détecté une intention de type Salutations. Dans l’étape suivante, vous allez connecter le bot à l’application LUIS que vous avez créée plutôt qu’à l’application LUIS par défaut.

## <a name="connect-your-luis-app-to-the-bot"></a>Connecter votre application LUIS au bot

Ouvrez **Paramètres de l’application** et ajoutez l’ID de votre application LUIS dans le champ **LuisAppId**. Si vous avez créé votre application HomeAutomation LUIS dans une région autre que l’Ouest des États-Unis, vous devez également modifier **LuisAPIHostName**. La clé **LuisAPIKey** actuellement configurée est votre clé de création. Elle devient votre clé d’abonnement lorsque votre trafic dépasse le quota du niveau gratuit. 

  ![Mettre à jour l’ID d’application LUIS dans Azure](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> Si vous ne connaissez pas l’ID de l’application LUIS pour l’[application Home Automation](luis-get-started-create-app.md), connectez-vous au site web [LUIS](luis-reference-regions.md) avec le compte que vous utilisez pour vous connecter à Azure. 
> 1. Cliquer sur **Mes applications**. 
> 2. Localisez l’application LUIS que vous avez créée et qui contient les intentions et les entités du domaine HomeAutomation.
> 3. Sur la page **Paramètres** de l’application LUIS, recherchez et copiez l’ID de l’application. Assurez-vous qu’elle [a effectué l’apprentissage](interactive-test.md) et [a été publiée](PublishApp.md). 

    > [!WARNING]
    > If you delete your app ID or LUIS key, the bot will stop working.

## <a name="modify-the-bot-code"></a>Modifier le code du bot

1. Cliquez sur **Générer**, puis sur **Ouvrir l’éditeur de code en ligne**.

   ![Ouvrir l’éditeur de code en ligne](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Cliquez avec le bouton droit sur `build.cmd` et sélectionnez **Exécuter à partir de la console** pour générer l’application. La génération se fait en plusieurs étapes dont le service se charge automatiquement pour vous. La génération se termine par « Terminé avec succès ».

3. Dans l’éditeur de code, ouvrez `/Dialogs/BasicLuisDialog.cs`. Le fichier contient le code suivant :

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>Modifier le code pour les intentions de HomeAutomation


1. Supprimer les trois attributs d’intentions et les méthodes pour **Salutation**, **Annuler** et **Aide**. Ces intentions ne sont pas utilisées dans le domaine prédéfini de HomeAutomation. Veillez à conserver l’attribut d’intention **Aucun** et la méthode. 

2. Ajoutez des dépendances au début du fichier, avec les autres dépendances :

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Ajoutez des constantes pour la gestion des chaînes en haut de la classe `BasicLuisDialog ` :

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Ajoutez le code pour les nouvelles intentions de `HomeAutomation.TurnOn` et `HomeAutomation.TurnOff` à l’intérieur de la classe `BasicLuisDialog ` :

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. Ajoutez le code pour obtenir toutes les entités trouvées par LUIS à l’intérieur de la `BasicLuisDialog ` classe :

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Modifiez la méthode **ShowLuisResult** dans la classe `BasicLuisDialog ` pour arrondir le score, collecter les entités et afficher le message de réponse dans le chatbot :

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>Générer le bot
Dans l’éditeur de code, cliquez avec le bouton droit sur `build.cmd` et sélectionnez **Exécuter à partir de la console**.

![Générer le bot web ](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

Le mode code est remplacé par une fenêtre de terminal indiquant la progression et les résultats de la build.

![Opération de génération du bot web réussie](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Une autre méthode de génération du bot consiste à sélectionner le nom du bot dans la barre supérieure bleue et à sélectionner **Ouvrir la console Kudu**. La console s’ouvre dans **D:\home**. 
> 
> Modifiez le répertoire en **D:\home\site\wwwroot** en tapant : `cd site\wwwroot`
>
> Exécutez le script de génération en tapant : `build.cmd`

## <a name="test-the-bot"></a>Tester le bot

Dans le portail Azure, cliquez sur **Test dans le chat web** pour tester le bot. Tapez des messages comme « Allumer les lumières » et « Éteindre mon radiateur » pour appeler les intentions qui vous y avez ajoutées.

   ![Tester le bot HomeAutomation dans la Discussion Web](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> Ce nouvel apprentissage de votre application LUIS est possible sans aucune modification du code du bot. Voir [Ajouter des exemples d’énoncés](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) et [Entraîner et tester votre application LUIS](https://docs.microsoft.com/azure/cognitive-services/LUIS/interactive-test). 

## <a name="download-the-bot-to-debug"></a>Télécharger le bot à déboguer
Si votre bot ne fonctionne pas, téléchargez le projet sur votre ordinateur local et poursuivez le [débogage](https://docs.microsoft.com/bot-framework/bot-service-debug-bot#debug-an-azure-app-service-web-app-c-bot). 

## <a name="learn-more-about-bot-framework"></a>En savoir plus sur Bot Framework
En savoir plus sur [Bot Framework](https://dev.botframework.com/) et sur les kits de développement logiciel (SDK) [3.x](https://github.com/Microsoft/BotBuilder) et [4.x](https://github.com/Microsoft/botbuilder-dotnet).

## <a name="next-steps"></a>Étapes suivantes

Ajoutez les intentions de LUIS et les boîtes du service Bot pour la gestion des intentions **Aide**, **Annuler** et **Salutations**. N’oubliez pas d’effectuer l’apprentissage de l’application web, de la publier et de la [générer](#build-the-bot). LUIS et le bot doivent avoir les mêmes intentions.

> [!div class="nextstepaction"]
> [Ajouter des intentions](./luis-how-to-add-intents.md)
> [Préparation vocale](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


<!-- Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-cs-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/csharp
[Github-LUIS-Samples-cs-hotel-bot-readme]: https://github.com/Microsoft/LUIS-Samples/blob/master/bot-integration-samples/hotel-finder/csharp/README.md
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[AssignedEndpointDoc]: https://docs.microsoft.com/azure/cognitive-services/LUIS/manage-keys
[VisualStudio]: https://www.visualstudio.com/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
<!-- tested on Win10 -->