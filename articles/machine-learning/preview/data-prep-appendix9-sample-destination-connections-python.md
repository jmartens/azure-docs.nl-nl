---
title: Voorbeeld bestemmingen/uitvoer mogelijk met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een een reeks voorbeelden van aangepaste gegevens bestemmingen/uitvoer met de voorbereiding voor Azure Machine Learning-gegevens
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: a9476b9ac3a5cc1f82ab1c09678972fb6b78b894
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="sample-of-destination-connections-python"></a>Voorbeeld van de doelverbindingen (Python) 
Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Schrijven naar Excel 


Schrijven naar Excel is een extra library vereist. Toevoegen van nieuwe tapewisselaars wordt beschreven in het overzicht van uitbreidbaarheid. `openpyxl` is de bibliotheek die u wilt toevoegen.

Voordat u naar Excel schrijven, is enkele andere wijzigingen mogelijk nodig. Sommige van de gegevenstypen die worden gebruikt in het voorbereiden van gegevens worden niet ondersteund in sommige formaten bestemming. Bijvoorbeeld, als "Error" objecten bestaan, won't ze correct geconverteerd naar Excel. Voordat u probeert te schrijven naar Excel, moet u dus een 'Foutwaarden vervangen'-transformatie die fouten uit kolommen verwijdert.

Als er al het vorige werk is voltooid, wordt de volgende regel de tabel met één blad in een Excel-document. Voeg een transformatie transformeren gegevensstroom (Script). Voer de volgende code in een expressie-sectie.


### <a name="on-windows"></a>In Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>Op Mac OS-en OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
