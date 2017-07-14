---
title: "PAM-katastrofåterställning | Microsoft Docs"
description: "Lär dig hur du konfigurerar Privileged Access Management för hög tillgänglighet och katastrofåterställning."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 2fab9af837ed11b1f2f7f32c9ced6d79c8cc9d00
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---

# Överväganden för hög tillgänglighet och haveriberedskap i skyddsmiljön
<a id="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment" class="xliff"></a>
I den här artikeln beskrivs överväganden för hög tillgänglighet och haveriberedskap när du distribuerar Active Directory Domain Services (AD DS) och Microsoft Identity Manager 2016 (MIM) för Privileged Access Management PAM.

Företag fokuserar på hög tillgänglighet och haveriberedskap för arbetsbelastningar i Windows Server, SQL Server och Active Directory. Men det är också viktigt att tillgängligheten till skyddsmiljön för privilegierad åtkomsthantering är tillförlitlig. Skyddsmiljön är en viktig del av organisationens IT-infrastruktur eftersom användarna interagerar med dess komponenter för att använda administrativa roller. Mer information om hög tillgänglighet i allmänhet finns i faktabladet [Microsoft – översikt över hög tillgänglighet](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc).

## Scenarier för hög tillgänglighet och haveriberedskap
<a id="high-availability-and-disaster-recovery-scenarios" class="xliff"></a>

När du planerar för hög tillgänglighet och haveriberedskap ska du ta hänsyn till följande frågor:

- Vilka funktioner kan påverkas av ett avbrott?
- Vilka funktioner är verksamhetskritiska och/eller kritiska för IT-uppgifter?
- Vilka risker finns det som kan leda till avbrott i de systemen?

Omfånget för dessa överväganden påverkar den totala kostnaden för distribution och användning. Det gör att organisationer kan prioritera vissa funktioner framför andra, och även godta risken för tillfälliga avbrott för funktioner med lägre prioritet. I följande tabell beskrivs en prioritetsordning som organisationen skulle kunna använda:

| **Skyddsskogsfunktion** | **Relativ prioritet under återställningen** | **Skydd om funktionen är inte tillgänglig** |
| --------------------------- | --------------------- | -------------- |
| Upprätta förtroende         | Låg | Vänta tills skyddsmiljön har återställts |
| Skydd för användare och grupper   | Låg | Vänta tills skyddsmiljön har återställts |
| MIM-administration          | Låg | Vänta tills skyddsmiljön har återställts |
| Aktivering av privilegierade roller  | Medel | Dedikerade smartkortstödda konton för att manuellt lägga till användare i administrativa grupper |
| Resurshantering         | Hög | Dedikerade smartkortstödda konton för att manuellt lägga till användare i administrativa grupper |
| Övervakning av användare och grupper i en befintlig skog | Låg | Vänta tills skyddsmiljön har återställts |

Nu ska vi ta en titt på var och en av de här funktionerna i skyddsskogen.

### Upprätta förtroende
<a id="trust-establishment" class="xliff"></a>

Det måste finnas ett skogsförtroende mellan domänerna i den befintliga skogen och skyddsmiljöns skog. Det är för att användare som autentiseras i skyddsmiljön ska kunna administrera resurser i de befintliga skogarna. Det kan krävas ytterligare konfiguration, bland annat för att tillåta migrering av användare från befintliga domäner i tidigare versioner av Windows Server.

När du ska upprätta förtroende måste de befintliga domänkontrollanterna i skogen vara online, liksom MIM- och AD-komponenterna i skyddsmiljön.  Om det sker ett avbrott i någon av dessa när förtroendet upprättas kan administratören försöka igen när avbrottet åtgärdats.  Om de befintliga domänkontrollanterna i skogen eller skyddsmiljön har återställts efter ett avbrott har MIM även PowerShell-cmdletarna `Test-PAMTrust` och `Test-PAMDomainConfiguration` som kan användas för att kontrollera att ett förtroende fortfarande är upprättat.

### Migrering av användare och grupper
<a id="user-and-group-migration" class="xliff"></a>

När förtroende har upprättats kan du skapa skuggrupper i skyddsmiljön, samt användarkonton för medlemmarna i dessa grupper och godkännare. Det gör att användarna kan aktivera privilegierade roller och återfå giltiga gruppmedlemskap.

