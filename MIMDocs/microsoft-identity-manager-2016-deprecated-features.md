---
title: MIM inaktuella funktioner och planerar för framtiden | Microsoft Docs
description: Den här artikeln dokument inaktuella funktioner för MIM Identity Manager 2016 SP1.
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: a4239f1d69d8a43d70dd38af16e9ef8be62bd33c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288919"
---
# <a name="deprecated-features"></a>Föråldrade funktioner

Den här artikeln beskriver inaktuella funktioner för Microsoft Identity Manager 2016 SP1. Där funktionen finns kvar i Microsoft Identity Manager fortfarande stöds. Funktioner rekommenderas inte för nya distributioner som kan tas bort i en version av funktionen.  För utvecklare rekommenderar vi inte använder inaktuella funktioner i alla nya program eller -lösningar.

> [!NOTE]
> Funktioner och funktionalitet som tas bort i Microsoft Identity Manager SP1 identifieras med **. <br>
> Mer information om support [livscykel för Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Microsoft rekommenderar inte kunder starta nya distributioner av Microsoft BHOLD-programsvit-komponenter. Befintliga distributioner av BHOLD fortsätter att stödjas. Azure AD nu innehåller [åtkomst till granskningar](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) som ersätter vissa BHOLD attestering kampanj funktioner.

## <a name="certificate-management"></a>Certifikathantering 

| **Kategori**                | **Inaktuell funktion**              | **Ersättning och kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Hantering av agenter | ** FIM certifikathantering | Hanteringsagent för FIM-certifikatet har tagits bort i MIM 2016.                                                             |

## <a name="service-and-portal"></a>Tjänst och portal

| **Kategori**                | **Inaktuell funktion**              | **Ersättning och kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programkonfiguration | Web Service configuration interface (ma-data och mv-data) | Möjligheten att konfigurera FIM-synkroniseringstjänsten via webbtjänsten för FIM-tjänsten tas bort i nästa version.
|

## <a name="synchronization-service"></a>Synkroniseringstjänst 

| **Kategori**                | **Inaktuell funktion**              | **Ersättning och kommentar**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programkonfiguration | Web Service configuration interface | Möjligheten att konfigurera FIM-synkroniseringstjänsten via FIM-tjänsten tas bort i nästa version.                                                          |
| Hantering av agenter           | Inbyggda MAs                        | Följande MAs har tagits bort i MIM 2016: </br> 1. ** MA för FIM-certifikathantering </br>2. ** MA för Lotus Notes</br> 3. ** MA för SAP R/3 </br> Lotus Notes och SAP R/3 MAs har ersatts med nya versioner. Mer information finns i [senaste Connector versionshistorik & hämtning](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Hantering av agenter           | ECMA1                               | Ramverk för utökningsbarhet ECMA1/XMA har ersatts av ECMA 2.0. Uppdaterar befintliga ECMA1 hanteringsagenter med ECMA2.0 kopplingar måste anges.                                                                                                                                          |
| Hantering av agenter           | Kör kopplingar utanför av processen      | Den här funktionen kommer inte att ersättas. Synkroniseringstjänsten kommer alltid att anropa kopplingen i samma process. Den ansvarar kopplingen för att starta och hantera andra. |
| Hantering av agenter           | Konfigurera partition visningsnamn    | Den här funktionen kommer inte att ersättas. Det här alternativet endast används för att ange ett alternativt namn för en partition i WMI-gränssnitt.                                                                                                                                                                       |
| Körning av profiler                | Kombinerade profiler                   | Kombinerade profiler import/Deltasynkronisering, fullständig import-/ Deltasynkronisering och fullständig import-synkronisering kommer tas bort. Du bör använda körning av profiler med två steg i stället. 

> [!NOTE]
> Du bör ha kombinerade körningsprofiler endast i miljöer där prestanda kan påverkas av ett stort antal befintliga disconnectors.


| **Kategori**                | **Inaktuell funktion**              | **Ersättning och kommentar**           |
|--------|-------|---|    
| Attributprioritet | Multi-hemdatorupplevelsen/samma prioritet                       | Samma prioritet tas bort. Det finns ingen ersättning för den här funktionen. I stället bör du konfigurera manuell prioritet. Du kan fortsätta att använda den här funktionen om miljön innehåller en hanteringsagent för FIM-tjänsten distribueras. Den här hanteringsagenten ger ingen manuell prioritet för att undvika export inte överordnade för deklarativ etablering. |
| Ansluta till regler           | Delta i ”valfri” objekttyp                             | Den här funktionen kommer inte att ersättas. Alla join-regler ska explicit definiera typ av metaversumobjekt de försöker ansluta till.       |
| Attributflöden      | Avmarkera ”Tillåt Null-värden” för exporterade värden            | Den här funktionen kommer inte att ersättas. ”Tillåt Null-värden” markeras alltid. Kontrollera att du har ”Tillåt Null-värden” markerat i den befintliga miljön.  |
| Attributflöden      | ”Kommer inte ihåg attribut”                            | Den här funktionen kommer inte att ersättas. Attribut kommer alltid att återkalla, vilket är den bästa praxis.  |
| Regeltillägget      | Kör metaversum och ma regler tillägget utanför av processen | Den här funktionen kommer inte att ersättas. Regler för metaversum och attributet flödet körs i samma process som Synkroniseringsmotorn.       |
| Regeltillägget      | Transaktionsegenskaper                                | Den här funktionen kommer inte att ersättas. Du bör undvika att överföra data mellan inkommande, etablering och utgående synkronisering med den här klassen för verktyget.  |
| Regeltillägget      | ExchangeUtils: Create55\* metoder                     | Metoder för att skapa objekt för Exchange 5.5-servrar kommer att tas bort.        |
| Gränssnittet            | Mms_Metaverse                                        | Vissa klassmedlemmar för ClmUtils tas bort i nästa version.   |

## <a name="next-steps"></a>Nästa steg
Läs mer om:

- Microsoft Identity Manager har fortfarande mycket gemensamt med föregångaren Forefront Identity Manager. Om du fortfarande använder FIM, eller om du vill få tillgång till ytterligare dokumentation, kan du ta en titt på [dokumentationsöversikten för FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Topologiöverväganden för distribution av MIM](topology-considerations.md) denna artikel presenterar flera olika topologier som du kan överväga att implementera.
- [Guide för kapacitetsplanering](capacity-planning-guide.md) du kan använda den här guiden, tillsammans med testmiljöerna, för att förstå lämplig omfattning för distributionen.
