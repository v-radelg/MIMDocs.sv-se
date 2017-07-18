---
title: "Guide för kapacitetsplanering | Microsoft Docs"
description: "Den här guiden hjälper dig att förstå de variabler du bör tänka på innan du distribuerar MIM 2016, inklusive belastningsnivåer och policybeslut."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/21/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1eadf7cff67d65c35f784adad94b5032d2792824
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Guide för kapacitetsplanering
<a id="capacity-planning-guide" class="xliff"></a>

Med Microsoft Identity Manager (MIM) kan du skapa, uppdatera och ta bort användarkonton i hela organisationen. Det ger även slutanvändare möjligheten att hantera sina egna konton med självbetjäningsfunktioner. Även i en liten miljö kan alla dessa åtgärder bli en stor arbetsbörda på kort tid.

Innan du börjar med MIM använder du den här guiden, tillsammans med testmiljöerna, för att förstå lämplig omfattning för distributionen. Den här artikeln går igenom ett antal vanliga faktorer som du bör tänka på. Varje distribution är emellertid unik och det bästa sättet att komma fram till vilka servrar, topologier och vilken hårdvara som bäst passar för dina behov är därför att testa dina scenarier i ett labb.

Om du ännu inte har bekantat dig med MIM 2016 och dess komponenter bör du läsa mer om  [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) innan du fortsätter.

## Översikt
<a id="overview" class="xliff"></a>
Det finns ett antal variabler som kan påverka den totala kapaciteten och prestandan vid distributionen av Microsoft Identity Manager. De olika sätt på vilka du rent fysiskt distribuerar MIM-komponenterna (topologin) och den maskinvara som komponenterna körs på är avgörande för vilken prestanda och kapacitet du kan förvänta dig av MIM-distributionen. Antalet konfigurationsobjekt för MIM-policyn och deras komplexitet kanske inte är lika självklara, men de är ändå viktiga faktorer som måste tas med i beräkningen vid planeringen av kapacitet. Slutligen, både distributionens  förväntade skala och den belastning som du räknar med att den utsätts för påverkar prestandan och kapaciteten på ett mer påtagligt sätt.

De faktorer som främst påverkar den kapacitet och prestanda som du kan förvänta vid en distribution av MIM 2016 diskuteras i följande tabell.

| Designfaktor | Överväganden |
| ------------- | -------------- |
| Topologi | Distributionen av MIM-tjänsterna till datorerna i nätverket. |
| Maskinvara | Den fysiska maskinvaran och alla virtualiserade maskinvaruspecifikationer som du kör för respektive MIM-komponent. Detta inkluderar konfigurationen av processor, minne, nätverkskort och hårddisk. |
| Konfigurationsobjekt för MIM-policyn | Antalet och typ av konfigurationsobjekt för MIM-policyn, vilka innefattar uppsättningar, hanteringsprincipregler (MPR) och arbetsflöden. |
| Skala | Antalet användare, grupper, beräknade grupper och anpassade objektgrupper, t.ex. de datorer som ska hanteras av MIM 2016. Tänk också på komplexiteten i dynamiska grupper och var noga med att ta med gruppkapsling i beräkningen. |
| Läs in | Användningsfrekvens. Till exempel hur ofta du förväntar dig att nya grupper eller användare skapas, lösenord återställs eller hur ofta portalen besöks under en viss tidsperiod. Observera att arbetsbelastningen kan variera under loppet av en timme, dag, vecka eller ett år. Beroende på vilken komponent det rör sig om kan du välja att utforma för hög belastning eller genomsnittlig belastning. |


## Vara värd för Microsoft Identity Manager-komponenter
<a id="hosting-microsoft-identity-manager-components" class="xliff"></a>

Komponenterna i Microsoft Identity Manager behöver inte finnas på samma dator. Beaktande av de här komponenterna och de fysiska eller virtuella värdmaskinerna är en viktig del av kapacitetsplaneringen.

Maskinvarufaktorer kan påverka prestandan i MIM-miljön. Exempel:
- Hur ser den fysiska diskkonfigurationen ut för den dator som kör MIM 2016-tjänstens SQL-databas? Antalet spindlar som utgör diskkonfigurationen eller distributionen av logg- och datafiler kan i hög grad påverka systemet.

Tänk också på de externa faktorerna för konfigurationen. Exempel:
- Om du använder ett datalagringsnätverk (SAN) som konfiguration för MIM 2016-tjänstens databas, vilka andra program delar detta SAN? De här programmen kan påverka databasens prestanda om de måste konkurrera om de delade diskresurserna för SAN.


