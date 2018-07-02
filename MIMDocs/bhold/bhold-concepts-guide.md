---
title: Guide för Microsoft BHOLD Suite begrepp | Microsoft Docs
description: Kom igång med MIM 2016-komponenterna genom att installera och konfigurera Synkroniseringstjänsten.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/14/2017
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 521025de3dc16a9bda02aed8287faeb3449192c1
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290075"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guide för Microsoft BHOLD Suite-begrepp

Microsoft Identity Manager 2016 (MIM) gör det möjligt för organisationer att hantera hela livscykeln för användaridentiteter och deras associerade autentiseringsuppgifter. Den kan konfigureras för att synkronisera identiteter centralt hantera certifikat och lösenord och etablera användare över heterogena system. Med MIM, IT-organisationer definiera och automatisera processer som används för att hantera identiteter från skapas till pensionering.

Microsoft BHOLD-programsvit utökar dessa funktioner för MIM genom att lägga till rollbaserad åtkomstkontroll. BHOLD kan organisationer definiera användarroller och styra åtkomsten till känsliga data och program. Åtkomsten baseras vad som är lämpligt för dessa roller. BHOLD-programsvit innehåller tjänster och verktyg som förenklar modellering av relationerna roll i organisationen. BHOLD mappar rollerna rättigheter och kontrollera att den roll- och associerade behörigheter på rätt sätt gäller för användare. Dessa funktioner är helt integrerade med MIM, tillhandahåller en integrerad upplevelse för slutanvändare och IT-personal är likadana.

Den här guiden hjälper dig att förstå hur BHOLD-programsvit fungerar med MIM och innehåller följande avsnitt:

- Rollbaserad åtkomstkontroll
- Hälsoattestering
- Analytics
- Rapportering
- Access Management-anslutningstjänsten
- MIM-integrering

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Det är den vanligaste metoden för att styra användarnas åtkomst till data och program via åtkomstkontrollinformation (DAC). I de flesta vanliga implementeringar kan har alla betydande objekt en identifierade ägare. Ägaren har möjlighet att bevilja eller neka åtkomst till objektet till andra baserat på enskilda identitet eller gruppmedlemskap. I praktiken DAC vanligtvis resulterar i en mängd olika säkerhetsgrupper, vissa som återspeglar organisationens struktur, andra som representerar funktionella grupperingar (till exempel jobbtyper eller projekttilldelningar) och andra som består av makeshift samlingar med användare och enheter som är länkade för flera tillfälliga ändamål. Eftersom organisationer växer, blir medlemskap i dessa grupper allt svårare att hantera. Till exempel om en medarbetare har överförts från ett projekt till en annan, måste de grupper som används för att styra åtkomsten till projekt tillgångar uppdateras manuellt. I sådana fall är det inte ovanligt att fel ska inträffa fel som kan störa projektet säkerhets- eller produktivitet.

MIM innehåller funktioner som hjälper till att minska det här problemet genom att tillhandahålla automatisk kontroll över medlemskap för gruppen och distribution. Men åtgärdar detta inte Prolifererande grupper som inte vara relaterade till varandra på ett strukturerat sätt inre komplexitet.

Ett sätt att avsevärt minska den här spridning är genom att distribuera rollbaserad åtkomstkontroll (RBAC). RBAC inte avlägsna DAC.  RBAC bygger på DAC genom att tillhandahålla ett ramverk för klassificering av användare och IT-resurser. På så sätt kan du se explicit deras förhållande och åtkomstbehörigheter som är lämpliga enligt den klassificeringen. Till exempel genom att tilldela en användare attribut som anger användarna befattning och projekttilldelningar användaren kan beviljas åtkomst till verktyg som behövs för användarens jobb och data som användaren behöver bidrar till ett visst projekt. När användaren tar ett annat jobb och annat projekttilldelningar, ändring av attribut som anger användarens befattning och projekt blockerar automatiskt åtkomst till resurser som krävs endast för den föregående positionen för användare.

