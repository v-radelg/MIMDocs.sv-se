---
title: Koncept guide för Microsoft BHOLD Suite | Microsoft Docs
description: Kom igång med MIM 2016-komponenterna genom att installera och konfigurera Synkroniseringstjänsten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 3749b74fd867601ee05f8e45d273ad2de9144b5b
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701426"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Koncept guide för Microsoft BHOLD Suite

Microsoft Identity Manager 2016 (MIM) gör det möjligt för organisationer att hantera hela livs cykeln för användar identiteter och deras associerade autentiseringsuppgifter. Den kan konfigureras för att synkronisera identiteter, centralt hantera certifikat och lösen ord och etablera användare i heterogena system. Med MIM kan IT-organisationer definiera och automatisera processer som används för att hantera identiteter från skapandet till pensionering.

Microsoft BHOLD Suite utökar dessa funktioner i MIM genom att lägga till rollbaserad åtkomst kontroll. BHOLD gör det möjligt för organisationer att definiera användar roller och kontrol lera åtkomsten till känsliga data och program. Åtkomsten baseras på vad som är lämpligt för dessa roller. BHOLD Suite innehåller tjänster och verktyg som fören klar modelleringen av roll relationerna i organisationen. BHOLD mappar dessa roller till rättigheter och kontrollerar att roll definitionerna och de associerade rättigheterna är korrekt kopplade till användarna. Dessa funktioner är helt integrerade med MIM och ger en sömlös upplevelse för slutanvändare och IT-personal.

Den här guiden hjälper dig att förstå hur BHOLD Suite fungerar med MIM och omfattar följande ämnen:

- Rollbaserad åtkomstkontroll
- Attestering
- Analytics
- Rapportering
- Åtkomst hanterings anslutning
- MIM-integrering

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Den vanligaste metoden för att kontrol lera användar åtkomst till data och program är via DAC (Discretionary Access Control). I de vanligaste implementeringarna har varje betydelsefullt objekt en identifierad ägare. Ägaren kan bevilja eller neka åtkomst till objektet till andra baserat på individuell identitet eller grupp medlemskap. I praktiken resulterar DAC vanligt vis i en mängd olika av säkerhets grupper, en del som återspeglar organisations strukturen, andra som representerar funktionella grupperingar (till exempel jobb typer eller projekt tilldelningar) och andra som består av Makeshift-samlingar av användare och enheter som är länkade för mer temporära syfte. När organisationer växer blir medlemskap i dessa grupper allt svårare att hantera. Om en anställd exempelvis överförs från ett projekt till ett annat, måste de grupper som används för att styra åtkomsten till projekt till gångarna uppdateras manuellt. I sådana fall är det inte ovanligt att misstag uppstår, misstag som kan störa projekt säkerheten eller produktiviteten.

MIM innehåller funktioner som hjälper dig att minimera det här problemet genom att tillhandahålla automatisk kontroll över medlemskap i grupper och distributions listor. Detta tar dock inte itu med den inneboende komplexiteten för de spridnings grupper som inte nödvändigt vis är relaterade till varandra på ett strukturerat sätt.

Ett sätt att avsevärt minska den här spridningen är genom att distribuera rollbaserad åtkomst kontroll (RBAC). RBAC förskjuter inte DAC.  RBAC bygger på DAC genom att tillhandahålla ett ramverk för att klassificera användare och IT-resurser. På så sätt kan du göra explicita relationer och de behörigheter som är lämpliga enligt denna klassificering. Genom att till exempel tilldela ett användarattribut som anger användarens jobb titel och projekt tilldelningar, kan användaren beviljas åtkomst till verktyg som behövs för användarens jobb och data som användaren behöver för att bidra till ett visst projekt. När användaren förutsätter ett annat jobb och olika projekt tilldelningar, ändrar attributen som anger användarens befattning och projekt automatiskt åtkomst till resurserna som krävs för den tidigare positionen.

