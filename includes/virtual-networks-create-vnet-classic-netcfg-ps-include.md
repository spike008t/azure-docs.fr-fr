---
title: Fichier Include
description: Fichier Include
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: bda289e73b9a782cd56c0c94b8f53e8002b1ccf4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66116842"
---
## <a name="how-to-create-a-virtual-network-using-a-network-config-file-from-powershell"></a>Création d’un réseau virtuel à l’aide d’un fichier de configuration réseau à partir de PowerShell
Azure utilise un fichier XML pour définir tous les réseaux virtuels disponibles pour un abonnement. Vous pouvez télécharger ce fichier, y apporter des changements pour modifier ou supprimer des réseaux virtuels existants, et en créer de nouveaux. Dans ce didacticiel, vous découvrez comment télécharger ce fichier, appelé fichier de configuration réseau (ou netcfg), et le modifier pour créer un réseau virtuel. Pour plus d’informations sur le fichier de configuration réseau, consultez la rubrique [Schéma de configuration du réseau virtuel Azure](https://msdn.microsoft.com/library/azure/jj157100.aspx).

Pour créer un réseau virtuel avec un fichier netcfg à l’aide de PowerShell, procédez comme suit :

1. Si vous n’avez jamais utilisé Azure PowerShell, procédez de la manière décrite dans l’article [Installer et configurer Azure PowerShell](/powershell/azureps-cmdlets-docs) puis connectez-vous à Azure et sélectionnez votre abonnement.
2. Dans la console Azure PowerShell, utilisez la cmdlet **Get-AzureVnetConfig** pour télécharger le fichier de configuration réseau dans un répertoire de votre ordinateur en exécutant la commande ci-dessous : 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Sortie attendue :
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Ouvrez le fichier que vous avez enregistré à l’étape 2 à l’aide de n’importe quelle application de l’éditeur XML ou texte, puis recherchez le  **\<VirtualNetworkSites >** élément. Si vous avez déjà créé des réseaux, chacun est affiché comme son propre  **\<VirtualNetworkSite >** élément.
4. Pour créer le réseau virtuel décrit dans ce scénario, ajoutez le code XML suivant juste sous le  **\<VirtualNetworkSites >** élément :

   ```xml
         <?xml version="1.0" encoding="utf-8"?>
         <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
           <VirtualNetworkConfiguration>
             <VirtualNetworkSites>
                 <VirtualNetworkSite name="TestVNet" Location="East US">
                   <AddressSpace>
                     <AddressPrefix>192.168.0.0/16</AddressPrefix>
                   </AddressSpace>
                   <Subnets>
                     <Subnet name="FrontEnd">
                       <AddressPrefix>192.168.1.0/24</AddressPrefix>
                     </Subnet>
                     <Subnet name="BackEnd">
                       <AddressPrefix>192.168.2.0/24</AddressPrefix>
                     </Subnet>
                   </Subnets>
                 </VirtualNetworkSite>
             </VirtualNetworkSites>
           </VirtualNetworkConfiguration>
         </NetworkConfiguration>
   ```
   
5. Enregistrez le fichier de configuration réseau.
6. Dans la console Azure PowerShell, utilisez la cmdlet **Set-AzureVnetConfig** pour charger le fichier de configuration réseau en exécutant la commande ci-dessous : 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Sortie retournée :
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Si **OperationStatus** n’a pas la valeur *Succeeded* dans les résultats retournés, vérifiez les éventuelles erreurs dans le fichier XML et effectuez à nouveau l’étape 6.

7. Dans la console Azure PowerShell, utilisez la cmdlet **Get-AzureVnetSite** pour vérifier que le nouveau réseau a été ajouté en exécutant la commande ci-dessous : 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   La sortie (abrégée) retournée contient le texte suivant :
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