Eftersom roller kan ingå i roller på ett hierarkiskt sätt (till exempel försäljningschef och säljare roller kan finnas i mer allmänna rollen för försäljning), är det enkelt att tilldela behörighet till specifika roller och ge fortfarande ännu behörighet till alla som delar den mer restriktiva rollen. Till exempel alla medicinska personal kan ges behörighet att visa en patienter poster i ett sjukhus, men endast läkare (en Underroll rollen medicinska) kan få rätt att ange villkoren för tålamod. På liknande sätt kan användare som tillhör rollen skrivfel nekas åtkomst till patientjournaler förutom fakturering lagerarbetare (en Underroll till rollen skrivfel), som kan beviljas åtkomst till de delarna av en patienter poster som krävs för att debiterar tålamod för tjänster tillhandahålls av sjukhus.

En annan fördel med RBAC är möjligheten att definiera och genomdriva uppdelning av uppgifter (Matjord). På så sätt kan en organisation att definiera kombinationer av roller som ger behörigheter som inte ska innehas av samma användare, så att en viss användare inte kan tilldelas roller som används för att initiera en betalning och för att auktorisera en betalning, till exempel. RBAC ger möjlighet att genomdriva sådana automatiskt istället att utvärdera ett effektivt genomförande av principen på per användare.

### <a name="bhold-role-model-objects"></a>BHOLD roll modellobjekt

Du kan använda BHOLD-programsvit för att ange och organisera roller i organisationen, mappa användare till roller och mappa behörighet till roller. Den här strukturen kallas en roll-modell och det innehåller och ansluter fem typer av objekt: 

- Organisationsenheter
- Användare
- Roller
- Behörigheter
- Program

#### <a name="organizational-units"></a>Organisationsenheter

Organisationsenheter (OrgUnits) är de viktigaste sätten som användare är uppdelade i modellen för BHOLD-rollen. Varje användare måste ha minst en OrgUnit. (I praktiken när en användare tas bort från den senaste organisationsenheten i BHOLD användarpost data tas bort från BHOLD-databasen.)

> [!Important]
> Organisationsenheter i modellen för BHOLD-rollen ska inte förväxlas med organisationsenheter i Active Directory Domain Services (AD DS). Normalt är organisationsenhetsstrukturen i BHOLD baserad på organisation och principer för ditt företag inte kraven i din nätverksinfrastruktur.

Även om det inte krävs, i de flesta fall är organisationsenheter strukturerade i BHOLD som representerar den hierarkiska strukturen för faktiska organisationen som liknar den nedan:

![](media/bhold-concepts-guide/org-chart.png)

I det här exemplet skulle rollen modellen organizationalganizatinal enhet för företaget som helhet (representeras av VD, eftersom VD inte är en del av en mororganizationalganizatinal enhet) eller organisationsenheten för BHOLD-rot (som alltid Det finns) kan användas för detta ändamål. OrgUnits som representerar företagets avdelningar leds av vice VD placeras i företagets organisationsenheten. Nästa, organisationens enheter motsvarar marknadsförings- och även skulle läggas till marknadsförings- och organisationsenheter och organisationsenheter som representerar regionala säljare placeras i organisationsenheten för den Östra region försäljningschef. Affärskontakter som inte hanterar andra användare, skulle göras medlemmar i organisationsenheten för försäljningschef Öst region. Observera att användare kan vara medlemmar i en organisationsenhet när som helst. En administrativ assistent som inte är en chef och rapporter direkt till VD skulle till exempel vara medlem i den VD organisationsenhet.

Förutom som representerar organisationsstruktur kan organisationsenheter också användas för att gruppera användare och andra organisationsenheter enligt funktionella kriterier t.ex projekt eller specialisering. Följande diagram visar hur organisationens enheter som ska användas för att gruppera affärskontakter enligt kundens typ:

![](media/bhold-concepts-guide/org-chart-02.png)

I det här exemplet skulle varje säljaren tillhör två organisationsenheter: en som representerar den associeras plats i organisationsstruktur management och en som representerar den associeras kundbas (detaljhandel eller företagets). Varje organisationsenhet kan tilldelas olika roller, som i sin tur kan tilldelas olika behörigheter för åtkomst till organisationens IT-resurser. Dessutom kan roller ärvas från överordnad organisationsenheter, förenkla processen för sprider roller till användare. Å andra sidan kan specifika roller förhindras från att ärvda, säkerställer att en viss roll endast kopplade till organisationsenheterna som är lämpliga.

