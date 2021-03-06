---
title: Overzicht van Azure SQL Database-beveiliging| Microsoft Docs
description: Meer informatie over Azure SQL Database en SQL Server-beveiliging, met inbegrip van de verschillen tussen de cloud en SQL Server on-premises.
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 03/12/2018
ms.author: giladm
ms.openlocfilehash: 1dc34f021fa6482c65cce0e922951ae329987c43
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="securing-your-sql-database"></a>Uw SQL Database beveiligen

In dit artikel worden de basisbeginselen van het beveiligen van de gegevenslaag van een toepassing met behulp van Azure SQL Database besproken. In het bijzonder helpt in dit artikel u op weg met resources voor het beveiligen van gegevens, het beheren van toegang en proactieve controle. 

Zie het [Beveiligingscentrum voor SQL Server Database Engine en Azure SQL Database](https://msdn.microsoft.com/library/bb510589) voor een volledig overzicht van beveiligingsfuncties die beschikbaar zijn in alle versies van SQL. In het [technische rapport over beveiliging en Azure SQL Database](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF-Bestand) is ook aanvullende informatie beschikbaar.

## <a name="protect-data"></a>Gegevens beveiligen

### <a name="encryption"></a>Versleuteling
SQL-Database beveiligt uw gegevens dankzij de codering voor gegevens in beweging met [Transport Layer Security](https://support.microsoft.com/kb/3135244), voor gegevens in rust met [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), en voor gegevens die in gebruik en [ Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx). 

> [!IMPORTANT]
>Alle verbindingen met Azure SQL Database vereisen te allen tijde versleuteling (SSL/TLS) wanneer gegevens naar en vanuit de database worden verzonden. In de verbindingsreeks van uw toepassing, moet u de parameters voor het versleutelen van de verbinding opgeven en *niet* het servercertificaat (dit geldt voor u als u de verbindingsreeks buiten de Azure-portal kopieert), anders vertrouwt de verbinding verifieert de identiteit van de server niet en is vatbaar voor 'man-in-the-middle'-aanvallen. Voor het stuurprogramma ADO.NET zijn deze verbindingsreeksparameters bijvoorbeeld **Encrypt=True** en **TrustServerCertificate=False**. 

Andere manieren om uw gegevens te versleutelen die u kunt overwegen:

* [Versleuteling op celniveau](https://msdn.microsoft.com/library/ms179331.aspx) om specifieke kolommen of zelfs cellen met gegevens met verschillende versleutelingssleutels te versleutelen.
* Als u een Hardware Security Module of centraal beheer van uw versleutelingssleutelhiërarchie nodig heeft, kunt u overwegen om [Azure Key Vault met SQL Server in een virtuele Azure-machine](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx) te gebruiken.

### <a name="data-discovery--classification"></a>Detectie van de gegevens & classificatie
Detectie van de gegevens & classificatie (momenteel in preview) biedt geavanceerde mogelijkheden die zijn ingebouwd in Azure SQL Database voor het detecteren, classificeren, labelen en beveiligen van de gevoelige gegevens in uw databases. Detecteren en classificeren van uw zoveel mogelijk gevoelige gegevens (business/financiële, gezondheidszorg, PII, enz.) kan een belangrijke rol spelen in uw organisatie Information protection-status. Deze kan dienen als infrastructuur voor:

- Verschillende security-scenario's, zoals controle (controleren) en waarschuwingen voor afwijkende toegang tot gevoelige gegevens.
- Toegang tot beheren en beperken van de beveiliging van databases met gevoelige gegevens.
- Helpt voldoen aan gegevens privacystandaarden en regelgeving.

Zie voor meer informatie [aan de slag met detectie van SQL DB Data & classificatie](sql-database-data-discovery-and-classification.md). 

## <a name="control-access"></a>Toegang beheren
Uw gegevens worden beveiligd met SQL Database door de toegang tot uw database te beperken met behulp van firewallregels, verificatiemechanismen die vereisen dat gebruikers hun identiteit bewijzen, en autorisatie voor gegevens via lidmaatschappen en machtigingen op basis van rollen. Ook wordt gebruikgemaakt van beveiliging op rijniveau en dynamische gegevensmaskering. Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.

> [!IMPORTANT]
> Het beheer van databases en logische servers in Azure wordt bepaald door de roltoewijzingen voor uw Portal-gebruikersaccount. Zie voor meer informatie over dit artikel [toegangsbeheer op basis van rollen in Azure-portal](../active-directory/role-based-access-control-what-is.md).
>

### <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels
Met firewalls wordt met behulp van [firewallregels](sql-database-firewall-configure.md) alle toegang tot uw databaseserver beperkt om uw gegevens te helpen beveiligen, totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.

### <a name="authentication"></a>Verificatie
SQL Database-verificatie verwijst naar hoe u uw identiteit bewijst bij het maken van verbinding met de database. SQL Database ondersteunt twee typen verificatie:
* **SQL-verificatie**, waarbij een gebruikersnaam en wachtwoord worden gebruikt. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze aanmeldingsgegevens kunt u zich bij elke database op die server als de database-eigenaar of 'dbo' verifiëren. 
* **Azure Active Directory-verificatie**, waarbij identiteiten worden gebruikt die worden beheerd in Azure Active Directory. Deze methode wordt ondersteund voor beheerde en geïntegreerde domeinen. Gebruik [waar mogelijk](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory-verificatie (geïntegreerde beveiliging). Als u Azure Active Directory-verificatie wilt gebruiken, moet u een andere serverbeheerder maken, de 'Azure AD-beheerder' genaamd, die Azure AD-gebruikers en -groepen kan beheren. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren. Zie [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een Azure AD-beheerder om Azure Active Directory-verificatie in te schakelen.

### <a name="authorization"></a>Autorisatie
Autorisatie verwijst naar wat een gebruiker kan doen binnen een Azure SQL Database. Dit wordt bepaald door de databaserollidmaatschappen en objectmachtigingen van uw gebruikersaccount. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing.

### <a name="row-level-security"></a>Beveiliging op rijniveau
Met beveiliging op rijniveau kunnen klanten de toegang tot rijen in een databasetabel beheren op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld groepslidmaatschap of uitvoeringscontext). Zie [Beveiliging op rijniveau](https://msdn.microsoft.com/library/dn765131) voor meer informatie.

### <a name="dynamic-data-masking"></a>Dynamische gegevensmaskering 
Gevoelige gegevens blootstelling SQL-Database dynamische-gegevensmaskering beperkt door het maskeren voor onbevoegde gebruikers. Dynamische-gegevensmaskering automatisch gedetecteerd potentieel gevoelige gegevens in Azure SQL Database en aanbevelingen actie worden uitgevoerd om te maskeren van deze velden, met minimale gevolgen voor de toepassingslaag. Dit werkt als volgt: de gevoelige gegevens in de resultatenset van een query die is uitgevoerd op toegewezen databasevelden, worden bedekt, terwijl de gegevens in de database niet worden gewijzigd. Zie voor meer informatie [aan de slag met SQL-Database dynamische-gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) kan worden gebruikt voor het beperken van blootstelling van gevoelige gegevens.

## <a name="proactive-monitoring"></a>Proactieve controle
Uw gegevens worden beveiligd met SQL Database met behulp van mogelijkheden voor controle en detectie van dreigingen. 

### <a name="auditing"></a>Controleren
SQL Database Auditing houdt databaseactiviteiten bij en helpt u bij de naleving van regelgeving door databasegebeurtenissen vast te leggen in een auditlogboek in uw Azure Storage-account. Door te controleren krijgt u informatie over actieve databaseactiviteiten. Het helpt u ook bij het analyseren en onderzoeken van activiteiten uit het verleden. Op deze manier kunt u mogelijke dreigingen, vermoedelijk misbruik of schendingen van de beveiliging identificeren. Zie [Get started with SQL Database Auditing](sql-database-auditing.md) (Aan de slag met SQL Database Auditing) voor aanvullende informatie.  

### <a name="threat-detection"></a>Detectie van bedreigingen
Detectie van dreigingen is een aanvulling op controle, dankzij een extra laag van beveiliging intelligence ingebouwd in de Azure SQL Database-service die ongebruikelijke en potentieel schadelijke probeert detecteert te openen of misbruik van databases. U wordt gewaarschuwd over verdachte activiteiten, potentiële beveiligingslekken naar voren SQL-injectieaanvallen en database afwijkende toegangspatronen. Dagelijks geconstateerde waarschuwingen kunnen bekeken worden vanuit [Azure Security Center](https://azure.microsoft.com/services/security-center/) en Geef details op van de verdachte activiteit en actie voor het onderzoeken en het risico dat het beste. Detectie van dreigingen kosten $15/server/maand. Het is gratis voor de eerste 60 dagen. Zie [Aan de slag met SQL Database Threat Detection](sql-database-threat-detection.md) voor meer informatie.
 
## <a name="compliance"></a>Naleving
Naast de bovenstaande functies en functionaliteit die kan helpen bij uw toepassing ook voorzien van verschillende beveiligingsvereisten, Azure SQL Database deel uitmaakt van de reguliere audits en tegen een aantal nalevingsstandaards is gecertificeerd. Zie het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL Database-nalevingscertificeringen](https://azure.microsoft.com/support/trust-center/services/) vindt, voor meer informatie.


## <a name="security-management"></a>Beveiligingsbeheer

SQL-Database helpt u bij de beveiliging van uw gegevens dankzij de database-scans en een gecentraliseerde beveiliging dashboard met [SQL Vulnerability Assessment](sql-vulnerability-assessment.md).

**Controle op beveiligingslekken**: [SQL Vulnerability Assessment](sql-vulnerability-assessment.md) (momenteel in preview) is een eenvoudig hulpprogramma ingebouwd in Azure SQL Database kunt u detecteren, bijhouden en herstellen van mogelijke database configureren beveiligingsproblemen. De beoordeling wordt uitgevoerd een beveiligingslek scan van uw database en genereert een rapport waarmee u inzicht in uw beveiligingsstatus, met inbegrip van bruikbare stappen voor het oplossen van beveiligingsproblemen en verbeter de databasebeveiliging van uw. Rapport van de beoordeling kan worden aangepast voor uw omgeving door een acceptabele basislijn voor de machtiging configuraties, configuraties en instellingen van de database. Dit kan u helpen bij:

- Voldoen aan nalevingsvereisten die database scan rapporten nodig hebt. 

- Voldoen aan gegevens privacy normen. 

- Bewaken van een dynamische database-omgeving waarin wijzigingen moeilijk zijn te volgen.

Zie voor meer informatie [SQL Vulnerability Assessment](sql-vulnerability-assessment.md).

## <a name="next-steps"></a>Volgende stappen

- Zie [Toegang beheren](sql-database-control-access.md) voor een discussie over het gebruik van functies voor toegangsbeheer in SQL Database.
- Zie voor een beschrijving van de Databasecontrole, [SQL Database auditing](sql-database-auditing.md).
- Zie voor een beschrijving van de detectie van dreigingen [SQL-Database de detectie van dreigingen](sql-database-threat-detection.md).
