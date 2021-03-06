---
title: Talen voor de gegevens wetenschappelijke virtuele Machine in Azure | Microsoft Docs
description: Talen voor de gegevens wetenschappelijke virtuele Machine in Azure
keywords: hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, hulpprogramma's voor gegevenswetenschap, gegevenswetenschap linux
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: bb36f79d6af66dfaceb63730d59713ab9da7c89e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Talen die worden ondersteund op de gegevens wetenschappelijke virtuele Machine 

De gegevens wetenschappelijke virtuele Machine (DSVM) wordt geleverd met verschillende vooraf samengestelde talen en ontwikkelingsprogramma's voor het bouwen van uw AI-toepassingen. Hier zijn enkele van de meest kenmerkende die zijn. 

## <a name="python-windows-server-2016-edition"></a>Python (Windows Server 2016 Edition)

|    |           |
| ------------- | ------------- |
| Ondersteunde versies | 2.7 en 3.6 |
| Ondersteunde DSVM edities      | Windows Server 2016     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Twee globale `conda` omgevingen worden gemaakt. <br /> * `root` omgeving zich bevindt op `/anaconda/` Python 3.6 is. <br/> * `python2` omgeving zich bevindt op `/anaconda/envs/python2`is Python 2.7       |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks voor Python zijn opgenomen     |
| Bijbehorende hulpprogramma's op de DSVM      | PySpark, R, Julia      |

> [!NOTE]
> Windows Server 2016 gemaakt vóór maart 2018 bevat Python 3.5 en Python 2.7. Python 2.7 is ook de conda **hoofdmap** omgeving en **py35** is de 3.5 Python-omgeving. 

### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?    

* Uitgevoerd in de opdrachtprompt

Opdrachtprompt openen en doe het volgende, afhankelijk van de versie van Python die u wilt uitvoeren. 

```
# To run Python 2.7
activate python2
python --version

# To run Python 3.6
activate 
python --version

```
* Gebruik in een IDE

Gebruik Python-Tools voor Visual Studio (PTVS) in de Visual Studio Community-editie geïnstalleerd. De instellingen alleen omgeving automatisch in PTVS standaard is Python 3.6. 

> [!NOTE]
> De PTVS op Python 2.7 verwijst, moet u een aangepaste omgeving maken in PTVS. Navigeer naar deze omgeving paden in de Visual Studio Community Edition stelt **extra** -> **Python Tools** -> **Python-omgevingen** en klik vervolgens op **+ aangepaste**. Stel de locatie op `c:\anaconda\envs\python2` en klik vervolgens op _automatische detectie_. 

* Gebruik in Jupyter

Open Jupyter en klik op de `New` om te maken van een nieuwe notebook. Op dit moment kunt u het type van de kernel als _Python [Conda Root]_ voor Python 3.6 en _Python [Conda env:python2]_ voor Python 2.7-omgeving. 

* Python-pakketten installeren

De standaard Python-omgevingen op de DSVM zijn globale omgeving kan worden gelezen door alle gebruikers. Maar alleen beheerders kunnen schrijven / globale pakketten installeren. Activeren om het pakket naar de globale omgeving installeert, naar de hoofdmap of python2 omgeving met de `activate` opdracht als Administrator. Vervolgens kunt u de package manager zoals `conda` of `pip` te installeren of bijwerken van pakketten. 

## <a name="python-linux-and-windows-server-2012-edition"></a>Python (Linux en Windows Server 2012 Edition)

|    |           |
| ------------- | ------------- |
| Ondersteunde versies | 2.7 en 3.5 |
| Ondersteunde DSVM edities      | Linux, Windows Server 2012    |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Twee globale `conda` omgevingen worden gemaakt. <br /> * `root` omgeving zich bevindt op `/anaconda/` Python 2.7 is. <br/> * `py35` omgeving zich bevindt op `/anaconda/envs/py35`Python 3.5 is       |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks voor Python zijn opgenomen     |
| Bijbehorende hulpprogramma's op de DSVM      | PySpark, R, Julia      |
### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?    

**Linux**
* Uitgevoerd in terminal

Open de terminal en doe het volgende, afhankelijk van de versie van Python die u wilt uitvoeren. 

```
# To run Python 2.7
source activate 
python --version

# To run Python 3.5
source activate py35
python --version

```
* Gebruik in een IDE

