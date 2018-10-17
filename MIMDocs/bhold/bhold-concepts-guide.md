---
title: Guide för Microsoft BHOLD Suite begrepp | Microsoft Docs
description: Kom igång med MIM 2016-komponenterna genom att installera och konfigurera Synkroniseringstjänsten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 32bd77140cf70047eaa02d363a1348e73783f87a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358847"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Microsoft BHOLD Suite begrepp Guide

Microsoft Identity Manager 2016 (MIM) gör det möjligt för organisationer att hantera hela livscykeln för användaridentiteter och deras associerade autentiseringsuppgifterna. Den kan konfigureras för att synkronisera identiteter centralt hantera certifikat och lösenord och etablerar användare i heterogena system. Med MIM, IT-organisationer definiera och automatisera processer som används för att hantera identiteter från skapandet pensionering.

Microsoft BHOLD-programsvit utökar funktionerna i MIM genom att lägga till rollbaserad åtkomstkontroll. BHOLD gör det möjligt för organisationer att definiera användarroller och för att styra åtkomsten till känsliga data och program. Åtkomsten baseras vad som är lämpligt för de här rollerna. BHOLD-programsvit innehåller tjänster och verktyg som gör det lättare för modellering av relationerna roll i organisationen. BHOLD mappar rollerna till rättigheter och kontrollera att den roll- och associerade behörigheter tillämpas korrekt för användare. Dessa funktioner är helt integrerade med MIM, vilket ger en sömlös upplevelse för slutanvändare och IT-personal som är både.

Den här guiden hjälper dig att förstå hur BHOLD-programsvit fungerar med MIM och innehåller följande avsnitt:

- Rollbaserad åtkomstkontroll
- Attestering
- Analytics
- Rapportering
- Access Management-anslutningstjänsten
- MIM-integrering

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Det är den vanligaste metoden för att styra användaråtkomst till data och program via åtkomstkontrollinformation (DAC). I de vanligaste-implementeringar kan har alla betydande objekt en identifierade ägare. Ägare har möjlighet att bevilja eller neka åtkomst till objektet till andra baserat på individuell identitet eller gruppmedlemskap. I praktiken DAC vanligtvis resulterar i en mängd olika säkerhetsgrupper vissa som återspeglar organisationens struktur, andra som representerar funktionella grupperingar (till exempel jobbtyper eller projekttilldelningar) och andra som består av makeshift samlingar med användare och enheter som är länkade för flera tillfälliga. Eftersom organisationer växer, blir det allt svårare att hantera medlemskap i dessa grupper. Till exempel om en anställd har överförts från ett projekt till en annan, måste de grupper som används för att styra åtkomsten till projekt tillgångar uppdateras manuellt. I sådana fall kan är det inte ovanligt att misstag ska ske, misstag som kan störa projekt säkerhets- eller produktivitet.

MIM innehåller funktioner som hjälper till att minska det här problemet genom att tillhandahålla automatisk kontroll över medlemskap för gruppen och distribution. Men behandlar detta inte inbäddade komplexitet Prolifererande grupper som inte nödvändigtvis relaterade till varandra på ett mer strukturerat sätt.

Ett sätt att avsevärt minska den här utbredningen är genom att distribuera rollbaserad åtkomstkontroll (RBAC). RBAC inte avlägsna DAC.  RBAC bygger på DAC genom att tillhandahålla ett ramverk för klassificering av användare och IT-resurser. På så sätt kan du se explicit deras relation och åtkomstbehörigheter som är lämpliga enligt denna klassificering. Till exempel genom att tilldela en användare attribut som anger användarna befattning och projekttilldelningar, användaren kan beviljas åtkomst till verktyg som behövs för användarens jobb och data som användaren behöver för att bidra till ett visst projekt. När användaren tar ett annat jobb och projekttilldelningar, ändring av attribut som anger användarens befattning och projekt blockerar automatiskt åtkomst till de resurser som krävs endast för den föregående positionen för användare.

