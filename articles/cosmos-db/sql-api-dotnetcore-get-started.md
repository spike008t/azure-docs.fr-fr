---
title: 'Tutoriel : Créer une application .NET Core pour gérer les données stockées dans le compte d’API SQL pour Azure Cosmos DB'
description: Ce tutoriel crée une application de base de données en ligne et de console C# à l’aide du SDK .NET Core de l’API SQL pour Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/21/2019
ms.author: sngun
Customer intent: As a developer, I want to build a .NET Core application to access and manage Azure Cosmos DB resources so that customers can utilize the global distribution, elastic scaling, multi-master, and other capabilities that Azure Cosmos DB offers.
ms.openlocfilehash: fdd66a28ea38e7ae34ce70958ac57f29bc66ceb7
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969245"
---
# <a name="tutorial-build-a-net-core-app-to-manage-data-stored-in-a-sql-api-account"></a>Tutoriel : Créer une application .NET Core pour gérer les données stockées dans un compte d’API SQL

> [!div class="op_single_selector"]
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (préversion)](sql-api-dotnet-core-get-started-preview.md)
> * [.NET](sql-api-get-started.md)
> * [.NET (préversion)](sql-api-dotnet-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Java asynchrone](sql-api-async-java-get-started.md)
> * [Node.JS](sql-api-nodejs-get-started.md)
> 

En tant que développeur, vous pouvez avoir des applications qui utilisent des données de documents NoSQL. Vous pouvez utiliser le compte d’API SQL dans Azure Cosmos DB pour stocker et accéder à ces données de documents. Ce tutoriel vous montre comment créer une application .NET Core pour créer et interroger des données stockées dans le compte d'’API SQL d'Azure Cosmos DB. 

Ce tutoriel décrit les tâches suivantes :

> [!div class="checklist"]
> * Création et connexion à un compte Azure Cosmos
> * Configuration de votre solution Visual Studio
> * Création d’une base de données en ligne
> * Création d’une collection
> * Création de documents JSON
> * Exécution d’opérations CRUD sur les éléments, le conteneur et la base de données

Vous n’avez pas le temps de créer l’application ? La solution complète est disponible sur [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started). 

## <a name="prerequisites"></a>Prérequis

