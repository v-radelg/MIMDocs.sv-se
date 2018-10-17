---
title: MIM inaktuella funktioner och planerar för framtiden | Microsoft Docs
description: Den här artikeln dokument inaktuella funktioner av MIM Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fcf9ec8387761b6f154a95d6100ef54a12d4caf8
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358453"
---
# <a name="deprecated-features"></a>Inaktuella funktioner

Den här artikeln beskriver de inaktuella funktionerna i Microsoft Identity Manager 2016 SP1. Där funktionen finns kvar i Microsoft Identity Manager, stöds det fortfarande. Funktioner rekommenderas inte för nya distributioner så kan de tas bort i en funktionsutgåva.  För utvecklare rekommenderar vi inte använder inaktuella funktioner i nya program eller lösningar.

> [!NOTE]
> Funktioner och funktionalitet som tas bort i Microsoft Identity Manager SP1 identifieras med **. <br>
> Mer information om support [livscykel för Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Microsoft rekommenderar inte kunder börjar nya distributioner av BHOLD-programsvit för Microsoft-komponenter. Befintliga distributioner av BHOLD fortsätter att stödjas. Azure AD nu innehåller [åtkomstgranskningar](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) som ersätter några av funktionerna för BHOLD-attestering kampanj.

## <a name="certificate-management"></a>Certifikathantering 

| **Kategori**                | **Föråldrad funktion**              | **Ersättning och kommentera**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Hanteringsagenter | ** FIM-certifikathantering | Hanteringsagent för FIM-certifikatet har tagits bort i MIM 2016.                                                             |

## <a name="service-and-portal"></a>Tjänst och portal

| **Kategori**                | **Föråldrad funktion**              | **Ersättning och kommentera**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programkonfiguration | Web Service-Konfigurationsgränssnittet (ma-data och mv-data) | Möjligheten att konfigurera FIM-synkroniseringstjänsten via webbtjänsten för FIM-tjänsten tas bort i nästa version.
|

## <a name="synchronization-service"></a>Synkroniseringstjänst 

| **Kategori**                | **Föråldrad funktion**              | **Ersättning och kommentera**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programkonfiguration | Konfigurationsgränssnittet för Web Service | Möjligheten att konfigurera FIM-synkroniseringstjänsten via FIM-tjänsten tas bort i nästa version.                                                          |
| Hanteringsagenter           | Inbyggda MAs                        | Följande MAs har tagits bort i MIM 2016: </br> 1. ** MA för FIM-certifikathantering </br>2. ** MA för Lotus Notes</br> 3. ** MA för SAP R/3 </br> Lotus Notes och SAP R/3 MAs har ersatts med nya versioner. Mer information finns i [senaste versionshistorik för Anslutningsappen och hämta](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Hanteringsagenter           | ECMA1                               | ECMA1/XMA utökningsbarhet framework har ersatts av ECMA 2.0. Uppdatera befintliga ECMA1 hanteringsagenter med ECMA2.0 kopplingar måste anges.                                                                                                                                          |
| Hanteringsagenter           | Kör kopplingar av utanför      | Den här funktionen kommer inte att ersättas. Synkroniseringstjänsten kommer alltid att anropa anslutningen i samma process. Det är ansvar av anslutningsappen som ska starta och hantera den andra processen. |
| Hanteringsagenter           | Konfigurera visningsnamn för partition    | Den här funktionen kommer inte att ersättas. Det här alternativet endast används för att ange ett alternativt namn för en partition i WMI-gränssnitt.                                                                                                                                                                       |
| Körning av profiler                | Kombinerade profiler                   | Den kombinerade profiler import/Deltasynkronisering och fullständig import-/ Deltasynkronisering fullständig import/synkronisering tas bort. Du bör använda körning av profiler med två steg i stället. 

> [!NOTE]
> Endast i miljöer där prestanda kan påverkas av ett stort antal befintliga disconnectors bör du behålla kombinerade körning av profiler.


| **Kategori**                | **Föråldrad funktion**              | **Ersättning och kommentera**           |
|--------|-------|---|    
| Attributprioritet | Multi-höjdpunkter/samma prioritet                       | Samma prioritet tas bort. Det finns ingen ersättning för den här funktionen. I stället bör du konfigurera manuell prioritet. Du kan fortsätta att använda den här funktionen om din miljö har en hanteringsagent för FIM-tjänsten distribueras. Den här hanteringsagenten ger inte manuell prioritet för att undvika export-not-överordnade för deklarativ etablering. |
| Ansluta till regler           | Gå med på ”alla” objekttyp                             | Den här funktionen kommer inte att ersättas. Alla regler för koppling bör explicit definiera typ av metaversumobjekt de försöker ansluta till.       |
| Attributflöden      | Avmarkera ”Tillåt att null-värden” för exporterade värden            | Den här funktionen kommer inte att ersättas. ”Tillåt att null-värden” kommer alltid att väljas. Kontrollera att du har ”Tillåt Null-värden” valt i den aktuella miljön.  |
| Attributflöden      | ”Kommer inte ihåg attribut”                            | Den här funktionen kommer inte att ersättas. Attribut kommer alltid att återkalla, vilket är den bästa metoden.  |
| Tillägg för regler      | Kör metaversum och ma regler tillägg av utanför | Den här funktionen kommer inte att ersättas. Postflödesregler metaversum och attributet körs i samma process som Synkroniseringsmotorn.       |
| Tillägg för regler      | Transaktionsegenskaper                                | Den här funktionen kommer inte att ersättas. Du bör undvika att överföra data mellan inkommande, etablering och utgående synkronisering med hjälp av den här klassen i verktyget.  |
| Tillägg för regler      | ExchangeUtils: Create55\* metoder                     | Metoder för att skapa objekt för Exchange 5.5-servrar kommer att tas bort.        |
| Gränssnittet            | Mms_Metaverse                                        | Vissa klassmedlemmar för ClmUtils tas bort i nästa version.   |

## <a name="next-steps"></a>Nästa steg
Läs mer om:

- Microsoft Identity Manager har fortfarande mycket gemensamt med föregångaren Forefront Identity Manager. Om du fortfarande använder FIM, eller om du vill få tillgång till ytterligare dokumentation, kan du ta en titt på [dokumentationsöversikten för FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Topologiöverväganden för distribution av MIM](topology-considerations.md) denna artikel presenterar flera olika topologier som du kan överväga att implementera.
- [Guide för kapacitetsplanering](capacity-planning-guide.md) du kan använda den här guiden, tillsammans med testmiljöerna, för att förstå lämplig omfattning för distributionen.