Eftersom roller kan finnas i roller på ett hierarkiskt sätt (till exempel rollerna för försäljningschef och säljrepresentant kan finnas i försäljning mer allmänna rollen), är det enkelt att tilldela lämpliga behörigheter för specifika roller och ge fortfarande ännu behörighet för alla som delar den mindre restriktiva rollen. Till exempel all medicinska personal kan få rätt att visa en patienter poster i ett sjukhus, men endast läkare (en Underroll till rollen medicinska) kan få möjlighet att ange villkoren för patienten. På samma sätt kan användare som tillhör rollen tjänstemän nekas åtkomst till patientjournaler förutom fakturering lagerarbetare (en Underroll till rollen tjänstemän), som kan beviljas åtkomst till dessa delar av en patienter poster som krävs för att debitera patienten för tjänster tillhandahålls av sjukhuset.

En annan fördel med RBAC är möjligheten att definiera och genomdriva uppdelning av uppgifter (matjordsutläggning). På så sätt kan en organisation att definiera kombinationer av roller som beviljar behörigheter som inte bör lyda av samma användare, så att en viss användare inte kan tilldelas roller som används för att initiera en betalning och för att auktorisera en betalning, till exempel. RBAC ger möjlighet att använda typen av princip automatiskt i stället för att behöva utvärdera en effektiv för principen per användare.

### <a name="bhold-role-model-objects"></a>BHOLD rollen modellobjekt

Du kan använda BHOLD-programsvit för att ange och organisera roller inom din organisation kartan användare till roller och kartan lämpliga behörigheter till roller. Den här strukturen kallas en roll-modell och den innehåller och ansluter fem typer av objekt: 

- Organisationsenheter
- Användare
- Roller
- Behörigheter
- Program

#### <a name="organizational-units"></a>Organisationsenheter

Organisationsenheter (OrgUnits) är de viktigaste sätten som användare är ordnade i modellen för BHOLD-rollen. Varje användare måste tillhöra minst en OrgUnit. (I själva verket när en användare tas bort från den senaste organisationsenheten i BHOLD användarposten data tas bort från BHOLD-databas.)

> [!Important]
> Organisationsenheter i modellen för BHOLD-roll ska inte förväxlas med organisationsenheter i Active Directory Domain Services (AD DS). Normalt baseras organisationsenhetsstrukturen i BHOLD på organisation och principer för ditt företag inte kraven för din nätverksinfrastruktur.

Även om det inte krävs, i de flesta fall är organisationsenheter strukturerade i BHOLD som representerar en hierarkisk struktur för den faktiska organisationen liknar den nedan:

![](media/bhold-concepts-guide/org-chart.png)

I det här exemplet skulle rollen modellen organizationalganizatinal enhet för företaget som helhet (som representeras av president eftersom ordförande inte är en del av en enhet för mororganizationalganizatinal) eller organisationsenheten för BHOLD-rot (som alltid Det finns) kan användas för detta ändamål. OrgUnits som representerar företagets avdelningar som leds av vice VD placeras i företagets organisationsenheten. Nästa, organisationens enheter för marknadsföring och försäljning styrelsen skulle läggas till marknadsföring och försäljning organisationsenheter och organisationsenheter som representerar de regionala försäljningschefer placeras i organisationsenheten för den East region försäljningschef. Försäljningsställen som inte hanterar andra användare, skulle göras medlemmar i organisationsenheten för försäljningschef east region. Observera att användare kan vara medlemmar i en organisationsenhet på vilken nivå. En administrativ assistent som inte är en chef och rapporter direkt till en vice president, skulle till exempel vara medlem i den Vp organisationsenhet.

Förutom som representerar organisationsstrukturen kan organisationsenheter också användas för att gruppera användare och andra organisationsenheter enligt funktionella kriterier, till exempel projekt eller specialisering. Följande diagram visar hur organisationens enheter som skulle användas för att gruppera försäljningsställen enligt kundtyp:

![](media/bhold-concepts-guide/org-chart-02.png)

I det här exemplet skulle varje säljaren tillhör två organisationsenheter: en som representerar den associera plats i organisationens hanteringsstruktur och en som representerar den associera kundbas (försäljning eller företagets). Varje organisationsenhet kan tilldelas olika roller som i sin tur kan tilldelas olika behörigheter för åtkomst till organisationens IT-resurser. Dessutom ärvas roller från överordnade organisationsenheter, förenkla processen för sprida roller till användare. Specifika roller kan å andra sidan, förhindras från att vara ärvt, se till att en viss roll är bara kopplade till lämpliga organisationsenheter.

