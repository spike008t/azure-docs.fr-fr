---
title: Vue d’ensemble d’Azure Maps | Microsoft Docs
description: Présentation d’Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 02/04/2019
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: fbb855db1ff5a2cf79826294365733614259e4b0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64575749"
---
# <a name="what-is-azure-maps"></a>Qu’est-ce qu’Azure Maps ?

Azure Maps est une collection de services géospatiaux qui s’appuient sur les données cartographiques les plus récentes pour fournir un contexte géographique précis à vos applications web et mobiles. Azure Maps se compose de plusieurs API REST : une API pour le rendu des **cartes** dans plusieurs styles, y compris l’imagerie satellite ; une API pour la **recherche** d’adresses, de sites et de points d’intérêt dans le monde entier ; une API pour le **routage** point à point, multipoint, à optimisation multipoint, par isochrones, de véhicules commerciaux, influencé par le trafic et matriciel ; une API pour l’affichage du trafic et des incidents ; une API pour obtenir la localisation de l’utilisateur via la **géolocalisation** ; une API pour la conversion d’adresses en **fuseaux horaires** et une API pour récupérer l’heure d’un lieu. De plus, Azure Maps offre des services pour le **geofencing**, le stockage de **données** cartographiques (hébergement d’informations de localisation dans Azure) et des **opérations spatiales** fournissant une géolocalisation intelligente via une analytique géospatiale. Les services Azure Maps sont disponibles directement en tant qu’API REST ou via nos solides **kit SDK web** et **kit Android SDK**. Ces outils permettent aux développeurs de développer et mettre à l’échelle rapidement des solutions intégrant des informations de localisation dans des solutions Azure à partir du cloud Azure. Ouvrez gratuitement votre [compte Azure Maps](https://azure.microsoft.com/services/azure-maps/) dès aujourd'hui et commencez à développer !

La vidéo suivante explique Azure Maps plus en détail :

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Contrôles de carte

### <a name="web-sdk"></a>Kit de développement logiciel (SDK) web

Le Kit de développement logiciel (SDK) web Azure Maps vous permet de personnaliser des cartes interactives avec du contenu et des images qui vous sont propres pour les afficher dans vos applications web ou mobiles. Ce contrôle utilise WebGL, ce qui vous permet d’afficher d’importants jeux de données avec des performances élevées. Développez avec le Kit de développement logiciel (SDK) en utilisant JavaScript ou TypeScript.

![Kit de développement logiciel (SDK) web Azure Maps](media/about-azure-maps/Introduction_WebMapControl.png)

### <a name="android-sdk"></a>Kit de développement logiciel Android

Le Kit de développement logiciel Android SDK Azure Maps vous permet de créer de puissantes applications de mappage mobiles. 

![Android SDK Azure Maps](media/about-azure-maps/AndroidSDK.png)

## <a name="services-in-azure-maps"></a>Services d’Azure Maps

Azure Maps se compose des six services suivants, qui peuvent fournir un contexte géographique à vos applications Azure.

### <a name="render-service"></a>Render Service

Render Service permet aux développeurs de créer des applications web et mobiles autour de la cartographie. Le service utilise des images graphiques raster de haute qualité disponibles en 19 niveaux de zoom ou des images de cartes au format vectoriel entièrement personnalisables.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

Render Service inclut désormais des API pour permettre aux développeurs de travailler avec des images satellite. Pour plus d’informations, consultez la page [Azure Maps Render APIs](https://docs.microsoft.com/rest/api/maps/render) (API Azure Maps Render).

### <a name="route-service"></a>Service d’itinéraire

Route Service comporte des fonctionnalités robustes de calcul géométrique pour les infrastructures réelles et des directions pour différents modes de transport. Le service permet aux développeurs de calculer des directions pour plusieurs modes de déplacement, notamment en voiture, en camion, à vélo ou à pied. Le service peut également prendre en compte d’autres entrées, par exemple, les conditions de circulation, les restrictions de poids et le transport de matières dangereuses.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

Route Service offre désormais un aperçu des fonctionnalités avancées telles que le traitement par lots de plusieurs requêtes d’itinéraire, les matrices de durée de trajet et de distance entre un ensemble de points d’origines et de destinations, et la recherche des itinéraires ou distances que vous pouvez parcourir en fonction de votre temps ou de vos besoins en carburant. Pour plus d’informations sur les fonctionnalités d’itinéraire, consultez la page [Azure Maps Route APIs](https://docs.microsoft.com/rest/api/maps/route) (API Azure Maps Route).

### <a name="search-service"></a>Service de recherche

Search Service permet aux développeurs de rechercher des adresses, des lieux, des listes d’entreprises par nom ou catégorie, et d’autres informations d’ordre géographique. Search Service peut aussi [géocoder en inversé](https://en.wikipedia.org/wiki/Reverse_geocoding) les adresses et rues de croisement en fonction des latitudes et longitudes.

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

Search Service fournit également des fonctionnalités avancées telles que la recherche sur un itinéraire, la recherche à l’intérieur d’une zone plus vaste, le traitement des groupes de requêtes de recherche, ainsi que de la recherche pour une zone plus grande et pas uniquement pour un emplacement. Les API pour le traitement par lot et la recherche par zone sont actuellement en préversion. Pour plus d’informations sur les fonctionnalités de recherche, consultez la page [Azure Maps Search APIs](https://docs.microsoft.com/rest/api/maps/search) (API Azure Maps Search).

### <a name="time-zone-service"></a>Time Zone Service

Time Zone Service permet d’interroger des informations actuelles, historiques et futures sur un fuseau horaire à l’aide de paires latitude-longitude ou d’un [ID IANA](https://www.iana.org/). Time Zone Service permet aussi de convertir les ID de fuseau horaire Microsoft Windows en fuseaux horaires IANA, de récupérer un décalage de fuseau horaire par rapport à UTC et d’obtenir l’heure actuelle dans un fuseau horaire respectif. Voici un exemple de réponse JSON type à une requête Time Zone Service :

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Pour plus d’informations sur ce service, consultez la page [Azure Maps Timezone APIs](https://docs.microsoft.com/rest/api/maps/timezone) (API Azure Maps Timezone).

### <a name="traffic-service"></a>Traffic Service

Traffic Service est une suite de services web permettant aux développeurs de créer des applications web et mobiles pour lesquelles des informations sur le trafic sont nécessaires. Ce service fournit deux types de données :

* flux de trafic : vitesses et durées de déplacement observées en temps réel pour toutes les routes clés du réseau ;
* incidents de trafic : vue actualisée des embouteillages et des incidents sur le réseau routier.

![Trafic Azure Maps](media/about-azure-maps/Introduction_Traffic.png)

Consultez la page [Azure Maps Traffic APIs](https://docs.microsoft.com/rest/api/maps/traffic) (API Azure Maps Traffic) pour plus d’informations.

### <a name="ip-to-location"></a>Localisation d’IP

Le service IP to Location vous permet de prévisualiser le code pays à deux lettres récupéré d’une adresse IP donnée. Ce service peut vous aider à adapter et améliorer l’expérience utilisateur en personnalisant le contenu des applications en fonction de l’emplacement géographique.

Pour plus d’informations sur les API REST pour l’adresse IP du service d’emplacement, visitez la page [Azure Maps Geolocation APIs](https://docs.microsoft.com/rest/api/maps/geolocation) (API de géolocalisation Azure Maps).

## <a name="programming-model"></a>Modèle de programmation

Azure Maps est conçu pour la mobilité et peut alimenter des applications multiplateformes. Il utilise un modèle de programmation indépendant du langage et il prend en charge la sortie au format JSON via les [API REST](https://docs.microsoft.com/rest/api/maps/).

En outre, Azure Maps fournit un [contrôle de carte JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control) pratique avec un modèle de programmation simple, ce qui facilite et accélère le développement des applications web et mobiles.

## <a name="usage"></a>Usage

Vous pouvez accéder aux services Maps via le [Portail Azure](https://portal.azure.com) et en créant un compte Azure Maps.

Azure Maps utilise un schéma d’authentification basé sur une clé. Votre compte est doté de deux clés prégénérées pour vous. Commencez par intégrer ces fonctionnalités de localisation dans votre application en utilisant l’une des clés et en formulant une requête au service Azure Maps.

## <a name="supported-regions"></a>Régions prises en charge

L’interface de programmation d’applications Azure Maps est actuellement disponible dans tous les pays et toutes les régions, à l’exception des régions suivantes :

* Argentine
* Chine
* Inde
* Maroc
* Pakistan
* Corée du Sud

Vérifiez que l’emplacement de votre adresse IP actuelle ne correspond pas à l’un des pays ou à l’une des régions non pris(e) en charge cité(e)s plus haut.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les nouvelles fonctionnalités d’Azure Maps :

> [!div class="nextstepaction"]
> [Route Matrix, Isochrones, IP lookup, et bien d’autres](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/)

Essayez un exemple d’application qui présente Azure Maps :

> [!div class="nextstepaction"]
> [Démarrage rapide : Créer une application web](quick-demo-map-app.md)
