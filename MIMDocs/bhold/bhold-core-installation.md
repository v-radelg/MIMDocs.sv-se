---
title: BHOLD kärninstallation | Microsoft Docs
description: BHOLD suite installationen core dokumentet
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 25825c1472dec44ed0e09519d4bc17809c1b95b1
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2018
---
# <a name="bhold-core-installation"></a>BHOLD-Core-installation

BHOLD-kärnmodul tillhandahåller huvudfunktionerna i BHOLD-programsvit i din miljö. BHOLD-kärnmodul måste installeras och konfigureras på en server i det lokala nätverket innan du kan installera andra BHOLD-programsvit moduler.

## <a name="bhold-core-installation-requirements"></a>BHOLD-Core installationskrav

BHOLD-kärnmodul utgör grunden för Microsoft BHOLD-programsvit. Innan du installerar andra Microsoft BHOLD-programsvit moduler måste du installera modulen BHOLD kärnor.

### <a name="bhold-core-hardware-requirements"></a>Maskinvarukrav för BHOLD kärnor

BHOLD-kärnmodul utgör grunden för Microsoft BHOLD-programsvit. Innan du installerar andra Microsoft BHOLD-programsvit moduler måste du installera modulen BHOLD kärnor.

|          |        |          |
|----------|--------|----------|
|**Komponent** |**minsta** | **Rekommenderas** |
|Processor | 64-bitars processor | Flera kärnor 64-bitars processor |
| Minne |3 GB | 6 GB eller mer |
|Lagring| 30 GB tillgängligt |Beror på storleken för distribution |
|Nätverkskort| 100 Mbit/s-anslutning till SQL-och Forefront Identity Manager (FIM) | 1 Gbit/s-anslutning till SQL- och FIM-servern|

De här rekommendationerna baseras på vanliga implementeringar och inte tar hänsyn till andra program som körs på servern. Du kan behöva använda högre prestanda komponenter beroende på din miljö.

### <a name="bhold-core-software-requirements"></a>BHOLD-Core programvarukrav

BHOLD-kärnmodul måste installeras på en dator som uppfyller följande krav:

- Servern måste köra Windows Server 2012 (64-bitars), Windows Server 2016 
- Servern måste vara medlem i en Active Directory Domain Services (AD DS)-domän. Servern kan vara en AD DS-domänkontroller i testmiljöer.
- BHOLD-Core får installeras av en användare har loggat in med ett konto i samma domän som servern och som tillhör gruppen Domänadministratörer i domänen och i gruppen Administratörer på servern.
- Microsoft Internet Information Services (IIS) med ASP.NET måste installeras på servern. IIS måste konfigureras med Windows-autentisering aktiverad. Om BHOLD Core installeras på Windows Server 2012/2016, måste IIS 6-Kompatibilitetshantering installeras. Om BHOLD Core installeras på Windows Server 2012, måste IIS 6-skriptverktyg installeras
- .NET 3.5 framework måste vara installerad.
  - Eftersom BHOLD-kärna kräver .NET 3.5, kan inte installeras på Server Core.
- Silverlight 4 krävs av andra BHOLD-moduler och så vi rekommenderar att du installerar den innan du installerar BHOLD kärnor.
- Microsoft SQL Server 2014 eller Microsoft SQL Server 2016 måste installeras på BHOLD Core-servern eller på en annan server i det lokala nätverket. 

Windows-klientdatorer måste köra Microsoft Internet Explorer version 6 eller senare och Microsoft Silverlight version 4 eller senare.

## <a name="before-you-begin"></a>Innan du börjar

BHOLD-kärnmodul kräver ett användarkonto som används för att autentisera och auktorisera BHOLD kärntjänsten till servern och andra enheter i nätverket. Det här avsnittet beskrivs hur du skapar och konfigurerar det aktuella användarkontot och ger preinstallation kalkylblad som hjälper dig att samla in den information du behöver att slutföra BHOLD Core-installationen.

