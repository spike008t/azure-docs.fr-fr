---
title: Fichier Include
description: Fichier Include
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 15047b9782ea86b91ba78e0d745c67892fdc8b76
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124101"
---
Le tableau `response` pour l’ajout des exemples d’énoncés indique la réussite ou l’échec de chaque exemple d’énoncé avec la propriété `hasError`. La réponse JSON suivante montre que les deux énoncés ont bien été ajoutés. 

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle today",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

Le texte JSON suivant illustre le résultat d’une requête d’apprentissage ayant abouti :

```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Le texte JSON suivant illustre le résultat d’une requête d’état d’entraînement ayant abouti. Chaque modelID est une intention. Chaque intention doit être entraînée sur tous les énoncés afin d’identifier correctement les énoncés qui appartiennent à l’intention ainsi que ceux qui n’y appartiennent pas. 

```JSON
[
    {
        "modelId": "0c694cf9-8c32-44b8-9ea0-3d30a7d901ca",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "10e53836-ade4-494e-9531-3bd6a944c510",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "21e48732-a512-4c33-b5ed-8ea629465269",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "edee15b1-9999-45c2-bbab-591d3a643033",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "aa78e06e-df81-4bb2-b2d9-a2fbb2f81c54",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "e39bb7bd-b417-41a9-a24f-caf4c47fc62c",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "3782eac7-db84-4d66-ba00-0598dffb48ee",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "a941d926-cb0f-47a8-ab7e-deba4378b96f",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "8137f40e-ce6d-40a5-881f-dfd46a05f7e0",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "dc08f95a-58b4-4064-a210-03fe34f75a3c",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    },
    {
        "modelId": "4fabdbed-5697-4562-8c7d-36e174efff2e",
        "details": {
            "statusId": 3,
            "status": "InProgress",
            "exampleCount": 48
        }
    }
]
```
