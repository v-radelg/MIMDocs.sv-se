---
title: Guide för kapacitetsplanering | Microsoft Docs
description: Den här guiden hjälper dig att förstå de variabler du bör tänka på innan du distribuerar MIM 2016, inklusive belastningsnivåer och policybeslut.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 32cdf03ffa0d0d282a6277af766f97e93e3a3f3a
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289021"
---
# <a name="capacity-planning-guide"></a>Guide för kapacitetsplanering

Med Microsoft Identity Manager (MIM) kan du skapa, uppdatera och ta bort användarkonton i hela organisationen. Det ger även slutanvändare möjligheten att hantera sina egna konton med självbetjäningsfunktioner. Även i en liten miljö kan alla dessa åtgärder bli en stor arbetsbörda på kort tid.

Innan du börjar med MIM använder du den här guiden, tillsammans med testmiljöerna, för att förstå lämplig omfattning för distributionen. Den här artikeln går igenom ett antal vanliga faktorer som du bör tänka på. Varje distribution är emellertid unik och det bästa sättet att komma fram till vilka servrar, topologier och vilken hårdvara som bäst passar för dina behov är därför att testa dina scenarier i ett labb.

Om du ännu inte har bekantat dig med MIM 2016 och dess komponenter bör du läsa mer om  [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) innan du fortsätter.

## <a name="overview"></a>Översikt

Det finns ett antal faktorer som kan påverka den totala kapaciteten och prestandan för din Microsoft Identity Manager-distribution:

- Det sätt som du rent fysiskt distribuerar MIM-komponenterna (topologin).
- Den maskinvara som komponenterna körs.
- Antalet och komplexiteten hos konfigurationsobjekt för MIM-policyn är viktiga faktorer att tänka på när du planerar kapaciteten.
- Distributionens förväntade skala distributionen och den förväntade belastningen är mer påtagligt faktorer som påverkar prestanda och kapacitet.

I följande tabell beskrivs de viktigaste faktorerna som påverkar kapacitet och prestanda för en distribution av MIM 2016:

| Designfaktor | Överväganden |
| ------------- | -------------- |
| Topologi | Distributionen av MIM-tjänsterna till datorerna i nätverket. |
| Maskinvara | Den fysiska maskinvaran (fysiska eller virtuella) för varje MIM-komponent inklusive CPU, minne, nätverkskort och hårddiskkonfigurationen. |
| Konfigurationsobjekt för MIM-policyn | Antalet och typ av konfigurationsobjekt för MIM-policyn, vilka innefattar uppsättningar, hanteringsprincipregler (MPR) och arbetsflöden. |
| Skala | Den användare, grupper, beräknade grupper och anpassade objekt som ska hanteras av MIM 2016. Tänk också på komplexiteten i dynamiska grupper och var noga med att ta med gruppkapsling i beräkningen. |
| Läs in | Användningsfrekvens. Åtgärder som till exempel ny grupp eller användare skapas, lösenord återställs eller portal sidbesök per minut eller timme. Observera att arbetsbelastningen kan variera under loppet av en timme, dag, vecka eller ett år. Beroende på vilken komponent det rör sig om kan du välja att utforma för hög belastning eller genomsnittlig belastning. |

## <a name="hosting-microsoft-identity-manager-components"></a>Vara värd för Microsoft Identity Manager-komponenter

Komponenterna i Microsoft Identity Manager behöver inte finnas på samma dator. Beaktande av de här komponenterna och de fysiska eller virtuella värdmaskinerna är en viktig del av kapacitetsplaneringen.

Maskinvarufaktorer kan påverka prestandan i MIM-miljön. Exempel:

- Hur ser den fysiska diskkonfigurationen ut för den dator som kör MIM 2016-tjänstens SQL-databas? Antalet spindlar som utgör diskkonfigurationen eller distributionen av logg- och datafiler kan i hög grad påverka systemet.

Tänk också på de externa faktorerna för konfigurationen. Exempel:

- Om du använder ett datalagringsnätverk (SAN) som konfiguration för MIM 2016-tjänstens databas, vilka andra program delar detta SAN? De här programmen kan påverka databasens prestanda om de måste konkurrera om de delade diskresurserna för SAN.