Eftersom roller kan ingå i roller på hierarkiskt sätt, (till exempel kan försäljnings chefens och försäljnings representantens roller ingå i den mer generella rollen försäljning), är det enkelt att tilldela lämpliga rättigheter till vissa roller och ännu fortfarande tillhandahålla lämpliga rättigheter till alla som delar rollen mer inkluderar också. Till exempel kan alla medicinska personal få till gång till rätten att se en patienter, men endast läkare (en under roll i medicinska rollen) kan få behörighet att ange förskrifter för patienten. På samma sätt kan användare som tillhör den här rollen nekas åtkomst till patient poster, förutom fakturerings administratörer (en under roll till den som är den som är den som är under roll), som kan beviljas åtkomst till de delar av en patienter som krävs för att fakturera patienten för tjänster tillhandahålls av sjukhus.

En ytterligare fördel med RBAC är möjligheten att definiera och upprätthålla separering av uppgifter (SoD). På så sätt kan en organisation definiera kombinationer av roller som beviljar behörigheter som inte ska innehas av samma användare, så att en viss användare inte kan tilldelas roller som gör det möjligt för användaren att initiera en betalning och för att auktorisera en betalning, till exempel. RBAC ger möjlighet att genomdriva en sådan princip automatiskt i stället för att behöva utvärdera den effektiva implementeringen av principen per användare.

### <a name="bhold-role-model-objects"></a>BHOLD roll modell objekt

Med BHOLD Suite kan du ange och ordna roller i din organisation, mappa användare till roller och mappa lämpliga behörigheter till roller. Den här strukturen kallas en roll modell och den innehåller och ansluter fem typer av objekt: 

- Organisatoriska enheter
- Användare
- Roller
- Behörigheter
- Program

#### <a name="organizational-units"></a>Organisatoriska enheter

Organisations enheter (OrgUnits) är det huvudsakliga sättet som användarna organiseras genom i BHOLD-roll modellen. Varje användare måste tillhöra minst en OrgUnit. (I själva verket tas användarens data post bort från BHOLD-databasen när en användare tas bort från den senaste organisationsenheten i BHOLD.)

> [!Important]
> Organisationsenheter i BHOLD-roll modellen bör inte förväxlas med organisationsenheter i Active Directory Domain Services (AD DS). Den organisatoriska enhets strukturen i BHOLD baseras vanligt vis på organisation och principer för din verksamhet, inte kraven i din nätverks infrastruktur.

Även om det inte är obligatoriskt, är det i de flesta fall organisationsenheter som är strukturerade i BHOLD för att representera den faktiska organisationens hierarkiska struktur, som liknar den som visas nedan:

![](media/bhold-concepts-guide/org-chart.png)

I det här exemplet skulle roll modellen organizationalganizatinal enhet för företaget som helhet (representeras av VD, eftersom VD inte är en del av en mororganizationalganizatinal-enhet) eller den BHOLD rot organisationsenheten (som alltid finns) kan användas för detta ändamål. OrgUnits som representerar de företags indelningar som leds av vice president placeras i företagets organisationsenhet. Sedan kommer organisationsenheter som motsvarar marknadsförings-och försäljnings ansvariga att läggas till i organisationsenheterna för marknadsföring och försäljning, och organisationsenheter som representerar de regionala försäljnings cheferna placeras i organisationsenheten för regions försäljnings chef, östra. Sälj ansvariga, som inte hanterar andra användare, kommer att bli medlemmar i organisationsenheten för den östra regionens försäljnings chef. Observera att användare kan vara medlemmar i en organisationsenhet på valfri nivå. En administrativ assistent, som inte är en chef och rapporter direkt till en vice VD, är till exempel medlem i vice presidentens organisationsenhet.

Förutom att representera organisations strukturen kan organisationsenheter också användas för att gruppera användare och andra organisationsenheter utifrån funktionella villkor, till exempel för projekt eller specialisering. Följande diagram visar hur organisationsenheter ska användas för att gruppera Sälj Associates enligt kund typ:

![](media/bhold-concepts-guide/org-chart-02.png)

I det här exemplet skulle varje Sälj partner tillhöra två organisationsenheter: en som representerar kopplingens plats i organisationens hanterings struktur och en som representerar kundens kund bas (detalj handel eller företag). Varje organisationsenhet kan tilldelas olika roller, som i sin tur kan tilldelas olika behörigheter för att komma åt organisationens IT-resurser. Dessutom kan roller ärvas från överordnade organisationsenheter, vilket fören klar processen för att sprida roller till användare. Å andra sidan kan vissa roller förhindras från att ärvas, vilket säkerställer att en speciell roll endast är kopplad till rätt organisationsenheter.

