---
title: Recherche d’images populaires sur le Web| Microsoft Docs
description: Découvrez comment utiliser l’API Recherche d’images Bing pour rechercher des images sur le web.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: b12524cd4c1896501820209b3a45746b8f38b210
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35367904"
---
# <a name="get-trending-images"></a>Obtenir des images populaires  

Pour obtenir des images qui sont populaires à l’heure actuelle, envoyez la requête GET suivante :  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

L’API Images populaires n’est actuellement disponible que sur les marchés suivants :  

- en-US (anglais, États-Unis)  
- en-CA (anglais, Canada)  
- en-AU (anglais, Australie)  
- zh-CN (chinois, Chine)

La réponse contient un objet [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#trendingimages) qui classe les images par catégorie. Utilisez l’élément `title` de la catégorie pour grouper les images dans votre expérience utilisateur. Les catégories sont susceptibles de changer tous les jours.  
  
```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  
  
Chaque vignette contient une image et des options permettant d’obtenir les images associées. Pour obtenir les images associées, vous pouvez utiliser la requête `text` pour appeler l’[API Recherche d’images](./search-the-web.md) et afficher vous-même les images associées. Vous pouvez également utiliser l’URL dans `webSearchUrl` pour rediriger l’utilisateur sur la page des résultats de la recherche d’images Bing qui contient les images associées. 

Si vous appelez l’API Recherche d’images pour obtenir les images associées, définissez le paramètre de requête [id](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#id) sur l’ID du champ `id`. En spécifiant l’ID, vous vous assurez que la réponse contient l’image (il s’agit de la première image de la réponse) et les images qui y sont associées. En outre, définissez le paramètre de requête [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#q) sur le texte dans le champ `text` de l’objet `query`.

L’exemple suivant illustre l’utilisation de l’ID de l’image pour obtenir les images associées de M. Smith dans la précédente réponse de l’API Images populaires.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  