---
title: "Prijscategorieën in Azure MySQL-Database"
description: "In dit artikel beschrijft de prijscategorieën in Azure-Database voor MySQL."
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6bd24da05c337a902ce0e4a2b9acf22a809eb653
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Azure Prijscategorieën MySQL-Database

Een Azure-Database voor de MySQL-server kunnen worden gemaakt in een van drie verschillende Prijscategorieën - Basic, algemeen gebruik en geoptimaliseerd voor geheugen. De Prijscategorieën worden onderscheiden door de hoeveelheid compute in vCores die kunnen worden ingericht, geheugen per vCore en de opslagtechnologie gebruikt voor het opslaan van de gegevens. Alle resources worden ingericht op het niveau van de MySQL-server. Een server kan een of meer databases hebben.

|    | **Basic** | **Voor algemene doeleinden** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| COMPUTE generatie | Gen 4, 5 Gen | Gen 4, 5 Gen | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16, 32 |
| Geheugen per vCore | 1x | 2x Basic | 2 x voor algemene doeleinden |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 1 TB | 5 GB tot 1 TB |
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| De back-up bewaarperiode database | 7 - 35 dagen | 7 - 35 dagen | 7 - 35 dagen |

De volgende tabel kan worden gebruikt als een beginpunt voor het kiezen van een prijscategorie:

| Prijscategorie | Beoogde workloads |
|:-------------|:-----------------|
| Basic | Werkbelastingen vereisen lichte berekenings- en i/o-prestaties. Voorbeelden zijn onder meer servers die worden gebruikt voor ontwikkeling of tests of kleinschalige, onregelmatig gebruikte toepassingen. |
| Algemeen doel | De meeste zakelijke workloads taakverdeling reken- en geheugencapaciteit met schaalbare i/o-doorvoer vereisen. Voorbeelden zijn server voor het hosten van Web en mobiele apps en andere bedrijfstoepassingen.|
| Geoptimaliseerd geheugen | Hoge prestaties database werkbelastingen in het geheugen prestaties vereisen voor snellere verwerking van transacties en hogere gelijktijdigheid van taken. Voorbeelden zijn server voor de verwerking van realtime gegevens en hoge prestaties transactionele of analytische apps.|

Nadat u een server hebt gemaakt, kan het aantal vCores worden gewijzigd omhoog of omlaag binnen enkele seconden. U kunt de hoeveelheid opslag van en de back-up bewaarperiode omhoog of omlaag zonder uitvaltijd voor de toepassing ook afzonderlijk aanpassen. Zie de vergroten/verkleinen sectie hieronder voor meer informatie.

## <a name="compute-generations-vcores-and-memory"></a>COMPUTE generaties, vCores en geheugen

COMPUTE-bronnen worden gegeven als vCores, die de logische CPU van de onderliggende hardware vertegenwoordigt. Op dit moment twee compute generaties Gen 4 en 5 Gen, krijgt u kunt kiezen uit. Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz-processors. Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2.3 GHz-processors.

Elke vCore is afhankelijk van de prijscategorie ingericht met een specifieke hoeveelheid geheugen. Wanneer u vergroten of het aantal vCores voor uw server verkleinen, wordt het geheugen verhoogt of verlaagt proportioneel. De laag voor algemene doeleinden biedt tweemaal de hoeveelheid geheugen per vCore vergeleken met de basisstaffel. De laag geoptimaliseerd voor geheugen bevat dubbele de hoeveelheid geheugen ten opzichte van de laag voor algemene doeleinden.

## <a name="storage"></a>Storage

De opslag die u inricht is de hoeveelheid opslagcapaciteit beschikbaar is voor uw Azure-Database voor MySQL-server. De opslag wordt gebruikt voor de databasebestanden, tijdelijke bestanden, transactielogboeken en de server MySQL Logboeken. De totale hoeveelheid opslag die u inricht definieert ook de i/o-capaciteit die beschikbaar is op uw server:

|    | **Basic** | **Voor algemene doeleinden** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 1 TB | 5 GB tot 1 TB |
| Maximale grootte van de verhoging | 1 GB | 1 GB | 1 GB |
| IOPS | Variabele |3 IOPS/GB<br/>Min 100 IOP's | 3 IOPS/GB<br/>Min 100 IOP's |

