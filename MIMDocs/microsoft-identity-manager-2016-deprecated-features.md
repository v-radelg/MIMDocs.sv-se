---
title: MIM-föråldrade funktioner och planering för framtiden | Microsoft Docs
description: I den här artikeln dokumenteras inaktuella funktioner i MIM Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 009d0e99e2da445d4df35dc9de81b297a65fe2a3
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044233"
---
# <a name="deprecated-features"></a>Föråldrade funktioner

I den här artikeln beskrivs de inaktuella funktionerna i Microsoft Identity Manager 2016 SP1. Om funktionen fortfarande finns i Microsoft Identity Manager, stöds den fortfarande. Funktioner rekommenderas inte för nya distributioner, eftersom de kan tas bort i en funktions utgåva.  För utvecklare rekommenderar vi att du inte använder inaktuella funktioner i nya program eller lösningar.

> [!NOTE]
> Funktioner som tas bort i Microsoft Identity Manager SP1 identifieras med * *. <br>
> Mer information om support [livs cykeln för Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Microsoft rekommenderar inte att kunder startar nya distributioner av Microsoft BHOLD Suite-komponenter. Befintliga distributioner av BHOLD kommer att fortsätta att stödjas. Azure AD ger nu [åtkomst granskningar](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) som ersätter några av kampanj funktionerna för BHOLD-attestering.

## <a name="certificate-management"></a>Certifikathantering 

| **Kategori**                | **Föråldrad funktion**              | **Ersätta och kommentera**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Hanterings agenter | \* * FIM-certifikat hantering | FIM-certifikatets hanterings agent har tagits bort i MIM 2016.                                                             |

## <a name="service-and-portal"></a>Tjänst och portal

| **Kategori**                | **Föråldrad funktion**              | **Ersätta och kommentera**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Program konfiguration | Konfigurations gränssnitt för webb tjänst (MA-data och MV-data) | Möjligheten att konfigurera FIM-synkroniseringstjänsten via FIM-tjänsten kommer att tas bort i nästa version.
|

## <a name="synchronization-service"></a>Synkroniseringstjänst 

| **Kategori**                | **Föråldrad funktion**              | **Ersätta och kommentera**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Program konfiguration | Konfigurations gränssnitt för webb tjänst | Möjligheten att konfigurera FIM-synkroniseringstjänsten via FIM-tjänsten kommer att tas bort i nästa version.                                                          |
| Hanterings agenter           | Inbyggda MAs                        | Följande MAs har tagits bort i MIM 2016: </br> 1. * * MA för FIM-certifikat hantering </br>2. * * MA för Lotus-anteckningar</br> 3. * * MA för SAP R/3 </br> Lotus Notes-och SAP R/3-MAs har ersatts med nya versioner. Mer information finns i [senaste versions historik för Connector & Hämta](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Hanterings agenter           | ECMA1                               | Utöknings ramverket ECMA1/XMA har ersatts av ECMA 2,0. Det krävs en uppdatering av befintliga ECMA1-hanterings agenter med ECMA 2.0-anslutningar.                                                                                                                                          |
| Hanterings agenter           | Köra anslutningar utanför proc      | Den här funktionen kommer inte att ersättas. Synkroniseringstjänsten anropar alltid anslutnings tjänsten i samma process. Det är kopplingens ansvar att starta och hantera den andra processen. |
| Hanterings agenter           | Konfigurera visnings namn för partition    | Den här funktionen kommer inte att ersättas. Det här alternativet användes bara för att ange ett alternativt namn för en partition i WMI-gränssnitten.                                                                                                                                                                       |
| Köra profiler                | Kombinerade profiler                   | De kombinerade profilerna delta import/Sync, fullständig import/delta-synkronisering och fullständig import/synkronisering tas bort. Du bör använda kör profiler med två steg i stället. 

> [!NOTE]
> Du bör endast behålla kombinerade körnings profiler i miljöer där prestandan skulle påverkas av ett stort antal befintliga disconnector.


| **Kategori**                | **Föråldrad funktion**              | **Ersätta och kommentera**           |
|--------|-------|---|    
| Attributets prioritet | Flera huvud/samma prioritet                       | Samma prioritet kommer att tas bort. Det finns ingen ersättning för den här funktionen. Du bör konfigurera manuell prioritet i stället. Du kan fortsätta att använda den här funktionen om din miljö har en hanterings agent för FIM-tjänsten distribuerad. Den här hanterings agenten ger inte manuell prioritet för att undvika export-inte-överordnade för deklarativ etablering. |
| Anslut till regler           | Anslut till objekt typen "any"                             | Den här funktionen kommer inte att ersättas. Alla kopplings regler ska uttryckligen definiera den metaversum objekt typ som de försöker ansluta till.       |
| Attribut flöden      | Avmarkera "Tillåt null-värden" för exporterade värden            | Den här funktionen kommer inte att ersättas. "Tillåt nullvärden" är alltid markerat. Se till att du har valt "Tillåt nullvärden" i din aktuella miljö.  |
| Attribut flöden      | "Återkalla inte attribut"                            | Den här funktionen kommer inte att ersättas. Attribut kommer alltid att återkallas, vilket är den bästa metoden.  |
| Regel tillägg      | Köra metaversum-och ma-regler utanför proc | Den här funktionen kommer inte att ersättas. Flödes reglerna för metaversum och-attribut körs i samma process som Synkroniseringsmotorn.       |
| Regel tillägg      | Transaktions egenskaper                                | Den här funktionen kommer inte att ersättas. Undvik att överföra data mellan inkommande, etablering och utgående synkronisering med hjälp av den här verktygs klassen.  |
| Regel tillägg      | ExchangeUtils: Create55\* metoder                     | Metoderna för att skapa objekt för Exchange 5,5-servrar kommer att tas bort.        |
| Gränssnitt            | Mms_Metaverse                                        | Alla medlemmar i ClmUtils-klass kommer att tas bort i nästa version.   |

## <a name="next-steps"></a>Nästa steg
Läs mer om:

- Microsoft Identity Manager har fortfarande mycket gemensamt med föregångaren Forefront Identity Manager. Om du fortfarande använder FIM, eller om du vill få tillgång till ytterligare dokumentation, kan du ta en titt på [dokumentationsöversikten för FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Överväganden vid topologier för distribution av MIM](topology-considerations.md) Den här artikeln beskriver flera topologier för distribution som du kan överväga att implementera.
- [Guide för kapacitets planering](capacity-planning-guide.md) Du kan använda den här guiden, tillsammans med test miljöer, för att förstå lämplig omfattning för distributionen.
