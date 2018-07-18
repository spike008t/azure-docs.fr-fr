---
title: Questions fréquentes (FAQ) sur l’API Recherche Web Bing sur Azure | Microsoft Docs
description: Obtenez des réponses aux questions courantes relatives à l’API Recherche Web Bing de Microsoft Cognitive Service sur Azure.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 321f571c48f2231d1ced43848cdefd17adaa1a08
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35368036"
---
# <a name="frequently-asked-questions-faq-about-bing-web-search-api-cognitive-services"></a>Questions fréquentes (FAQ) sur l’API Recherche Web Bing (Cognitive Services)
 
 Trouvez les réponses aux questions fréquemment posées sur les concepts, codes et scénarios relatifs à l’API Recherche Web Bing pour Microsoft Cognitive Services sur Azure.

## <a name="response-headers-in-javascript"></a>En-têtes de réponse dans JavaScript

Les en-têtes suivants peuvent se produire dans les réponses de l’API Recherche Web Bing.

|||
|-|-|
|`X-MSEdge-ClientID`|L’ID unique que Bing a affecté à l’utilisateur|
|`BingAPIs-Market`|Le marché qui a été utilisé pour répondre à la demande|
|`BingAPIs-TraceId`|L’entrée du journal sur le serveur de l’API Bing pour cette requête (pour la prise en charge)|

Il est particulièrement important de conserver l’identifiant du client et de le renvoyer avec les demandes suivantes. Lorsque vous effectuez cette opération, la recherche utilise un contexte passé pour classer les résultats de la recherche et propose également une expérience utilisateur cohérente.

Toutefois, lorsque vous appelez l’API Recherche Web Bing depuis JavaScript, les fonctionnalités de sécurité intégrées de votre navigateur (CORS) peuvent vous empêcher d’accéder aux valeurs de ces en-têtes.

Pour accéder aux en-têtes, vous pouvez effectuer la requête d’API Recherche Web Bing via un proxy CORS. La réponse émanant d’un proxy de ce type a un en-tête `Access-Control-Expose-Headers` qui met les en-têtes de réponse sur liste verte et les rend disponibles pour JavaScript.

Il est facile d’installer un proxy CORS pour autoriser l’[application du tutoriel](tutorial-bing-web-search-single-page-app.md) à accéder aux en-têtes des clients facultatifs. Tout d’abord, si ce n’est pas déjà fait, [installez Node.js](https://nodejs.org/en/download/). Entrez ensuite la commande suivante à l’invite de commandes.

    npm install -g cors-proxy-server

Ensuite, remplacez le point de terminaison de l’API Recherche Web Bing dans le fichier HTML par :

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Enfin, démarrez le proxy CORS avec la commande suivante :

    cors-proxy-server

Laissez la fenêtre de commande ouverte pendant que vous utilisez l’application du tutoriel ; la fermeture de la fenêtre entraîne l’arrêt du proxy. Dans la section des en-têtes HTTP (qui peut être développée) sous les résultats de recherche, vous pouvez maintenant voir l’en-tête `X-MSEdge-ClientID` (entre autres) et vérifier qu’il est identique pour toutes les requêtes.

## <a name="response-headers-in-production"></a>En-têtes de réponse en production

L’approche du proxy CORS décrite dans la réponse précédente est appropriée pour le développement, le test et l’apprentissage. 

Toutefois, dans un environnement de production, vous devez héberger un script côté serveur dans le même domaine que la page web qui utilise l’API Recherche Web Bing. Ce script doit effectuer les appels d’API à la demande à partir de la page web JavaScript et transmettre tous les résultats, y compris les en-têtes, au client. Étant donné que les deux ressources (la page et le script) partagent une origine, le CORS n’intervient pas et les en-têtes spéciaux sont accessibles pour JavaScript dans la page web. 

Cette approche protège également votre clé API de l’exposition au public, étant donné que seul le script côté serveur en a besoin. Le script peut utiliser une autre méthode pour s’assurer que la demande est autorisée.

## <a name="next-steps"></a>Étapes suivantes

Votre question concerne-t-elle une fonctionnalité manquante ? Demandez-la ou votez pour elle sur notre [site web UserVoice](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Voir aussi

 [Dépassement de la capacité de la pile : Cognitive Services](http://stackoverflow.com/questions/tagged/bing-api)