---
title: Tutoriel - Router le trafic pour améliorer la réponse d’un site web à l’aide d’Azure Traffic Manager
description: Cet article de didacticiel explique comment créer un profil Traffic Manager pour développer un site web très réactif.
services: traffic-manager
author: asudbring
Customer intent: As an IT Admin, I want to route traffic so I can improve website response by choosing the endpoint with lowest latency.
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2018
ms.author: allensu
ms.openlocfilehash: 304beeae02da5836ba88a56d7166fc681e263501
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258353"
---
# <a name="tutorial-improve-website-response-using-traffic-manager"></a>Didacticiel : Améliorer la réponse d’un site web à l’aide de Traffic Manager

Ce didacticiel explique comment utiliser Traffic Manager pour créer un site web très réactif en dirigeant le trafic utilisateur vers le site web avec la latence la plus faible. En règle générale, le centre de données dont la latence est la plus faible est géographiquement le plus proche.

Ce tutoriel vous montre comment effectuer les opérations suivantes :

> [!div class="checklist"]
> * Créer deux machines virtuelles exécutant un site web de base sur IIS
> * Créer deux machines virtuelles de test pour afficher Traffic Manager en action
> * Configurer le nom DNS des machines virtuelles exécutant IIS
> * Créer un profil Traffic Manager pour de meilleures performances de site web
> * Ajouter des points de terminaison de machine virtuelle dans le profil Traffic Manager
> * Afficher Traffic Manager en action

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites"></a>Conditions préalables

Pour afficher Traffic Manager en action, ce didacticiel requiert que vous déployiez les éléments suivants :

- deux instances de sites Web de base en cours d’exécution dans différentes régions Azure - **est des États-Unis** et **Europe de l’ouest**.
- deux machines virtuelles de test pour le test de Traffic Manager - une machine virtuelle dans **est des États-Unis** et la deuxième machine virtuelle dans **Europe de l’ouest**. Les machines virtuelles de test permettent d’illustrer la façon dont Traffic Manager achemine le trafic utilisateur vers le site web qui est exécuté dans la même région, car il fournit la latence la plus faible.

### <a name="sign-in-to-azure"></a>Connexion à Azure

Connectez-vous au portail Azure sur https://portal.azure.com.

### <a name="create-websites"></a>Créer des sites web

Dans cette section, vous allez créer deux instances de site web qui fournissent les deux points de terminaison de service pour le profil Traffic Manager dans deux régions Azure. La création des deux sites web comprend les étapes suivantes :

1. La création de deux machines virtuelles pour l’exécution d’un site web de base, l’une située dans la région **USA Est** et l’autre dans la région **Europe Ouest**.
2. L’installation d’un serveur IIS sur chaque machine virtuelle, et la mise à jour de la page de site web par défaut qui décrit le nom de la machine virtuelle à laquelle un utilisateur est connecté lorsqu’il visite le site web.

#### <a name="create-vms-for-running-websites"></a>Créer des machines virtuelles pour exécuter des sites web

Dans cette section, vous allez créer deux machines virtuelles *myIISVMEastUS* et *myIISVMWestEurope* dans le **est des États-Unis** et **Europe de l’ouest** régions Azure.

1. Dans le coin supérieur, à gauche du portail Azure, sélectionnez **créer une ressource** > **calcul** > **Windows Server Datacenter 2019**.
2. Dans **Créer une machine virtuelle**, tapez ou sélectionnez les valeurs suivantes sous l’onglet **De base** :

   - **Abonnement** > **Groupe de ressources** : Sélectionnez **créer** , puis tapez **myResourceGroupTM1**.
   - **Détails de l’instance** > **Nom de la machine virtuelle** : Type *myIISVMEastUS*.
   - **Détails de l’instance** > **région**:  Sélectionnez **USA Est**.
   - **Compte d’administrateur** > **nom d’utilisateur**:  Entrez un nom d’utilisateur de votre choix.
   - **Compte d’administrateur** > **mot de passe**:  Entrez un mot de passe de votre choix. Le mot de passe doit contenir au moins 12 caractères et satisfaire aux [exigences de complexité définies](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).
   - **Les règles de Port de trafic entrant** > **ports d’entrée publics**: Sélectionnez **Autoriser les ports sélectionnés**.
   - **Les règles de Port de trafic entrant** > **sélectionner des ports entrants**: Sélectionnez **RDP** et **HTTP** dans la zone déroulante.

