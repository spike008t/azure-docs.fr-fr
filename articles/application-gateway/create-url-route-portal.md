---
title: Tutoriel - Créer une passerelle d’application avec des règles d’acheminement par chemin d’accès URL - Portail Azure
description: Dans ce tutoriel, découvrez comment créer des règles d’acheminement par chemin d’accès URL pour une passerelle d’application et un groupe de machines virtuelles identiques avec le portail Azure.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/18/2019
ms.author: victorh
ms.openlocfilehash: 5307f7674635fd33241e1faba9bb0b7c0432d10b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66134880"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Didacticiel : Créer une passerelle d’application avec des règles d’acheminement par chemin d’accès à l’aide du portail Azure

Vous pouvez utiliser le portail Azure pour configurer des [règles d’acheminement par chemin d’accès URL](url-route-overview.md) lors de la création d’une [passerelle d’application](overview.md). Ce didacticiel montre comment créer des pools principaux à l’aide de machines virtuelles. Il permet ensuite de définir des règles d’acheminement qui font en sorte que le trafic web arrive sur les serveurs voulus dans les pools.

Ce tutoriel vous montre comment effectuer les opérations suivantes :

> [!div class="checklist"]
> * Créer une passerelle Application Gateway
> * Créer des machines virtuelles pour les serveurs principaux
> * Créer des pools principaux avec les serveurs principaux
> * Créer un écouteur principal
> * Créer une règle d’acheminement par chemin d’accès

![Exemple d’acheminement d’URL](./media/create-url-route-portal/scenario.png)

Si vous préférez, vous pouvez suivre ce tutoriel en utilisant [Azure CLI](tutorial-url-route-cli.md) ou [Azure PowerShell](tutorial-url-route-powershell.md).

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="sign-in-to-azure"></a>Connexion à Azure

