---
title: Azure SQL Database multitenant app voorbeeld - Wingtip SaaS | Microsoft Docs
description: Meer informatie over met behulp van een multitenant voorbeeldtoepassing die gebruikmaakt van Azure SQL Database, het Wingtip SaaS-voorbeeld
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: 451e1fc87fc5f626e78760d8cd5c4115ea02bb0d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="introduction-to-a-multi-tenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Inleiding tot een multitenant SaaS-app die gebruikmaakt van de database per tenant patroon met SQL-Database

De *Wingtip SaaS* toepassing is een voorbeeld van multitenant-app. De app gebruikmaakt van de database per tenant, het patroon van de SaaS-toepassing, om meerdere tenants te verwerken. De app gepresenteerd functies van Azure SQL Database waarmee SaaS-scenario's met verschillende SaaS-ontwerp- en patronen. Als u snel gebruiksklaar, wordt de Wingtip SaaS-app implementeert in minder dan vijf minuten!

Bron en het beheer toepassingsscripts zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub-opslagplaats. Voordat u begint, bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de Wingtip Tickets management scripts deblokkeren.

## <a name="application-architecture"></a>Toepassingsarchitectuur

De app Wingtip SaaS maakt gebruik van de database per tenant en elastische pools SQL gebruikt om het te optimaliseren. Voor het inrichten en de toewijzing van tenants tot hun gegevens, wordt de catalogusdatabase van een gebruikt. De kern Wingtip SaaS-toepassing maakt gebruik van een groep met drie voorbeeld tenants, plus de catalogusdatabase. Veel van de Wingtip SaaS zelfstudies tot invoegtoepassingen voor de initiële implementatie leiden is voltooid, door de introductie van analytische databases tussen meerdere databases Schemabeheer, enzovoort.


![Wingtip SaaS-architectuur](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Tijdens het doorlopen van de zelfstudies en werken met de app, is het belangrijk dat u zich richten op de SaaS-patronen die betrekking heeft op de gegevenslaag. Met andere woorden, kijkt u vooral naar de gegevenslaag en niet zozeer naar de app zelf. Informatie over de implementatie van deze SaaS patronen essentieel is voor het implementeren van deze patronen in uw toepassingen bij het beoordelen van de benodigde wijzigingen voor uw specifieke bedrijfsvereisten.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL Database Wingtip SaaS-zelfstudies

Nadat de app is geïmplementeerd, gebruik de volgende zelfstudies die voort op de eerste implementatie bouwen. Deze zelfstudies verkennen algemene SaaS-patronen die van de ingebouwde functies van SQL-Database, SQL Data Warehouse en andere Azure-services gebruikmaken. Zelfstudies bevatten PowerShell-scripts, met gedetailleerde uitleg die aanzienlijk te vereenvoudigen, begrijpen en implementeren van de dezelfde patronen voor SaaS-beheer in uw toepassingen.


| Zelfstudie | Beschrijving |
|:--|:--|
| [Richtlijnen en tips voor het voorbeeld van Azure SQL Database multitenant SaaS-app](saas-tenancy-wingtip-app-guidance-tips.md) | **BEGIN HIER!** Downloaden en uitvoeren van PowerShell-scripts voor het voorbereiden van de onderdelen van de toepassing. |
|[Implementeren en Verken de Wingtip SaaS-toepassing](saas-dbpertenant-get-started-deploy.md)|  Implementeer en Verken de Wingtip SaaS-toepassing naar uw Azure-abonnement. |
|[Inrichten en catalogus tenants](saas-dbpertenant-provision-and-catalog.md)| Informatie over hoe de toepassing verbinding maakt met tenants met behulp van een catalogusdatabase, en hoe tenants in de catalogus worden toegewezen aan hun gegevens. |
|[Bewaken en beheren van prestaties](saas-dbpertenant-performance-monitoring.md)| Informatie over het gebruik van de controlemogelijkheden van SQL-Database en het instellen van meldingen wanneer drempelwaarden worden overschreden. |
|[Monitor met logboekanalyse (OMS)](saas-dbpertenant-log-analytics.md) | Meer informatie over het gebruik van [logboekanalyse](../log-analytics/log-analytics-overview.md) voor het bewaken van grote hoeveelheden bronnen, tussen meerdere pools. |
|[Herstellen van een enkele tenant](saas-dbpertenant-restore-single-tenant.md)| Ontdek hoe u een tenant-database naar een eerder tijdstip herstellen. Stappen om een parallelle database, waarbij de bestaande tenant-database online te herstellen, zijn ook opgenomen. |
|[Tenant-databaseschema beheren](saas-tenancy-schema-management.md)| Informatie over het schema, en het bijwerken van referentiegegevens, voor alle databases van de tenant. |
|[Cross-tenant gedistribueerde query's uitvoeren](saas-tenancy-cross-tenant-reporting.md) | Maakt de database van een ad-hoc analytics en realtime gedistribueerde query's uitvoeren op alle tenants.  |
|[Analytics worden uitgevoerd op gegevens van de uitgepakte tenant](saas-tenancy-tenant-analytics.md) | Tenant-gegevens ophalen naar een analytics database of de data warehouse voor offline analytics query's. |


## <a name="next-steps"></a>Volgende stappen

- [Algemene richtlijnen en tips bij het implementeren en in het voorbeeld Wingtip Tickets SaaS-app](saas-tenancy-wingtip-app-guidance-tips.md)

- [De Wingtip SaaS-toepassing implementeren](saas-dbpertenant-get-started-deploy.md)