När du ska migrera användare och grupper måste de befintliga domänkontrollanterna i skogen vara online, liksom MIM- och AD-komponenterna i skyddsmiljön.   Om det inte går att nå de befintliga domänkontrollanterna i skogen går det inte att lägga till fler användare och grupper i skyddsmiljön, men befintliga användare och grupper påverkas inte. Om det sker ett avbrott i någon av komponenterna under migreringen kan administratören försöka igen när avbrottet åtgärdats.

### MIM-administration
<a id="mim-administration" class="xliff"></a>
När användare och grupper har migrerats kan en administratör göra ytterligare konfigurering av rolltilldelningen i MIM och koppla användare som kandidater för att aktiveras för roller.  De kan även konfigurera MIM-principer för godkännande och Azure MFA.  

När du ska administrera MIM måste skyddsmiljöns MIM- och AD-komponenter vara online.

### Aktivering av privilegierade roller
<a id="privileged-role-activation" class="xliff"></a>
När användare vill aktivera en privilegierad roll måste de autentiseras för skyddsmiljöns domän, och skicka en begäran till MIM.  I MIM ingår SOAP- och REST-API:er samt användargränssnitt i PowerShell och på en webbsida.

När du ska aktivera privilegierade roller måste skyddsmiljöns MIM- och AD-komponenter vara online.  Om MIM är konfigurerat att använda [Azure MFA för aktivering](use-azure-mfa-for-activation.md) av den valda rollen krävs dessutom internetåtkomst för att kontakta tjänsten Azure MFA.

### Resurshantering
<a id="resource-management" class="xliff"></a>
När en användare har aktiverats i rollen kan domänkontrollanten generera en Kerberos-biljett för användaren som kan användas av domänkontrollanter i de befintliga domänerna och identifierar användarens nya tillfälliga gruppmedlemskap.

För resurshantering krävs det att en domänkontrollant för resursen är online, samt en domänkontrollant i skyddsmiljön.  När användare har aktiverats måste inte MIM eller SQL vara online i skyddsmiljön för att utfärda deras Kerberos-biljetter.  (Obs! Med Windows Server 2012 R2 som funktionsnivå för skyddsmiljön måste MIM vara online för att avsluta tillfälliga gruppmedlemskap.)

### Övervakning av användare och grupper i den befintliga skogen
<a id="monitoring-of-users-and-groups-in-the-existing-forest" class="xliff"></a>
MIM innehåller också en PAM-övervakningstjänst som regelbundet kontrollerar användarna och grupperna i de befintliga domänerna och uppdaterar MIM-databasen och AD.  Den här tjänsten behöver inte vara online för rollaktivering och under resurshantering.

När du ska övervaka begäran måste de befintliga domänkontrollanterna i skogen vara online, liksom MIM- och AD-komponenterna i skyddsmiljön.  

## Distributionsalternativ
<a id="deployment-options" class="xliff"></a>
I [miljööversikten](environment-overview.md) illustreras en grundläggande topologi som visar tekniken som inte är avsedd för hög tillgänglighet. Det här avsnittet beskriver hur organisationer med en enda plats och organisationer med flera befintliga platser utökar den topologin för att tillhandahålla hög tillgänglighet.

### Nätverk
<a id="networking" class="xliff"></a>

Nätverkstrafiken mellan datorer i skyddsmiljön bör separeras från de befintliga nätverken, till exempel genom att använda ett annat fysiskt eller virtuellt nätverk.  Beroende på riskerna för skyddsmiljön kan det även krävas oberoende fysiska anslutningar mellan datorerna.  En del redundansklusterteknik har ytterligare krav på nätverksgränssnitt.

Värddatorerna för Active Directory Domain Services och MIM-tjänsterna i skyddsmiljön måste ha dubbelriktad anslutning till resurser i den befintliga skogen om:
- användare ska kunna autentiseras av PRIV-skogens domänkontrollanter
- användare ska kunna begära aktivering
- användare ska kunna ha Kerberos-biljetter som kan användas av resurser i den befintliga skogen
- MIM ska kunna övervaka den befintliga skogens domäner
- MIM ska kunna skicka e-post via e-postservrarna i den befintliga skogen.

