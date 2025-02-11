---
title: Renouveler un certificat Azure Application Gateway
description: Découvrez comment renouveler un certificat associé à un écouteur de passerelle Application Gateway.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
origin.date: 08/15/2018
ms.date: 03/12/2019
ms.author: v-junlch
ms.openlocfilehash: 90200f7be6c71346441922365fc4439111dd8701
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66133694"
---
# <a name="renew-application-gateway-certificates"></a>Renouveler des certificats Application Gateway

À un moment donné, vous devrez renouveler vos certificats si vous avez configuré votre passerelle Application Gateway pour le chiffrement SSL.

Vous pouvez renouveler un certificat associé à un écouteur à l’aide du Portail Azure, d’Azure PowerShell ou d’Azure CLI :

## <a name="azure-portal"></a>Portail Azure

Pour renouveler un certificat d’écouteur à partir du portail, accédez à vos écouteurs Application Gateway. Cliquez sur l’écouteur dont vous devez renouveler le certificat, puis cliquez sur **Renew or edit selected certificate** (Renouveler ou modifier le certificat sélectionné).

![Renouvellement de certificat](./media/renew-certificate/ssl-cert.png)

Chargez votre nouveau certificat PFX, attribuez-lui un nom, tapez le mot de passe, puis cliquez sur **Enregistrer**.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Pour renouveler votre certificat à l’aide d’Azure PowerShell, utilisez le script suivant :

```azurepowershell
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli
az network application-gateway ssl-cert update `
  -n "<CertName>" `
  --gateway-name "<AppGatewayName>" `
  -g "ResourceGroupName>" `
  --cert-file <PathToCerFile> `
  --cert-password "<password>"
```

## <a name="next-steps"></a>Étapes suivantes

Pour découvrir comment configurer le déchargement SSL avec la passerelle Azure Application Gateway, consultez la [configuration du déchargement SSL](application-gateway-ssl-portal.md)

<!-- Update_Description: code update -->