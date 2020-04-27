---
title: Installation av BHOLD FIM/MIM-integrering | Microsoft Docs
description: Integrerings modulen för BHOLD Lägg till självbetjänings roll hantering till MIM och FIM
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3005e06606ec4b3b6854003213c712770376b35d
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042210"
---
# <a name="bhold-fimmim-integration-installation"></a>Installation av BHOLD FIM/MIM-integrering

Modulen BHOLD FIM-integrering lägger till självbetjänings roll hantering i Microsoft Identity Manager, vilket gör det möjligt för användare att begära ytterligare roller och verkställa vem som kan ta på dessa roller. BHOLD FIM-integrering utökar FIM-portalen för att göra det enkelt att hantera användarnas roller som en del av den övergripande FIM-administrationen. I det här avsnittet beskrivs hur du måste konfigurera nätverks infrastrukturen så att du kan installera och använda BHOLD FIM-integrering. Det beskriver också hur du installerar BHOLD FIM-integrering och konfiguration som krävs när du har installerat BHOLD FIM-integrering.

## <a name="bhold-fim-integration-installation-requirements"></a>Installations krav för BHOLD FIM-integration

BHOLD FIM integration-modulen utökar FIM-portalen och FIM-tjänsten så att användarna kan hantera sina roller i FIM-portalen. Därför är det viktigt att BHOLD Core-modulen och de nödvändiga FIM-funktionerna installeras och konfigureras innan du installerar BHOLD FIM integration-modulen.
Följande är de program varu komponenter som måste finnas på datorn innan du kan installera BHOLD FIM integration-modulen:

- Microsoft Identity Manager 2016-portalen och-tjänsten
- Microsoft Silverlight 3 eller senare
- Internet Information Services och ASP.NET
- Microsoft Silverlight-verktyg

