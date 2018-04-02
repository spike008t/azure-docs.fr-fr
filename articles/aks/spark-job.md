---
title: Exécuter une tâche Apache Spark avec Azure Container Service (AKS)
description: Utiliser Azure Container Service (AKS) pour exécuter une tâche Apache Spark
services: container-service
author: lenadroid
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/15/2018
ms.author: alehall
ms.custom: mvc
ms.openlocfilehash: 9d57f572ba159191f5b634b5ea604563ac2f7801
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/16/2018
---
# <a name="running-apache-spark-jobs-on-aks"></a>Exécution de tâches Apache Spark sur AKS

[Apache Spark][apache-spark] est un moteur rapide pour le traitement des données à grande échelle. À compter de la [version 2.3.0 de Spark][spark-latest-release], Apache Spark prend en charge l’intégration native avec des clusters Kubernetes. Azure Container Service (AKS) est un environnement Kubernetes géré s’exécutant dans Azure. Ce document décrit en détail la préparation et l’exécution de tâches Apache Spark sur un cluster Azure Container Service (AKS).

## <a name="prerequisites"></a>Prérequis


Pour effectuer les étapes de cet article, vous avez besoin des éléments suivants :

* Des connaissances de base sur Kubernetes et [Apache Spark][spark-quickstart].
* Un compte [Docker Hub][docker-hub] ou un [registre Azure Container Registry][acr-create].
* Azure CLI [installé][azure-cli] sur votre système de développement.
* [JDK 8][java-install] installé sur votre système.
* SBT ([Scala Build Tool][sbt-install]) installé sur votre système.
* L’installation des outils en ligne de commande Git sur votre système

## <a name="create-an-aks-cluster"></a>Créer un cluster AKS

Spark est utilisé pour le traitement des données à grande échelle et nécessite que les nœuds Kubernetes soient dimensionnés pour répondre aux exigences en matière de ressources Spark. Nous recommandons une taille minimale de `Standard_D3_v2` pour vos nœuds Azure Container Service (AKS).
 
Si vous avez besoin d’un cluster AKS qui respecte cette recommandation minimale, exécutez les commandes suivantes.

Créez un groupe de ressources pour le cluster.

```azurecli
az group create --name mySparkCluster --location eastus
```

Créez le cluster AKS avec des nœuds dont la taille est `Standard_D3_v2`.

```azurecli
az aks create --resource-group mySparkCluster --name mySparkCluster --node-vm-size Standard_D3_v2
```

Connectez-vous au cluster AKS.

```azurecli
az aks get-credentials --resource-group mySparkCluster --name mySparkCluster
```

Si vous utilisez un registre Azure Container Registry (ACR) pour stocker des images conteneur, configurez l’authentification entre AKS et ACR. Pour connaître les étapes à suivre, consultez la [documentation de l’authentification ACR][acr-aks].

## <a name="build-the-spark-source"></a>Générer la source Spark

Avant d’exécuter des travaux Spark sur un cluster AKS, vous devez générer le code source Spark et l’empaqueter dans une image conteneur. La source Spark inclut des scripts qui peuvent être utilisés pour exécuter ce processus. 

Clonez le dépôt de projet Spark sur votre système de développement.

```bash
git clone https://github.com/apache/spark
```

Passez au répertoire du dépôt cloné et enregistrez le chemin de la source Spark dans une variable.

```bash
cd spark
sparkdir=$(pwd)
```

Si plusieurs versions de JDK sont installées, définissez `JAVA_HOME` pour utiliser la version 8 pour la session active. 

```bash
export JAVA_HOME=`/usr/libexec/java_home -d 64 -v "1.8*"`
```

Exécutez la commande suivante pour générer le code source Spark avec prise en charge de Kubernetes.

```bash
./build/mvn -Pkubernetes -DskipTests clean package
```

La commande suivante crée les images conteneur Spark et le place à un registre d’image conteneur. Remplacez `registry.example.com` par le nom de votre registre de conteneurs. Si vous utilisez Docker Hub, cette valeur est le nom de Registre. Si vous utilisez Azure Container Registry (ACR), cette valeur est le nom de serveur de connexion ACR.

```bash
./bin/docker-image-tool.sh -r registry.example.com -t v1 build
```

Envoyez (push) l’image conteneur vers votre registre d’image conteneur.

```bash
./bin/docker-image-tool.sh -r registry.example.com -t v1 push
```

## <a name="prepare-a-spark-job"></a>Préparer un travail Spark

