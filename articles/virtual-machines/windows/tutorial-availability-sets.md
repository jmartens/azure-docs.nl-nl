---
title: Zelfstudie over beschikbaarheidssets voor virtuele Windows-machines in Azure | Microsoft Docs
description: Meer informatie over de beschikbaarheidssets voor virtuele Windows-machines in Azure.
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 02/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 247f86dafe35d69dd742583d246862b739d9fe90
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-use-availability-sets"></a>Beschikbaarheidssets gebruiken

In deze zelfstudie leert u hoe u de beschikbaarheid en betrouwbaarheid van uw Virtual Machine-oplossingen op Azure kunt verhogen met behulp van beschikbaarheidssets. Beschikbaarheidssets zorgen ervoor dat de VM's die u op Azure implementeert, worden verdeeld over meerdere geïsoleerde hardwareknooppunten in een cluster. Dit zorgt ervoor dat als er zich binnen Azure een hardware- of softwarestoring voordoet, er slechts een subset van uw VM's wordt beïnvloed en dat uw totale oplossing beschikbaar en operationeel blijft. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een VM maken in een beschikbaarheidsset
> * Beschikbare VM-grootten controleren
> * Azure Advisor controleren

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 5.3 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 

## <a name="availability-set-overview"></a>Overzicht beschikbaarheidsset

Een beschikbaarheidsset is een logische groeperingsmogelijkheid die u in Azure kunt gebruiken om ervoor te zorgen dat de VM-resources die u erin plaatst, van elkaar worden geïsoleerd wanneer ze in een Azure-datacenter worden geïmplementeerd. Azure zorgt ervoor dat de VM's die u in een beschikbaarheidsset plaatst, op meerdere fysieke servers, rekenrekken, opslageenheden en netwerkswitches worden uitgevoerd. Als er zich een hardware- of softwarestoring in Azure voordoet, wordt slechts een subset van uw VM's getroffen en blijft uw totale toepassing actief en beschikbaar voor uw klanten. Beschikbaarheidssets zijn essentieel wanneer u betrouwbare cloudoplossingen wilt bouwen.

Laten we eens kijken naar een typische VM-oplossing met vier front-end webservers en twee back-end VM's die een database hosten. Met Azure wilt u twee beschikbaarheidssets definiëren voordat u uw VM's implementeert: een beschikbaarheidsset voor de weblaag en een beschikbaarheidsset voor de databaselaag. Bij het maken van een nieuwe VM kunt u vervolgens de beschikbaarheidsset opgeven als parameter voor de opdracht az vm create, en zorgt Azure er automatisch voor dat de VM's die u binnen de beschikbare set maakt, worden geïsoleerd over meerdere fysieke hardwareresources. Als er een probleem is met de fysieke hardware waarop een van uw webserver- of databaseserver-VM's draait, weet u dat de andere instanties van uw webserver en database-VM's actief blijven, omdat ze worden uitgevoerd op andere hardware.

Gebruik beschikbaarheidssets wanneer u betrouwbare VM-oplossingen wilt implementeren in Azure.

## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

U kunt een beschikbaarheidsset maken met [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). In dit voorbeeld stellen we het aantal update- en foutdomeinen in op *2* voor de beschikbaarheidsset met de naam *myAvailabilitySet* in de resourcegroep *ResourceGroupAvailability*.

Maak een resourcegroep.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

Maak een beheerde beschikbaarheidsset met behulp van [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) met de parameter `-sku aligned`.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>VM's maken in een beschikbaarheidsset
VM's moeten worden gemaakt binnen de beschikbaarheidsset om ervoor te zorgen dat ze correct over de hardware worden verdeeld. U kunt geen bestaande VM toevoegen aan een beschikbaarheidsset nadat deze is gemaakt. 

De hardware op een locatie is onderverdeeld in meerdere updatedomeinen en foutdomeinen. Een **updatedomein** is een groep VM's en onderliggende hardware die op hetzelfde moment opnieuw kan worden opgestart. VM's in hetzelfde **foutdomein** delen dezelfde opslag en hebben een gemeenschappelijke voedingsbron en netwerkswitch. 

Wanneer u een VM maakt met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm), gebruikt u de parameter `-AvailabilitySetName` om de naam van de beschikbaarheidsset op te geven.

Stel eerst een beheerdersnaam en -wachtwoord in voor de virtuele machine met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Maak nu twee VM's met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in de beschikbaarheidsset.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzureRmVm `
        -ResourceGroupName "myResourceGroupAvailability" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -PublicIpAddressName "myPublicIpAddress$i" `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred
}
```

Door de parameter `-AsJob` wordt de VM gemaakt als achtergrondtaak, zodat u weer terugkeert naar de PowerShell-prompts. U kunt details van achtergrondtaken bekijken met de cmdlet `Job`. Het duurt enkele minuten om beide VM's te maken en te configureren. Wanneer dit klaar is, hebt u twee virtuele machines die zijn gedistribueerd over de onderliggende hardware. 

Als u de beschikbaarheidsset in de portal bekijkt door naar Resource Groups > myResourceGroupAvailability > myAvailabilitySet te gaan, ziet u hoe de VM's zijn verdeeld over de twee fout- en updatedomeinen.

![Beschikbaarheidsset in de portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Beschikbare VM-grootten controleren 

U kunt later meer VM's toevoegen aan de beschikbaarheidsset, maar u moet weten welke VM-grootten beschikbaar zijn op de hardware. Gebruik [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) om een lijst weer te geven met alle beschikbare grootten op de hardwarecluster voor de beschikbaarheidsset.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Azure Advisor controleren 

U kunt ook Azure Advisor gebruiken om meer informatie te krijgen over het verbeteren van de beschikbaarheid van uw VM's. Azure Advisor helpt u best practices voor het optimaliseren van uw Azure-implementaties te volgen. Het analyseert uw resourceconfiguratie en gebruikstelemetrie, en raadt vervolgens oplossingen aan die u kunnen helpen de kosteneffectiviteit, prestaties, hoge beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

Meld u aan bij [Azure Portal](https://portal.azure.com), selecteer **Alle services** en typ **Advisor**. Het Advisor-dashboard toont de persoonlijke aanbevelingen voor het geselecteerde abonnement. Zie [Aan de slag met Azure Advisor](../../advisor/advisor-get-started.md) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een VM maken in een beschikbaarheidsset
> * Beschikbare VM-grootten controleren
> * Azure Advisor controleren

Ga naar de volgende zelfstudie voor meer informatie over virtuele-machineschaalsets.

> [!div class="nextstepaction"]
> [Een VM-schaalset maken](tutorial-create-vmss.md)


