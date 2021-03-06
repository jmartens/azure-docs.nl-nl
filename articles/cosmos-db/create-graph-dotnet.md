---
title: Een Azure Cosmos DB .NET Framework- of Core-toepassing ontwikkelen met Graph API | Microsoft Docs
description: Presenteert een .NET Framework-/Core-codevoorbeeld dat u kunt gebruiken om verbinding te maken met Azure Cosmos DB en een query uit te voeren
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/08/2018
ms.author: lbosq
ms.openlocfilehash: 38869444d43a3fb5c37a222ef58d30fc607106aa
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-cosmos-db-build-a-net-framework-or-core-application-using-the-graph-api"></a>Azure Cosmos DB: een .NET Framework- of Core-toepassing ontwikkelen met Graph API

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u een Azure Cosmos DB-account, een database en een graaf (container) kunt maken met behulp van Azure Portal. U gaat vervolgens een console-app ontwikkelen en uitvoeren met behulp van het opensourcestuurprogramma [Gremlin.Net](http://tinkerpop.apache.org/docs/3.2.7/reference/#gremlin-DotNet).  

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

Als u Visual Studio 2017 al hebt geïnstalleerd, controleert u of u alles hebt geïnstalleerd tot en met [Visual Studio 2017 Update 3](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Een graaf toevoegen

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Graph API-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een venster in een git-terminal zoals git bash en `cd` naar uw werkmap.  

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-gremlindotnet-getting-started.git
    ```

3. Open vervolgens Visual Studio en open het oplossingenbestand.

4. Herstel de NuGet-pakketten in het project. Dit geldt voor het stuurprogramma Gremlin.Net, evenals het pakket Newtonsoft.Json.

5. U kunt het stuurprogramma Gremlin.Net versie 3.2.7 ook handmatig installeren met Nuget-pakketbeheer of het [Nuget-opdrachtregelprogramma](https://docs.microsoft.com/en-us/nuget/install-nuget-client-tools): 

    ```bash
    nuget install Gremlin.Net -Version 3.2.7
    ```

## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand Program.cs en u zult zien dat deze regels code de Azure Cosmos DB-resources maken. 

* Stel uw verbindingsparameters in op basis van het hierboven gemaakte account (regel 19): 

    ```csharp
    private static string hostname = "your-endpoint.gremlin.cosmosdb.azure.com";
    private static int port = 443;
    private static string authKey = "your-authentication-key";
    private static string database = "your-database";
    private static string collection = "your-collection-or-graph";
    ```

* De uit te voeren Gremlin-opdrachten worden vermeld in een woordenboek (regel 26):

    ```csharp
    private static Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
    {
        { "Cleanup",        "g.V().drop()" },
        { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
        { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
        { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
        { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
        { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
        { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
        { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
        { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
        { "CountVertices",  "g.V().count()" },
        { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
        { "Project",        "g.V().hasLabel('person').values('firstName')" },
        { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
        { "Traverse",       "g.V('thomas').out('knows').hasLabel('person')" },
        { "Traverse 2x",    "g.V('thomas').out('knows').hasLabel('person').out('knows').hasLabel('person')" },
        { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
        { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
        { "CountEdges",     "g.E().count()" },
        { "DropVertex",     "g.V('thomas').drop()" },
    };
    ```


* Maak een `GremlinServer`-verbindingsobject met de bovenstaande parameters (regel 52):

    ```csharp
    var gremlinServer = new GremlinServer(hostname, port, enableSsl: true, 
                                                    username: "/dbs/" + database + "/colls/" + collection, 
                                                    password: authKey);
    ```

* Maak een nieuw `GremlinClient`-object (regel 56):

    ```csharp
    var gremlinClient = new GremlinClient(gremlinServer);
    ```

* Voer elke Gremlin-query uit met behulp van het `GremlinClient`-object met een asynchrone taak (regel 63). Hiermee worden de Gremlin-query's gelezen uit het hierboven gedefinieerde woordenboek (regel 26):

    ```csharp
    var task = gremlinClient.SubmitAsync<dynamic>(query.Value);
    task.Wait();
    ```

* Haal het resultaat op en lees de waarden, die zijn opgemaakt als een woordenboek, met behulp van de `JsonSerializer`-klasse vanuit Newtonsoft.Json:

    ```csharp
    foreach (var result in task.Result)
    {
        // The vertex results are formed as dictionaries with a nested dictionary for their properties
        string output = JsonConvert.SerializeObject(result);
        Console.WriteLine(String.Format("\tResult:\n\t{0}", output));
    }
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Klik in [Azure Portal](http://portal.azure.com/) op **Sleutels**. 

    Kopieer het eerste gedeelte van de URI-waarde.

    ![Een toegangssleutel in Azure Portal bekijken en kopiëren op de pagina Sleutels](./media/create-graph-dotnet/keys.png)

2. Plak in Program.cs de waarde over `your-endpoint` in de variabele `hostname` op regel 19. 

    `"private static string hostname = "your-endpoint.gremlin.cosmosdb.azure.com";`

    De eindpuntwaarde ziet er nu als volgt:

    `"private static string hostname = "testgraphacct.gremlin.cosmosdb.azure.com";`

3. Kopieer de waarde van uw **primaire sleutel** uit de portal en plak deze in de variabele `authkey`, waarbij de tijdelijke aanduiding `"your-authentication-key"` op regel 21 wordt vervangen. 

    `private static string authKey = "your-authentication-key";`

4. Plak met behulp van de gegevens van de hierboven gemaakte database de databasenaam in de variabele `database` op regel 22. 

    `private static string database = "your-database";`

5. Plak op dezelfde manier de verzameling met behulp van de gegevens van de hierboven gemaakte verzameling (die ook de grafieknaam is) in de variabele `collection` op regel 23. 

    `private static string collection = "your-collection-or-graph";`

6. Sla het bestand Program.cs op. 

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="run-the-console-app"></a>De console-app uitvoeren

Klik op Ctrl+F5 om de toepassing uit te voeren. De toepassing drukt zowel de Gremlin-queryopdrachten als de -resultaten af in de console.

   In het consolevenster worden de hoekpunten en randen weergegeven die aan de graaf worden toegevoegd. Zodra het script is voltooid, drukt u op Enter om het consolevenster te sluiten.

## <a name="browse-using-the-data-explorer"></a>Bladeren met Data Explorer

U kunt nu teruggaan naar Data Explorer in Azure Portal en door uw nieuwe graafgegevens bladeren en er query’s op uitvoeren.

1. De nieuwe database wordt in Data Explorer weergegeven in het deelvenster Grafieken. Vouw de database en verzamelingsknooppunten uit en klik vervolgens op **Grafiek**.

2. Klik op de knop **Filter toepassen** om de standaardquery te gebruiken om alle hoekpunten in de grafiek weer te geven. De gegevens die worden gegenereerd door de voorbeeldapp worden weergegeven in het deelvenster Grafen.

    U kunt op de grafiek in- en uitzoomen, u kunt u de grafiek uitvouwen voor meer ruimte, extra hoekpunten toevoegen en hoekpunten verplaatsen in de weergaveruimte.

    ![De grafiek weergeven in Data Explorer in Azure Portal](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal: 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een graaf gemaakt met Data Explorer en hebt u een app uitgevoerd. U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)