### Minimitopologier för hög tillgänglighet
<a id="minimal-high-availability-topologies" class="xliff"></a>
En organisation kan välja vilka funktioner i skyddsmiljön som kräver hög tillgänglighet, med följande begränsningar:

- Det krävs minst två domänkontrollanter för hög tillgänglighet för en funktion som tillhandahålls av skyddsmiljön.  
- För hög tillgänglighet för aktiveringsbegäran krävs det minst två datorer som är värdar för MIM-tjänsten och hög tillgänglighet för SQL Server.
- För hög tillgänglighet för SQL Server med redundanskluster krävs det minst två servrar som tillhandahåller SQL Server och de måste vara andra än domänkontrollanten.
- För att minska risken för angrepp på servrarna bör MIM-tjänsten inte installeras på domänkontrollanten.

Den minsta topologin för hög tillgänglighet för alla funktioner i en skyddsmiljö består av minst fyra servrar och delat lagringsutrymme. Två servrar måste konfigureras som domänkontrollanter för att tillhandahålla Active Directory Domain Services. De andra två servrarna kan konfigureras som ett redundanskluster för SQL Server och MIM-tjänsten.

I en typisk distribution av skyddsmiljön skulle det dessutom finnas en arbetsstation för privilegierad administration för hantering av dessa servrar, samt en övervakningskomponent

I följande diagram visas en möjlig arkitektur:

![skyddstopologi – diagram](media/bastion1.png)

Ytterligare servrar kan konfigureras för var och en av dessa funktioner för att ge högre prestanda under belastningstillstånd eller för geografisk redundans, vilket beskrivs nedan.

### Distributioner med stöd för flera platser
<a id="deployments-supporting-multiple-sites" class="xliff"></a>
Valet av rätt distributionstopologi för resurser som distribueras på flera platser beror på tre faktorer:  
- Mål och risker med hög tillgänglighet och haveriberedskap  
- Maskinvarukapaciteten för hantering av skyddsmiljön  
- Den administrativa arbetsmodellen för varje plats.

En av de enklaste metoderna är att hantera skyddsmiljön på en viss plats.  Under normala förhållanden ansluter användare till MIM-distributionen i platsens skyddsmiljö och begär aktivering och aktiveringen gäller för resurser på varje plats.  Om nätverkslänken bryts eller webbplatsen som är värd för skyddsmiljön inte är tillgänglig kan autentiseringsuppgifter som är offline kommas åt på en annan plats så att det går att utföra tillfällig administration tills nätverket är anslutet igen.  Den här metoden kan vara lämplig för situationer när lokal administration för en viss plats, till exempel ett avdelningskontor, förväntas vara sällsynt och begränsad till att återansluta platsen till resten av företagets nätverk.

![Enkel skyddsmiljö för topologi för flera platser – diagram](media/bastion2.png)

För hög tillgänglighet och haveriberedskap mellan platser kan du också distribuera skyddsmiljöns komponenter på varje plats genom att dela en gemensam PRIV-katalog och SQL-databas.  I den här topologin kan användare på de olika platserna fortsätta arbeta avskilt om nätverkslänken bryts.

![Flerskyddsmiljö för topologi för flera platser – diagram](media/bastion3.png)

En begränsning med den här distributionsmetoden är att SQL Server kräver ett kluster som täcker båda platserna, vilket kan vara komplicerat att distribuera. I så fall kan du överväga att enbart replikera skyddsmiljöns Active Directory (PRIV-skog).  Om det sker ett nätverksavbrott mellan platser kan användare på plats B som redan tidigare har aktiverat sina privilegierade roller fortsätta att administrera resurser på plats B.

![Replikerad skyddsmiljö för topologi för flera platser – diagram](media/bastion4.png)

Om varje plats representerar en separat administrativ gräns, är det också möjligt att distribuera flera oberoende skyddsmiljöer.  Även om varje skyddsmiljö skulle ha samma programvara skulle domännamnen för dem vara olika och det skulle inte finnas några gemensamma faktorer mellan katalogerna och databaserna i de olika skyddsmiljöerna. En användare som vill hantera resurser på en viss plats får aktivera ett användarkonto i platsens skyddsmiljö.

