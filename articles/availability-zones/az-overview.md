---
title: Overzicht van de beschikbaarheid van Zones | Microsoft Docs
description: Dit artikel bevat een overzicht van beschikbaarheid Zones in Azure.
services: 
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 9274cb1fc490ab61e962e8a7918a9c44976dc755
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Overzicht van beschikbaarheid Zones in Azure (Preview)

Beschikbaarheid Zones helpen u te beschermen tegen storingen datacenter-niveau. Ze bevinden zich in een Azure-regio en elk een eigen onafhankelijke heeft energie-bron-, netwerk- en koeling. Tolerantie, zodat is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke en logische scheiding van beschikbaarheid Zones binnen een regio beschermt toepassingen en gegevens tegen zoneniveau fouten. 

![Conceptueel overzicht van één zone tijdelijk niet beschikbaar in een regio](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regio's die ondersteuning bieden voor beschikbaarheid Zones

- VS - oost 2
- VS Centraal
- West-Europa
- Frankrijk - centraal

## <a name="services-that-support-availability-zones"></a>Services die ondersteuning bieden voor beschikbaarheid Zones

De Azure-services die ondersteuning bieden voor beschikbaarheid Zones zijn:

- Virtuele Linux-machines
- Virtuele Windows-machines
- Schaalsets voor virtuele machines
- Beheerde schijven
- Load Balancer
- Openbaar IP-adres
- Zone-Redundant Storage
- SQL Database

## <a name="get-started-with-the-availability-zones-preview"></a>Aan de slag met de preview beschikbaarheid Zones

De evaluatieversie van de beschikbaarheid van Zones is beschikbaar in de VS-Oost 2, ons centraal, West-Europa en Frankrijk centrale regio's voor de specifieke Azure-services. 

1. [Aanmelden voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). 
2. Aanmelden bij uw Azure-abonnement.
3. Kies een regio die ondersteuning biedt voor beschikbaarheid Zones.
4. Gebruik een van de volgende koppelingen om te starten met behulp van de beschikbaarheid van Zones met uw service. 
    - [Een virtuele machine maken](../virtual-machines/windows/create-portal-availability-zone.md)
    - [Een virtuele-machineschaalset maken](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
    - [Voeg een schijf worden beheerd met behulp van PowerShell toe](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [Load balancer](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>Volgende stappen
- [Snelstartsjablonen](http://aka.ms/azqs)
