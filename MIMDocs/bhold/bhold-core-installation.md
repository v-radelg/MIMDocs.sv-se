---
title: BHOLD Core-installation | Microsoft Docs
description: Core-dokument för BHOLD Suite-installation
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: c4dfb4184292ba1b5da8c4e3e176d53e6a885ed8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042278"
---
# <a name="bhold-core-installation"></a>BHOLD Core-installation

BHOLD Core-modulen innehåller viktiga funktioner i BHOLD Suite i din miljö. BHOLD Core-modulen måste installeras och konfigureras på en server i det lokala nätverket innan du kan installera andra BHOLD Suite-moduler.

## <a name="bhold-core-installation-requirements"></a>Krav för BHOLD Core-installation

BHOLD Core-modulen utgör grunden för Microsoft BHOLD Suite. Du måste installera BHOLD Core-modulen innan du installerar andra Microsoft BHOLD Suite-moduler.

### <a name="bhold-core-hardware-requirements"></a>Maskin varu krav för BHOLD-kärnan

BHOLD Core-modulen utgör grunden för Microsoft BHOLD Suite. Du måste installera BHOLD Core-modulen innan du installerar andra Microsoft BHOLD Suite-moduler.

|          |        |          |
|----------|--------|----------|
|**Komponent** |**Lägst** | **Rekommenderas** |
|Processor | 64-bitars processor | Multicore 64-bitars processor |
| Minne |3 GB | 6 GB eller mer |
|Lagring| 30 GB tillgängligt |Är beroende av distributions storlek |
|Nätverkskort| 100 Mbps-anslutning till SQL-och Forefront Identity Manager (FIM)-Server | 1Gbps anslutning till SQL och FIM-Server|

De här rekommendationerna baseras på vanliga implementeringar och tar inte hänsyn till andra program som körs på servern. Du kan behöva använda mer avancerade komponenter beroende på din specifika miljö.

### <a name="bhold-core-software-requirements"></a>BHOLD Core program varu krav

BHOLD Core-modulen måste installeras på en dator som uppfyller följande krav:

- Servern måste köra Windows Server 2012 (64-bitars), Windows Server 2016 
- Servern måste vara medlem i en Active Directory Domain Services-domän (AD DS). I test miljöer kan servern vara en AD DS-domänkontrollant.
- BHOLD-kärnan måste installeras av en användare som är inloggad med ett konto i samma domän som servern och som tillhör gruppen domän administratörer i domänen och gruppen Administratörer på servern.
- Microsoft Internet Information Services (IIS) med ASP.NET måste vara installerat på servern. IIS måste konfigureras med Windows-autentisering aktive rad. Om BHOLD Core installeras på Windows Server 2012/2016 måste IIS 6-hanterings kompatibilitet vara installerat. Om BHOLD Core installeras på Windows Server 2012 måste skript verktyg för IIS 6 installeras
- .NET 3,5-ramverket måste vara installerat.
  - Eftersom BHOLD Core kräver .NET 3,5, kan den inte installeras på Server Core.
- Silverlight 4 krävs av andra BHOLD-moduler och vi rekommenderar att du installerar det innan du installerar BHOLD Core.
- Microsoft SQL Server 2014 eller Microsoft SQL Server 2016 måste vara installerat på BHOLD Core-servern eller på en annan server i det lokala nätverket. 

Windows-klientdatorer måste köra Microsoft Internet Explorer version 6 eller senare och Microsoft Silverlight version 4 eller senare.

## <a name="before-you-begin"></a>Innan du börjar

Modulen BHOLD Core kräver ett användar konto som används för att autentisera och auktorisera BHOLD Core-tjänsten på servern och andra nätverks enheter. I det här avsnittet beskrivs hur du skapar och konfigurerar det användar kontot och ger ett för installations kalkyl blad som hjälper dig att samla in den information du behöver för att slutföra BHOLD Core-installationen.

