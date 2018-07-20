---
title: Vue d’ensemble du modèle de licence Widevine avec Azure Media Services | Microsoft Docs
description: Cette rubrique donne un aperçu d’un modèle de licence Widevine utilisé pour configurer des licences Widevine.
author: juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: juliako
ms.openlocfilehash: e3af5efd253458401c13f6174d9567f932482eb0
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133309"
---
# <a name="widevine-license-template-overview"></a>Vue d’ensemble du modèle de licence Widevine

Vous pouvez utiliser Azure Media Services pour configurer et demander des licences Google Widevine. Quand le lecteur tente de lire votre contenu Widevine protégé, une demande est envoyée au service de remise de licence pour obtenir une licence. Si le service de licence approuve la demande, le service émet la licence. Elle est envoyée au client et utilisée pour déchiffrer et lire le contenu spécifié.

Une demande de licence Widevine se présente sous forme de message JSON.  

>[!NOTE]
> Vous pouvez créer un message vide sans valeur, simplement « {} ». Dans ce cas, un modèle de licence est créé avec les valeurs par défaut. Les valeurs par défaut fonctionnent pour la plupart des cas. Les scénarios de remise de licence Microsoft doivent toujours utiliser les valeurs par défaut. Si vous devez définir les valeurs « provider » et « content_id », un fournisseur doit correspondre aux informations d’identification Widevine.

    {  
       "payload":"<license challenge>",
       "content_id": "<content id>"
       "provider": "<provider>"
       "allowed_track_types":"<types>",
       "content_key_specs":[  
          {  
             "track_type":"<track type 1>"
          },
          {  
             "track_type":"<track type 2>"
          },
          …
       ],
       "policy_overrides":{  
          "can_play":<can play>,
          "can persist":<can persist>,
          "can_renew":<can renew>,
          "rental_duration_seconds":<rental duration>,
          "playback_duration_seconds":<playback duration>,
          "license_duration_seconds":<license duration>,
          "renewal_recovery_duration_seconds":<renewal recovery duration>,
          "renewal_server_url":"<renewal server url>",
          "renewal_delay_seconds":<renewal delay>,
          "renewal_retry_interval_seconds":<renewal retry interval>,
          "renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>Message JSON

| NOM | Valeur | Description |
| --- | --- | --- |
| payload |Chaîne codée en Base64 |La demande de licence envoyée par un client. |
| content_id |Chaîne codée en Base64 |Identificateur utilisé afin de dériver l’ID de clé et la clé de contenu pour chaque content_key_specs.track_type. |
| provider |chaîne |Utilisé pour rechercher les stratégies et les clés de contenu. Si la remise de clé Microsoft est utilisée pour la remise de licence Widevine, ce paramètre est ignoré. |
| policy_name |chaîne |Nom d'une stratégie précédemment enregistrée. facultatif. |
| allowed_track_types |enum |SD_ONLY ou SD_HD. Contrôle les clés de contenu incluses dans une licence. |
| content_key_specs |Tableau de structures JSON (voir la section « Spécifications de clé de contenu »).  |Un contrôle plus fin sur les clés de contenu à retourner. Pour plus d’informations, consultez la section « Spécifications de clé de contenu ». Une seule valeur allowed_track_types et content_key_specs peut être spécifiée. |
| use_policy_overrides_exclusively |Booléen, true ou false |Utiliser les attributs de la stratégie spécifiés par policy_overrides, et ignorer toutes les stratégies stockées précédemment. |
| policy_overrides |Structure JSON (voir la section « Remplacements de stratégies »). |Paramètres de stratégie pour cette licence.  Si cette ressource comporte une stratégie prédéfinie, les valeurs spécifiées sont utilisées. |
| session_init |Structure JSON (voir « Initialisation de la session »). |Des données facultatives sont passées à la licence. |
| parse_only |Booléen, true ou false |La demande de licence est analysée, mais aucune licence n’est émise. Toutefois, les valeurs de la demande de licence sont retournées dans la réponse. |

## <a name="content-key-specs"></a>Spécifications de clé de contenu
S’il existe une stratégie préexistante, il est inutile de spécifier des valeurs dans la spécification de clé de contenu. La stratégie préexistante associée à ce contenu est utilisée pour déterminer la protection de sortie, telle que HDCP (High-bandwidth Digital Content Protection) et CGMS (Copy General Management System). Si aucune stratégie préexistante n’est inscrite auprès du serveur de licences Widevine, le fournisseur de contenu peut injecter les valeurs dans la demande de licence.   

Chaque valeur content_key_specs doit être spécifiée pour toutes les pistes, quelle que soit l’option use_policy_overrides_exclusively. 

| NOM | Valeur | Description |
| --- | --- | --- |
| content_key_specs. track_type |chaîne |Un nom de type de piste. Si la valeur content_key_specs est spécifiée dans la demande de licence, assurez-vous de spécifier tous les types de pistes de façon explicite. Dans le cas contraire, vous serez confronté à un échec de lecture des 10 dernières secondes. |
| content_key_specs  <br/> security_level |uint32 |Définit la configuration requise de robustesse du client pour la lecture. <br/> - Chiffrement whitebox logiciel requis. <br/> - Chiffrement logiciel et décodeur masqué requis. <br/> - Le matériel de clé et les opérations de chiffrement doivent être effectués dans un environnement d’exécution approuvé soutenu par le matériel. <br/> - Le chiffrement et le décodage du contenu doivent être effectués dans un environnement d’exécution approuvé soutenu par le matériel.  <br/> - Le chiffrement, le décodage et le traitement du support (compressé et décompressé) doivent être gérés dans un environnement d’exécution approuvé soutenu par le matériel. |
| content_key_specs <br/> required_output_protection.hdc |string : HDCP_NONE, HDCP_V1 ou HDCP_V2 |Indique si HDCP est obligatoire. |
| content_key_specs <br/>key |Chaîne<br/>encodée Base64 |Clé de contenu à utiliser pour cette piste. Si spécifié, track_type ou key_id est requis. Le fournisseur de contenu peut utiliser cette option pour injecter la clé de contenu de cette piste, au lieu de laisser le serveur de licences Widevine générer ou rechercher une clé. |
| content_key_specs.key_id |Chaîne binaire codée en Base64, 16 octets |Identificateur unique pour la clé. |

## <a name="policy-overrides"></a>Remplacements de stratégies
| NOM | Valeur | Description |
| --- | --- | --- |
| policy_overrides&#46;can_play |Booléen, true ou false |Indique que la lecture du contenu est autorisée. La valeur par défaut est false. |
| policy_overrides&#46;can_persist |Booléen, true ou false |Indique que la licence peut être rendue persistante dans le stockage non volatile pour une utilisation hors connexion. La valeur par défaut est false. |
| policy_overrides&#46;can_renew |Booléen, true ou false |Indique que le renouvellement de cette licence est autorisé. Si la valeur est true, la durée de la licence peut être étendue par pulsation. La valeur par défaut est false. |
| policy_overrides&#46;license_duration_seconds |int64 |Indique la fenêtre de temps pour cette licence spécifique. La valeur 0 indique qu'il n'existe aucune limite de durée. La valeur par défaut est 0. |
| policy_overrides&#46;rental_duration_seconds |int64 |Indique la fenêtre de temps où la lecture est autorisée. La valeur 0 indique qu'il n'existe aucune limite de durée. La valeur par défaut est 0. |
| policy_overrides&#46;playback_duration_seconds |int64 |Fenêtre d’affichage du temps une fois la lecture démarrée dans la durée de la licence. La valeur 0 indique qu'il n'existe aucune limite de durée. La valeur par défaut est 0. |
| policy_overrides&#46;renewal_server_url |chaîne |Toutes les demandes de pulsation (renouvellement) de cette licence sont dirigées vers l’URL spécifiée. Ce champ est utilisé uniquement si can_renew a la valeur true. |
| policy_overrides&#46;renewal_delay_seconds |int64 |Nombre de secondes après license_start_time avant la première tentative de renouvellement. Ce champ est utilisé uniquement si can_renew a la valeur true. La valeur par défaut est 0. |
| policy_overrides&#46;renewal_retry_interval_seconds |int64 |Spécifie le délai en secondes entre les demandes de renouvellement de licence suivantes, en cas d'échec. Ce champ est utilisé uniquement si can_renew a la valeur true. |
| policy_overrides&#46;renewal_recovery_duration_seconds |int64 |La fenêtre de temps pendant laquelle la lecture peut se poursuivre pendant la tentative de renouvellement, malgré l’échec en raison de problèmes de backend avec le serveur de licences. La valeur 0 indique qu'il n'existe aucune limite de durée. Ce champ est utilisé uniquement si can_renew a la valeur true. |
| policy_overrides&#46;renew_with_usage |Booléen, true ou false |Indique que la licence est envoyée pour renouvellement quand l’utilisation commence. Ce champ est utilisé uniquement si can_renew a la valeur true. |

## <a name="session-initialization"></a>Initialisation de la session
| NOM | Valeur | Description |
| --- | --- | --- |
| provider_session_token |Chaîne codée en Base64 |Ce jeton de session est repassé dans la licence et existe dans les renouvellements suivants. Le jeton de session n’est pas persistant au-delà des sessions. |
| provider_client_token |Chaîne codée en Base64 |Jeton client à renvoyer dans la réponse de la licence. Si la demande de licence contient un jeton client, cette valeur est ignorée. Le jeton client persiste au-delà des sessions de la licence. |
| override_provider_client_token |Booléen, true ou false |Si la valeur est false et si la demande de licence contient un jeton client, utilisez le jeton de la demande même si un jeton client a été spécifié dans cette structure. Si la valeur est true, utilisez toujours le jeton spécifié dans cette structure. |

## <a name="configure-your-widevine-license-with-net"></a>Configurer vos licences Widevine avec .NET 

Media Services propose une classe vous permettant de configurer une licence Widevine. Pour créer la licence, transférez JSON vers [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate).

Pour configurer le modèle, vous pouvez :

1.  Créer ou coder directement une chaîne JSON (qui peut être sujette aux erreurs) ;

    ```csharp
        ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = @"{""allowed_track_types"":""SD_HD"",""content_key_specs"":[{""track_type"":""SD"",""security_level"":1,""required_output_protection"":{""hdcp"":""HDCP_V2""}}],""policy_overrides"":{""can_play"":true,""can_persist"":true,""can_renew"":false}}"
    };
    ```

2.  Créer les classes nécessaires avec des propriétés mappées sur ces attributs JSON et les instancier avant de les sérialiser sur une chaîne JSON. Voici un exemple de ces classes et de la façon dont elles sont instanciées et sérialisées.

    ```csharp
    public class policy_overrides
    {
        public bool can_play { get; set; }
        public bool can_persist { get; set; }
        public bool can_renew { get; set; }
        public int rental_duration_seconds { get; set; }    //Indicates the time window while playback is permitted. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int playback_duration_seconds { get; set; }  //The viewing window of time after playback starts within the license duration. A value of 0 indicates that there is no limit to the duration. Default is 0.
        public int license_duration_seconds { get; set; }   //Indicates the time window for this specific license. A value of 0 indicates that there is no limit to the duration. Default is 0.
    }

    public class content_key_spec
    {
        public string track_type { get; set; }
        public int security_level { get; set; }
        public output_protection required_output_protection { get; set; }
    }

    public class output_protection
    {
        public string hdcp { get; set; }
    }

    public class widevine_template
    {
        public string allowed_track_types { get; set; }
        public content_key_spec[] content_key_specs { get; set; }
        public policy_overrides policy_overrides { get; set; }
    }
    ```

### <a name="configure-the-license"></a>Configurer la licence

Utilisez les classes définies dans la section précédente pour créer un JSON utilisé pour configurer [WidevineTemplate](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.contentkeypolicywidevineconfiguration.widevinetemplate?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_ContentKeyPolicyWidevineConfiguration_WidevineTemplate) :

```csharp
void ConfigureLicense()
{
    widevine_template objwidevine_template = new widevine_template()
    {
        allowed_track_types = "SD_HD",
        content_key_specs = new content_key_spec[]
        {
            new content_key_spec()
            {
                track_type = "SD",
                security_level = 1,
                required_output_protection = new output_protection()
                {
                hdcp = "HDCP_V2"
                }
            }
        },
        policy_overrides = new policy_overrides()
        {
            can_play = true,
            can_persist = true,
            can_renew = false,
            license_duration_seconds = 2592000,
            playback_duration_seconds = 10800,
            rental_duration_seconds = 604800,
        }
    };

    ContentKeyPolicyWidevineConfiguration objContentKeyPolicyWidevineConfiguration = new ContentKeyPolicyWidevineConfiguration
    {
        WidevineTemplate = Newtonsoft.Json.JsonConvert.SerializeObject(objwidevine_template)
    };
}
```

## <a name="next-steps"></a>Étapes suivantes

[Vue d’ensemble](content-protection-overview.md)