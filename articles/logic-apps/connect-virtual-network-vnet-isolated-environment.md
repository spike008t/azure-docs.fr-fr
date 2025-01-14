---
title: Connexion à des réseaux virtuels Azure à partir d’Azure Logic Apps via un environnement de service d’intégration (ISE)
description: Créer un environnement de service d’intégration (ISE, Integration Service Environment) afin que les applications logiques et les comptes d’intégration puissent accéder aux réseaux virtuels Azure, tout en restant privés et isolés de l’environnement Azure public ou « mondial »
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: bd1f06c93a75673f86f0c52f78cad8a60f7a1a1e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65961455"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Connexion à des réseaux virtuels Azure à partir d’Azure Logic Apps à l'aide d'un environnement de service d’intégration (ISE)

Pour les scénarios où vos applications logiques et vos comptes d’intégration ont besoin d’accéder à un [réseau virtuel Azure](../virtual-network/virtual-networks-overview.md), créez un [*environnement de service d’intégration* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Une fenêtre ISE est un environnement privé et isolé qui utilise le stockage dédié et autres ressources qui sont maintenues séparées à partir du service Logic Apps public ou « global ». Cette séparation réduit également l’impact que d’autres locataires Azure peuvent avoir sur les performances de vos applications. Votre environnement ISE est *injecté* dans votre réseau virtuel Azure, qui déploie ensuite le service Logic Apps dans votre réseau virtuel. Quand vous créez une application logique ou un compte d’intégration, sélectionnez cet environnement ISE comme emplacement. Votre application logique ou votre compte d’intégration peut ensuite accéder directement à des ressources, comme des machines virtuelles, des serveurs, des systèmes et des services, dans votre réseau virtuel.

![Sélection d’un environnement de service d’intégration](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Cet article vous explique comment effectuer ces tâches :

* Vérifiez que tous les ports nécessaires sur un réseau virtuel sont ouverts afin que le trafic peut transiter de votre environnement de service d’intégration (ISE) entre les sous-réseaux dans ce réseau virtuel.

* Créez votre environnement de service d’intégration.

* Créez une application logique qui peut s’exécuter dans votre environnement de service d’intégration.

* Créez un compte d’intégration pour vos applications logiques dans votre environnement de service d’intégration.

Pour plus d’informations sur les environnements de service d’intégration, consultez [Accéder à des ressources Réseau virtuel Azure à partir d’Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Conditions préalables

* Un abonnement Azure. Si vous n’avez pas d’abonnement Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscrivez-vous pour bénéficier d’un compte Azure gratuit</a>.

  > [!IMPORTANT]
  > Logic apps, les déclencheurs intégrés, les actions intégrées et les connecteurs qui s’exécutent dans votre utilisation ISE un plan de tarification différents à partir du plan de tarification basé sur la consommation. Pour plus d’informations, consultez [Tarifs Logic Apps](../logic-apps/logic-apps-pricing.md).

* Un [réseau virtuel Azure](../virtual-network/virtual-networks-overview.md). Si vous n’avez pas de réseau virtuel, découvrez comment [créer un réseau virtuel Azure](../virtual-network/quick-create-portal.md). 

  * Votre réseau virtuel doit avoir quatre *vide* sous-réseaux pour le déploiement et la création de ressources dans votre ISE. Vous pouvez créer ces sous-réseaux à l’avance, ou vous pouvez attendre jusqu'à ce que vous créez votre ISE où vous pouvez créer des sous-réseaux en même temps. En savoir plus sur [exigences du sous-réseau](#create-subnet). 
  
    > [!NOTE]
    > Si vous utilisez [ExpressRoute](../expressroute/expressroute-introduction.md), qui fournit une connexion privée aux services cloud Microsoft, vous devez [créer une table de routage](../virtual-network/manage-route-table.md) qui possède ce qui suit acheminer et lier cette table avec chaque sous-réseau utilisé par votre ISE :
    > 
    > **Nom**: <*nom d’itinéraire*><br>
    > **Préfixe d’adresse**: 0.0.0.0/0<br>
    > **Tronçon suivant** : Internet

  * Assurez-vous que votre réseau virtuel [rend ces ports disponibles](#ports) afin que votre ISE fonctionne correctement et reste accessible.

* Si vous souhaitez utiliser des serveurs DNS personnalisés pour votre réseau virtuel Azure, [configurer ces serveurs en suivant ces étapes](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) avant de déployer votre ISE à votre réseau virtuel. Sinon, chaque fois que vous modifiez votre serveur DNS, vous devez également redémarrer votre ISE, une fonctionnalité disponible avec la préversion publique de l'environnement.

* Des connaissances de base en [création d’applications logiques](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="check-network-ports"></a>Vérifiez les ports réseau

Lorsque vous utilisez un environnement de service d’intégration (ISE) avec un réseau virtuel, un problème courant du programme d’installation rencontre un ou plusieurs ports bloqués. Les connecteurs que vous utilisez pour créer des connexions entre votre ISE et le système de destination peuvent également avoir leurs propres exigences de port. Par exemple, si vous communiquez avec un système FTP en utilisant le connecteur FTP, assurez-vous que le port que vous utilisez sur ce système FTP, comme le port 21 pour l’envoi de commandes, est disponible.

Pour contrôler le trafic entre les sous-réseaux du réseau virtuel dans lequel vous déployez ISE, vous pouvez configurer [groupes de sécurité réseau](../virtual-network/security-overview.md) par [filtrage du trafic réseau entre les sous-réseaux](../virtual-network/tutorial-filter-network-traffic.md). Toutefois, votre ISE doit spécifiques ports sont ouverts sur le réseau virtuel qui utilise des groupes de sécurité réseau. De cette façon, votre ISE reste accessible et puisse fonctionner correctement, afin que vous ne perdiez l’accès à votre environnement ISE. Sinon, si tous les ports requis ne sont pas disponibles, votre ISE cesse de fonctionner.

Ces tableaux décrivent les ports du réseau virtuel que votre ISE utilise et où ces ports sont utilisés. Le [balises de service Gestionnaire de ressources](../virtual-network/security-overview.md#service-tags) représente un groupe de préfixes d’adresses IP qui aident à réduire la complexité lors de la création de règles de sécurité.

> [!IMPORTANT]
> Pour la communication interne à l’intérieur de vos sous-réseaux, ISE nécessite que vous ouvrez tous les ports au sein de ces sous-réseaux.

| Objectif | Direction | Ports | Étiquette du service source | Étiquette du service de destination | Notes |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Communication depuis Azure Logic Apps | Règle de trafic sortant | 80 & 443 | VirtualNetwork | Internet | Le port dépend du service externe avec lequel communique le service Logic Apps |
| Azure Active Directory | Règle de trafic sortant | 80 & 443 | VirtualNetwork | AzureActiveDirectory | |
| Dépendance du Stockage Azure | Règle de trafic sortant | 80 & 443 | VirtualNetwork | Stockage | |
| Communication intersubnet | Trafic entrant et sortant | 80 & 443 | VirtualNetwork | VirtualNetwork | Pour la communication entre les sous-réseaux |
| Communication vers Azure Logic Apps | Trafic entrant | 443 | Internet  | VirtualNetwork | L’adresse IP de l’ordinateur ou un service qui appelle n’importe quel déclencheur de requête ou un webhook qui existe dans votre application logique. Fermeture ou de blocage de ce port empêche les appels HTTP vers logic apps avec déclencheurs de demande.  |
| Exécution de l’historique de l’application logique | Trafic entrant | 443 | Internet  | VirtualNetwork | L’adresse IP de l’ordinateur à partir duquel vous affichez l’application logique de l’historique d’exécution. Bien que la fermeture ou de blocage de ce port ne vous empêche pas d’afficher l’historique d’exécution, vous ne pouvez pas afficher les entrées et sorties de chaque étape que l’historique d’exécution. |
| Gestion des connexions | Règle de trafic sortant | 443 | VirtualNetwork  | Internet | |
| Publier des journaux de diagnostic et métriques | Règle de trafic sortant | 443 | VirtualNetwork  | AzureMonitor | |
| Communication à partir d’Azure Traffic Manager | Trafic entrant | 443 | AzureTrafficManager | VirtualNetwork | |
| Concepteur Logic Apps - Propriétés dynamiques | Trafic entrant | 454 | Internet  | VirtualNetwork | Les demandes proviennent d’applications logique [point de terminaison d’accès entrant des adresses IP dans cette région](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Dépendance de gestion App Service | Trafic entrant | 454 & 455 | AppServiceManagement | VirtualNetwork | |
| Déploiement du connecteur | Trafic entrant | 454 & 3443 | Internet  | VirtualNetwork | Nécessaire pour le déploiement et la mise à jour des connecteurs. Fermeture ou de blocage de ce port entraîne des déploiements ISE à échouer et empêche les correctifs ou mises à jour du connecteur. |
| Dépendance SQL Azure | Règle de trafic sortant | 1433 | VirtualNetwork | SQL |
| Azure Resource Health | Règle de trafic sortant | 1886 | VirtualNetwork | Internet | Pour la publication de l’état d’intégrité dans Resource Health |
| Gestion des API - Point de terminaison de gestion | Trafic entrant | 3443 | APIManagement  | VirtualNetwork | |
| Dépendance du journal pour la stratégie Event Hub et l’agent de surveillance | Règle de trafic sortant | 5672 | VirtualNetwork  | EventHub | |
| Accès aux instances du Cache Azure pour Redis entre instances de rôle | Trafic entrant <br>Règle de trafic sortant | 6379-6383 | VirtualNetwork  | VirtualNetwork | En outre, pour ISE travailler avec le Cache Azure pour Redis, vous devez ouvrir ces [décrites dans le Cache de Azure pour Redis Forum aux questions sur les ports entrants et sortants](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Trafic entrant | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Créer votre environnement de service d’intégration

Pour créer votre environnement de service d’intégration, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com), dans le menu principal d’Azure, sélectionnez **Créer une ressource**.
Dans la zone de recherche, entrez « environnement de service d’intégration » comme filtre.

   ![Créer une ressource](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Dans le volet de la création d’environnement de Service d’intégration, choisissez **créer**.

   ![Choisissez « Créer ».](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Spécifiez ces informations pour votre environnement, puis choisissez **Vérifier + créer**, par exemple :

   ![Spécifier les informations pour l’environnement](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propriété | Obligatoire | Value | Description  |
   |----------|----------|-------|-------------|
   | **Abonnement** | Oui | <*Azure-subscription-name*> | Abonnement Azure à utiliser pour votre environnement |
   | **Groupe de ressources** | Oui | <*nom-groupe-de-ressources-Azure*> | Groupe de ressources Azure où vous voulez créer votre environnement |
   | **Nom de l’environnement de service d’intégration** | Oui | <*nom-environnement*> | Nom à donner à votre environnement |
   | **Lieu** | Oui | <*région-centre de données-Azure*> | Région du centre de données Azure où déployer votre environnement |
   | **Capacité supplémentaire** | Oui | 0 à 10 | Le nombre d’unités de traitement supplémentaires à utiliser pour cette ressource ISE. Pour ajouter capacité après sa création, consultez [capacité d’ajouter des ISE](#add-capacity). |
   | **Réseau virtuel** | Oui | <*Azure-virtual-network-name*> | Réseau virtuel Azure où vous voulez injecter votre environnement, pour que les applications logiques de cet environnement puissent accéder à votre réseau virtuel. Si vous n’avez pas un réseau, [créez d’abord un réseau virtuel Azure](../virtual-network/quick-create-portal.md). <p>**Important !** Vous pouvez effectuer cette injection *seulement*  quand vous créez votre ISE. |
   | **Sous-réseaux** | Oui | <*subnet-resource-list*> | Un environnement ISE nécessite quatre sous-réseaux *vides* pour la création des ressources dans votre environnement. Pour créer chaque sous-réseau, [suivez les étapes décrites dans ce tableau](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Créer un sous-réseau**

   Pour créer des ressources dans votre environnement, votre ISE a besoin de quatre *vide* sous-réseaux qui ne sont pas délégués à n’importe quel service. 
   Vous *ne peut pas* modifier ces adresses de sous-réseau après avoir créé votre environnement. Chaque sous-réseau doit répondre aux critères suivants :

   * A un nom qui commence par un caractère alphabétique ou un trait de soulignement et n’a pas ces caractères : `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * Utilise le [Interdomain routage format CIDR (Classless)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) et un espace d’adressage de classe B.

   * Utilise au moins un `/27` dans l’adresse de l’espace, car chaque sous-réseau doit avoir 32 adresses comme la *minimale*. Exemple :

     * `10.0.0.0/27` a 32 adresses car 2<sup>(32-27)</sup> est 2<sup>5</sup> ou 32.

     * `10.0.0.0/24` a 256 adresses car 2<sup>(32-24)</sup> est 2<sup>8</sup> ou 256.

     * `10.0.0.0/28` a 16 adresses seulement et est trop petite, car 2<sup>(32-28)</sup> est 2<sup>4</sup> ou 16.

     Pour en savoir plus sur le calcul des adresses, consultez [blocs CIDR de IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Si vous utilisez [ExpressRoute](../expressroute/expressroute-introduction.md), n’oubliez pas de [créer une table de routage](../virtual-network/manage-route-table.md) qui possède ce qui suit acheminer et lier cette table avec chaque sous-réseau utilisé par votre ISE :

     **Nom**: <*nom d’itinéraire*><br>
     **Préfixe d’adresse**: 0.0.0.0/0<br>
     **Tronçon suivant** : Internet

   1. Sous la liste **Sous-réseaux**, choisissez **Gérer la configuration du sous-réseau**.

      ![Gérer la configuration du sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. Dans le volet **Sous-réseaux**, choisissez **Sous-réseau**.

      ![Ajouter un sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. Dans le volet **Ajouter un sous-réseau**, spécifiez ces informations.

      * **Nom** : Nom de votre sous-réseau
      * **Plage d’adresses (bloc CIDR)** : Plage de votre sous-réseau dans votre réseau virtuel, au format CIDR

      ![Détails de l’ajout d’un sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Une fois que vous avez terminé, sélectionnez **OK**.

   1. Répétez ces étapes pour trois autres sous-réseaux.

      > [!NOTE]
      > Si les sous-réseaux que vous tentez de créer ne sont pas valides, le portail Azure affiche un message, mais ne bloque pas votre progression.

1. Une fois qu’Azure a validé les informations de votre ISE, choisissez **Créer**, par exemple :

   ![Après la validation, choisissez « Créer »](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure commence le déploiement de votre environnement, mais ce processus *peut prendre* jusqu’à deux heures. 
   Pour vérifier l’état du déploiement, dans votre barre d’outils Azure, choisissez l’icône Notifications, qui ouvre le volet Notifications.

   ![Vérifier l’état du déploiement](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Un fois le déploiement terminé, Azure montre cette notification :

   ![Déploiement réussi](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   Sinon, suivez les instructions pour le portail Azure pour la résolution des problèmes de déploiement.

   > [!NOTE]
   > Si le déploiement échoue ou si vous supprimez votre ISE, Azure peut prendre jusqu'à une heure avant de libérer vos sous-réseaux. Ce délai signifie signifie que vous devrez peut-être patienter avant de réutiliser ces sous-réseaux dans un autre ISE. 
   >
   > Si vous supprimez votre réseau virtuel, Azure prend généralement jusqu'à deux heures avant de libérer de vos sous-réseaux, mais cette opération peut prendre plus longtemps. 
   > Lors de la suppression de réseaux virtuels, assurez-vous qu’aucune ressource n’est toujours connectés. Consultez [supprimer le réseau virtuel](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Pour voir votre environnement, choisissez **Accéder à la ressource** si Azure n’accède pas automatiquement à votre environnement une fois le déploiement terminé.  

Pour plus d’informations sur la création de sous-réseaux, consultez [ajouter un sous-réseau de réseau virtuel](../virtual-network/virtual-network-manage-subnet.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Créer une application logique - Environnement de service d’intégration

Pour créer des applications logiques qui s’exécutent dans votre environnement de service d’intégration (ISE), [créer vos applications logiques à l’accoutumée](../logic-apps/quickstart-create-first-logic-app-workflow.md) , sauf lorsque vous définissez la **emplacement** propriété, sélectionnez votre ISE à partir de la  **Environnements de service d’intégration** section, par exemple :

  ![Sélection d’un environnement de service d’intégration](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

Pour connaître les différences dans comment les déclencheurs et actions travail et comment ils sont étiquetés lorsque vous utilisez une ISE par rapport au service global Logic Apps, consultez [isolé par rapport à global dans la vue d’ensemble de l’ISE](connect-virtual-network-vnet-isolated-environment-overview.md#difference).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Créer un compte d’intégration - Environnement de service d’intégration

Si vous souhaitez utiliser un compte d’intégration avec logic apps dans un environnement de service d’intégration (ISE), ce compte d’intégration doit utiliser le *même environnement* en tant que les applications logiques. Les applications logiques dans un environnement de service d’intégration peuvent référencer seulement des comptes d’intégration de ce même environnement.

Pour créer un compte d’intégration qui utilise une fenêtre ISE, [créer votre compte d’intégration de la façon habituelle](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , sauf lorsque vous définissez la **emplacement** propriété, sélectionnez votre ISE à partir de la **intégration environnements de service** section, par exemple :

![Sélection d’un environnement de service d’intégration](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>Ajouter de la capacité de l’ISE

Votre unité de base ISE a résolu la capacité, si vous avez besoin de davantage de débit, vous pouvez donc ajouter des unités d’échelle. Vous pouvez l’échelle automatique basée sur les mesures de performances ou sur un nombre d’unités de traitement supplémentaire. Si vous choisissez la mise à l’échelle en fonction de mesures, vous pouvez choisir à partir de différents critères et spécifier les conditions de seuil pour répondre à ces critères.

1. Dans le portail Azure, recherchez votre ISE.

1. Pour consulter les mesures de performances et d’utilisation pour votre ISE, sur le menu principal de votre ISE, sélectionnez **vue d’ensemble**.

   ![Afficher l’utilisation pour ISE](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. Comment configurer la mise à l’échelle, sous **paramètres**, sélectionnez **monter en charge**. Sur le **configurer** , choisir **activer la mise à l’échelle**.

   ![Activer la mise à l’échelle](./media/connect-virtual-network-vnet-isolated-environment/scale-out.png)

1. Pour **nom du paramètre de mise à l’échelle**, fournissez un nom pour votre paramètre.

1. Dans le **par défaut** , choisissez soit **mise à l’échelle selon une mesure** ou **mise à l’échelle à un nombre d’instances spécifique**.

   * Si vous choisissez basée sur l’instance, entrez le nombre d’unités de traitement entre 0 et 10 (inclus).

   * Si vous choisissez basée sur une mesure, procédez comme suit :

     1. Dans le **règles** , choisissez **ajouter une règle**.

     1. Sur le **règle de mise à l’échelle** volet, configurer vos critères et l’action à prendre lorsque la règle se déclenche.

     1. Lorsque vous avez terminé, choisissez **ajouter**.

1. Lorsque vous avez terminé avec vos paramètres de mise à l’échelle, enregistrez vos modifications.

## <a name="next-steps"></a>Étapes suivantes

* En apprendre davantage sur le [Réseau virtuel Azure](../virtual-network/virtual-networks-overview.md)
* Découvrir l’[Intégration d’un réseau virtuel pour les services Azure](../virtual-network/virtual-network-for-azure-services.md)
