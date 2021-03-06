---
title: Ondersteuning voor Azure IoT Hub MQTT begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - ondersteuning voor apparaten die verbinding maken met een IoT Hub apparaat gerichte eindpunt met behulp van de protocollen MQTT-protocol. Bevat informatie over ingebouwde MQTT ondersteuning op het apparaat van de Azure IoT SDK's.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9acda980583319414cc9e8668424907947a257db
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/13/2018
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>Communiceren met uw iothub met behulp van de protocollen MQTT-protocol

IoT-Hub maakt gebruik van apparaten om te communiceren met de IoT Hub apparaat-eindpunten met:

* [Protocollen MQTT v3.1.1] [ lnk-mqtt-org] op poort 8883
* Protocollen MQTT v3.1.1 via WebSocket op poort 443.

Alle communicatie met IoT Hub moet worden beveiligd met behulp van TLS/SSL. Daarom ondersteunt IoT Hub geen niet-beveiligde verbindingen via poort. 1883.

## <a name="connecting-to-iot-hub"></a>Verbinden met IoT Hub

Een apparaat kunt MQTT protocol verbinding maken met een IoT hub met gebruiken:

* Ofwel de bibliotheken in de [Azure IoT SDK's][lnk-device-sdks].
* Of het protocol MQTT rechtstreeks.

## <a name="using-the-device-sdks"></a>De SDK's van het apparaat

[Apparaat-SDK's] [ lnk-device-sdks] die ondersteuning bieden voor het protocol MQTT zijn beschikbaar voor Java, Node.js, C, C# en Python. De apparaat-SDK's gebruiken de standaard IoT Hub-verbindingsreeks een verbinding maken met een IoT-hub. De parameter van de client-protocol voor het gebruik van de protocollen MQTT protocol moet worden ingesteld op **MQTT**. Standaard, het apparaat SDK's verbinding maken met een IoT Hub met de **CleanSession** vlag ingesteld op **0** en gebruik **QoS 1** voor uitwisseling van berichten met de IoT-hub.

Wanneer een apparaat is verbonden met een IoT-hub, bieden de apparaat-SKD's methoden waarmee het apparaat voor het uitwisselen van berichten met een IoT-hub.

De volgende tabel bevat koppelingen naar codevoorbeelden voor elke ondersteunde taal en Hiermee geeft u de parameter moet een verbinding maken met IoT Hub met behulp van de protocollen MQTT-protocol gebruiken.

| Taal | Parameter protocol |
| --- | --- |
| [Node.js][lnk-sample-node] |azure-iot-device-mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migreren van een apparaat-app van AMQP naar MQTT

Als u de [apparaat-SKD's][lnk-device-sdks], wijzigen van de parameter protocol in de clientinitialisatie van de, zoals eerder gezegd overschakelen via AMQP naar MQTT vereist.

Wanneer doet, zorg er dan voor dat het volgende controleren:

* AMQP retourneert fouten voor veel voorwaarden terwijl MQTT wordt de verbinding verbroken. Als gevolg hiervan uw uitzonderingsverwerking logica mogelijk enkele wijzigingen.
* MQTT biedt geen ondersteuning voor de *afwijzen* operations tijdens het ontvangen van [cloud-naar-apparaatberichten][lnk-messaging]. Als uw back-endserver voor apps een reactie ontvangen van de app voor het apparaat moet, kunt u overwegen [methoden directe][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Met behulp van het protocol MQTT rechtstreeks

Als een apparaat niet kan het apparaat-SDK's, kan deze nog steeds verbinding maken met de openbare apparaat eindpunten met de protocollen MQTT-protocol op poort 8883. In de **CONNECT** pakket het apparaat moet de volgende waarden gebruiken:

* Voor de **ClientId** veld, gebruikt u de **deviceId**.

* Voor de **gebruikersnaam** veld `{iothubhostname}/{device_id}/api-version=2016-11-14`, waarbij `{iothubhostname}` is de volledige CName van de IoT-hub.

    Bijvoorbeeld, als de naam van uw IoT-hub is **contoso.azure devices.net** en als de naam van uw apparaat **MyDevice01**, de volledige **gebruikersnaam** veld moet bevatten:

    `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`

* Voor de **wachtwoord** veld, gebruikt u een SAS-token. De indeling van het SAS-token is hetzelfde als voor de HTTPS- en het AMQP-protocollen:

  `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`

  > [!NOTE]
  > Als u verificatie via x.509-certificaat gebruikt, zijn SAS-token wachtwoorden niet vereist. Zie voor meer informatie [x.509-beveiliging instellen in uw Azure-IoT-Hub][lnk-x509]

  Zie voor meer informatie over het genereren van SAS-tokens de apparaat-sectie van [IoT Hub met behulp van beveiligingstokens][lnk-sas-tokens].

  Bij het testen, kunt u ook gebruiken de [apparaat explorer] [ lnk-device-explorer] hulpprogramma snel een SAS-token dat u kunt kopiëren en plakken in uw eigen code genereren:

  1. Ga naar de **Management** tabblad **apparaat Explorer**.
  2. Klik op **SAS-Token** (top rechts).
  3. Op **SASTokenForm**, selecteer het apparaat in de **DeviceID** vervolgkeuzelijst. Stel uw **TTL**.
  4. Klik op **genereren** uw token maken.

     De SAS-token dat gegenereerd, heeft de volgende structuur:

     `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

     Het deel van dit token gebruiken als de **wachtwoord** veld verbinding maken via de protocollen MQTT is:

     `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

MQTT verbinding maken met en pakketten verbreken, IoT-Hub geeft een gebeurtenis op de **Operations Monitoring** kanaal. Deze gebeurtenis biedt extra informatie die u helpen kan bij het oplossen van problemen met de netwerkverbinding.

De apparaat-app kan aangeven een **wordt** bericht de **CONNECT** pakket. De app voor het apparaat moet worden gebruikt `devices/{device_id}/messages/events/{property_bag}` of `devices/{device_id}/messages/events/{property_bag}` als de **wordt** onderwerpnaam voor het definiëren van **wordt** berichten moeten worden doorgestuurd als een bericht telemetrie. In dit geval als de netwerkverbinding is gesloten, maar een **DISCONNECT** pakket niet eerder is ontvangen van het apparaat en IoT Hub verzendt de **wordt** bericht opgegeven in de **CONNECT** pakket naar het kanaal telemetrie. Het kanaal telemetrie mag ofwel de standaard **gebeurtenissen** eindpunt of een aangepaste eindpunt gedefinieerd door de IoT-Hub routering. Het bericht heeft de **iothub MessageType** eigenschap met de waarde **wordt** toegewezen.

### <a name="tlsssl-configuration"></a>TLS/SSL-configuratie

Gebruik de protocollen MQTT protocol rechtstreeks uw client *moet* verbinding maken via TLS/SSL. Pogingen om deze stap overslaan mislukt met fouten.

Om een TLS-verbinding tot stand brengen, kunt u wellicht te downloaden en verwijzen naar het DigiCert Baltimore-basiscertificaat. Dit certificaat is de die gebruikmaakt van Azure voor het beveiligen van de verbinding. U vindt dit certificaat in de [Azure-iot-sdk-c] [ lnk-sdk-c-certs] opslagplaats. Meer informatie over deze certificaten kan worden gevonden op [Digicert van website][lnk-digicert-root-certs].

Een voorbeeld van hoe u dit doet met de Python-versie van de [Paho MQTT bibliotheek] [ lnk-paho] door de Eclipse-Foundation kan er als volgt.

Installeer eerst de bibliotheek Paho uit uw omgeving opdrachtregel:

```cmd/sh
pip install paho-mqtt
```

Implementeer de client in een pythonscript. Vervang de tijdelijke aanduidingen als volgt:

* `<local path to digicert.cer>` het pad naar een lokaal bestand met de DigiCert Baltimore-basiscertificaat is. U kunt dit bestand maken door het kopiëren van de gegevens van het certificaat van [certs.c](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) ook de regels in de Azure IoT SDK voor C. `-----BEGIN CERTIFICATE-----` en `-----END CERTIFICATE-----`, verwijdert u de `"` aanhalingstekens aan het begin en einde van elke regel en Verwijder de `\r\n` tekens aan het einde van elke regel.
* `<device id from device registry>` is de ID van een apparaat dat u hebt toegevoegd aan uw IoT-hub.
* `<generated SAS token>` een SAS-token voor het apparaat dat is gemaakt, zoals eerder is beschreven in dit artikel is.
* `<iot hub name>` de naam van uw IoT-hub.

```python
from paho.mqtt import client as mqtt
import ssl

path_to_root_cert = "<local path to digicert.cer>"
device_id = "<device id from device registry>"
sas_token = "<generated SAS token>"
iot_hub_name = "<iot hub name>"

def on_connect(client, userdata, flags, rc):
  print ("Device connected with result code: " + str(rc))
def on_disconnect(client, userdata, rc):
  print ("Device disconnected with result code: " + str(rc))
def on_publish(client, userdata, mid):
  print ("Device sent message")

client = mqtt.Client(client_id=device_id, protocol=mqtt.MQTTv311)

client.on_connect = on_connect
client.on_disconnect = on_disconnect
client.on_publish = on_publish

client.username_pw_set(username=iot_hub_name+".azure-devices.net/" + device_id, password=sas_token)

client.tls_set(ca_certs=path_to_root_cert, certfile=None, keyfile=None, cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1, ciphers=None)
client.tls_insecure_set(False)

client.connect(iot_hub_name+".azure-devices.net", port=8883)

client.publish("devices/" + device_id + "/messages/events/", "{id=123}", qos=1)
client.loop_forever()
```

### <a name="sending-device-to-cloud-messages"></a>Verzenden van apparaat-naar-cloud-berichten

Nadat u de verbinding is geslaagd, een apparaat kunt verzenden berichten naar IoT Hub met `devices/{device_id}/messages/events/` of `devices/{device_id}/messages/events/{property_bag}` als een **onderwerpnaam**. De `{property_bag}` element kunt u het apparaat te verzenden van berichten met aanvullende eigenschappen in een url-notatie. Bijvoorbeeld:

```text
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Dit `{property_bag}` element gebruikt dezelfde codering als voor query-tekenreeks in het HTTPS-protocol.

Hier volgt een lijst met IoT Hub implementatie gedrag:

* IoT Hub biedt geen ondersteuning voor QoS-2-berichten. Als een apparaat-app een bericht met publiceert **QoS 2**, IoT Hub de netwerkverbinding wordt gesloten.
* IoT Hub behouden berichten niet bewaard is gebleven. Als een apparaat een bericht met verzendt de **behouden** vlag ingesteld op 1, IoT-Hub voegt de **x-opt-behouden** -eigenschap van toepassing op het bericht. In dit geval wordt in plaats van het behouden blijven van het bericht behouden, IoT-Hub wordt doorgegeven aan de back-end-app.
* IoT Hub ondersteunt slechts één actieve MQTT verbinding per apparaat. Een nieuwe verbinding MQTT namens de dezelfde apparaat-ID zorgt ervoor dat de IoT-Hub verwijderen van de bestaande verbinding.

Zie voor meer informatie [Messaging-handleiding voor ontwikkelaars][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Cloud-naar-apparaat-berichten ontvangen

Voor het ontvangen van berichten uit IoT Hub, een apparaat moet abonneren met `devices/{device_id}/messages/devicebound/#` als een **onderwerp Filter**. Het jokerteken met meerdere niveaus `#` in het onderwerp Filter wordt alleen gebruikt om aanvullende eigenschappen ontvangen in de onderwerpnaam van het apparaat toestaan. IoT Hub is niet toegestaan voor het gebruik van de `#` of `?` jokertekens voor het filteren van subonderwerpen. Omdat in IoT Hub is niet algemeen pub-sub messaging broker, ondersteunt deze alleen de gedocumenteerde onderwerpnamen en onderwerp filters.

Het apparaat niet ontvangt berichten uit IoT Hub, totdat deze is geabonneerd op het eindpunt apparaatspecifieke, vertegenwoordigd door de `devices/{device_id}/messages/devicebound/#` onderwerp filter. Nadat een abonnement is ingesteld, ontvangt het apparaat cloud-naar-apparaat-berichten dat is verzonden naar deze na de tijd van het abonnement. Als het apparaat verbinding met maakt **CleanSession** vlag ingesteld op **0**, het abonnement over de verschillende sessies worden bewaard. In dit geval wordt de volgende keer dat het apparaat verbinding maakt met **CleanSession 0** ontvangen geen openstaande berichten verzonden terwijl de verbinding verbroken. Als het apparaat gebruikmaakt van **CleanSession** vlag ingesteld op **1** echter er komt geen ontvangen alle berichten uit IoT Hub totdat het apparaat-eindpunt worden bijgehouden.

IoT Hub biedt berichten met de **onderwerpnaam** `devices/{device_id}/messages/devicebound/`, of `devices/{device_id}/messages/devicebound/{property_bag}` wanneer er eigenschappen van berichten. `{property_bag}` url-codering sleutel-waardeparen van berichteigenschappen bevat. Alleen de toepassingseigenschappen en de gebruiker instelbare eigenschappen (zoals **messageId** of **correlationId**) zijn opgenomen in de eigenschappenverzameling. Namen van de eigenschap System hebben het voorvoegsel  **$** , toepassingseigenschappen oorspronkelijke naam van de eigenschap met geen voorvoegsel gebruiken.

Wanneer een app voor het apparaat is lid van een onderwerp met **QoS 2**, IoT-Hub geeft het maximum aantal QoS-niveau 1 in de **SUBACK** pakket. Daarna biedt IoT Hub berichten op het apparaat met behulp van QoS-1.

### <a name="retrieving-a-device-twins-properties"></a>Ophalen van de eigenschappen van een apparaat-twin

Eerst een apparaat is lid van `$iothub/twin/res/#`, voor het ontvangen van reacties van de bewerking. Vervolgens wordt een leeg bericht verzonden naar onderwerp `$iothub/twin/GET/?$rid={request id}`, met de ingestelde waarde voor **aanvraag-ID**. De service verzendt vervolgens een antwoordbericht, waarin de gegevens van het apparaat twin op onderwerp `$iothub/twin/res/{status}/?$rid={request id}`, met behulp van dezelfde **aanvraag-ID** als de aanvraag.

Aanvraag-ID mag geldige waarde voor de waarde van een bericht eigenschap conform [messaging Ontwikkelaarshandleiding voor IoT-Hub][lnk-messaging], en de status wordt gevalideerd als een geheel getal.

Hoofdtekst van de reactie bevat het eigenschappengedeelte van het apparaat twin. Het volgende fragment toont bijvoorbeeld de hoofdtekst van de identiteit registervermelding beperkt naar het lid 'Eigenschappen':

```json
{
    "properties": {
        "desired": {
            "telemetrySendFrequency": "5m",
            "$version": 12
        },
        "reported": {
            "telemetrySendFrequency": "5m",
            "batteryLevel": 55,
            "$version": 123
        }
    }
}
```

De mogelijke statuscodes zijn:

|Status | Beschrijving |
| ----- | ----------- |
| 200 | Geslaagd |
| 429 | Te veel aanvragen (beperkt), conform [beperking IoT-Hub][lnk-quotas] |
| 5** | Server-fouten |

Zie voor meer informatie [Ontwikkelaarshandleiding voor apparaat horende][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Update apparaat twin gerapporteerd eigenschappen

De volgende procedure wordt beschreven hoe een apparaat bijgewerkt voor de gerapporteerde eigenschappen in de apparaat-twin in IoT-Hub:

1. Een apparaat moet zich eerst abonneren op de `$iothub/twin/res/#` onderwerp van de bewerking reacties van IoT-Hub ontvangt.

1. Een apparaat verzendt een bericht met de apparaat-twin update naar de `$iothub/twin/PATCH/properties/reported/?$rid={request id}` onderwerp. Dit bericht bevat een **aanvraag-ID** waarde.

1. De service verzendt vervolgens een antwoordbericht met de nieuwe ETag-waarde voor de verzameling gemelde eigenschappen op onderwerp `$iothub/twin/res/{status}/?$rid={request id}`. Dit antwoordbericht maakt gebruik van dezelfde **aanvraag-ID** als de aanvraag.

De berichttekst voor de aanvraag bevat een JSON-document met nieuwe waarden voor de gerapporteerde eigenschappen. Elk lid in het JSON-document updates of het bijbehorende lid in de apparaat-twin document toevoegen. Een lid is ingesteld op `null`, verwijdert u het lid van het containerobject. Bijvoorbeeld:

```json
{
    "telemetrySendFrequency": "35m",
    "batteryLevel": 60
}
```

De mogelijke statuscodes zijn:

|Status | Beschrijving |
| ----- | ----------- |
| 200 | Geslaagd |
| 400 | Ongeldige aanvraag. Ongeldige JSON |
| 429 | Te veel aanvragen (beperkt), conform [beperking IoT-Hub][lnk-quotas] |
| 5** | Server-fouten |

Zie voor meer informatie [Ontwikkelaarshandleiding voor apparaat horende][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>Ontvangende gewenste eigenschappen updatemeldingen

Wanneer een apparaat is verbonden, IoT-Hub meldingen worden verzonden naar het onderwerp `$iothub/twin/PATCH/properties/desired/?$version={new version}`, die de inhoud van de update die wordt uitgevoerd door de back-end oplossing bevatten. Bijvoorbeeld:

```json
{
    "telemetrySendFrequency": "5m",
    "route": null
}
```

Als voor updates voor de eigenschap `null` waarden betekent dat het JSON-Objectlid wordt verwijderd.

> [!IMPORTANT]
> IoT Hub genereert wijzigingsmeldingen alleen wanneer apparaten zijn verbonden. Zorg ervoor dat voor het implementeren van de [apparaat opnieuw verbinden stroom] [ lnk-devguide-twin-reconnection] te houden van de gewenste eigenschappen gesynchroniseerd tussen IoT Hub en de app voor het apparaat.

Zie voor meer informatie [Ontwikkelaarshandleiding voor apparaat horende][lnk-devguide-twin].

### <a name="respond-to-a-direct-method"></a>Reageren op een directe methode

Eerst een apparaat heeft om zich te abonneren op `$iothub/methods/POST/#`. IoT Hub verzendt methodeaanvragen naar het onderwerp `$iothub/methods/POST/{method name}/?$rid={request id}`, met een geldige JSON of een lege hoofdtekst.

Om te reageren, het apparaat verzendt een bericht met een geldige JSON of een lege hoofdtekst naar het onderwerp `$iothub/methods/res/{status}/?$rid={request id}`. In dit bericht is de **aanvraag-ID** moet overeenkomen met de naam in het aanvraagbericht en **status** moet een geheel getal.

Zie voor meer informatie [directe methode Ontwikkelaarshandleiding voor][lnk-methods].

### <a name="additional-considerations"></a>Aanvullende overwegingen

Als een definitieve overweging wanneer moet u het gedrag van protocollen MQTT protocol aan de cloud aanpassen raadpleegt u de [protocolgateway van Azure IOT][lnk-azure-protocol-gateway]. Deze software kunt u een aangepaste protocol krachtige gateway implementeren die interfaces rechtstreeks met IoT Hub. De Azure IoT-protocol-gateway kunt u het apparaat-protocol voor brownfield MQTT implementaties of andere aangepaste protocollen wilt aanpassen. Deze aanpak is echter vereist die u uitvoert en een aangepaste protocol-gateway werkt.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het protocol MQTT, de [MQTT documentatie][lnk-mqtt-docs].

Zie voor meer informatie over het plannen van de implementatie van uw IoT-Hub:

* [Azure gecertificeerd voor de catalogus van IoT-apparaat][lnk-devices]
* [Ondersteuning voor aanvullende protocollen][lnk-protocols]
* [Vergelijken met Event Hubs][lnk-compare]
* [Schalen, HA en Noodherstel][lnk-scaling]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-x509]: iot-hub-security-x509-get-started.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
[lnk-sdk-c-certs]: https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c
[lnk-digicert-root-certs]: https://www.digicert.com/digicert-root-certificates.htm
[lnk-paho]: https://pypi.python.org/pypi/paho-mqtt
