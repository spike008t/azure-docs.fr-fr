---
title: Analyse de données JSON et d’AVRO dans Stream Analytics
description: Cet article explique comment utiliser des types de données complexes tels que des tableaux, et des données au format JSON ou CSV.
services: stream-analytics
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ad30d363c8e3ea0264ba79db5417e572614a6739
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496836"
---
# <a name="parse-json-and-avro-data-in-azure-stream-analytics"></a>Analyser des données JSON et Avro dans Azure Stream Analytics

Azure Stream Analytics prend en charge le traitement d’événements dans les formats de données CSV, JSON et Avro. Des données JSON et Avro peuvent contenir des types complexes, tels que des objets imbriqués (enregistrements) et des tableaux.

## <a name="array-data-types"></a>Données de type tableau

Les données de type tableau sont des collections ordonnées de valeurs. Certaines opérations courantes sur des valeurs de tableau sont décrites ci-dessous. Ces exemples supposent que les événements en entrée ont une propriété nommée « arrayField » qui est un type de données de tableau.

Ces exemples utilisent les fonctions [GetArrayElement](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelement-azure-stream-analytics), [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics) et [GetArrayLength](https://msdn.microsoft.com/azure/stream-analytics/reference/getarraylength-azure-stream-analytics), ainsi que l’opérateur [APPLY](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics).

## <a name="examples"></a>Exemples
Sélectionnez un élément de tableau à un index spécifié (en sélectionnant le premier élément du tableau) :

```SQL
SELECT
    GetArrayElement(arrayField, 0) AS firstElement
FROM input
```

Sélectionnez une longueur de tableau :

```SQL
SELECT
    GetArrayLength(arrayField) AS arrayLength
FROM input
```

Sélectionnez tous les éléments du tableau en tant qu’événements individuels. L’opérateur [APPLY](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) avec la fonction intégrée [GetArrayElements](https://msdn.microsoft.com/azure/stream-analytics/reference/getarrayelements-azure-stream-analytics) extrait tous les éléments du tableau en tant qu’événements individuels :

```SQL
SELECT
    arrayElement.ArrayIndex,
    arrayElement.ArrayValue
FROM input as event
CROSS APPLY GetArrayElements(event.arrayField) AS arrayElement
```

## <a name="record-data-types"></a>Données de type enregistrement
Des données de type enregistrement sont utilisées pour représenter des tableaux JSON et Avro quand des formats correspondants sont utilisés dans les flux de données en entrée. Ces exemples montrent un capteur qui lit les événements en entrée au format JSON. Voici un exemple d’événement unique :

```json
{
    "DeviceId" : "12345",
    "Location" :
    {
        "Lat": 47,
        "Long": 122
    },
    "SensorReadings" :
    {
        "Temperature" : 80,
        "Humidity" : 70,
        "CustomSensor01" : 5,
        "CustomSensor02" : 99
    }
}
```

## <a name="examples"></a>Exemples
Utilisez la notation par points (.) pour accéder aux champs imbriqués. Par exemple, cette requête sélectionne les coordonnées de Latitude et Longitude sous la propriété d’emplacement dans les données JSON précédentes :

```SQL
SELECT
    DeviceID,
    Location.Lat,
    Location.Long
FROM input
```

Utilisez la fonction [GetRecordPropertyValue](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordpropertyvalue-azure-stream-analytics) si le nom de propriété est inconnu. Par exemple, imaginez qu’un exemple de flux de données doive être joint avec des données de référence contenant des seuils pour chaque capteur :

```json
{
    "DeviceId" : "12345",
    "SensorName" : "Temperature",
    "Value" : 75
}
```

```SQL
SELECT
    input.DeviceID,
    thresholds.SensorName
FROM input
JOIN thresholds
ON
    input.DeviceId = thresholds.DeviceId
WHERE
    GetRecordPropertyValue(input.SensorReadings, thresholds.SensorName) > thresholds.Value
```

Pour convertir les champs d’enregistrement en événements distincts, utilisez l’opérateur [APPLIQUER](https://msdn.microsoft.com/azure/stream-analytics/reference/apply-azure-stream-analytics) avec la fonction [GetRecordProperties](https://msdn.microsoft.com/azure/stream-analytics/reference/getrecordproperties-azure-stream-analytics). Par exemple, pour convertir un exemple de flux en flux d’événements avec des lectures de capteurs individuels, vous pourriez utiliser la requête suivante :

```SQL
SELECT
    event.DeviceID,
    sensorReading.PropertyName,
    sensorReading.PropertyValue
FROM input as event
CROSS APPLY GetRecordProperties(event.SensorReadings) AS sensorReading
```

Vous pouvez sélectionner toutes les propriétés d’un enregistrement imbriqué à l’aide du caractère générique «  * ». Prenons l'exemple suivant :

```SQL
SELECT input.SensorReadings.*
FROM input
```

Le résultat est le suivant :

```json
{
    "Temperature" : 80,
    "Humidity" : 70,
    "CustomSensor01" : 5,
    "CustomSensor022" : 99
}
```

## <a name="see-also"></a>Voir aussi
[Types de données dans Azure Stream Analytics](https://msdn.microsoft.com/azure/stream-analytics/reference/data-types-azure-stream-analytics)
