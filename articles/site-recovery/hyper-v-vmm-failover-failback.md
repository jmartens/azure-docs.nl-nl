---
title: Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar een secundair datacenter met Site Recovery | Microsoft Docs
description: Informatie over het Hyper-V-machines met uw secundaire on-premises site failover en failback naar primaire site met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: raynew
ms.openlocfilehash: 3740ec9917499f6a1e87905befe86598a18f68e6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Failover en mislukken back-Hyper-V-machines die zijn gerepliceerd naar de locatie van uw secundaire on-premises

De [Azure Site Recovery](site-recovery-overview.md) service beheert en regelt de replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

In dit artikel wordt beschreven hoe failover van een Hyper-V-virtuele machine in een cloud System Center Virtual Machine Manager (VMM) naar een secundaire site voor VMM worden beheerd. Nadat u hebt failover, schakelt u terug naar uw on-premises site wanneer deze beschikbaar is. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Failover van een Hyper-V virtuele machine uit een primaire VMM-cloud naar een secundaire VMM-cloud
> * Beveilig opnieuw vanaf de secundaire site naar de primaire en failback
> * Optioneel beginnen van primaire repliceren naar de secundaire opnieuw

## <a name="failover-and-failback"></a>Failover en failback

Failover en failback bestaat uit drie fasen:

1. **Failover naar de secundaire site**: failover machines van de primaire site naar de secundaire.
2. **Failback van de secundaire site**: virtuele machines repliceren van secundaire op primaire en voer een geplande failover failback.
3. Na de geplande failover moet u eventueel starten vanaf de primaire site repliceren naar de secundaire opnieuw.


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u hebt voltooid, een [disaster recovery inzoomen](hyper-v-vmm-test-failover.md) om te controleren of alles werkt zoals verwacht.
- Zorg ervoor dat de primaire en secundaire VMM-servers zijn verbonden met Site Recovery voor het voltooien van failback.



## <a name="run-a-failover-from-primary-to-secondary"></a>Een failover van primaire naar secundaire uitvoert

U kunt een reguliere of geplande failover uitvoeren voor Hyper-V-machines.

- Gebruik een reguliere failover voor onverwachte storingen. Wanneer u deze failover uitvoert, Site Recovery maakt een virtuele machine in de secundaire site en drijft de. U kunt de failover uitvoeren op een specifiek herstelpunt. Verlies van gegevens kan optreden, afhankelijk van het herstelpunt dat u gebruikt.
- Een geplande failover kan worden gebruikt voor onderhoud, hetzij tijdens de verwachte uitval. Deze optie biedt geen gegevens verloren gaan. Wanneer een geplande failover wordt geactiveerd, worden de Bronmachines afgesloten. Niet-gesynchroniseerde gegevens worden gesynchroniseerd en de failover wordt geactiveerd. 
- 
Deze procedure wordt beschreven hoe een reguliere failover uit te voeren.


1. In **instellingen** > **gerepliceerde items** klikt u op de virtuele machine > **Failover**.
2. In **Failover** Selecteer een **herstelpunt** failover naar. U kunt een van de volgende opties gebruiken:
    - **Meest recente** (standaard): deze optie verwerkt eerst alle gegevens die worden verzonden naar Site Recovery. Het biedt de laagste RPO (beoogd herstelpunt) omdat de replica-VM gemaakt nadat de failover is de gegevens die is gerepliceerd naar de Site Recovery wanneer de failover is geactiveerd.
    - **Meest recente verwerkte**: deze optie is overgenomen van de virtuele machine naar het recentste herstelpunt dat is verwerkt door Site Recovery. Deze optie biedt een laag RTO (beoogde hersteltijd), omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.
    - **Meest recente app-consistente**: deze optie is overgenomen van de virtuele machine naar de meest recente app-consistente herstelpunt verwerkt door Site Recovery. 