* Un compte Azure actif. Si vous n’en avez pas, vous pouvez vous inscrire pour bénéficier d’un [compte gratuit](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Téléchargez et utilisez gratuitement [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Si vous développez une application de plateforme Windows universelle (UWP), vous devez utiliser **Visual Studio 2017 avec la version 15.4** ou une version ultérieure. Veillez à activer **le développement Azure** lors de l’installation de Visual Studio.

    * Pour Mac OS ou Linux, vous pouvez développer des applications .NET Core à partir de la ligne de commande en installant le [SDK .NET Core](https://www.microsoft.com/net/core#macos) pour la plateforme de votre choix. 

    * Pour Windows, vous pouvez développer des applications .NET Core à partir de la ligne de commande en installant le [SDK .NET Core](https://www.microsoft.com/net/core#windows). 

## <a name="create-an-azure-cosmos-account"></a>Créer un compte Azure Cosmos

Procédez comme suit pour créer un compte Azure Cosmos :

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Configurer votre solution Visual Studio

1. Ouvrez **Visual Studio 2017** sur votre ordinateur.

2. Dans le menu **Fichier**, sélectionnez **Nouveau**, puis choisissez **Projet**.

3. Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Modèles** > **Visual C#** > **.NET Core** > **Application console (.NET Core)**, nommez votre projet **DocumentDBGettingStarted**, puis cliquez sur **OK**.

   ![Capture d’écran de la fenêtre Nouveau projet](./media/sql-api-dotnetcore-get-started/nosql-tutorial-new-project-2.png)

4. Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur **DocumentDBGettingStarted**.

5. Dans le même menu, sélectionnez **Gérer les packages NuGet**.

   ![Capture d'écran du menu du clic droit pour le projet](./media/sql-api-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)

6. Sous l’onglet **NuGet**, cliquez sur **Parcourir** en haut de la fenêtre, puis entrez **azure documentdb** dans la zone de recherche. Vérifiez que la case **Inclure la préversion** est cochée.

7. Dans les résultats, recherchez **Microsoft.Azure.DocumentDB.Core** et cliquez sur **Installer**.

   L’ID de package de la bibliothèque est [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Si vous ciblez une version de .NET Framework (net461, par exemple) qui n’est pas prise en charge par ce package NuGet .NET Core, utilisez [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) qui prend en charge toutes les versions de .NET Framework à partir de la version 4.5.

8. À l’invite, acceptez les installations de packages NuGet et le contrat de licence.

Maintenant que la configuration est terminée, nous pouvons commencer l’écriture du code. Vous trouverez le projet de code complet de ce tutoriel dans [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).

## <a id="Connect"></a>Se connecter à un compte Azure Cosmos DB

Se connecter à un compte Azure Cosmos en important les dépendances requises. Pour importer les dépendances, ajoutez le code suivant au début de votre fichier Program.cs :

```csharp
using System;

// ADD THIS PART TO YOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

Ajoutez ensuite ces deux constantes et votre variable *client* dans votre classe publique *Program*.

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

Ensuite, accédez au [Portail Azure](https://portal.azure.com) pour récupérer votre URI et votre clé primaire. L’URI et la clé primaire Azure Cosmos DB sont nécessaires pour que votre application sache où se connecter et qu’Azure Cosmos DB approuve la connexion de votre application.

Dans le portail Azure, accédez à votre compte Azure Cosmos, puis sélectionnez **Clés**.

Copiez l’URI à partir du portail et collez-le dans `<your endpoint URI>` dans le fichier program.cs. Ensuite, copiez la clé primaire à partir du portail, puis collez-la dans `<your key>`. Assurez-vous de supprimer le < et > , mais laissez les guillemets doubles autour de votre point de terminaison et de la clé.

![Obtenir des clés à partir du Portail Azure][keys]

Nous allons démarrer l’application de prise en main en créant une instance de **DocumentClient**.

Sous la méthode **Main**, ajoutez la nouvelle tâche asynchrone appelée **GetStartedDemo** qui va instancier notre nouveau **DocumentClient**.

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART TO YOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

Ajoutez le code suivant pour exécuter votre tâche asynchrone à partir de la méthode **Main** . La méthode **Main** intercepte les exceptions et les consigne dans la console.

```csharp
static void Main(string[] args)
{
        // ADD THIS PART TO YOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key to exit.");
                Console.ReadKey();
        }
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour générer et exécuter l’application.

## <a id="CreateDatabase"></a>Créer une base de données

Avant d'ajouter le code permettant de créer une base de données, ajoutez une méthode d'assistance pour l'écriture sur la console. Copiez et collez la méthode **WriteToConsoleAndPromptToContinue** sous la méthode **GetStartedDemo**.

```csharp
// ADD THIS PART TO YOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Créez votre base de données Azure Cosmos DB en utilisant la méthode `CreateDatabaseAsync` de la classe **DocumentClient**. Une base de données est le conteneur logique de stockage de documents JSON partitionné entre les collections. Copiez et collez le code suivant dans votre méthode **GetStartedDemo** en dessous de la création du client. Cela va créer la base de données *FamilyDB*.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour exécuter l’application.

## <a id="CreateColl"></a>Créer une collection

Créez une collection à l’aide de la méthode `CreateDocumentCollectionAsync` de la classe **DocumentClient**. Une collection est un conteneur de documents JSON. Elle est associée à une logique d'application JavaScript.

> [!WARNING]
> **CreateDocumentCollectionAsync** crée une collection avec un débit réservé, ce qui a des conséquences sur la tarification. Pour plus d’informations, consultez la [page sur la tarification](https://azure.microsoft.com/pricing/details/cosmos-db/).

Copiez et collez le code suivant dans votre méthode **GetStartedDemo** en dessous de la création de la base de données. Ce code crée une collection de documents appelée *FamilyCollection_oa*.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour exécuter l’application.

## <a id="CreateDoc"></a>Créer des documents JSON

Créez un document à l’aide de la méthode `CreateDocumentAsync` de la classe **DocumentClient**. Les documents sont du contenu JSON (arbitraire) défini par l'utilisateur. Vous pouvez maintenant insérer un ou plusieurs documents. 

Vous commencez par créer une classe **Family** représentant les objets stockés dans Azure Cosmos DB. Vous créez également les sous-classes **Parent**, **Child**, **Pet** et **Address** qui seront utilisées dans **Family**. Les documents doivent avoir une propriété **Id** sérialisée comme **id** dans JSON. Créez ces classes en ajoutant les sous-classes internes suivantes après la méthode **GetStartedDemo**. Copiez et collez les classes **Family**, **Parent**, **Child**, **Pet** et **Address** sous la méthode **WriteToConsoleAndPromptToContinue**.

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key to continue ...");
    Console.ReadKey();
}

// ADD THIS PART TO YOUR CODE
public class Family
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public string LastName { get; set; }
    public Parent[] Parents { get; set; }
    public Child[] Children { get; set; }
    public Address Address { get; set; }
    public bool IsRegistered { get; set; }
    public override string ToString()
    {
            return JsonConvert.SerializeObject(this);
    }
}

public class Parent
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
}

public class Child
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
    public string Gender { get; set; }
    public int Grade { get; set; }
    public Pet[] Pets { get; set; }
}

public class Pet
{
    public string GivenName { get; set; }
}

public class Address
{
    public string State { get; set; }
    public string County { get; set; }
    public string City { get; set; }
}
```

Copiez et collez la méthode **CreateFamilyDocumentIfNotExists** sous votre méthode **CreateDocumentCollectionIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

Insérez ensuite deux documents, un pour la famille Andersen, l’autre pour la famille Wakefield. Copiez et collez le code suivant `// ADD THIS PART TO YOUR CODE` dans votre méthode **GetStartedDemo** en dessous de la création de la collection de documents.

```csharp
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FirstName = "Henriette Thaulow",
                        Gender = "female",
                        Grade = 5,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Fluffy" }
                        }
                }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FamilyName = "Merriam",
                        FirstName = "Jesse",
                        Gender = "female",
                        Grade = 8,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Goofy" },
                                new Pet { GivenName = "Shadow" }
                        }
                },
                new Child
                {
                        FamilyName = "Miller",
                        FirstName = "Lisa",
                        Gender = "female",
                        Grade = 1
                }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour exécuter l’application.

![Relation hiérarchique existant entre le compte, la base de données en ligne, la collection](./media/sql-api-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Interroger les ressources Azure Cosmos DB

Azure Cosmos DB prend en charge les requêtes enrichies sur les documents JSON stockés dans chaque collection. L’exemple de code suivant affiche différentes requêtes (à l’aide de la syntaxe SQL d’Azure Cosmos DB et de LINQ) que vous pouvez exécuter sur les documents insérés à l’étape précédente.

Copiez et collez la méthode **ExecuteSimpleQuery** sous votre méthode **CreateFamilyDocumentIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find the Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute the same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Copiez et collez le code suivant dans votre méthode **GetStartedDemo** en dessous de la création du deuxième document.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART TO YOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour exécuter l’application.

Le schéma suivant montre comment la syntaxe de requête SQL d’Azure Cosmos DB est appelée sur la collection que vous avez créée. La même logique s’applique à la requête LINQ.

![Diagramme illustrant l’étendue et la signification de la requête utilisée par le didacticiel NoSQL pour créer une application de console C#](./media/sql-api-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

Le mot clé `FROM` est facultatif dans la requête, car les requêtes Azure Cosmos DB sont déjà étendues à une collection unique. Par conséquent, « FROM Families f » peut être remplacé par «FROM root r » ou par tout autre nom de variable que vous choisissez. Azure Cosmos DB déduit alors que Families, root, ou le nom de variable choisi, référencent par défaut la collection actuelle.

## <a id="ReplaceDocument"></a>Remplacer le document JSON

Azure Cosmos DB prend en charge le remplacement des documents JSON.  

Copiez et collez la méthode **ReplaceFamilyDocument** sous votre méthode **ExecuteSimpleQuery**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
    this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
}
```

Copiez et collez le code suivant dans votre méthode **GetStartedDemo** en dessous de l’exécution de la requête. Après avoir remplacé le document, cela exécutera à nouveau la même requête pour afficher le document modifié.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
// Update the Grade of the Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour exécuter l’application.

## <a id="DeleteDocument"></a>Supprimer le document JSON

Azure Cosmos DB prend en charge la suppression des documents JSON.  

Copiez et collez la méthode **DeleteFamilyDocument** sous votre méthode **ReplaceFamilyDocument**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
    Console.WriteLine("Deleted Family {0}", documentName);
}
```

Copiez et collez le code suivant dans votre méthode **GetStartedDemo** en dessous de l’exécution de la deuxième requête.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO CODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour exécuter l’application.

## <a id="DeleteDatabase"></a>Supprimer la base de données

Supprimer la base de données créée revient à supprimer la base de données et toutes les ressources enfants (collections, documents, etc.). Copiez et collez le code suivant dans votre méthode **GetStartedDemo** sous le document, pour supprimer la base de données et toutes ses ressources enfants.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART TO CODE
// Clean up/delete the database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Appuyez sur le bouton **DocumentDBGettingStarted** pour exécuter l’application.

## <a id="Run"></a>Exécuter votre application de console C#

Appuyez sur le bouton **DocumentDBGettingStarted** dans Visual Studio pour générer l’application en mode débogage. La sortie de votre application de prise en main doit s’afficher dans la fenêtre de la console. Celle-ci doit présenter les résultats des requêtes que nous avons ajoutées, qui doivent correspondre au texte d'exemple ci-dessous.

```bash
Created FamilyDB_oa
Press any key to continue ...
Created FamilyCollection_oa
Press any key to continue ...
Created Family Andersen.1
Press any key to continue ...
Created Family Wakefield.7
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key to exit.
```

Vous avez terminé le tutoriel et vous disposez d’une application de console C# qui fonctionne.

## <a name="clean-up-resources"></a>Supprimer des ressources

Dès que vous n’en avez plus besoin, vous pouvez supprimer le groupe de ressources, le compte Azure Cosmos et toutes les ressources associées. Pour cela, sélectionnez le groupe de ressources de la machine virtuelle, sélectionnez **Supprimer**, puis confirmez le nom du groupe de ressources à supprimer.

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez appris à créer une application .NET Core pour gérer des données stockées dans le compte d'API SQL d'Azure Cosmos DB. Vous pouvez maintenant passer à l’article suivant :

> [!div class="nextstepaction"]
> [Créer une application de console Java avec le compte d’API SQL d'Azure Cosmos DB](sql-api-java-get-started.md)

[create-sql-api-dotnet.md#create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-dotnetcore-get-started/nosql-tutorial-keys.png