>{! VIKTIGT} när du installerar BHOLD Core kan du använda en befintlig SQL Server databas som BHOLD Core-databas, eller så kan du tillåta att installations guiden för BHOLD Core skapar BHOLD Core-databasen åt dig. Om du väljer att använda en befintlig databas måste du se till att inga andra användare kan komma åt databasen när du installerar BHOLD Core. Innan du installerar BHOLD Core kontrollerar du att åtkomst kontrollerna för SQL Server databasen inte tillåter åtkomst av någon annan än den användare som installerar BHOLD Core.

## <a name="required-user-and-group"></a>Nödvändig användare och grupp

BHOLD Core-modulen måste kunna logga in på domänen med ett användar konto som är dedikerat för detta ändamål och som är medlem i två specifika säkerhets grupper, inklusive en som har skapats specifikt för BHOLD Core-modulen. Medlemskap i gruppen domän administratörer krävs för att kunna utföra den här proceduren.
**Skapa och konfigurera BHOLD Core-användare och säkerhets grupp**

1.  Klicka på **Start**på en domänkontrollant, peka på **administrations verktyg**och klicka sedan på **Active Directory användare och datorer**.

2.  I konsol trädet expanderar du den domän där kontot ska skapas, högerklickar på **användare**, pekar på **nytt**och klickar sedan på **grupp**.

3.  I dialog rutan **nytt objekt – grupp** , i **grupp namn**, skriver du namnet på gruppen (BHOLD default: BHOLDApplicationGroup) och klickar sedan på **OK**.

4.  Högerklicka på **Användare**, peka på **Nya**, och klicka sedan på **Användare**.

5.  I **fullständigt namn**skriver du ett namn som hjälper dig att identifiera kontot, till exempel BHOLD Core Service-konto.

6.  I **användarens inloggnings namn**skriver du användar namnet för BHOLD Core-TJÄNSTKONTOT (BHOLD standard: b1user) och klickar sedan på **Nästa**.

7.  I **lösen ord** och **Bekräfta lösen ord**skriver du lösen ordet för tjänst kontot.

8.  Rensa **användaren måste byta lösen ord vid nästa inloggning**. Välj **användare kan inte ändra lösen ord** och **lösen ord upphör aldrig att gälla**, klicka på **Nästa**och sedan på **Slutför**.

9.  I fönstret konsol resultat högerklickar du på användar kontot och klickar sedan på **Lägg till i en grupp**.

10. I dialog rutan **Välj grupper** anger du visnings namnet för den grupp som du skapade tidigare, skriver ett semikolon (;) och skriver sedan IIS_IUSRS.

11. Klicka på **kontrol lera namn**och sedan på **OK**.  

Följande procedur måste utföras på den dator där BHOLD Core-modulen ska installeras. Du måste vara inloggad som medlem i gruppen domän administratörer för att kunna utföra den här proceduren.

## <a name="bhold-core-installation-worksheet"></a>Kalkyl blad för BHOLD Core-installation

Innan du börjar installera BHOLD Core-modulen måste du vara beredd på att tillhandahålla den information som krävs för att slutföra installationen av installations guiden för BHOLD Core. I följande kalkyl blad kan du registrera informationen så att du är redo att tillhandahålla den när den behövs. Varje avsnitt motsvarar en sida i installations guiden för BHOLD Core.

### <a name="account-settings"></a>Kontoinställningar

