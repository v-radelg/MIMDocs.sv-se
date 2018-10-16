---
title: BHOLD-core Installation | Microsoft Docs
description: BHOLD suite installationen core-dokument
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 4499c2846655ff75462794d684cae44f03134f1c
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333538"
---
# <a name="bhold-core-installation"></a>BHOLD-Core-installation

BHOLD-kärna-modulen innehåller viktiga funktioner i BHOLD-programsvit i din miljö. BHOLD-Core-modul måste installeras och konfigureras på en server i det lokala nätverket innan du kan installera andra BHOLD-programsvit moduler.

## <a name="bhold-core-installation-requirements"></a>Installationskrav för BHOLD-kärna

BHOLD-kärnmodul utgör grunden för BHOLD-programsvit för Microsoft. Innan du installerar andra Microsoft BHOLD-programsvit moduler måste du installera BHOLD-Core-modul.

### <a name="bhold-core-hardware-requirements"></a>Maskinvarukrav för BHOLD-kärna

BHOLD-kärnmodul utgör grunden för BHOLD-programsvit för Microsoft. Innan du installerar andra Microsoft BHOLD-programsvit moduler måste du installera BHOLD-Core-modul.

|          |        |          |
|----------|--------|----------|
|**Komponent** |**Minst** | **Rekommenderas** |
|Processor | 64-bitars processor | Flerkärniga 64-bitars processor |
| Minne |3 GB | 6 GB eller mer |
|Lagring| 30 GB som är tillgängliga |Beror på distributionsstorlek |
|Nätverkskort| 100 Mbit/s-anslutning till SQL-och Forefront Identity Manager (FIM) | 1 Gbit/s-anslutning till SQL- och FIM-servern|

De här rekommendationerna är baserade på vanliga implementeringar och tar inte hänsyn till andra program som körs på servern. Du kan behöva använda högre prestanda komponenter beroende på din miljö.

### <a name="bhold-core-software-requirements"></a>Programvarukrav för BHOLD-kärna

BHOLD-Core-modul måste installeras på en dator som uppfyller följande krav:

- Servern måste köra Windows Server 2012 (64-bitars), Windows Server 2016 
- Servern måste vara medlem i en Active Directory Domain Services (AD DS)-domän. I testmiljöer, kan servern vara en AD DS-domänkontrollant.
- BHOLD-kärna måste installeras av en användare som loggat in med ett konto i samma domän som servern och som tillhör gruppen Domänadministratörer i domänen och i gruppen Administratörer på servern.
- Microsoft Internet Information Services (IIS) med ASP.NET måste installeras på servern. IIS måste konfigureras med Windows-autentisering aktiverad. Om BHOLD Core installeras på Windows Server 2012/2016, måste IIS 6-Kompatibilitetshantering installeras. Om BHOLD Core installeras på Windows Server 2012, måste IIS 6-skriptverktyg installeras
- .NET 3.5 framework måste installeras.
  - Eftersom BHOLD-kärna kräver .NET 3.5, kan inte installeras på Server Core.
- Silverlight 4 krävs av andra BHOLD-moduler och så vi rekommenderar att du installerar den innan du installerar BHOLD-kärna.
- Microsoft SQL Server 2014 eller Microsoft SQL Server 2016 måste installeras på BHOLD-Core-servern eller på en annan server i det lokala nätverket. 

Windows-klientdatorer måste köra Microsoft Internet Explorer version 6 eller senare och Microsoft Silverlight version 4 eller senare.

## <a name="before-you-begin"></a>Innan du börjar

BHOLD-Core-modul kräver ett användarkonto som används för att autentisera och auktorisera BHOLD Core-tjänst på servern och andra nätverksentiteter. Det här avsnittet beskriver hur du skapar och konfigurerar det aktuella användarkontot och ger ett preinstallation kalkylblad som hjälper dig att samla in den information du behöver att slutföra installationen för BHOLD-kärna.