OrgUnits kan skapas i BHOLD-programsvit med hjälp av BHOLD-Core web-portalen eller genom att använda Generator för BHOLD-modellen.

#### <a name="users"></a>Användare

Som nämnts ovan måste varje användare ha minst en organisationsenhet (OrgUnit). Eftersom organisationsenheter är den primära mekanismen för att associera en användare med roller, i flesta organisationer tillhör en användare flera OrgUnits att göra det enklare att koppla roller till användaren. I vissa fall, men kan den behöva koppla en roll till en användare förutom eventuella OrgUnits som användaren tillhör. Därför måste kan en användare tilldelas direkt till en roll, samt erhålla roller från OrgUnits som användaren tillhör.

När en användare inte är aktiv i organisationen (Tag för medicinska ledighet, till exempel) kan användaren pausas, som återkallar behörigheter för alla användares utan att ta bort användaren från rollen modellen. När du återvänder till tull kan användaren återaktiveras, vilket återställer alla behörigheter som tilldelats av användarens roller.

Objekt för användare kan antingen skapas individuellt i BHOLD via webbportalen för BHOLD kärnor och kan importeras i grupp med hjälp av BHOLD modellen Generator eller genom för att importera användarinformation från med Access Management-anslutningstjänsten med FIM-synkroniseringstjänsten källor som Active Directory Domain Services eller personal-program.

Användare kan skapas direkt i BHOLD utan att använda FIM-synkroniseringstjänsten. Detta kan vara användbart när modeling roller i en Förproduktion eller testmiljö. Du kan också tillåta externa användare (till exempel anställda av en underleverantörer) att tilldelas roller och därmed få åtkomst till IT-resurser utan att anställda-databasen. Dessa användare kommer dock inte använda BHOLD självbetjäning funktioner.

#### <a name="roles"></a>Roller

Som tidigare anges, under rollbaserad modellen för åtkomstkontroll (RBAC), behörigheter som är associerade med roller i stället för enskilda användare. Detta gör det möjligt att ge varje användare behörighet att utföra användarens uppgifter genom att ändra användarens roller i stället separat bevilja eller neka användarbehörighet. Kräver inte längre IT-avdelningen deltagande följaktligen tilldelningen av behörighet, men i stället kan utföras som en del av hantering av företaget. En roll kan aggregera behörigheter för åtkomst till olika system, antingen direkt eller via underroller, vilket minskar behovet av IT-medverkan i Hantera användarbehörigheter ytterligare.

Det är viktigt att Observera att roller är en funktion i RBAC-modellen. roller är vanligtvis inte etablerats målprogram. I det här kan RBAC som ska användas tillsammans med befintliga program som inte är utformade för att använda roller eller ändra rollen definitioner för att möta behoven hos ändra business modeller utan att behöva ändra själva programmen. Om ett målprogram är utformad för roller, kan du associerade roller i modellen för BHOLD-roll med motsvarande programroller genom att behandla programspecifika roller som behörigheter.

BHOLD, att du kan tilldela en roll till en användare i första hand med två mekanismer:

- Genom att tilldela en roll till en organisations enhet (organisationsenhet) som användaren är medlem
- Genom att tilldela en roll direkt till en användare

En roll som tilldelats till en överordnad organisationsenhet eventuellt kan ärvas av dess medlem organisationsenheter. När en roll har tilldelats eller ärvs av en organisationsenhet, kan det anges som en effektiv eller föreslagna roll. Om det är en effektiv roll har alla användare i organisationsenheten tilldelats rollen. Om det är en föreslagna roll måste det aktiveras för varje användare eller medlem organisationsenhet ska bli effektiv för användaren eller organisatorisk enhet medlemmar. Detta gör det möjligt att tilldela användare en delmängd av de roller som är kopplade till en organisationsenhet, i stället för att automatiskt tilldela alla roller för den organisationsenhet för alla medlemmar. Dessutom roller kan få start- och slutdatum och gränser kan placeras på procentuella andelen användare i en organisationsenhet som en roll kan tillämpas.

Följande diagram illustrerar hur en användare kan tilldelas en roll i BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