Gebruik PyCharm in de Visual Studio Community-editie geïnstalleerd. 

* Gebruik in Jupyter

Open Jupyter en klik op de `New` om te maken van een nieuwe notebook. Op dit moment kunt u het type van de kernel als _Python [Conda Root]_ voor Python 2.7 en _Python [Conda env:py35]_ voor 3.5 Python-omgeving. 

* Python-pakketten installeren

De standaard Python-omgevingen op de DSVM zijn globale omgevingen kan worden gelezen door alle gebruikers. Maar alleen beheerders kunnen schrijven / globale pakketten installeren. Activeren om het pakket naar de globale omgeving installeert, naar de hoofdmap of py35 omgeving met de `source activate` opdracht als beheerder of als een gebruiker met sudo-machtigingen. Vervolgens kunt u een Pakketbeheer zoals `conda` of `pip` te installeren of bijwerken van pakketten. 

**Windows 2012**
* Uitgevoerd in de opdrachtprompt

Opdrachtprompt openen en doe het volgende, afhankelijk van de versie van Python die u wilt uitvoeren. 

```
# To run Python 2.7
activate 
python --version

# To run Python 3.5
activate py35
python --version

```
* Gebruik in een IDE

Gebruik Python-Tools voor Visual Studio (PTVS) in de Visual Studio Community-editie geïnstalleerd. De instellingen alleen omgeving automatisch in PTVS in Python 2.7. 
> [!NOTE]
> De PTVS op Python 3.5 verwijst, moet u een aangepaste omgeving maken in PTVS. Navigeer naar deze omgeving paden in de Visual Studio Community Edition stelt **extra** -> **Python Tools** -> **Python-omgevingen** en klik vervolgens op **+ aangepaste**. Stel de locatie op `c:\anaconda\envs\py35` en klik vervolgens op _automatische detectie_. 

* Gebruik in Jupyter

Open Jupyter en klik op de `New` om te maken van een nieuwe notebook. Op dit moment kunt u het type van de kernel als _Python [Conda Root]_ voor Python 2.7 en _Python [Conda env:py35]_ voor 3.5 Python-omgeving. 

* Python-pakketten installeren

De standaard Python-omgevingen op de DSVM zijn globale omgeving kan worden gelezen door alle gebruikers. Maar alleen beheerders kunnen schrijven / globale pakketten installeren. Activeren om het pakket naar de globale omgeving installeert, naar de hoofdmap of py35 omgeving met de `activate` opdracht als Administrator. Vervolgens kunt u de package manager zoals `conda` of `pip` te installeren of bijwerken van pakketten. 

## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Ondersteunde versies | Microsoft R Open 3.x (100% compatibel is met CRAN R<br /> Microsoft R Server 9.x ontwikkelaarsversie (een schaalbare gereed R platform)|
| Ondersteunde DSVM edities      | Linux, Windows     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Windows: `C:\Program Files\Microsoft\ML Server\R_SERVER` <br />Linux: ` /usr/lib64/microsoft-r/3.3/lib64/R`    |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks voor R zijn opgenomen     |
| Bijbehorende hulpprogramma's op de DSVM      | SparkR, Python, Julia      |
### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?    

**Windows**:

* Uitgevoerd in de opdrachtprompt

Opdrachtprompt openen en typt u `R`.

* Gebruik in een IDE

Gebruik RTools voor Visual Studio (RTVS) geïnstalleerd in de Visual Studio Community edition of RStudio. Deze zijn beschikbaar in het startmenu of als een pictogram op het bureaublad. 

* Gebruik in Jupyter

Open Jupyter en klik op de `New` om te maken van een nieuwe notebook. Op dit moment kunt u het type van de kernel als _R_ Jupyter R kernel (IRKernel) gebruiken. 

* R-pakketten installeren

R is geïnstalleerd op de DSVM in een globale-omgeving kan worden gelezen door alle gebruikers. Maar alleen beheerders kunnen schrijven / globale pakketten installeren. Om te installeren pakket aan de globale omgeving, R met een van de methoden die hierboven worden uitgevoerd. U kunt de R package manager voert `install.packages()` te installeren of bijwerken van pakketten. 

**Linux**:

* Uitgevoerd in terminal

Open terminal en alleen uitvoeren `R`.  

* Gebruik in een IDE

