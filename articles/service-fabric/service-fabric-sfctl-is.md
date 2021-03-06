---
title: Azure Service Fabric CLI - sfctl is | Microsoft Docs
description: Beschrijving van de Service Fabric-CLI sfctl opdrachten is.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 02/22/2018
ms.author: ryanwi
ms.openlocfilehash: d3a4c7a308bdf7132cebffc13a5e7214ec73eaf2
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="sfctl-is"></a>sfctl is
Vragen en opdrachten naar de service infrastructuur verzenden.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
|    opdracht| Een administratieve opdracht uitvoeren op het opgegeven exemplaar van de Service-infrastructuur.|
|    query  | Hiermee wordt een alleen-lezen-query op het opgegeven infrastructuur service-exemplaar.|


## <a name="sfctl-is-command"></a>sfctl is opdracht
Een administratieve opdracht uitvoeren op het opgegeven exemplaar van de Service-infrastructuur.

Deze API biedt voor clusters met een of meer exemplaren van de Service-infrastructuur is geconfigureerd, een manier voor het verzenden van infrastructuur-specifieke opdrachten op een bepaalde instantie van de Service-infrastructuur. Beschikbare opdrachten en hun bijbehorende antwoord indelingen variëren afhankelijk van de infrastructuur waarop het cluster wordt uitgevoerd. Deze API biedt ondersteuning voor het Service Fabric-platform; Dit is niet bedoeld voor rechtstreeks gebruik vanuit uw code. 

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --opdracht [vereist]| De tekst van de opdracht kan worden aangeroepen. De inhoud van de opdracht is specifiek voor infrastructuur. |
| --service-id     | De identiteit van de infrastructuur-service. De ID is de volledige naam van de service infrastructuur zonder de "fabric:' URI-schema. Deze parameter is alleen vereist voor clusters met meer dan één exemplaar van infrastructuur-service wordt uitgevoerd.|
| --time-out -t     | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug          | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h        | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o      | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query          | JMESPath queryreeks. Zie voor meer informatie over en voorbeelden http://jmespath.org/.|
| --uitgebreide        | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="sfctl-is-query"></a>sfctl is-query
Hiermee wordt een alleen-lezen-query op het opgegeven infrastructuur service-exemplaar.

Deze API biedt voor clusters met een of meer exemplaren van de Service-infrastructuur is geconfigureerd, een manier voor het verzenden van infrastructuur-query's op een bepaalde instantie van de Service-infrastructuur. Beschikbare opdrachten en hun bijbehorende antwoord indelingen variëren afhankelijk van de infrastructuur waarop het cluster wordt uitgevoerd. Deze API biedt ondersteuning voor het Service Fabric-platform; Dit is niet bedoeld voor rechtstreeks gebruik vanuit uw code.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --opdracht [vereist]| De tekst van de opdracht kan worden aangeroepen. De inhoud van de opdracht is specifiek voor infrastructuur.|
| --service-id     | De identiteit van de infrastructuur-service. De ID is de volledige naam van de service infrastructuur zonder de "fabric:' URI-schema. Deze parameter is alleen vereist voor clusters met meer dan één exemplaar van infrastructuur-service wordt uitgevoerd.|
| --time-out -t     | Server time-out in seconden.  Standaard: 60.|

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --debug          | Vergroot de uitgebreidheid logboekregistratie om weer te geven van dat alle fouten opsporen in Logboeken.|
| --help -h        | Deze help-bericht en afsluiten weergeven.|
| --uitvoer -o      | De indeling van de uitvoer.  Toegestane waarden: json, jsonc, tabel, tsv.  Standaard: json.|
| --query          | JMESPath queryreeks. Zie http://jmespath.org/ voor meer informatie.|
| --uitgebreide        | Logboekregistratie uitgebreidheid verhogen. Gebruik--foutopsporing voor volledige foutopsporingslogboeken.|

## <a name="next-steps"></a>Volgende stappen
- [Instellen van](service-fabric-cli.md) de Service Fabric-CLI.
- Informatie over het gebruik van de Service Fabric CLI met behulp van de [steekproef scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).