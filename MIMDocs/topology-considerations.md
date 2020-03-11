---
title: Topologiguide för distribution | Microsoft Docs
description: Förstå komponenterna i MIM 2016 och få förslag på hur du kan distribuera dem i din miljö.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7f2d78b487e3c20fbd656aad37b67a6382b67df0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044335"
---
# <a name="topology-considerations"></a>Topologiöverväganden
Du kan distribuera Microsoft Identity Manager (MIM)-komponenter på en och samma server eller på flera servrar i flera olika konfigurationer. Vilken topologi du väljer för din distribution påverkar vilken prestanda du kan få ut från MIM. Denna artikel presenterar flera olika topologier som du kan överväga att implementera.


> [!NOTE]
> De här alternativen är tillämpliga för distributioner som endast använder MIM-synkronisering, MIM-tjänst och MIM-portal för identitetshantering.  För distributioner med MIM CM, MIM BHOLD-programsvit eller för privilegierad åtkomsthantering gäller andra distributionsalternativ.


## <a name="mim-components"></a>MIM-komponenter
När du utformar din distributionstopologi är det viktigt att känna till vad varje komponent gör och hur de alla samverkar.

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**MIM-portal** – ett gränssnitt för lösenordsåterställning, grupphantering och administrativa åtgärder.
    -
- **MIM-tjänst** – en webbtjänst som implementerar funktionerna för identitetshantering i MIM 2016.
- **MIM-synkroniseringstjänst** – synkroniserar data med andra system för identitetshantering.
- **Microsoft SQL Server** – MIM-tjänsten och MIM-synkroniseringstjänsten sparar båda data i SQL-databaser.

I nedanstående tabell visas värdalternativen för varje enskild MIM-komponent. De kan ha samma dator som värd eller distribueras på flera servrar och kluster.

| | MIM-portal | MIM-tjänst | MIM-synkroniseringstjänst | SQL Server |
| --- | --- | --- | --- | --- |
| Samma dator | Ja | Ja | Ja | Ja |
| Enskild server | Ja | Ja | Ja | Ja |
| Kluster för utjämning av nätverksbelastning | Ja | Ja | | |
| Serverkluster | | | | Ja |


## <a name="multitier-topology"></a>Topologi i flera skikt
Topologin i flera skikt är den mest tillämpade topologitypen. Den erbjuder störst flexibilitet. MIM-portalen, MIM-tjänsten och databaserna är indelade i separata skikt och distribueras på flera olika datorer. Denna topologi gör skalningen av de olika MIM-komponenterna mer flexibel. Du kan exempelvis skala MIM-portalen horisontellt genom att lägga till ytterligare servrar i ett kluster för utjämning av nätverksbelastning (NLB). På liknande sätt kan du skala MIM-tjänsten genom att använda ett NLB-kluster och genom att öka antalet datorer (noder) i klustret efter behov.

När topologi i flera skikt används utses en särskild dator som värd för varje SQL-databas (en för varje MIM-tjänst och en annan för MIM-synkroniseringstjänsten). Skalbarheten för prestandan hos datorer som är värdar för SQL-databaser kan utökas genom att lägga till eller uppgradera hårdvara, t.ex. genom att uppgradera processorer, lägga till extra processorer, öka eller uppgradera RAM-minnet eller uppgradera hårddiskkonfigurationerna för att öka läs- och skrivbehörigheter samt minska svarstiden.

![Diagram över MIM med topologi i flera skikt](media/MIM-topo-multitier.png)

I denna konfiguration är samma dator värd för MIM-synkroniseringstjänsten och dess databas. Samma prestanda ska dock vara möjlig att uppnå om det finns en dedikerad nätverksanslutning på 1 Gigabit mellan MIM-synkroniseringstjänsten och dess databas när olika datorer är värdar för dem.


## <a name="multitier-topology-with-multiple-mim-services"></a>Diagram över topologi i flera skikt med flera MIM-tjänster
Det kan ta lång tid att synkronisera data med externa system och processen kan utgöra en stor belastning på systemet under tiden den pågår. Om synkroniseringskonfigurationen resulterar i utlösandet av principer med arbetsflöden, delar dessa principer resurser med slutanvändarens arbetsflöden. Sådana problem förstärkas med arbetsflöden för autentisering, som lösenordsåterställningar, som genomförs i realtid och där slutanvändaren väntar på att processen ska slutföras. Genom att tillhandahålla en instans av MIM-tjänsten för slutanvändaråtgärder och en separat portal för synkronisering av administrationsinformation kan du erbjuda bättre svarstider för slutanvändaråtgärder.

![Diagram över topologi i flera skikt med flera MIM-tjänster](media/MIM-topo-multitier-multiservice.png)

Precis som för standardversionen av topologi i flera skikt kan du höja MIM-portalens prestanda genom att använda ett NLB-kluster och öka antalet noder i klustret efter behov.

Datorerna som kör SQL Server och som är värdar för MIM-synkroniseringstjänsten och MIM-tjänstdatabasen påverkar avsevärt den övergripande prestandan för din MIM-distribution. Därför ska du följa rekommendationerna i dokumentationen för SQL Server för att optimera databasprestandan. Mer information finns i följande dokument:

## <a name="see-also"></a>Se även

- Den nedladdnings bara [guiden för kapacitets planering för Forefront Identity Manager (FIM) 2010](https://www.microsoft.com/en-us/download/details.aspx?id=7437) ger mer information om test-och prestanda testnings resultat.