RStudio geïnstalleerd op de Linux DSVM gebruiken.  

* Gebruik in Jupyter

Open Jupyter en klik op de `New` om te maken van een nieuwe notebook. Op dit moment kunt u het type van de kernel als _R_ Jupyter R kernel (IRKernel) gebruiken. 

* R-pakketten installeren

R is geïnstalleerd op de DSVM in een globale-omgeving kan worden gelezen door alle gebruikers. Maar alleen beheerders kunnen schrijven / globale pakketten installeren. Om te installeren pakket aan de globale omgeving, R met een van de methoden die hierboven worden uitgevoerd. U kunt de R package manager voert `install.packages()` te installeren of bijwerken van pakketten. 


## <a name="julia"></a>Julia

|    |           |
| ------------- | ------------- |
| Ondersteunde versies | 0.6 |
| Ondersteunde DSVM edities      | Linux, Windows     |
| Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM?  | Windows: Geïnstalleerd op `C:\JuliaPro-VERSION`<br /> Linux: Geïnstalleerd op `/opt/JuliaPro-VERSION`    |
| Koppelingen naar voorbeelden      | Voorbeeld Jupyter-notebooks voor Julia zijn opgenomen     |
| Bijbehorende hulpprogramma's op de DSVM      | Python, R      |
### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?    

**Windows**:

* Uitgevoerd in de opdrachtprompt

Opdrachtprompt openen en voert `julia`. 
* Gebruik in een IDE

Gebruik `Juno` de Julia IDE geïnstalleerd op de DSVM en beschikbaar zijn als een snelkoppeling op het bureaublad.

* Gebruik in Jupyter

Open Jupyter en klik op de `New` om te maken van een nieuwe notebook. Op dit moment kunt u het type van de kernel als `Julia VERSION` 

* Julia pakketten installeren

De standaardlocatie Julia is een globale-omgeving kan worden gelezen door alle gebruikers. Maar alleen beheerders kunnen schrijven / globale pakketten installeren. Julia met behulp van een van de bovenstaande methoden om het pakket naar de globale omgeving installeert, worden uitgevoerd. Vervolgens kunt u het pakket Julia manager opdrachten zoals uitvoeren `Pkg.add()` te installeren of bijwerken van pakketten. 


**Linux**:
* Uitgevoerd in de terminal.

Open terminal en alleen uitvoeren `julia`. 
* Gebruik in een IDE

Gebruik `Juno` de Julia IDE geïnstalleerd op de DSVM en beschikbaar zijn als een snelkoppeling naar de toepassing menu.

* Gebruik in Jupyter

Open Jupyter en klik op de `New` om te maken van een nieuwe notebook. Op dit moment kunt u het type van de kernel als `Julia VERSION` 

* Julia pakketten installeren

De standaardlocatie Julia is een globale-omgeving kan worden gelezen door alle gebruikers. Maar alleen beheerders kunnen schrijven / globale pakketten installeren. Julia met behulp van een van de bovenstaande methoden om het pakket naar de globale omgeving installeert, worden uitgevoerd. Vervolgens kunt u het pakket Julia manager opdrachten zoals uitvoeren `Pkg.add()` te installeren of bijwerken van pakketten. 

## <a name="other-languages"></a>Andere talen

**C#**: beschikbaar in Windows is en toegankelijk is via de editie Visual Studio Community of in een `Developer Command Prompt for Visual Studio` waar u alleen kunt uitvoeren `csc` opdracht. 

**Java**: OpenJDK is beschikbaar op Linux- en Windows-editie van het DSVM en ingesteld op het pad. U kunt typen `javac` of `java` opdracht op de opdrachtprompt in Windows of bash-shell in Linux Java gebruiken. 

**node.js**: node.js is beschikbaar op Linux- en Windows-editie van het DSVM en ingesteld op het pad. U kunt typen `node` of `npm` opdracht op de opdrachtprompt in Windows of bash-shell in Linux voor toegang tot node.js. In Windows, wordt de Node.js-tools voor Visual Studio-extensie te hebben van een grafische IDE voor het ontwikkelen van uw node.js-toepassing geïnstalleerd. 

**F #**: beschikbaar in Windows is en toegankelijk is via de editie Visual Studio Community of in een `Developer Command Prompt for Visual Studio` waar u alleen kunt uitvoeren `fsc` opdracht. 