I det här diagrammet roll A tilldelas till en organisationsenhet som en ärvas roll och så ärvs av dess medlem organisationsenheter och alla användare inom dessa organisationsenheter. Rollen B är tilldelad som en föreslagna roll för en organisationsenhet. Den måste aktiveras innan en användare i organisationsenheten kan auktoriseras med rollens behörigheter. C är ett effektivt roll, så dess behörighet omedelbart gäller för alla användare i organisationsenheten. Rollen D är direkt kopplade till användaren och så dess behörighet omedelbart gäller för den användaren.

Dessutom kan kan en roll aktiveras för en användare baserat på en användares attribut. Mer information finns i attributbaserad auktorisering.

#### <a name="permissions"></a>Behörigheter

En behörighet i BHOLD motsvarar ett tillstånd som importerats från ett målprogram. Det vill säga när BHOLD är konfigurerad för att arbeta med ett program, får den en lista över godkännanden som BHOLD kan länkas till roller. När Active Directory Domain Services (AD DS) läggs till BHOLD som ett program får exempelvis en lista över säkerhetsgrupper som som BHOLD behörigheter kan länkas till roller i BHOLD.

Behörigheter som är specifika för program. BHOLD innehåller en enhetlig behörigheter så att behörigheter kan vara associerade med roller utan rollen chefer att förstå implementeringen av behörigheter. I praktiken kan olika system tillämpa behörigheter på olika sätt. Programspecifika anslutningen från FIM-synkroniseringstjänsten till programmet avgör hur behörighetsändringar för en användare har angetts för det aktuella programmet. 

#### <a name="applications"></a>Program

BHOLD implementerar en metod för att använda rollbaserad åtkomstkontroll (RBAC) för externa program. Det vill säga när BHOLD har etablerats med användare och behörigheter från ett program, associera BHOLD dessa behörigheter med användare genom att tilldela roller till användare och sedan länka behörigheter till roller. Programmets bakgrunden kan sedan mappa rätt behörigheter till sina användare baserat på rollbehörighet/mappningen i BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Utveckla BHOLD-programsvit rollen modellen

För att hjälpa dig att utveckla rollen modellen innehåller BHOLD-programsvit modellen Generator, ett verktyg som är både lättanvända och omfattande.

Innan du använder modellen Generator, måste du skapa ett antal filer som definierar de objekt som modellen generatorn används för att konstruera rollen modellen. Information om hur du skapar de här filerna finns teknisk referens för Microsoft BHOLD Suite.

Det första steget i med hjälp av BHOLD modellen Generator är att importera de här filerna för att läsa in de grundläggande byggstenarna i modellen Generator. När filerna har lästs, kan du ange villkor som modellen generatorn används för att skapa flera klasser av roller:

- I medlemskapsroller som är kopplade till användare baserat på OrgUnits (organisationsenheter) som användaren tillhör
- Attributet roller som är kopplade till användare baserat på användarens attribut i BHOLD-databas
- Föreslagna roller som är kopplade till en organisationsenhet, men måste vara aktiverat för specifika användare
- Ägarskap roller som ger en användarkontroll över organisationsenheter och roller som ägare inte har angetts i de importerade filerna

> [!Important]
> När överföringen av filer, väljer du den **behåller befintliga modellen** kryssrutan bara i testmiljöer. I produktionsmiljöer, måste du använda modellen Generator för att skapa den första rollen modellen. Du kan inte använda den för att ändra en befintlig roll modell i BHOLD-databas.

Efter att modellen Generator skapar rollerna i modellen för rollen, kan sedan du exportera rollen modellen till BHOLD-databas i form av en XML-fil.

### <a name="advanced-bhold-features"></a>Avancerade BHOLD-funktioner

Föregående avsnitt beskrivs de grundläggande funktionerna i rollbaserad åtkomstkontroll (RBAC) i BHOLD. Det här avsnittet beskrivs ytterligare funktioner i BHOLD som ger förbättrad säkerhet och flexibilitet i din organisations implementering av RBAC. Det här avsnittet ger översikter över följande BHOLD-funktioner:

- Kardinalitet
- Uppdelning av uppgifter
- Kontexten anpassningsbar behörigheter
- Attributbaserad auktorisering
- Flexibla attributtyper


#### <a name="cardinality"></a>Kardinalitet