Préparez ensuite un travail Spark. Un fichier jar est utilisé pour contenir le travail Spark. Il est nécessaire lors de l’exécution de la commande `spark-submit`. Ce fichier jar peut être rendu accessible par le biais d’une URL publique ou être pré-intégrés dans une image conteneur. Dans cet exemple, un exemple de fichier jar est créé pour calculer la valeur de Pi. Ce fichier jar est ensuite chargé dans le stockage Azure. Si vous avez un fichier jar existant, n’hésitez pas à le remplacer.

Créez un répertoire dans lequel vous voulez créer le projet pour un travail Spark.

```bash
mkdir myprojects
cd myprojects
```

Créez un projet Scala à partir d’un modèle.

```bash
sbt new sbt/scala-seed.g8
```

Quand vous y êtes invité, entrez `SparkPi` pour le nom du projet.

```bash
name [Scala Seed Project]: SparkPi
```

Accédez au répertoire du projet nouvellement créé.

```bash
cd sparkpi
```

Exécutez les commandes suivantes pour ajouter un plug-in SBT, ce qui permet d’empaqueter le projet sous forme de fichier jar.

```bash
touch project/assembly.sbt
echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.6")' >> project/assembly.sbt
```

Exécutez ces commandes pour copier l’exemple de code dans le projet nouvellement créé et ajouter toutes les dépendances nécessaires.

```bash
EXAMPLESDIR="src/main/scala/org/apache/spark/examples"
mkdir -p $EXAMPLESDIR
cp $sparkdir/examples/$EXAMPLESDIR/SparkPi.scala $EXAMPLESDIR/SparkPi.scala

cat <<EOT >> build.sbt
// https://mvnrepository.com/artifact/org.apache.spark/spark-sql
libraryDependencies += "org.apache.spark" %% "spark-sql" % "2.3.0" % "provided"
EOT

sed -ie 's/scalaVersion.*/scalaVersion := "2.11.11",/' build.sbt
sed -ie 's/name.*/name := "SparkPi",/' build.sbt
```

Pour empaqueter le projet dans un fichier jar, exécutez la commande suivante.

```bash
sbt assembly
```

Une fois l’empaquetage terminé, une sortie similaire à ce qui suit doit s’afficher.

```bash
[info] Packaging /Users/me/myprojects/sparkpi/target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 10 s, completed Mar 6, 2018 11:07:54 AM
```

## <a name="copy-job-to-storage"></a>Travail le travail dans le stockage

Créez un compte de stockage et un conteneur Azure pour stocker le fichier jar.

```azurecli
RESOURCE_GROUP=sparkdemo
STORAGE_ACCT=sparkdemo$RANDOM
az group create --name $RESOURCE_GROUP --location eastus
az storage account create --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT --sku Standard_LRS
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT -o tsv`
```

Chargez le fichier jar dans le compte de stockage Azure avec les commandes suivantes.

```bash
CONTAINER_NAME=jars
BLOB_NAME=SparkPi-assembly-0.1.0-SNAPSHOT.jar
FILE_TO_UPLOAD=target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar

echo "Creating the container..."
az storage container create --name $CONTAINER_NAME
az storage container set-permission --name $CONTAINER_NAME --public-access blob

echo "Uploading the file..."
az storage blob upload --container-name $CONTAINER_NAME --file $FILE_TO_UPLOAD --name $BLOB_NAME

jarUrl=$(az storage blob url --container-name $CONTAINER_NAME --name $BLOB_NAME | tr -d '"')
```

La variable `jarUrl` contient maintenant le chemin d’accès accessible publiquement du fichier jar.

## <a name="submit-a-spark-job"></a>Envoyer un travail Spark

Avant d’envoyer le travail Spark, vous avez besoin de l’adresse du serveur d’API Kubernetes. Utilisez la commande `kubectl cluster-info` pour obtenir cette adresse.

Découvrez l’URL où le serveur d’API Kubernetes s’exécute.

```bash
kubectl cluster-info
```

Notez l’adresse et le port.

```bash
Kubernetes master is running at https://<your api server>:443
```

Revenez à la racine du dépôt Spark.

```bash
cd $sparkdir
```

Envoyez le travail à l’aide de `spark-submit`. 

Remplacez la valeur `<kubernetes-api-server>` par votre adresse de serveur d’API et votre port. Remplacez `<spark-image>` par le nom de votre image conteneur au format `<your container registry name>/spark:<tag>`.

```bash
./bin/spark-submit \
  --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
  --deploy-mode cluster \
  --name spark-pi \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.executor.instances=3 \
  --conf spark.kubernetes.container.image=<spark-image> \
  $jarUrl
