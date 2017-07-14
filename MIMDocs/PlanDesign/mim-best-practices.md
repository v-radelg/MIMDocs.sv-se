---
title: "Rekommenderade metoder för Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.translationtype: MT
ms.sourcegitcommit: 3bb89e2c86724e6f6d32e4043fa37da74e2b7b24
ms.openlocfilehash: a0d00c7e5d99e43d3fb0b3011a3851f7194bfdf2
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---


# Rekommenderade metoder för Microsoft Identity Manager 2016
<a id="microsoft-identity-manager-2016-best-practices" class="xliff"></a>

Det här avsnittet beskriver rekommenderade metoder för att distribuera och använda Microsoft Identity Manager 2016 (MIM)

## Konfiguration av SQL
<a id="sql-setup" class="xliff"></a>
>[!NOTE]
Följande rekommendationer för att konfigurera en server som kör SQL förutsätter en SQL-instans som är dedikerad till FIM-tjänsten och en SQL-instans som är dedikerad till FIMSynchronizationService-databasen. Om du kör FIM-tjänsten i en konsoliderad miljö måste du göra de justeringar som krävs för din konfiguration.

Konfiguration av SQL-servern (Structured Query Language) är viktig för optimala systemprestanda. Optimala MIM-prestanda i storskaliga implementeringar beror på användning av rekommenderade metoder för en server som kör SQL. Mer information finns i följande avsnitt om rekommenderade metoder för SQL:

-   [10 bästa metoderna för lagring](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Optimera tempdb-prestanda](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [Artikel om SQL Server praxis](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Organisera om och bygga om index](http://go.microsoft.com/fwlink/?LinkID=188269)

### Ange storlek på data och loggfiler på förhand
<a id="presize-data-and-log-files" class="xliff"></a>

Förlita dig inte på automatisk tillväxt. Hantera i stället tillväxten av dessa filer manuellt. Du kan låta automatisk tillväxt vara på av säkerhetsskäl, men du bör hantera datafilernas tillväxt proaktivt. Exempelstorlekar för MIM-databasen finns i [Guide för kapacitetsplanering för FIM](http://go.microsoft.com/fwlink/?LinkID=185246).

### Ange storlek på data och loggfiler på förhand
<a id="to-presize-sql-data-and-log-files" class="xliff"></a>

1.  Starta SQL Server Management Studio.

2.  Navigera till databasen FIMService, högerklicka på FIMService och klicka sedan på Egenskaper.

3.  På sidan Filer utökar du databasfilerna till önskad storlek.

### Isolera loggen från datafiler
<a id="isolate-log-from-data-files" class="xliff"></a>

Följ rekommenderade metoder för SQL-servern för att isolera transaktionen och dataloggfilerna för databaserna på separata fysiska diskar.

Skapa ytterligare tempdb-filer

För att uppnå optimala prestanda rekommenderar vi att du skapar en datafil per processorkärna i tempdb-filen.

### Skapa ytterligare tempdb-filer
<a id="to-create-additional-tempdb-files" class="xliff"></a>

1.  Starta SQL Server Management Studio.

2.  Gå till databasens tempdb i Systemdatabaser, högerklicka på tempdb och klicka sedan på Egenskaper.

3.  Skapa en datafil för varje processorkärna på sidan Filer. Se till att skilja tempdb-data och loggfilerna åt på olika enheter och spindlar.

### Se till att det finns tillräckligt med utrymme för loggfiler
<a id="ensure-adequate-space-for-log-files" class="xliff"></a>

Det är viktigt att förstå återställningsmodellens diskkrav. Enkelt återställningsläge kan vara lämpligt under den inledande systembelastningen för att begränsa diskutrymmesanvändningen, men data som skapas efter den senaste säkerhetskopieringen riskerar dataförlust. När du använder fullständigt återställningsläge måste du hantera diskanvändningen via säkerhetskopieringar, som omfattar täta säkerhetskopieringar av transaktionsloggen för att förhindra hög diskutrymmesanvändning. Mer information finns i [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Översikt över återställningsmodell).

### Begränsa SQL Server-minne
<a id="limit-sql-server-memory" class="xliff"></a>

Beroende på hur mycket minne som finns på SQL Server och om du delar SQL Server med andra tjänster (d.v.s. MIM 2016-tjänsten och MIM 2016-synkroniseringstjänsten) kanske du vill begränsa minnesanvändningen för SQL. Det kan du göra på följande sätt.

1.  Starta SQL Server Enterprise Manager.

2.  Välj Ny fråga.

3.  Kör följande fråga:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  Det här exemplet konfigurerar om SQL Server så att den inte använder mer än 12 gigabyte (GB) minne.

4.  Verifiera inställningen med hjälp av följande fråga:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### Konfiguration av säkerhetskopiering och återställning
<a id="backup-and-recovery-configuration" class="xliff"></a>

Normalt bör du säkerhetskopiera databasen enligt organisationens säkerhetskopieringsprinciper. Om inkrementella säkerhetskopior inte är planerade bör databasen ställas in på enkelt återställningsläge. Se till att du förstår innebörden av de olika återställningsmodellerna innan du implementerar en säkerhetskopieringsstrategi, samt diskutrymmeskraven för dessa modeller. Den fullständiga återställningsmodellen kräver täta loggsäkerhetskopior för att undvika hög diskutrymmesanvändning. Mer information finns i [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Översikt över återställningsmodell) och [Guide för säkerhetskopiering och återställning av FIM 2010](http://go.microsoft.com/fwlink/?LinkID=165864).

## Skapa ett administratörskonto för säkerhetskopiering för FIM-tjänsten efter installation
<a id="create-a-backup-administrator-account-for-the-fimservice-after-installation" class="xliff"></a>


>[!IMPORTANT]
Medlemmar i FIM-tjänstens administratörsuppsättning har unika behörigheter som är viktiga för FIM-distributionen. Om du inte kan logga in som en del i administratörsuppsättningen är den enda lösningen att återgå till en tidigare säkerhetskopia av systemet. Vi rekommenderar att du åtgärdar denna situation genom att lägga till andra användare i den administrativa uppsättningen för FIM som en del av konfigurationen efter installation.

## FIM-tjänst
<a id="fim-service" class="xliff"></a>


### Konfigurera FIM-tjänstens Service Exchange-postlåda
<a id="configuring-fim-service-service-exchange-mailbox" class="xliff"></a>

Följande är rekommenderade metoder för att konfigurera Microsoft Exchange Server för MIM 2016-tjänstens tjänstekonto.

- Konfigurera tjänstekontot så att det endast kan ta emot e-post från interna e-postadresser. Tjänstekontots postlåda ska specifikt aldrig kunna ta emot e-post från externa SMTP-servrar.

#### Konfigurera tjänstkontot
<a id="to-configure-the-service-account" class="xliff"></a>

1.  I Exchange Management-konsolen väljer du **FIM-tjänstens tjänstkonto**.

2.  Välj Egenskaper, välj Inställningar för e-postflöde och välj sedan **Mail Delivery Restrictions** (Begränsningar för e-postleverans).

3.  Markera kryssrutan **Kräv att alla avsändare är autentiserade**.

Mer information finns i [Configure Message Delivery Restrictions](http://go.microsoft.com/fwlink/?LinkID=183625) (Konfigurera begränsningar för meddelandeleverans).

-   Konfigurera tjänstekontot så att det avvisar e-postmeddelanden som har en storlek över 1 MB. Följ rekommenderade metoder för att [konfigurera begränsningar för meddelandestorlek](http://go.microsoft.com/fwlink/?LinkID=183626) för en brevlåda eller e-postaktiverad offentlig mapp.

-   Konfigurera tjänstekontot så att det har en kvot för e-postlagring på 5 GB. För bästa resultat följer du de rekommenderade metoder som anges i [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929) (Konfigurera lagringskvoter för en postlåda).

## MIM-portal
<a id="mim-portal" class="xliff"></a>


### Inaktivera SharePoint-indexering
<a id="disable-sharepoint-indexing" class="xliff"></a>

Vi rekommenderar att du inaktiverar Microsoft Office SharePoint®-indexering. Det finns inga dokument som måste vara indexerade och indexering orsakar många poster i felloggen och utgör en risk för prestandaproblem med FIM 2010. Inaktivera SharePoint-indexering

1.  Klicka på Start på den server som är värd för 2016 MIM-portalen.

2.  Klicka på Alla program.

3.  I listan Alla program klickar du på Administrationsverktyg.

4.  Klicka på Central Administration av SharePoint under Administrationsverktyg.

5.  Klicka på Åtgärder på sidan Central administration.

6.  Under Global konfiguration på sidan Åtgärder klickar du på Definitioner av tidsinställda jobb.

7.  På sidan Definitioner av tidsinställda jobb klickar du på Uppdatering för SharePoint Services Search.

8.  Klicka på Inaktivera på sidan Redigera tidsinställt jobb.

## Inledande datainläsning för MIM 2016
<a id="mim-2016-initial-data-load" class="xliff"></a>

Det här avsnittet innehåller ett antal åtgärder för att förbättra prestanda för den inledande datainläsningen från ett externt system till FIM 2010. Det är viktigt att förstå att ett antal av dessa steg är tillfälliga under den första populationen av systemet och ska återställas när den har slutförts. Detta är en engångsåtgärd och är inte en kontinuerlig synkronisering.

>[!NOTE]
Mer information om hur du synkroniserar användare mellan FIM 2010 och Active Directory Domain Services (AD DS) finns i [Hur synkroniserar jag användare från Active Directory till FIM](http://go.microsoft.com/fwlink/?LinkID=188277) i FIM-dokumentationen.

>[!IMPORTANT]
Se till att du har följt de rekommenderade metoder som beskrivs i avsnittet om SQL-konfiguration i den här handledningen.                                                                                                                                                      |

### Steg 1: Konfigurera SQL Server för inledande datainläsning
<a id="step-1-configure-the-sql-server-for-initial-data-load" class="xliff"></a>
När du planerar att läsa in stora mängder data för första gången kan du förkorta den tid det tar att fylla databasen genom att tillfälligt stänga av fulltextsökningen och sedan sätta på den igen när exporten i MIM 2016-hanteringsagenten (FIM MA) har slutförts.

Stänga av fulltextsökning tillfälligt:

1.  Starta SQL Server Management Studio.

2.  Välj Ny fråga.

3.  Kör följande SQL-uttryck:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

Det är viktigt att du förstår diskkraven för SQL Server-återställningsmodellen. Beroende på schemat för säkerhetskopiering kan du överväga att använda enkelt återställningsläge under den inledande systeminläsningen för att begränsa diskutrymmesanvändningen, men du måste förstå vad detta innebär ur dataförlustperspektiv.
När du använder fullständigt återställningsläge måste du hantera diskanvändningen via säkerhetskopieringar, som omfattar täta säkerhetskopieringar av transaktionsloggen för att förhindra hög diskutrymmesanvändning.

>[!IMPORTANT]
Om du inte implementerar dessa metoder kan det leda till hög diskutrymmesanvändning, som eventuellt kan leda till att diskutrymmet tar slut. Du hittar mer information om det här avsnittet i [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Översikt över återställningsmodell). [Guide för säkerhetskopiering och återställning av FIM](http://go.microsoft.com/fwlink/?LinkID=165864) innehåller ytterligare information.

### Steg 2: Använd den minsta nödvändiga MIM-konfigurationen under inläsningen
<a id="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process" class="xliff"></a>

Under den inledande inläsningen bör du bara använda den lägsta konfiguration som krävs av FIM-konfigurationen för hanteringsprincipreglerna (MPR) och uppsättningsdefinitionerna. När inläsningen är klar kan du skapa ytterligare uppsättningar som krävs för distributionen. Använd inställningen Kör vid principuppdatering på åtgärdsarbetsflöden för att verkställa dessa principer retroaktivt på inlästa data.

### Steg 3: Konfigurera och fylla i FIM-tjänsten med externa identitetsdata
<a id="step-3-configure-and-populate-the-fim-service-with-external-identity-data" class="xliff"></a>


Du bör nu följa de metoder som beskrivs i Hur synkroniserar jag användare från Active Directory

Domain Services för FIM för att konfigurera och synkronisera systemet med användare från Active Directory. Om du behöver synkronisera gruppinformationen beskrivs metoderna för den processen i Hur synkroniserar jag grupper från Active Directory Domain Services till FIM.

#### Synkroniserings- och exportsekvenser
<a id="synchronization-and-export-sequences" class="xliff"></a>

Optimera prestanda genom att köra en export efter en synkroniseringskörning som ger ett stort antal väntande exportåtgärder i en anslutarplats.

Kör sedan en bekräftande importkörning på hanteringsagenten som är kopplad till berörd anslutarplats. Om du till exempel behöver köra körningsprofiler för synkronisering på flera hanteringsagenter som en del av en inledande databelastning bör du köra en export följt av en deltaimport efter varje enskild synkroniseringskörning.

Utför följande steg för varje källhanteringsagent som är en del av initieringscykeln:

1.  Fullständig import på en källhanteringsagent.

2.  Fullständig synkronisering på källhanteringsagenten.

3.  Export på alla berörda målhanteringsagenter med stegvisa exportåtgärder.

4.  Deltaimport på alla berörda målhanteringsagenter med stegvisa exportåtgärder.

### Steg 4: Verkställa den fullständiga MIM-konfigurationen
<a id="step-4-apply-your-full-mim-configuration" class="xliff"></a>


När den första datainläsningen har slutförts bör du verkställa den fullständiga MIM-konfigurationen för distributionen.

Beroende på dina scenarier kan detta omfatta att skapa ytterligare uppsättningar, MPR och arbetsflöden. Vid eventuella principer som du måste verkställa retroaktivt på alla befintliga objekt i systemet använder du inställningen Kör vid principuppdatering på åtgärdsarbetsflöden för att verkställa dessa principer retroaktivt på inlästa data.

### Steg 5: Konfigurera om SQL till tidigare inställningar
<a id="step-5-reconfigure-sql-to-previous-settings" class="xliff"></a>


Kom ihåg att ändra SQL-inställningen till dess normala inställningar. Du måste bland annat:

-   Aktivera fulltextsökning

-   Uppdatera säkerhetskopieringsprincipen enligt organisationsprincipen

När du har slutfört den inledande datainläsningen måste du aktivera fulltextsökning igen. Kör följande SQL-uttryck för att aktivera fulltextsökning igen:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Om du vill växla till enkelt återställningsläge ser du till att konfigurera om säkerhetskopieringsschemat i enlighet med organisationens säkerhetskopieringsprincip. Ytterligare information om säkerhetskopieringsscheman i FIM finns i [Guide för säkerhetskopiering och återställning av FIM](http://go.microsoft.com/fwlink/?LinkID=165864).

## Konfigurationsmigrering
<a id="configuration-migration" class="xliff"></a>


### Undvik att ändra visningsnamn
<a id="avoid-changing-display-names" class="xliff"></a>

För många objekttyper, till exempel MPR, använder skriptet syncproduction.ps1 visningsnamnet som det enda fästpunktsattributet mellan två system. Det innebär att om ett befintligt MPR-visningsnamn ändras raderas befintlig MPR och en ny MPR skapas. Det beror på att migreringsprocessen inte kan sammankoppla hanteringsprincipregler vars kopplingsvillkor har ändrats. Du kan undvika detta problem genom att koppla ett anpassat attribut till alla konfigurationsobjekttyper och använda det attributet som kopplingsvillkor. På så sätt kan du ändra visningsnamn utan att påverka migreringsprocessen.

### Undvik att ändra innehållet i mellanliggande filer
<a id="avoid-changing-the-content-of-intermediate-files" class="xliff"></a>

Även om filformatet och Application Programming Interface (API) för objekten på låg nivå är offentliga och ändringar stöds av utvecklare rekommenderar vi inte att du ändrar innehållet i mellanliggande format under migreringen. Det kan dock vara nödvändigt att ta bort hela importobjekt från changes.xml eller utföra åtgärder för att söka och ersätta i pilot.xml för att byta ut versionsnummer eller pilot-DNS-information (Domain Name System) mot produktions-DNS-information.

### Se till att versionsnumret i pilot.xml är rätt vid migrering mellan versioner
<a id="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions" class="xliff"></a>

Även om migreringar mellan versionsnummer varken rekommenderas eller stöds kan du ofta göra detta genom att byta ut pilotversionsnumret mot produktionsversionsnumret i pilot.xml. Mer specifikt kräver objekten WorkflowDefinition och

ActivityInformationConfiguration versionsnumret för att exakt kunna hänvisa till arbetsflödesaktiviteter i produktionsmiljön. Om versionsnumret inte byts ut leder det till att cmdlet:en Compare-FIMConfig identifierar skillnader mellan XOML-attributen (Extensible Object Markup Language) i WorkflowDefinitions och migrerar pilotens versionsnummer. FIM-tjänsten för produktion kan misslyckas med att starta arbetsflödesaktiviteter med fel versionsnummer.

### Undvik cykliska referenser
<a id="avoid-cyclic-references" class="xliff"></a>

Normal rekommenderas inte cykliska referenser i en MIM-konfiguration.
Cykler uppstår dock ibland när uppsättning A hänvisar till uppsättning B och uppsättning B också hänvisar till uppsättning A. För att undvika problem med cykliska referenser bör du ändra definitionen i uppsättning A eller uppsättning B så att de båda inte hänvisar till varandra. Starta sedan om migreringsprocessen. Om du har cykliska referenser och cmdlet:en Compare-FIMConfig ger ett fel som resultat måste du bryta cykeln manuellt. Eftersom cmdlet:en Compare-FIMConfig skapar en lista med ändringar i prioritetsordning får det inte finnas några cykler bland referenserna för konfigurationsobjekt.

## Säkerhet
<a id="security" class="xliff"></a>

### MIM MA-konto
<a id="mim-ma-account" class="xliff"></a>

MIM MA-kontot betraktas inte som ett tjänstkonto och ska vara ett vanligt användarkonto. Kontona måste kunna logga in lokalt för att FIM-synkroniseringstjänstens tjänstkonto ska kunna personifiera den.

Aktivera MIM MA-kontot för lokal inloggning

1.  Klicka på Start, klicka på Administrationsverktyg och klicka sedan på Lokal säkerhetsprincip.

2.  Öppna noden Lokala principer och klicka på Tilldelning av användarrättigheter.

3.  I principen Tillåt lokal inloggning ser du till att FIM MA-kontot anges uttryckligen, annars lägger du till det i en av de grupper som redan har åtkomst.

### FIM-synkroniseringstjänst och FIM-tjänstekonton
<a id="fim-synchronization-service-and-fim-services-accounts" class="xliff"></a>

Tjänstekontona bör vara begränsade för att kunna konfigurera servrarna som kör MIM-serverkomponenterna på ett säkert sätt. Använd föregående metod för att aktivera MIM MA-kontot och ange följande begränsningar för FIM-synkroniseringstjänsten och FIM-tjänstekontona:

-   Neka inloggning som batchjobb

-   Neka lokal inloggning

-   Neka tillgång till den här datorn från nätverket

Tjänstekontona bör inte vara medlem i den lokala administratörsgruppen.

FIM-synkroniseringstjänstens tjänstekonto bör inte vara medlem i de säkerhetsgrupper som används för att styra åtkomst till FIM-synkroniseringstjänsten (grupper som börjar med FIMSync, till exempel FIMSyncAdmins o.s.v.).

>[!IMPORTANT]
 Om du väljer alternativen för att använda samma konto för båda tjänstekontona och inte skiljer FIM-tjänsten och FIM-synkroniseringstjänsten åt kan du inte konfigurera Neka tillgång till den här datorn från nätverket på mms-synkroniseringstjänstens server. Om åtkomst nekas förhindrar det att FIM-tjänsten kontaktar FIM-synkroniseringstjänsten för att ändra konfigurationen och hantera lösenord.

### Lösenordsåterställning som distribueras på kioskliknande datorer bör ange lokal säkerhet för att rensa virtuell minnesväxlingsfil
<a id="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile" class="xliff"></a>

Vid distribution av FIM-lösenordsåterställning på en dator som är avsedd att vara en kiosk rekommenderar vi att den lokala säkerhetsprincipinställningen Avstängning: Rensa den virtuella växlingsfilen aktiveras för att säkerställa att känslig information från processminnet inte är tillgänglig för obehöriga användare.

### Implementera SSL för FIM-portalen
<a id="implementing-ssl-for-the-fim-portal" class="xliff"></a>

Vi rekommenderar starkt att du använder SSL (secure sockets layer) på FIM-portalservern för att skydda trafiken mellan klienterna och servern.

Implementera SSL:

1.  Öppna IIS-hanteraren på MIM-portalservern.

2.  Klicka på namnet på den lokala datorn.

3.  Klicka på Servercertifikat.

4.  Klicka på Skapa certifikatförfrågan.

5.  I textrutan Eget namn anger du serverns namn.

6.  Klicka på Nästa och sedan på Nästa.

7.  Spara filen var som helst. Du behöver ha åtkomst till denna plats under följande steg.

8.  I Windows Internet Explorer® bläddrar du till https://servernamn/certsrv. Ersätt servernamn med namnet på den server som utfärdar certifikat.

9.  Klicka på Begär ett nytt certifikat.

10. Klicka på Skicka en avancerad begäran.

11. Klicka på Skicka en certifikatbegäran genom att använda en base-64-krypterad.

12. Klistra in innehållet i filen som du sparade i föregående steg.

13. I Certifikatmall väljer du Webbserver.

14. Klicka på Skicka.

15. Spara certifikatet på skrivbordet.

16. I IIS-hanteraren klickar du på Complete Certification Request (Slutför certifieringsbegäran).

17. Led IIS-hanteraren till det certifikat du precis har sparat på skrivbordet.

18. Ange serverns namn som Eget namn.

19. Klicka på Platser och välj sedan SharePoint – 80.

20. Klicka på Bindningar och klicka sedan på Lägg till.

21. Välj https.

22. För certifikatet väljer du det som har samma namn som servern (detta är det certifikat du precis har importerat).

23. Klicka på Ok.

24. Ta bort HTTP-bindningen.

25. Klicka på SSL-inställningar och kontrollera Kräv SSL.

26. Spara inställningarna.

27. Klicka på Start, klicka på Administrationsverktyg och klicka sedan på Central administration för SharePoint 3.0.

28. Klicka på Åtgärder och klicka på Alternativa åtkomstmappningar.

29. Klicka på http://servernamn.

30. Ändra http://servernamn till https://servernamn och klicka sedan på OK.

31. Klicka på Start, klicka på Kör, skriv iisreset och klicka på OK.

## Prestanda
<a id="performance" class="xliff"></a>

För optimal prestandakonfiguration:

-   Använd de rekommenderade metoder för SQL-konfiguration som beskrivs i avsnittet om konfiguration av SQL i detta dokument.

-   Inaktivera SharePoint-indexering på FIM 2010 R2-portalplatsen. Mer information finns i avsnittet Inaktivera SharePoint-indexering i det här dokumentet.

## Specifika rekommenderade metoder (jag vill ta bort det här och komprimera det här avsnittet så att bara funktionerna finns vid rubrik 2-nivå i stället för 3)
<a id="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3" class="xliff"></a>


### Hantering av begäran
<a id="request-management" class="xliff"></a>

MIM 2016 rensar som standard förfallna systemobjekt, vilket omfattar slutförda begäranden med kopplade godkännanden, godkännandesvar och arbetsflödesinstanser under en 30-dagarsintervall. Om organisationen behöver ha en längre begäranshistorik bör du exportera begäranden från MIM och lagra dem i en extra databas för att spara dem efter perioden på 30 dagar. Även om det går att konfigurera raderingsperioden på 30 dagar för begäranden kan en förlängning av denna tid ha en negativ effekt på prestanda till följd av de ytterligare objekten i systemet.

### Hantering av principregler
<a id="management-policy-rules" class="xliff"></a>

#### Använd lämplig MPR-typ
<a id="use-the-appropriate-mpr-type" class="xliff"></a>

MIM erbjuder två typer av MPR, Begäran och Uppsättningsövergång:

-   MPR för begäran (RMPR)

 - Används för att definiera åtkomstkontrollprincipen (autentisering, auktorisering och åtgärd) för åtgärderna Skapa, Läsa, Uppdatera eller Ta bort (Create, Read, Update eller Delete, CRUD) mot resurser.
 - Tillämpas när en CRUD-åtgärd utfärdas mot en målresurs i FIM.
   - Omfattar de matchande kriterier som definieras i regeln, d.v.s. de CRUD-begäranden som regeln gäller för.


-   MPR för uppsättningsövergång (TMPR)
 - Används för att definiera principer oavsett hur objektet har fått aktuellt tillstånd som representeras av övergångsuppsättningen. Använd TMPR för modellera rättighetsprinciper.
 - Tillämpas när en resurs går in i eller lämnar en associerad uppsättning.
 - Omfattar medlemmarna i uppsättningen.

>[ANMÄRKNING] Mer information finns i [Utforma regler för affärsprinciper](http://go.microsoft.com/fwlink/?LinkID=183691).

#### Aktivera MPR endast efter behov
<a id="only-enable-mprs-as-necessary" class="xliff"></a>

Använd principen om lägsta behörighet när konfigurationen verkställs. MPR styr åtkomstprincipen för FIM-distributionen. Aktivera endast de funktioner som används av de flesta användare. Det är till exempel inte alla användare som använder FIM för hantering av grupper, så kopplade grupphanterings-MPR ska inaktiveras. FIM levereras med de flesta icke-administratörsbehörigheterna inaktiverade som standard.

#### Duplicera inbyggda MPR i stället för att ändra direkt
<a id="duplicate-built-in-mprs-instead-of-directly-modifying" class="xliff"></a>

Om du behöver ändra inbyggda MPR bör du skapa en ny MPR med den konfiguration som krävs och inaktivera inbyggd MPR. Detta säkerställer att eventuella framtida ändringar i inbyggda MPR som införs genom uppgraderingsprocessen inte påverkar systemkonfigurationen negativt.

#### Behörighet för slutanvändare bör använda explicita attributlistor som omfattar användarnas affärsbehov
<a id="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs" class="xliff"></a>

Om explicita attributlistor används hjälper det till att förhindra att behörighet beviljas av misstag till obehöriga användare när attribut läggs till i objekt.
Administratörer bör behöva bevilja åtkomst explicit till nya attribut istället för att försöka ta bort åtkomsten.

Åtkomst till data bör begränsas till användarnas affärsbehov. Gruppmedlemmar bör till exempel inte ha åtkomst till filterattributet för den grupp de är medlemmar i. Filtret kan oavsiktligt avslöja organisationsdata som användaren normalt inte ska ha åtkomst till.

#### MPR bör återspegla gällande behörigheter i systemet
<a id="mprs-should-reflect-effective-permissions-in-the-system" class="xliff"></a>

Undvik att bevilja behörighet till attribut som användaren aldrig kan använda. Du bör till exempel inte bevilja behörighet till att ändra grundläggande resursattribut som objectType. Trots MPR nekar systemet till alla försök att ändra en resurstyp efter att den har skapats.

#### Läsbehörighet ska vara separat från behörigheter för att ändra och skapa när du använder explicita attribut i MPR
<a id="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs" class="xliff"></a>

När attribut anges explicit i MPR ska attributen som krävs för att ändra och skapa normalt skilja sig från de som är tillgängliga för att läsa. Läsa kan till exempel beviljas över systemattribut som Creator eller objectId, medan Skapa eller Ändra inte kan anges för systemattribut.

#### Behörighet för att skapa ska vara separat från behörigheter för att ändra när du använder explicita attribut i regler
<a id="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules" class="xliff"></a>

Åtgärden för att skapa kräver att användaren väljer objectType som en del av åtgärden. Detta är ett grundläggande systemattribut som inte kan ändras efter en åtgärd för att skapa.

#### Använd en begärans-MPR för alla attribut med samma åtkomstkrav
<a id="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements" class="xliff"></a>

För attribut med samma åtkomstkrav som inte förväntas att ändras kan du förbättra effektiviteten genom att kombinera dem i en enda begärans-MPR.

#### Undvik att ge obegränsad åtkomst till valda huvudgrupper
<a id="avoid-giving-unrestricted-access-even-to-selected-principal-groups" class="xliff"></a>

Behörigheter definieras som en positiv försäkran i FIM. Eftersom FIM inte stöder nekande behörigheter blir det svårt att ange undantag i behörigheterna om man ger obegränsad åtkomst till en resurs. Rekommenderad metod är att endast bevilja de behörigheter som behövs.

>[!NOTE]
Avsnittet om rättigheter följer nedan. Jag undrar hur man sammanfogar dem för att undvika att skapa 5 nivårubriker
#### Använd TMPR för att definiera anpassade rättigheter
<a id="use-tmprs-to-define-custom-entitlements" class="xliff"></a>

Använd MPR för uppsättningsövergång (TMPR) i stället för RMPR för att definiera anpassade rättigheter.
TMPR utgör en tillståndsbaserad modell för att tilldela eller ta bort rättigheter baserat på medlemskap i definierade övergångsuppsättningar, eller roller, och medföljande arbetsflödesaktiviteter. TMPR ska alltid definieras i par, ett för resurser som går in och ett för resurser som går ut. Dessutom bör varje övergångs-MPR innehålla separata arbetsflöden för etablerings- och borttagningsaktiviteter.

>[!NOTE]
Eventuella borttagningsarbetsflöden bör säkerställa att attributet Kör vid principuppdatering är inställt på sant.

#### Aktivera uppsättningsövergången i MPR sist
<a id="enable-the-set-transition-in-mpr-last" class="xliff"></a>

När du skapar ett TMPR-par aktiverar du Uppsättningsövergång i MPR sist. Den här ordningen säkerställer att ingen resurs finns kvar med rättigheten om den läggs till i och tas bort från uppsättningen när in-MPR är aktiverad men innan ut-MPR aktiveras.

#### Arbetsflöden i TMPR bör kontrollera målresursens tillstånd först
<a id="workflows-in-tmpr-should-check-target-resource-state-first" class="xliff"></a>

Etableringsarbetsflöden bör först kontrollera för att fastställa om målresursen redan har etablerats i enlighet med rättigheten. Om den har det ska den inte göra någonting.

Borttagningsarbetsflöden bör först kontrollera för att fastställa om målresursen har etablerats. Om den har det ska målresursen tas bort.
Annars ska den inte göra någonting.

#### Välj Kör vid principuppdatering för TMPR
<a id="select-run-on-policy-update-for-tmprs" class="xliff"></a>

Detta säkerställer att rätt etableringsbeteende verkställs när principuppdateringar implementeras och använder flaggan Kör vid principuppdatering på åtgärdsarbetsflöden som är kopplade till TMPR. Detta säkerställer att ändringar i principdefinitionerna verkställer åtgärdsarbetsflöden på nya medlemmar i övergångsuppsättningen.

#### Undvik att koppla samma rättighet till två olika övergångsuppsättningar
<a id="avoid-associating-the-same-entitlement-with-two-different-transition-sets" class="xliff"></a>

Om samma rättighet kopplas till två olika övergångsuppsättningar kan det leda till att återkallelse och ny tilldelning av rättigheter sker i onödan om resursen flyttar från en uppsättning till en annan. Den rekommenderade metoden är att se till att en uppsättning innehåller alla resurser som kräver den kopplade rättigheten. Detta säkerställer ett förhållande en mot en mellan övergångsuppsättningen och rättigheten som beviljar arbetsflödet.

#### Använd en lämplig sekvens med åtgärder när du tar bort rättigheter i systemet
<a id="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system" class="xliff"></a>

Ordningen för stegen som utförs när rättigheter tas bort i systemet kan leda till två olika driftresultat. Se till att du förstår vilken ordning som gäller för den effekt du vill ha.

Ta bort en rättighet från systemet (och återkalla den från alla medlemmar som har rättigheten för närvarande):

1.  Inaktivera T-In MPR. På så sätt undviker du nya beviljanden.

2.  Ta bort T-Set-filtret eller ändra det så att det är tomt. Det får alla befintliga medlemmar att göra en övergång ut och det verkställer principen för övergång ut, inklusive de konfigurerade avetableringsarbetsflöden som är kopplade till rättigheten.

3.  Inaktivera T-Out MPR.

Ta bort en rättighet men låta de aktuella medlemmarna vara (t.ex. sluta använda FIM för att hantera rättigheten):

1.  Inaktivera T-In MPR. På så sätt undviker du nya beviljanden.

2.  Inaktivera T-Out MPR.

3.  Ta bort T-Set-filtret eller ändra det så att det är tomt. Eftersom uppsättningen inte längre är kopplad till en TMPR verkställs inga avetableringsarbetsflöden.

### Uppsättningar
<a id="sets" class="xliff"></a>

När du använder de rekommenderade metoderna för uppsättningar måste du överväga effekten av optimeringen på hanterbarheten och hur enkel framtida administration blir.
Lämplig testning med förväntad produktionsskala ska utföras för att identifiera den rätta balansen mellan prestanda och hanterbarhet innan dessa rekommendationer verkställs.

>[!NOTE]
Alla följande riktlinjer gäller dynamiska uppsättningar och dynamiska grupper.


#### Minska användningen av dynamisk kapsling
<a id="minimize-the-use-of-dynamic-nesting" class="xliff"></a>

Det här gäller filtret i en uppsättning som hänvisar till attributet ComputedMember i en annan uppsättning. En vanlig orsak till kapslade uppsättningar är att undvika att duplicera ett medlemskapsvillkor över flera uppsättningar. Även om den här metoden kan ge bättre hanterbarhet för uppsättningarna försämras prestanda. Du kan förbättra prestandan genom att duplicera medlemskapsvillkoren för en kapslad uppsättning i stället för att kapsla själva uppsättningen.

Det kan hända att du påträffar fall då du inte kan undvika kapsling av uppsättningar för att tillfredsställa ett funktionskrav. Det här är de huvudsakliga situationer då du bör kapsla uppsättningar. Kapsling av uppsättningar måste till exempel användas på följande sätt för att definiera uppsättningen för alla grupper utan ägare som är heltidsmedarbetare: `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]`, där ”X” är uppsättningens ObjectID för alla heltidsmedarbetare.

#### Minska användningen av negativa villkor
<a id="minimize-the-use-of-negative-conditions" class="xliff"></a>

Negativa villkor är de medlemskapsvillkor som använder följande operatorer eller funktioner: `!=`, `not()`, `\<` , `\<=`. Optimera prestanda genom att uttrycka det villkor du vill använda med flera positiva villkor i stället för ett negativt villkor, där det är möjligt.

#### Minska användningen av medlemskapsvillkor baserat på referensattribut med flera värden
<a id="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes" class="xliff"></a>

Användning av villkor baserat på referensattribut med flera värden ska minimeras, eftersom om det finns stora mängder sådana uppsättningar kan det påverka prestanda för åtgärder på det attribut som används i medlemskapsvillkoret.

### Återställning av lösenord
<a id="password-reset" class="xliff"></a>

#### Kioskliknande datorer som används för lösenordsåterställning bör ange lokal säkerhet för att rensa den virtuella minnesväxlingsfilen
<a id="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile" class="xliff"></a>

Vid distribution av FIM 2010-lösenordsåterställning på en dator som är avsedd att vara en kiosk rekommenderar vi att den lokala säkerhetsprincipinställningen Avstängning: Rensa den virtuella växlingsfilen aktiveras för att säkerställa att känslig information från processminnet inte är tillgänglig för obehöriga användare.

#### Användare bör alltid registrera sig för lösenordsåterställning på en dator som de är inloggade på
<a id="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to" class="xliff"></a>

När en användare försöker registrera sig för lösenordsåterställning via en webbportal initierar FIM 2010 alltid registrering för den inloggade användarens räkning, oavsett vem som är inloggad på webbplatsen. Användare bör alltid registrera sig för lösenordsåterställning på en dator som de är inloggade på.

#### Ställ inte in registernyckeln AvoidPdcOnWan på true
<a id="do-not-set-the-avoidpdconwan-registry-key-to-true" class="xliff"></a>

När du använder MIM 2016-lösenordsåterställning ska du inte ställa in registernyckeln AvoidPdcOnWan på true.

Om den här registernyckeln ställs in på true är det mycket troligt att användaren går igenom lösenordsportarna, får sitt lösenord återställt på den primära domänkontrollanten (PDC) och försöker logga in. På grund av den här registernyckeln utför inte den lokala domänkontrollanten den sekundära valideringen med PDC och därför nekas inloggningsbegäran. Om användaren nekas tillräckligt många gånger kan denne bli utelåst från domänen och måste ringa support.

#### Aktivera inte loggning av lösenord i klartext
<a id="do-not-turn-on-logging-of-clear-text-passwords" class="xliff"></a>

Det är möjligt att logga lösenord i klartext när spårning av diagnostiktjänstenivå aktiveras i Windows

Communication Foundation (WCF). Det här alternativet är inaktiverat som standard och vi avråder från att aktivera det i produktionsmiljöer. De här lösenorden visas som klartextelement i ett krypterat SOAP-meddelande (Simple Object Access Protocol) när användarna registrerar sig för lösenordsåterställning. Mer information finns i [Configuring Message Logging](http://go.microsoft.com/fwlink/?LinkID=168572) (Konfigurera meddelandeloggning).

#### Mappa inte ett auktoriseringsarbetsflöde till processen för lösenordsåterställning
<a id="do-not-map-an-authorization-workflow-to-the-password-reset-process" class="xliff"></a>

Du bör inte koppla ett auktoriseringsarbetsflöde till en åtgärd för lösenordsåterställning.
Lösenordsåterställning kräver ett synkront svar, och auktoriseringsarbetsflöden som innehåller aktiviteter som exempelvis godkännandeaktiviteten är asynkrona.

#### Mappa inte flera åtgärdsaktiviteter till lösenordsåterställning
<a id="do-not-map-multiple-action-activities-to-password-reset" class="xliff"></a>

Du bör inte koppla ett arbetsflöde som innehåller fler än en åtgärdsaktivitet till en åtgärd för lösenordsåterställning. Ett exempelscenario skulle vara att bifoga en andra aktivitet för AD DS-lösenordsåterställning till en MPR för lösenordsåterställning. Det här scenariot stöds inte.

#### Kräv omregistrering när aktiviteter läggs till, tas bort eller deras ordning ändras i ett befintligt arbetsflöde
<a id="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow" class="xliff"></a>

När du lägger till, tar bort eller ändrar ordningen för autentiseringsaktiviteter i ett befintligt arbetsflöde ska du alltid välja alternativet att kräva omregistrering. Användare som försöker autentisera för lösenordsåterställning efter att en aktivitet har lagts till eller tagits bort från ett arbetsflöde, men innan de har registrerats om kan råka ut för oönskade effekter.

### Portalkonfiguration och konfiguration av skärmen för resurskontroll
<a id="portal-configuration-and-resource-control-display-configuration" class="xliff"></a>

#### Överväg att lägga till en sekretessfriskrivningsklausul på användarprofilsidan
<a id="consider-adding-a-privacy-disclaimer-to-the-user-profile-page" class="xliff"></a>

I MIM kan en del användarprofilinformation som standard visas för andra användare. För att hjälpa användarna bör administratörerna överväga att lägga till anpassad text på användarprofilsidan som följer företagets principer. Mer information om att lägga till en anpassad text på en MIM-portalsida finns i introduktionen till att [konfigurera och anpassa FIM-portalen](http://go.microsoft.com/fwlink/?LinkID=165848).

### Schema
<a id="schema" class="xliff"></a>

#### Ta inte bort resurstyperna Person eller Grupp
<a id="do-not-delete-person-or-group-resource-types" class="xliff"></a>

Även om resurstyperna Person och Grupp inte är markerade som huvudresurstyper ska själva resurserna eller attributen som har tilldelats till dem inte tas bort. Användargränssnittet (UI) i MIM-portalen kräver att resurstyperna Person och Grupp och deras attribut finns.

#### Ändra inte huvudattributen
<a id="do-not-modify-the-core-attributes" class="xliff"></a>

Det finns 13 huvudattribut som tilldelas till alla resurstyper. Du bör inte på något sätt ändra deras relation till någon resurstyp. De 13 huvudattributen är:

-   CreatedTime

-   Creator

-   DeletedTime

-   Beskrivning

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Språk

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Ta inte bort schemaresursen med ett beroende av granskningskrav

Du bör inte ta bort schemaresurserna medan du fortfarande har granskningskrav för dessa resurser.

#### Göra reguljära uttryck skiftlägesokänsliga
<a id="making-regular-expressions-case-insensitive" class="xliff"></a>

I FIM kan det vara bra att göra vissa reguljära uttryck skiftlägesokänsliga. Du kan ignorera skiftläge i en grupp genom att använda ?!:. För Typ av anställd kan du till exempel använda

`\^(?!:contractor\|full time employee)%.`

#### Beräkning av medlemsattributet
<a id="calculation-of-the-member-attribute" class="xliff"></a>

Medlemsattributet som är exponerat för synkroniseringsmotorn mappas faktiskt till ComputedMembers. Det är en kombination av villkorsbaserade medlemmar och manuellt valda medlemmar. Även om du lägger till alla tre attributen (Filter, ExplicitMembers och ComputedMembers) sker inte den dynamiska beräkningen av medlemsattributet för andra resurstyper än grupp och uppsättning.

#### Inledande och avslutande blanksteg i strängar ignoreras
<a id="leading-and-trailing-spaces-in-strings-are-ignored" class="xliff"></a>

I FIM kan du ange strängar med inledande och avslutande blanksteg, men FIM-systemet ignorerar dessa blanksteg. Om du skickar en sträng med ett ledande och avslutande blanksteg ignorerar synkroniseringsmotorn och webbtjänster dessa blanksteg.

#### Tomma strängar är inte lika med null
<a id="empty-strings-do-not-equal-null" class="xliff"></a>

Tomma strängar är inte lika med null i den här versionen av FIM. Tomma strängindata betraktas som ett giltigt värde. Ej tillgängligt betraktas som ett null-värde.

### Arbetsflöde och bearbetning av begäran
<a id="workflow-and-request-processing" class="xliff"></a>

#### Ta inte bort standardarbetsflöden som medföljer MIM 2016
<a id="do-not-delete-default-workflows-that-are-shipped-with-mim-2016" class="xliff"></a>

Följande arbetsflöden medföljer FIM 2010 och ska inte tas bort:

-   Förfalloarbetsflöde

-   Filtervalideringsarbetsflöde för administratörer

-   Filtervalideringsarbetsflöde för icke-administratörer

-   Arbetsflöde för meddelande om gruppförfallodatum

-   Gruppvalideringsarbetsflöde

-   Arbetsflöde för ägargodkännande

-   Arbetsflöde för lösenordsåterställningsåtgärd

-   AuthN-arbetsflöde för lösenordsåterställning

-   Förfrågarvalidering med ägarauktorisering

-   Förfrågarvalidering utan ägarauktorisering

-   Systemarbetsflöde som krävs för registrering

#### Kör inte två eller flera godkännandeaktiviteter parallellt
<a id="do-not-run-two-or-more-approvalactivities-in-parallel" class="xliff"></a>

Du ska inte köra två eller flera godkännandeaktiviteter parallellt. Det kan leda till att begäran fastnar i auktoriseringsfasen. För flera godkännanden kan du antingen inkludera en större lista med godkännare i godkännandet eller sätta de två aktiviteterna i en sekvens efter varandra.

#### Auktoriseringsaktiviteter bör inte ändra MIM-resursdata
<a id="authorization-activities-should-not-modify-mim-resources-data" class="xliff"></a>

Undvik att använda aktiviteter som ändrar MIM-resurserna, t.ex. funktionsutvärderaraktiviteten, som en del av arbetsflödena i auktoriseringsarbetsflöden. Eftersom begäran inte har utförts under auktoriseringspunkten i bearbetningen kan eventuella ändringar som görs i identitetsinformationen verkställas trots att begäran kan komma att avvisas.

### Förstå FIM-tjänstpartitioner
<a id="understanding-fim-service-partitions" class="xliff"></a>

Målet med FIM är att bearbeta begäranden som kan initieras av olika FIM-klienter, till exempel FIM-synkroniseringstjänsten och självbetjäningskomponenterna, enligt konfigurerade affärsregler. Varje FIM-tjänstinstans har skapats så att den tillhör en logisk grupp som består av en eller flera FIM-tjänstinstanser, som också kallas FIM-tjänstpartition. Om du endast har en FIM-tjänstinstans distribuerad för att hantera alla begäranden kan det hända att det uppstår fördröjningar i bearbetningen. En del åtgärder kan även överskrida de standardtidsgränsvärden som gäller för självbetjäningsåtgärder. FIM-tjänstpartitioner kan hjälpa dig att lösa detta problem. Mer information finns i Förstå FIM-tjänstpartitioner.

