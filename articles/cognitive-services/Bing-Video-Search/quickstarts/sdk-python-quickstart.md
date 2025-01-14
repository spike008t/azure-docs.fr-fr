---
title: 'Démarrage rapide : Rechercher des vidéos à l’aide du SDK Recherche de vidéos Bing pour Python'
titleSuffix: Azure Cognitive Services
description: Utilisez ce guide de démarrage rapide pour envoyer des requêtes de recherche de vidéos à l’aide du SDK Recherche de vidéos Bing pour Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: ae91deb5b49b0eb202175fdd242ef0d91975bb25
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798224"
---
#  <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-python"></a>Démarrage rapide : Effectuer une recherche de vidéos avec le SDK Recherche de vidéos Bing pour Python

Utilisez ce guide de démarrage rapide pour démarrer une recherche de vidéos avec le SDK Recherche de vidéos Bing pour Python. Si l’outil Recherche de vidéos Bing dispose d’une API REST compatible avec la plupart des langages de programmation, le SDK offre quant à lui un moyen facile d’intégrer le service à vos applications. Le code source de cet exemple est disponible sur [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py). Il contient d’autres fonctionnalités et annotations.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Prérequis

- [Python](https://www.python.org/) 2.x ou 3.x
- Le SDK Recherche de vidéos Bing pour Python

Nous vous recommandons d’utiliser un [environnement virtuel](https://docs.python.org/3/tutorial/venv.html) python. Vous pouvez installer et lancer un environnement virtuel avec le [module venv](https://pypi.python.org/pypi/virtualenv). Installez virtualenv pour Python 2.7 avec :

```console
python -m venv mytestenv
```
   
Installez le SDK Recherche de vidéos Bing avec :

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Créer et initialiser l’application

1. Créez un fichier Python dans votre éditeur ou IDE favori, puis ajoutez les instructions d’importation suivantes. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchAPI
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```
2. Créez une variable pour votre clé d’abonnement. 
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    ```

## <a name="create-the-search-client"></a>Créer le client de recherche

Créez une instance de `CognitiveServicesCredentials`, et instanciez le client :

    ```python
    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-get-a-response"></a>Envoyer une requête de recherche et obtenir une réponse

1. Utilisez `client.videos.search()` avec votre requête de recherche pour envoyer une demande à l’API Recherche de vidéos Bing et obtenir une réponse.

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. Si la réponse contient des résultats de recherche, obtenez le premier, puis imprimez son ID, son nom et sont URL.

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer une application web monopage](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Voir aussi 

* [Qu’est-ce que l’API Recherche de vidéos Bing ?](../overview.md)
* [Exemples du Kit de développement logiciel (SDK) .NET pour Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)