*Kardinalitet* refererar till implementeringen av affärsregler som är utformade för att begränsa antalet gånger som två entiteter kan vara relaterade till varandra. När det gäller BHOLD, kan kardinalitet regler fastställas för roller, behörigheter och användare.

Du kan konfigurera en roll om du vill begränsa följande:

- Det maximala antalet användare som en föreslagna roll kan aktiveras
- Det maximala antalet underroller som kan länkas till rollen
- Det maximala antalet behörigheter som kan kopplas till rollen

Du kan konfigurera en behörighet för att begränsa följande:

- Det maximala antalet roller som kan länkas till behörigheten
- Det maximala antalet användare som kan beviljas behörighet

Du kan konfigurera en användare om du vill begränsa följande:

- Det maximala antalet roller som kan länkas till användaren
- Det maximala antalet behörigheter som kan tilldelas till användare via rolltilldelningar

#### <a name="separation-of-duties"></a>Uppdelning av uppgifter

Uppdelning av uppgifter (Matjord) är en princip för företag som syftar till att förhindra att enskilda användare möjlighet att utföra åtgärder som inte ska vara tillgänglig för en enskild person. En medarbetare ska t.ex, det går inte att begära en betalning och auktorisera betalningen. Principen om Matjord gör det möjligt för organisationer att implementera ett system för kontroller och balanser att minimera sina utsättas för risk från medarbetare fel eller fel.

BHOLD implementerar Matjord genom att du kan definiera inkompatibla behörigheter. När dessa behörigheter har definierats BHOLD tillämpar Matjord genom att förhindra att skapa roller som är länkade till inkompatibla behörigheter om de kopplas direkt eller via arv och genom att förhindra användare från att tilldelas flera roller som, när kombinerade, beviljar inkompatibla behörigheter igen med direkttilldelning eller via arv. Du kan också kan konflikter åsidosättas.

#### <a name="context-adaptable-permissions"></a>Kontexten anpassningsbar behörigheter

Du kan minska det totala antalet behörighet att hantera genom att skapa behörigheter som kan ändras automatiskt baserat på ett objektattribut. Kontexten anpassningsbar behörigheter (CAP) kan du definiera en formel som en behörighet-attribut som ändrar hur behörigheten används av program som är associerade med behörigheten. Du kan till exempel skapa en formel som ändringar som har behörighet till en mapp (via en säkerhetsgrupp är associerad med den mappen access control list) baserat på om en användare som tillhör en organisations enhet (organisationsenhet) som innehåller ständig eller minimera anställda. Om användaren flyttas från en organisationsenhet till en annan, används automatiskt behörigheten nya och gamla behörigheten är inaktiverad. 

Formeln CAP kan fråga värdena för attribut som har tillämpats på program, behörigheter, organisationsenheter och användare.

#### <a name="attribute-based-authorization"></a>Attributbaserad auktorisering

Ett sätt att kontrollera om en roll som är kopplad till en organisationsenhet (organisationsenhet) har aktiverats för en viss användare i organisationsenheten är att använda attributbaserad auktorisering (ABA). Med hjälp av ABA kan aktivera du automatiskt en roll endast när vissa regler baserat på en användares attribut är uppfyllda. Du kan till exempel länka en roll till en organisationsenhet som blir aktiv för en användare bara om användarens befattning matchar befattning i ABA-regeln. Detta eliminerar behovet av att manuellt aktivera en föreslagna roll för en användare. I stället kan en roll aktiveras för alla användare i en organisationsenhet som har ett attributvärde som uppfyller rollens ABA regeln. Regler kan kombineras så att rollen aktiveras endast när en användarattribut uppfyller alla ABA-regler som angetts för rollen.

Det är viktigt att Observera att resultaten av ABA regeln tester begränsas av Kardinalitetsinställningar. Om kardinalitetsinställning i en regel som anger att fler än två användare kan tilldelas en roll, och om en regel för ABA annars vill aktivera en roll för fyra användare, till exempel aktiveras rollen endast för de två första användare som skickar ABA-test.

#### <a name="flexible-attribute-types"></a>Flexibla attributtyper