Connectez-vous au portail Azure sur [https://portal.azure.com](https://portal.azure.com).

## <a name="create-an-application-gateway"></a>Créer une passerelle Application Gateway

Un réseau virtuel est nécessaire pour la communication entre les ressources que vous créez. Deux sous-réseaux sont créés dans cet exemple : une pour la passerelle d’application et l’autre pour les serveurs principaux. Vous pouvez créer un réseau virtuel en même temps que la passerelle d’application.

1. Sélectionnez **Nouveau** dans le coin supérieur gauche du portail Azure.
2. Sélectionnez **Mise en réseau**, puis sélectionnez **Application Gateway** dans la liste de suggestions.
3. Entrez ces valeurs pour la passerelle d’application :

   - *myAppGateway* : pour le nom de la passerelle d’application.
   - *myResourceGroupAG* : pour le nouveau groupe de ressources.

     ![Créer une nouvelle passerelle d’application](./media/create-url-route-portal/application-gateway-create.png)

4. Acceptez les valeurs par défaut des autres paramètres, puis sélectionnez **OK**.
5. Sélectionnez **Choisir un réseau virtuel**, **Créer nouveau**, puis entrez ces valeurs pour le réseau virtuel :

   - *myVNet* : pour le nom du réseau virtuel.
   - *10.0.0.0/16* : pour l’espace d’adressage du réseau virtuel.
   - *myAGSubnet* : pour le nom du sous-réseau.
   - *10.0.0.0/24* : pour l’espace d’adressage du sous-réseau.

     ![Création d’un réseau virtuel](./media/create-url-route-portal/application-gateway-vnet.png)

6. Sélectionnez **OK** pour créer le réseau virtuel et le sous-réseau.
7. Sélectionnez **Choisir une adresse IP publique**, **Créer nouveau**, puis entrez le nom de l’adresse IP publique. Dans cet exemple, l’adresse IP publique est nommée *myAGPublicIPAddress*. Acceptez les valeurs par défaut des autres paramètres, puis sélectionnez **OK**.
8. Acceptez les valeurs par défaut pour la configuration de l’écouteur, laissez le pare-feu d’applications web désactivé, puis sélectionnez **OK**.
9. Passez en revue les paramètres sur la page de résumé, puis cliquez sur **OK** pour créer les ressources réseau et la passerelle d’application. La création de la passerelle d’application peut prendre plusieurs minutes. Patientez jusqu’à ce que le déploiement soit terminé avant de passer à la section suivante.

### <a name="add-a-subnet"></a>Ajouter un sous-réseau

1. Sélectionnez **Toutes les ressources** dans le menu de gauche, puis cliquez sur **myVNet** dans la liste des ressources.
2. Sélectionnez **Sous-réseaux**, puis **Sous-réseau**.

    ![Créer un sous-réseau](./media/create-url-route-portal/application-gateway-subnet.png)

3. Entrez *myBackendSubnet* pour le nom du sous-réseau, puis sélectionnez **OK**.

## <a name="create-virtual-machines"></a>Créer des machines virtuelles

Dans cet exemple, vous créez trois machines virtuelles à utiliser en tant que serveurs principaux pour la passerelle d’application. Vous allez aussi installer IIS sur les machines virtuelles pour vérifier que la passerelle d’application a bien été créée.

1. Sélectionnez **Nouveau**.
2. Cliquez sur **Compute**, puis sélectionnez **Windows Server 2016 Datacenter** dans la liste de suggestions.
3. Entrez ces valeurs pour la machine virtuelle :

    - *myVM1* : pour le nom de la machine virtuelle.
    - *azureuser* : pour le nom d’utilisateur administrateur.
    - *Azure123456!* pour le mot de passe.
    - Sélectionnez **Utiliser l’existant**, puis *myResourceGroupAG*.

4. Sélectionnez **OK**.
5. Sélectionnez **DS1_V2** pour la taille de la machine virtuelle, puis **Sélectionner**.
6. Assurez-vous que **myVNet** est sélectionné pour le réseau virtuel et que le sous-réseau est **myBackendSubnet**. 
7. Sélectionnez **Désactivé** pour désactiver les diagnostics de démarrage.
8. Sélectionnez **OK**, vérifiez les paramètres sur la page de résumé, puis sélectionnez **Créer**.

### <a name="install-iis"></a>Installer IIS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Ouvrez l’interpréteur de commandes interactif et assurez-vous qu’il est défini sur **PowerShell**.

    ![Installer l’extension personnalisée](./media/create-url-route-portal/application-gateway-extension.png)

2. Exécutez la commande suivante pour installer IIS sur la machine virtuelle : 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. Créez deux autres machines virtuelles et installez IIS à l’aide de la procédure que vous venez de terminer. Entrez les noms *myVM2* et *myVM3* pour les noms et les valeurs de VMName dans Set-AzVMExtension.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Créer des pools principaux avec les machines virtuelles

1. Sélectionnez **Toutes les ressources**, puis **myAppGateway**.
2. Sélectionnez **Pools principaux**. Un pool par défaut a été automatiquement créé avec la passerelle d’application. Sélectionnez **appGatewayBackendPool**.
3. Sélectionnez **Ajouter une cible** pour ajouter *myVM1* à appGatewayBackendPool.

    ![Ajouter des serveurs principaux](./media/create-url-route-portal/application-gateway-backend.png)

4. Sélectionnez **Enregistrer**.
5. Sélectionnez **Pools principaux**, puis **Ajouter**.
6. Entrez le nom *imagesBackendPool* et ajoutez *myVM2* à l’aide de **Ajouter une cible**.
7. Sélectionnez **OK**.
8. Sélectionnez à nouveau **Ajouter** pour ajouter un autre pool principal avec le nom *videoBackendPool* et ajoutez-y *myVM3*.

## <a name="create-a-backend-listener"></a>Créer un écouteur principal

1. Sélectionnez **Écouteurs**, puis **De base**.
2. Entrez *myBackendListener* pour le nom, *myFrontendPort* pour le nom du port frontal, puis *8080* pour le port de l’écouteur.
3. Sélectionnez **OK**.

## <a name="create-a-path-based-routing-rule"></a>Créer une règle d’acheminement par chemin d’accès

1. Sélectionnez **Règles** , puis **Basé sur le chemin**.
2. Entrez *rule2* pour le nom.
3. Entrez *Images* pour le nom du premier chemin. Entrez */images/*\* pour le chemin d’accès. Sélectionnez **imagesBackendPool** pour le pool principal.
4. Entrez *Video* pour le nom du second chemin. Entrez */video/*\* pour le chemin d’accès. Sélectionnez **videoBackendPool** pour le pool principal.

    ![Créer une règle basée sur le chemin](./media/create-url-route-portal/application-gateway-route-rule.png)

5. Sélectionnez **OK**.

## <a name="test-the-application-gateway"></a>Tester la passerelle d’application

1. Sélectionnez **Toutes les ressources**, puis **myAGPublicIPAddress**.

    ![Enregistrer l’adresse IP publique de la passerelle d’application](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Copiez l’adresse IP publique, puis collez-la dans la barre d’adresses de votre navigateur. Par exemple, http://40.121.222.19.

    ![Tester l’URL de base dans la passerelle d’application](./media/create-url-route-portal/application-gateway-iistest.png)

3. Modifiez l’URL http://&lt;ip-address&gt;:8080/images/test.htm, en remplaçant &lt;ip-address&gt; avec votre adresse IP. Vous devriez voir quelque chose ressemblant à ceci :

    ![Tester l’URL images dans la passerelle d’application](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. Modifiez l’URL http://&lt;ip-address&gt;:8080/video/test.htm, en remplaçant &lt;ip-address&gt; avec votre adresse IP. Vous devriez voir quelque chose ressemblant à ceci :

    ![Tester l’URL vidéo dans la passerelle d’application](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Supprimer des ressources

Quand vous n’avez plus besoin des ressources que vous avez créées avec la passerelle d’application, supprimez le groupe de ressources. En supprimant le groupe de ressources, vous supprimez aussi la passerelle d’application et toutes ses ressources associées. 

Pour supprimer le groupe de ressources :

1. Dans le menu de gauche du portail Azure, sélectionnez **Groupes de ressources**.
2. Dans la page **Groupes de ressources**, recherchez **myResourceGroupAG** dans la liste, puis sélectionnez ce groupe de ressources.
3. Dans la page **Groupe de ressources**, sélectionnez **Supprimer le groupe de ressources**.
4. Entrez *myResourceGroupAG* dans **TYPE THE RESOURCE GROUP NAME**, puis sélectionnez **Supprimer**.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [En savoir plus sur ce que vous pouvez faire avec Azure Application Gateway](application-gateway-introduction.md)