3. Sélectionnez le **gestion** onglet, ou sélectionnez **suivant : Disques**, puis **Suivant : Mise en réseau**, puis **suivant : Gestion**. Sous **Supervision**, définissez **Diagnostics de démarrage** sur **Désactivé**.
4. Sélectionnez **Revoir + créer**.
5. Passez en revue les paramètres, puis cliquez sur **créer**.  
6. Suivez les étapes pour créer une deuxième machine virtuelle nommée *myIISVMWestEurope*, avec un **groupe de ressources** nom de *myResourceGroupTM2*, un **emplacement**de *Europe de l’ouest*et tous les autres paramètres identique *myIISVMEastUS*.
7. La création des machines virtuelles peut prendre plusieurs minutes. Attendez que les deux machines virtuelles aient été créées avant de passer aux étapes restantes.

   ![Créer une machine virtuelle](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>Installer IIS et personnaliser la page web par défaut

Dans cette section, vous installez le serveur IIS sur les deux machines virtuelles *myIISVMEastUS* et *myIISVMWestEurope*, puis mettez à jour la page de site Web par défaut. La page de site web personnalisée affiche le nom de la machine virtuelle à laquelle vous êtes connecté lorsque vous visitez le site web à partir d’un navigateur web.

1. Sélectionnez **Toutes les ressources** dans le menu de gauche, puis, dans la liste de ressources, cliquez sur *myIISVMEastUS* qui se trouve dans le groupe de ressources *myResourceGroupTM1*.
2. Dans la page **Vue d’ensemble**, cliquez sur **Se connecter**, puis sur **Se connecter à la machine virtuelle**, et sélectionnez **Télécharger le fichier RDP**.
3. Ouvrez le fichier .rdp téléchargé. Si vous y êtes invité, sélectionnez **Connexion**. Entrez le nom d’utilisateur et le mot de passe spécifiés lors de la création de la machine virtuelle. Vous devrez peut-être sélectionner **Plus de choix**, puis **Utiliser un autre compte**, pour spécifier les informations d’identification que vous avez entrées lorsque vous avez créé la machine virtuelle.
4. Sélectionnez **OK**.
5. Un avertissement de certificat peut s’afficher pendant le processus de connexion. Si vous recevez l’avertissement, sélectionnez **Oui** ou **Continuer** pour poursuivre le processus de connexion.
6. Sur le bureau du serveur, accédez à **Outils d’administration Windows**>**Gestionnaire de serveur**.
7. Lancez Windows PowerShell sur VM1 et utilisez les commandes suivantes pour installer le serveur IIS et mettre à jour le fichier htm par défaut.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![Installer IIS et personnaliser la page web](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)
8. Fermez la connexion RDP établie avec la machine virtuelle *myIISVMEastUS*.
9. Répétez les étapes 1-8 avec en créant une connexion RDP avec la machine virtuelle *myIISVMWestEurope* au sein de la *myResourceGroupTM2* groupe de ressources pour installer IIS et personnaliser sa page de web par défaut.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configurer les noms DNS des machines virtuelles exécutant IIS

Traffic Manager achemine le trafic utilisateur en fonction du nom DNS des points de terminaison de service. Dans cette section, vous configurez les noms DNS pour les serveurs IIS - *myIISVMEastUS* et *myIISVMWestEurope*.

1. Cliquez sur **Toutes les ressources** dans le menu de gauche, puis, dans la liste de ressources, sélectionnez *myIISVMEastUS* qui se trouve dans le groupe de ressources *myResourceGroupTM1*.
2. Dans la page **Vue d’ensemble**, sous **Nom DNS**, sélectionnez **Configurer**.
3. Dans la page **Configuration**, sous l’étiquette du nom DNS, ajoutez un nom unique, puis sélectionnez **Enregistrer**.
4. Répétez les étapes 1 à 3, pour la machine virtuelle nommée *myIISVMWestEurope* qui se trouve dans le *myResourceGroupTM2* groupe de ressources.

### <a name="create-test-vms"></a>Créer des machines virtuelles de test

Dans cette section, vous allez créer une machine virtuelle (*myVMEastUS* et *myVMWestEurope*) dans chaque région Azure (**est des États-Unis** et **Europe de l’ouest**). Vous allez utiliser ces machines virtuelles pour tester comment Traffic Manager achemine le trafic vers le serveur IIS le plus proche lorsque vous accédez au site web.

1. Dans le coin supérieur, à gauche du portail Azure, sélectionnez **créer une ressource** > **calcul** > **Windows Server Datacenter 2019**.
2. Dans **Créer une machine virtuelle**, tapez ou sélectionnez les valeurs suivantes sous l’onglet **De base** :

   - **Abonnement** > **Groupe de ressources** : Sélectionnez **myResourceGroupTM1**.
   - **Détails de l’instance** > **Nom de la machine virtuelle** : Type *myVMEastUS*.
   - **Détails de l’instance** > **région**:  Sélectionnez **USA Est**.
   - **Compte d’administrateur** > **nom d’utilisateur**:  Entrez un nom d’utilisateur de votre choix.
   - **Compte d’administrateur** > **mot de passe**:  Entrez un mot de passe de votre choix. Le mot de passe doit contenir au moins 12 caractères et satisfaire aux [exigences de complexité définies](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).
   - **Les règles de Port de trafic entrant** > **ports d’entrée publics**: Sélectionnez **Autoriser les ports sélectionnés**.
   - **Les règles de Port de trafic entrant** > **sélectionner des ports entrants**: Sélectionnez **RDP** dans la zone déroulante.

3. Sélectionnez le **gestion** onglet, ou sélectionnez **suivant : Disques**, puis **Suivant : Mise en réseau**, puis **suivant : Gestion**. Sous **Supervision**, définissez **Diagnostics de démarrage** sur **Désactivé**.
4. Sélectionnez **Revoir + créer**.
5. Passez en revue les paramètres, puis cliquez sur **créer**.  
6. Suivez les étapes pour créer une deuxième machine virtuelle nommée *myVMWestEurope*, avec un **groupe de ressources** nom de *myResourceGroupTM2*, un **emplacement** de *Europe de l’ouest*et tous les autres paramètres identique *myVMEastUS*.
7. La création des machines virtuelles peut prendre plusieurs minutes. Attendez que les deux machines virtuelles aient été créées avant de passer aux étapes restantes.

## <a name="create-a-traffic-manager-profile"></a>Créer un profil Traffic Manager

Créez un profil Traffic Manager qui dirige le trafic utilisateur en l’envoyant vers le point de terminaison dont la latence est la plus faible.

1. Dans l’angle supérieur gauche de l’écran, cliquez sur **Créer une ressource** > **Mise en réseau** > **Profil Traffic Manager** > **Créer**.
2. Dans **Créer un profil Traffic Manager**, entrez ou sélectionnez les informations suivantes, acceptez les valeurs par défaut pour les autres paramètres, puis choisissez **Créer** :

    | Paramètre                 | Valeur                                              |
    | ---                     | ---                                                |
    | Nom                   | Ce nom doit être unique au sein de la zone trafficmanager.net et affiche le nom DNS, trafficmanager.net, qui est utilisé pour accéder à votre profil Traffic Manager.                                   |
    | Méthode de routage          | Sélectionnez la méthode de routage **Performances**.                                       |
    | Abonnement            | Sélectionnez votre abonnement.                          |
    | Groupe de ressources          | Sélectionnez le groupe de ressources *myResourceGroupTM1*. |
    | Lieu                | Sélectionnez **USA Est**. Ce paramètre fait référence à l’emplacement du groupe de ressources et n’a pas d’impact sur le profil Traffic Manager qui sera déployé globalement.                              |
    |

    ![Créer un profil Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Ajouter des points de terminaison Traffic Manager

Ajoutez les deux machines virtuelles en cours d’exécution IIS serveurs - *myIISVMEastUS* & *myIISVMWestEurope* pour acheminer le trafic d’utilisateur au point de terminaison le plus proche à l’utilisateur.

1. Dans la barre de recherche du portail, recherchez le nom du profil Traffic Manager que vous avez créé dans la section précédente et sélectionnez le profil dans les résultats affichés.
2. Dans **Profil Traffic Manager**, dans la section **Paramètres**, cliquez sur **Points de terminaison**, puis sur **Ajouter**.
3. Entrez ou sélectionnez les informations suivantes, acceptez les valeurs par défaut pour les autres paramètres, puis cliquez sur **OK** :

    | Paramètre                 | Valeur                                              |
    | ---                     | ---                                                |
    | type                    | Point de terminaison Azure                                   |
    | Nom           | myEastUSEndpoint                                        |
    | Type de ressource cible           | Adresse IP publique                          |
    | Ressource cible          | Sélectionnez **Choisir une adresse IP publique** pour afficher la liste des ressources pourvues d’adresses IP publiques dans le même abonnement. Dans **Ressource**, sélectionnez l’adresse IP publique nommée *myIISVMEastUS-ip*. Il s’agit de l’adresse IP publique de la machine virtuelle serveur IIS qui se trouve dans la région USA Est.|
    |        |           |

4. Répétez les étapes 2 et 3 pour ajouter un autre point de terminaison nommé *myWestEuropeEndpoint* pour l’adresse IP publique *myIISVMWestEurope-ip* qui est associé à la machine virtuelle nommée du serveur IIS  *myIISVMWestEurope*.
5. Lorsque l’ajout de deux points de terminaison est terminé, ceux-ci s’affichent dans **Profil Traffic Manager** avec leur état de surveillance défini sur **En ligne**.

    ![Ajouter un point de terminaison Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-endpoint.png)

## <a name="test-traffic-manager-profile"></a>Tester le profil Traffic Manager

Dans cette section, vous allez tester comment Traffic Manager achemine le trafic utilisateur vers la machine virtuelle exécutant le site web la plus proche afin de fournir la latence minimale. Pour afficher Traffic Manager en action, procédez comme suit :

1. Déterminez le nom DNS de votre profil Traffic Manager.
2. Affichez Traffic Manager en action comme suit :
    - À partir de la machine virtuelle de test (*myVMEastUS*) qui se trouve dans la région **USA Est**, dans un navigateur web, accédez au nom DNS de votre profil Traffic Manager.
    - À partir de la machine virtuelle de test (*myVMWestEurope*) qui se trouve dans le **Europe de l’ouest** région, dans un navigateur web, recherchez le nom DNS de votre profil Traffic Manager.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Déterminer le nom DNS du profil Traffic Manager

Dans ce didacticiel, par souci de simplicité, vous utilisez le nom DNS du profil Traffic Manager pour visiter les sites web.

Vous pouvez déterminer le nom DNS du profil Traffic Manager en procédant comme suit :

1. Dans la barre de recherche du portail, recherchez le nom du **profil Traffic Manager** que vous avez créé dans la section précédente. Dans les résultats affichés, cliquez sur le profil Traffic Manager.
2. Cliquez sur **Overview**.
3. Le **profil Traffic Manager** affiche le nom DNS de votre profil Traffic Manager nouvellement créé. Dans les déploiements en environnements de production, vous configurez un nom de domaine personnalisé pour qu’il pointe vers le nom de domaine Traffic Manager, à l’aide d’un enregistrement DNS CNAME.

   ![Nom DNS Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Afficher Traffic Manager en action

Dans cette section, vous pouvez voir Traffic Manager en action.

1. Sélectionnez **Toutes les ressources** dans le menu de gauche, puis, dans la liste de ressources, cliquez sur *myVMEastUS* qui se trouve dans le groupe de ressources *myResourceGroupTM1*.
2. Dans la page **Vue d’ensemble**, cliquez sur **Se connecter**, puis sur **Se connecter à la machine virtuelle**, et sélectionnez **Télécharger le fichier RDP**.
3. Ouvrez le fichier .rdp téléchargé. Si vous y êtes invité, sélectionnez **Connexion**. Entrez le nom d’utilisateur et le mot de passe spécifiés lors de la création de la machine virtuelle. Vous devrez peut-être sélectionner **Plus de choix**, puis **Utiliser un autre compte**, pour spécifier les informations d’identification que vous avez entrées lorsque vous avez créé la machine virtuelle.
4. Sélectionnez **OK**.
5. Un avertissement de certificat peut s’afficher pendant le processus de connexion. Si vous recevez l’avertissement, sélectionnez **Oui** ou **Continuer** pour poursuivre le processus de connexion.
1. Dans un navigateur web sur la machine virtuelle *myVMEastUS*, tapez le nom DNS de votre profil Traffic Manager pour afficher votre site web. Comme la machine virtuelle se trouve dans la région **USA Est**, vous êtes dirigé vers le site web hébergé sur le serveur IIS *myIISVMEastUS* le plus proche qui se trouve dans la région **USA Est**.

   ![Tester le profil Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

2. Connectez-vous à la machine virtuelle *myVMWestEurope* située dans la région **Europe Ouest** en suivant les étapes 1 à 5, puis accédez au nom de domaine du profil Traffic Manager à partir de cette machine virtuelle. Étant donné que la machine virtuelle se trouve dans **Europe de l’ouest**, vous êtes désormais redirigé vers le site Web hébergé sur le plus proche du serveur IIS *myIISVMWestEurope* qui se trouve dans **Europe de l’ouest**.

   ![Tester le profil Traffic Manager](./media/tutorial-traffic-manager-improve-website-response/westeurope-traffic-manager-test.png)

## <a name="delete-the-traffic-manager-profile"></a>Supprimer le profil Traffic Manager

Lorsque vous n’en avez plus besoin, supprimez les groupes de ressources (**ResourceGroupTM1** et **ResourceGroupTM2**). Pour ce faire, sélectionnez le groupe de ressources (**ResourceGroupTM1** ou **ResourceGroupTM2**), puis **Supprimer**.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Répartir le trafic entre un ensemble de points de terminaison](traffic-manager-configure-weighted-routing-method.md)