## Användare och grupper
<a id="users-and-groups" class="xliff"></a>
Antalet användare och grupper i miljön är en av de viktigaste faktorerna när du funderar på skalan för en distribution. Det finns emellertid flera andra relaterade faktorer du bör ta med i beräkningen inför planeringen.

- Kan användare skapa grupper? Om så är fallet bör du beräkna hur det faktum att användare skapar nya grupper påverkar tillväxttakten för nya grupper i miljön.

- Ska dynamiska grupper distribueras? Räkna ut hur många och vilka typer av dynamiska grupper du kan förvänta dig i miljön.


## Förväntade belastningsnivåer
<a id="expected-load-levels" class="xliff"></a>
Du bör också fundera på vilken typ av belastning som MIM-komponenterna kommer att utsättas för. Du kan förmodligen få en ungefärlig uppskattning av de här uppgifterna genom att titta på de befintliga programmen i din miljö. Här följer ett antal relevanta frågor du bör tänka på:

- Hur ofta förväntar du dig en begäran att gå med i eller lämna en grupp?

- Hur ofta förväntar du dig att en användare skapar en statisk eller dynamisk grupp?

- Hur många icke-användardrivna åtgärder förväntar du dig, t.ex. synkronisering av ändringar från externa system? Se till att du tar hänsyn till belastningar som skapas genom att identitetsuppgifter synkroniseras med externa system.

- Vilka slags scenarier planerar du att distribuera? Olika scenarier bidrar till olika belastningsmönster. Klientdatorer som har MIM 2016-klienten installerad validerar till exempel regelbundet om registrering krävs vid inloggning, vilket ökar systembelastningen.

- Förväntar du dig stora variationer i belastningsnivåerna, från normal till hög belastning? Efter semesterperioder brukar det till exempel ske ett stort antal återställningar av lösenord. Se till att köra systemunderhåll och synkroniseringsscheman vid andra tider än under de förväntade användningstopparna. Ta med höga belastningsperioder i beräkningen när du överväger kapacitetsplaneringen.


## Konfigurationsobjekt för policy
<a id="policy-configuration-objects" class="xliff"></a>

Konfigurationsobjekt för Microsoft Identity Manager-policyn inkluderar hanteringsprincipregler (MPR), uppsättningar, arbetsflöden och synkroniseringsregler för en viss distribution. MIM-distributioner är unika för varje kund eftersom policykonfigurationerna varierar efter varje distributions behov. Viktiga prestandafaktorer som har att göra med konfigurationsobjekten för MIM-policyn är till exempel:

- **Uppsättningar** Varje åtgärd i systemet måste utvärderas gentemot befintliga uppsättningsmedlemskap och uppdateringar som orsakar ändringar i uppsättningsmedlemskap. En enkel förändring såsom ändring av en anställds kontorsnummer får till exempel sällan stora konsekvenser. Andra ändringar, t.ex. ett chefsbyte, kan emellertid sätta igång en dominoeffekt, vilket kan påverka flera objekt på flera nivåer.

- **Hanteringsprincipregler** Hanteringsprincipregler hanterar regler för åtkomstkontroll och sätter igång arbetsflöden. När du skapar hanteringsprincipregler kan du behöva öka antalet uppsättningar, så att du kan registrera olika övergångstillstånd för objekt. De här extra uppsättningarna kan sätta igång ytterligare arbetsflöden, där varje arbetsflöde är kopplat till unika begäranden i systemet. Detta blir sedan ytterligare ett objekt som ska tas med när du planerar kapaciteten.

Konfiguration av MIM-policyn omfattar också beslut om etablering i din miljö. Var noga med att tänka på följande:

- Ska du etablera främmande säkerhetsprinciper i flera skogar i domäntjänster för Active Directory (AD DS)? Det innebär att det genereras fler arbetsflöden och begäranden, vilket leder till extra belastning på systemet.

- Ska du använda kodlös etablering? Om du gör det påverkar det både antalet poster för förväntade regler och kopplade begäranden och arbetsflöden i systemet.


## Se även
<a id="see-also" class="xliff"></a>
- [Topologiska överväganden när du ska distribuera MIM](topology-considerations.md)
- Du kan hämta [Kapacitetsplaneringsguiden för Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) som innehåller mer information om en testversion och resultat av prestandatest.