Dessutom måste modulerna BHOLD Core och Access Management Connector redan vara distribuerade på en server i miljön, och FIM måste konfigureras med en eller flera BHOLD-hanterings agenter. Information om hur du installerar och konfigurerar BHOLD Core-modulen finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Information om hur du installerar och använder Access Management Connector-modulen finns i installation och Test Lab-guide för [Access Management Connector](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) [: BHOLD Access Management Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> Namnet på FIM-tjänstdatabasen måste vara FIMService. Installationen av BHOLD FIM-integration Miss fungerar om FIM inte har installerats med standard databas namnet för FIM-tjänsten.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera BHOLD FIM-integrering måste du skapa en BHOLD-katalog i rot katalogen på C: disk enhet (C:\BHOLD).

Dessutom måste du vara beredd att tillhandahålla den information som installations guiden för BHOLD FIM-integration kräver för att slutföra installationen. I följande kalkyl blad kan du registrera informationen så att du är redo att tillhandahålla den när den behövs.

### <a name="bholdfim-account-settings"></a>BHOLDFim konto inställningar

| **Objekt**                            | **Beskrivning**                                                                                                                                                                                                               | **Värde**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovidern på domän** | När du väljer det här anger du att Active Directory Domain Services säkerhet ska styra åtkomsten till BHOLD-kärnan.                                                                                                                    | Markera kryssrutan. **Viktigt:** Installationen Miss kommer om den här kryss rutan inte är markerad.                                                                                                                                                                                                                   |
| **Domain**                          | Anger den domän som innehåller det **tjänst konto** som du skapade när du installerade BHOLD-kärnan. Mer information finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domän namnet anges automatiskt av guiden. Ändra bara namnet om det är felaktigt. **Viktigt:** Ange domän namnet med hjälp av NetBIOS-namnet (Short), inte det fullständigt kvalificerade domän namnet (FQDN). Om FQDN för domänen till exempel är fabrikam.com anger du domän namnet som FABRIKAM. |
| **Användar**                        | Anger inloggnings namnet för BHOLD Core Service-användarkontot.                                                                                                                                                              | Skriv användar kontots namn här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                        | Anger lösen ordet för tjänstens användar konto.                                                                                                                                                                           | Skriv lösen ordet här: **viktigt:** se till att behålla det här lösen ordet på en dold, säker plats.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM-tjänstens inställningar

| **Objekt**            | **Beskrivning**                                                                                                                                                                                                                               | **Värde**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Användare**            | Anger inloggnings namnet för ett konto med administratörs behörighet för FIM. Microsoft rekommenderar starkt att du inte använder kontot som är kopplat till rot användaren i BHOLD Core (som standard kontot som används för att installera BHOLD Core). | Skriv användar kontots namn här:                                                                   |
| **Lösenord**        | Anger lösen ordet för FIM-administratörens användar konto.                                                                                                                                                                                 | Skriv lösen ordet här: **viktigt:** se till att behålla det här lösen ordet på en dold, säker plats. |
| **FIM-databas**    | Anger namnet på FIM-tjänstdatabasen.                                                                                                                                                                                               | FIMService                                                                                          |
| **Webbplats-IP/port** | Anger namn eller IP-adress för FIM-portalwebbplatsen och webbplats porten.                                                                                                                                                               | Skriv Server namnet eller adressen och porten här:                                                     |

### <a name="bhold-core-connection"></a>BHOLD Core-anslutning

| **Objekt**               | **Beskrivning**                                                                                                                                                                                                                                                                                                                                                                               | **Värde**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domain**             | Anger namnet på domänen för det konto som anges i **användare**, nedan. Ange domänen i NetBIOS (Short)-format.                                                                                                                                                                                                                                                                   | Skriv användar kontots domän namn här?                                                            |
| **Användare**               | Anger inloggnings namnet för kontot för **en BHOLD-användare som är ansvarig** för alla användare och roller och har behörighet att länka och ta bort länkar till användar roller. Microsoft rekommenderar starkt att du inte använder kontot som är kopplat till rot användaren i BHOLD Core (som standard kontot som används för att installera BHOLD Core). Det här kontot kan vara samma konto som du använder för att ansluta till FIM | Skriv användar kontots namn här:                                                                   |
| **Lösenord**           | Anger lösen ordet för det användar konto som anges i **användaren**.                                                                                                                                                                                                                                                                                                                             | Skriv lösen ordet här: **viktigt:** se till att behålla det här lösen ordet på en dold, säker plats. |
| **IP-/dator adress** | Anger IP-adressen för BHOLD Core-webbplatsens Server. Använd inte Server namnet.                                                                                                                                                                                                                                                                                                        | Skriv IP-adressen här:                                                                          |
| **Portnummer**        | Anger port numret för BHOLD Core-webbplatsen.                                                                                                                                                                                                                                                                                                                                          | Skriv port numret här:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Installation av BHOLD FIM-integration

Om du vill installera BHOLD FIM-integrering loggar du in som medlem i gruppen domän administratörer, laddar ned följande fil och kör den som administratör på den server som du vill installera BHOLD FIM-integrering på:

- BholdFIMIntegration<em>\<version\></em>\_release. msi

Ersätt * \<version\> * med versions numret för den BHOLD FIM-integrering som du installerar.

Om du vill köra program filen som administratör högerklickar du på filen och klickar sedan på **Kör som administratör**.

![köra MSI](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Uppgifter efter installation

När du har installerat BHOLD FIM-integrering måste du konfigurera Microsoft SharePoint för att ge BHOLD-tjänstkontot plats-ägar behörighet. Om FIM-portalen har kon figurer ATS för att använda Secure Sockets Layer säkerhet (SSL) måste du också ändra filer som innehåller referenser till adresserna för de BHOLD-sidor som har lagts till i FIM-portalen.

### <a name="configuring-sharepoint"></a>Konfigurerar SharePoint

För att fungera korrekt kräver BHOLD FIM-integrering att BHOLD-tjänstkontot har plats medlems behörigheter i Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Tilldela plats medlems behörigheter till BHOLD-tjänstkontot

1.  Logga in på servern som kör BHOLD FIM-integrering med administratörs behörighet.

2.  Klicka på **Start**och sedan på **Internet Exporer**.

3.  I adress fältet skriver <https://localhost> du om SharePoint har kon figurer ATS för att använda SSL-säkerhet <http://localhost>, annars skriver du.

4.  På vänster sida av sidan **grupp webbplats** klickar du på **personer och grupper**.

5.  Under **grupper** klickar du på **grupp webbplats medlemmar**och i verktygsfältet i Center-fönstret klickar du på **nytt**och sedan på **Lägg till användare**.

6.  På sidan **Lägg till användare: grupp webbplats** , i **användare/grupper**, skriver du BHOLDApplicationGroup och klickar sedan på knappen kontrol lera namn i rutan **användare/grupper** . Grupp namnet bör lösas för att inkludera domän namnet.

7.  Klicka på **ge användare behörigheter direkt**, Välj **fullständig behörighet – har fullständig kontroll**och klicka sedan på **OK**.

8.  Kontrol lera att BHOLDApplicationGroup visas i **behörigheter: grupp webbplats**och stäng sedan Internet Explorer.

![köra MSI](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Konfigurera BHOLD för att stödja SSL

Om FIM-portalen har kon figurer ATS för att använda SSL-säkerhet måste du ändra filerna på FIM-servern så att länkar till BHOLD-sidor öppnas. Filerna finns i följande mapp: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

I följande tabell visas filerna och de ursprungliga och ändrade versionerna av strängarna som ska redige ras.

| **Fil**                  | **Ursprunglig sträng**                                                                                                                   | **Ändrad sträng**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics. aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns. aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | 
| AttestationMain. aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx? hideMenu = 1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx? hideMenu = 1 |
| Repor ting. aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Självbetjänings. aspx          | RoleExchangePoint = http://\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. svc, TransportMode = transport | RoleExchangePoint = https://\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. svc, TransportMode = transport |

Där:

-   BHOLD_Server anger namnet på den BHOLD-server som finns i den ursprungliga versionen av filen * \<\> *

-   MIM_Server anger namnet på FIM-servern som finns i den ursprungliga versionen av filen * \<\> *

-   BHOLD_Server_FQDN anger det fullständigt kvalificerade domän namnet (FQDN) för BHOLD-servern * \<\> *

-   MIM_Port anger port numret för FIM-servern som finns i den ursprungliga versionen av filen * \<\> *

-   MIM_Server_FQDN anger FIM-serverns fullständiga domän namn * \<\> *

-   MIM_SSL_Port anger en annan port för användning med SSL på FIM-servern * \<\> *

### <a name="enable-approval-workflows-in-bhold-core"></a>Aktivera godkännande arbets flöden i BHOLD Core

När FIM och BHOLD är integrerade för självbetjäning, körs arbets flöden för godkännanden i FIM-tjänsten. Detta liknar arbets flödes modellen för begär Anden som kommer från FIM-portalen, till exempel när en användare skickar en begäran om att ansluta till en distributions lista. Det finns viktiga skillnader mellan arbets flöden för BHOLD-roller och andra arbets flöden som finns i FIM-tjänsten, men. När det gäller BHOLD, arbets flödes parametrar som anger vilka användare som måste godkänna en roll förfrågan kommer att ha sitt ursprung i BHOLD, i stället för att lagras i arbets flödes definitionerna i FIM-tjänstdatabasen. Dessa parametrar tillhandahålls till FIM-tjänsten av BHOLD när den första begäran görs och ett åtgärds arbets flöde kommunicerar resultaten tillbaka till BHOLD-kärnan.

BHOLD väljer en god kännare för en självbetjänings förfrågan på något av tre sätt:

-   **Line Manager som god kännare: rollbaserad markering för en organisationsenhet (OrgUnit)** Om en roll har ett attribut med namnet roletype som är inställt på god kännare eller rull trappor, och om rollen är länkad till en eller flera användare i kontexten för en OrgUnit, måste förfrågningar från användare inom den OrgUnit godkännas av en av de användare som är länkade till rollen med god kännaren eller Eskaleraren roletype.

-   **Line Manager som god kännare: attribut-baserat val för en OrgUnit** Varje OrgUnit kan ha ett eller flera attribut som anger alias för användare som kan godkänna roll tilldelningar för andra användare i OrgUnit. Attributen heter approver1, approver2 och så vidare. När en användare i OrgUnit begär en roll tilldelning dirigerar BHOLD begäran (via FIM) till de användare som anges av attributen OrgUnit-god kännare. Om en OrgUnit inte har någon av dessa attribut, kontrollerar BHOLD överordnad OrgUnits upp till rot-OrgUnit.

-   **Roll hanteraren som god kännare: attribut-baserat val för en roll** En roll kan ha en eller flera attribut (även kallat approver1 och så vidare) som anger alias för användare som kan godkänna tilldelningen av rollen. När en användare begär att tilldelas en roll som har dessa attribut för god kännare, skickar BHOLD begäran till de användare som anges av attributen.

Om en god kännare för en självbetjänings roll-begäran inte anges av någon av dessa metoder, tilldelar BHOLD automatiskt rollen utan att kräva godkännande. Av den anledningen bör du, direkt efter att ha installerat BHOLD FIM-integrering, konfigurera rot-OrgUnit med ett alias för en god kännare, till exempel rot kontot. Detta förhindrar att en användare oavsiktligt beviljas en roll innan en mer omfattande godkännande princip kan implementeras.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Så här konfigurerar du en god kännare för rot-OrgUnit

1.  Logga in på BHOLD Core-servern som administratör.

2.  Klicka på **Start**och sedan på **Internet Explorer**.

3.  I adress fältet i Internet Explorer skriver <http://localhost:5151/bhold/core>du och trycker sedan på RETUR-tangenten.

4.  På Start sidan för BHOLD Core, under **attribut def**, klickar du på **attributtyper**.

5.  På sidan **attributtyp** klickar du på **Lägg till**.

6.  På **sidan Lägg till attributtyp**, i **identitet**, skriver du Approver1, i listan **datatyp** klickar du på **alfanumerisk**, i **maximal längd**, skriv 255 i **listan med värden**, klickar på **Nej**, på **engelska**, skriver god kännare 1, klickar på **OK**och klickar sedan på **klar**.

7.  I det vänstra fönstret, under **attribut def** , klickar du på **attribut typ uppsättningar**.

8.  På sidan **Attributtyper anger** du klickar du på **Lägg till**.

9.  På sidan **Lägg till attributtyp** anger du OrgUnit-attribut i **Beskrivning**och klickar sedan på **OK**.

10. På sidan **OrgUnit attribut** expanderar du **attributtyper**och klickar sedan på **ändra**.

11. I listan **attributtyp** klickar du på **approver1**, klickar på **Lägg till**och klickar sedan på **färdig**.

12. I den vänstra rutan klickar du på **objekt typer**.

13. Klicka på **OrgUnit**på sidan **objekt typer** .

14. Expandera **Attribute Type Sets**på sidan **objekt typ/OrgUnit** och klicka sedan på **ändra**.

15. På sidan **länk attributtypen Type set/OrgUnit** , i **ordning**, skriver du in 10 i listan **attributtyp** , klickar på **OrgUnit attribut**, klickar på **Lägg till**och klickar sedan på **klart**.

16. I den vänstra rutan, under **modell**, klickar du på **organisationsenheter**.

17. På sidan **organisations enheter** klickar du på **rot**.

18. På sidan **organisations enhet/rot** klickar du på **ändra**.

19. På sidan **ändra attribut/rot för organisationsenheten** , i **god kännare**, anger du domän och användar namn för den användare som ska godkänna begär Anden om tilldelning av roller, i * \<formatet\>domän*\\*\<användare\>*, * \<där\> domän* är NetBIOS (Short) domän namn och * \<användare\> * är användarens inloggnings namn.
20. Klicka på **OK**.

> [!IMPORTANT]
> Domänen och användar namnet måste matcha standard Ali Aset för en användare i BHOLD Core-databasen.

Som ett alternativ till att ange en god kännare för organisatoriska enheter kan du ange en god kännare för föreslagna roller i BHOLD Core-databasen. Det gör du genom att skapa attributet approver1, lägga till det i ett attribut-utskrivna som är associerat med roll objekt typen och sedan ändra varje föreslagen roll för att ange god kännare.

Om du vill ge bättre arbets flödes säkerhet, förutom god kännare, bör du ange ytterligare godkännande lägen och användare genom att skapa och fylla i följande attribut för OrgUnits och roller:

- eskalerar<em>\<n\></em>

- ägare<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- meddelande<em>\<n\></em>

där * \<n\> * anger ett valfritt numeriskt suffix för att tillhandahålla flera attribut av samma typ.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Verifiera godkännande arbets flöden som kon figurer ATS i FIM-tjänsten

BHOLD FIM-integrering skapar uppsättningar, arbets flödes definitioner och hanterings princip regler (MPR) till FIM-tjänsten. Om du har anpassat FIM-distributionen för att ändra uppsättningarna med administratörer eller uppsättningar med användare som kan göra begär Anden, bör du se till att MPR refererar till rätt användar uppsättningar.

> [!NOTE]
> Innan användare av FIM-portalen kan använda självbetjänings funktionerna som tillhandahålls av BHOLD, måste användarnas konton synkroniseras till BHOLD-databasen från FIM-synkroniseringstjänsten. I synnerhet måste det finnas en användar post i BHOLD Core-databasen och i FIM-tjänstdatabasen för varje användare som kan göra en självbetjänings förfrågan eller anges som god kännare eller eskalerare för självbetjänings begär Anden.

## <a name="next-steps"></a>Nästa steg

- Information om hur du installerar FIM-portalen och andra FIM-funktioner finns i [planering och arkitektur](https://technet.microsoft.com/library/ee808044.aspx) i Microsoft Forefront Technical Library.
- [Installations guide för BHOLD](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
