---
title: VMware naar Azure replicatie Azure Site Recovery-architectuur | Microsoft Docs
description: In dit artikel biedt een overzicht van de onderdelen en gebruikt wanneer de lokale virtuele VMware-machines repliceren naar Azure met Azure Site Recovery-architectuur
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: raynew
ms.openlocfilehash: 4dd9d4f5f26d70f9130f48e2bf40ce71943a6c6d
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware naar Azure replicatie-architectuur

In dit artikel beschrijft de architectuur en processen die worden gebruikt wanneer u replicatie, failover en herstellen van virtuele VMware-machines (VM's) tussen een on-premises VMware-site en Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Architectuuronderdelen

De volgende tabel en afbeelding vindt een weergave op hoog niveau van de onderdelen die worden gebruikt voor VMware-replicatie naar Azure.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Azure** | Een Azure-abonnement, Azure Storage-account en Azure-netwerk. | Gerepliceerde gegevens van de lokale virtuele machines wordt opgeslagen in het opslagaccount. Virtuele machines in Azure worden gemaakt met de gerepliceerde gegevens wanneer u een failover van on-premises naar Azure uitvoert. De Azure-VM's maken verbinding met het virtuele Azure-netwerk wanneer ze worden gemaakt.
**Configuratie-servercomputer** | Een één on-premises machine. Het is raadzaam dat u uitvoert als een VMware-VM die kan worden geïmplementeerd vanaf een gedownloade OVF-sjabloon.<br/><br/> De machine wordt uitgevoerd voor alle on-premises Site Recovery onderdelen, waaronder de configuratieserver, de processerver en de hoofddoelserver. | **Configuratieserver**: coördineert de communicatie tussen de on-premises en Azure en beheert gegevensreplicatie.<br/><br/> **Processerver**: standaard geïnstalleerd op de configuratieserver. Het ontvangt replicatiegegevens; Deze optimaliseert met caching, compressie en codering; en verzendt het naar Azure Storage. Azure Site Recovery Mobility-Service de processerver ook geïnstalleerd op virtuele machines die u repliceren wilt, en de werking van automatische detectie van lokale computers. Naarmate uw implementatie groeit, kunt u het voor afzonderlijke processervers voor het afhandelen van grotere hoeveelheden replicatieverkeer kunt toevoegen.<br/><br/> **Hoofddoelserver**: standaard geïnstalleerd op de configuratieserver. Replicatiegegevens verwerkt tijdens de failback vanuit Azure. U kunt een afzonderlijke hoofddoelserver voor failback toevoegen voor grote implementaties.
**VMware-servers** | Virtuele VMware-machines worden gehost op on-premises vSphere ESXi-servers. U wordt aangeraden een vCenter-server voor het beheren van de hosts. | Tijdens de implementatie van Site Recovery kunt u VMware-servers toevoegen aan de Recovery Services-kluis.
**Gerepliceerde machines** | Mobility-Service is geïnstalleerd op elke VMware VM die u wilt repliceren. | We raden u aan automatische installatie van de processerver. U kunt ook de service handmatig te installeren of gebruiken van een methode geautomatiseerde implementatie, zoals System Center Configuration Manager.

**VMware naar Azure-architectuur**

![Onderdelen](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Replicatieproces

1. Voorbereiden op Azure-resources en on-premises onderdelen.
2. Geef de bron replicatie-instellingen in de Recovery Services-kluis. Als onderdeel van dit proces voor de lokale configuratie-server instellen. Een voorbereid OVF-sjabloon downloaden en importeren in VMware voor de virtuele machine maken voor het implementeren van deze server als een VMware-VM.
3. Doel replicatie-instellingen opgeven, maak een replicatiebeleid voor en replicatie inschakelen voor uw VMware-machines.
4. Machines repliceren in overeenstemming met het replicatiebeleid en een initiële kopie van de VM-gegevens worden gerepliceerd naar Azure Storage.
5. Nadat de initiële replicatie is voltooid, begint de replicatie van wijzigingen naar Azure. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.

    * Machines communiceren met de configuratieserver op poort 443 voor HTTPS binnenkomend verkeer voor replicatiebeheer.

    * Machines replicatiegegevens verzenden naar de processerver op poort HTTPS 9443 binnenkomende (kan worden aangepast).

    * De configuratieserver coördineert het replicatiebeheer met Azure via de uitgaande HTTPS-poort 443.

    * De processerver ontvangt gegevens van bronmachines, optimaliseert versleutelt en verzendt het naar Azure Storage via poort 443 uitgaand.

    * Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004. Meerdere VM’s worden gebruikt als u meerdere machines groepeert in de replicatiegroepen die crashconsistent en app-consistent herstelpunten delen bij failover. Deze methode is handig als machines dezelfde werkbelasting worden uitgevoerd en moeten consistent zijn.

6. Verkeer wordt gerepliceerd naar Azure storage openbare eindpunten via internet. U kunt ook [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) van Azure ExpressRoute gebruiken. Verkeer via een site-naar-site virtueel particulier netwerk (VPN) van een on-premises site repliceren naar Azure wordt niet ondersteund.


**VMware naar Azure replicatieproces**

![Replicatieproces](./media/concepts-vmware-to-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Failover- en failbackproces

Nadat de replicatie is ingesteld en u een disaster recovery Inzoomen (testfailover) om te controleren of alles werkt zoals verwacht worden uitgevoerd, kunt u een failover en failback uitvoeren als nodig is voor.

1. U kunt één machine failover of herstelplannen failover voor meerdere virtuele machines maken.

2. Wanneer u een failover uitvoert, worden virtuele Azure-machines in Azure-opslag van gerepliceerde gegevens gemaakt.

3. Na activering van de eerste failover doorvoeren u om te starten met het openen van de werkbelasting van de Azure VM.

Als uw primaire on-premises site weer beschikbaar is, kunt u een failback uitvoeren.
1. U moet voor het instellen van een failback-infrastructuur, met inbegrip van:

    * **Tijdelijke processerver in Azure**: als u wilt een failback van Azure, instellen van een virtuele machine van Azure om te fungeren als een processerver voor het afhandelen van replicatie van Azure. U kunt deze virtuele machine verwijderen wanneer de failback is voltooid.

    * **VPN-verbinding**: als u wilt een failback uit, moet u een VPN-verbinding (of ExpressRoute) van het netwerk van Azure naar de lokale site.

    * **Afzonderlijke hoofddoelserver**: standaard wordt de hoofddoelserver die is geïnstalleerd met de configuratieserver op de virtuele machine van de lokale VMware de failback afgehandeld. Als u back grote hoeveelheden verkeer mislukken wilt, een afzonderlijke on-premises hoofddoelserver server instellen voor dit doel.

    * **Failbackbeleid**: als u wilt repliceren naar de on-premises site, hebt u een failbackbeleid nodig. Dit beleid is automatisch gemaakt tijdens het maken van uw replicatiebeleid voor van on-premises naar Azure.
2. Nadat de onderdelen aanwezig is zijn, de failback in drie fasen:

    a. Stap 1: De Azure VM's opnieuw beveiligen zodat ze vanaf Azure repliceren terug naar de lokale VMware-machines.

    b. Stap 2: Een failover naar de lokale site uitvoert.

    c. Stap 3: Nadat werkbelastingen terug zijn uitgevallen, u u replicatie voor de lokale virtuele machines.

**VMware failback vanuit Azure**

![Failback](./media/concepts-vmware-to-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Volgende stappen

Ga als volgt [in deze zelfstudie](tutorial-vmware-to-azure.md) VMware naar Azure replicatie inschakelen.