OrgUnits kan skapas i BHOLD Suite med hjälp av BHOLD Core-webbportalen eller med BHOLD Model Generator.

#### <a name="users"></a>Användare

Som anges ovan måste varje användare tillhöra minst en organisationsenhet (OrgUnit). Eftersom organisationsenheter är huvudmekanismen för att associera en användare med roller, i de flesta organisationer som en specifik användare hör till flera OrgUnits för att göra det lättare att associera roller med den användaren. I vissa fall kan det dock vara nödvändigt att associera en roll med en användare som skiljer sig från alla OrgUnits som användaren tillhör. Det innebär att en användare kan tilldelas direkt till en roll och hämta roller från OrgUnits som användaren tillhör.

När en användare inte är aktiv i organisationen (till exempel för medicinsk ledighet, till exempel) kan användaren stängas av, vilket återkallar alla användarens behörigheter utan att ta bort användaren från roll modellen. När du återvänder till tullen kan användaren återaktiveras, vilket återställer alla behörigheter som beviljats av användarens roller.

Objekt för användare kan skapas individuellt i BHOLD via BHOLD Core-webbportalen, eller så kan de importeras i bulk med hjälp av BHOLD modell generator eller med hjälp av åtkomst hanterings anslutningen med FIM-synkroniseringstjänsten för att importera användar information från sådana källor som Active Directory Domain Services eller personal resurser.

Användare kan skapas direkt i BHOLD utan att använda FIM-synkroniseringstjänsten. Detta kan vara användbart när du modellerar roller i en för produktions-eller test miljö. Du kan också tillåta externa användare (t. ex. anställda på en underleverantör) att tilldelas roller och därmed få till gång till IT-resurser utan att läggas till i medarbetar databasen. Dessa användare kommer dock inte att kunna använda självbetjänings funktionerna i BHOLD.

#### <a name="roles"></a>Roller

Som tidigare nämnts, under modellen rollbaserad åtkomst kontroll (RBAC), är behörigheter kopplade till roller i stället för enskilda användare. Detta gör det möjligt att ge varje användare de behörigheter som krävs för att utföra användarens uppgifter genom att ändra användarens roller i stället för att separat bevilja eller neka användar behörighet. Därför kräver tilldelningen av behörigheter inte längre IT-avdelningens medverkan, utan kan i stället utföras som en del av hanteringen av verksamheten. En roll kan aggregera behörigheter för åtkomst till olika system, antingen direkt eller via användning av under roller, vilket ytterligare minskar behovet av IT-inblandning i hanteringen av användar behörigheter.

Det är viktigt att notera att roller är en funktion i själva RBAC-modellen. rollerna är vanligt vis inte etablerade för mål program. Detta gör att RBAC kan användas tillsammans med befintliga program som inte är utformade för att använda roller eller ändra roll definitionerna uppfyller behoven för att ändra affärs modeller utan att behöva ändra själva programmen. Om ett mål program är utformat för att använda roller, kan du associera roller i BHOLD-roll modellen med motsvarande program roller genom att behandla de programspecifika rollerna som behörigheter.

I BHOLD kan du tilldela en roll till en användare huvudsakligen genom två mekanismer:

- Genom att tilldela en roll till en organisationsenhet (organisatorisk enhet) som användaren är medlem i
- Genom att tilldela en roll direkt till en användare

En roll som är tilldelad en överordnad organisationsenhet kan alternativt ärvas av dess medlems organisatoriska enheter. När en roll tilldelas eller ärvs av en organisationsenhet, kan den utses som en effektiv eller föreslagen roll. Om det är en effektiv roll tilldelas rollen alla användare i organisationsenheten. Om det är en föreslagen roll måste den aktive ras för att varje användare eller organisations enhet ska bli giltig för den användaren eller organisationsenhetens medlemmar. Detta gör det möjligt att tilldela användare en delmängd av rollerna som är kopplade till en organisationsenhet, i stället för att automatiskt tilldela alla medlemmars roller i organisationen. Dessutom kan roller tilldelas start-och slutdatum och gränser kan placeras på procent andelen användare i en organisationsenhet för vilken en roll kan vara effektiv.

Följande diagram illustrerar hur en enskild användare kan tilldelas en roll i BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