OrgUnits kan skapas i BHOLD-programsvit med hjälp av BHOLD-kärna webbportal eller genom att använda Generator för BHOLD-modellen.

#### <a name="users"></a>Användare

Som nämnts ovan måste varje användare tillhöra minst en organisationsenhet (OrgUnit). Eftersom organisationsenheter är den primära mekanismen för att associera en användare till roller, i flesta organisationer tillhör en viss användare flera OrgUnits att göra det enklare att associera roller med den användaren. I vissa fall, men kan det vara nödvändigt att koppla en roll till en användare förutom eventuella OrgUnits som användaren tillhör. Därför måste kan en användare tilldelas direkt till en roll, samt erhålla roller från OrgUnits som användaren tillhör.

När en användare inte är aktiv i organisationen (Tag bort för medicinska ledighet, till exempel) kan användaren stängas av, som återkallar alla användarens behörigheter utan att ta bort användaren från rollen modellen. Vid tillbaka till tull kan kan användaren återaktiveras, vilket återställer alla behörigheter som beviljats av användarens roller.

Objekt för användare kan antingen skapas individuellt i BHOLD via webbportalen för BHOLD-kärna eller importeras gruppvis med hjälp av BHOLD-modellen Generator eller genom för att importera användarinformation från med Access Management-anslutningstjänsten med FIM-synkroniseringstjänsten sådana källor som Active Directory Domain Services eller hr-program.

Användare kan skapas direkt i BHOLD utan att använda FIM-synkroniseringstjänsten. Detta kan vara användbart när modellering roller i en Förproduktion eller testmiljö. Du kan också tillåta externa användare (till exempel anställda i en underleverantörernas) till tilldelas roller och därmed få åtkomst till IT-resurser utan att anställda-databas. Dessa användare kommer dock inte att kunna använda BHOLD Self service-funktioner.

#### <a name="roles"></a>Roller

Som vi nämnde tidigare, under rollbaserad modellen för åtkomstkontroll (RBAC), behörigheter som är associerade med roller i stället för enskilda användare. Detta gör det möjligt att ge varje användare behörighet att utföra användarens uppgifter genom att ändra användarens roller i stället separat bevilja eller neka användarbehörighet. Det betyder tilldelning av behörigheter kräver inte längre IT-avdelningen deltagande, men i stället kan utföras som en del av hantering av företaget. En roll kan aggregera behörigheter för åtkomst till olika system, antingen direkt eller via underroller, vilket ytterligare minskar behovet av IT-utrustning vid hantering av användarbehörigheter.

Det är viktigt att Observera att roller är en funktion i RBAC-modellen. roller tillhandahålls vanligtvis inte target program. Den här möjliggör RBAC för att användas tillsammans med befintliga program som inte är utformade för att använda roller eller att ändra rollen definitioner för att uppfylla behoven hos ändra affärsmodeller utan att behöva ändra själva programmen. Om ett målprogram är utformat för att använda roller, kan du associerade roller i modellen för BHOLD-roll med motsvarande programroller genom att behandla de programspecifika rollerna som behörigheter.

BHOLD, att du kan tilldela en roll till en användare främst genom två sätt:

- Genom att tilldela en roll till en organisations enhet (organisationsenhet) som användaren är medlem
- Genom att tilldela en roll direkt till en användare

En roll som tilldelats till en överordnad organisationsenhet eventuellt kan ärvas av dess medlem organisationsenheter. När en roll tilldelas eller ärvs av en organisationsenhet, kan det anges som en effektiv eller föreslagna roll. Om det är en effektiv roll tilldelas rollen som med alla användare i organisationsenheten. Om det är en föreslagna roll måste vara aktiverat för varje användare eller medlem organisationsenhet att börja gälla för den användaren eller organisatorisk enhet medlemmar. Detta gör det möjligt att tilldela användare till en delmängd av de roller som är associerade med en organisationsenhet i stället för att automatiskt tilldela alla roller för den organisationsenhet för alla medlemmar. Dessutom roller kan ges start- och slutdatumen och begränsningar kan läggas på procentandelen användare i en organisationsenhet som en roll kan gälla.

Följande diagram illustrerar hur en enskild användare kan tilldelas en roll i BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