Systemets av attribut i BHOLD är mycket utökningsbart. Du kan definiera nya attributtyper för sådana objekt som användare, organisationens enheter (organisationsenheter) och roller. Attribut kan definieras med värden som är heltal, booleskt värde (Ja/Nej), alfanumeriskt, datum, tid och e-postadresser. Attribut kan anges som enda värden eller en lista med värden.

## <a name="attestation"></a>Hälsoattestering

BHOLD-programsvit innehåller verktyg som du kan använda för att kontrollera att enskilda användare har rätt behörighet att utföra sina uppgifter. Administratören kan använda portalen som tillhandahålls av modulen BHOLD attestering för att utforma en hantera attestering processen.

Attesteringen processen med kampanjer i vilken kampanj förvaltare har möjlighet och innebär att kontrollera att de användare som är ansvarig har lämplig åtkomst till BHOLD-hanterade program och behörigheter i dessa program. En kampanj ägare är utsedda att övervaka kampanjen och se till att kampanjen utförs korrekt. Du kan skapa en kampanj för att förekomma en gång eller återkommande.

Steward för en kampanj kommer normalt att en chef som utfärda användare som tillhör en eller flera organisationsenheter som hanteraren ansvarar rättigheter. Förvaltare kan väljas automatiskt för användare som godkänd av i en kampanj baserat på användarattribut eller förvaltare för en kampanj kan definieras genom att lista dem i en fil som kopplar varje användare som godkänd av i kampanjen till en steward.

När en kampanj börjar BHOLD skickar ett e-postmeddelande till kampanj förvaltare och ägare och skickar sedan påminnelser med jämna mellanrum för att underhålla förlopp i kampanjen. Förvaltare dirigeras till en kampanj portal där de kan se en lista över användare som är ansvarig och de roller som är kopplade till dessa användare. Förvaltare kan sedan kontrollera om de är ansvariga för var och en av användarna som visas och godkänna eller neka behörighet för var och en av användarna som visas.

Kampanj ägare även använda portalen för att övervaka förloppet för kampanjen och kampanjaktiviteter loggas så kampanj ägare kan analysera de åtgärder som vidtagits under kampanjen.

## <a name="analytics"></a>Analytics

En av de viktiga överväganden när implementera ett omfattande rättigheter-baserad åtkomst (RBAC) system är balansen mellan underhålla strikt åtkomstkontroll och undvika onödiga (eller kan försämras, oväntat) att komma åt. Prestanda för den här balansen ofta resulterar i en åtkomstkontroll struktur som är så komplex att oväntade samverkan mellan principer är nästan oundvikligt.

Därför är det viktigt att kunna analysera effekterna av principer för åtkomstkontroll innan de börjar faktiskt på plats. Modulen analyser av BHOLD-programsvit får du möjlighet att utföra den här analysen genom att låta dig att utveckla regler som representerar dina principer och visar användarna vars behörigheter överensstämmer eller står i konflikt med regeln. Baserat på den här analysen kan du justera principen eller ändra roller och behörigheter för att undvika konflikter med principen.

BHOLD Analytics-portalen ger dig möjlighet att skapa regeluppsättningar som består av en eller flera regler som du skapar för att testa en viss princip eller en grupp av principer. En regel består av följande viktiga delar:

- En rubrik och en beskrivning som gör att du kan identifiera och beskriva regeln
- En status som anger om regeln är klar för granskning, som ska granskas eller godkännas
- Ett element som har angetts (till exempel användare eller behörigheter) som regeln är utformad för att testa
- Valfria delmängd filter som är uttryck som du kan använda för att välja en lämplig undergrupp till elementet undersökas
- En eller flera regeln filter som är uttryck som representerar den princip som ska testas.

En regel kan testa något av följande element:

- Användare
- Organisationsenheter
- Roller
- Behörigheter
- Program
- Konton

Följande diagram illustrerar en enkel regel som består av två delmängd regler och två filterregler:

![](media/bhold-concepts-guide/rules.png)

Observera skillnaden i effekt inte en delmängd filter och inte ett filter för regeln: misslyckas delmängd filter tar bort ett elementobjekt från testning av efterföljande filter medan misslyckas filter regeln gör objektet som ska rapporteras som inkompatibla. Endast de objekt som klarar alla delmängd filter och regeln filtren redovisas som överensstämmande.