I det här diagrammet tilldelas roll A en organisationsenhet som en ärftlig roll, och den ärvs av dess medlemmars organisationsenheter och alla användare inom dessa organisationsenheter. Roll B är tilldelad som en föreslagen roll för en organisationsenhet. Den måste aktive ras innan en användare i organisationsenheten kan auktoriseras med rollens behörigheter. Roll C är en effektiv roll, så dess behörigheter gäller omedelbart för alla användare i organisationsenheten. Roll D är direkt länkad till användaren och dess behörigheter gäller omedelbart för den användaren.

Dessutom kan en roll aktive ras för en användare baserat på en användares attribut. Mer information finns i attribut-baserad auktorisering.

#### <a name="permissions"></a>Behörigheter

En behörighet i BHOLD motsvarar en auktorisering som importer ATS från ett mål program. Det vill säga när BHOLD har kon figurer ATS för att fungera med ett program, får den en lista över auktoriseringar som BHOLD kan länka till roller. Exempel: när Active Directory Domain Services (AD DS) läggs till BHOLD som ett program får den en lista över säkerhets grupper som BHOLD-behörigheter kan länkas till roller i BHOLD.

Behörigheter är bara för program. BHOLD tillhandahåller en enda, enhetlig vy över behörigheter så att behörigheter kan associeras med roller utan att roll hanterare måste förstå implementerings information om behörigheterna. I praktiken kan olika system framtvinga en behörighet på ett annat sätt. Programspecifik anslutning från FIM-synkroniseringstjänsten till programmet avgör hur behörighets ändringar för en användare tillhandahålls för programmet. 

#### <a name="applications"></a>Program

BHOLD implementerar en metod för att tillämpa rollbaserad åtkomst kontroll (RBAC) till externa program. Det vill säga när BHOLD är etablerad med användare och behörigheter från ett program, kan BHOLD associera dessa behörigheter med användare genom att tilldela roller till användarna och sedan länka behörigheterna till rollerna. Programmets bakgrunds process kan sedan mappa rätt behörigheter till användare baserat på roll-/behörighets mappningen i BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Utveckla BHOLD-paketets roll modell

För att hjälpa dig att utveckla din roll modell tillhandahåller BHOLD Suite modell generator, ett verktyg som är både enkelt att använda och omfattande.

Innan du använder modell Generator måste du skapa en serie filer som definierar de objekt som modell generatorn använder för att skapa roll modellen. Information om hur du skapar de här filerna finns i teknisk referens för Microsoft BHOLD Suite.

Det första steget i att använda BHOLD modell Generator är att importera de här filerna för att läsa in de grundläggande Bygg stenarna i modell generatorn. När filerna har lästs in kan du ange kriterier som modell generatorn använder för att skapa flera klasser av roller:

- Medlemskaps roller som tilldelas till en användare baserat på de OrgUnits (organisationsenheter) som användaren tillhör
- Attributrelationer som tilldelas en användare baserat på användarens attribut i BHOLD-databasen
- Föreslagna roller som är länkade till en organisationsenhet men måste aktive ras för vissa användare
- Ägarskaps roller som ger en användar kontroll över organisationsenheter och roller för vilka en ägare inte anges i de importerade filerna

> [!Important]
> När du laddar upp filer markerar du kryss rutan **Behåll befintlig modell** endast i test miljöer. I produktions miljöer måste du använda modell generator för att skapa den inledande roll modellen. Du kan inte använda den för att ändra en befintlig roll modell i BHOLD-databasen.

När modell generatorn skapar dessa roller i roll modellen kan du exportera roll modellen till BHOLD-databasen i form av en XML-fil.

### <a name="advanced-bhold-features"></a>Avancerade BHOLD-funktioner

I föregående avsnitt beskrivs de grundläggande funktionerna i rollbaserad åtkomst kontroll (RBAC) i BHOLD. Det här avsnittet beskriver ytterligare funktioner i BHOLD som ger förbättrad säkerhet och flexibilitet för organisationens implementering av RBAC. Det här avsnittet innehåller översikter över följande BHOLD-funktioner:

- Kardinalitet
- Separering av uppgifter
- Kontext-anpassningsbara behörigheter
- Attribut-baserad auktorisering
- Flexibla attributtyper


#### <a name="cardinality"></a>Kardinalitet

