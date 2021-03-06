---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Java | Microsoft Docs
description: Aan de slag verzenden naar Event Hubs met behulp van Java
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 5c8c24e1f168be4b46ccfdb1d0c268866fc8ff7d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Gebeurtenissen verzenden naar Azure Event Hubs met Java

## <a name="introduction"></a>Inleiding
Event Hubs is een zeer schaalbaar systeem die kan worden miljoenen gebeurtenissen per seconde opnemen voor het inschakelen van een toepassing te verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Zodra verzameld in een event hub, kunt u transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslagcluster.

Zie voor meer informatie de [overzicht van Event Hubs][Event Hubs overview].

Deze zelfstudie laat zien hoe gebeurtenissen verzenden naar een event hub met behulp van een Java-consoletoepassing. Zie voor het ontvangen van gebeurtenissen met de bibliotheek Java Event Processor Host [in dit artikel](event-hubs-java-get-started-receive-eph.md), of klik op de juiste ontvangende taal in de tabel links van inhoud.

Om deze zelfstudie hebt voltooid, moet u het volgende:

* Een Java-ontwikkelomgeving. Voor deze zelfstudie gaan we ervan uit [Eclipse](https://www.eclipse.org/).
* Een actief Azure-account. <br/>Als u geen account hebt, kunt u binnen een paar minuten een gratis account maken. Zie <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Gratis proefversie van Azure</a> voor meer informatie.

## <a name="send-messages-to-event-hubs"></a>Berichten verzenden naar Event Hubs
De Java-clientbibliotheek voor Event Hubs is beschikbaar voor gebruik in Maven-projecten uit de [Maven centrale opslagplaats](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). U kunt verwijzen naar deze bibliotheek met behulp van de volgende declaratie van afhankelijkheid in het bestand Maven-project:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Voor verschillende soorten build-omgevingen, kunt u de meest recente uitgebrachte JAR-bestanden uit expliciet verkrijgen de [Maven centrale opslagplaats](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Voor de uitgever van een eenvoudige gebeurtenis en importeer de *com.microsoft.azure.eventhubs* -pakket voor de klassen van Event Hubs-client en de *com.microsoft.azure.servicebus* , zoals het pakket voor hulpprogramma klassen Algemene uitzonderingen die worden gedeeld met de Azure Service Bus messaging-client. 

Maak voor het volgende voorbeeld eerst een nieuw Maven-project voor een console/shell-toepassing in uw favoriete Java-ontwikkelomgeving. Naam van de klasse `Send`.     

```java
import java.io.IOException;
import java.nio.charset.*;
import java.util.*;

import com.microsoft.azure.eventhubs.*;

public class Send
{
    public static void main(String[] args) 
            throws EventHubException, IOException
    {
```

De namen van de naamruimte en event hub vervangen door de waarden die worden gebruikt bij het maken van de event hub.

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Vervolgens maakt u een enkelvoudige gebeurtenis door het omzetten van een tekenreeks in de UTF-8-byte-codering. Vervolgens maakt u een nieuw exemplaar van de Event Hubs-client uit de verbindingsreeks en verzend het bericht.   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    
    // close the client at the end of your program
    ehClient.closeSync();
    }
}

``` 

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [De eventprocessorhost gebeurtenissen ontvangen](event-hubs-java-get-started-receive-eph.md)
* [Event Hubs-overzicht][Event Hubs overview]
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