Varje filter som består av en typ, en operator (som är typen beroende), en nyckel (ett element) och ett värde som nyckeln har testats av operatorn. Följande filter skulle till exempel testa om antalet användare i en delmängd av element är större än 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Typ:**   | Antal   |
| **Nyckel:**  | Användare  |
| **Operatorn**  | >  |
| **Värde:** | 10 |

Regler filter kan tre typer av och använda operatorer som är specifika för typ, som anges:

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
  - **Kan inte ha någon och kan inte ha alla**
  - **Kan bara har någon och kan bara ha alla**
  - **Exklusivt har någon och uteslutande har alla**

> [!Note]
> Restriktiva filter kan använda de angivna operatorerna för att testa en nyckel mot en uppsättning med flera värden.

Om du vill testa implementering av en uppdelning av uppgifter (Matjord) princip som anger att inga användare som har behörighet för begäran betalning är också att godkänna betalning behörighet kan du skapa en regel som liknar följande:

|   |  |
|---|--|
|Namn:| Betalningen Matjord Test|
|Element:| Användare|
|Vissa filter:| Med vilken behörighet begära betalning|
|Filter för regeln: | Får inte ha några behörigheter Godkänn betalning|

När du kör den här regeln visar modulen BHOLD Analytics antalet användare i den valda delmängden (antal användare med behörighet att begära betalning), antalet användare som är kompatibla med regeln och antalet användare som inte är kompatibla med regeln. Du kan sedan visa inkompatibla användarna så att du kan korrigera problemet.

Du kan också spara rapporten som en fil med kommaavgränsade värden (CSV) eller XML-filen så att du kan analysera resultaten senare förutom att visa resultaten. Du kan också anpassa resulterande rapporten för att visa ytterligare information som kan hjälpa dig att bättre förstå effekten. Till exempel om du vill testa användare, kan du visa (eller rapporten) konton med kompatibla eller inkompatibla användare så att du kan se vilka program som ingår.

Eftersom en regel kan innehålla flera filter kan du ansluta filter för att testa om det finns en viss kombination av villkor. Resultatet är produkten för ett booleskt och test av alla filter som standard, men du kan ange att en eller test av en filterkombination ska utföras.

Till exempel om din företagsprincip kräver chefer måste ha behörigheten Ändra betalning eller behörigheten godkänner betalning, kan du testa följer principen genom att skapa en regel som liknar följande:


|  |  |
|--|--|
|Namn: | Ändra betalning Matjord Test|
|Element: | Användare |
|Vissa filter: | Har någon roll Manager|
| Regel filter: |Måste ha någon behörighet att ändra betalning </br> Måste ha någon behörighet Godkänn betalning|

Som standard rapporteras alla användare som är en chef som har både ändra betalningen och behörighet att begära betalning som kompatibel. Principen kräver dock att en hanterare har antingen behörighet inte nödvändigtvis båda. Om du vill testa verkliga efterlevnad med principer måste du använda eller booleska operatorn med regeln för att avgöra om det finns några chefer som inte har antingen godkänna betalning behörighet eller ändra betalning behörighet.

Till skillnad från andra operatorer i **uteslutande har några** och **uteslutande har alla** operatörer visar kompatibiliteten för objekt som annars skulle undantas av ett filter som en del. Till exempel att testa en princip för att alla chefer (och endast chefer) har behörigheten godkänner granskningar kan du skapa en regel på följande sätt:

|  |  |
|--|--|
|Namn: | Granska godkännande Test|
|Element: | Användare|
| Vissa filter: | Har någon roll Manager
|Filter för regeln: | Exklusivt har alla behörigheten godkänner granskningar|

Den här regeln ska rapportera som kompatibla användare som är ansvariga för och har behörigheten godkänner omdömen och användare som inte är chefer och som inte har behörigheten godkänner granskningar. Däremot rapporteras chefer som inte har denna behörighet och användare som inte är chefer men har den behörigheten som inte är kompatibla.

Som nämndes tidigare, kan du kombinera regler i en regeluppsättning, vilket gör det enklare att kategorisera och hantera regler för att uppfylla dina affärsbehov.