*Kardinalitet* syftar på implementeringen av affärs regler som är utformade för att begränsa antalet gånger som två entiteter kan vara relaterade till varandra. Om du använder BHOLD kan du upprätta kardinalitet för roller, behörigheter och användare.

Du kan konfigurera en roll för att begränsa följande:

- Det maximala antalet användare för vilka en föreslagen roll kan aktive ras
- Det maximala antalet under roller som kan länkas till rollen
- Det maximala antalet behörigheter som kan länkas till rollen

Du kan konfigurera en behörighet för att begränsa följande:

- Det maximala antalet roller som kan länkas till behörigheten
- Det maximala antalet användare som kan beviljas behörigheten

Du kan konfigurera en användare så att den begränsar följande:

- Det maximala antalet roller som kan länkas till användaren
- Det maximala antalet behörigheter som kan tilldelas användaren via roll tilldelningar

#### <a name="separation-of-duties"></a>Separering av uppgifter

Separering av uppgifter (SoD) är en affärs princip som kan förhindra att personer får möjlighet att utföra åtgärder som inte ska vara tillgängliga för en enskild person. En medarbetare ska till exempel inte kunna begära en betalning och godkänna betalningen. Principen för SoD gör det möjligt för organisationer att implementera ett system med kontroller och balanser för att minimera deras exponering för risk från medarbetares fel eller fel uppförande.

BHOLD implementerar SoD genom att låta dig definiera inkompatibla behörigheter. När dessa behörigheter har definierats tvingar BHOLD SoD genom att förhindra skapandet av roller som är länkade till inkompatibla behörigheter, oavsett om de är länkade direkt eller genom arv, och hindra användare från att tilldelas flera roller som, när kombinerat beviljar inkompatibla behörigheter, återigen genom direkt tilldelning eller arv. Du kan också åsidosätta konflikter.

#### <a name="context-adaptable-permissions"></a>Kontext-anpassningsbara behörigheter

Genom att skapa behörigheter som kan ändras automatiskt baserat på ett objekt-attribut kan du minska det totala antalet behörigheter som du måste hantera. Med Sammanhangs beroende behörighet kan du definiera en formel som ett behörighets attribut som ändrar hur behörigheten tillämpas av programmet som är associerat med behörigheten. Du kan till exempel skapa en formel som ändrar åtkomst behörigheten till en filmapp (via en säkerhets grupp som är associerad med mappens åtkomst kontrol lista) baserat på om en användare tillhör en organisationsenhet (Organisationsenhet) som innehåller heltids-eller kontrakts anställda. Om användaren flyttas från en organisationsenhet till en annan, tillämpas den nya behörigheten automatiskt och den gamla behörigheten inaktive ras. 

CAP-formeln kan fråga värdena för attribut som har tillämpats på program, behörigheter, organisationsenheter och användare.

#### <a name="attribute-based-authorization"></a>Attribut-baserad auktorisering

Ett sätt att kontrol lera om en roll som är länkad till en organisationsenhet (Organisationsenhet) har Aktiver ATS för en viss användare i organisationsenheten är att använda ABA (Attribute-based Authorization). Genom att använda ABA kan du automatiskt aktivera en roll när vissa regler som baseras på en användares attribut är uppfyllda. Du kan till exempel länka en roll till en organisationsenhet som bara är aktiv för en användare om användarens befattning matchar jobb titeln i ABA-regeln. Detta eliminerar behovet av att manuellt aktivera en föreslagen roll för en användare. I stället kan en roll aktive ras för alla användare i en organisationsenhet som har ett attributvärde som uppfyller rollens ABA-regel. Regler kan kombineras, så att en roll endast aktive ras när en användares attribut uppfyller alla ABA-regler som har angetts för rollen.

Det är viktigt att Observera att resultaten av ABA-regler är begränsade av inställningarna för kardinalitet. Till exempel, om inställningen kardinalitet för en regel anger att det inte finns fler än två användare som kan tilldelas en roll, och om en ABA-regel annars aktiverar en roll för fyra användare, aktive ras rollen bara för de första två användare som klarar ABA-testet.

#### <a name="flexible-attribute-types"></a>Flexibla attributtyper

Systemet med attribut i BHOLD är mycket utöknings Bart. Du kan definiera nya attributtyper för sådana objekt som användare, organisationsenheter (organisationsenheter) och roller. Attribut kan definieras för att ha värden som är heltal, booleska (Ja/Nej), alfanumeriska data, datum, tid och e-postadresser. Attribut kan anges som enstaka värden eller en värde lista.

