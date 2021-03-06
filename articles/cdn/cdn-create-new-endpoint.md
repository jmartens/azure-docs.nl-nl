---
title: Aan de slag met Azure CDN | Microsoft Docs
description: In dit onderwerp leert u hoe u het Content Delivery Network (CDN) van Azure inschakelt. In deze zelfstudie wordt uitgelegd hoe u een nieuw CDN-profiel en -eindpunt maakt.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2018
ms.author: mazha
ms.openlocfilehash: f1681b0796885cd6dc880303485edb97c15cda71
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="getting-started-with-azure-cdn"></a>Aan de slag met Azure CDN
In dit artikel wordt beschreven hoe u Azure [Content Delivery Network (CDN)](cdn-overview.md) inschakelt door een nieuw CDN-profiel en -eindpunt te maken.

## <a name="create-a-new-cdn-profile"></a>Nieuwe CDN-profielen maken
Een CDN-profiel is een verzameling van CDN-eindpunten. Elk profiel kan een of meer CDN-eindpunten bevaten. U kunt meerdere profielen gebruiken om de CDN-eindpunten te ordenen op basis van het internetdomein, de webtoepassing of andere criteria.

> [!NOTE]
> Een Azure-abonnement heeft standaardlimieten voor de volgende resources:
> - Het aantal CDN-profielen dat kan worden gemaakt
> - Het aantal eindpunten dat kan worden gemaakt in een CDN-profiel 
> - Het aantal aangepaste domeinen dat kan worden toegewezen aan een eindpunt
>
> Zie [CDN-limieten](https://docs.microsoft.com/azure/azure-subscription-service-limits#cdn-limits) voor meer informatie over CDN-abonnementen.
>
> De prijzen van CDN worden standaard toegepast op het niveau van het CDN-profiel. Als u verschillende Azure CDN-prijscategorieën wilt gebruiken, moet u daarom meerdere CDN-profielen maken.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Nieuwe CDN-eindpunten maken
**Een nieuw CDN-eindpunt maken**

1. Navigeer in [Azure Portal](https://portal.azure.com) naar uw CDN-profiel. Mogelijk hebt u het profiel in de vorige stap vastgemaakt aan het dashboard. Zo niet, dan kunt u het vinden door **Alle services** en vervolgens **CDN-profielen** te selecteren. Selecteer in het deelvenster **CDN-profielen** het profiel waaraan u uw eindpunt wilt toevoegen. 
   
    Het deelvenster CDN-profiel wordt weergegeven.
   
    ![CDN-profiel][cdn-profile-settings]

2. Selecteer **Eindpunt**.
   
    ![De knop Eindpunt toevoegen][cdn-new-endpoint-button]
   
    Het deelvenster **Een eindpunt toevoegen** wordt weergegeven.
   
    ![Deelvenster Eindpunt toevoegen][cdn-add-endpoint]

3. Voer voor **Naam** een unieke naam voor het nieuwe CDN-eindpunt in. Deze naam wordt gebruikt om toegang te krijgen tot uw resources in de cache in het domein `<endpointname>.azureedge.net`.

4. Selecteer een oorsprongtype voor **Oorsprongtype**. Selecteer **Storage** voor een Azure Storage-account, **Cloudservice** voor een Azure Cloud-service, **Web App** voor een Azure-webtoepassing of **Aangepaste oorsprong** voor een andere openbaar toegankelijke webserveroorsprong (gehost in Azure of ergens anders).
   
    ![Oorsprongtype CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)

5. Selecteer of typ uw domein van oorsprong in **Hostnaam van oorsprong**. De vervolgkeuzelijst bevat alle beschikbare oorsprongen van het type dat u hebt opgegeven in stap 4. Als u **Aangepaste oorsprong** als oorsprongtype hebt geselecteerd, voert u het domein in van de aangepaste oorsprong.
    
6. Voer bij **Oorsprongpad** het pad in naar de resources die u wilt opslaan in de cache. U kunt het vak ook leeg laten, zodat alle resources in het domein dat u in stap 5 hebt opgegeven, kunnen worden opgeslagen in de cache.
    
7. Geef bij **Host-header van oorsprong** de host-header op die voor elke aanvraag door Azure CDN moet worden verzonden of laat de standaardinstelling staan.
   
   > [!WARNING]
   > Bepaalde oorsprongtypen, zoals Azure Storage en Web Apps, vereisen dat de hostheader overeenkomt met het domein van de oorsprong. Tenzij u een oorsprong hebt waarvoor een andere host-header is vereist dan die van het domein, laat u de standaardwaarde ongewijzigd.
   > 
    
8. Geef voor **Protocol** en **Poort van oorsprong** de protocollen en poorten op die worden gebruikt voor toegang tot uw resources die aan de oorsprong liggen. Er moet minimaal één protocol (HTTP of HTTPS) worden geselecteerd. Gebruik het domein dat is verstrekt door het CDN (`<endpointname>.azureedge.net`) voor toegang tot HTTPS-inhoud. 
   
   > [!NOTE]
   > De waarde voor **Poort van oorsprong** bepaalt alleen de poort die door het eindpunt wordt gebruikt om informatie op te halen bij de oorsprong. Het eindpunt zelf is alleen beschikbaar voor eindclients op de standaard-HTTP- en -HTTPS-poorten (80 en 443), ongeacht de waarde voor **Poort van oorsprong**.  
   > 
   > Eindpunten in **Azure CDN van Akamai**-profielen staan niet het volledige TCP-poortbereik voor oorsprongpoorten toe. Zie [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Door Azure CDN van Akamai toegestane poorten van oorsprong) voor een lijst met poorten van oorsprong die niet zijn toegestaan.  
   > 
   > Wanneer u toegang hebt tot CDN-inhoud via HTTPS, gelden de volgende beperkingen:
   > 
   > * Gebruik het SSL-certificaat dat door het CDN is verstrekt. Certificaten van derden worden niet ondersteund.
   > * HTTPS-ondersteuning voor aangepaste Azure CDN-domeinen is alleen beschikbaar bij **Azure CDN van Verizon**-producten (Standard en Premium). Dit wordt niet ondersteund in producten van **Azure CDN van Akamai**. Zie [HTTPS op een aangepast Azure CDN-domein configureren](cdn-custom-ssl.md) voor meer informatie.
    
9. Selecteer **Toevoegen** om het nieuwe eindpunt te maken.
   
   Zodra het eindpunt is gemaakt, wordt deze weergegeven in de lijst met eindpunten voor het profiel.
    
   ![CDN-eindpunt][cdn-endpoint-success]
    
   > [!IMPORTANT]
   > Het eindpunt is niet onmiddellijk beschikbaar voor gebruik, aangezien het enige tijd vergt om de registratie door te geven. Profielen van **Azure CDN van Akamai** worden doorgaans binnen één minuut doorgegeven. Profielen van **Azure CDN van Verizon** worden doorgaans binnen 90 minuten doorgegeven, maar in sommige gevallen kan dit langer duren.
    > 
    > Als u de CDN-domeinnaam probeert te gebruiken voordat de eindpuntconfiguratie is doorgegeven aan de POP's, kunt u een HTTP 404-reactiestatus ontvangen. Zie [Problemen oplossen met CDN-eindpunten die 404-statusberichten retourneren](cdn-troubleshoot-endpoint.md) als u een paar uur nadat u uw eindpunt hebt gemaakt, nog steeds een 404-reactiestatus ontvangt.
    > 
    > 

## <a name="see-also"></a>Zie ook
* [Het cachegedrag van aanvragen beheren met queryreeksen](cdn-query-string.md)
* [CDN-inhoud toewijzen aan een aangepast domein](cdn-map-content-to-custom-domain.md)
* [Vooraf assets op een Azure CDN-eindpunt laden](cdn-preload-endpoint.md)
* [Een Azure CDN-eindpunt leegmaken](cdn-purge-endpoint.md)
* [Problemen oplossen voor CDN-eindpunten die 404-statusberichten retourneren](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