Du kan också definiera en uppsättning globala filter som, när aktiverad gäller för alla regler som ska testas. Om du ofta behöver undanta en delmängd av poster när du testar regler i olika regeluppsättningar kan du ange globala filter som du kan aktivera eller inaktivera efter behov.

## <a name="reporting"></a>Rapportering

BHOLD-rapportmodulen ger dig möjlighet att visa information i rollen modellen via en mängd olika rapporter. BHOLD-rapportmodulen ger en omfattande uppsättning inbyggda rapporter, plus den innehåller en guide som du kan använda för att skapa anpassade rapporter för både grundläggande och avancerade. När du kör en rapport måste du omedelbart visa resultaten eller spara resultatet i en Microsoft Excel (.xlsx)-fil. Om du vill visa den här filen med hjälp av Microsoft Excel 2000, Microsoft Excel 2002 eller Microsoft Excel 2003 kan du hämta och installera Microsoft Office Compatibility Pack för Word, Excel och PowerPoint-filformat.


Du kan använda BHOLD rapportmodulen huvudsakligen för att producera rapporter som baseras på aktuella rollinformation. Genererade rapporter för granskning ändringar identitetsinformation har Forefront Identity Manager 2010 R2 en rapporteringsfunktion för FIM-tjänsten som är implementerad i System Center Service Manager-datalagret. Mer information om FIM rapportering finns i Forefront Identity Manager 2010 och Forefront Identity Manager 2010 R2-dokumentationen i det tekniska biblioteket för Forefront Identity Management.

Följande: kategorier som omfattas av inbyggda rapporter

- Administration
- Hälsoattestering
- Kontroller
- Aktiv åtkomstkontroll
- Loggning
- Modell
- Statistik
- Arbetsflöde

Du kan skapa rapporter och lägga till dem i dessa kategorier, eller så kan du definiera egna kategorier där du kan placera anpassade och inbyggda rapporter.

När du skapar en rapport vägleder guiden dig genom att ange följande parametrar:

- Identifierande information, inklusive namn, beskrivning, kategori, användning och målgrupp
- Fält som ska visas i rapporten
- Frågor som anger vilka objekt som ska analyseras
- Ordning rader som ska sorteras
- Fält som används för att dela upp rapporten i avsnitt
- Filter för att begränsa de element som returneras i rapporten

Du kan förhandsgranska rapporten som du gjort hittills och spara den om du inte behöver ange ytterligare parametrar i varje steg i guiden. Du kan också flytta tillbaka till föregående steg att ändra parametrar som du angav tidigare i guiden.

## <a name="access-management-connector"></a>Access Management-anslutningstjänsten

Modulen BHOLD Suite Access Management-anslutningstjänsten stöder både inledande och pågående synkronisering av data till BHOLD. Access Management-anslutningstjänsten fungerar med FIM-synkroniseringstjänsten för att flytta data mellan BHOLD Core-databasen, MIM metaversum och målprogram och identitetslagringar.

Tidigare versioner av BHOLD krävs flera MAs att styra dataflödet mellan MIM och mellanliggande BHOLD-databastabeller. BHOLD-programsvit SP1 kan du definiera hanteringsagenter (MAs) i MIM som tillhandahåller överföra data direkt mellan BHOLD och MIM i Access Management-anslutningstjänsten.

## <a name="mim-integration"></a>MIM-integrering

Ett viktigt och kraftfull funktion i Forefront Identity Manager 2010 och Forefront Identity Manager 2010 R2 är självbetjäningsportalen som kan användarna visa och hantera information om deras identitet och medlemskap. MIM-Integration utökar MIM-portalen med självbetjäning rollhantering. Till exempel en användare med hjälp av BHOLD-funktioner i MIM-portalen, kan begära rolltilldelning och kan visa aktiva roller och väntande begäranden. Ytterligare funktioner som kan beviljas delegerade administratörer, till exempel möjligheten att begäran rolltilldelningar för andra användare.

Det är viktigt att notera att BHOLD-tillägg till MIM-portalen stöder självbetjäning roll och Arbetsflödeshantering och rapportering. Andra funktioner för administration av BHOLD, samt attestering, som tillhandahålls av webbportaler av BHOLD-moduler som hanteras separat från MIM-portalen.

## <a name="next-steps"></a>Nästa steg

- [BHOLD-installationsguiden](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