## <a name="attestation"></a>Attestering

BHOLD-sviten innehåller verktyg som du kan använda för att kontrol lera att enskilda användare har fått rätt behörighet för att utföra sina affärs uppgifter. Administratören kan använda portalen som tillhandahålls av modulen BHOLD attestering för att utforma en hantera attesterings processen.

Attesteringen utförs med hjälp av kampanjer där kampanj administratörer får möjlighet att kontrol lera att de användare för vilka de är ansvariga har lämplig åtkomst till BHOLD-hanterade program och rätt behörigheter inom dessa program. En kampanj ägare utses för att övervaka kampanjen och se till att kampanjen genomförs korrekt. En kampanj kan skapas för att ske en gång eller regelbundet.

Normalt är vården för en kampanj en ansvarig som bestyrkar åtkomst rättigheterna för användare som tillhör en eller flera organisationsenheter som chefen ansvarar för. Administratörer kan väljas automatiskt för användare som besvaras i en kampanj baserat på användarattribut, eller så kan en kampanjs administratörer definieras genom att lista dem i en fil som mappar varje användare som betecknas i kampanjen till en vård.

När en kampanj börjar skickar BHOLD ett e-postmeddelande till kampanjens beställningar och ägare och skickar sedan regelbundna påminnelser för att hjälpa dem att underhålla förloppet i kampanjen. Vården dirigeras till en kampanj portal där de kan se en lista över de användare som de är ansvariga för och de roller som är tilldelade till dessa användare. Behållarna kan sedan bekräfta om de är ansvariga för var och en av de listade användarna och godkänner eller nekar åtkomst behörigheterna för var och en av de listade användarna.

Kampanj ägare använder också portalen för att övervaka kampanjens förlopp och kampanj aktiviteter loggas så att kampanj ägare kan analysera de åtgärder som vidtagits under kampanjen.

## <a name="analytics"></a>Analytics

Ett av de viktigaste aspekterna vid implementeringen av en omfattande användarbaserad åtkomst kontroll (RBAC) är balansen mellan att upprätthålla strikt åtkomst kontroll och undvika onödiga (eller, sämre, oväntade) hinder för åtkomst. Arbetet med att underhålla detta saldo leder ofta till en åtkomst kontroll struktur som är så komplicerad att oväntade interaktioner mellan principer är nästan oundvikliga.

Av den anledningen är det viktigt att kunna analysera effekterna av åtkomst kontroll principer innan de faktiskt placeras på plats. Analytics-modulen i BHOLD Suite ger dig möjlighet att utföra den här analysen genom att låta dig utveckla regler som representerar dina principer och sedan Visa de användare vars behörigheter uppfyller eller står i konflikt med regeln. Baserat på den här analysen kan du justera principen eller ändra roller och behörigheter för att undvika konflikter med principen.

BHOLD Analytics-portalen ger dig möjlighet att skapa rulesets som består av en eller flera regler som du skapar för att testa en viss princip eller grupp med principer. En regel består av följande huvud delar:

- En rubrik och beskrivning som gör att du kan identifiera och beskriva regeln
- En status som anger om regeln är klar för granskning, granskas eller godkänns
- En element uppsättning (till exempel användare eller behörigheter) som regeln är utformad för att testas
- Valfria del filter som är uttryck som du kan använda för att välja en lämplig under grupp för det element som ska undersökas
- Ett eller flera regel filter som är uttryck som representerar den princip som testas.

En regel kan testa vilken som helst av följande element uppsättningar:

- Användare
- Organisatoriska enheter
- Roller
- Behörigheter
- Program
- Konton

Följande diagram illustrerar en enkel regel bestående av två delmängds regler och två filter regler:

![](media/bhold-concepts-guide/rules.png)

Observera skillnaden i effekterna av att ett del filter inte har filtrerats och att regel filtret inte fungerar: Om du inte skickar ett del mängds filter raderas ett element objekt från testningen av efterföljande filter, medan ett regel filter Miss låter att objektet rapporteras som inkompatibelt. Endast de objekt som uppfyller alla del mängds filter och alla regel filter rapporteras som kompatibla.

