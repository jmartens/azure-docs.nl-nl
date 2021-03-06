---
title: Azure Site Recovery probleemoplossing van VMware naar Azure | Microsoft Docs
description: Fouten bij het repliceren van virtuele machines in Azure oplossen.
services: site-recovery
author: anoopkv
manager: gauravd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: anoopkv
ms.openlocfilehash: a03766f8a22399f7d72a01f8d744bfd1cef90ac3
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2018
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Oplossen van problemen met de installatie van de Mobility-Service push

Dit artikel worden veelvoorkomende problemen die u mee te maken mogelijk wanneer u probeert te installeren van Azure Site Recovery Mobility-Service op de bronserver beveiliging in te schakelen.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Fout 78007 - de gevraagde bewerking kan niet worden voltooid.
Deze fout kan worden gegenereerd door de service om verschillende redenen. Kies de bijbehorende providerfout probleem verder oplossen.

* [Fout 95103](#error-95103---protection-could-not-be-enabled-ep0854) 
* [Fout 95105](#error-95105---protection-could-not-be-enabled-ep0856) 
* [Fout 95107](#error-95107---protection-could-not-be-enabled-ep0858) 
* [Fout 95108](#error-95108---protection-could-not-be-enabled-ep0859) 
* [Fout 95117](#error-95117---protection-could-not-be-enabled-ep0865) 
* [Fout 95213](#error-95213---protection-could-not-be-enabled-ep0874) 
* [Fout 95224](#error-95224---protection-could-not-be-enabled-ep0883) 
* [Fout 95265](#error-95265---protection-could-not-be-enabled-ep0902) 


## <a name="error-95105---protection-could-not-be-enabled-ep0856"></a>Fout 95105 - beveiliging kan niet worden ingeschakeld (EP0856)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95105 </br>**Bericht:** Push-installatie van Mobility-Service naar de bronmachine is mislukt met foutcode **EP0856**. <br> Beide **bestands- en printerdeling** is niet toegestaan op de bron-machines of er zijn problemen met de netwerkconnectiviteit tussen de processerver en de bronmachine netwerk.| **Bestands- en printerdeling** is niet ingeschakeld. | Toestaan dat **bestands- en printerdeling** in Windows Firewall op de bronmachine. Op de bronmachine onder **Windows Firewall** > **toestaan van een app of functie via Firewall**, selecteer **bestands- en printerdeling voor alle profielen**. </br> Controleer daarnaast de volgende vereisten om met succes de pushinstallatie te voltooien.<br> Lees meer over [problemen met het oplossen van WMI](#troubleshoot-wmi-issues).


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Fout 95107 - beveiliging kan niet worden ingeschakeld (EP0858)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95107 </br>**Bericht:** Push-installatie van Mobility-Service naar de bronmachine is mislukt met foutcode **EP0858**. <br> Ofwel de referenties die zijn opgegeven voor het installeren van de Mobility-Service zijn onjuist of het gebruikersaccount heeft onvoldoende bevoegdheden. | Voor het installeren van de Mobility-Service op de bronmachine opgegeven gebruikersreferenties zijn onjuist. | Zorg dat de opgegeven gebruikersreferenties voor de bronmachine op de configuratieserver juist zijn. <br> Als u wilt toevoegen of bewerken van gebruikersreferenties, Ga naar de configuratieserver en selecteer **Cspsconfigtool** > **-account beheren**. </br> Controleer daarnaast de volgende [vereisten](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) push-installatie is voltooid.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Fout 95117 - beveiliging kan niet worden ingeschakeld (EP0865)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95117 </br>**Bericht:** Push-installatie van Mobility-Service naar de bronmachine is mislukt met foutcode **EP0865**. <br> Op de bronmachine wordt niet uitgevoerd of er zijn problemen met de netwerkconnectiviteit tussen de processerver en de bronmachine. | Problemen met de netwerkconnectiviteit tussen de processerver en de bronserver. | Controleer de verbinding tussen de processerver en de bronserver. </br> Controleer daarnaast de volgende [vereisten](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) push-installatie is voltooid.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Fout 95103 - beveiliging kan niet worden ingeschakeld (EP0854)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95103 </br>**Bericht:** Push-installatie van Mobility-Service naar de bronmachine is mislukt met foutcode **EP0854**. <br> De Windows Management Instrumentation (WMI) is niet toegestaan op de bronmachine of er zijn problemen met de netwerkconnectiviteit tussen de processerver en de bronmachine.| WMI wordt in Windows Firewall geblokkeerd. | Kan WMI in Windows Firewall. Onder **Windows Firewall** > **toestaan van een app of functie via Firewall**, selecteer **WMI voor alle profielen**. </br> Controleer daarnaast de volgende [vereisten](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) push-installatie is voltooid.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Fout 95213 - beveiliging kan niet worden ingeschakeld (EP0874)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95213 </br>**Bericht:** installatie van Mobility-Service naar de bronmachine % SourceIP; is mislukt met foutcode **EP0874**. <br> | De versie van het besturingssysteem op de bronmachine wordt niet ondersteund. <br>| Zorg ervoor dat de bronmachine versie van het besturingssysteem wordt ondersteund. Lees de [ondersteuningsmatrix](https://aka.ms/asr-os-support). </br> Controleer daarnaast de volgende [vereisten](https://aka.ms/pushinstallerror) push-installatie is voltooid.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Fout 95108 - beveiliging kan niet worden ingeschakeld (EP0859)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95108 </br>**Bericht:** Push-installatie van Mobility-Service naar de bronmachine is mislukt met foutcode **EP0859**. <br>| Ofwel de referenties die zijn opgegeven voor het installeren van de Mobility-Service zijn onjuist of het gebruikersaccount heeft onvoldoende bevoegdheden. <br>| Zorg ervoor dat de opgegeven referenties zijn de **hoofdmap** van accountreferenties. Als u wilt toevoegen of bewerken van gebruikersreferenties, Ga naar de configuratieserver en selecteer de **Cspsconfigtool** snelkoppelingspictogram op het bureaublad. Selecteer **-account beheren** toevoegen of bewerken van referenties.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Fout 95265 - beveiliging kan niet worden ingeschakeld (EP0902)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95265 </br>**Bericht:** Push-installatie van Mobility-Service naar de bronmachine is voltooid, maar de bronmachine moet opnieuw worden gestart voor sommige wijzigingen in het systeem te laten treden. <br>| Een oudere versie van de Mobility-Service is al geïnstalleerd op de server.| Replicatie van de virtuele machine naadloos wordt voortgezet.<br> De server opnieuw opgestart tijdens uw volgende onderhoudsvenster voordelen van de nieuwe verbeteringen in de Mobility-Service ophalen.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Fout 95224 - beveiliging kan niet worden ingeschakeld (EP0883)

**Foutcode** | **Mogelijke oorzaken** | **Fout-specifieke aanbevelingen**
--- | --- | ---
95224 </br>**Bericht:** Push-installatie van Mobility-Service naar de bronmachine % SourceIP; is mislukt met foutcode **EP0883**. Systeem opnieuw is opgestart vanaf een eerdere installatie of update is in behandeling.| Het systeem is niet opnieuw opgestart wanneer u een oudere of incompatibele versie van de Mobility-Service verwijderen.| Zorg ervoor dat er geen versie van de Mobility-Service op de server bestaat. <br> De server opnieuw opgestart en de taak beveiliging inschakelen opnieuw.|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Resource toe aan het oplossen van problemen voor push-installatie

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Bestand problemen op te delen problemen met afdrukken
* [In- of uitschakelen van bestandsdeling met Groepsbeleid](https://technet.microsoft.com/library/cc754359(v=ws.10).aspx)
* [Bestands- en printerdeling via Windows Firewall inschakelen](https://technet.microsoft.com/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>Problemen met WMI
* [WMI-basistest](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [WMI-probleemoplossing](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Het oplossen van problemen met WMI-scripts en WMI-services](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over hoe](tutorial-vmware-to-azure.md) voor het instellen van herstel na noodgevallen voor virtuele VMware-machines.