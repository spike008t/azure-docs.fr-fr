---
title: Fichier Include
description: Fichier Include
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/29/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 291ec651061b7a8a3ea3c0645a6bd6581d529ef6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244999"
---
## <a name="sign-in-to-azure"></a>Connexion à Azure 

Connectez-vous au portail Azure sur https://portal.azure.com.

> [!NOTE]
> Si vous inscrit pour utiliser des galeries d’images partagé pendant la préversion, vous devrez réinscrire le `Microsoft.Compute` fournisseur. Ouvrez [Cloud Shell](https://shell.azure.com/bash) et tapez : `az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>Créer une galerie d’images

Une galerie d’images est la ressource principale utilisée pour activer le partage d’image. Les caractères autorisés pour le nom de galerie sont les lettres majuscules ou minuscules, les chiffres et les points. Le nom de la galerie ne peut pas contenir de tirets.  Les noms de galerie doivent être uniques dans votre abonnement. 

L’exemple suivant crée une galerie nommée *myGallery* dans le groupe de ressources *myGalleryRG*.

1. Sélectionnez **Créer une ressource** dans le coin supérieur gauche du portail Azure.
1. Utilisez le type **Galerie d’images partagé** dans la zone de recherche, puis sélectionnez **Galerie d’images partagé** dans les résultats.
1. Dans le **Galerie d’images partagé** , cliquez sur **créer**.
1. Sélectionnez l’abonnement approprié.
1. Dans **groupe de ressources**, sélectionnez **créer** et type *myGalleryRG* pour le nom.
1. Dans **nom**, type *myGallery* pour le nom de la galerie.
1. Laissez la valeur par défaut pour **région**.
1. Vous pouvez taper une brève description de la galerie, par exemple *ma galerie d’images pour le test.* puis cliquez sur **révision + créer**.
1. Une fois la validation réussie, sélectionnez **créer**.
1. Lorsque le déploiement est terminé, sélectionnez **accéder à la ressource**.
   
## <a name="create-an-image-definition"></a>Créer une définition d’image 

Définitions d’image créer un regroupement logique des images. Ils sont utilisés pour gérer les informations sur les versions de l’image qui sont créés au sein de celles-ci. Noms de définition d’image peuvent être constitués de périodes, des chiffres, des points, des tirets et des lettres majuscules ou minuscules. Pour plus d’informations sur les valeurs que vous pouvez spécifier pour une définition de l’image, consultez [Image définitions](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

Créez la définition d’image de galerie à l’intérieur de votre galerie. Dans cet exemple, l’image de la galerie est nommé *myImageDefinition*.

1. Sur la page de votre galerie d’images, sélectionnez **ajouter une nouvelle définition de l’image** à partir du haut de la page. 
1. Pour **nom de la définition Image**, type *myImageDefinition*.
1. Pour **système d’exploitation**, sélectionnez l’option appropriée en fonction de votre image source.
1. Pour **Publisher**, type *myPublisher*. 
1. Pour **offrent**, type *myOffer*.
1. Pour **référence (SKU)** , type *mySKU*.
1. Lorsque vous avez terminé, sélectionnez **révision + créer**.
1. Une fois la définition de l’image est validé, sélectionnez **créer**.
1. Lorsque le déploiement est terminé, sélectionnez **accéder à la ressource**.


## <a name="create-an-image-version"></a>Créer une version d’image

Créer une version de l’image à partir d’une image managée. Dans cet exemple, la version d'image, *1.0.0*, elle est répliquée dans les deux centres de données *USA Centre-Ouest* et *USA Centre Sud*. Lors du choix des régions cibles pour la réplication, n’oubliez pas que vous devez également inclure le *source* région en tant que cible pour la réplication.

Les caractères autorisés pour la version d’image sont les nombres et les points. Les nombres doivent être un entier 32 bits. Format : *MajorVersion*.*MinorVersion*.*Patch*.

1. Dans la page de votre définition de l’image, sélectionnez **ajouter version** à partir du haut de la page.
1. Dans **région**, sélectionnez la région où se trouve votre image managée. Versions de l’image doivent être créés dans la même région que l’image managée qu’ils sont créés à partir de.
1. Pour **nom**, type *1.0.0*. Le nom de version d’image doit respecter *majeure*. *mineure*. *correctif* mettre en forme à l’aide des entiers. 
1. Dans **image Source**, sélectionnez votre image managée source à partir de la liste déroulante.
1. Dans **exclure du dernier**, conservez la valeur par défaut *non*.
1. Pour **vie date de fin de**, sélectionnez une date dans le calendrier qui est de quelques mois à l’avenir.
1. Dans **réplication**, laissez le **par défaut du nombre de réplicas** comme 1. Vous devez répliquer vers la région source, par conséquent, laissez le premier réplica en tant que la valeur par défaut et choisissez une deuxième région réplica soit *est des États-Unis*.
1. Lorsque vous avez terminé, sélectionnez **révision + créer**. Azure validera la configuration.
1. Lors de la version de l’image est validé, sélectionnez **créer**.
1. Lorsque le déploiement est terminé, sélectionnez **accéder à la ressource**.

Il peut prendre un certain temps pour répliquer l’image à l’ensemble des régions cible.

## <a name="share-the-gallery"></a>Partager la galerie

Nous recommandons que vous partagez l’accès au niveau de la galerie d’image. Les éléments suivants vous guide à travers le partage de la galerie que vous venez de créer.

1. Ouvrez le [portail Azure](https://portal.azure.com).
1. Dans le menu de gauche, sélectionnez **groupes de ressources**. 
1. Dans la liste des groupes de ressources, sélectionnez **myGalleryRG**. Le panneau de votre groupe de ressources s’ouvre.
1. Dans le menu à gauche de la **myGalleryRG** page, sélectionnez **contrôle d’accès (IAM)** . 
1. Sous **ajouter une attribution de rôle**, sélectionnez **ajouter**. Le **ajouter une attribution de rôle** volet s’ouvre. 
1. Sous **rôle**, sélectionnez **lecteur**.
1. Sous **attribuer l’accès à**, conservez la valeur par défaut **utilisateur, groupe ou principal du service Azure AD**.
1. Sous **sélectionnez**, entrez l’adresse de messagerie de la personne que vous souhaitez inviter.
1. Si l’utilisateur se trouve en dehors de votre organisation, vous verrez le message **cet utilisateur sera envoyé un e-mail qui leur permet de collaborer avec Microsoft.** Sélectionnez l’utilisateur avec l’adresse de messagerie, puis cliquez **enregistrer**.

Si l’utilisateur se trouve en dehors de votre organisation, ils obtiendront un message d’invitation à rejoindre l’organisation. L’utilisateur doit accepter l’invitation, puis ils seront en mesure de voir la galerie et toutes les définitions de l’image et les versions dans leur liste de ressources.

