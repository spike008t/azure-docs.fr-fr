---
title: Alertes de journal dans Azure Monitor
description: Déclenchez des e-mails, des notifications, des URL de sites web d’appel (webhooks) ou une automatisation lorsque les conditions de requête analytique que vous spécifiez sont remplies pour Alertes Azure.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 5/31/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: ae35c735cffeb8cd85af1f32bb2d14ede6dc6b69
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427415"
---
# <a name="log-alerts-in-azure-monitor"></a>Alertes de journal dans Azure Monitor

Cet article fournit des informations sur les alertes de journal, qui sont l’un des types d’alertes pris en charge dans les [Alertes Azure](../../azure-monitor/platform/alerts-overview.md), et qui permettent aux utilisateurs d’utiliser la plateforme d’analyse d’Azure comme base pour la génération d’alertes.

Une alerte de journal consiste en des règles de recherche pour les [journaux Azure Monitor](../../azure-monitor/learn/tutorial-viewdata.md) ou [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events). Pour en savoir plus sur son utilisation, voir [Création d’alertes de journal dans Azure](../../azure-monitor/platform/alerts-log.md)

> [!NOTE]
> Les données de journal populaires des [journaux Azure Monitor](../../azure-monitor/learn/tutorial-viewdata.md) sont désormais également disponibles sur la plateforme de métrique dans Azure Monitor. Pour la vue de détails, consultez [Alerte de métrique pour les journaux d’activité](../../azure-monitor/platform/alerts-metric-logs.md)


## <a name="log-search-alert-rule---definition-and-types"></a>Règle d’alerte de recherche dans les journaux - Définition et types

Des règles de recherche dans les journaux sont créées par Alertes Azure pour exécuter automatiquement des requêtes de journal spécifiées à intervalles réguliers.  Si les résultats de la requête de journal répondent à des critères particuliers, un enregistrement d’alerte est généré. La règle peut alors exécuter automatiquement une ou plusieurs actions à l’aide de [groupes d’actions](../../azure-monitor/platform/action-groups.md). Le rôle [Contributeur de surveillance Azure](../../azure-monitor/platform/roles-permissions-security.md) pour la création, la modification et la mise à jour des alertes de journal peut être nécessaire ainsi que des droits d’accès et d’exécution de requête pour la ou les cibles d’analyse dans la règle ou la requête d’alerte. Au cas où la création de l’utilisateur n’a pas accès à toutes les cibles d’analytique dans la règle d’alerte ou de la requête d’alerte - la création de règles peut échouer ou la règle d’alerte de journal est exécutée avec les résultats partiels.

Les règles de recherche dans les journaux sont définies par les détails suivants :

- **Requête de journal**.  La requête qui s’exécute chaque fois que la règle d’alerte est déclenchée.  Les enregistrements retournés par cette requête permettent de déterminer si une alerte doit être déclenchée. Une requête d’analytique peut concerner un espace de travail Log Analytics ou une application Application Insights spécifique et même porter sur [plusieurs ressources Log Analytics et Application Insights](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) sous réserve que l’utilisateur dispose de droits d’accès et de requête à toutes les ressources. 
    > [!IMPORTANT]
    > [requête d’inter-ressources](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) prise en charge dans les alertes de journal pour les alertes Application Insights et log pour [Analytique de journal configurées à l’aide des API de scheduledQueryRules](../../azure-monitor/platform/alerts-log-api-switch.md) uniquement.

    Certaines combinaisons et commandes d’analytique ne peuvent pas être utilisées dans des alertes de journal ; pour plus de détails, consultez [Requêtes d’alertes de journal dans Azure Monitor](../../azure-monitor/platform/alerts-log-query.md).