```

Cette opération démarre le travail Spark, ce qui achemine l’état du travail vers votre session shell. Pendant que le travail s’exécute, vous pouvez voir un pod de pilotes et des pods d’exécuteurs Spark à l’aide de la commande kubectl get pods. Ouvrez une seconde session de terminal pour exécuter ces commandes.

```console
$ kubectl get pods

NAME                                               READY     STATUS     RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   1/1       Running    0          16s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-1   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-2   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-3   0/1       Init:0/1   0          4s
```

Pendant que le travail s’exécute, vous pouvez également accéder à l’interface utilisateur Spark. Dans la deuxième session de terminal, utilisez la commande `kubectl port-forward` pour fournir un accès à l’interface utilisateur Spark.

```bash
kubectl port-forward spark-pi-2232778d0f663768ab27edc35cb73040-driver 4040:4040
```

Pour accéder à l’interface utilisateur Spark, ouvrez l’adresse `127.0.0.1:4040` dans un navigateur.

![Interface utilisateur Spark](media/aks-spark-job/spark-ui.png)

## <a name="get-job-results-and-logs"></a>Obtenir les journaux et les résultats du travail

Une fois le travail terminé, le pod de pilotes a l’état « Completed ». Obtenez le nom du pod avec la commande suivante.

```bash
kubectl get pods --show-all
```

Sortie :

```bash
NAME                                               READY     STATUS      RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   0/1       Completed   0          1m
```

Utilisez la commande `kubectl logs` pour obtenir des journaux à partir du pod de pilotes Spark. Remplacez le nom du pod par le nom de votre pod de pilotes.

```bash
kubectl logs spark-pi-2232778d0f663768ab27edc35cb73040-driver
```

Dans ces journaux, vous pouvez voir le résultat du travail Spark, qui est la valeur de Pi.

```bash
Pi is roughly 3.152155760778804
```

## <a name="package-jar-with-container-image"></a>Empaqueter un fichier jar avec une image conteneur

Dans l’exemple ci-dessus, le fichier jar Spark a été chargé dans le stockage Azure. L’autre possibilité consiste à empaqueter le fichier jar dans des images Docker personnalisées.

Pour ce faire, recherchez le `dockerfile` de l’image Spark située dans le répertoire `$sparkdir/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/`. Ajoutez une instruction `ADD` pour le travail Spark `jar` quelque part entre les déclarations`WORKDIR` et `ENTRYPOINT`.

Mettez à jour le chemin du fichier jar en définissant l’emplacement du fichier `SparkPi-assembly-0.1.0-SNAPSHOT.jar` sur votre système de développement. Vous pouvez également utiliser votre propre fichier jar.

```bash
WORKDIR /opt/spark/work-dir

ADD /path/to/SparkPi-assembly-0.1.0-SNAPSHOT.jar SparkPi-assembly-0.1.0-SNAPSHOT.jar

ENTRYPOINT [ "/opt/entrypoint.sh" ]
```

Générez et envoyez (push) l’image avec les scripts Spark inclus.

```bash
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> build
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> push
```

Lors de l’exécution du travail, au lieu d’indiquer une URL de fichier jar distante, le schéma `local://` peut être utilisé avec le chemin du fichier jar dans l’image Docker.

```bash
./bin/spark-submit \
    --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=3 \
    --conf spark.kubernetes.container.image=<spark-image> \
    local:///opt/spark/work-dir/<your-jar-name>.jar
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus de détails, consultez la documentation Spark.

> [!div class="nextstepaction"]
> [Documentation Spark][spark-docs]

<!-- LINKS - external -->
[apache-spark]: https://spark.apache.org/
[docker-hub]: https://docs.docker.com/docker-hub/
[java-install]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[sbt-install]: https://www.scala-sbt.org/1.0/docs/Setup.html
[spark-docs]: https://spark.apache.org/docs/latest/running-on-kubernetes.html
[spark-latest-release]: https://spark.apache.org/releases/spark-release-2-3-0.html
[spark-quickstart]: https://spark.apache.org/docs/latest/quick-start.html


<!-- LINKS - internal -->
[acr-aks]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-auth-aks
[acr-create]: https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli
[aks-quickstart]: https://docs.microsoft.com/en-us/azure/aks/
[azure-cli]: https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest
[storage-account]: https://docs.microsoft.com/en-us/azure/storage/common/storage-azure-cli