| **Konfigurationsobjektet**                                    | **Beskrivning**                                                                                                                                                                                                                                                                                             | **Värde**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovidern på domän/dator** | När du väljer det här anger du att Active Directory Domain Services säkerhet ska styra åtkomsten till BHOLD-kärnan.                                                                                                                                                                                                  | Markera kryss rutan. **Viktigt:** Installationen Miss kommer om den här kryss rutan inte är markerad.                                                                 |
| **Domän**                                  | Anger den domän som innehåller BHOLD-servern, tjänst kontot och program gruppen. **Viktigt:** Ange domän namnet med hjälp av NetBIOS-namnet (Short), inte det fullständigt kvalificerade domän namnet (FQDN). Om FQDN för domänen till exempel är fabrikam.com anger du domän namnet som CONTOSO. | Skriv domän namnet här:                                                                                                                                        |
| **Program grupp**                       | Anger namnet på den säkerhets grupp som du skapade tidigare i den [begärda användaren och gruppen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Skriv grupp namnet här:                                                                                                                                         |
| **Tjänst användare**                            | Anger inloggnings namnet för det tjänst användar konto som du skapade tidigare i den [begärda användaren och gruppen](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Skriv användar kontots namn här:                                                                                                                                  |
| **Lösenord**                                | Anger lösen ordet för BHOLD Core Service-användarkontot.                                                                                                                                                                                                                                              | Skriv lösen ordet här: **viktigt:** se till att behålla det här lösen ordet på en dold, säker plats.                                                                |
| **Webbplats-IP/port**                         | Anger IP-adressen och port numret för den webbplats som ska skapas på intranät servern. Ändra standardvärdet (\*) endast om du inte kommer att använda samma IP-adress som standard webbplatsen. Ändra port numret till endast en tillgänglig port om standard porten (5151) redan används.             | Om en IP-adress som inte är standard används av standard webbplatsen skriver du här: om standard port numret redan används skriver du BHOLD webbplats port nummer här: |

### <a name="database-settings"></a>Databas inställningar

| **Konfigurationsobjektet**                                       | **Beskrivning**                                                                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd integrerad säkerhet**                    | Anger att Windows-autentisering används för att komma åt databasen.                                                                                                                                                                                                     | Markera kryss rutan om Windows-autentisering används för att ansluta till SQL Server. Avmarkera kryss rutan om SQL Server autentisering används. Databasen måste ha skapats innan du kör BHOLD Core-installationen om SQL Server autentisering används. **Obs:** Om Windows-autentisering används måste du vara inloggad med ett konto som har rollen sysadmin-server på databas servern. |
| **Databas användare** och **databas lösen ord** | Anger användar namn och lösen ord för en användare med Server rollen sysadmin på databas servern. Dessa värden anges bara när SQL Server autentisering används.                                                                                               | Skriv SQL Server användar namn här: Skriv SQL Server användar lösen ord här: **Obs!** se till att behålla det här lösen ordet på en dold, säker plats.                                                                                                                                                                                                                                                  |
| **Databas server** och **databas namn**   | Anger NetBIOS-namnet på databas servern och namnet på databasen (standard: B1) som BHOLD Core-installationen kommer att skapa. Om du inte använder standard instansen av databas servern anger du databas Server instansen i formatet *\<server\>* \\ *\<instans\>* . | Skriv Server namnet (eller server och instans) här: Skriv databas namnet här:                                                                                                                                                                                                                                                                                                                   |
| **Gör begränsningar för databas användaren**    | Föråldrad.                                                                                                                                                                                                                                                                 | Ändra inte standardvärdet                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD Core-installation

Om du vill installera BHOLD Core-modulen loggar du in som medlem i gruppen domän administratörer, laddar ned följande fil och kör den som administratör på den server som du vill installera BHOLD Core-modulen på: 

- BholdCore *\<Version\>* \_release. msi

Ersätt *\<version\>* med versions numret för den BHOLD Core-version som du installerar.

Om du vill köra program filen som administratör högerklickar du på filen och klickar sedan på **Kör som administratör**.

### <a name="postinstallation-settings"></a>Postinstallation-inställningar

När BHOLD Core-installationen är klar måste du konfigurera Windows-brandväggen och ändra avancerade inställningar i BHOLD Core-programpoolen i Internet Information Services för att slutföra BHOLD Core-konfigurationen. Om det behövs bör du även ändra systemattributen för BHOLD så att de uppfyller dina krav.

#### <a name="configuring-windows-firewall"></a>Konfigurera Windows-brandväggen

Om användarna kommer åt BHOLD med hjälp av webbläsare på fjärrdatorer måste du konfigurera Windows-brandväggen på BHOLD Core-servern för att tillåta inkommande anslutningar till den webbplats port som du angav när du installerade BHOLD Core.

Om du vill utföra den här proceduren måste du vara medlem i gruppen Administratörer på den lokala datorn.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Tillåta inkommande anslutningar till BHOLD-webbplatsen

1.  Klicka på **Start**, peka på **administrations verktyg**, högerklicka på **Windows-brandvägg med avancerade inställningar**och klicka sedan på **Kör som administratör**.

2.  Klicka på **regler för inkommande**trafik i den vänstra rutan och klicka sedan på **ny regel**i den högra rutan.

3.  I guiden ny inkommande regel klickar du på **port**och klickar sedan på **Nästa**.

4.  Kontrol lera att **TCP** är markerat, i **specifika lokala portar**, ange standard port numret för BHOLD (5151) eller port numret som du angav när du installerade BHOLD Core och klicka sedan på **Nästa**.

5.  Kontrol lera att **Tillåt anslutningen** är markerad och klicka sedan på **Nästa**.

6.  På sidan **profil** avmarkerar du kryss rutor för platser där du inte vill ha åtkomst till BHOLD-webbplatsen och klickar sedan på **Nästa**.

7.  På sidan **namn** anger du ett namn för regeln (till exempel Tillåt inkommande anslutningar till BHOLD Core) och klickar sedan på **Slutför**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Aktivera 32-bitars program för BHOLD Core-programpoolen

För att IIS ska fungera korrekt med modulen BHOLD Core måste du konfigurera BHOLD Core-programpoolen så att den stöder 32-bitars program. Du måste vara inloggad med det konto som används för att installera BHOLD Core-modulen för att utföra den här proceduren.

**Så här aktiverar du stöd för 32-bitars program för BHOLD Core-programpoolen**

1.  Öppna Internet Information Services Manager genom att klicka på **Start**, peka på **Administratörs verktyg**och klicka på **Internet Information Services (IIS) Manager**.

2.  I konsol trädet expanderar du Server namnet och klickar sedan på **programpooler**.

3.  I listan **programpooler** högerklickar du på **CoreAppPool**och klickar sedan på **Avancerade inställningar**.

4.  I dialog rutan **Avancerade inställningar** i listan **Aktivera 32-bitars program** väljer du **Sant**och klickar sedan på **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Etablera tjänstens huvud namn för BHOLD-webbplatsen

Om nätverks namnet som används för att kontakta BHOLD-webbplatsen inte är samma som serverns värdnamn, måste du upprätta ett SPN (Service Principal Name) för HTTP. Om du till exempel använder en CNAME-resurspost i DNS för att ange ett alias för servern, eller om du använder Utjämning av nätverks belastning, måste du registrera dessa ytterligare nätverks adresser i Active Directory. Om du inte gör det kan Internet Explorer inte använda Kerberos-protokollet när du kontaktar BHOLD-webbplatsen.

> [!IMPORTANT]
> Om modulen BHOLD Core är installerad på samma dator som FIM-portalen måste du skapa DNS-resursposter (CNAME eller A) med olika värdnamn för servrarna som kör BHOLD Core och servern som kör FIM-portalen. Det går bara att upprätta ett tjänst huvud namn för ett visst par av typen tjänst-typ/Server-alias, och därför kräver BHOLD Core och FIM-portalen separata SPN-namn eftersom de ofta körs under olika konton. Setspn-kommandot rapporterar ett fel om ett SPN redan har upprättats under ett annat konto.

Det krävs minst medlemskap i **Domänadministratörer**, eller motsvarande, för att kunna slutföra den här proceduren.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Så här upprättar du SPN för webbplatsen BHOLD

1.  Klicka på **Start**på Active Directory Domain Services domänkontrollant, klicka på **alla program**, klicka på **tillbehör**, högerklicka på **kommando tolken**och klicka sedan på **Kör som administratör**.

2.  Skriv följande kommando i kommando tolken och tryck sedan på RETUR: setspn – S HTTP/ *\<networkalias\> \<domän\>* \\ *\<AccountName\>* där:

    -   *\<networkalias\>* är den adress som klienter använder för att kontakta BHOLD-webbplatsen

    -   *\<domän\>* \\ *\<AccountName\>* är domänen och användar namnet för BHOLD Core-tjänstkontot som du skapade när du installerade BHOLD Core.

3.  Upprepa föregående steg för alla andra namn som klienter använder för att kontakta BHOLD-webbplatsen, till exempel CNAME-alias, namn som innehåller ett fullständigt kvalificerat domän namn eller namn som innehåller ett NetBIOS-domännamn (Short).

#### <a name="setting-bhold-system-attributes"></a>Ange systemattribut för BHOLD

För att verifiera att installationen av BHOLD Core-modulen lyckades, öppnar du BHOLD Core-portalen och visar systemattributen. För att se till att modulen BHOLD Core fungerar korrekt i din miljö kan du ändra följande BHOLD-systemattribut efter behov:

| **Basattributet**                | **Beskrivning**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ingen historik**                | Ange till Y om webbplatsen för BHOLD körs på en klustrad webb tjänst för att säkerställa att nyligen visade objekt fungerar korrekt. Ange till N om webbplatsen för BHOLD körs på en fristående IIS-server.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Ange till Y för att säkerställa att organisationsenheter (orgunits) bara kan flyttas till orgunits med samma organisations typ som den överordnade orgunit. Detta förhindrar till exempel att en Project-orgunit flyttas till en avdelnings-orgunit. Ange till N om du vill tillåta att en orgunit placeras i en orgunit av en annan typ. |
| **Dagar mellan ABA-körning**     | Ange ett tvåsiffrigt heltal för att ange intervallet (i dagar) mellan två ABA-körningar (Attribute-based Authorization). Om du till exempel vill ange att ABA-körningar ska avgränsas med två dagar skriver du 02.                                                                                                                     |
| **Start timme för ABA-körning**    | Ange ett tvåsiffrigt heltal för att ange den timme på dagen då en attribut-baserad auktoriseringsfel ska utföras. Om du till exempel vill ange att ABA-körningen ska ske kl 11:00 (23:00), skriv 23.                                                                                                             |
| **Systemets kardinalitet**       | Ange till N om du inte vill ha en kontroll av systemets kardinalitet i BHOLD. Standardvärdet är Y.                                                                                                                                                                                                                             |
| **Logging**                  | Ange till N om du inte vill att ändringar ska loggas. Standardvärdet är Y.                                                                                                                                                                                                                                            |
| **SystemQueue-bearbetning**   | Ange till N om du inte vill att system kön ska behandlas. Ändra inte det här värdet om inget annat har inriktats av produkt supporten.                                                                                                                                                                                           |

Du måste vara inloggad som medlem i gruppen domän administratörer för att kunna utföra den här proceduren.

**Ange ett systemattribut för BHOLD**

1.  Klicka på **Start**, klicka på **alla program**och klicka sedan på **Internet Explorer**.

2.  I rutan adress skriver du, där *\<server\>* är namnet på BHOLD-webbplats servern och *\<porten\>* är port numret som är kopplat till webbplatsen.

3.  Klicka på **Start**, klicka på **värden**och klicka sedan på **ändra**.

4.  Leta upp namnet på attributet som du vill ändra, skriv det nya värdet i rutan bredvid attributnamnet och klicka sedan på **OK**.

## <a name="next-steps"></a>Nästa steg

När du har installerat BHOLD Core och verifierat att installationen lyckades, kan du installera ytterligare moduler. I det här läget är BHOLD-databasen i huvudsak tom, som bara innehåller ett användar konto, rot kontot och en organisationsenhet (orgunit), rot-orgunit. Om du vill lägga till fler användare i BHOLD-databasen kan du antingen installera Access Management Connector-modulen eller modulen BHOLD modell generator, beroende på dina behov. Du kan använda Access Management Connector-modulen för att importera användar data från FIM-synkroniseringstjänsten eller så kan du använda BHOLD modell generator för att importera användar data från en uppsättning strukturerade filer. Mer information om hur du använder Access Management Connector-modulen finns i [Test Lab-guide: BHOLD Access Management Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Mer information om hur du använder modulen BHOLD modell Generator finns i:

- [Koncept guide för Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite-TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