I det här diagrammet roll A tilldelas till en organisationsenhet som en roll som kan ärvas och så ärvs av dess medlem organisationsenheter och alla användare inom dessa organisationsenheter. Som en föreslagna roll tilldelas rollen B för en organisationsenhet. Den måste aktiveras innan en användare i organisationsenheten kan auktoriseras med rollens behörigheter. C är en effektiv roll, så att dess behörigheter gäller direkt för alla användare i organisationsenheten. Rollen D är länkad direkt till användaren och så dess behörigheter gäller direkt för den användaren.

Dessutom kan kan en roll aktiveras för en användare baserat på en användares attribut. Mer information finns i attributbaserade auktorisering.

#### <a name="permissions"></a>Behörigheter

En behörighet i BHOLD motsvarar en auktorisering som importerats från en målprogrammet. Det vill säga när BHOLD konfigureras för att arbeta med ett program, får den en lista över godkännanden som BHOLD kan länkas till roller. Till exempel när Active Directory Domain Services (AD DS) har lagts till av BHOLD som ett program, får den en lista med säkerhetsgrupper som som BHOLD-behörigheter kan länkas till roller i BHOLD.

Behörigheter som är specifika för program. BHOLD ger en enhetlig vy över behörigheter så att behörigheter kan associeras med roller utan rollen hanterare kan förstå implementeringen av behörigheter. I praktiken är kan olika system tvinga en behörighet på olika sätt. Den programspecifika anslutningen från FIM-synkroniseringstjänsten så att programmet anger hur ändrade behörigheter för en användare har angetts till programmet. 

#### <a name="applications"></a>Program

BHOLD implementerar en metod för att använda rollbaserad åtkomstkontroll (RBAC) för externa program. Det vill säga när BHOLD etableras med användare och behörigheter från ett program, kan BHOLD koppla dessa behörigheter till användare genom att tilldela roller till användare och sedan länka behörigheter i rollerna. Programmets bakgrundsprocess kan sedan mappa rätt behörigheter till sina användare baserat på rollbehörighet/mappningen i BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Utveckla förebild BHOLD-programsvit

Så att du utvecklar din modell för rollen, ger BHOLD-programsvit modellen Generator, ett verktyg som är både enkelt att använda och omfattande.

Innan du använder modellen Generator, måste du skapa ett antal filer som definierar de objekt som modell Generator används för att konstruera rollen modellen. Information om hur du skapar de här filerna finns i Microsoft BHOLD Suite Teknisk referens.

Det första steget i med hjälp av Generator för BHOLD-modellen är att importera de här filerna för att läsa in de grundläggande byggstenarna i modellen Generator. När filerna har lästs, kan du ange villkor som modell Generator används för att skapa flera klasser av roller:

- Medlemskapsroller som har tilldelats en användare utifrån OrgUnits (organisationsenheter) som användaren tillhör
- Attributet roller som har tilldelats en användare baserat på användarattribut i BHOLD-databas
- Föreslagna roller som är kopplade till en organisationsenhet men måste vara aktiverat för specifika användare
- Ägarskap roller som ger en användarkontroll över organisationsenheter och roller som ägare inte har angetts i de importerade filerna

> [!Important]
> När du överför filer, Välj den **Behåll befintlig modell** kryssrutan bara i testmiljöer. I produktionsmiljöer, måste du använda modellen Generator för att skapa den första modellen för rollen. Du kan inte använda den för att ändra en befintlig roll-modell i BHOLD-databas.

Du kan sedan exportera modellen rollen till BHOLD-databas i form av en XML-fil när modellen generatorn skapar rollerna i rollen modellen.

### <a name="advanced-bhold-features"></a>Avancerade BHOLD-funktioner

Föregående avsnitt beskrivs de grundläggande funktionerna i rollbaserad åtkomstkontroll (RBAC) i BHOLD. Det här avsnittet beskrivs ytterligare funktioner i BHOLD som ger förbättrad säkerhet och flexibilitet till din organisations implementering av RBAC. Det här avsnittet innehåller översikter över följande BHOLD-funktioner:

- kardinalitet
- Uppdelning av uppgifter
- Kontext anpassningsbar behörigheter
- Attributbaserad auktorisering
- Flexibla attributtyper


#### <a name="cardinality"></a>kardinalitet

*Kardinalitet* refererar till implementeringen av affärsregler som är utformade för att begränsa antalet gånger som två entiteter kan vara relaterade till varandra. När det gäller BHOLD, kan kardinalitet regler fastställas för roller, behörigheter och användare.