![Oberoende skyddsmiljöer för topologier för flera platser – diagram](media/bastion5.png)

Slutligen är mer komplexa distributioner möjliga, eftersom flera skyddsmiljöer kan konfigureras separat för att hantera resurser i en viss domän.

![Komplex skyddsmiljö för topologier för flera platser – diagram](media/bastion6.png)

### Värdbaserad skyddsmiljö
<a id="hosted-bastion-environment" class="xliff"></a>
En del organisationer har också övervägt att upprätta skyddsmiljön separat från deras befintliga platser. Programvaran för skyddsmiljön kan finnas på en virtualiseringsplattform i företagets nätverk eller hos ett externt webbhotell.  När du utvärderar den här metoden ska du tänka på att:

- Administrationen av skyddsmiljön måste isoleras från administratörskontona i den befintliga domänen om du ska kunna ge skydd mot attacker från de befintliga domänerna.
- Skyddsmiljön kräver TCP/IP-anslutning till domänkontrollanterna i den befintliga domänen.  Du hittar en lista med portar i [Konfigurera en brandvägg för domäner och förtroenden](https://support.microsoft.com/kb/179442).
- För en virtualiserad distribution av Active Directory Domain Services krävs specifika funktioner från virtualiseringsplattform. Mer information om det finns i [Distribution och konfiguration av virtualiserad domänkontrollant](https://technet.microsoft.com/library/jj574223.aspx).
- För en distribution med hög tillgänglighet för SQL Server för MIM-tjänsten krävs särskild lagringskonfiguration, vilket beskrivs i avsnittet [SQL Server-databaslagring](#sql-server-database-storage) nedan.  För närvarande kan inte alla webbhotell erbjuda värdtjänster för Windows Server med diskkonfigurationer som är lämpliga för SQL Server-redundanskluster.

## Procedurer för förberedelse och återställning av distribution
<a id="deployment-preparation-and-recovery-procedures" class="xliff"></a>
När du förbereder dig för att distribuera en skyddsmiljö med hög tillgänglighet och haveriberedskap måste du överväga hur du ska installera Windows Server Active Directory, SQL Server och dess databas i ett delat lagringsutrymme, samt MIM-tjänsten och dess PAM-komponenter.

### Windows Server
<a id="windows-server" class="xliff"></a>
Windows Server innehåller en inbyggd funktion för hög tillgänglighet som gör att flera datorer tillsammans fungerar som ett redundanskluster. De klustrade servrarna är sammankopplade med fysiska kablar och programvara. Om det blir fel på en eller flera av klusternoderna börjar andra noder att tillhandahålla tjänsten (en process som kallas redundans).   Mer information finns i [Översikt över redundansklustring](https://technet.microsoft.com/library/hh831579.aspx).

Kontrollera att operativsystemet och programmen i skyddsmiljön tar emot uppdateringar för säkerhetsproblem. Vissa av dessa uppdateringar kan kräva en omstart av servern. Därför ska du koordinera tiderna för när uppdateringarna tillämpas på servrar för att undvika långvariga avbrott. En metod är att använda [Klustermedveten uppdatering](https://technet.microsoft.com/library/hh831694.aspx) för servrarna i ett redundanskluster i Windows Server.

Servrarna i skyddsmiljön är anslutna till en domän och beroende av domäntjänsterna. Kontrollera att de inte av misstag har konfigurerats med ett beroende av en viss domänkontrollant för till exempel DNS-tjänster.

### Active Directory i skyddsmiljö
<a id="bastion-environment-active-directory" class="xliff"></a>
Windows Server Active Directory Domain Services har inbyggt stöd för hög tillgänglighet och haveriberedskap.

#### Förberedelse
<a id="preparation" class="xliff"></a>
I en typisk produktionsdistribution av Privileged Access Management ingår minst två domänkontrollanter i skyddsmiljön. Anvisningar för hur du konfigurerar den första domänkontrollanten i skyddsmiljön finns i steg 2 i distributionsartiklarna, [Förbereda PRIV-domänkontrollanten](step-2-prepare-priv-domain-controller.md).

Anvisningar för att lägga till en ytterligare domänkontrollant finns i [Installera en replikerad Windows Server 2012-domänkontrollant på en befintlig domän (nivå 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Om domänkontrollanten ska finnas på en virtualiseringsplattform som Hyper-V läser du varningarna i [Distribution och konfiguration av virtualiserad domänkontrollant](https://technet.microsoft.com/library/jj574223.aspx).

#### Återställning
<a id="recovery" class="xliff"></a>
Se till att minst en domänkontrollant finns i skyddsmiljön innan du startar om andra servrar efter ett avbrott.

Active Directory distribuerar FSMO-rollerna ( Flexible Single Master Operation) mellan domänkontrollanter inom en domän enligt beskrivningen i [Så här fungerar åtgärdshanterare](https://technet.microsoft.com/library/cc780487.aspx).  Vid ett avbrott för en domänkontrollant kan du behöva överföra en eller flera av de [Domänkontrollant-roller ](https://technet.microsoft.com/library/cc786438.aspx) som har tilldelats domänkontrollanten.

När du har fastställt att en domänkontrollant inte kan återgå i produktion ska du kontrollera om några roller har tilldelats domänkontrollanten och överföra dem efter behov. Anvisningar finns i [Visa de aktuella innehavarna av verksamhetshanterarrollen](https://technet.microsoft.com/library/cc816893.aspx) och relaterade artiklar.

Du rekommenderas också att kontrollera DNS-inställningarna för datorer som är anslutna till skyddsmiljön, samt domänkontrollanterna i CORP-domäner som har en förtroenderelation till den domänkontrollanten, så inga av dem har inbyggda beroenden på IP-adressen för domänkontrollantens datorn.

### SQL Server-databaslagring
<a id="sql-server-database-storage" class="xliff"></a>
För en distribution med hög tillgänglighet krävs SQL Server-redundanskluster, och instanser för SQL Server-redundanskluster svarar på delad lagring mellan alla noder för databas- och logglagring. Det delade lagringsutrymmet kan bestå av WSFC-klusterdiskar (Windows Server Failover Clustering), diskar på ett SAN-nätverk (Storage Area Network) eller filer som delas på en SMB-server.  Obs! De måste vara tilldelade särskilt för skyddsmiljön. Vi rekommenderar inte att lagringsutrymmet delas med andra arbetsbelastningar utanför skyddsmiljön eftersom det kan äventyra skyddsmiljöns integritet.

### SQL Server
<a id="sql-server" class="xliff"></a>
För MIM-tjänsten krävs en SQL Server-distribution i skyddsmiljön.   För hög tillgänglighet kan SQL distribueras med hjälp av en redundansklusterinstans (FCI). Till skillnad från i fristående instanser skyddas den höga tillgängligheten för SQL-server av förekomsten av redundanta noder i redundansklusterinstanser. Vid ett avbrott eller en planerad uppgradering flyttas resursgruppens ägande till en annan nod i Windows Server-redundansklustret.

Om du bara behöver stöd för haveriberedskap men inte för hög tillgänglighet kan du använda loggöverföring, transaktionsreplikering, replikering av ögonblicksbilder och databasspegling istället för redundansklustring.   

#### Förberedelse
<a id="preparation" class="xliff"></a>
När du installerar SQL Server i skyddsmiljön måste den vara fristående från eventuella andra SQL Server som redan finns i CORP-skogarna.  Vi rekommenderar dessutom att SQL Server distribueras på en dedikerad server som inte är samma som för domänkontrollanten.
Mer information finns i SQL Server-handboken till [AlwaysOn-instanser för redundanskluster](https://msdn.microsoft.com/library/ms189134.aspx).

#### Återställning
<a id="recovery" class="xliff"></a>
Om SQL Server har konfigurerats för haveriberedskap med hjälp av loggöverföring måste du vidta åtgärder för att uppdatera SQL Server vid återställning.  Dessutom måste varje instans av MIM-tjänsten startas om.

Vid ett avbrott för SQL Server eller om anslutningen mellan SQL Server och MIM-tjänsten bryts, rekommenderar vi att du startar om alla MIM-tjänster när SQL Server har återställts.  På så sätt säkerställer du att MIM-tjänsten återupprättar anslutningen till SQL Server.

### MIM-tjänst
<a id="mim-service" class="xliff"></a>
MIM-tjänsten måste också bearbeta aktiveringsbegäran.  Om värddatorn för MIM-tjänsten ska kunna stängas av för underhåll medan aktiveringsbegäran fortfarande tas emot, kan du distribuera flera datorer för MIM-tjänsten.  Observera att MIM-tjänsten inte är inblandad i Kerberos-uppgifter när en användare har lagts till i en grupp.  

#### Förberedelse
<a id="preparation" class="xliff"></a>
Du rekommenderas att distribuera MIM-tjänsten på flera servrar som är anslutna till PRIV-domänen.
Information om hög tillgänglighet finns i Windows Server-dokumenten [ Maskinvarukrav och lagringsalternativ för redundanskluster](https://technet.microsoft.com/library/jj612869.aspx) och [Skapa ett redundanskluster för Windows Server 2012](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx).

Vid produktionsdistribution över flera servrar kan du kan använda Utjämning av nätverksbelastning för att distribuera bearbetningsbelastningen.  Du bör även ha ett enda alias (till exempel A- eller CNAME-poster) så att ett allmänt namn visas för användaren.

>[!IMPORTANT]
> Om du använder en annan teknik för belastningsutjämning än funktionen för utjämning av nätverksbelastning i Windows Server 2012 R2, ska du kontrollera att lösningen omdirigerar sessioner till samma server och inte till slumpmässiga servrar.

I en MIM distribution med flera servrar har varje MIM-tjänst ett externt värdnamn, ett tjänstnamn och ett tjänstpartitionsnamn.  Standardvärdet för tjänstnamnet är datorns namn och standardvärdet för det externa värdnamnet och tjänstpartitionsnamnet konfigureras under installationen av MIM-tjänsten på skärmen där du uppmanas att ange MIM-tjänstserverns adress. Dessa tre namn lagras i filen %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config som attribut `externalHostName`, `serviceName` och `servicePartitionName` i konfigurationsnoden `resourceManagementService`.  

När en MIM-tjänst tar emot en begäran lagras tjänstpartitionens namn som ett attribut i begäran.   Därefter tillåts bara andra installationer av MIM-tjänsten med samma tjänstpartitionsnamn att samverka med begäran.  Om PAM-scenariot omfattar manuella godkännanden eller annan bearbetning av långvariga begäranden måste du därför se till att varje MIM-tjänsten har samma `servicePartitionName`-attribut i konfigurationsfilen.

#### Återställning
<a id="recovery" class="xliff"></a>
Se till att minst en Active Directory-domänkontrollant och SQL Server är tillgängliga i skyddsmiljön innan du startar om MIM-tjänsten efter ett avbrott.  

En arbetsflödesinstans kan endast slutföras av en MIM-tjänstserver som har samma tjänstpartitionsnamn och tjänstnamn som MIM-tjänstservern som startade den.  Om en dator som är värd för MIM-tjänsten som bearbetar begäranden inte kan återtas i bruk efter ett avbrott måste du installera MIM-tjänsten på en ny dator. Efter installationen redigerar du filen *resourcemanagementservice.exe.config* och anger attributen `serviceName` och `servicePartitionName` för den nya MIM-distributionen på den nya MIM-tjänsten till samma värdnamn och tjänstpartitionsnamn som för den dator som tagits ur bruk.

### MIM PAM-komponenter
<a id="mim-pam-components" class="xliff"></a>
Installationsprogrammet för MIM-tjänsten och -portalen innehåller också ytterligare PAM-komponenter, inklusive PowerShell-moduler och två tjänster.

#### Förberedelse
<a id="preparation" class="xliff"></a>
Komponenterna i Privileged Access Management ska installeras på varje dator i skyddsmiljön där MIM-tjänsten installeras.  De kan inte läggas till senare.

#### Återställning
<a id="recovery" class="xliff"></a>
Se till att MIM-tjänsten körs på minst en server efter återställning efter ett avbrott.  Se till att MIM PAM-övervakningstjänsten körs även på den servern med hjälp av `net start "PAM Monitoring service"`.

Om funktionsnivån för skyddsmiljöns skog är Windows Server 2012 R2, använder du kommandot `net start "PAM Component service"` till att kontrollera att MIM PAM-komponenttjänsten också körs på den servern.

