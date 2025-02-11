---
title: Mise à niveau de l’API Recherche d’actualités Bing version 5 vers version 7
titlesuffix: Azure Cognitive Services
description: Permet d’identifier les parties de votre application que vous devez mettre à jour pour utiliser la version 7.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: scottwhi
ms.openlocfilehash: 235cc1c74c099a71d289d38369ebc10132564825
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66383295"
---
# <a name="news-search-api-upgrade-guide"></a>Guide de mise à niveau de l’API Recherche d'actualités Bing

Ce guide de mise à niveau identifie les changements entre la version 5 et la version 7 de l’API Recherche d'actualités Bing. Utilisez-le pour identifier les parties de votre application que vous avez besoin de mettre à jour pour utiliser la version 7.

## <a name="breaking-changes"></a>Dernières modifications

### <a name="endpoints"></a>Points de terminaison

- Le numéro de version du point de terminaison a changé entre la version 5 et la version 7. Par exemple, https://api.cognitive.microsoft.com/bing/**v7.0**/news/search.

### <a name="error-response-objects-and-error-codes"></a>Objets de réponse d’erreur et codes d’erreur

- Toutes les requêtes ayant échoué doivent maintenant inclure un objet `ErrorResponse` dans le corps de la réponse.

- Ajout des champs suivants à l’objet `Error`.  
  - `subCode`&mdash;Partitionne le code d’erreur en compartiments discrets, si possible
  - `moreDetails`&mdash;Informations supplémentaires sur l’erreur décrite dans le champ `message`

- Remplacement des codes d’erreur de la version 5 par les valeurs `code` et `subCode` suivantes.

|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing renvoie ServerError à chaque fois que l’une des conditions de sous-code se produit. La réponse inclut ces erreurs si le code d’état HTTP est 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqué|Bing retourne InvalidRequest à chaque fois que la demande comporte une partie non valide. Par exemple, un paramètre obligatoire est manquant ou une valeur de paramètre n’est pas valide.<br/><br/>Si l’erreur est ParameterMissing ou ParameterInvalidValue, le code d’état HTTP est 400.<br/><br/>Si l’erreur est HttpNotAllowed, le code d’état HTTP est 410.
|RateLimitExceeded||Bing retourne RateLimitExceeded chaque fois que vous dépassez votre quota de requêtes par seconde (QPS) ou par mois (QPM).<br/><br/>Bing renvoie le code d’état HTTP 429 si vous dépassez votre quota de requêtes par seconde (QPS), ou le code d’état 403 si vous dépassez votre quota mensuel de requêtes (QPM).
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourne InvalidAuthorization s’il ne parvient pas à identifier l’appelant. Par exemple, l’en-tête `Ocp-Apim-Subscription-Key` est manquant ou la clé d’abonnement n’est pas valide.<br/><br/>Il y a redondance si plusieurs méthodes d’authentification sont spécifiées.<br/><br/>Si l’erreur est InvalidAuthorization, le code d’état HTTP est 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourne InsufficientAuthorization lorsque l’appelant n’est pas autorisé à accéder à la ressource. Cela peut se produire si la clé d’abonnement a été désactivée ou a expiré. <br/><br/>Si l’erreur est InsufficientAuthorization, le code d’état HTTP est 403.

- Le code suivant mappe les codes d’erreur précédents vers les nouveaux codes. En cas de dépendance vis-à-vis des codes d’erreur de la version 5, modifiez votre code en conséquence.

|Code de la version 5|code.subCode de la version 7
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Désactivé|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Bloqué|InvalidRequest.Blocked

### <a name="object-changes"></a>Modifications d’objet

- Ajout du champ `contractualRules` à l’objet [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle). Le champ `contractualRules` contient une liste de règles que vous devez respecter (l’attribution d’article, par exemple). Vous devez appliquer l’attribution fournie dans `contractualRules` au lieu d’utiliser `provider`. L’article inclut `contractualRules` seulement lorsque la réponse de l’[API Recherche Web Bing](../bing-web-search/search-the-web.md) contient Actualités.

## <a name="non-breaking-changes"></a>Changements non cassants

### <a name="query-parameters"></a>Paramètres de requête

- Ajout de Produits comme valeur possible définissable pour le paramètre de requête [category](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#category). Consultez [Catégories par marchés](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference).

- Ajout du paramètre de requête [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortby), qui retourne les rubriques tendance triées par date en commençant par la plus récente.

- Ajout du paramètre de requête [Since](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#since), qui renvoie les rubriques tendance découvertes par Bing pendant ou après le timestamp d’époque Unix.

### <a name="object-changes"></a>Modifications d’objet

- Ajout du champ `mentions` à l’objet [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle). Le champ `mentions` contient une liste d’entités (personnes ou lieux) trouvées dans l’article.

- Ajout du champ `video` à l’objet [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#newsarticle). Le champ `video` contient une vidéo en lien avec l’article. La vidéo est soit \<iframe\>, et vous pouvez l’intégrer, soit un vidéo miniature.

- Ajout du champ `sort` à l’objet [News](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#news). Le champ `sort` indique l’ordre de tri des articles. Par exemple, les articles sont triés par pertinence (par défaut) ou par date.

- Ajout de l’objet [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#sortvalue), qui définit l’ordre de tri. Le champ `isSelected` indique si la réponse a utilisé l’ordre de tri. Si la valeur est **true**, la réponse a utilisé l’ordre de tri. Si `isSelected` est **false**, vous pouvez utiliser l’URL dans le champ `url` pour demander un ordre de tri différent.