Du kan konfigurera en roll för att begränsa följande:

- Det maximala antalet användare som en föreslagna roll kan aktiveras
- Det maximala antalet underroller som kan länkas till rollen
- Det maximala antalet behörigheter som kan länkas till rollen

Du kan konfigurera en behörighet för att begränsa följande:

- Det maximala antalet roller som kan länkas till behörighet
- Det maximala antalet användare som kan beviljas behörighet

Du kan konfigurera en användare för att begränsa följande:

- Det maximala antalet roller som kan länkas till användaren
- Det maximala antalet behörigheter som kan tilldelas till användare via rolltilldelningar

#### <a name="separation-of-duties"></a>Uppdelning av uppgifter

Uppdelning av uppgifter (matjordsutläggning) är en business-princip som söker efter att förhindra att enskilda användare från att få möjlighet att utföra åtgärder som inte får vara tillgängliga för en enskild person. En medarbetare bör till exempel går inte att begära en betalning och auktorisera betalningen. Principen om Matjord gör det möjligt för organisationer att implementera ett system av kontroller och balanser att minimera sina exponeringen för risk från medarbetare fel eller som ett fel.

BHOLD implementerar Matjord genom att låta dig definiera inkompatibla behörigheter. När behörigheterna definieras BHOLD tillämpar Matjord genom att förhindra att skapandet av roller som är länkade till inkompatibla behörigheter, oavsett om de är länkad direkt eller genom arv och genom att förhindra att användare inte tilldelas flera roller som, när kombineras, beviljar inkompatibla behörigheter igen genom att direkt tilldelning eller via arv. Du kan också kan konflikter åsidosättas.

#### <a name="context-adaptable-permissions"></a>Kontext anpassningsbar behörigheter

Du kan minska det totala antalet behörigheter som du behöver hantera genom att skapa behörigheter som kan ändras automatiskt baserat på ett objektattribut. Kontext anpassningsbar behörigheter (CAP) kan du definiera en formel som ett attribut för behörigheten som ändrar hur behörigheten används av programmet som är associerade med behörigheten. Du kan till exempel skapa en formel som ändras behörighet till en mapp (via en säkerhetsgrupp som är associerade med mappens access control list) baserat på om en användare som tillhör en organisations enhet (organisationsenhet) som innehåller heltid eller minimera anställda. Om användaren flyttas från en organisationsenhet till en annan ny behörighet tillämpas automatiskt och gamla behörigheten är inaktiverad. 

CAP-formeln kan fråga värdena för attribut som har tillämpats för program, behörigheter, organisationsenheter och användare.

#### <a name="attribute-based-authorization"></a>Attributbaserad auktorisering

Ett sätt att kontrollera om en roll som är länkad till en organisationsenhet (organisationsenhet) är aktiverad för en viss användare i organisationsenheten är att använda attributbaserade auktorisering (ABA). Genom att använda ABA kan aktivera du automatiskt en roll bara när vissa regler baserat på en användares attribut är uppfyllda. Du kan till exempel länka en roll till en organisationsenhet som aktiveras för en användare endast om användarens befattning matchar befattning i ABA-regeln. Detta eliminerar behovet av att manuellt aktivera en föreslagna roll för en användare. I stället kan en roll aktiveras för alla användare i en organisationsenhet som har ett attributvärde som uppfyller rollens ABA-regel. Regler kan kombineras så att en roll aktiveras endast när en användares attribut uppfyller alla ABA-regler som angetts för rollen.

Det är viktigt att notera att resultaten av ABA regeln tester begränsas av Kardinalitetsinställningar. Om inställningen kardinalitet för en regel som anger att mer än två användare kan tilldelas en roll, och om en regel för ABA annars vill aktivera en roll för fyra användare, till exempel aktiveras rollen endast för de första två användare som skickar ABA-test.

#### <a name="flexible-attribute-types"></a>Flexibla attributtyper

Systemet av attribut i BHOLD är mycket anpassningsbara. Du kan definiera nya typer för sådana objekt som användare, organisationens enheter (organisationsenheter) och roller. Attribut kan vara definierad så att värden är heltal, booleskt (Ja/Nej), alfanumeriska, datum, tid och e-postadresser. Attribut kan anges som enda värden eller en lista med värden.