Varje filter består av en typ, en operator (som är av typen Dependent), en nyckel (ett av de element) och ett värde mot vilket nyckeln testas av operatorn. Följande filter kommer till exempel att testa om antalet användare i ett Elements delmängd överskrider 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Typ:**   | Antal   |
| **Knapp**  | Användare  |
| **Operator**  | >  |
| **Värde:** | 10 |

Regel filtren kan vara av tre typer och använder operatörer som är speciella för deras typ, enligt vad som anges:

- Attribut
  - < och >
  - = och! =
  - **Ingår**
  - **Innehåller inte**
- Antal
  - < och >
  - = och! =
- Restriktiva
  - **Måste ha alla och måste ha alla**
  - **Det får inte finnas några och kan inte ha alla**
  - **Kan bara ha alla**
  - **Enbart ha några och exklusivt har alla**

> [!Note]
> Begränsade filter kan använda de angivna operatorerna för att testa en nyckel mot en uppsättning med flera värden.

Om du till exempel vill testa implementeringen av en SoD-princip (ansvars fördelning) som säger att ingen användare som har behörigheten be om betalning också har behörigheten godkänna betalning, kan du skapa en regel som följande:

|   |  |
|---|--|
|Namn:| Betalnings SoD-test|
|Brevpost| Användare|
|Del mängds filter:| Med en begäran om behörighets betalning|
|Regel filter: | Det går inte att godkänna betalning för någon behörighet|

När du kör den här regeln visar modulen BHOLD-analys antalet användare i den valda del mängden (antalet användare med behörigheten begär ande betalning), antalet användare som följer regeln och antalet användare som inte följer regeln. Du kan sedan Visa de inkompatibla användarna så att du kan åtgärda inkonsekvensen.

Förutom att visa resultaten kan du också spara rapporten som en CSV-eller XML-fil så att du kan analysera resultaten senare. Du kan också anpassa den resulterande rapporten för att visa ytterligare information som kan hjälpa dig att bättre förstå påverkan. Om du till exempel testar användare kan du Visa (eller rapportera) konton för de inkompatibla eller inkompatibla användarna så att du kan se vilka program som ingår.

Eftersom en regel kan innehålla flera filter kan du ansluta filter för att testa om det finns en viss kombination av villkor. Som standard är resultatet produkten av ett och booleska test av alla filter, men du kan ange att en eller ett test av filter kombinationen ska utföras.

Om din affärs policy till exempel kräver att chefer har behörigheten Ändra betalning eller godkänn betalning, kan du testa efterlevnaden av principen genom att skapa en regel som följande:


|  |  |
|--|--|
|Namn: | Ändra betalnings SoD-test|
|Brevpost | Användare |
|Del mängds filter: | Med någon roll hanterare|
| Regel filter: |Måste ha behörighet att ändra betalning </br> Måste ha behörighet för att godkänna betalning|

Som standard rapporteras alla användare som är ansvariga för både ändra betalning och begär ande betalnings behörighet som kompatibla. Principen kräver dock att en chef har antingen behörighet, inte nödvändigt vis båda. Om du vill testa den faktiska efterlevnaden av principen måste du använda operatorn eller Boolean med regeln för att avgöra om det finns några hanterare som inte har behörigheten Ändra betalning eller godkänn betalnings behörighet.

Till skillnad från andra operatorer, **har det exklusivt** och **exklusivt sett alla** operatorer som anger kompatibilitet för objekt som annars skulle uteslutas av ett del mängds filter. Om du till exempel vill testa en princip som alla hanterare (och endast administratörer) har behörigheten Godkänn granskning kan du skapa en regel på följande sätt:

|  |  |
|--|--|
|Namn: | Granska godkännande test|
|Brevpost | Användare|
| Del mängds filter: | Med någon roll hanterare
|Regel filter: | Ha enbart behörighet att godkänna granskningar|

Den här regeln rapporterar som kompatibla användare som är chefer och har behörigheten Godkänn granskningar och användare som inte är administratörer och som inte har behörigheten Godkänn granskning. Administratörer som inte har den behörigheten och användare som inte är hanterare, men som har den behörigheten rapporteras som icke-kompatibla.

Som tidigare nämnts kan du kombinera regler till en ruleset, vilket gör det enklare för dig att kategorisera och hantera regler som uppfyller dina affärs behov.

