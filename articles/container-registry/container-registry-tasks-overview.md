---
title: Automatiser les mises à jour correctives du système d’exploitation et du framework avec Azure Container Registry Tasks (ACR Tasks)
description: Introduction à ACR Tasks, une suite de fonctionnalités d’Azure Container Registry qui permet la création et la mise à jour corrective d’images de conteneur sécurisées et automatisées dans le cloud.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 05/20/2019
ms.author: danlep
ms.openlocfilehash: cc182743c3879ab2748f92022437bc23c26c371c
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65977196"
---
# <a name="automate-os-and-framework-patching-with-acr-tasks"></a>Automatiser les mises à jour correctives du système d’exploitation et du framework avec ACR Tasks

Les conteneurs fournissent de nouveaux niveaux de virtualisation en isolant les dépendances d’application et de développement des spécifications opérationnelles et d’infrastructure. Ce qui demeure, toutefois, est la nécessité de gérer les mises à jour correctives de cette virtualisation d’application.

## <a name="what-is-acr-tasks"></a>Qu’est-ce qu’ACR Tasks ?

**ACR Tasks** est une suite de fonctionnalités d’Azure Container Registry. Elle fournit la création d’image conteneur informatique pour Linux, Windows et ARM, et peut automatiser [la mise à jour corrective du framework et du système d’exploitation](#automate-os-and-framework-patching) pour vos conteneurs Docker. Non seulement ACR Tasks étend votre cycle de développement « en double interne » au cloud avec des builds d’image de conteneur à la demande, mais il permet également de créer des builds automatisées lors de la validation du code source ou lors de la mise à jour de l’image de base d’un conteneur. Avec les déclencheurs de mise à jour d’image de base, vous pouvez automatiser votre flux de travail de mise à jour corrective du système d’exploitation et du framework d’application tout en garantissant la sécurité de l’environnement et en respectant les critères fondamentaux des conteneurs immuables.

Générer et tester des images de conteneur avec ACR Tasks de quatre manières :

* [Tâche rapide](#quick-task) : générer et envoyer les images de conteneur à la demande dans Azure, sans avoir besoin d’une installation locale de Docker Engine. Pensez `docker build`, `docker push` dans le cloud. Générez un build à partir du code source local ou d’un référentiel Git.
* [Génération lors de la validation du code source](#automatic-build-on-source-code-commit) : déclenchez automatiquement une génération d’image de conteneur lors de la validation du code dans un référentiel Git.
* [Génération lors de la mise à jour d'images de base](#automate-os-and-framework-patching) : déclenchez une image de conteneur lors de la mise à jour de l’image de base de cette image.
* [Tâches multi-étapes](#multi-step-tasks): définissez les tâches à plusieurs étapes qui génèrent des images, exécutent des conteneurs comme des commandes et envoient des images à un registre. Cette fonctionnalité d’opérations push et de build d’image parallèles, de test et de l’exécution des tâches de la demande ACR tâches prend en charge.

## <a name="quick-task"></a>Tâche rapide

Le cycle de développement « en boucle interne », c’est-à-dire le processus itératif d’écriture de code, de génération de build et de test de votre application avant de valider le code source, correspond au début de la gestion du cycle de vie des conteneurs.

Avant de valider votre première ligne de code, les fonctionnalité [tâche rapide](container-registry-tutorial-quick-task.md) d’ACR Tasks peut fournir un environnement de développement intégré en déchargeant vos builds d’image de conteneur dans Azure. Avec les tâches rapides, vous pouvez vérifier vos définitions de build automatisées et identifier les problèmes potentiels avant de valider votre code.

En utilisant le format `docker build` bien connu, la commande [az acr build][az-acr-build] dans Azure CLI prend un *contexte* (ensemble de fichiers à générer), l’envoie à ACR Tasks et, par défaut, envoie (push) l’image générée à son registre lors de son achèvement.

Pour une introduction, consultez le Guide de démarrage rapide pour [générer et exécuter une image de conteneur](container-registry-quickstart-task-cli.md) dans Azure Container Registry.  

Le tableau suivant présente quelques exemples d’emplacements de contexte pris en charge pour ACR Tasks :

| Emplacement du contexte | Description  | Exemples |
| ---------------- | ----------- | ------- |
| Système de fichiers local | Fichiers dans un répertoire sur le système de fichiers local. | `/home/user/projects/myapp` |
| Branche principale GitHub | Fichiers dans la branche maître (ou autre branche par défaut) d’un référentiel GitHub.  | `https://github.com/gituser/myapp-repo.git` |
| Branche GitHub | Branche spécifique d’un référentiel GitHub.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub PR | Demande de tirage dans un référentiel GitHub. | `https://github.com/gituser/myapp-repo.git#pull/23/head` |
| Sous-dossier de GitHub | Fichiers dans un sous-dossier d’un référentiel GitHub. L’exemple affiche la combinaison de spécifications de PR et de sous-dossier. | `https://github.com/gituser/myapp-repo.git#pull/24/head:myfolder` |
| Tarball distant | Fichiers dans une archive compressée sur un serveur Web à distance. | `http://remoteserver/myapp.tar.gz` |

ACR Tasks est conçu comme un précurseur du cycle de vie de conteneurs. Par exemple, intégrez ACR Tasks dans votre solution CI/CD. En exécutant [az login][az-login] avec un [principal de service][az-login-service-principal], votre solution CI/CD pourra alors émettre des commandes [az acr build][az-acr-build] pour lancer des générations d’image.

Découvrez comment utiliser les tâches rapides dans le premier didacticiel d’ACR Tasks, [Générer des images de conteneur dans le cloud avec Azure Container Registry Tasks](container-registry-tutorial-quick-task.md).

## <a name="automatic-build-on-source-code-commit"></a>Génération automatique lors de la validation du code source

Utilisez ACR Tasks pour déclencher automatiquement une génération d’image de conteneur lors de la validation du code dans un référentiel Git. Les tâches de génération, configurables avec la commande Azure CLI [az acr task][az-acr-task], vous permettent de spécifier un référentiel Git et éventuellement une branche et un Dockerfile. Lorsque votre équipe valide du code dans le référentiel, un webhook créé par ACR Tasks déclenche la génération de l’image de conteneur définie dans le référentiel.

> [!IMPORTANT]
> Si vous avez créé précédemment des tâches pendant la préversion avec la commande `az acr build-task`, ces tâches doivent être recréées à l’aide de la commande [az acr task][az-acr-task].

Découvrez comment déclencher la génération de builds lors de la validation du code source dans le deuxième didacticiel d’ACR Tasks, [Automatiser les générations d’image de conteneur avec Azure Container Registry Tasks](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatiser les mises à jour correctives du système d’exploitation et du framework

L’aptitude d’ACR Tasks à améliorer réellement votre pipeline de génération de conteneur provient de sa capacité à détecter la mise à jour d’une image de base. Quand l’image de base mise à jour est envoyée à votre registre, ACR Tasks peut générer automatiquement des images d’application basées sur elle.

Les images de conteneur appartiennent en gros à deux catégories : les images de *base* et les images d’*application*. En règle générale, vos images de base incluent les frameworks d’application et de système d’exploitation sur lesquels votre application est générée, ainsi que d’autres personnalisations. Ces images de base sont elles-mêmes généralement basées sur des images publiques en amont, par exemple : [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet] ou [Node.js][base-node]. Plusieurs de vos images d’application peuvent partager une image de base commune.

Quand une image de framework d’application ou de système d’exploitation est mise à jour par le chargé de maintenance en amont, par exemple avec un correctif de sécurité critique du système d’exploitation, vous devez également mettre à jour vos images de base pour qu’elles contiennent ce correctif critique. Chaque image d’application doit ensuite être également recréée pour inclure ces correctifs en amont maintenant inclus dans votre image de base.

Comme ACR Tasks détecte de manière dynamique les dépendances de l’image de base quand il génère une image de conteneur, il peut savoir quand l’image de base d’une image d’application est mise à jour. Avec une [tâche de build](container-registry-tutorial-base-image-update.md#create-a-task) préconfigurée, ACR Tasks **regénère automatiquement chaque image d’application** pour vous. Grâce à ces détection et regénération automatiques, ACR Tasks vous permet d’économiser le temps et les efforts normalement nécessaires au suivi et à la mise à jour manuels de chaque image d’application faisant référence à votre image de base mise à jour.

Pour en savoir plus sur la mise à jour corrective du système d’exploitation et du framework, consultez le troisième didacticiel d’ACR Tasks, [Automatiser la génération des images en fonction de la mise à jour d’une image de base avec Azure Container Registry Tasks](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Les mises à jour de l’image de base ne déclenchent des générations que lorsque les images de base et d’application se trouvent dans le même registre de conteneurs Azure ou lorsque l’image de base réside dans un référentiel Docker Hub public.

## <a name="multi-step-tasks"></a>Tâches multiétapes

Tâches multi-étapes fournissent la définition de tâche basée sur une étape et de l’exécution de la conception, test et de mise à jour corrective des images de conteneur dans le cloud. Les étapes de la tâche définissent les opérations build et push d’une image de conteneur individuelle. Elles permettent également de définir l’exécution d’un ou plusieurs conteneurs, en utilisant à chaque étape le conteneur comme son environnement d’exécution.

Par exemple, vous pouvez créer une tâche à plusieurs étapes qui automatise les opérations suivantes :

1. Générer (build) une image de l’application web
1. Exécuter le conteneur d’application web
1. Compiler une image test d’application web
1. Exécuter le conteneur de test de l’application web qui effectue des tests sur le conteneur d’application en cours d’exécution
1. Si les tests réussissent, compilez un package d’archivage du graphique Helm
1. Exécutez la commande `helm upgrade` à l’aide du nouveau package d’archivage du graphique Helm

Les tâches à plusieurs étapes vous permettent de fractionner la génération, l’exécution et le test d’une image en étapes plus composables, avec prise en charge des dépendances entre chacune d’elles. Grâce aux tâches à plusieurs étapes d’ACR Tasks, vous contrôlez plus finement la génération d’images, le test la mise à jour corrective du système d’exploitation et du framework.

Pour en savoir plus sur les tâches à plusieurs étapes, consultez [Run multi-step build, test, and patch tasks in ACR Tasks (Exécuter des tâches à plusieurs étapes de génération, de test et de correction dans ACR Tasks)](container-registry-tasks-multi-step.md).

## <a name="view-task-logs"></a>Afficher les journaux de tâche

Chaque exécution de la tâche génère une sortie de journal que vous pouvez inspecter pour déterminer si les étapes de la tâche a été exécuté avec succès. Si vous utilisez le [build d’acr az](/cli/azure/acr#az-acr-build), [acr az exécuter](/cli/azure/acr#az-acr-run), ou [exécution de la tâche az acr](/cli/azure/acr/task#az-acr-task-run) pour déclencher la tâche de commande, la sortie de journal pour l’exécution de la tâche est diffusée vers la console et également stocké pour une utilisation ultérieure récupération. Afficher les journaux pour une tâche exécutent dans le portail Azure, ou utilisent le [journaux des tâches az acr](/cli/azure/acr/task#az-acr-task-logs) commande.

À compter de juillet 2019, données et journaux pour la tâche s’exécute dans un Registre seront conservées par défaut pendant 30 jours et vider automatiquement. Si vous souhaitez archiver les données pour exécuter une tâche, activer l’archivage à l’aide de la [az acr update-exécution de la tâche](/cli/azure/acr/task#az-acr-task-update-run) commande. L’exemple suivant active l’archivage pour l’exécution de la tâche *cf11* dans le Registre *myregistry*.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>Étapes suivantes

Lorsque vous êtes prêt à automatiser le système d’exploitation et la mise à jour corrective du framework en créant vos images de conteneur dans le cloud, Découvrez les trois parties [série de didacticiels ACR tâches](container-registry-tutorial-quick-task.md).

Si vous le souhaitez, vous pouvez installer l’[extension Docker pour Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) et l’extension [Compte Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) pour utiliser vos registres de conteneurs Azure. Dans Visual Studio Code, vous pouvez tirer (pull) et envoyer (push) des images vers un registre de conteneurs Azure, et exécuter ACR Tasks.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task]: /cli/azure/acr
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