## <a name="attestation"></a>Attestering

BHOLD-programsvit innehåller verktyg som du kan använda för att verifiera att enskilda användare har fått behörighet att utföra sina uppgifter. Administratören kan använda på portalen som tillhandahålls av BHOLD-attestering-modulen för att utforma en hantera attesteringsprocessen.

Attesteringsprocessen med kampanjer i vilken kampanj förvaltare ges möjlighet och innebär att kontrollera att de användare som är ansvarig har lämplig åtkomst till BHOLD-hanterade program och rätt behörigheter på dessa program. En kampanjägare är utsedda att övervaka kampanjen och säkerställa att kampanjen utförs korrekt. Du kan skapa en kampanj för att förekomma en gång eller återkommande.

Steward för en kampanj kommer normalt att en chef som kommer intyga åtkomsträttigheterna för användare som tillhör en eller flera organisationsenheter som chefen är ansvarig. Förvaltare kan väljas automatiskt för de användare som har godkänt dem i en kampanj baserat på användarattribut eller förvaltare för en kampanj kan definieras genom att lista dem i en fil som kopplar varje användare som godkänts i kampanjen till en steward.

När en kampanj börjar BHOLD skickar ett e-postmeddelande till kampanjen förvaltare och ägare och sedan skickar regelbundna påminnelser som hjälper dem att underhålla förloppet i kampanjen. Förvaltare dirigeras till en kampanj-portal där de kan se en lista över användare som är ansvarig och de roller som är tilldelade till dessa användare. Förvaltare kan sedan bekräfta om de ansvarar för var och en av användarna som visas och godkänna eller neka åtkomsträttigheterna för var och en av användarna som visas.

Kampanjen ägare även använda portalen för att övervaka förloppet för kampanjen och kampanjaktiviteter loggas så kampanj ägare kan analysera de åtgärder som vidtagits under kampanjen.

## <a name="analytics"></a>Analytics

En av de viktiga överväganden när implementerar ett omfattande rights-baserad åtkomst (RBAC) system är balans mellan upprätthålla strikta åtkomstkontroll och undvika onödiga (eller, sämre, oväntat) hinder för att få åtkomst till. Det enklare att underhålla balans ofta resulterar i en access-control-struktur som är så komplex att oväntade samverkan mellan principer är nästan oundvikligt med.

Därför finns är det viktigt för att kunna analysera effekterna av principer för åtkomstkontroll innan de börjar faktiskt på plats. Analytics-modulen för BHOLD-programsvit får du möjligheten att utföra den här analysen du utveckla regler som representerar dina principer och sedan som visar användarna vars behörigheter följer eller står i konflikt med regeln. Baserat på den här analysen kan du justera principen eller ändra roller och behörigheter för att undvika konflikter med principen.

BHOLD-Analytics-portalen ger dig möjlighet att skapa regeluppsättningar som består av en eller flera regler som du skapar för att testa en viss princip eller en grupp av principer. En regel består av följande huvuddelar:

- En rubrik och beskrivning som hjälper dig identifiera och Beskriv regeln
- En status som anger om regeln är redo för granskning, som granskas eller godkännas
- Ett element som har angetts (till exempel användare eller behörigheter) som regeln är utformad för att testa
- Valfritt delmängd filter som är uttryck som du kan använda för att välja en lämplig undergrupp till elementet undersökas
- Ett eller flera regeln filter som är uttryck som representerar den princip som testas.

En regel kan testa någon av följande element:

- Användare
- Organisationsenheter
- Roller
- Behörigheter
- Program
- Konton

Följande diagram illustrerar en enkel regel som består av två delmängd regler och två filterregler:

![](media/bhold-concepts-guide/rules.png)

Observera skillnaden i effekten inte en delmängd filter och inte ett filter för regeln: ett delmängd filter som misslyckas tar bort ett elementobjekt från testning genom att efterföljande filter, medan misslyckas ett filter i regeln gör objektet som ska rapporteras som inkompatibla. Endast de objekt som klarar alla delmängd-filter och alla filter för regeln redovisas som överensstämmande.

