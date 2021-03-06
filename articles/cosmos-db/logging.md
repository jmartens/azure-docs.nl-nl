---
title: Diagnostische logboekregistratie van Azure DB Cosmos | Microsoft Docs
description: Met deze zelfstudie kunt u aan de slag met Azure Cosmos DB logboekregistratie.
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: mimig
ms.openlocfilehash: f647387b4e80c36339a456b8e9a2cfade7ac8102
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="azure-cosmos-db-diagnostic-logging"></a>Diagnostische logboekregistratie van Azure DB Cosmos

Zodra u hebt gestart met behulp van een of meer Azure DB die Cosmos-databases, u kunt controleren hoe en wanneer uw databases worden geopend. In dit artikel biedt een overzicht van alle beschikbare logboeken op de Azure-platform en vervolgens wordt uitgelegd hoe u Diagnostische logboekregistratie voor controledoeleinden logboeken te verzenden in te schakelen [Azure Storage](https://azure.microsoft.com/services/storage/), ze streamt [Azure Event Hubs ](https://azure.microsoft.com/services/event-hubs/), en/of exporteren naar [logboekanalyse](https://azure.microsoft.com/services/log-analytics/), die deel uitmaakt van [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite).

## <a name="logs-available-in-azure"></a>Logboeken beschikbaar in Azure

Voordat we krijgen tot uw account voor Azure Cosmos DB bewaking, kunnen een aantal zaken over logboekregistratie en controle verduidelijken. Er zijn verschillende typen logboeken op de Azure-platform. Er zijn [Azure activiteitenlogboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), gebeurtenissen, heartbeat bewaking, operations-Logboeken, enzovoort. Er zijn breed spectrum van Logboeken. U ziet een volledige lijst met Logboeken in [Azure Log Analytics](https://azure.microsoft.com/en-us/services/log-analytics/) in de Azure portal. 

De volgende afbeelding toont de verschillende soorten Azure logboeken beschikbaar.

![Verschillende soorten Azure Logboeken](./media/logging/azurelogging.png)

In de bovenstaande afbeelding **Rekenresources** vertegenwoordigen de Azure-resources waarvoor u toegang hebt tot het Gastbesturingssysteem. Bijvoorbeeld Azure Virtual Machines, virtuele-machineschaalsets, enz. Azure Container Service worden beschouwd als bronnen berekenen. COMPUTE resources activiteitenlogboeken, diagnostische logboeken en toepassingslogboeken genereren. Raadpleeg voor meer informatie de [Azure Monitoring – rekenresources](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---compute-subset) artikel.

**Niet-rekenresources** bronnen waarin u kan geen toegang tot het onderliggende besturingssysteem en samenwerken met resource. Bijvoorbeeld: Netwerkbeveiligingsgroepen Logic Apps enzovoort. **Cosmos DB** is een niet-compute-resource. U kunt de logboeken voor niet-rekenresources weergeven in het activiteitenlogboek of doordat de optie Logboeken met diagnostische gegevens in de portal. Raadpleeg voor meer informatie de [Azure Monitoring – niet rekenresources](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---everything-else) artikel.

Het activiteitenlogboek registreert de bewerkingen op abonnementsniveau voor Cosmos DB, bewerkingen, zoals ListKeys, schrijven DatabaseAccounts enzovoort worden geregistreerd. Diagnostische logboeken bieden meer gedetailleerde logboekregistratie en kunt u aan te melden DataPlaneRequests (maken, lezen, Query... ) en MongoRequests.


Voor onze bespreking kunt richten op de Azure-activiteit, Azure diagnostisch en metrische gegevens. Wat is dus het verschil tussen deze drie logboeken? 

### <a name="azure-activity-log"></a>Azure Activity Log

De Azure Activity Log is een abonnementlogboek biedt inzicht in het abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Het activiteitenlogboek rapporten besturingselement vlak gebeurtenissen voor uw abonnementen onder de beheercategorie. Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die zijn gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. 

Het activiteitenlogboek verschilt van diagnostische logboeken. Activiteitenlogboeken bevatten gegevens over de bewerkingen van een resource van buitenaf (de ' besturingselement-vlak'). In de context Azure Cosmos DB maken een deel van het besturingselement vlak bewerkingen behoren verzameling, lijst met sleutels, delete-sleutels, lijst database, enzovoort. Logboeken met diagnostische gegevens worden gegenereerd door een resource en bieden informatie over de werking van de bron (de "data-vlak'). Sommige van de voorbeelden van gegevens vlak diagnostische logboeken zijn delete, insert, readfeed bewerking, enzovoort.

Activiteitenlogboeken (vlak beheerbewerkingen) is mogelijk veel meer uitgebreidere aard, daarbij het volledige e-mailadres van de aanroeper, aanroeper IP-adres, Resourcenaam, bewerkingsnaam en TenantId, enz. Het activiteitenlogboek bevat verschillende [categorieën](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema) van gegevens. Zie voor volledige informatie over de schema's uit deze categorieën [Azure Activity Log gebeurtenis schema](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema).  Diagnostische logboeken kan echter zijn beperkende aard zoals PII gegevens vaak uit deze is verwijderd. Dus moet u wellicht de IP-adres van de aanroeper, maar de laatste octent wordt verwijderd.

### <a name="azure-metrics"></a>Azure metrische gegevens

[Azure metrische gegevens](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-metrics), de belangrijkste soort Azure telemetriegegevens (ook wel prestatiemeteritems) die door de meeste Azure-resources. Metrische gegevens kunt u informatie weergeven over de doorvoer, opslag, consistentie, beschikbaarheid en de latentie van uw Azure DB die Cosmos-resources. Zie voor meer informatie [bewaking en foutopsporing met metrische gegevens in Azure Cosmos DB](use-metrics.md).

### <a name="azure-diagnostic-logs"></a>Azure diagnostische logboeken

Azure diagnostische logboeken zijn verzonden door een resource van Logboeken en leveren van uitgebreide, regelmatig gegevens over de werking van de bron. De inhoud van deze logboeken varieert per resourcetype. Diagnostische logboeken niveau resource afwijken van de Gast OS-niveau diagnostische logboeken. Gastbesturingssysteem diagnostische logboeken zijn deze die worden verzameld door een agent wordt uitgevoerd binnen een virtuele machine of andere ondersteund resourcetype. Diagnostische logboeken niveau resource vereisen geen gegevens van de resource-specifieke agent en vastleggen van de Azure-platform zelf, terwijl de Gast OS-niveau diagnostische logboeken vastleggen van gegevens van het besturingssysteem en toepassingen die worden uitgevoerd op een virtuele machine.

![Diagnostische logboekregistratie voor opslag, Event Hubs of Operations Management Suite via Log Analytics](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>Wat is vastgelegd door diagnostische logboeken van Azure?

* Alle back-end voor geverifieerde aanvragen (TCP/REST) worden voor alle API's, vastgelegd, ook mislukte aanvragen als gevolg van toegangsmachtigingen, systeemfouten of ongeldige aanvragen. Ondersteuning voor de gebruiker geïnitieerd grafiek, Cassandra, en tabel API-aanvragen zijn momenteel niet beschikbaar.
* Bewerkingen op de database, inclusief CRUD-bewerkingen op alle documenten, containers en -databases.
* Bewerkingen voor sleutels, waaronder maken, wijzigen of verwijderen van deze sleutels.
* Niet-geverifieerde aanvragen die in een 401-respons resulteren. Dit zijn bijvoorbeeld aanvragen die geen Bearer-token hebben, ongeldige of verlopen aanvragen of aanvragen met een ongeldig token.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Schakelt logboekregistratie in de Azure portal

Diagnostische gegevens als logboekregistratie wilt inschakelen, hebt u de volgende bronnen:

* Een bestaande Azure DB die Cosmos-account, database en container. Zie voor instructies over het maken van deze resources [een databaseaccount maken met de Azure-portal](create-sql-api-dotnet.md#create-a-database-account), [CLI voorbeelden](cli-samples.md), of [voorbeelden PowerShell](powershell-samples.md).

Schakel diagnostische gegevens vastleggen in de Azure portal door het volgende doen:

1. In de [Azure-portal](https://portal.azure.com), in uw Azure DB-Cosmos-account, klikt u op **diagnostische logboeken** in de navigatiebalk aan de linkerkant en klik vervolgens op **diagnostische gegevens inschakelen**.

    ![Schakel Diagnostische logboekregistratie voor Azure Cosmos DB in de Azure portal](./media/logging/turn-on-portal-logging.png)

2. In de **diagnostische instellingen** pagina, de volgende handelingen uit: 

    * **Naam**. Voer een naam voor de logboeken te maken.

    * **Archiveren naar een opslagaccount**. Om deze optie gebruikt, moet u een bestaand opslagaccount verbinding maken met. Zie voor informatie over het maken van een nieuw opslagaccount in de portal [een opslagaccount maken](../storage/common/storage-create-storage-account.md) en volg de instructies voor het maken van een Resource Manager voor algemene doeleinden-account. Keer vervolgens terug naar deze pagina in de portal voor uw storage-account selecteren. Duurt enkele minuten duren voordat de zojuist gemaakte storage-accounts worden weergegeven in de vervolgkeuzelijst.
    * **Stream naar een event hub**. Om deze optie gebruikt, moet u een Event Hub-naamruimte en event hub verbinding maken met. Zie het maken van een naamruimte Event Hubs [een Event Hubs-naamruimte en een event hub met de Azure portal maken](../event-hubs/event-hubs-create.md). Keer vervolgens terug naar deze pagina in de portal om de Event Hub-naamruimte en de beleid-naam te selecteren.
    * **Verzenden met logboekanalyse**.     Om deze optie gebruikt, gebruik een bestaande werkruimte of maak een nieuwe werkruimte voor logboekanalyse volgens de stappen voor het [Maak een nieuwe werkruimte](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) in de portal. Zie voor meer informatie over het weergeven van uw logboeken in logboekanalyse [weergave wordt geregistreerd in logboekanalyse](#view-in-loganalytics).
    * **Meld u DataPlaneRequests**. Selecteer deze optie aanvragen moeten worden geregistreerd back-end van Azure Cosmos DB onderliggende gedistribueerde platform voor SQL, grafiek, MongoDB, Cassandra en tabel API-accounts. Als u naar een opslagaccount archiveert, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaartermijn is verstreken.
    * **Meld u MongoRequests**. Selecteer deze optie om de gebruiker gestarte logboekaanvragen van Azure Cosmos DB front-end voor voor de MongoDB-API-accounts.  Als u naar een opslagaccount archiveert, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaartermijn is verstreken.
    * **Metrische aanvragen**. Selecteer deze optie om de uitgebreide gegevens opslaan in [Azure metrische gegevens](../monitoring-and-diagnostics/monitoring-supported-metrics.md). Als u naar een opslagaccount archiveert, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaartermijn is verstreken.

3. Klik op **Opslaan**.

    Als u een foutbericht ontvangt ' kan niet bijwerken van diagnostische gegevens voor \<Werkruimtenaam >. Het abonnement \<abonnements-id > is niet geregistreerd voor het gebruik van microsoft.insights. " Ga als volgt de [oplossen Azure Diagnostics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) instructies voor het registreren van het account en probeer deze procedure.

    Als u wilt wijzigen hoe uw logboeken met diagnostische gegevens in de toekomst op elk gewenst moment worden opgeslagen, kunt u op elk gewenst moment naar deze pagina op terugkeren om de instellingen diagnostische logboeken voor uw account te wijzigen.

## <a name="turn-on-logging-using-cli"></a>Schakel logboekregistratie met CLI

Voor het inschakelen van metrische gegevens en logboekregistratie van diagnostische gegevens met de Azure CLI, gebruik de volgende opdrachten:

- Om opslag van diagnostische logboeken in een Opslagaccount, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   De `resourceId` is de naam van de Azure DB die Cosmos-account. De `storageId` is de naam van het opslagaccount waarnaar u wilt de logboeken verzenden.

- Om streaming van logboeken met diagnostische gegevens naar een Event Hub, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   De `resourceId` is de naam van de Azure DB die Cosmos-account. De `serviceBusRuleId` is een tekenreeks met deze indeling:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Om in te schakelen naar een werkruimte voor logboekanalyse verzenden van diagnostische logboeken, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

U kunt deze parameters zodat meerdere uitvoeropties combineren.

## <a name="turn-on-logging-using-powershell"></a>Schakel logboekregistratie met behulp van PowerShell

Als u Diagnostische logboekregistratie met behulp van PowerShell, moet u Azure Powershell met een versie 1.0.1 of hoger.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) om Azure PowerShell te installeren en te koppelen aan uw Azure-abonnement.

Als u Azure PowerShell al hebt geïnstalleerd en de versie van de PowerShell-console niet weet, typt u `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Verbinding maken met uw abonnementen
Start een Azure PowerShell-sessie en gebruik de volgende opdracht om u aan te melden bij uw Azure-account:  

```powershell
Login-AzureRmAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Azure PowerShell haalt alle abonnementen op die zijn gekoppeld aan dit account en gebruikt standaard het eerste abonnement.

Als u meerdere abonnementen hebt, moet u wellicht specifiek opgeven welk abonnement is gebruikt voor het maken van uw Azure Sleutelkluis. Typ het volgende als u de abonnementen voor uw account wilt zien:

```powershell
Get-AzureRmSubscription
```

Als u het abonnement dat is gekoppeld aan de Azure DB die Cosmos-account dat u zich aanmeldt, typ:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Als u meerdere abonnementen die zijn gekoppeld aan uw account is het belangrijk om op te geven van het abonnement hebt.
>
>

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie over het configureren van Azure PowerShell.

### <a id="storage"></a>Een nieuw opslagaccount voor uw logboeken maken
Hoewel u een bestaand opslagaccount voor uw Logboeken gebruiken kunt, in deze zelfstudie wordt een nieuw opslagaccount maken toegewezen aan Azure DB die Cosmos-Logboeken. Voor het gemak we de accountdetails van de opslag opslaat in een variabele met de naam **sa**.

Nog gemakkelijker te maken van beheer in deze zelfstudie gebruiken we dezelfde resourcegroep als de database met onze Azure DB die Cosmos-database. Vervang de waarden voor ContosoResourceGroup, contosocosmosdblogs en 'Noordelijk Centraal, VS' voor uw eigen waarden, indien van toepassing:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Als u een bestaand opslagaccount gebruiken, moet hetzelfde abonnement gebruiken als uw abonnement Azure Cosmos DB en het Resource Manager-implementatiemodel, in plaats van het klassieke implementatiemodel moet gebruiken.
>
>

### <a id="identify"></a>De Azure DB die Cosmos-account voor uw logboeken identificeren
De naam van de Azure DB die Cosmos-account aan een variabele met de naam ingesteld **account**, waarbij ResourceName de naam van de Azure DB die Cosmos-account.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Logboekregistratie inschakelen
Gebruik de cmdlet Set-AzureRmDiagnosticSetting, samen met de variabelen voor het nieuwe opslagaccount, Azure DB die Cosmos-account en de categorie die u wilt inschakelen, logboeken u schakelt de logboekregistratie voor Azure Cosmos DB. Voer de volgende opdracht, instellen van de **-ingeschakeld** markering **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

De uitvoer voor de opdracht moet als volgt uit:

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

Hiermee bevestigt u dat logboekregistratie nu is ingeschakeld voor uw database, het opslaan van gegevens naar uw opslagaccount.

U kunt eventueel ook een retentiebeleid instellen voor uw logboeken, zodat oudere logboeken automatisch worden verwijderd. Stel bijvoorbeeld een retentiebeleid in met behulp van de vlag **- RetentionEnabled** ingesteld op **$true** en stel de parameter **- RetentionInDays** in op **90**, zodat logboeken die ouder zijn dan 90 dagen automatisch worden verwijderd.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Toegang tot uw logboeken
Azure DB van de Cosmos-logboeken voor **DataPlaneRequests** categorie worden opgeslagen in de **insights-logboeken--vlak-gegevensaanvragen** container in het opslagaccount dat u hebt opgegeven. 

Maak eerst een variabele voor de containernaam. Deze wordt gebruikt in de rest van de procedure.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Als u alle blobs in deze container wilt weergeven, typt u:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

De uitvoer ziet er ongeveer als volgt uit:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

Als u in deze uitvoer zien kunt, de blobs de volgende naamgevingsregels: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

De datum- en tijdwaarden maken gebruik van UTC.

Aangezien hetzelfde opslagaccount kan worden gebruikt voor het verzamelen van Logboeken voor meerdere resources, is de volledig gekwalificeerde resource-ID in de blob-naam erg handig om te openen of downloaden van alleen de blobs die u nodig hebt. Maar eerst laten we zien hoe u alle blobs kunt downloaden.

Maak eerst een map waarin u de blobs wilt downloaden. Bijvoorbeeld:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Haal vervolgens een lijst met alle blobs op:  

```powershell
$blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Sluis deze lijst via 'Get-AzureStorageBlobContent' door om de blobs in de doelmap te downloaden:

```powershell
$blobs | Get-AzureStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Wanneer u deze tweede opdracht uitvoert de  **/**  scheidingsteken in de blobnamen een volledige mapstructuur onder de doelmap maakt. Deze mapstructuur wordt gebruikt om te downloaden en opslaan van de blobs als bestanden.

Als u alleen specifieke blobs wilt downloaden, moet u jokertekens gebruiken. Bijvoorbeeld:

* Als u meerdere databases hebt en wilt downloaden van Logboeken voor slechts één database, met de naam CONTOSOCOSMOSDB3:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Als u meerdere resourcegroepen hebt en logboeken voor slechts één resourcegroep wilt downloaden, gebruikt u `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Als u alle logboeken voor de maand van juli 2017 downloaden wilt, gebruikt u `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Daarnaast doet u het volgende:

* Query uitvoeren op de status van diagnostische instellingen voor uw database-resource: `Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
* Uitschakelen van logboekregistratie van **DataPlaneRequests** categorie voor uw account database resource: `Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`


De blobs die worden geretourneerd in elk van deze query's worden opgeslagen als tekst, die is opgemaakt als een JSON-blob, zoals wordt weergegeven in de volgende code. 

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

Zie voor meer informatie over de gegevens in elke blob JSON, [uw logboeken Azure Cosmos DB interpreteren](#interpret).

## <a name="managing-your-logs"></a>Het beheren van uw logboeken

Diagnostische logboeken worden beschikbaar gesteld in uw account twee uur vanaf het moment dat de Azure DB die Cosmos-bewerking is uitgevoerd. Het is aan u om uw logboeken in uw opslagaccount te beheren:

* Gebruik standaardmethoden van Azure voor toegangsbeheer om uw logboeken te beveiligen door het aantal gebruikers te beperken dat toegang heeft tot de logboeken.
* Verwijder de logboeken die u niet meer in uw opslagaccount wilt bewaren.
* De bewaarperiode voor gegevens vlak aanvragen gearchiveerd naar een opslagaccount in de portal is geconfigureerd als **logboek DataPlaneRequests** is geselecteerd. Als u wilt wijzigen die instelling, Zie [schakelt logboekregistratie in de Azure portal](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-log-analytics"></a>Logboeken bekijken in Log Analytics

Als u hebt geselecteerd de **verzenden met logboekanalyse** optie bij het inschakelen van diagnostische logboekregistratie diagnostische gegevens van uw verzameling wordt doorgestuurd naar logboekanalyse binnen twee uur. Dit betekent dat als u onmiddellijk na het inschakelen van logboekregistratie logboekanalyse bekijkt, u niet alle gegevens ziet. Twee uur wachten en probeer het opnieuw 

Voordat u uw logboeken weergeeft, moet u te zien als uw werkruimte voor logboekanalyse is bijgewerkt naar de nieuwe Log Analytics query language gebruiken. U kunt dit controleren, opent u de [Azure-portal](https://portal.azure.com), klikt u op **logboekanalyse** aan de linkerkant, selecteer vervolgens de naam van de werkruimte zoals weergegeven in de volgende afbeelding. De **OMS-werkruimte** pagina wordt weergegeven zoals in de volgende afbeelding.

![Log Analytics in de Azure portal](./media/logging/azure-portal.png)

Als u het volgende bericht wordt weergegeven op de **OMS-werkruimte** pagina uw werkruimte is niet bijgewerkt voor het gebruik van de nieuwe taal. Zie voor meer informatie over het bijwerken naar de nieuwe querytaal [Upgrade van uw Azure-logboekanalyse-werkruimte naar nieuwe logboek zoekopdracht](../log-analytics/log-analytics-log-search-upgrade.md). 

![Log analytics upgrade melding](./media/logging/upgrade-notification.png)

Open de zoekpagina logboek voor het weergeven van de diagnostische gegevens in Log Analytics vanuit het menu links of de module Beheer van de pagina zoals in de volgende afbeelding.

![Meld u opties voor zoeken in de Azure portal](./media/logging/log-analytics-open-log-search.png)

Nu dat u hebt ingeschakeld verzamelen van gegevens, het volgende logboek zoeken voorbeeld uitvoert met behulp van de nieuwe querytaal om te zien van de logboeken van de tien meest recente `AzureDiagnostics | take 10`.

![Voorbeeld nemen 10 logboek zoeken](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Query's

Hier volgen enkele aanvullende query's kunt u in de **logboek zoeken** in om u te helpen bij het bewaken van uw Azure DB die Cosmos-containers. Deze query's werken met de [nieuwe taal](../log-analytics/log-analytics-log-search-upgrade.md). 

Zie voor meer informatie over de betekenis van de gegevens die door elke logboek zoekactie geretourneerd, [uw logboeken Azure Cosmos DB interpreteren](#interpret).

* Alle diagnostische logboeken van de Azure Cosmos-database voor de opgegeven periode.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* Tien meest recent vastgelegde gebeurtenissen.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* Alle bewerkingen, gegroepeerd op bewerkingstype.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* Alle bewerkingen, gegroepeerd per Resource.

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Alle gebruikersactiviteit, gegroepeerd per resource. Houd er rekening mee dat dit een activiteitenlogboek, niet een diagnostische logboeken is.

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Welke bewerkingen duurt langer dan 3 milliseconden.

    ```
    AzureDiagnostics | where toint(duration_s) > 30000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* Welke agent wordt uitgevoerd de bewerkingen.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Wanneer zijn langlopende bewerkingen uitvoeren.

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Zie voor meer informatie over het gebruik van de nieuwe zoekopdracht logboek taal [Understanding logboek zoekt in logboekanalyse](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Uw logboeken interpreteren

Diagnostische gegevens die zijn opgeslagen in Azure Storage en Log Analytics gebruiken een vergelijkbaar schema. 

De volgende tabel beschrijft de inhoud van elke logboekvermelding.

| Azure Storage-veld of eigenschap | Log Analytics-eigenschap | Beschrijving |
| --- | --- | --- |
| tijd | TimeGenerated | De datum en tijd (UTC) wanneer de bewerking opgetreden. |
| resourceId | Resource | Het account voor Azure Cosmos DB waarvoor logboeken zijn ingeschakeld.|
| category | Category | DataPlaneRequests is de enige beschikbare waarde voor Azure DB die Cosmos-Logboeken. |
| operationName | OperationName | De naam van de bewerking. Deze waarde kan een van de volgende bewerkingen zijn: maken, bijwerken, lezen, ReadFeed, verwijderen, vervangen, uitvoeren, SqlQuery, Query, JSQuery, Head, HeadFeed of Upsert.   |
| properties | N.v.t. | De inhoud van dit veld worden beschreven in de volgende rijen. |
| ActivityId | activityId_g | De unieke GUID voor de geregistreerde bewerking. |
| UserAgent | userAgent_s | Een tekenreeks die opgeeft van de clientagent van de gebruiker uitvoeren van de aanvraag. De indeling is {agent gebruikersnaam} / {version}.|
| resourceType | ResourceType | Het type van de toegang tot bronnen. Deze waarde kan een van de volgende resourcetypen zijn: Database, verzameling, Document, bijlage, gebruiker, machtiging, opgeslagen procedure, Trigger, UserDefinedFunction of aanbieding. |
| statusCode |statusCode_s | De status van de reactie van de bewerking. |
| requestResourceId | ResourceId | De resourceId die betrekking hebben op de aanvraag kan verwijzen naar databaseRid, collectionRid of documentRid afhankelijk van de bewerking uitgevoerd.|
| clientIpAddress | clientIpAddress_s | De client-IP-adres. |
| requestCharge | requestCharge_s | Het aantal RUs gebruikt door de bewerking |
| collectionRid | collectionId_s | De unieke ID voor de verzameling.|
| Duur | duration_s | De duur van de bewerking in de maatstreepjes. |
| requestLength | requestLength_s | De lengte van de aanvraag, in bytes. |
| responseLength | responseLength_s | De lengte van het antwoord, in bytes.|
| resourceTokenUserRid | resourceTokenUserRid_s | Dit is niet-lege wanneer [resource tokens](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) worden gebruikt voor verificatie en verwijst naar het resource-ID van de gebruiker. |

## <a name="next-steps"></a>Volgende stappen

- Een goed begrip van niet alleen het inschakelen van logboekregistratie, maar ook de metrische gegevens en logboekbestanden categorieën ondersteund door de verschillende Azure krijgen lezen services de [overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) en [overzicht van Azure Diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikelen.
- Deze artikelen voor meer informatie over event hubs gelezen:
   - [Wat is Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Lees [metrische gegevens en diagnostische logboeken downloaden vanaf Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
- Lees [Understanding logboek zoekt in Log Analytics](../log-analytics/log-analytics-log-search-new.md)
