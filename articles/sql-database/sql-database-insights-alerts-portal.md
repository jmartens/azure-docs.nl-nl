---
title: Azure portal gebruiken om waarschuwingen van de SQL-Database maken | Microsoft Docs
description: De Azure portal gebruiken voor het maken van waarschuwingen van de SQL-Database, die meldingen of automation activeren kunnen wanneer de door u opgegeven voorwaarden wordt voldaan.
author: aamalvea
manager: craigg
services: sql-database
ms.service: sql-database
ms.custom: monitor and tune
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.openlocfilehash: 611b88c540902bc7a72d53671dacd098d9798b48
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="use-azure-portal-to-create-alerts-for-azure-sql-database-and-data-warehouse"></a>Azure portal gebruiken om waarschuwingen te maken voor Azure SQL Database en datawarehouse

## <a name="overview"></a>Overzicht
In dit artikel leest u hoe Azure SQL Database en datawarehouse waarschuwingen met de Azure portal instellen. Dit artikel bevat ook aanbevolen procedures voor het instellen van waarschuwing perioden.    

U kunt een waarschuwing op basis van bewaking metrische gegevens voor of gebeurtenissen op uw Azure-services kunt ontvangen.

* **Metrische waarden** -de waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u in beide richtingen toewijst. Dat wil zeggen, deze beide wordt geactiveerd wanneer de voorwaarde voor het eerst wordt voldaan en vervolgens later wanneer die voorwaarde wordt niet langer wordt voldaan.    
* **Activiteit logboekgebeurtenissen** -een waarschuwing kunt activeren voor *elke* gebeurtenis of alleen wanneer een bepaald aantal gebeurtenissen optreden.

U kunt de volgende handelingen uit als er wordt een waarschuwing configureren:

* e-mailmeldingen verzenden naar de servicebeheerder en medebeheerders
* e-mail verzenden naar andere e-mailberichten die u opgeeft.
* een webhook aanroepen

U kunt configureren en informatie ophalen over met behulp van regels voor waarschuwingen

* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Opdrachtregelinterface (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Een waarschuwingsregel maken op een waarde met de Azure-portal
1. In de [portal](https://portal.azure.com/), zoekt de resource die u geïnteresseerd bent in de bewaking en selecteert u deze.
2. Deze stap is verschillend voor SQL-database en elastische pools versus SQL DW: 

   - **SQL-database & alleen elastische Pools**: Selecteer **waarschuwingen** of **waarschuwing regels** onder de sectie bewaking. De tekst en het pictogram mogelijk enigszins afwijken voor verschillende bronnen.  
   
     ![Bewaking](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **SQL DW alleen**: Selecteer **bewaking** onder de sectie Algemene taken. Klik op de **DWU gebruik** grafiek.

     ![ALGEMENE TAKEN](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Selecteer de **waarschuwing toevoegen** opdracht en vul de velden in.
   
    ![Waarschuwing toevoegen](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Naam** uw Waarschuwing regel en kies een **beschrijving**, die ook weergegeven in e-mailmeldingen.
5. Selecteer de **metriek** u wilt bewaken, en kies vervolgens een **voorwaarde** en **drempelwaarde** waarde voor de metriek. Ook voor kiezen de **periode** tijdsperiode waarin de meetwaarde regel moet worden voldaan voordat de waarschuwing triggers. Dus bijvoorbeeld als u de periode 'PT5M' en de waarschuwing CPU dan 80 zoekt %, de waarschuwing wordt geactiveerd wanneer de **gemiddelde** CPU is al meer dan 80% 5 minuten. Zodra de eerste trigger is plaatsvindt, deze opnieuw wordt geactiveerd wanneer de gemiddelde CPU lager dan 80% meer dan 5 minuten is. De meting van CPU doet zich voor elke 1 minuut. Raadpleeg de onderstaande tabel voor ondersteunde tijdvensters en typt u de aggregatie dat elke waarschuwing gebruikt niet alle waarschuwingen de gemiddelde waarde gebruiken.   
6. Controleer **e-eigenaars...**  als u wilt dat beheerders en medebeheerders wilt ontvangen wanneer de waarschuwing wordt geactiveerd.
7. Als u aanvullende e-mailberichten een melding ontvangen wilt wanneer de waarschuwing wordt geactiveerd, deze toevoegen in de **aanvullende beheerder email(s)** veld. Scheid meerdere e-mailberichten met puntkomma's -  *email@contoso.com;email2@contoso.com*
8. Plaats in een geldige URI in de **Webhook** als u wilt dat deze wordt aangeroepen wanneer de waarschuwing wordt geactiveerd.
9. Selecteer **OK** wanneer u klaar bent voor het maken van de waarschuwing.   

Binnen een paar minuten de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

## <a name="managing-your-alerts"></a>Uw waarschuwingen beheren
Wanneer u een waarschuwing hebt gemaakt, kunt u deze selecteren en:

* Een grafiek weer met de metrische drempel en de werkelijke waarden van de vorige dag weergeven.
* Bewerk of verwijder deze.
* **Schakel** of **inschakelen** deze als u wilt tijdelijk stoppen of te hervatten ontvangen van meldingen voor deze waarschuwing.


## <a name="sql-database-alert-values"></a>Waarschuwing waarden SQL-Database

| Resourcetype | Naam van meetwaarde | Beschrijvende naam | Samenvoegingstype | Minimale waarschuwing tijdvenster|
| --- | --- | --- | --- | --- |
| SQL-database | cpu_percent | CPU-percentage | Gemiddeld | 5 minuten |
| SQL-database | physical_data_read_percent | Gegevens-I/O-percentage | Gemiddeld | 5 minuten |
| SQL-database | log_write_percent | Percentage van logboek-i/o | Gemiddeld | 5 minuten |
| SQL-database | dtu_consumption_percent | DTU-percentage | Gemiddeld | 5 minuten |
| SQL-database | opslag | Totale grootte | Maximum | 30 minuten |
| SQL-database | connection_successful | Geslaagde verbindingen | Totaal | 10 minuten |
| SQL-database | connection_failed | Mislukte verbindingen | Totaal | 10 minuten |
| SQL-database | blocked_by_firewall | Geblokkeerd door een Firewall | Totaal | 10 minuten |
| SQL-database | impasse | Impassen | Totaal | 10 minuten |
| SQL-database | storage_percent | Databaseomvangpercentage | Maximum | 30 minuten |
| SQL-database | xtp_storage_percent | In het geheugen OLTP opslag percent(Preview) | Gemiddeld | 5 minuten |
| SQL-database | workers_percent | Percentage van de werknemers | Gemiddeld | 5 minuten |
| SQL-database | sessions_percent | Percentage van sessies | Gemiddeld | 5 minuten |
| SQL-database | dtu_limit | DTU limiet | Gemiddeld | 5 minuten |
| SQL-database | dtu_used | DTU gebruikt | Gemiddeld | 5 minuten |
||||||
| Elastische groep | cpu_percent | CPU-percentage | Gemiddeld | 10 minuten |
| Elastische groep | physical_data_read_percent | Gegevens-I/O-percentage | Gemiddeld | 10 minuten |
| Elastische groep | log_write_percent | Percentage van logboek-i/o | Gemiddeld | 10 minuten |
| Elastische groep | dtu_consumption_percent | DTU-percentage | Gemiddeld | 10 minuten |
| Elastische groep | storage_percent | Opslagpercentage | Gemiddeld | 10 minuten |
| Elastische groep | workers_percent | Percentage van de werknemers | Gemiddeld | 10 minuten |
| Elastische groep | eDTU_limit | limiet voor eDTU | Gemiddeld | 10 minuten |
| Elastische groep | storage_limit | Opslaglimiet bereikt | Gemiddeld | 10 minuten |
| Elastische groep | eDTU_used | eDTU gebruikt | Gemiddeld | 10 minuten |
| Elastische groep | storage_used | Gebruikte opslag | Gemiddeld | 10 minuten |
||||||               
| SQL data warehouse | cpu_percent | CPU-percentage | Gemiddeld | 10 minuten |
| SQL data warehouse | physical_data_read_percent | Gegevens-I/O-percentage | Gemiddeld | 10 minuten |
| SQL data warehouse | opslag | Totale grootte | Maximum | 10 minuten |
| SQL data warehouse | connection_successful | Geslaagde verbindingen | Totaal | 10 minuten |
| SQL data warehouse | connection_failed | Mislukte verbindingen | Totaal | 10 minuten |
| SQL data warehouse | blocked_by_firewall | Geblokkeerd door een Firewall | Totaal | 10 minuten |
| SQL data warehouse | service_level_objective | Serviceniveaudoelstelling van de database | Totaal | 10 minuten |
| SQL data warehouse | dwu_limit | dwu-limiet | Maximum | 10 minuten |
| SQL data warehouse | dwu_consumption_percent | DWU-percentage | Gemiddeld | 10 minuten |
| SQL data warehouse | dwu_used | DWU gebruikt | Gemiddeld | 10 minuten |
||||||


## <a name="next-steps"></a>Volgende stappen
* [Een overzicht van Azure monitoring](../monitoring-and-diagnostics/monitoring-overview.md) met inbegrip van de typen gegevens u kunt verzamelen en controleren.
* Meer informatie over [configureren van webhooks in waarschuwingen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Ophalen van een [overzicht van diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) en gedetailleerde hoge frequentie metrische gegevens verzamelen over uw service.
* Ophalen van een [overzicht van metrische gegevens verzameling](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) om ervoor te zorgen dat uw service beschikbaar is en reageert.