>{! VIKTIG} vid installation av BHOLD-kärna, du kan använda en befintlig SQL Server-databas som BHOLD-kärna-databas eller du kan tillåta BHOLD Core installationsguiden ska skapa BHOLD-kärna-databasen. Om du väljer att använda en befintlig databas måste du kontrollera att inga andra användare har åtkomst till databasen under installation av BHOLD-kärna. Innan du installerar BHOLD-kärna, kontrollerar du att åtkomstkontroller på SQL Server-databasen inte tillåter åtkomst av någon annan än den användare som installerar BHOLD-kärna.

## <a name="required-user-and-group"></a>Nödvändiga användare och grupper

BHOLD-Core-modul måste kunna logga in på domänen med ett användarkonto som är dedikerad för detta ändamål och som är medlem i två specifika säkerhetsgrupper, inklusive ett som har skapats specifikt för BHOLD-Core-modul. Medlemskap i gruppen Domänadministratörer krävs för att utföra den här proceduren.
**Du skapar och konfigurerar gruppen för BHOLD-kärna användare och säkerhetsgrupper**

1.  Klicka på en domänkontrollant, **starta**, peka på **Administrationsverktyg**, och klicka sedan på **Active Directory-användare och datorer**.

2.  I konsolträdet expanderar du den domän där kontot är att skapa, högerklicka på **användare**, peka på **New**, och klicka sedan på **grupp**.

3.  I den **nytt objekt – grupp** i dialogrutan **gruppnamn**, skriver du namnet på gruppen (BHOLD standard: BHOLDApplicationGroup), och klicka sedan på **OK**.

4.  Högerklicka på **användare**, peka på **New**, och klicka sedan på **användaren**.

5.  I **fullständigt namn**, Skriv ett namn som hjälper dig identifiera kontot, till exempel BHOLD Core-tjänstkontot.

6.  I **användarens inloggningsnamn**, skriver du användarnamnet för tjänstkontot för BHOLD-kärna (BHOLD standard: b1user), och klicka sedan på **nästa**.

7.  I **lösenord** och **Bekräfta lösenord**, skriver du lösenordet för tjänstkontot.

8.  Rensa **användaren måste byta lösenord vid nästa inloggning**väljer **användaren kan inte ändra lösenordet** och **lösenordet upphör aldrig att gälla**, klickar du på **nästa**, och klicka sedan på **Slutför**.

9.  Högerklicka på användarkontot i resultatfönstret i konsolen och klicka sedan på **lägga till i en**.

10. I den **Välj grupper** dialogrutan skriver du namnet på den grupp som du skapade tidigare, skriver ett semikolon (;) och skriv sedan IIS_IUSRS.

11. Klicka på **Kontrollera namn**, och klicka sedan på **OK**.  

Följande procedur måste utföras på den dator där BHOLD-Core-modul som ska installeras. Du måste vara inloggad som medlem i gruppen Domänadministratörer för att utföra den här proceduren.

## <a name="bhold-core-installation-worksheet"></a>Kalkylblad för installation av BHOLD-kärna

Innan du börjar installera modulen BHOLD-kärna, måste förberedas för att ange den information som installationsguiden för BHOLD-kärna kräver för att slutföra installationen. Följande kalkylblad beskriver hur du registrerar du den informationen så att du är redo att ange det när det behövs. Varje avsnitt motsvarar en sida i guiden för installation av BHOLD-kärna.

### <a name="account-settings"></a>Kontoinställningar