- **Période**  Spécifie l’intervalle de temps pour la requête. La requête retourne uniquement les enregistrements qui ont été créés dans cette plage précédant son exécution. La période limite les données extraites pour la requête de journal afin d’empêcher les abus, et contourne toute commande de temps (comme « il y a ») utilisée dans une requête de journal. <br>*Par exemple, si la période est définie sur 60 minutes et la requête exécutée à 13 h 15, seuls les enregistrements créés entre 12 h 15 et 13 h 15 sont retournés pour l’exécution de la requête de journal. Maintenant, si la requête de journal utilise une commande de temps telle que « il y a » (7d), la requête de journal est exécutée uniquement pour les données collectées entre 12 h 15 et 13 h 15, comme si celles-ci n’avaient existé que pendant les 60 minutes passées, et non pendant les sept jours de données spécifiés dans la requête de journal.*

- **Fréquence**.  Spécifie la fréquence à laquelle la requête doit être exécutée. Peut être toute valeur comprise entre 5 minutes et 24 heures. La valeur doit être inférieure ou égale à la période.  Si la valeur est supérieure à la période, vous risquez de manquer des enregistrements.<br>*Par exemple, imaginons une période de 30 minutes associée à une fréquence de 60 minutes.  Si la requête est exécutée à 13 h, les enregistrements entre 12 h 30 et 13 h sont renvoyés.  La requête s’exécute ensuite à 14 h, moment auquel elle renvoie les enregistrements entre 13 h 30 et 14 h.  Les enregistrements créés entre 13 h 00 et 13 h 30 ne sont jamais analysés.*

- **Seuil**.  Les résultats de la recherche dans les journaux sont évalués pour déterminer si une alerte doit être créée.  Le seuil diffère selon le type de règle d’alerte de recherche dans les journaux.

Les règles de recherche dans les journaux, qu’il s’agisse de [journaux Azure Monitor](../../azure-monitor/learn/tutorial-viewdata.md) ou d’[Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events), peuvent être de deux types. Chacun de ces types est décrit en détail dans les sections suivantes.