## <a name="users-and-groups"></a>Användare och grupper

Antalet användare och grupper i miljön är en av de viktigaste faktorerna när du funderar på skalan för en distribution. Det finns emellertid flera andra relaterade faktorer du bör ta med i beräkningen inför planeringen.

- Kan användare skapa grupper? Om så är fallet bör du beräkna hur det faktum att användare skapar nya grupper påverkar tillväxttakten för nya grupper i miljön.

- Ska dynamiska grupper distribueras? Räkna ut hur många och vilka typer av dynamiska grupper du kan förvänta dig i miljön.

## <a name="expected-load-levels"></a>Förväntade belastningsnivåer

Du bör också fundera på vilken typ av belastning som MIM-komponenterna kommer att utsättas för. Du behöver att beräkna belastningen genom att titta på de befintliga programmen i din miljö. Här följer ett antal relevanta frågor du bör tänka på:

- Hur ofta förväntar du dig en begäran att gå med i eller lämna en grupp?

- Hur ofta förväntar du dig att en användare skapar en statisk eller dynamisk grupp?

- Hur många icke-användardrivna åtgärder förväntar du dig, t.ex. synkronisering av ändringar från externa system? Se till att du tar hänsyn till belastningar som skapas genom att identitetsuppgifter synkroniseras med externa system.

- Vilka slags scenarier planerar du att distribuera? Olika scenarier bidrar till olika belastningsmönster. Klientdatorer som har MIM 2016-klienten installerad regelbundet kontrollera till exempel om registrering krävs vid inloggning.

- Förväntar du dig stora variationer i belastningsnivåerna, från normal till hög belastning? Till exempel brukar det till att många lösenordsåterställning efter semesterperioder. Se till att köra systemunderhåll och synkroniseringsscheman vid andra tider än under de förväntade användningstopparna. Ta med höga belastningsperioder i beräkningen när du överväger kapacitetsplaneringen.

## <a name="policy-configuration-objects"></a>Konfigurationsobjekt för policy

Konfigurationsobjekt för MIM-policyn inkluderar Hanteringsprincipregler, uppsättningar, arbetsflöden och Synkroniseringsregler för en distribution. MIM-distributioner är unika för varje kund eftersom policykonfigurationerna varierar efter varje distributions behov. KPI-överväganden omfattar följande konfigurationsobjekt för MIM-policyn:

- **Uppsättningar** Varje åtgärd i systemet måste utvärderas gentemot befintliga uppsättningsmedlemskap och uppdateringar som orsakar ändringar i uppsättningsmedlemskap. Till exempel får en ändring av en anställds kontorsnummer inte ha en stor inverkan. Andra ändringar, t.ex. ett chefsbyte, kan emellertid sätta igång en dominoeffekt, vilket kan påverka flera objekt på flera nivåer.

- **Hanteringsprincipregler** Hanteringsprincipregler hanterar regler för åtkomstkontroll och sätter igång arbetsflöden. Skapa MPR kan skapa behovet av att öka antalet uppsättningar så att du kan registrera olika övergångstillstånd för objekt. De här extra uppsättningarna kan sätta igång ytterligare arbetsflöden, där varje arbetsflöde är kopplat till unika begäranden i systemet. Detta blir sedan ytterligare ett objekt som ska tas med när du planerar kapaciteten.

Konfiguration av MIM-policyn omfattar också beslut om etablering i din miljö. Var noga med att tänka på följande:

- Ska du etablera främmande säkerhetsprinciper i flera skogar i domäntjänster för Active Directory (AD DS)? Det innebär att det genereras fler arbetsflöden och begäranden, vilket leder till extra belastning på systemet.

- Ska du använda kodlös etablering? Om du gör det påverkar det både antalet poster för förväntade regler och kopplade begäranden och arbetsflöden i systemet.

## <a name="next-steps"></a>Nästa steg

- [Topologiska överväganden när du ska distribuera MIM](topology-considerations.md)
- Det nedladdningsbara [Forefront Identity Manager (FIM) 2010 kapacitet Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) innehåller mer information om en testversion och resultaten av prestandatest.