Varje filter består av en typ, en operator (som är typen beroende), en nyckel (ett av elementen) och ett värde som nyckeln har testats av operatorn. Följande filter skulle till exempel testa om antalet användare i en delmängd av element överskrider 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Typ:**   | Antal   |
| **Nyckel:**  | Användare  |
| **Operator**  | >  |
| **Värde:** | 10 |

Regler för filter kan tre typer av och använda operatorer som är specifika för typ, som anges:

- Attribut
  - < och >
  - = och! =
  - **innehåller**
  - **Innehåller inte**
- Antal
  - < och >
  - = och! =
- Restriktiva
  - **Måste ha någon och måste ha alla**
  - **Får inte innehålla något och kan inte ha alla**
  - **Kan bara ha någon och kan endast ha alla**
  - **Exklusivt har någon och exklusivt har alla**

> [!Note]
> Restriktiva filter kan använda de angivna operatorerna för att testa en nyckel mot en uppsättning med flera värden.

Om du vill testa implementeringen av en uppdelning av uppgifter (matjordsutläggning)-princip som anger att inga användare som har behörighet för begär betalning är också att godkänna betalning behörighet kan du skapa en regel som liknar följande:

|   |  |
|---|--|
|Namn:| Betalning Matjord Test|
|Elementet:| Användare|
|Vissa filter:| Med vilken behörighet som begär betalning|
|Regeln filter: | Det går inte att ha någon behörighet Godkänn betalning|

När du kör den här regeln visar BHOLD analysmodul antalet användare i den valda delmängden (antal användare med behörighet att begära betalning), hur många användare som är kompatibla med regeln och antalet användare som inte är kompatibla med regeln. Du kan sedan Visa icke-kompatibla användare så att du kan korrigera problemet.

Du kan också spara rapporten som en fil med kommaavgränsade värden (CSV) eller XML-fil så att du kan analysera resultaten senare förutom att visa resultatet. Du kan också anpassa den resulterande rapporten för att visa ytterligare information som hjälper dig att bättre förstå effekten. Till exempel om du vill testa användare, kan du visa (eller rapporten) konton kompatibla eller inkompatibla användare så att du kan se vilka program som ingår.

Eftersom en regel kan innehålla flera filter kan du dessutom koppla filter för att testa om det finns en viss uppsättning villkor. Resultatet är produkten av ett booleskt och test av alla filter som standard, men du kan ange att ett OR-test av filterkombination ska utföras.

Till exempel om ditt företag-principen kräver hanterare kan ha behörigheten Ändra betalning eller godkänna betalning behörighet, kan du testa efterlevnad med principen genom att skapa en regel som liknar följande:


|  |  |
|--|--|
|Namn: | Ändra betalningsmetod Matjord Test|
|Elementet: | Användare |
|Vissa filter: | Att ha någon roll Manager|
| Regeln filter: |Måste ha vilken behörighet som helst ändra betalningsmetod </br> Måste ha någon behörighet Godkänn betalning|

Som standard rapporteras alla användare som är en chef som har både ändra betalningen och behörigheten begär betalning som kompatibel. Principen kräver dock att en chef har antingen behörighet inte nödvändigtvis båda. Om du vill testa faktiska efterlevnad med principen, måste du använda eller booleska operatorn med regeln för att avgöra om det finns några chefer som inte har antingen ändra betalning behörighet eller godkänna betalning behörighet.

Till skillnad från andra operatörer den **exklusivt har några** och **exklusivt har alla** operatörer visar kompatibiliteten för objekt som annars skulle undantas av en delmängd-filtret. Till exempel att testa en princip för att alla chefer (och endast chefer) har behörigheten godkänna granskningar kan du skapa en regel på följande sätt:

|  |  |
|--|--|
|Namn: | Granska godkännande Test|
|Elementet: | Användare|
| Vissa filter: | Att ha någon roll Manager
|Regeln filter: | Exklusivt har någon behörighet godkänna granskningar|

Den här regeln ska rapporteras som kompatibla användare som är ansvariga och har behörighet för godkänna granskningar och användare som inte är chefer och som inte har behörigheten godkänna granskningar. Omvänt kan rapporteras chefer som inte har den behörighet och användare som inte är chefer men har den behörigheten som inte är kompatibla.

Som nämndes tidigare, kan du kombinera regler i en regeluppsättning, vilket gör det enklare för dig att kategorisera och hantera regler för att uppfylla dina affärsbehov.

