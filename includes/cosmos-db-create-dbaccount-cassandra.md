1. Meld u in een nieuw browservenster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **Een resource maken** > **Databases** > **Azure Cosmos DB**.
   
   ![Het deelvenster Databases in Azure Portal](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. Voer op de pagina **Nieuw account** de instellingen in voor het nieuwe Azure Cosmos DB-account. 
 
    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Id|*Voer een unieke naam in*|Voer een unieke naam in om dit Azure Cosmos DB-account te identificeren. Omdat *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw contactpunt te maken, gebruikt u een unieke, maar identificeerbare id.<br><br>De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 50 tekens lang zijn.
    API|Cassandra|De API bepaalt het type te maken account. Azure Cosmos DB biedt vijf API's die aansluiten bij de behoeften van uw toepassing: SQL (documentdatabase), Gremlin (grafiekdatabase) MongoDB (documentdatabase), Azure Table en Cassandra. Momenteel is voor elke API een afzonderlijk account nodig. <br><br>Selecteer **Cassandra**, omdat u in deze snelstartgids een database met brede kolommen maakt waarop query's kunnen worden uitgevoerd met behulp van CQL-syntaxis.<br><br>Als Cassandra (brede kolom) niet in de lijst wordt weergegeven, moet u [een aanvraag indienen om deel te nemen](../articles/cosmos-db/cassandra-introduction.md#sign-up-now) aan het API-previewprogramma voor Cassandra.<br><br> [Meer informatie over de Cassandra-API](../articles/cosmos-db/cassandra-introduction.md)|
    Abonnement|*Uw abonnement*|Selecteer het Azure-abonnement dat u wilt gebruiken voor dit Azure Cosmos DB-account. 
    Resourcegroep|*Geef dezelfde unieke naam op zoals hierboven bij id*|Voer een nieuwe resourcegroepnaam in voor het account. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken. 
    Locatie|*Selecteer de regio die het dichtst bij uw gebruikers is gelegen*|Selecteer de geografische locatie waar u het Azure Cosmos DB-account gaat hosten. Gebruik de locatie die zich het dichtst bij uw gebruikers bevindt, zodat ze de snelst mogelijke toegang tot de gegevens hebben.
    Vastmaken aan dashboard | Selecteer | Selecteer dit vakje, zodat uw nieuwe databaseaccount wordt toegevoegd aan uw portaldashboard voor eenvoudige toegang.

    Klik vervolgens op **Maken**.

    ![De blade Nieuw account voor Azure Cosmos DB](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-2.png)

4. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account geeft de portaldashboard de tegel **Azure Cosmos DB implementeren** weer.

    ![De tegel Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount-cassandra/deploying-cosmos-db.png)

    Nadat het account is gemaakt, wordt de pagina **Gefeliciteerd! Uw Azure Cosmos DB-account is gemaakt** weergegeven. 

