---
title: Filtrage géographique sur un domaine Azure Front Door Service | Microsoft Docs
description: Dans cet article, vous en apprendrez plus sur la stratégie de filtrage géographique Azure Front Door Service
services: frontdoor
documentationcenter: ''
author: KumudD
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: kumud;tyao
ms.openlocfilehash: da56c9664ce533709676ded677add4a2391dce03
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64865861"
---
# <a name="what-is-geo-filtering-on-a-domain-for-azure-front-door"></a>Présentation du filtrage géographique sur un domaine Azure Front Door

Par défaut, Azure Front Door Service répond aux demandes de l’utilisateur, quel que soit son emplacement. Toutefois, dans certains cas, vous devrez peut-être limiter l’accès à vos applications web en fonction du pays/de la région. Le service de pare-feu d’applications web (WAF) au niveau de Front Door vous permet de définir une stratégie à l’aide de règles d’accès personnalisées pour un chemin d’accès spécifique à votre point de terminaison. Cette stratégie autorisera ou bloquera l’accès à partir de pays/régions spécifiés. 

Une stratégie WAF est généralement constituée d’un ensemble de règles personnalisées. Une règle se compose de conditions de correspondance, d’une action et d’une priorité. Dans les conditions de correspondance, vous pourrez définir une variable de correspondance, un opérateur et une valeur de correspondance.  En ce qui concerne la règle de filtrage géographique, la variable de correspondance est REMOTE_ADDR, l’opérateur est GeoMatch, la valeur correspond à l’indicatif à deux lettres du pays d’intérêt. Vous pouvez combiner une condition GeoMatch et une condition de correspondance de chaîne REQUEST_URI pour créer une règle de filtrage géographique basé sur le chemin d’accès.

Vous pouvez configurer une stratégie de filtrage géographique pour votre porte d’entrée à l’aide d’[Azure PowerShell](front-door-tutorial-geo-filtering.md) ou de notre [modèle de démarrage rapide](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering).

## <a name="country-code-reference"></a>Référence de l’indicatif de pays

|Code pays | Nom du pays |
| ----- | ----- |
| AD | Andorre |
| AE | Émirats Arabes Unis|
| AF | Afghanistan|
| AG | Antigua-et-Barbuda|
| AL | Albanie|
| AM | Arménie|
| AO | Angola|
| AR | Argentine|
| AS | Samoa américaines|
| AT | Autriche|
| AU | Australie|
| AZ | Azerbaïdjan|
| BA | Bosnie-Herzégovine|
| BB | Barbade|
| BD | Bangladesh|
| BE | Belgique|
| BF | Burkina Faso|
| BG | Bulgarie|
| BH | Bahreïn|
| BI | Burundi|
| BJ | Bénin|
| BL | Saint-Barthélemy|
| BN | Brunei Darussalam|
| BO | Bolivie|
| BR | Brésil|
| BS | Les Bahamas|
| BT | Bhoutan|
| BW | Botswana|
| BY | Bélarus|
| BZ | Belize|
| CA | Canada|
| CD | République démocratique du Congo|
| CF | République centrafricaine|
| CH | Suisse|
| CI | Côte d'Ivoire|
| CL | Chili|
| CM | Cameroun|
| CN | Chine|
| CO | Colombie|
| CR | Costa Rica|
| CU | Cuba|
| CV | Cabo Verde|
| CY | Chypre|
| CZ | République tchèque|
| DE | Allemagne|
| DK | Danemark|
| DO | République dominicaine|
| DZ | Algérie|
| EC | Équateur|
| EE | Estonie|
| EG | Égypte|
| ES | Espagne|
| ET | Éthiopie|
| FI | Finlande|
| FJ | Fidji|
| FM | Micronésie, États fédérés de|
| FR | France|
| GB | Royaume-Uni|
| GE | Géorgie|
| GF | Guyane française|
| GH | Ghana|
| GN | Guinée|
| GP | Guadeloupe|
| GR | Grèce|
| GT | Guatemala|
| GY | Guyane|
| HK | Hong Kong (R.A.S.)|
| HN | Honduras|
| HR | Croatie|
| HT | Haïti|
| HU | Hongrie|
| ID | Indonésie|
| IE | Irlande|
| IL | Israël|
| IN | Inde|
| IQ | Irak|
| IR | Iran, République islamique|
| IS | Islande|
| IT | Italie|
| JM | Jamaïque|
| JO | Jordanie|
| JP | Japon|
| KE | Kenya|
| KG | Kirghizistan|
| KH | Cambodge|
| KI | Kiribati|
| KN | Saint-Christophe-et-Niévès|
| KP | Corée, République populaire démocratique de|
| KR | Corée, République de|
| KW | Koweït|
| KY | Caïmans (îles)|
| KZ | Kazakhstan|
| LA | Laos, République démocratique|
| LB | Liban|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesotho|
| LT | Lituanie|
| LU | Luxembourg|
| LV | Lettonie|
| LY | Libye |
| MA | Maroc|
| MD | Moldova|
| MG | Madagascar|
| MK | Macédoine du Nord|
| ML | Mali|
| MM | Myanmar|
| MN | Mongolie|
| MO | Macao R.A.S.|
| MQ | Martinique|
| MR | Mauritanie|
| MT | Malte|
| MV | Maldives|
| MW | Malawi|
| MX | Mexique|
| MY | Malaisie|
| MZ | Mozambique|
| N/D | Namibie|
| NE | Niger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Pays-bas|
| NON | Norvège|
| NP | Népal|
| NR | Nauru|
| NZ | Nouvelle-Zélande|
| OM | Oman|
| PA | Panama|
| PE | Pérou|
| PH | Philippines|
| PK | Pakistan|
| PL | Pologne|
| PR | Porto Rico|
| PT | Portugal|
| PW | Palau|
| PY | Paraguay|
| QA | Qatar|
| RE | La réunion|
| RO | Roumanie|
| RS | Serbie|
| RU | Fédération de Russie|
| RW | Rwanda|
| SA | Arabie Saoudite|
| SD | Soudan|
| SE | Suède|
| SG | Singapour|
| SI | Slovénie|
| SK | Slovaquie|
| SN | Sénégal|
| SO | Somalie|
| SR | Surinam|
| SS | Soudan du Sud|
| SV | El Salvador|
| SY | République arabe syrienne|
| SZ | Swaziland|
| TC | Turques-et-Caïques (îles)|
| TG | Togo|
| MJ | Thaïlande|
| TN | Tunisie|
| TR | Turquie|
| TT | Trinité-et-Tobago|
| TW | Taïwan|
| TZ | Tanzanie, République de|
| UA | Ukraine|
| UG | Ouganda|
| FR | États-Unis|
| UY | Uruguay|
| UZ | Ouzbékistan|
| VC | Saint-Vincent-et-les-Grenadines|
| VE | Venezuela|
| VG | Vierges britanniques (îles)|
| VI | Vierges américaines (îles)|
| VN | Vietnam|
| ZA | Afrique du Sud|
| ZM | Zambie|
| ZW | Zimbabwe|

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur la [sécurité de la couche Application avec Front Door](front-door-application-security.md).
- Découvrez comment [créer une porte d’entrée](quickstart-create-front-door.md).
