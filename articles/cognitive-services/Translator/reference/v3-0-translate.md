---
title: Méthode Translate de l’API de traduction de texte Translator Text | Microsoft Docs
titleSuffix: Cognitive Services
description: Utilisez la méthode Translate de l’API de traduction de texte Translator Text.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: d8d5e1e2fac747fa733f1d92c08008b7eac2a1bc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35370673"
---
# <a name="text-api-30-translate"></a>API de traduction de texte Translator Text 3.0 : Translate

Traduit du texte.

## <a name="request-url"></a>URL de la demande

Envoyez une demande `POST` à :

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
```

## <a name="request-parameters"></a>Paramètres de la demande

Les paramètres de demande transmis à la chaîne de requête sont les suivants :

<table width="100%">
  <th width="20%">Paramètre de requête</th>
  <th>Description</th>
  <tr>
    <td>api-version</td>
    <td>*Paramètre obligatoire*.<br/>Version de l’API demandée par le client. La valeur doit être `3.0`.</td>
  </tr>
  <tr>
    <td>from</td>
    <td>*Paramètre facultatif*.<br/>Spécifie la langue du texte d’entrée. Trouvez les langues disponibles pour la traduction en recherchant [langues prises en charge](.\v3-0-languages.md) à l’aide de l’étendue `translation`. Si le paramètre `from` n’est pas spécifié, une détection automatique de la langue est appliquée pour déterminer la langue source.</td>
  </tr>
  <tr>
    <td>to</td>
    <td>*Paramètre obligatoire*.<br/>Spécifie la langue du texte de sortie. La langue cible doit être l’une des [langues prises en charge](.\v3-0-languages.md) incluses dans l’étendue `translation`. Par exemple, utilisez `to=de` pour traduire en allemand.<br/>Il est possible de traduire en plusieurs langues simultanément en répétant le paramètre dans la chaîne de requête. Par exemple, utilisez `to=de&to=it` pour traduire en allemand et italien.</td>
  </tr>
  <tr>
    <td>textType</td>
    <td>*Paramètre facultatif*.<br/>Définit si le texte en cours de traduction est au format texte brut ou HTML. Tout code HTML doit être un élément bien formé et complet. Les valeurs possibles sont : `plain` (par défaut) ou `html`.</td>
  </tr>
  <tr>
    <td>category</td>
    <td>*Paramètre facultatif*.<br/>Chaîne spécifiant la catégorie (domaine) de la traduction. Ce paramètre est utilisé pour obtenir des traductions d’un système personnalisé créé avec [Custom Translator](../customization.md). La valeur par défaut est `general`.</td>
  </tr>
  <tr>
    <td>ProfanityAction</td>
    <td>*Paramètre facultatif*.<br/>Spécifie comment les vulgarités doivent être traitées dans les traductions. Les valeurs possibles sont : `NoAction` (valeur par défaut), `Marked` ou `Deleted`. Pour comprendre comment traiter les vulgarités, voir [Gestion de la vulgarité](#handle-profanity).</td>
  </tr>
  <tr>
    <td>ProfanityMarker</td>
    <td>*Paramètre facultatif*.<br/>Spécifie comment vulgarités doit être marquées dans les traductions. Les valeurs possibles sont : `Asterisk` (par défaut) ou `Tag`. Pour comprendre comment traiter les vulgarités, voir [Gestion de la vulgarité](#handle-profanity).</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td>*Paramètre facultatif*.<br/>Spécifie s’il faut inclure une projection d’alignement du texte source vers le texte traduit. Les valeurs possibles sont `true` ou `false` (par défaut). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td>*Paramètre facultatif*.<br/>Spécifie s’il faut inclure des limites de longueur de phrase aux texte d’entrée et au texte traduit. Les valeurs possibles sont `true` ou `false` (par défaut).</td>
  </tr>
  <tr>
    <td>suggestedFrom</td>
    <td>*Paramètre facultatif*.<br/>Spécifie une langue de base si la langue du texte d’entrée ne peut pas être identifiée. La détection automatique de la langue est appliquée en cas d’omission du paramètre `from`. Si la détection échoue, la langue `suggestedFrom` est présupposée.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Paramètre facultatif*.<br/>Spécifie le script du texte d’entrée.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Paramètre facultatif*.<br/>Spécifie le script du texte traduit.</td>
  </tr>
</table> 

Les en-têtes de demande sont les suivants :

<table width="100%">
  <th width="20%">En-têtes</th>
  <th>Description</th>
  <tr>
    <td>_One authorization_<br/>_header_</td>
    <td>*En-tête de demande obligatoire*.<br/>Voir les [options disponibles pour l’authentification](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td>*En-tête de demande obligatoire*.<br/>Spécifie le type de contenu de la charge utile. Les valeurs possibles sont les suivantes : `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*En-tête de demande obligatoire*.<br/>Longueur du corps de la demande.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Facultatif*.<br/>GUID généré par le client pour identifier de façon unique la demande. Vous pouvez omettre cet en-tête si vous incluez l’ID de trace dans la chaîne de requête à l’aide d’un paramètre de requête nommé `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Corps de la demande

Le corps de la demande est un tableau JSON. Chaque élément du tableau est un objet JSON avec une propriété de chaîne nommée `Text`, qui représente la chaîne à traduire.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

Les limites suivantes s'appliquent :

* Le tableau ne peut pas compter plus de 25 éléments.
* L’intégralité du texte inclus dans la demande ne peut pas dépasser 5 000 caractères, espaces compris.

## <a name="response-body"></a>Response body

Une réponse correcte est un tableau JSON avec un résultat pour chaque chaîne dans le tableau d’entrée. Un objet de résultat inclut les propriétés suivantes :

  * `detectedLanguage` : objet décrivant la langue détectée via les propriétés suivantes :

      * `language` : chaîne représentant le code de la langue détectée.

      * `score` : valeur flottante indiquant le niveau de confiance dans le résultat. Le score est compris entre zéro et un, un score faible indiquant un niveau de confiance bas.

    La propriété `detectedLanguage` n’est présente dans l’objet de résultat que quand la détection automatique de la langue est demandée.

  * `translations` : tableau des résultats de la traduction. La taille du tableau correspond au nombre de langues cibles spécifié par le paramètre de requête `to`. Chaque élément dans le tableau inclut ce qui suit :

    * `to` : chaîne représentant le code de langue de la langue cible.

    * `text` : chaîne fournissant le texte traduit.

    * `transliteration` : objet fournissant le texte traduit dans le script spécifié par le paramètre `toScript`.

      * `script` : chaîne spécifiant le script cible.   

      * `text` : chaîne fournissant le texte traduit dans le script cible.

    L’objet `transliteration` n’est pas inclus si une translittération n’a pas lieu.

    * `alignment` : objet avec une propriété de chaîne unique nommée `proj`, qui mappe le texte d’entrée au texte traduit. Les informations d’alignement ne sont fournies que quand le paramètre de requête `includeAlignment` est `true`. L’alignement est renvoyé en tant que valeur de chaîne au format suivant : `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  Le signe deux-points sépare les index de début et de fin, le signe tiret sépare les langues, et une espace sépare les mots. Un mot peut s’aligner avec zéro, un ou plusieurs mots dans l’autre langue, et les mots alignés peuvent ne pas être contigus. Quand aucune information d’alignement n’est disponible, l’élément d’alignement est vide. Pour obtenir un exemple et les restrictions, voir [Obtenir les informations d’alignement](#obtain-alignment-information).

    * `sentLen` : objet renvoyant les limites de longueur de phrase dans les textes d’entrée et de sortie.

      * `srcSentLen` : tableau d’entiers représentant les longueurs des phrases dans le texte d’entrée. La longueur du tableau correspond au nombre de phrases, et les valeurs sont les longueurs des phrases.

      * `transSentLen` : tableau d’entiers représentant les longueurs des phrases dans le texte traduit. La longueur du tableau correspond au nombre de phrases, et les valeurs sont les longueurs des phrases.

    Les limites de longueur de phrase ne sont incluses que si le paramètre de requête `includeSentenceLength` est `true`.

  * `sourceText` : objet avec une propriété de chaîne unique nommée `text`, qui fournit le texte d’entrée dans le script par défaut de la langue source. La propriété `sourceText` est présente uniquement quand l’entrée est exprimée dans un script qui n’est pas le script habituel pour la langue. Par exemple, si l’entrée est de l’arabe écrit dans un script latin, `sourceText.text` est le même texte en arabe converti en script arabe.

Des exemples de réponses JSON sont fournis dans la section [exemples](#examples).

## <a name="response-status-codes"></a>Codes d’état de réponse

Les codes d’état HTTP qu’une demande peut renvoyer sont les suivants. 

<table width="100%">
  <th width="20%">Code d’état</th>
  <th>Description</th>
  <tr>
    <td>200</td>
    <td>Vous avez réussi !</td>
  </tr>
  <tr>
    <td>400</td>
    <td>L’un des paramètres de demande est manquant ou non valide. Corrigez les paramètres de demande avant de réessayer.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>Il n’a pas été possible d’authentifier la demande. Vérifiez que les informations d’identification sont spécifiées et valides.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>La demande n’est pas autorisée. Vérifiez le message d’erreur détaillé. Cela indique souvent que toutes les traductions gratuites fournies avec un abonnement d’essai ont été utilisées.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>L’appelant envoie trop de demandes.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Une erreur inattendue s’est produite. Si l’erreur persiste, signalez-la en fournissant les informations suivantes : date et heure de la défaillance, identificateur de la demande dans l’en-tête de réponse,`X-RequestId` et identificateur du client dans l’en-tête de demande `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Serveur temporairement indisponible. relancez la requête. Si l’erreur persiste, signalez-la en fournissant les informations suivantes : date et heure de la défaillance, identificateur de la demande dans l’en-tête de réponse,`X-RequestId` et identificateur du client dans l’en-tête de demande `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Exemples

### <a name="translate-a-single-input"></a>Traduire une entrée unique

Cet exemple montre comment traduire une phrase unique de l’anglais en chinois simplifié.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Le corps de la réponse est le suivant :

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

Le `translations` tableau inclut un élément qui fournit la traduction de l’élément de texte dans l’entrée.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Traduire une entrée unique avec détection automatique de la langue

Cet exemple montre comment traduire une phrase unique de l’anglais en chinois simplifié. La demande ne spécifie pas la langue d’entrée. La détection automatique de la langue source est utilisée à la place.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Le corps de la réponse est le suivant :

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
La réponse est similaire à la réponse de l’exemple précédent. Étant donné que la détection automatique de la langue a été demandée, la réponse inclut également des informations sur la langue détectée pour le texte d’entrée. 

### <a name="translate-with-transliteration"></a>Traduire avec translittération

Étendons l’exemple précédent en ajoutant la translittération. La requête suivante demande une traduction chinoise écrite en script Latin.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Le corps de la réponse est le suivant :

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"text":"nǐ hǎo , nǐ jiào shén me míng zì ？","script":"Latn"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

Le résultat de la traduction inclut à présent une `transliteration` propriété qui fournit le texte traduit en caractères latins.

### <a name="translate-multiple-pieces-of-text"></a>Traduire plusieurs éléments de texte

La traduction de plusieurs chaînes en une fois nécessite simplement de spécifier un tableau de chaînes dans le corps de la demande.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

---

Le corps de la réponse est le suivant :

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },            
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### <a name="translate-to-multiple-languages"></a>Traduire en plusieurs langues

Cet exemple montre comment traduire une même entrée en plusieurs langues en utilisant une seule requête.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Le corps de la réponse est le suivant :

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### <a name="handle-profanity"></a>Traiter la vulgarité

Le service Translator conserve normalement dans la traduction toute vulgarité présente dans la source. Le degré de vulgarité et le contexte qui rend des mots vulgaires diffèrent selon les cultures. Par conséquent, le degré de vulgarité dans la langue cible peut être amplifié ou réduit.

Si vous souhaitez éviter toute vulgarité dans la traduction, indépendamment de la présence de vulgarité dans le texte source, vous disposez d’une option de filtrage de la vulgarité. L’option vous permet de spécifier si vous souhaitez que la vulgarité soit supprimée, que les vulgarités soient marquées à l’aide de balises appropriées (ce qui vous offre la possibilité d’ajouter votre propre post-traitement), ou qu’aucune action ne soit appliquée. Les valeurs acceptées de `ProfanityAction` sont `Deleted`, `Marked` et `NoAction` (par défaut).

<table width="100%">
  <th width="20%">ProfanityAction</th>
  <th>Action</th>
  <tr>
    <td>`NoAction`</td>
    <td>Il s’agit du comportement par défaut. La vulgarité de la source est reflétée dans la cible.<br/><br/>
    **Exemple de source (japonais)**  : 彼はジャッカスです。<br/>
    **Exemple de traduction (anglais)**  : Il est un imbécile.
    </td>
  </tr>
  <tr>
    <td>`Deleted`</td>
    <td>Les mots vulgaires sont retirés de la cible sans remplacement.<br/><br/>
    **Exemple de source (japonais)**  : 彼はジャッカスです。<br/>
    **Exemple de traduction (anglais)**  : Il est un.
    </td>
  </tr>
  <tr>
    <td>`Marked`</td>
    <td>Les mots vulgaires sont remplacés par un marqueur dans la sortie. Le marqueur varie selon le paramètre `ProfanityMarker`.<br/><br/>
Pour `ProfanityMarker=Asterisk`, les mots vulgaires sont remplacés par `***` :<br/>
    **Exemple de source (japonais)**  : 彼はジャッカスです。<br/>
    **Exemple de traduction (anglais)**  : il est un \*\*\*.<br/><br/>
Pour `ProfanityMarker=Tag`, les mots vulgaires sont entourés de balises XML &lt;profanity&gt; et &lt;/profanity&gt; :<br/>
    **Exemple de source (japonais)**  : 彼はジャッカスです。<br/>
    **Exemple de traduction (anglais)**  : il est un &lt;profanity&gt;imbécile&lt;/profanity&gt;.
  </tr>
</table> 

Par exemple : 

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Cette demande renvoie :

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Comparez à :

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Cette dernière demande renvoie :

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>Traduire du contenu incluant un balisage et décider ce qui est traduit

Il est courant de traduire du contenu incluant un balisage, tel que le contenu d’une page HTML ou d’un document XML. Lors de la traduction de contenu incluant des balises, incluez le paramètre de requête `textType=html`. De plus, il est parfois utile d’exclure du contenu spécifique de la traduction. Vous pouvez utiliser l’attribut `class=notranslate` pour spécifier le contenu qui doit rester dans la langue d’origine. Dans l’exemple suivant, le contenu du premier élément `div` ne sera pas traduit, tandis que le contenu du deuxième élément `div` le sera.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Voici un exemple de demande.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

---

La réponse est la suivante :

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Obtenir les informations d’alignement

Pour recevoir les informations d’alignement, spécifiez `includeAlignment=true` sur la chaîne de requête.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation.'}]"
```

---

La réponse est la suivante :

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

Les informations d’alignement commencent par `0:2-0:1`, ce qui signifie que les trois premiers caractères dans le texte source (`The`) mappent aux deux premiers caractères dans le texte traduit (`La`).

Notez que les restrictions suivantes s’appliquent :

* L’alignement est renvoyé uniquement pour un sous-ensemble de paires de langues :
  - de l’anglais vers toute autre langue ;
  - de toute langue vers l’anglais, à l’exception du chinois simplifié, du chinois traditionnel et du letton vers anglais ;
  - du japonais au coréen ou inversement.
* Vous ne recevrez pas d’alignement si la phrase est une traduction définie. Des traductions définies sont, par exemple, « Ceci est un test », « Je t’aime » et d’autres phrases extrêmement fréquentes.

### <a name="obtain-sentence-boundaries"></a>Obtenir les limites de longueur de phrase

Pour recevoir des informations sur la longueur des phrases dans le texte source et le texte traduit, spécifiez `includeSentenceLength=true` dans la chaîne de requête.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

---

La réponse est la suivante :

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### <a name="translate-with-dynamic-dictionary"></a>Traduire avec un dictionnaire dynamique

Si vous connaissez déjà la traduction que vous souhaitez appliquer à un mot ou à une phrase, vous pouvez la fournir en tant que balisage dans la demande. Le dictionnaire dynamique n’est sûr que pour des noms composés, tels que des noms propres et des noms de produits.

Le balisage à fournir utilise la syntaxe suivante.

``` 
<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>
```

Par exemple, considérez la phrase : « Le mot wordomatic est une entrée de dictionnaire ». Pour conserver le mot _wordomatic_ dans la traduction, envoyez la demande suivante :

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

Le résultat est le suivant :

```
[
    {
        "translations":[
            {"text":"Das Wort "wordomatic" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Cette fonctionnalité opère de la même façon avec `textType=text` ou `textType=html`. Elle doit être utilisée avec parcimonie. La façon appropriée et de loin préférable de personnaliser une traduction consiste à utiliser Custom Translator. Custom Translator utilise totalement le contexte et les probabilités statistiques. Si vous avez ou pouvez vous permettre de créer des données d’apprentissage qui montrent votre mot ou phrase en contexte, vous obtenez de bien meilleurs résultats. [En savoir plus sur Custom Translator](../customization.md).
 