- **[Nombre de résultats](#number-of-results-alert-rules)** . Alerte unique créée lorsque le nombre d’enregistrements renvoyés par la recherche dans les journaux dépasse un nombre spécifié.
- **[Mesure métrique](#metric-measurement-alert-rules)** .  Alerte créée pour chaque objet des résultats de la recherche dans les journaux dont les valeurs dépassent le seuil spécifié.

Les différences entre les types de règles d’alerte sont présentées ci-dessous.

- Une règle d’alerte *Nombre de résultats* crée toujours une alerte unique, tandis que la règle d’alerte *Mesure de métriques* crée une alerte pour chaque objet dépassant le seuil.
- Les règles d’alerte *Nombre de résultats* créent une alerte quand le seuil est dépassé une seule fois. Les règles d’alerte *Mesure métrique* peuvent créer une alerte lorsque le seuil est dépassé un certain nombre de fois au cours d’un intervalle de temps spécifique.

### <a name="number-of-results-alert-rules"></a>Règles d’alerte Nombre de résultats

Les règles d’alerte **Nombre de résultats** créent une alerte unique lorsque le nombre d’enregistrement renvoyés par la requête de recherche dépasse le seuil spécifié. Ce type de règle d’alerte est idéal pour la gestion des événements, par exemple les journaux des événements Windows, les journaux d’activité Syslog, la réponse WebApp et les journaux d’activité personnalisés.  Vous pouvez créer une alerte quand un événement d’erreur particulier est créé, ou quand plusieurs événements d’erreur sont créés au cours d’une période particulière.

**Seuil** : Le seuil pour les règles d’alerte Nombre de résultats est supérieur ou inférieur à une valeur particulière.  Si le nombre d’enregistrements renvoyés par cette recherche dans les journaux satisfait à ce critère, une alerte est créée.

Pour déclencher une alerte sur un événement unique, définissez le nombre de résultats sur une valeur supérieure à 0 et vérifiez si un événement spécifique a été créé depuis la dernière exécution de la requête. Certaines applications peuvent consigner une erreur occasionnelle qui ne doit pas nécessairement déclencher une alerte.  Par exemple, l’application peut recommencer le processus à l’origine de l’événement d’erreur et voir sa nouvelle tentative couronnée de succès.  Dans ce cas, la création de l’alerte ne se justifie que si plusieurs événements sont créés au cours d’une période particulière.  

Dans certains cas, vous pouvez créer une alerte en l’absence d’événement.  Par exemple, un processus peut enregistrer des événements réguliers pour indiquer qu’il fonctionne correctement.  S’il ne consigne aucun de ces événements au cours d’une période particulière, une alerte doit être créée.  Dans ce cas, vous devez définir le seuil sur une valeur **inférieure à 1**.

#### <a name="example-of-number-of-records-type-log-alert"></a>Exemple d’alerte de journal de type Nombre d’enregistrements

Imaginez que vous cherchiez à savoir à quel moment votre application web renvoie aux utilisateurs une réponse avec le code 500, autrement dit une erreur interne au serveur. Il conviendrait de créer une règle d’alerte paramétrée comme suit :  

- **Requête :** requests | where resultCode == "500"<br>
- **Période :** 30 minutes<br>
- **Fréquence des alertes :** 5 minutes<br>
- **Valeur de seuil :** Supérieur à 0<br>

L’alerte exécute alors la requête toutes les 5 minutes, avec 30 minutes de données, pour rechercher tous les enregistrements associés à un code de résultat 500. Si un seul enregistrement est trouvé, elle déclenche l’alerte et lance l’action configurée.

### <a name="metric-measurement-alert-rules"></a>Règles d’alerte Mesure métrique

**Mesure métrique** règles d’alerte créent une alerte pour chaque objet dans une requête avec une valeur qui dépasse un seuil spécifié et conditions du déclencheur spécifiées. Contrairement aux **nombre de résultats** règles, d’alerte **mesure métrique** fonctionnent de règles d’alerte lorsque le résultat de l’analytique fournit une série chronologique. Elles se distinguent des règles d’alerte **Nombre de résultats** comme suit.

- **Fonction d’agrégation** : Détermine le calcul à effectuer et, éventuellement, un champ numérique à agréger.  Par exemple, **count()** renvoie le nombre d’enregistrements dans la requête, et **avg(CounterValue)** renvoie la moyenne du champ CounterValue sur l’intervalle. La fonction d’agrégation dans une requête doit être nommée/appelée : AggregatedValue et fournir une valeur numérique. 

- **Champ de groupe** : Un enregistrement avec une valeur agrégée est créé pour chaque instance de ce champ, et une alerte peut être générée pour chacun.  Par exemple, si vous souhaitez générer une alerte pour chaque ordinateur, vous pourriez utiliser **by Computer**. Dans le cas où plusieurs champs de groupe sont spécifiés dans une requête d’alerte, l’utilisateur peut spécifier le champ à utiliser pour trier les résultats à l’aide du paramètre **Agréger sur** (metricColumn)

    > [!NOTE]
    > L’option *Agréger sur* (metricColumn) est disponible uniquement pour les alertes de journal de type Mesure de métriques pour Application Insights et pour les alertes de journal pour [Log Analytics configurées à l’aide d’API scheduledQueryRules](../../azure-monitor/platform/alerts-log-api-switch.md).

- **Intervalle** :  Définit l’intervalle de temps pendant lequel les données sont agrégées.  Par exemple, si vous avez spécifié **5 minutes**, un enregistrement est créé pour chaque instance du champ de groupe agrégé à intervalles de 5 minutes sur la période spécifiée pour l’alerte.

    > [!NOTE]
    > Une fonction Bin doit être utilisée dans la requête pour spécifier l’intervalle. Étant donné qu’une commande bin() peut entraîner des intervalles inégaux, une alerte convertit automatiquement une commande bin en commande bin_at avec l’heure appropriée lors de l’exécution, pour garantir des résultats avec un point fixe. Le type d’alerte de journal Mesure de métriques est conçu pour être utilisé avec les requêtes comportant jusqu’à trois instances d’une commande bin().
    
- **Seuil** : Le seuil des règles d’alerte Mesure métrique est défini par une valeur d’agrégation et un nombre de violations.  Si un point de données de la recherche dans les journaux dépasse cette valeur, elle est considérée comme une violation.  Si le nombre de violations pour un objet dans les résultats dépasse la valeur spécifiée, une alerte est créée pour cet objet.

Une configuration incorrecte de l’option *Agréger sur* ou *metricColumn* peut entraîner un mauvais déclenchement des règles d’alerte. Pour plus d’informations, consultez la [section de résolution La règle d’alerte Mesure métrique est incorrecte](alert-log-troubleshoot.md#metric-measurement-alert-rule-is-incorrect).

#### <a name="example-of-metric-measurement-type-log-alert"></a>Exemple d’alerte de journal de type Mesure de métriques

Prenons le scénario suivant : vous souhaitez créer une alerte si le taux d’utilisation du processeur d’un ordinateur dépasse 90 % à trois reprises en l’espace de 30 minutes.  Il conviendrait de créer une règle d’alerte paramétrée comme suit :  

- **Requête :** Perf | where ObjectName == "Processor" and CounterName == "% Processor Time" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer<br>
- **Période :** 30 minutes<br>
- **Fréquence des alertes :** 5 minutes<br>
- **Logique d'alerte - Condition et seuil :** Supérieur à 90<br>
- **Champ de groupe (Aggregate-on) :** Computer
- **Déclencher l’alerte selon :** nombre total de violations supérieur à 2<br>

La requête créerait une valeur moyenne pour chaque ordinateur à intervalles de 5 minutes.  Cette requête serait exécutée toutes les 5 minutes pour les données collectées au cours des 30 minutes précédentes. Le champ de groupe choisi (Aggregate-on) correspondant à la colonne « Ordinateur », la valeur AggregatedValue est fractionnée pour différentes valeurs « Ordinateur » et l'utilisation moyenne du processeur de chaque ordinateur est déterminée pour une période de 5 minutes.  À titre d'exemple, le résultat de la requête pour trois ordinateurs se présenterait comme suit.


|TimeGenerated [UTC] |Computer  |AggregatedValue  |
|---------|---------|---------|
|20XX-xx-xxT01:00:00Z     |   srv01.contoso.com      |    72     |
|20XX-xx-xxT01:00:00Z     |   srv02.contoso.com      |    91     |
|20XX-xx-xxT01:00:00Z     |   srv03.contoso.com      |    83     |
|...     |   ...      |    ...     |
|20xx-xx-xxT01:30:00Z     |   srv01.contoso.com      |    88     |
|20xx-xx-xxT01:30:00Z     |   srv02.contoso.com      |    84     |
|20xx-xx-xxT01:30:00Z     |   srv03.contoso.com      |    92     |

Si le résultat de la requête devait être tracé, il se présenterait comme suit.

![Résultats de l’exemple de requête](media/alerts-unified-log/metrics-measurement-sample-graph.png)

Dans cet exemple, nous voyons, par période de 5 minutes pour chacun des trois ordinateurs - utilisation moyenne du processeur telle que calculée pour 5 minutes. Seuil de 90 violé par srv01 une seule fois à 1:25. En comparaison, srv02 dépasse le seuil de 90 à 1:10, 1:15 et 1:25, alors que srv03 le dépasse de 90 à 1:10, 1:15, 1:20 et 1:30.
L'alerte étant configurée pour se déclencher au-delà de deux violations, nous constatons que seuls srv02 et srv03 répondent aux critères. Ainsi, des alertes distinctes seraient créées pour srv02 et srv03 dans la mesure où le seuil de 90 % a été dépassé deux fois au cours de plusieurs périodes.  Si le paramètre *Déclencher l’alerte selon :* était configuré pour l’option *Violations continues*, une alerte serait déclenchée **uniquement** pour srv03, car il a dépassé le seuil lors de trois périodes consécutives entre 1:10 et 1:20. Et **non** pour srv02, car il a dépassé le seuil lors de deux périodes entre 1:10 et 1:15.

## <a name="log-search-alert-rule---firing-and-state"></a>Règle d’alerte Recherche dans les journaux - déclenchement et état

La règle d’alerte Recherche dans les journaux s’applique sur la logique indiquée par l’utilisateur, conformément à la configuration et à la requête d’analyse personnalisée utilisée. Depuis la logique d’analyse, y compris la condition exacte ou raison pourquoi doit déclencher la règle d’alerte est encapsulé dans une requête analytique - qui peut varier dans chaque règle d’alerte de journal. Alertes Azure a des informations rare du scénario en cours d’évaluation lorsque la condition de seuil d’alerte de recherche de journal est atteint ou dépassée spécifique cause sous-jacente (ou). Par conséquent, les alertes de journal portent le nom que sans état. Et les règles d’alerte seront continuent de déclencher, tant que la condition d’alerte est remplie par le résultat de requête analytique personnalisée fournie. Sans l’alerte chaque bien résolu, en tant que la logique de la cause exacte de l’échec de la surveillance est masquée à l’intérieur de la requête analytique fournie par l’utilisateur. Il existe n’actuellement aucun mécanisme pour les alertes Azure Monitor pour effet de façon concluante de déduire l’origine du problème à résoudre.

Nous permet de voir la même chose avec un exemple pratique. Supposons que nous disposons d’une règle d’alerte journal appelée *alerte de journal Contoso*, selon la configuration dans le [exemple fourni pour l’alerte de journal de type de nombre de résultats](#example-of-number-of-records-type-log-alert) - où la requête d’alerte personnalisée est conçue pour rechercher 500 code de résultat dans les journaux.

- À 13 h 05 lorsque Contoso alerte de journal a été exécutée par des alertes Azure, le résultat de recherche de journal générait zéro enregistrement avec le code de sortie de 500. Dans la mesure où zéro est inférieur au seuil et l’alerte n’est pas déclenchée.
- À l’itération suivante à 1:10 PM lorsque Contoso alerte de journal a été exécutée par des alertes Azure, les résultats de recherche de journal fournissent cinq enregistrements avec le code de résultat 500. Étant donné que cinq dépasse le seuil et l’alerte est déclenchée, dont les actions sont associées se déclenche.
- À 1 h 15 Lorsque Contoso alerte de journal a été exécutée par des alertes Azure, le résultat de recherche de journal fourni deux enregistrements avec le code de résultat 500. Dans la mesure où deux dépasse le seuil et l’alerte est déclenchée, dont les actions sont associées se déclenche.
- Désormais à l’itération suivante à 20 h Lorsque Contoso alerte de journal a été exécutée par une alerte Azure, les résultats de recherche de journal fourni là encore zéro enregistrement avec le code de résultat 500. Dans la mesure où zéro est inférieur au seuil et l’alerte n’est pas déclenchée.

Mais dans le cas répertorié ci-dessus, à 13 h 15 - alertes Azure ne peut pas déterminer que les problèmes sous-jacents vu à 1:10 sont conservées et s’il existe des échecs de nouveau nets. Comme la requête fournie par l’utilisateur peut être en prenant en compte les enregistrements antérieurs - alertes Azure peuvent être vraiment. Dans la mesure où la logique de l’alerte est encapsulée dans la requête d’alerte - donc les deux enregistrements avec le code de résultat 500 vu à 1 h 15 peuvent ou peut ne pas être déjà vu à 13 h 10. Par conséquent, pour raisonnables, lors de l’alerte de journal de Contoso est exécutée à 13 h 15, action configurée est déclenchée à nouveau. Maintenant à 1 h 20 lorsque zéro enregistrement sont visibles avec le code de résultat 500 - alertes Azure ne peut pas être certains que la cause de code de résultat 500 vu à 1 h 10 et 1:15 PM est maintenant résolue et alertes Azure Monitor peuvent déduire en toute confiance les problèmes de 500 erreur ne se produira pour la même raison s à nouveau. Par conséquent, Contoso--alerte de journal sera ne modifié pas résolu dans le tableau de bord Azure alerte et/ou de notifications envoyées indiquant la résolution d’alerte. À la place l’utilisateur qui comprend la condition exacte ou la raison pour le code incorporé dans la requête analytique, peut [marquez l’alerte comme étant fermé](alerts-managing-alert-states.md) en fonction des besoins.

## <a name="pricing-and-billing-of-log-alerts"></a>Tarification et facturation des alertes de journal

La tarification applicable aux alertes de journal est présentée à la page [Tarification d’Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/). Dans les factures d’Azure, les Alertes de journal sont représentées comme étant de type `microsoft.insights/scheduledqueryrules` avec :

- Alertes de journal sur Application Insights, affichées avec le nom exact de l’alerte, ainsi que le groupe de ressources et les propriétés de l’alerte
- Alertes de journal sur Log Analytics, affichées avec le nom exact de l’alerte ainsi que le groupe de ressources et les propriétés de l’alerte (création avec l’[API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules))

L’[API Log Analytics héritée](../../azure-monitor/platform/api-alerts.md) présente des actions d’alerte et des planifications dans le cadre de la recherche enregistrée Log Analytics, et non des [ressources Azure](../../azure-resource-manager/resource-group-overview.md) propres. Par conséquent, pour activer la facturation pour de telles alertes de journal héritées créées pour Log Analytics à l’aide du portail Microsoft Azure **sans** [basculer vers la nouvelle API](../../azure-monitor/platform/alerts-log-api-switch.md) ou via l’[API Log Analytics héritée](../../azure-monitor/platform/api-alerts.md), des pseudo règles d’alerte masquées sont créées sur `microsoft.insights/scheduledqueryrules` pour la facturation sur Azure. Les pseudo règles d’alerte masquées créées pour la facturation sur `microsoft.insights/scheduledqueryrules` comme indiqué en tant que `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>`, de pair avec les propriétés de groupe de ressources et d’alerte.

> [!NOTE]
> Si des caractères non valides tels que `<, >, %, &, \, ?, /` sont présents, ils seront remplacés par `_` dans le nom de la pseudo règle d’alerte masquée et par conséquent, également dans la facture Azure.

Pour supprimer les ressources scheduleQueryRules masquées créées pour la facturation des règles d’alerte à l’aide de l’[API Log Analytics héritée](api-alerts.md), l’utilisateur peut effectuer l’une des opérations suivantes :

- N’importe quel utilisateur peut [changer la préférence d’API pour les règles d’alerte sur l’espace de travail Log Analytics](../../azure-monitor/platform/alerts-log-api-switch.md) et sans perte de ses règles d’alerte ou déplacement de surveillance vers l’API [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) conforme à Azure Resource Manager. Ainsi, vous n’avez plus besoin de créer des pseudo règles d’alerte masquées pour la facturation.
- Ou si l’utilisateur ne souhaite pas changer de préférence d’API, l’utilisateur devra **supprimer** la planification d’origine et l’action d’alerte au moyen de l’[API Log Analytics héritée](api-alerts.md) ou supprimer dans le [portail Microsoft Azure la règle d’alerte de journal d’origine](../../azure-monitor/platform/alerts-log.md#view--manage-log-alerts-in-azure-portal)

En outre pour les ressources scheduleQueryRules masqué créées pour la facturation des règles d’alerte à l’aide de [API d’Analytique de journal héritée](api-alerts.md), toute opération de modification telles que PUT échouera. Comme le `microsoft.insights/scheduledqueryrules` sont de type pseudo règles aux fins de facturation les règles d’alerte créées à l’aide [API d’Analytique de journal héritée](api-alerts.md). Toute modification de la règle d’alerte doit être effectuée à l’aide [API d’Analytique de journal héritée](api-alerts.md) (ou) utilisateur peut [basculer la préférence de l’API pour les règles d’alerte](../../azure-monitor/platform/alerts-log-api-switch.md) à utiliser [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) à la place.

## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur la [création d’alertes de journal dans Azure](../../azure-monitor/platform/alerts-log.md).
* Comprendre les [webhooks dans les alertes de journal dans Azure](alerts-log-webhook.md).
* En savoir plus sur [Alertes Azure](../../azure-monitor/platform/alerts-overview.md).
* En savoir plus sur [Application Insights](../../azure-monitor/app/analytics.md).
* En savoir plus sur [Log Analytics](../../azure-monitor/log-query/log-query-overview.md).