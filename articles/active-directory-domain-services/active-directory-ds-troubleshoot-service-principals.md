---
title: 'Azure Active Directory Domain Services: Configuratie van de Service-Principal probleemoplossing | Microsoft Docs'
description: Het Service-Principal-configuratie voor Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: ergreenl
ms.openlocfilehash: e1be075ba2d3e6ae7512ccc030073fd7f1862502
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Ongeldige configuratie voor Service-Principal voor uw beheerde domein oplossen

In dit artikel helpt u bij het oplossen van fouten van de service-principal-gerelateerde configuratie die in het volgende bericht weergegeven resulteren:

## <a name="alert-aadds102-service-principal-not-found"></a>Waarschuwing AADDS102: Service-Principal is niet gevonden

**Waarschuwing:** *A Service-Principal is vereist voor Azure AD Domain Services naar behoren is verwijderd uit uw Azure AD-directory. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.*

[Service-principals](../active-directory/develop/active-directory-application-objects.md) toepassingen zijn die Microsoft gebruikt om te beheren, bijwerken en onderhouden van uw beheerde domein. Als ze worden verwijderd, wordt de mogelijkheid van Microsoft voor het onderhouden van uw domein verbroken.


## <a name="check-for-missing-service-principals"></a>Controleer op ontbrekende service-principals
Gebruik de volgende stappen uit om te bepalen welke service principals moeten opnieuw worden gemaakt:

1. Navigeer naar de [bedrijfstoepassingen - alle toepassingen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) pagina in de Azure-portal.
2. In de **weergeven** vervolgkeuzelijst **alle toepassingen** en klik op **toepassen**.
3. Gebruik de volgende tabel zoeken voor elk toepassings-ID door de ID in het zoekvak te plakken en drukt u op te geven. Als de zoekresultaten leeg zijn, moet u de service-principal opnieuw maken door de stappen in de kolom 'Oplossing'.

| Toepassings-id | Oplossing |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Maak opnieuw een ontbrekende service-principal met PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Opnieuw registreren bij de naamruimte Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Opnieuw registreren bij de naamruimte Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Service-principals self corrigeren](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Maak opnieuw een ontbrekende Service-Principal met PowerShell
Volg deze stappen als een service-principal met de ID ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` ontbreekt in uw Azure AD-directory.

**Oplossing:** moet u Azure AD PowerShell om deze stappen te voltooien. Zie voor meer informatie over het installeren van Azure AD PowerShell [in dit artikel](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Om dit probleem op te lossen, typt u de volgende opdrachten in een PowerShell-venster:
1. Installeer de Azure AD PowerShell-module en importeren.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Controleer of de service-principal die is vereist voor Azure AD Domain Services in uw directory ontbreekt door het uitvoeren van de volgende PowerShell-opdracht:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. De service-principal maken door de volgende PowerShell-opdracht te typen:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Nadat u de ontbrekende service principal gemaakt hebt, wacht twee uren en controleer de status van uw beheerde domein.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Opnieuw registreren bij de Microsoft AAD-naamruimte met behulp van de Azure-portal
Volg deze stappen als een service-principal met de ID ```443155a6-77f3-45e3-882b-22b3a8d431fb``` of ```abba844e-bc0e-44b0-947a-dc74e5d09022``` ontbreekt in uw Azure AD-directory.

**Oplossing:** gebruik van de volgende stappen uit om te herstellen Domain Services op de map:

1. Navigeer naar de [abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) pagina in de Azure-portal.
2. Kies het abonnement uit de tabel die is gekoppeld aan uw beheerde domein
3. Met de linkernavigatiebalk kiezen **Resourceproviders**
4. Zoek 'Microsoft.AAD' in de tabel en klik op **opnieuw te registreren**
5. Weergeven om te controleren of de waarschuwing is opgelost, de health-pagina voor uw beheerde domein in twee uur.


## <a name="service-principals-that-self-correct"></a>Service-Principals self corrigeren
Volg deze stappen als een service-principal met de ID ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` ontbreekt in uw Azure AD-directory.

**Oplossing:** Azure AD Domain Services kan detecteren wanneer deze specifieke service-principal ontbreekt, is onjuist geconfigureerd of is verwijderd. De service wordt automatisch opnieuw gemaakt deze service-principal. Echter, moet u de toepassing verwijderen en object waarmee de verwijderde toepassing, zoals als de certificeringsinstantie via implementeert, de toepassing en het object niet langer wordt gewerkt kunnen worden gewijzigd door de nieuwe service-principal. Dit leidt tot een nieuwe fout in uw domein. Volg de stappen die worden beschreven in de [sectie voor AADDS105](#alert-aadds105-password-synchronization-application-is-out-of-date) om dit probleem te voorkomen. Controleer nadat de status van uw beheerde domein na twee uur om ervoor te zorgen dat de nieuwe service-principal is opnieuw gemaakt.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Waarschuwing AADDS105: Wachtwoord synchronisatie van toepassing is verouderd

**Waarschuwing:** de service-principal met de toepassings-ID 'd87dcbc6-a371-462e-88e3-28ad15ec4e64' is verwijderd en Microsoft kon opnieuw te maken. Deze service-principal beheert een andere service-principal en een toepassing die worden gebruikt voor synchronisatie van wachtwoorden. De beheerde service-principal en de toepassing niet worden geautoriseerd onder de nieuwe service-principal en uiteindelijk verouderd wanneer de synchronisatie van het certificaat verloopt. Dit betekent dat de zojuist gemaakte service-principal kan niet worden bijgewerkt van de oude beheerde toepassingen en synchronisatie van AAD-objecten die worden beïnvloed.


**Oplossing:** moet u Azure AD PowerShell om deze stappen te voltooien. Zie voor meer informatie over het installeren van Azure AD PowerShell [in dit artikel](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Om dit probleem op te lossen, typt u de volgende opdrachten in een PowerShell-venster:
1. Installeer de Azure AD PowerShell-module en importeren.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. Verwijder de oude toepassing en het object met de volgende PowerShell-opdrachten

    ```powershell
    $app = Get-AzureADApplication -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Nadat u beide hebt verwijderd, wordt het systeem zelf herstellen en opnieuw maken van de toepassingen die nodig zijn voor Wachtwoordsynchronisatie. Twee uur wachten om te controleren of de waarschuwing is hersteld, en controleer de status van uw domein.


## <a name="contact-us"></a>Contact opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).