Du kan också definiera en uppsättning av globala filter som, när det är aktiverat, gäller för alla regler som ska testas. Om du behöver ofta undanta en delmängd av poster när du testar regler i olika regeluppsättningar, kan du ange globala filter som du kan aktivera eller inaktivera efter behov.

## <a name="reporting"></a>Rapportering

BHOLD-rapportmodul ger dig möjlighet att visa information i rollen modellen via en mängd olika rapporter. BHOLD-rapportmodul ger en omfattande uppsättning inbyggda rapporter, samt den innehåller en guide som du kan använda för att skapa både grundläggande och avancerade anpassade rapporter. När du kör en rapport kan du omedelbart visa resultaten eller spara resultaten i en Microsoft Excel (.xlsx)-fil. Om du vill visa den här filen med hjälp av Microsoft Excel 2000, Microsoft Excel 2002 eller Microsoft Excel 2003, kan du hämta och installera Microsoft Office Compatibility Pack för Word, Excel och PowerPoint filformat.


Du kan använda BHOLD rapportmodulen huvudsakligen för att skapa rapporter som baseras på aktuella rollinformation. Genererade rapporter för granskning ändringar identitetsinformation har Forefront Identity Manager 2010 R2 en rapporteringsfunktionen för FIM-tjänsten som är implementerad i System Center Service Manager-datalagret. Mer information om FIM rapportering finns i Forefront Identity Manager 2010 och Forefront Identity Manager 2010 R2-dokumentationen i det tekniska biblioteket för Forefront Identity Management.

Kategorier som omfattas av de inbyggda rapporterna är följande:

- Administration
- Attestering
- kontroller
- Aktiv åtkomstkontroll
- Loggning
- Modell
- statistik
- Arbetsflöde

Du kan skapa rapporter och lägga till dem i dessa kategorier, eller du kan definiera dina egna kategorier som du kan placera anpassade och inbyggda rapporter.

När du skapar en rapport i guiden beskriver hur du med följande parametrar:

- Identifierande information, inklusive namn, beskrivning, kategori, användning och målgrupp
- Fält som ska visas i rapporten
- Frågor som anger vilka objekt som ska analyseras
- Ordning i vilken rader ska sorteras
- Fält som används för att dela upp rapporten i avsnitt
- Filter för att förfina de element som returneras i rapporten

Du kan förhandsgranska rapporten när du gjort hittills och spara den om du inte behöver ange ytterligare parametrar i varje steg i guiden. Du kan också flytta tillbaka till föregående steg att ändra parametrar som du angav tidigare i guiden.

## <a name="access-management-connector"></a>Access Management-anslutningstjänsten

Modulen BHOLD Suite Access Management-anslutningstjänsten har stöd för både första och pågående synkronisering av data till BHOLD. Management-anslutningstjänsten åtkomst fungerar med FIM-synkroniseringstjänsten för att flytta data mellan BHOLD-kärna-databasen, MIM metaversum och målprogram och identitetslagringar.

Äldre versioner av BHOLD kräver flera MAs att styra dataflödet mellan MIM och mellanliggande BHOLD-databastabeller. BHOLD-programsvit SP1 kan du definiera hanteringsagenter (MAs) i MIM med dataöverföring direkt mellan BHOLD och MIM i Access Management-anslutningstjänsten.

## <a name="mim-integration"></a>MIM-integrering

En viktig och kraftfull funktion i Forefront Identity Manager 2010 och Forefront Identity Manager 2010 R2 är självbetjäningsportalen som kan användarna visa och hantera sin identitet och medlemskap information. MIM-integrering utökar MIM-portalen med självbetjäning rollhantering. Exempelvis kan en användare med hjälp av BHOLD-funktioner i MIM-portalen kan begära rolltilldelning och kan visa aktiva roller och väntande begäranden. Ytterligare funktioner kan beviljas till delegerade administratörer, som möjligheten att förfrågan rolltilldelningar för andra användare.

Det är viktigt att Observera att BHOLD-tillägg till MIM-portalen stöder självbetjäningsrollen och hanterings- och rapportering. Andra BHOLD administrativa funktioner, samt attestering, tillhandahålls av webbportalerna för BHOLD-moduler som är värd separat från MIM-portalen.

## <a name="next-steps"></a>Nästa steg

- [BHOLD-installationsguide](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