| **Objekt**                                    | **Beskrivning**                                                                                                                                                                                                                                                                                             | **Värde**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovider på domän/datorn** | Anger att Active Directory Domain Services-säkerhet ska styra åtkomst till BHOLD-kärna.                                                                                                                                                                                                  | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                 |
| **Domän**                                  | Anger domänen som innehåller den BHOLD server-tjänstkontot och programgrupp. **Viktigt:** ange domännamnet genom att använda NetBIOS (kort)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om det fullständiga Domännamnet för domänen fabrikam.com, ange domännamnet som CONTOSO. | Skriva domännamn här:                                                                                                                                        |
| **Programgrupp**                       | Anger namnet på säkerhetsgruppen som du skapade tidigare i [krävs användar- och](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Skriva gruppnamn här:                                                                                                                                         |
| **Service-användare**                            | Anger inloggningsnamn för användarkontot som du skapade tidigare i [krävs användar- och](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Skriv namnet på användarkontot här:                                                                                                                                  |
| **Lösenord**                                | Anger lösenordet för användarkontot för BHOLD-kärna.                                                                                                                                                                                                                                              | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                |
| **Webbplatsen IP och Port**                         | Anger numret för IP-adressen och porten på webbplatsen som ska skapas på intranätserver. Ändrar du standardvärdet (\*) endast om du inte använder samma IP-adress som standardwebbplatsen. Ändra det aktuella portnumret som en tillgänglig port används endast om standard port (5151) redan.             | Om en icke-IP-adress används av standardwebbplatsen, skriva här: om Standardportnumret är redan används, Skriv BHOLD webbplats portnumret här: |

### <a name="database-settings"></a>Databasinställningar

| **Objekt**                                       | **Beskrivning**                                                                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda integrerad säkerhet**                    | Anger att Windows-autentisering används för att få åtkomst till databasen.                                                                                                                                                                                                     | Markera kryssrutan om Windows-autentisering används för att ansluta till SQL Server. Avmarkera kryssrutan om du använder SQL Server-autentisering. Databasen måste ha skapats innan kör BHOLD Core installationen om SQL Server-autentisering används. **Obs:** om Windows-autentisering används behöver du måste vara inloggad med ett konto som har rollen sysadmin på databasservern. |
| **Databasanvändare** och **lösenord för databas** | Anger användarnamn och lösenord för en användare med rollen sysadmin på databasservern. De här värdena tillhandahålls endast när SQL Server-autentisering används.                                                                                               | Skriv namnet här på SQL Server-användare: skriva här användarlösenord SQL Server: **Obs:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                                                  |
| **Databasserver** och **databasnamn**   | Anger NetBIOS-namnet på databasservern och namnet på databasen (standard: b1) som skapas av installationsprogrammet för BHOLD-kärna. Om du inte använder standard-databasserverinstans, anger du instansen i formatet  *\<server\>*\\*\<instans\>* . | Skriv namn här server (eller server och instans): Skriv namnet på databasen här:                                                                                                                                                                                                                                                                                                                   |
| **Kontrollera begränsningar för databasanvändaren**    | Föråldrad.                                                                                                                                                                                                                                                                 | Ändra inte standardvärdet                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD-kärninstallationen

Logga in som medlem i gruppen Domänadministratörer för att installera modulen BHOLD-kärna, hämta följande fil och köra den som administratör på den server som du planerar att installera modulen BHOLD-kärnor på: 

- BholdCore  *\<Version\>*\_Release.msi

Ersätt *\<Version\>* med versionsnumret för BHOLD-kärna-versionen som du installerar.

Om du vill köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

### <a name="postinstallation-settings"></a>Viktig inställningar

När BHOLD Core-installationen är klar måste du konfigurera Windows-brandväggen och ändra avancerade inställningar i BHOLD-kärna-programpoolen i Internet Information Services för att slutföra konfigurationen av BHOLD-kärna. Om det behövs kan ändra du också BHOLD systemattribut efter dina behov.

#### <a name="configuring-windows-firewall"></a>Konfigurera Windows-brandväggen

Om användarna ska komma åt BHOLD med webbläsare på fjärrdatorer, måste du konfigurera Windows-brandväggen på BHOLD-Core-servern att tillåta inkommande anslutningar på webbplats-port som du angav när du installerade BHOLD-kärna.

Om du vill utföra den här proceduren måste du vara medlem i gruppen Administratörer på den lokala datorn.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>För att tillåta inkommande anslutningar till BHOLD-webbplats

1.  Klicka på **starta**, peka på **Administrationsverktyg**, högerklickar du på **Windows-brandväggen med avancerade inställningar**, och klicka sedan på **kör som administratör**.

2.  I den vänstra rutan klickar du på **regler för inkommande trafik**, i den högra rutan klickar du på **ny regel**.

3.  I guiden Ny inkommande regel klickar du på **Port**, och klicka sedan på **nästa**.

4.  Se till att **TCP** har valts i **specifika lokala portar**, skriver du standardportarna för BHOLD-kärna (5151) eller det portnummer som du angav när du installerade BHOLD-kärna och klicka sedan på  **Nästa**.

5.  Se till att **Tillåt anslutningen** är markerad och klicka sedan på **nästa**.

6.  På den **profil** avmarkerar du kryssrutorna för platser som du inte vill ha åtkomst till BHOLD-webbplatsen och klicka sedan på **nästa**.

7.  På den **namn** sidan, Skriv ett namn för regeln (till exempel tillåter inkommande anslutningar till BHOLD-kärna) och klicka sedan på **Slutför**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Aktivera 32-bitars program för programpoolen BHOLD-kärna

Om du vill att IIS ska fungera korrekt med BHOLD-Core-modul, måste du konfigurera BHOLD-kärna programpoolen för 32-bitars program. Du måste logga in med det konto som används för att installera modulen BHOLD-kärnor för att utföra den här proceduren.

**Aktivera stöd för 32-bitars program för programpoolen BHOLD-kärna**

1.  Öppna IIS-hanteraren genom att klicka på **starta**, peka på **Administratörsverktyg**, och klicka sedan på **Internet Information Services (IIS) Manager**.

2.  Expandera servernamnet i konsolträdet och klicka sedan på **programpooler**.

3.  I den **programpooler** högerklickar du på **CoreAppPool**, och klicka sedan på **avancerade inställningar**.

4.  I den **avancerade inställningar** i dialogrutan den **aktivera 32-bitars program** väljer **SANT**, och klicka sedan på **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Upprätta tjänstens huvudnamn för BHOLD-webbplats

Om det nätverksnamn som används för att kontakta BHOLD-webbplatsen inte är samma som värdnamn för server, måste du upprätta ett tjänstens huvudnamn (SPN) för HTTP. Till exempel måste om du använder en CNAME-resurspost i DNS för att ange ett alias för servern, eller om du använder Utjämning av nätverksbelastning, du registrera dessa ytterligare nätverksadresser i Active Directory. Om du inte göra det, kan inte Internet Explorer använda Kerberos-protokollet när du kontaktar BHOLD-webbplatsen.

> [!IMPORTANT]
> Om BHOLD-Core-modul är installerad på samma dator som FIM-portalen, måste du skapa DNS-resursposter (CNAME-post eller A) med olika värdnamn för servrar som kör BHOLD-kärna och den server som kör FIM-portalen. Endast en SPN kan upprättas för en viss-typ/server-alias för service-par och BHOLD-kärna och FIM-portalen kräver separat SPN: er eftersom de vanligtvis körs under olika konton. Kommandot setspn rapporterar ett fel om ett SPN redan finns under ett annat konto.

Medlemskap i **Domänadministratörer**, eller motsvarande minimikravet för att kunna slutföra den här proceduren.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Upprätta SPN för BHOLD-webbplats

1.  Klicka på domänkontrollanten för Active Directory Domain Services **starta**, klickar du på **alla program**, klickar du på **tillbehör**, högerklickar du på **Kommandotolken** , och klicka sedan på **kör som administratör**.

2.  Skriv följande kommando vid kommandotolken och tryck sedan på RETUR: setspn – S HTTP / *\<networkalias\> \<domän\>* \\ *\<accountname\>* där:

    -   *\<networkalias\>*  är den adress som klienter använder för att kontakta BHOLD-webbplats

    -   *\<domän\>*\\*\<accountname\>*  är domänen och användarnamnet namnet på tjänstkontot för BHOLD-kärnor som du skapade när du har installerat BHOLD-kärna.

3.  Upprepa det föregående steget för alla andra namn som klienter använder för att kontakta BHOLD-webbplats till exempel CNAME-alias, namn som innehåller ett fullständigt kvalificerat domännamn eller namn som innehåller ett NetBIOS-domännamn (korta).

#### <a name="setting-bhold-system-attributes"></a>Inställningen av BHOLD-systemattribut

Öppna BHOLD-kärna-portalen och visa systemattribut för att kunna verifiera att installationen av BHOLD-Core-modul har lyckats. Du kan dessutom ändra följande BHOLD systemattribut, efter behov för att säkerställa att de BHOLD-modulen huvudfunktionerna korrekt i din miljö:

| **Attributet**                | **Beskrivning**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Ange Y om BHOLD-webbplatsen körs på en klustrad webbtjänst så att nyligen visade objekt fungerar korrekt. Ange N om BHOLD-webbplatsen körs på en fristående IIS-server.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Ange till Y så att organisationsenheter (orgunits) kan flyttas till orgunits med samma organisationens typ som den överordnade orgunit. Till exempel förhindrar detta en projekt orgunit som flyttas till en avdelning orgunit. Ange till N att tillåta en orgunit ska placeras i en orgunit av en annan typ. |
| **Kör dagar mellan ABA**     | Ange att en tvåsiffrig heltal anger intervallet (i dagar) mellan två attributbaserade auktorisering (ABA) körs. Om du vill ange att ABA körningar skiljs åt av två dagar, exempelvis 02.                                                                                                                     |
| **Starta timmes ABA kör**    | Ställ in på en tvåsiffrig heltal vill timme på dagen när auktorisation attributbaserade kör inträffar. Till exempel sker om du vill ange som ABA kör vid 11:00 (23:00), skriver du 23.                                                                                                             |
| **System kardinalitet**       | Ange N om du inte vill att en systemkontroll kardinalitet i BHOLD. Standardvärdet är Y.                                                                                                                                                                                                                             |
| **Loggning**                  | Ange N om du inte vill att ändringarna ska loggas. Standardvärdet är Y.                                                                                                                                                                                                                                            |
| **SystemQueue bearbetning**   | Ange N om du inte vill att kön Systembearbetning. Ändra inte det här värdet om inget annat anges att göra det genom att produktsupport.                                                                                                                                                                                           |

Du måste vara inloggad som medlem i gruppen Domänadministratörer för att utföra den här proceduren.

**Ange ett BHOLD-systemattribut**

1.  Klicka på **starta**, klickar du på **alla program**, och klicka sedan på **Internet Explorer**.

2.  I adressfältet skriver, där *\<server\>* är namnet på BHOLD-webbplatsservern och *\<port\>* är portnummer som är bunden till webbplatsen.

3.  Klicka på **Start**, klickar du på **värden**, och klicka sedan på **ändra**.

4.  Hitta namnet på det attribut som du vill ändra, Skriv det nya värdet i rutan bredvid attributnamnet på och klicka sedan på **OK**.

## <a name="next-steps"></a>Nästa steg

När du har installerat BHOLD-kärna och verifiera att installationen har lyckats, kan du installera ytterligare moduler. BHOLD-databas blir nu i stort sett tom, som innehåller endast ett användarkonto, rotkontot och en organisationsenhet (orgunit) rot-orgunit. Om du vill lägga till fler användare i BHOLD-databasen, kan du antingen installera modulen Access Management-anslutningstjänsten eller modulen BHOLD modellen Generator beroende på dina behov. Du kan använda modulen Access Management-anslutningstjänsten för att importera data från FIM-synkroniseringstjänsten eller du kan använda Generator för BHOLD-modellen för att importera data från en uppsättning strukturerade filer. Mer information om hur du använder modulen Access Management-anslutningstjänsten finns i [Test Lab-Guide: BHOLD Access Management-anslutningstjänsten](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Mer information om hur du använder modulen Generator för BHOLD-modellen finns:

- [Microsoft BHOLD Suite begrepp Guide](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