>{! VIKTIGT} när du installerar BHOLD kärnor, du kan använda en befintlig SQL Server-databas som Core BHOLD-databas eller kan du tillåta BHOLD Core guiden ska skapa BHOLD Core-databasen. Om du väljer att använda en befintlig databas måste du se till att inga andra användare kan komma åt databasen medan du installerar BHOLD kärnor. Kontrollera att åtkomstkontroller på SQL Server-databasen inte tillåter åtkomst av någon annan än den användare som installerar BHOLD Core innan du installerar BHOLD kärnor.

## <a name="required-user-and-group"></a>Nödvändiga användare och grupper

BHOLD-kärnmodul måste kunna logga in på domänen med ett användarkonto som är dedikerad för detta ändamål och som är medlem av två specifika säkerhetsgrupper, inklusive en som skapats specifikt för BHOLD-kärnmodul. Medlemskap i gruppen Domänadministratörer krävs för att utföra den här proceduren.
**Skapa och konfigurera BHOLD Core gruppen användare och säkerhetsgrupper**

1.  Klicka på en domänkontrollant, **starta**, peka på **Administrationsverktyg**, och klicka sedan på **Active Directory-användare och datorer**.

2.  I konsolträdet expanderar du den domän där kontot som ska skapas, högerklicka på **användare**, peka på **ny**, och klicka sedan på **gruppen**.

3.  I den **nytt objekt – grupp** i dialogrutan **gruppnamn**, skriver du namnet på gruppen (BHOLD standard: BHOLDApplicationGroup), och klicka sedan på **OK**.

4.  Högerklicka på **användare**, peka på **ny**, och klicka sedan på **användaren**.

5.  I **fullständiga namn**, Skriv ett namn som hjälper dig att identifiera kontot, till exempel BHOLD Core-tjänstkontot.

6.  I **användarens inloggningsnamn**, skriver du namnet på tjänstkontot BHOLD kärnor (BHOLD standard: b1user), och klicka sedan på **nästa**.

7.  I **lösenord** och **Bekräfta lösenord**, skriver du lösenordet för tjänstkontot.

8.  Rensa **användaren måste byta lösenord vid nästa inloggning**väljer **användaren kan inte ändra lösenordet** och **lösenordet upphör aldrig att gälla**, klickar du på **nästa**, och klicka sedan på **Slutför**.

9.  Högerklicka på användarkontot i resultatfönstret i konsolen och klicka sedan på **lägger till**.

10. I den **Välj grupper** dialogrutan Skriv visningsnamnet för den grupp som du skapade tidigare, ett semikolon (;), och skriv sedan IIS_IUSRS.

11. Klicka på **Kontrollera namn**, och klicka sedan på **OK**.  

Följande procedur måste utföras på den dator där BHOLD-kärnmodul ska installeras. Du måste vara inloggad som medlem i gruppen Domänadministratörer utföra den här proceduren.

## <a name="bhold-core-installation-worksheet"></a>Kalkylblad för BHOLD Core-installation

Innan du börjar installera modulen BHOLD Core, måste förberedas för att ange information som installationsguiden BHOLD Core krävs för att slutföra installationen. Följande kalkylblad hjälper dig att registrera informationen så att du är redo att leverera den när det behövs. Varje avsnitt motsvarar en sida i guiden BHOLD kärnor.

### <a name="account-settings"></a>Kontoinställningar

