---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 9d9790c9b3dbe3b130be999dd76092ae64f7b52c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66169953"
---
|Nom du paramètre| Type | Description | Valeurs possibles|
|-|-|-|-|
| /ServerMode|Obligatoire|Spécifie si les serveurs de configuration et de processus doivent être installés ou si seul le serveur de processus doit être installé|CS<br>PS|
|/InstallLocation|Obligatoire|Dossier d’installation des composants| N’importe quel dossier sur l’ordinateur|
|/MySQLCredsFilePath|Obligatoire|Chemin d’accès du fichier où sont stockées les informations d’identification du serveur MySQL|Le fichier doit être au format spécifié ci-dessous|
|/VaultCredsFilePath|Obligatoire|Chemin d’accès du fichier d’informations d’identification du coffre|Chemin d’accès valide du fichier|
|/EnvType|Obligatoire|Type d’environnement à protéger |VMware<br>NonVMware|
|/PSIP|Obligatoire|Adresse IP de la carte réseau à utiliser pour le transfert de données de réplication| Une adresse IP valide|
|/CSIP|Obligatoire|Adresse IP de la carte réseau sur laquelle le serveur de configuration écoute| Une adresse IP valide|
|/PassphraseFilePath|Obligatoire|Chemin d’accès complet vers l’emplacement du fichier de phrase secrète|Chemin d’accès valide du fichier|
|/BypassProxy|Facultatif|Indique que le serveur de configuration se connecte à Azure sans proxy|Pour obtenir cette valeur de Venu|
|/ProxySettingsFilePath|Facultatif|Paramètres de proxy (Le proxy par défaut nécessite une authentification, sinon il faut un proxy personnalisé)|Le fichier doit être au format spécifié ci-dessous|
|DataTransferSecurePort|Facultatif|Numéro de port sur le PSIP à utiliser pour les données de réplication| Numéro de port valide (valeur par défaut : 9433)|
|/SkipSpaceCheck|Facultatif|Ignorer la vérification des espaces pour le disque du cache| |
|/AcceptThirdpartyEULA|Obligatoire|Cet indicateur implique l’acceptation du CLUF tiers| |
|/ShowThirdpartyEULA|Facultatif|Affiche le CLUF tiers. S’il est fourni en entrée, tous les autres paramètres sont ignorés| |