Extra opslagcapaciteit kan worden toegevoegd tijdens en na het maken van de server. De laag Basic biedt geen garantie van een IOPS. In het algemeen en de geoptimaliseerd voor geheugen Prijscategorieën schalen het aantal IOPS dat met de opslaggrootte van de ingerichte in een ratio van 3:1.

U kunt uw i/o-verbruik in de Azure-portal of met behulp van Azure CLI-opdrachten kunt bewaken. De relevante meetgegevens voor het bewaken van zijn [opslaglimiet bereikt, opslagpercentage opslag gebruikt en i/o-procent](concepts-monitoring.md).

## <a name="backup"></a>Back-up maken

De service wordt automatisch een back-ups van uw server. De minimale bewaarperiode voor back-ups is zeven dagen. U kunt een bewaarperiode van maximaal 35 dagen instellen. De bewaarperiode kan worden aangepast op elk gewenst moment tijdens de levensduur van de server. U kunt kiezen tussen lokaal redundante en geografisch redundante back-ups. Geografisch redundante back-ups worden ook opgeslagen in de [regio geo gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) van de regio in de server wordt gemaakt. Dit biedt een niveau van bescherming in het geval van een noodgeval. Ook krijgt u de mogelijkheid uw server naar een andere Azure-regio waarin de service beschikbaar met geografisch redundante back-ups is herstellen. Houd er rekening mee dat het is niet mogelijk om te wijzigen tussen de twee opties voor back-upopslag zodra de server is gemaakt.

## <a name="scale-resources"></a>Resources omhoog/omlaag schalen

U kunt de vCores, de hoeveelheid opslag- en back-up bewaarperiode onafhankelijk wijzigen na het maken van uw server. U kunt de prijscategorie of het type back-upopslag niet wijzigen nadat een server is gemaakt. vCores en de back-up bewaarperiode kunnen worden geschaald omhoog of omlaag. De grootte van de opslagruimte kan alleen worden verhoogd. Schalen van de resources kan worden gedaan door middel van de portal of Azure CLI. Een voorbeeld voor het schalen met CLI kunt u vinden [hier](scripts/sample-scale-server.md).

Wanneer u het aantal vCores wijzigt, wordt een kopie van de oorspronkelijke server gemaakt met de nieuwe compute-toewijzing. Nadat de nieuwe server actief is, worden verbindingen via overgeschakeld naar de nieuwe server. Tijdens het korte moment wanneer het systeem overgeschakeld naar de nieuwe server, kunnen geen nieuwe verbindingen tot stand worden gebracht en alle niet-doorgevoerde transacties teruggedraaid. Dit venster verschilt, maar in de meeste gevallen is minder dan een minuut.

Schaalbaarheid van opslag en het wijzigen van de back-up bewaarperiode zijn true online bewerkingen. Er is geen downtime of gevolgen voor uw toepassing. Zoals IOPS met de grootte van de ingerichte opslag uitbreiden, kunt u het aantal IOPS dat beschikbaar is op uw server door te schalen opslag van verhogen.

## <a name="pricing"></a>Prijzen

Controleer de service [pagina met prijzen](https://azure.microsoft.com/pricing/details/mysql/) prijzen voor de meest actuele informatie. Om te zien wat de gewenste configuratie kosten de [Azure-portal](https://portal.azure.com/#create/Microsoft.MySQLServer) bevat de maandelijkse kosten in de **prijscategorie** tabblad op basis van de opties die u hebt geselecteerd. Als u niet een Azure-abonnement hebt, kunt u de Azure prijscategorie Rekenmachine een geschatte prijs ophalen. Ga naar de [Azure prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/) website, klikt u vervolgens op **items toevoegen**, vouw de **Databases** categorie, en kies **Azure Database voor MySQL** voor het aanpassen van de opties.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [een MySQL-server maken in de portal](howto-create-manage-server-portal.md)
- Meer informatie over hoe [bewaken en schalen van een Azure-Database voor de MySQL-server met Azure CLI](scripts/sample-scale-server.md)
- Meer informatie over de [Service-beperkingen](concepts-limits.md)