3. De versleutelingssleutel is niet relevant is in dit scenario.
4. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor probeert virtuele bronmachines afgesloten voordat de failover. Site Recovery wordt ook proberen te synchroniseren van on-premises gegevens die nog niet nog naar de secundaire site verzonden voordat de failover. Houd er rekening mee dat de failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.
5. U moet nu mogelijk om te zien van de virtuele machine in de secundaire VMM-cloud.
6. Nadat u hebt gecontroleerd dat de virtuele machine **doorvoeren** de failover. Hiermee verwijdert u de beschikbare herstelpunten.

> [!WARNING]
> **Een failover uitgevoerd niet annuleren**: voordat failover wordt gestart, VM-replicatie is gestopt. Als u een failover uitgevoerd, stopt de failover, annuleren, maar de virtuele machine opnieuw won't repliceren.  


## <a name="reprotect-and-fail-back"></a>Beveiligt en een failback uit op

Starten van de secundaire site repliceren naar de primaire en failback naar de primaire site. Nadat de virtuele machines worden uitgevoerd in de primaire site opnieuw, kunt u deze repliceren naar de secundaire site opnieuw.  

1. In **instellingen** > **gerepliceerde items** klikt u op de virtuele machine en schakel **omgekeerde replicatie**. De virtuele machine wordt gestart repliceren naar de primaire site.
2. Klik op de virtuele machine > **geplande Failover**.
3. In **geplande Failover bevestigen**, Controleer of de failoverrichting (van de secundaire VMM-cloud) en selecteert u de bron- en doellocaties. 
4. In **gegevenssynchronisatie**, opgeven hoe u wilt synchroniseren:
    - **Synchroniseren van gegevens voordat failover wordt uitgevoerd (alleen nog deltawijzigingen gesynchroniseerd)**— deze optie wordt de virtuele machine uitvaltijd geminimaliseerd omdat het synchroniseert zonder dat de virtuele machine afgesloten. Dit is wat het doet:
        - Maakt een momentopname van de replica virtuele machine en kopieert deze naar de primaire Hyper-V-host. De replica virtuele machine wordt uitgevoerd.
        - Hiermee sluit u de replica virtuele machine, zodat er geen nieuwe wijzigingen worden uitgevoerd. De uiteindelijke set wijzigingen worden overgebracht naar de primaire site en de virtuele machine in de primaire site wordt gestart.
    - **Synchroniseren van gegevens tijdens de failover alleen (volledige download)**: Gebruik deze optie als u hebt uitgevoerd in de secundaire site gedurende een lange periode. Deze optie is sneller, omdat er meerdere schijfwijzigingen verwacht, en niet besteed tijd aan controlesom voldoende. Deze optie voert het downloaden van een schijf. Het is ook handig wanneer de primaire virtuele machine is verwijderd.
5. De versleutelingssleutel is niet relevant is in dit scenario.
6. Start de failover. U kunt de voortgang van de failover volgen op de **taken** tabblad.
7. Als u hebt geselecteerd voor het synchroniseren van gegevens voordat de failover wordt uitgevoerd, nadat de eerste gegevenssynchronisatie wordt uitgevoerd en u klaar bent om de replica virtuele machine in de secundaire site afsluiten, klikt u op **taken** > geplande failover taaknaam >  **Voltooi de Failover**. Dit de secundaire virtuele machine is afgesloten, verzendt de meest recente wijzigingen aan de primaire site en de primaire virtuele machine wordt gestart.
8. Controleer of de virtuele machine beschikbaar is in de primaire VMM-cloud.
9. De primaire virtuele machine is nu in behandeling doorvoeren. Klik op **doorvoeren**, de failover doorvoeren.
10. Als u wilt repliceren van de primaire virtuele machine naar de secundaire site opnieuw start, schakelt u **omgekeerde replicatie**.


> [!NOTE]
> Omgekeerde replicatie repliceert alleen wijzigingen die sinds de replica virtuele machine is uitgeschakeld en alleen nog deltawijzigingen worden verzonden.

## <a name="next-steps"></a>Volgende stappen
[Bekijk de stap](hyper-v-vmm-disaster-recovery.md) voor Hyper-V-machines repliceren naar een secundaire site.