Du kan också definiera en uppsättning globala filter som, när det är aktiverat, gäller för alla regler som testas. Om du ofta behöver undanta en viss delmängd poster när du testar regler i olika rulesets, kan du ange globala filter som du kan aktivera eller inaktivera vid behov.

## <a name="reporting"></a>Rapportering

BHOLD repor ting-modulen ger dig möjlighet att visa information i roll modellen via en rad olika rapporter. BHOLD repor ting-modulen innehåller en omfattande uppsättning inbyggda rapporter, och innehåller en guide som du kan använda för att skapa både grundläggande och avancerade anpassade rapporter. När du kör en rapport kan du direkt Visa resultaten eller Spara resultaten i en Microsoft Excel-fil (. xlsx). Om du vill visa den här filen med Microsoft Excel 2000, Microsoft Excel 2002 eller Microsoft Excel 2003 kan du hämta och installera Microsoft Office Compatibility Pack för Word-, Excel-och PowerPoint-filformat.


Du använder BHOLD repor ting module för att skapa rapporter som baseras på den aktuella roll informationen. För genererade rapporter om gransknings ändringar av identitets information har Forefront Identity Manager 2010 R2 en rapporterings funktion för FIM-tjänsten som implementeras i System Center Service Manager data lagret. Mer information om FIM-rapportering finns i Forefront Identity Manager 2010 och Forefront Identity Manager 2010 R2-dokumentationen i det tekniska biblioteket för Forefront Identity Management.

Kategorier som omfattas av de inbyggda rapporterna innehåller följande:

- Administration
- Attestering
- Kontroller
- Ankomstreg Access Control
- Loggning
- Modell
- Uppgifterna
- Arbetsflöde

Du kan skapa rapporter och lägga till dem i dessa kategorier, eller så kan du definiera egna kategorier där du kan placera anpassade och inbyggda rapporter.

När du skapar en rapport vägleder vi dig genom att tillhandahålla följande parametrar i guiden:

- Identifierings information, inklusive namn, beskrivning, kategori, användning och mål grupp
- Fält som ska visas i rapporten
- Frågor som anger vilka objekt som ska analyseras
- Ordningen som raderna ska sorteras i
- Fält som används för att dela upp rapporten i avsnitt
- Filter för att förfina de element som returneras i rapporten

I varje steg i guiden kan du förhandsgranska rapporten eftersom du har definierat den hittills och spara den om du inte behöver ange ytterligare parametrar. Du kan också gå tillbaka till föregående steg för att ändra parametrarna som du angav tidigare i guiden.

## <a name="access-management-connector"></a>Åtkomst hanterings anslutning

BHOLD Suite Access Management Connector-modulen stöder både inledande och pågående synkronisering av data till BHOLD. Anslutnings hanterings anslutningen fungerar med FIM-synkroniseringstjänsten för att flytta data mellan BHOLD Core-databasen, MIM-metaversum och mål program och identitets lager.

Tidigare versioner av BHOLD krävde flera MAs för att styra data flödet mellan MIM och mellanliggande BHOLD databas tabeller. I BHOLD Suite SP1 kan du med åtkomst hanterings anslutningen definiera hanterings agenter (MAs) i MIM som tillhandahåller data överföring direkt mellan BHOLD och MIM.

## <a name="mim-integration"></a>MIM-integrering

En viktig och kraftfull funktion i Forefront Identity Manager 2010 och Forefront Identity Manager 2010 R2 är självbetjänings portalen som gör att slutanvändare kan visa och hantera sin identitets-och medlemskaps information. MIM-integrering utökar MIM-portalen med självbetjänings roll hantering. Genom att använda BHOLD-funktionerna i MIM-portalen kan en användare exempelvis begära roll tilldelning och Visa aktiva roller och väntande begär Anden. Ytterligare funktioner kan beviljas delegerade administratörer, till exempel möjligheten att begära roll tilldelningar för andra användare.

Det är viktigt att Observera att BHOLD-tilläggen till MIM-portalen stöder självbetjänings roll och arbets flödes hantering och rapportering. Andra BHOLD administrations funktioner, samt attestering, tillhandahålls av webb portalerna i BHOLD-modulerna, som finns separat från MIM-portalen.

## <a name="next-steps"></a>Nästa steg

- [Installations guide för BHOLD](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
