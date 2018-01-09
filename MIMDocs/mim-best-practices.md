---
title: "Rekommenderade metoder för Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/05/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bb967bfb43218384044e324c270d3d6b35d33afe
ms.sourcegitcommit: b4513f0f72ac6efd5c2610863f4e3e8c8e65c860
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/08/2018
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Rekommenderade metoder för Microsoft Identity Manager 2016

Det här avsnittet beskriver rekommenderade metoder för att distribuera och använda Microsoft Identity Manager 2016 (MIM)

## <a name="sql-setup"></a>Konfiguration av SQL
>[!NOTE]
Följande rekommendationer för att konfigurera en server som kör SQL förutsätter en SQL-instans som är dedikerad till FIM-tjänsten och en SQL-instans som är dedikerad till FIMSynchronizationService-databasen. Om du kör FIM-tjänsten i en konsoliderad miljö måste du göra de justeringar som krävs för din konfiguration.

Konfiguration av SQL-servern (Structured Query Language) är viktig för optimala systemprestanda. Optimala MIM-prestanda i storskaliga implementeringar beror på användning av rekommenderade metoder för en server som kör SQL. Mer information finns i följande avsnitt om rekommenderade metoder för SQL:

-   [10 bästa metoderna för lagring](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Optimera tempdb-prestanda](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [Artikel om SQL Server praxis](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Organisera om och bygga om index](http://go.microsoft.com/fwlink/?LinkID=188269)

### <a name="presize-data-and-log-files"></a>Ange storlek på data och loggfiler på förhand

Förlita dig inte på automatisk tillväxt. Hantera i stället tillväxten av dessa filer manuellt. Du kan låta automatisk tillväxt vara på av säkerhetsskäl, men du bör hantera datafilernas tillväxt proaktivt. Exempelstorlekar för MIM-databasen finns i [Guide för kapacitetsplanering för FIM](http://go.microsoft.com/fwlink/?LinkID=185246).

### <a name="to-presize-sql-data-and-log-files"></a>Ange storlek på data och loggfiler på förhand

1.  Starta SQL Server Management Studio.

2.  Navigera till databasen FIMService, högerklicka på FIMService och klicka sedan på Egenskaper.

3.  På sidan Filer utökar du databasfilerna till önskad storlek.

### <a name="isolate-log-from-data-files"></a>Isolera loggen från datafiler

Följ rekommenderade metoder för SQL-servern för att isolera transaktionen och dataloggfilerna för databaserna på separata fysiska diskar.

Skapa ytterligare tempdb-filer

För att uppnå optimala prestanda rekommenderar vi att du skapar en datafil per processorkärna i tempdb-filen.

### <a name="to-create-additional-tempdb-files"></a>Skapa ytterligare tempdb-filer

1.  Starta SQL Server Management Studio.

2.  Gå till databasens tempdb i Systemdatabaser, högerklicka på tempdb och klicka sedan på Egenskaper.

3.  Skapa en datafil för varje processorkärna på sidan Filer. Se till att skilja tempdb-data och loggfilerna åt på olika enheter och spindlar.

### <a name="ensure-adequate-space-for-log-files"></a>Se till att det finns tillräckligt med utrymme för loggfiler

Det är viktigt att förstå återställningsmodellens diskkrav. Enkelt återställningsläge kan vara lämpligt under den inledande systembelastningen för att begränsa diskutrymmesanvändningen, men data som skapas efter den senaste säkerhetskopieringen riskerar dataförlust. När du använder fullständigt återställningsläge måste du hantera diskanvändningen via säkerhetskopieringar, som omfattar täta säkerhetskopieringar av transaktionsloggen för att förhindra hög diskutrymmesanvändning. Mer information finns i [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Översikt över återställningsmodell).

### <a name="limit-sql-server-memory"></a>Begränsa SQL Server-minne

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

  Det här exemplet konfigurerar SQLServer för att använda mer än 12 gigabyte (GB) minne.

4.  Verifiera inställningen med hjälp av följande fråga:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### <a name="backup-and-recovery-configuration"></a>Konfiguration av säkerhetskopiering och återställning

I allmänhet bör du arbeta med databasadministratören för att utforma en strategi för säkerhetskopiering och återställning. Vissa rekommendationerna omfattar:
- Säkerhetskopiera databasen enligt organisationens princip för säkerhetskopiering. 
- Om inkrementella säkerhetskopior inte är planerade bör databasen ställas in på enkelt återställningsläge. 
- Se till att du förstår följderna av olika återställningsmodeller innan du implementerar din strategi för säkerhetskopiering. Läs om kraven på diskutrymme för dessa modeller. Den fullständiga återställningsmodellen kräver täta loggsäkerhetskopior för att undvika hög diskutrymmesanvändning. 

Mer information finns i [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Översikt över återställningsmodell) och [Guide för säkerhetskopiering och återställning av FIM 2010](http://go.microsoft.com/fwlink/?LinkID=165864).

## <a name="create-a-backup-administrator-account-for-the-fim-service-after-installation"></a>Skapa ett administratörskonto för säkerhetskopiering för FIM-tjänsten efter installationen

Medlemmar i uppsättningen av FIM-tjänst-administratörer har unika behörigheter som är nödvändiga för att fungera i MIM-distributionen. Om du inte logga in som en del av uppsättningen administratörer är den enda lösningen att återställa en tidigare säkerhetskopia av systemet. Vi rekommenderar att du åtgärdar denna situation genom att lägga till andra användare i den administrativa uppsättningen för FIM som en del av konfigurationen efter installation.

## <a name="fim-service"></a>FIM-tjänst


### <a name="configuring-fim-service-service-exchange-mailbox"></a>Konfigurera FIM-tjänstens Service Exchange-postlåda

Följande är rekommenderade metoder för att konfigurera Microsoft Exchange Server för MIM 2016-tjänstens tjänstekonto.

- Konfigurera tjänstekontot så att det endast kan ta emot e-post från interna e-postadresser. Tjänstekontots postlåda ska specifikt aldrig kunna ta emot e-post från externa SMTP-servrar.

#### <a name="to-configure-the-service-account"></a>Konfigurera tjänstkontot

1.  I Exchange Management-konsolen väljer du **FIM-tjänstens tjänstkonto**.

2.  Välj Egenskaper, välj Inställningar för e-postflöde och välj sedan **Mail Delivery Restrictions** (Begränsningar för e-postleverans).

3.  Markera kryssrutan **Kräv att alla avsändare är autentiserade**.

Mer information finns i [Configure Message Delivery Restrictions](http://go.microsoft.com/fwlink/?LinkID=183625) (Konfigurera begränsningar för meddelandeleverans).

-   Konfigurera tjänstekontot så att det avvisar e-postmeddelanden som har en storlek över 1 MB. Följ rekommenderade metoder för att [konfigurera begränsningar för meddelandestorlek](http://go.microsoft.com/fwlink/?LinkID=183626) för en brevlåda eller e-postaktiverad offentlig mapp.

-   Konfigurera tjänstekontot så att det har en kvot för e-postlagring på 5 GB. För bästa resultat följer du de rekommenderade metoder som anges i [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929) (Konfigurera lagringskvoter för en postlåda).

## <a name="mim-portal"></a>MIM-portal


### <a name="disable-sharepoint-indexing"></a>Inaktivera SharePoint-indexering

Vi rekommenderar att du inaktiverar Microsoft Office SharePoint®-indexering. Det finns inga dokument som behöver indexeras. Indexering gör att många poster och eventuella prestandaproblem i MIM. Om du vill inaktivera SharePoint-indexering utför stegen nedan:

1.  Klicka på Start på den server som är värd för 2016 MIM-portalen.

2.  Klicka på Alla program.

3.  I listan Alla program klickar du på Administrationsverktyg.

4.  Klicka på Central Administration av SharePoint under Administrationsverktyg.

5.  Klicka på Åtgärder på sidan Central administration.

6.  Under Global konfiguration på sidan Åtgärder klickar du på Definitioner av tidsinställda jobb.

7.  På sidan Definitioner av tidsinställda jobb klickar du på Uppdatering för SharePoint Services Search.

8.  Klicka på Inaktivera på sidan Redigera tidsinställt jobb.

## <a name="mim-2016-initial-data-load"></a>Inledande datainläsning för MIM 2016

Det här avsnittet innehåller ett antal steg för att öka prestandan för den ursprungliga data från externa systemet till MIM. Det är viktigt att förstå att ett antal här utförs endast under den inledande ifyllning av systemet. De ska återställas när belastningen. Detta är en engångsåtgärd och är inte en kontinuerlig synkronisering.

>[!NOTE]
Mer information om hur du synkroniserar användare mellan MIM och Active Directory Domain Services (AD DS) finns [hur gör jag synkronisera användare från Active Directory till FIM](http://go.microsoft.com/fwlink/?LinkID=188277) i FIM-dokumentationen.

>[!IMPORTANT]
Se till att du har följt de rekommenderade metoder som beskrivs i avsnittet om SQL-konfiguration i den här handledningen. 

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>Steg 1: Konfigurera SQL Server för inledande datainläsning
Den inledande inläsningen av data kan vara en tidskrävande process. När du planerar att först läsa in stora mängder data kan minska du den tid det tar att fylla i databasen genom att tillfälligt inaktivera fulltextsökning och aktivera det igen efter exporten på MIM 2016-hanteringsagenten (FIM MA) har slutförts.

Stänga av fulltextsökning tillfälligt:

1.  Starta SQL Server Management Studio.

2.  Välj Ny fråga.

3.  Kör följande SQL-uttryck:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

>[!IMPORTANT]
Om du inte implementerar dessa metoder kan det leda till hög diskutrymmesanvändning, som eventuellt kan leda till att diskutrymmet tar slut. Du hittar mer information om det här avsnittet i [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Översikt över återställningsmodell). [Guide för säkerhetskopiering och återställning av FIM](http://go.microsoft.com/fwlink/?LinkID=165864) innehåller ytterligare information.

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>Steg 2: Använd den minsta nödvändiga MIM-konfigurationen under inläsningen

Under den inledande inläsningen bör du bara använda den lägsta konfiguration som krävs av FIM-konfigurationen för hanteringsprincipreglerna (MPR) och uppsättningsdefinitionerna. När inläsningen är klar kan du skapa ytterligare uppsättningar som krävs för distributionen. Använd inställningen Kör vid principuppdatering på åtgärdsarbetsflöden för att verkställa dessa principer retroaktivt på inlästa data.

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>Steg 3: Konfigurera och fylla i FIM-tjänsten med externa identitetsdata

Nu ska du följa beskrivs procedurerna hur gör jag synkronisera användare från Active Directory Domain Services till FIM-guide för att konfigurera och synkronisera datorn med användare från Active Directory. Om du behöver synkronisera information om procedurer för den här processen beskrivs i den [hur gör jag synkronisera grupper från Active Directory Domain Services till FIM](https://technet.microsoft.com/library/ff686936(v=ws.10).aspx) guide.

#### <a name="synchronization-and-export-sequences"></a>Synkroniserings- och exportsekvenser

Optimera prestanda genom att köra en export efter en synkroniseringskörning som ger ett stort antal väntande exportåtgärder i en anslutarplats. Kör sedan en bekräftande importkörning på hanteringsagenten som är kopplad till berörd anslutarplats. Om du till exempel behöver köra körningsprofiler för synkronisering på flera hanteringsagenter som en del av en inledande databelastning bör du köra en export följt av en deltaimport efter varje enskild synkroniseringskörning.
Utför följande steg för varje källhanteringsagent som är en del av initieringscykeln:

1.  Fullständig import på en källhanteringsagent.

2.  Fullständig synkronisering på källhanteringsagenten.

3.  Export på alla berörda målhanteringsagenter med stegvisa exportåtgärder.

4.  Deltaimport på alla berörda målhanteringsagenter med stegvisa exportåtgärder.

### <a name="step-4-apply-your-full-mim-configuration"></a>Steg 4: Verkställa den fullständiga MIM-konfigurationen

När den första datainläsningen har slutförts bör du verkställa den fullständiga MIM-konfigurationen för distributionen.

Beroende på dina scenarier kan detta omfatta att skapa ytterligare uppsättningar, MPR och arbetsflöden. Vid eventuella principer som du måste verkställa retroaktivt på alla befintliga objekt i systemet använder du inställningen Kör vid principuppdatering på åtgärdsarbetsflöden för att verkställa dessa principer retroaktivt på inlästa data.

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>Steg 5: Konfigurera om SQL till tidigare inställningar

Kom ihåg att ändra SQL-inställningen till dess normala inställningar. Du måste bland annat:

-   Aktivera fulltextsökning

-   Uppdatera säkerhetskopieringsprincipen enligt organisationsprincipen

När du har slutfört den inledande datainläsningen måste du aktivera fulltextsökning igen. Kör följande SQL-uttryck för att aktivera fulltextsökning igen:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Om du vill växla till enkelt återställningsläge ser du till att konfigurera om säkerhetskopieringsschemat i enlighet med organisationens säkerhetskopieringsprincip. Ytterligare information om säkerhetskopieringsscheman i FIM finns i [Guide för säkerhetskopiering och återställning av FIM](http://go.microsoft.com/fwlink/?LinkID=165864).

## <a name="configuration-migration"></a>Konfigurationsmigrering


### <a name="avoid-changing-display-names"></a>Undvik att ändra visningsnamn

För många objekttyper, till exempel MPR, använder skriptet syncproduction.ps1 visningsnamnet som det enda fästpunktsattributet mellan två system. Det innebär att om ett befintligt MPR-visningsnamn ändras raderas befintlig MPR och en ny MPR skapas. Det beror på att migreringsprocessen inte kan sammankoppla hanteringsprincipregler vars kopplingsvillkor har ändrats. Du kan undvika detta problem genom att koppla ett anpassat attribut till alla konfigurationsobjekttyper och använda det attributet som kopplingsvillkor. På så sätt kan du ändra visningsnamn utan att påverka migreringsprocessen.

### <a name="avoid-changing-the-content-of-intermediate-files"></a>Undvik att ändra innehållet i mellanliggande filer

Även om filformatet och Application Programming Interface (API) för objekten på låg nivå är offentliga och ändringar stöds av utvecklare rekommenderar vi inte att du ändrar innehållet i mellanliggande format under migreringen. Det kan dock vara nödvändigt att ta bort hela importobjekt från changes.xml eller utföra åtgärder för att söka och ersätta i pilot.xml för att byta ut versionsnummer eller pilot-DNS-information (Domain Name System) mot produktions-DNS-information.

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>Se till att versionsnumret i pilot.xml är rätt vid migrering mellan versioner

Även om migreringar mellan versionsnummer varken rekommenderas eller stöds kan du ofta göra detta genom att byta ut pilotversionsnumret mot produktionsversionsnumret i pilot.xml. Mer specifikt kräver objekten WorkflowDefinition och

ActivityInformationConfiguration versionsnumret för att exakt kunna hänvisa till arbetsflödesaktiviteter i produktionsmiljön. Om versionsnumret inte byts ut leder det till att cmdlet:en Compare-FIMConfig identifierar skillnader mellan XOML-attributen (Extensible Object Markup Language) i WorkflowDefinitions och migrerar pilotens versionsnummer. FIM-tjänsten för produktion kan misslyckas med att starta arbetsflödesaktiviteter med fel versionsnummer.

### <a name="avoid-cyclic-references"></a>Undvik cykliska referenser

Normal rekommenderas inte cykliska referenser i en MIM-konfiguration. Cykler uppstår dock ibland när uppsättning A hänvisar till uppsättning B och uppsättning B också hänvisar till uppsättning A. För att undvika problem med cykliska referenser bör du ändra definitionen i uppsättning A eller uppsättning B så att de båda inte hänvisar till varandra. Starta sedan om migreringsprocessen. Om du har cykliska referenser och cmdlet:en Compare-FIMConfig ger ett fel som resultat måste du bryta cykeln manuellt. Eftersom cmdlet:en Compare-FIMConfig skapar en lista med ändringar i prioritetsordning får det inte finnas några cykler bland referenserna för konfigurationsobjekt.

## <a name="security"></a>Säkerhet

### <a name="mim-ma-account"></a>MIM MA-konto

MIM MA-kontot betraktas inte som ett tjänstkonto och ska vara ett vanligt användarkonto. Kontona måste kunna logga in lokalt för att FIM-synkroniseringstjänstens tjänstkonto ska kunna personifiera den.

Aktivera MIM MA-kontot för lokal inloggning

1.  Klicka på Start, klicka på Administrationsverktyg och klicka sedan på Lokal säkerhetsprincip.

2.  Öppna noden Lokala principer och klicka på Tilldelning av användarrättigheter.

3.  I principen Tillåt lokal inloggning ser du till att FIM MA-kontot anges uttryckligen, annars lägger du till det i en av de grupper som redan har åtkomst.

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>FIM-synkroniseringstjänst och FIM-tjänstekonton

Tjänstekontona bör vara begränsade för att kunna konfigurera servrarna som kör MIM-serverkomponenterna på ett säkert sätt. Använd föregående metod för att aktivera MIM MA-kontot och ange följande begränsningar för FIM-synkroniseringstjänsten och FIM-tjänstekontona:

-   Neka inloggning som batchjobb

-   Neka lokal inloggning

-   Neka tillgång till den här datorn från nätverket

Tjänstekontona bör inte vara medlem i den lokala administratörsgruppen.

FIM-synkroniseringstjänstens tjänstekonto bör inte vara medlem i de säkerhetsgrupper som används för att styra åtkomst till FIM-synkroniseringstjänsten (grupper som börjar med FIMSync, till exempel FIMSyncAdmins o.s.v.).

>[!IMPORTANT]
 Om du väljer alternativen för att använda samma konto för båda tjänstekontona och inte skiljer FIM-tjänsten och FIM-synkroniseringstjänsten åt kan du inte konfigurera Neka tillgång till den här datorn från nätverket på mms-synkroniseringstjänstens server. Om åtkomst nekas förhindrar det att FIM-tjänsten kontaktar FIM-synkroniseringstjänsten för att ändra konfigurationen och hantera lösenord.

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>Lösenordsåterställning som distribueras på kioskliknande datorer bör ange lokal säkerhet för att rensa virtuell minnesväxlingsfil

Vid distribution av FIM-lösenordsåterställning på en dator som är avsedd att vara en kiosk rekommenderar vi att den lokala säkerhetsprincipinställningen Avstängning: Rensa den virtuella växlingsfilen aktiveras för att säkerställa att känslig information från processminnet inte är tillgänglig för obehöriga användare.

### <a name="implementing-ssl-for-the-fim-portal"></a>Implementera SSL för FIM-portalen

Vi rekommenderar starkt att du använder SSL (secure sockets layer) på FIM-portalservern för att skydda trafiken mellan klienterna och servern.

Implementera SSL:

1.  Öppna IIS-hanteraren på MIM-portalservern.

2.  Klicka på namnet på den lokala datorn.

3.  Klicka på Servercertifikat.

4.  Klicka på Skapa certifikatförfrågan.

5.  I textrutan Eget namn anger du serverns namn.

6.  Klicka på Nästa och sedan på Nästa.

7.  Spara filen var som helst. Du behöver ha åtkomst till denna plats under följande steg.

8.  Bläddra till https://servername/certsrv. Ersätt servernamn med namnet på den server som utfärdar certifikat.

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

## <a name="performance"></a>Prestanda

För optimal prestandakonfiguration:

-   Använd de rekommenderade metoder för SQL-konfiguration som beskrivs i avsnittet om konfiguration av SQL i detta dokument.

-   Inaktivera SharePoint-indexering på MIM-portalwebbplatsen. Mer information finns i avsnittet Inaktivera SharePoint-indexering i det här dokumentet.

## <a name="feature-specific-best-practices"></a>Funktionen specifika Metodtips 


### <a name="request-management"></a>Hantering av begäran

MIM 2016 rensar som standard förfallna systemobjekt, vilket omfattar slutförda begäranden med kopplade godkännanden, godkännandesvar och arbetsflödesinstanser under en 30-dagarsintervall. Om organisationen behöver ha en längre begäranshistorik bör du exportera begäranden från MIM och lagra dem i en extra databas för att spara dem efter perioden på 30 dagar. Även om det går att konfigurera raderingsperioden på 30 dagar för begäranden kan en förlängning av denna tid ha en negativ effekt på prestanda till följd av de ytterligare objekten i systemet.

### <a name="management-policy-rules"></a>Hantering av principregler

#### <a name="use-the-appropriate-mpr-type"></a>Använd lämplig MPR-typ

MIM erbjuder två typer av MPR, Begäran och Uppsättningsövergång:

-  MPR för begäran (RMPR)

  - Används för att definiera åtkomstkontrollprincipen (autentisering, auktorisering och åtgärd) för åtgärderna Skapa, Läsa, Uppdatera eller Ta bort (Create, Read, Update eller Delete, CRUD) mot resurser.
  - Tillämpas när utfärdas en CRUD-åtgärd mot en målresurs i MIM.
  - Omfattar de matchande kriterier som definieras i regeln, d.v.s. de CRUD-begäranden som regeln gäller för.

- MPR för uppsättningsövergång (TMPR)
  - Används för att definiera principer oavsett hur objektet har fått aktuellt tillstånd som representeras av övergångsuppsättningen. Använd TMPR för modellera rättighetsprinciper.
  - Tillämpas när en resurs går in i eller lämnar en associerad uppsättning.
  - Omfattar medlemmarna i uppsättningen.

>[ANMÄRKNING] Mer information finns i [Utforma regler för affärsprinciper](http://go.microsoft.com/fwlink/?LinkID=183691).

#### <a name="only-enable-mprs-as-necessary"></a>Aktivera MPR endast efter behov

Använd principen om lägsta behörighet när konfigurationen verkställs. MPR styra åtkomstprincipen till MIM-distributionen. Aktivera endast de funktioner som används av de flesta användare. Inte alla användare använda MIM för grupphantering, så associerade grupphantering MPR ska inaktiveras. MIM levereras med de flesta icke-administratörsbehörighet inaktiverad som standard.

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>Duplicera inbyggda MPR i stället för att ändra direkt
Om du behöver ändra inbyggda MPR bör du skapa en ny MPR med den konfiguration som krävs och inaktivera inbyggd MPR. Detta säkerställer att eventuella framtida ändringar i inbyggda MPR som införs genom uppgraderingsprocessen inte påverkar systemkonfigurationen negativt.

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>Behörighet för slutanvändare bör använda explicita attributlistor som omfattar användarnas affärsbehov
Om explicita attributlistor används hjälper det till att förhindra att behörighet beviljas av misstag till obehöriga användare när attribut läggs till i objekt. Administratörer bör behöva bevilja åtkomst explicit till nya attribut istället för att försöka ta bort åtkomsten.

Åtkomst till data bör begränsas till användarnas affärsbehov. Gruppmedlemmar bör till exempel inte ha åtkomst till filterattributet för den grupp de är medlemmar i. Filtret kan oavsiktligt avslöja organisationsdata som användaren normalt inte ska ha åtkomst till.

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>MPR bör återspegla gällande behörigheter i systemet
Undvik att bevilja behörighet till attribut som användaren aldrig kan använda. Du bör till exempel inte bevilja behörighet till att ändra grundläggande resursattribut som objectType. Trots MPR nekar systemet till alla försök att ändra en resurstyp efter att den har skapats.

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>Läsbehörighet ska vara separat från behörigheter för att ändra och skapa när du använder explicita attribut i MPR

När attribut anges explicit i MPR ska attributen som krävs för att ändra och skapa normalt skilja sig från de som är tillgängliga för att läsa. Läsa kan till exempel beviljas över systemattribut som Creator eller objectId, medan Skapa eller Ändra inte kan anges för systemattribut.

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>Behörighet för att skapa ska vara separat från behörigheter för att ändra när du använder explicita attribut i regler

Åtgärden för att skapa kräver att användaren väljer objectType som en del av åtgärden. Detta är ett grundläggande systemattribut som inte kan ändras efter en åtgärd för att skapa.

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>Använd en begärans-MPR för alla attribut med samma åtkomstkrav

För attribut med samma åtkomstkrav som inte förväntas att ändras kan du förbättra effektiviteten genom att kombinera dem i en enda begärans-MPR.

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>Undvik att ge obegränsad åtkomst till valda huvudgrupper

I MIM, har behörigheter definierats som en positiv kontrollen. Eftersom MIM inte stöder neka behörigheter, leder ger obegränsad åtkomst till en resurs till att tillhandahålla eventuella undantag i området behörigheter. Rekommenderad metod är att endast bevilja de behörigheter som behövs.

#### <a name="use-tmprs-to-define-custom-entitlements"></a>Använd TMPR för att definiera anpassade rättigheter

Använd MPR för uppsättningsövergång (TMPR) i stället för RMPR för att definiera anpassade rättigheter. TMPR utgör en tillståndsbaserad modell för att tilldela eller ta bort rättigheter baserat på medlemskap i definierade övergångsuppsättningar, eller roller, och medföljande arbetsflödesaktiviteter. TMPR ska alltid definieras i par, ett för resurser som går in och ett för resurser som går ut. Dessutom bör varje övergångs-MPR innehålla separata arbetsflöden för etablerings- och borttagningsaktiviteter.

>[!NOTE]
Eventuella borttagningsarbetsflöden bör säkerställa att attributet Kör vid principuppdatering är inställt på sant.

#### <a name="enable-the-set-transition-in-mpr-last"></a>Aktivera uppsättningsövergången i MPR sist

När du skapar ett TMPR-par aktiverar du Uppsättningsövergång i MPR sist. Den här ordningen säkerställer att ingen resurs finns kvar med rättigheten om den läggs till i och tas bort från uppsättningen när in-MPR är aktiverad men innan ut-MPR aktiveras.

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>Arbetsflöden i TMPR bör kontrollera målresursens tillstånd först

Etableringsarbetsflöden bör först kontrollera för att fastställa om målresursen redan har etablerats i enlighet med rättigheten. Om den har det ska den inte göra någonting.

Borttagningsarbetsflöden bör först kontrollera för att fastställa om målresursen har etablerats. Om den har det ska målresursen tas bort. Annars ska den inte göra någonting.

#### <a name="select-run-on-policy-update-for-tmprs"></a>Välj Kör vid principuppdatering för TMPR

Detta säkerställer att rätt etableringsbeteende verkställs när principuppdateringar implementeras och använder flaggan Kör vid principuppdatering på åtgärdsarbetsflöden som är kopplade till TMPR. Detta säkerställer att ändringar i principdefinitionerna verkställer åtgärdsarbetsflöden på nya medlemmar i övergångsuppsättningen.

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>Undvik att koppla samma rättighet till två olika övergångsuppsättningar

Om samma rättighet kopplas till två olika övergångsuppsättningar kan det leda till att återkallelse och ny tilldelning av rättigheter sker i onödan om resursen flyttar från en uppsättning till en annan. Den rekommenderade metoden är att se till att en uppsättning innehåller alla resurser som kräver den kopplade rättigheten. Detta säkerställer ett förhållande en mot en mellan övergångsuppsättningen och rättigheten som beviljar arbetsflödet.

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>Använd en lämplig sekvens med åtgärder när du tar bort rättigheter i systemet

Ordningen för stegen som utförs när rättigheter tas bort i systemet kan leda till två olika driftresultat. Se till att du förstår vilken ordning som gäller för den effekt du vill ha.

Ta bort en rättighet från systemet (och återkalla den från alla medlemmar som har rättigheten för närvarande):

1.  Inaktivera T-In MPR. På så sätt undviker du nya beviljanden.

2.  Ta bort T-Set-filtret eller ändra det så att det är tomt. Det får alla befintliga medlemmar att göra en övergång ut och det verkställer principen för övergång ut, inklusive de konfigurerade avetableringsarbetsflöden som är kopplade till rättigheten.

3.  Inaktivera T-Out MPR.

Ta bort rätt men lämna de aktuella medlemmarna fristående (till exempel att sluta använda MIM för att hantera berättigandet):

1.  Inaktivera T-In MPR. På så sätt undviker du nya beviljanden.

2.  Inaktivera T-Out MPR.

3.  Ta bort T-Set-filtret eller ändra det så att det är tomt. Eftersom uppsättningen inte längre är kopplad till en TMPR verkställs inga avetableringsarbetsflöden.

### <a name="sets"></a>Uppsättningar

När du använder de rekommenderade metoderna för uppsättningar måste du överväga effekten av optimeringen på hanterbarheten och hur enkel framtida administration blir. Lämplig testning med förväntad produktionsskala ska utföras för att identifiera den rätta balansen mellan prestanda och hanterbarhet innan dessa rekommendationer verkställs.

>[!NOTE]
> Alla följande riktlinjer gäller dynamiska uppsättningar och dynamiska grupper.


#### <a name="minimize-the-use-of-dynamic-nesting"></a>Minska användningen av dynamisk kapsling

Det här gäller filtret i en uppsättning som hänvisar till attributet ComputedMember i en annan uppsättning. En vanlig orsak till kapslade uppsättningar är att undvika att duplicera ett medlemskapsvillkor över flera uppsättningar. Även om den här metoden kan ge bättre hanterbarhet för uppsättningarna försämras prestanda. Du kan förbättra prestandan genom att duplicera medlemskapsvillkoren för en kapslad uppsättning i stället för att kapsla själva uppsättningen.

Det kan hända att du påträffar fall då du inte kan undvika kapsling av uppsättningar för att tillfredsställa ett funktionskrav. Det här är de huvudsakliga situationer då du bör kapsla uppsättningar. Kapsling av uppsättningar måste till exempel användas på följande sätt för att definiera uppsättningen för alla grupper utan ägare som är heltidsmedarbetare: `/Group[not(Owner = /Set[ObjectID = ‘X’]/ComputedMember]`, där ”X” är uppsättningens ObjectID för alla heltidsmedarbetare.

#### <a name="minimize-the-use-of-negative-conditions"></a>Minska användningen av negativa villkor

Negativa villkor är de medlemskapsvillkor som använder följande operatorer eller funktioner: `!=`, `not()`, `\<` , `\<=`. Optimera prestanda genom att uttrycka det villkor du vill använda med flera positiva villkor i stället för ett negativt villkor, där det är möjligt.

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>Minska användningen av medlemskapsvillkor baserat på referensattribut med flera värden

Användning av villkor baserat på referensattribut med flera värden ska minimeras, eftersom om det finns stora mängder sådana uppsättningar kan det påverka prestanda för åtgärder på det attribut som används i medlemskapsvillkoret.

### <a name="password-reset"></a>Återställning av lösenord

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>Kioskliknande datorer som används för lösenordsåterställning bör ange lokal säkerhet för att rensa den virtuella minnesväxlingsfilen

När du distribuerar MIM lösenordsåterställning på en arbetsstation som är avsedd att vara en kiosk rekommenderar vi att avstängningen: Rensa virtuellt minne växlingsfilens storlek med lokala säkerhetsprincipen aktiveras för att kontrollera att känslig information från processminnet som inte är tillgänglig för obehöriga användare.

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>Användare bör alltid registrera sig för lösenordsåterställning på en dator som de är inloggade på

När en användare försöker registrera dig för lösenordsåterställning via en webbportal, initierar MIM alltid registrering för den inloggade användaren, oavsett vem som är inloggad på webbplatsen. Användare bör alltid registrera sig för lösenordsåterställning på en dator som de är inloggade på.

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>Ställ inte in registernyckeln AvoidPdcOnWan på true

När du använder MIM 2016-lösenordsåterställning ska du inte ställa in registernyckeln AvoidPdcOnWan på true.

Om den här registernyckeln ställs in på true är det mycket troligt att användaren går igenom lösenordsportarna, får sitt lösenord återställt på den primära domänkontrollanten (PDC) och försöker logga in. På grund av den här registernyckeln utför inte den lokala domänkontrollanten den sekundära valideringen med PDC och därför nekas inloggningsbegäran. Om användaren nekas tillräckligt många gånger kan denne bli utelåst från domänen och måste ringa support.

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>Aktivera inte loggning av lösenord i klartext

Det är möjligt att logga lösenord i klartext när spårning av diagnostiktjänstenivå aktiveras i Windows

Communication Foundation (WCF). Det här alternativet är inaktiverat som standard och vi avråder från att aktivera det i produktionsmiljöer. De här lösenorden visas som klartextelement i ett krypterat SOAP-meddelande (Simple Object Access Protocol) när användarna registrerar sig för lösenordsåterställning. Mer information finns i [Configuring Message Logging](http://go.microsoft.com/fwlink/?LinkID=168572) (Konfigurera meddelandeloggning).

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>Mappa inte ett auktoriseringsarbetsflöde till processen för lösenordsåterställning

Du bör inte koppla ett auktoriseringsarbetsflöde till en åtgärd för lösenordsåterställning. Lösenordsåterställning kräver ett synkront svar, och auktoriseringsarbetsflöden som innehåller aktiviteter som exempelvis godkännandeaktiviteten är asynkrona.

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>Mappa inte flera åtgärdsaktiviteter till lösenordsåterställning

Du bör inte koppla ett arbetsflöde som innehåller fler än en åtgärdsaktivitet till en åtgärd för lösenordsåterställning. Ett exempelscenario skulle vara att bifoga en andra aktivitet för AD DS-lösenordsåterställning till en MPR för lösenordsåterställning. Det här scenariot stöds inte.

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>Kräv omregistrering när aktiviteter läggs till, tas bort eller deras ordning ändras i ett befintligt arbetsflöde

När du lägger till, tar bort eller ändrar ordningen för autentiseringsaktiviteter i ett befintligt arbetsflöde ska du alltid välja alternativet att kräva omregistrering. Användare som försöker autentisera för lösenordsåterställning efter att en aktivitet har lagts till eller tagits bort från ett arbetsflöde, men innan de har registrerats om kan råka ut för oönskade effekter.

### <a name="portal-configuration-and-resource-control-display-configuration"></a>Portalkonfiguration och konfiguration av skärmen för resurskontroll

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>Överväg att lägga till en sekretessfriskrivningsklausul på användarprofilsidan

I MIM kan en del användarprofilinformation som standard visas för andra användare. För att hjälpa användarna bör administratörerna överväga att lägga till anpassad text på användarprofilsidan som följer företagets principer. Mer information om att lägga till en anpassad text på en MIM-portalsida finns i introduktionen till att [konfigurera och anpassa FIM-portalen](http://go.microsoft.com/fwlink/?LinkID=165848).

### <a name="schema"></a>Schema

#### <a name="do-not-delete-person-or-group-resource-types"></a>Ta inte bort resurstyperna Person eller Grupp

Även om resurstyperna Person och Grupp inte är markerade som huvudresurstyper ska själva resurserna eller attributen som har tilldelats till dem inte tas bort. Användargränssnittet (UI) i MIM-portalen kräver att resurstyperna Person och Grupp och deras attribut finns.

#### <a name="do-not-modify-the-core-attributes"></a>Ändra inte huvudattributen

Det finns 13 huvudattribut som tilldelas till alla resurstyper. Du bör inte på något sätt ändra deras relation till någon resurstyp. De 13 huvudattributen är:

-   CreatedTime

-   Creator

-   DeletedTime

-   Description

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

#### <a name="making-regular-expressions-case-insensitive"></a>Göra reguljära uttryck skiftlägesokänsliga

I MIM, kan det vara bra att göra vissa reguljära uttryck skiftlägeskänsligt. Du kan ignorera skiftläge i en grupp genom att använda ?!:. För Typ av anställd kan du till exempel använda

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>Beräkning av medlemsattributet

Medlemsattributet som är exponerat för synkroniseringsmotorn mappas faktiskt till ComputedMembers. Det är en kombination av villkorsbaserade medlemmar och manuellt valda medlemmar. Även om du lägger till alla tre attributen (Filter, ExplicitMembers och ComputedMembers) sker inte den dynamiska beräkningen av medlemsattributet för andra resurstyper än grupp och uppsättning.

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>Inledande och avslutande blanksteg i strängar ignoreras

Du kan ange strängar med inledande och avslutande blanksteg i MIM, men MIM ignoreras dessa utrymmen. Om du skickar en sträng med ett ledande och avslutande blanksteg ignorerar synkroniseringsmotorn och webbtjänster dessa blanksteg.

#### <a name="empty-strings-do-not-equal-null"></a>Tomma strängar är inte lika med null

Tomma strängar är inte lika med null i den här versionen av MIM. Tomma strängindata betraktas som ett giltigt värde. Ej tillgängligt betraktas som ett null-värde.

### <a name="workflow-and-request-processing"></a>Arbetsflöde och bearbetning av begäran

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>Ta inte bort standardarbetsflöden som medföljer MIM 2016

Följande arbetsflöden levereras med MIM och ska inte tas bort:

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

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>Kör inte två eller flera godkännandeaktiviteter parallellt

Du ska inte köra två eller flera godkännandeaktiviteter parallellt. Det kan leda till att begäran fastnar i auktoriseringsfasen. För flera godkännanden kan du antingen inkludera en större lista med godkännare i godkännandet eller sätta de två aktiviteterna i en sekvens efter varandra.

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>Auktoriseringsaktiviteter bör inte ändra MIM-resursdata

Undvik att använda aktiviteter som ändrar MIM-resurserna, t.ex. funktionsutvärderaraktiviteten, som en del av arbetsflödena i auktoriseringsarbetsflöden. Eftersom begäran inte har utförts under auktoriseringspunkten i bearbetningen kan eventuella ändringar som görs i identitetsinformationen verkställas trots att begäran kan komma att avvisas.

### <a name="understanding-fim-service-partitions"></a>Förstå FIM-tjänstpartitioner

Syftet med MIM är att bearbeta förfrågningar som kan startas av olika MIM-klienter, till exempel FIM-synkroniseringstjänsten och självbetjäning komponenterna enligt konfigurerade affärsregler. Varje FIM-tjänstinstans har skapats så att den tillhör en logisk grupp som består av en eller flera FIM-tjänstinstanser, som också kallas FIM-tjänstpartition. Om du endast har en FIM-tjänstinstans distribuerad för att hantera alla begäranden kan det hända att det uppstår fördröjningar i bearbetningen. En del åtgärder kan även överskrida de standardtidsgränsvärden som gäller för självbetjäningsåtgärder. FIM-tjänstpartitioner kan hjälpa dig att lösa detta problem.

Mer information finns i [förstå FIM-tjänsten partitioner](https://social.technet.microsoft.com/wiki/contents/articles/2363.understanding-fim-service-partitions.aspx).

## <a name="next-steps"></a>Nästa steg
- [FIM-säkerhetskopiering och återställning Guide](http://go.microsoft.com/fwlink/?LinkID=165864)
- [Hur gör jag synkronisera användare från Active Directory till FIM](http://go.microsoft.com/fwlink/?LinkID=188277) 
- [Översikt över säkerhetsmodell Recovery](http://go.microsoft.com/fwlink/?LinkID=185370).