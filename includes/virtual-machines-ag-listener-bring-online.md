---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 760bb5b62e9bba9b7a83f99760f7fe5d8c399dfb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165421"
---
1. Dans le Gestionnaire du cluster de basculement, développez les **Rôles**, puis mettez votre groupe de disponibilité en surbrillance.  

2. Dans l’onglet **Ressources**, cliquez avec le bouton droit sur le nom de l’écouteur, puis cliquez sur **Propriétés**.

3. Cliquez sur l'onglet **Dépendances** . Si plusieurs ressources sont répertoriées, vérifiez que les adresses IP ont des dépendances OR (et non des dépendances AND).  

4. Cliquez sur **OK**.

5. Cliquez avec le bouton droit sur le nom de l’écouteur, puis cliquez sur **Mettre en ligne**.

6. Lorsque l’écouteur est en ligne, dans l’onglet **Ressources**, cliquez avec le bouton droit sur le groupe de disponibilité, puis cliquez sur **Propriétés**.
   
    ![Configurer la ressource du groupe de disponibilité](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Créez une dépendance sur la ressource de nom de l’écouteur (pas le nom de ressources d’adresse IP), puis cliquez sur **OK**.
   
    ![Ajouter une dépendance pour le nom de l’écouteur](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. Démarrez SQL Server Management Studio, puis connectez-vous au réplica principal.

9. Accédez à **Haute disponibilité AlwaysOn** > **Groupes de disponibilité** > **\<AvailabilityGroupName\>** > **Écouteurs de groupe de disponibilité**.  
    Le nom de l’écouteur que vous avez créé dans le Gestionnaire du cluster de basculement doit être affiché.

10. Cliquez avec le bouton droit sur le nom de l’écouteur, puis cliquez sur **Propriétés**.

11. Dans la zone **Port**, spécifiez le numéro de port pour l’écouteur du groupe de disponibilité à l’aide de l’élément $EndpointPort utilisé précédemment (dans ce didacticiel, la valeur par défaut était 1433), puis cliquez sur **OK**.