| **Objektet**                                    | **Beskrivning**                                                                                                                                                                                                                                                                                             | **Värde**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda Security Provider på domänen/dator** | Anger att Active Directory Domain Services-säkerhet kommer att styra åtkomsten till BHOLD-kärna.                                                                                                                                                                                                  | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                 |
| **Domän**                                  | Anger den domän som innehåller den BHOLD server-tjänstkontot och programgruppen. **Viktigt:** ange domännamnet genom att använda NetBIOS (korta)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om det fullständiga Domännamnet för domänen är fabrikam.com, ange namnet på en domän som CONTOSO. | Skriva domännamnet här:                                                                                                                                        |
| **Programgruppen**                       | Anger namnet på säkerhetsgruppen som du skapade tidigare i [krävs användar- och](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Skriv namnet på här:                                                                                                                                         |
| **Tjänstanvändaren**                            | Anger inloggningsnamnet på tjänstkontot för användare som du skapade tidigare i [krävs användar- och](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Skriv namnet på användarkontot här:                                                                                                                                  |
| **Lösenord**                                | Anger lösenordet för användarkontot för BHOLD kärnor.                                                                                                                                                                                                                                              | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                |
| **Webbplatsen IP och Port**                         | Anger numret för IP-adress och port för webbplatsen som ska skapas på intranätserver. Ändra standardvärdet (\*) bara om du inte använder samma IP-adress som standardwebbplatsen. Ändra portnumret som en tillgänglig port används endast om standard port (5151) redan.             | Om en icke-IP-adress används av standardwebbplatsen, Skriv det här: om Standardportnumret är redan används, Skriv BHOLD webbplatsens portnummer här: |

### <a name="database-settings"></a>Databasinställningar

| **Objektet**                                       | **Beskrivning**                                                                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda integrerad säkerhet**                    | Anger att Windows-autentisering används för att få åtkomst till databasen.                                                                                                                                                                                                     | Markera kryssrutan om Windows-autentisering används för att ansluta till SQL Server. Avmarkera kryssrutan om SQL Server-autentisering används. Databasen måste ha skapats innan du kör BHOLD Core installation om SQL Server-autentisering används. **Obs:** om Windows-autentisering används, du måste vara inloggad med ett konto som har serverrollen sysadmin på databasservern. |
| **Databasanvändare** och **lösenord för databas** | Anger användarnamn och lösenord för en användare med serverrollen sysadmin på databasservern. Dessa värden anges endast när SQL Server-autentisering används.                                                                                               | Skriv namnet här på SQL Server-användare: skriva SQL Server användarens lösenord här: **Obs:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                                                  |
| **Databasserver** och **databasnamn**   | Anger namnet på databasen och NetBIOS-namnet på databasservern (standard: b1) som skapas av installationsprogrammet för BHOLD-kärnor. Om du inte använder standard-databasserverinstans, anger du databasserverinstansen i formatet  *\<server\>*\\*\<instans\>* . | Skriv namnet här server (eller server och instans): skriva namnet på databasen här:                                                                                                                                                                                                                                                                                                                   |
| **Kontrollera begränsningar för databasanvändare**    | Föråldrad.                                                                                                                                                                                                                                                                 | Ändra inte standardvärdet                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD kärninstallationen

Logga in som medlem i gruppen Domänadministratörer om du vill installera modulen BHOLD kärnor, hämta följande fil och kör det som administratör på den server som du tänker installera modulen BHOLD kärnor på: 

- BholdCore  *\<Version\>*\_Release.msi

Ersätt *\<Version\>* med versionsnumret för BHOLD Core-versionen som du installerar.

För att köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

### <a name="postinstallation-settings"></a>Viktig inställningar

När BHOLD Core-installationen är klar, måste du konfigurera Windows-brandväggen och ändra avancerade inställningar i BHOLD Core programpoolen i Internet Information Services för att slutföra konfigurationen BHOLD kärnor. Om det behövs kan ändra du också BHOLD systemattribut som motsvarar dina behov.

#### <a name="configuring-windows-firewall"></a>Konfigurera Windows-brandväggen

Om användarna ansluter till BHOLD via en webbläsare på fjärrdatorer, måste du konfigurera Windows-brandväggen på BHOLD Core-server att tillåta inkommande anslutningar till porten för webbplatsen som du angav när du installerade BHOLD kärnor.

Om du vill utföra den här proceduren måste du vara medlem i gruppen Administratörer på den lokala datorn.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Att tillåta inkommande anslutningar på webbplatsen BHOLD

1.  Klicka på **starta**, peka på **Administrationsverktyg**, högerklicka på **Windows-brandväggen med avancerade inställningar**, och klicka sedan på **kör som administratör**.

2.  I den vänstra rutan klickar du på **regler för inkommande trafik**, och klickar sedan på i den högra rutan, **ny regel**.

3.  I guiden Ny inkommande regel klickar du på **Port**, och klicka sedan på **nästa**.

4.  Se till att **TCP** är markerad i **specifika lokala portar**, Skriv portnumret standard BHOLD kärnor (5151) eller portnumret som du angav när du installerade BHOLD kärnor och klicka sedan på  **Nästa**.

5.  Se till att **tillåter anslutningen** är markerad och klicka sedan på **nästa**.

6.  På den **profil** avmarkerar du kryssrutorna för platser som du inte vill ha åtkomst till webbplatsen BHOLD, och klicka sedan på **nästa**.

7.  På den **namn** sidan, Skriv ett namn för regeln (till exempel Tillåt inkommande anslutningar till BHOLD kärna) och klicka sedan på **Slutför**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Aktivera 32-bitars program för programpoolen BHOLD kärnor

Om du vill att IIS ska fungera korrekt med BHOLD-kärnmodul, måste du konfigurera BHOLD Core programpoolen för att stödja 32-bitarsprogram. Du måste logga in med kontot används för att installera modulen BHOLD kärnor för att utföra den här proceduren.

**Att aktivera stöd för 32-bitarsprogram för programpoolen BHOLD kärnor**

1.  Öppna IIS-hanteraren genom att klicka på **starta**, peka på **Administratörsverktyg**, och klicka sedan på **Internet Information Services (IIS) Manager**.

2.  I konsolträdet expanderar du servernamnet och klicka sedan på **programpooler**.

3.  I den **programpooler** högerklickar **CoreAppPool**, och klicka sedan på **avancerade inställningar**.

4.  I den **avancerade inställningar** i dialogrutan den **aktivera 32-bitarsprogram** väljer **SANT**, och klicka sedan på **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Upprätta tjänstens huvudnamn för BHOLD-webbplats

Om nätverksnamnet som används för att kontakta BHOLD-webbplatsen inte är samma som värdnamn för server, måste du upprätta en tjänstens huvudnamn (SPN) för HTTP. Till exempel måste om du använder en CNAME-resurspost i DNS för att ange ett alias för servern, eller om du använder Utjämning av nätverksbelastning, du registrera dessa ytterligare nätverksadresser i Active Directory. Om du inte göra det, kan inte Internet Explorer använda Kerberos-protokollet vid kontakt med BHOLD-webbplatsen.

>[!IMPORTANT]
Om modulen BHOLD Core installeras på samma dator som FIM-portalen, måste du skapa DNS-resursposter (CNAME-post eller A) med olika värdnamn för servrar som kör BHOLD kärnor och den server som kör FIM-portalen. Endast ett SPN kan upprättas för en viss service-typen/server-alias-par och BHOLD kärnor och FIM-portalen kräver separat SPN eftersom de vanligtvis körs under olika konton. Kommandot setspn rapporterar ett fel om ett SPN-namn finns redan under ett annat konto.

Medlemskap i **Domänadministratörer**, eller motsvarande krävs för att slutföra den här proceduren.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Att fastställa SPN för BHOLD-webbplats

1.  Klicka på domänkontrollanten för Active Directory Domain Services **starta**, klickar du på **alla program**, klickar du på **tillbehör**, högerklicka på **kommandotolk** , och klicka sedan på **kör som administratör**.

2.  Skriv följande kommando vid kommandotolken och tryck sedan på RETUR: setspn – S HTTP / *\<networkalias\> \<domän\>* \\ *\<accountname\>* där:

    -   *\<networkalias\>*  är den adress som klienter använder för att kontakta BHOLD-webbplats

    -   *\<domän\>*\\*\<accountname\>*  heter domänen och användarnamnet BHOLD Core kontot som du skapade när du installerade BHOLD kärnor.

3.  Upprepa det föregående steget för alla andra namn som klienter använder för att kontakta BHOLD-webbplats till exempel CNAME-alias, namn som innehåller ett fullständigt kvalificerat domännamn eller namn som innehåller ett NetBIOS-domännamn (korta).

#### <a name="setting-bhold-system-attributes"></a>Inställningen BHOLD systemattribut

Öppna BHOLD Core-portalen för att kunna verifiera att installationen av BHOLD-kärnmodul lyckades och visa systemattribut. Du kan dessutom ändra följande BHOLD systemattribut, efter behov för att säkerställa att de BHOLD-modulen huvudfunktionerna korrekt i din miljö:

| **Attributet**                | **Beskrivning**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Ange Y om BHOLD-webbplatsen körs på en klustrad webbtjänst så att nyligen visas objekt fungerar korrekt. Ange N om BHOLD-webbplatsen körs på en fristående IIS-server.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Ställ in Y om du vill se till att organisationsenheter (orgunits) kan endast flyttas till orgunits med samma organisationens typ som den överordnade orgunit. Till exempel Detta förhindrar att ett projekt orgunit flyttas till en avdelning orgunit. Ange till N så att en orgunit ska placeras i en orgunit av en annan typ. |
| **Kör dagar mellan ABA**     | Ange ett heltal med två siffror anger intervallet (i dagar) mellan två attributbaserad auktorisering (ABA) körs. Skriv till exempel 02 om du vill ange att ABA körs skiljs åt av två dagar.                                                                                                                     |
| **Starta timmes ABA kör**    | Ange ett tvåsiffrig heltal att ange timme på dagen när kör attributbaserad tillstånd inträffar. Till exempel sker om du vill ange att ABA kör vid 11:00 (23:00) skriver du 23.                                                                                                             |
| **System kardinalitet**       | Ange N om du inte vill att en systemkontroll kardinalitet i BHOLD. Standardvärdet är Y.                                                                                                                                                                                                                             |
| **Loggning**                  | Ange N om du inte vill att ändringarna ska loggas. Standardvärdet är Y.                                                                                                                                                                                                                                            |
| **SystemQueue bearbetning**   | Ange N om du inte vill system kön bearbetning. Ändra inte det här värdet om inte dirigeras till att göra det med produktsupport.                                                                                                                                                                                           |

Du måste vara inloggad som medlem i gruppen Domänadministratörer utföra den här proceduren.

**Ange ett systemattribut BHOLD**

1.  Klicka på **starta**, klickar du på **alla program**, och klicka sedan på **Internet Explorer**.

2.  Ange i adressfältet, där *\<server\>* är namnet på webbplatsservern BHOLD och *\<port\>* binds portnummer till webbplatsen.

3.  Klicka på **Start**, klickar du på **värden**, och klicka sedan på **ändra**.

4.  Leta reda på namnet på det attribut som du vill ändra, Skriv det nya värdet i rutan bredvid attributnamnet på och klicka sedan på **OK**.

## <a name="next-steps"></a>Nästa steg

När du har installerat BHOLD kärnor och verifiera att installationen har slutförts, kan du installera ytterligare moduler. Nu är BHOLD-databas i stort sett tomt med bara ett användarkonto, rotkontot och en organisationsenhet (orgunit) rot orgunit. Om du vill lägga till fler användare i BHOLD-databasen, kan du antingen installera modulen Access Management-anslutningstjänsten eller modulen BHOLD modellen Generator beroende på dina behov. Du kan använda modulen Access Management-anslutningstjänsten för att importera data från FIM-synkroniseringstjänsten och du kan använda Generator för BHOLD-modellen för att importera data från en uppsättning strukturerade filer. Mer information om hur du använder modulen Access Management-anslutningstjänsten finns [Test Lab-Guide: BHOLD Access Management-anslutningstjänsten](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Mer information om hur du använder modulen BHOLD modellen Generator finns:

- [Guide för Microsoft BHOLD Suite-begrepp](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
