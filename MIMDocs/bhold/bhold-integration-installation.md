---
title: Installation av BHOLD FIM/MIM-integrering | Microsoft Docs
description: BHOLD-integrationsmodul lägga till självbetjäning rollhantering i MIM och FIM
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: aac41b173538fe9127e14a4b9a7eb540c000fe3b
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334034"
---
# <a name="bhold-fimmim-integration-installation"></a>Installation av BHOLD FIM/MIM-integrering

BHOLD FIM integrationsmodul lägger du till självbetjäningsrollen hantering till prisalternativ för Microsoft Identity Manager, vilket gör det möjligt för användare att begära ytterligare roller och tillämpa som kan utföra på rollerna. BHOLD FIM integrationsmodul utökar FIM-portalen för att göra det enkelt att hantera användarnas roller som en del av övergripande FIM-administration. Det här avsnittet beskriver hur du måste konfigurera nätverksinfrastrukturen så att du kan installera och använda modulen BHOLD FIM-integrering. Den behandlar också hur du installerar BHOLD FIM-integreringsmodul och konfigurationen som krävs när du har installerat modulen BHOLD FIM-integrering.

## <a name="bhold-fim-integration-installation-requirements"></a>Installationskrav för BHOLD FIM-integrering

BHOLD FIM integrationsmodul utökar den FIM-portalen och FIM-tjänsten för att tillåta användare att hantera deras roller i FIM-portalen. Därför är det viktigt att BHOLD-Core-modul och nödvändiga FIM-funktioner installeras och konfigureras innan du installerar modulen BHOLD FIM-integrering.
Följande är de programvarukomponenter som måste finnas på datorn innan du kan installera modulen BHOLD FIM-integrering:

- Microsoft Identity Manager 2016-portalen och Service
- Microsoft Silverlight 3 eller senare
- IIS och ASP.NET
- Microsoft Silverlight-verktyg

Dessutom BHOLD-kärna och Access Management-anslutningstjänsten moduler måste redan distribueras på en server i miljön och FIM måste konfigureras med en eller flera av BHOLD-hanteringsagenter. Information om installation och konfigurering av BHOLD-Core-modul finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Information om att installera och använda Access Management-anslutningstjänsten-modulen finns i [Access Management Kopplingsinstallationen](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) och [Test Lab-Guide: BHOLD Access Management-anslutningstjänsten](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> Namnet på FIM-tjänstdatabasen måste vara FIM-tjänsten. BHOLD FIM integrerade installationen misslyckas om FIM inte har installerats med standarddatabasnamnet för FIM-tjänsten.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera modulen BHOLD FIM-integrering, måste du skapa en BHOLD-katalog i rotkatalogen på enhet C: disk (C:\BHOLD).

Dessutom måste du vara beredd att ange den information som installationsguiden för BHOLD FIM-integrering krävs för att slutföra installationen. Följande kalkylblad beskriver hur du registrerar du den informationen så att du är redo att ange det när det behövs.

### <a name="bholdfim-account-settings"></a>BHOLDFim kontoinställningar

| **Objekt**                            | **Beskrivning**                                                                                                                                                                                                               | **Värde**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovider på domän** | Anger att Active Directory Domain Services-säkerhet ska styra åtkomst till BHOLD-kärna.                                                                                                                    | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                          | Anger den domän som innehåller den **tjänstkontot** som du skapade när du installerar BHOLD-kärna. Mer information finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om det är felaktigt. **Viktigt:** ange domännamnet genom att använda NetBIOS (kort)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om det fullständiga Domännamnet för domänen fabrikam.com, ange domännamnet som FABRIKAM. |
| **Användarnamn**                        | Logga in namnet på användarkontot för BHOLD-kärna.                                                                                                                                                              | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                        | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                           | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Inställningar för FIM-tjänsten

| **Objekt**            | **Beskrivning**                                                                                                                                                                                                                               | **Värde**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Användare**            | Anger inloggningsnamn för ett konto med administratörsbehörighet för FIM. Microsoft rekommenderar att du inte använder det konto som är associerade med rotanvändaren i BHOLD Core (som standard det konto som används för att installera BHOLD-kärna). | Skriv namnet på användarkontot här:                                                                   |
| **Lösenord**        | Anger lösenordet för användarkontot för FIM-administratör.                                                                                                                                                                                 | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats. |
| **FIM-databasen**    | Anger namnet på FIM-tjänstdatabasen.                                                                                                                                                                                               | FIM-tjänsten                                                                                          |
| **Webbplatsen IP och Port** | Anger namn eller IP-adressen för FIM-portalservern och webbplatsport.                                                                                                                                                               | Skriv servernamnet eller adressen och porten här:                                                     |

### <a name="bhold-core-connection"></a>BHOLD-kärna-anslutning

| **Objekt**               | **Beskrivning**                                                                                                                                                                                                                                                                                                                                                                               | **Värde**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domän**             | Anger namnet på domänen för kontot som angetts i **användaren**nedan. Ange domänen i NetBIOS (korta) format.                                                                                                                                                                                                                                                                   | Skriv namnet på användarkontot domän här?                                                            |
| **Användare**               | Anger inloggningsnamnet på kontot som **en BHOLD-användare som är en ansvarig** över alla användare och roller och har behörighet att länka till och ta bort länken till användarroller. Microsoft rekommenderar att du inte använder det konto som är associerade med rotanvändaren i BHOLD Core (som standard det konto som används för att installera BHOLD-kärna). Det här kontot kan vara samma konto som används för att ansluta till FIM | Skriv namnet på användarkontot här:                                                                   |
| **Lösenord**           | Anger lösenordet för det användarkonto som anges i **användaren**.                                                                                                                                                                                                                                                                                                                             | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats. |
| **Loggningsdatorn IP-adress** | Anger IP-adressen för BHOLD-kärna-webbplatsservern. Använd inte namnet på servern.                                                                                                                                                                                                                                                                                                        | Skriv den här IP-adressen:                                                                          |
| **Portnummer**        | Anger portnumret för BHOLD-kärna-webbplatsen.                                                                                                                                                                                                                                                                                                                                          | Skriv portnumret här:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Installation av BHOLD FIM-integrering

Logga in som medlem i gruppen Domänadministratörer för att installera modulen BHOLD FIM-integrering, hämta följande fil och köra den som administratör på den server som du planerar att installera modulen BHOLD FIM-integrering på:

- BholdFIMIntegration<em>\<Version\></em>\_Release.msi

Ersätt *\<Version\>* med det lägre versionsnumret för den BHOLD FIM-integrering-versionen som du installerar.

Om du vill köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

![Kör msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Uppgifter efter installationen

När du har installerat BHOLD FIM-integrering, måste du konfigurera Microsoft SharePoint om du vill ge BHOLD-tjänsten behörigheterna webbplatsens ägare. Om FIM-portalen är konfigurerad för att använda Secure Sockets Layer (SSL)-säkerhet, måste du ändra filer som innehåller referenser till adresserna för BHOLD-sidor som lagts till i FIM-portalen.

### <a name="configuring-sharepoint"></a>Konfigurera SharePoint

Ska fungera korrekt måste kräver BHOLD FIM integrationen BHOLD-tjänstkontot har behörigheter för plats-medlem i Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Att bevilja behörigheter för plats-medlem för BHOLD-tjänstkontot

1.  Logga in på den server som kör BHOLD FIM-integrering med administratörsbehörighet.

2.  Klicka på **starta**, och klicka sedan på **Internet Exporer**.

3.  I adressfältet skriver <https://localhost> om SharePoint är konfigurerad för att använda SSL-säkerhet, ange annars <http://localhost>.

4.  På vänster sida av den **gruppwebbplats** klickar du på **personer och grupper**.

5.  Under **grupper** klickar du på **plats teammedlemmar**, och klicka på i verktygsfältet rutan i mitten **New**, och klicka sedan på **Lägg till användare**.

6.  På den **Lägg till användare: gruppwebbplats** sidan **användare/grupper**, Skriv BHOLDApplicationGroup och klicka sedan på knappen Kontrollera namn under den **användare/grupper** box. Gruppnamnet ska matcha inkludera domännamnet.

7.  Klicka på **ge användare behörighet direkt**väljer **fullständig kontroll – har fullständig kontroll**, och klicka sedan på **OK**.

8.  Kontrollera att BHOLDApplicationGroup är med i **behörigheter: gruppwebbplats**, och stäng sedan Internet Explorer.

![Kör msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Konfigurera BHOLD för SSL

Om FIM-portalen är konfigurerad för att använda SSL-säkerhet, måste du ändra filerna på FIM-servern så att länkar till BHOLD sidor öppnas. Filerna finns i följande mapp: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

I följande tabell visas filerna och den ursprungliga och den ändrade versionen med strängar som ska redigeras.

| **Filen**                  | **Ursprungliga strängen**                                                                                                                   | **Ändrade strängen**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint = http: / /\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint.svc TransportMode = Transport | RoleExchangePoint = https: / /\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint / BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Där:

-   *\<BHOLD_Server\>*  anger namnet på BHOLD-server enligt den ursprungliga versionen av filen

-   *\<MIM_Server\>*  anger namnet på FIM-server enligt den ursprungliga versionen av filen

-   *\<BHOLD_Server_FQDN\>*  anger det fullständigt kvalificerade domännamnet (FQDN) för BHOLD-servern

-   *\<MIM_Port\>*  anger du portnumret för FIM-server enligt den ursprungliga versionen av filen

-   *\<MIM_Server_FQDN\>*  Anger FQDN för FIM-server

-   *\<MIM_SSL_Port\>*  anger en annan port för användning med SSL på FIM-servern

### <a name="enable-approval-workflows-in-bhold-core"></a>Aktivera godkännandearbetsflöden i BHOLD-kärna

När du har FIM och BHOLD integrerat för självbetjäning, kör arbetsflöden för godkännande i FIM-tjänsten. Detta liknar modellen arbetsflöde för begäranden som har sitt ursprung i FIM-portalen, till exempel när en användare skickar en begäran om att ansluta till en distributionslista. Det finns viktiga skillnader mellan BHOLD rollen arbetsflöden och andra arbetsflöden finns i FIM-tjänsten men. När det gäller BHOLD kommer parametrar för arbetsflöde som anger vilka användare måste godkänna begäran om en roll i BHOLD i stället för att lagras i arbetsflödesdefinitioner i FIM-tjänstens databas. Dessa parametrar tillhandahålls av BHOLD FIM-tjänsten när den första begäran görs och en åtgärdsarbetsflödet resultaten tillbaka till BHOLD-kärna.

BHOLD väljer godkännare för en begäran om självbetjäning på något av tre sätt:

-   **Produktionsledare som godkännare: rollbaserad val för en organisationsenhet (OrgUnit)** om en roll har ett attribut med namnet roletype som har angetts till godkännare eller rulltrappa, och om rollen är länkad till en eller flera användare i samband med en OrgUnit, begär från användare i den OrgUnit måste vara godkända av en av de användare som är länkad till rollen med godkännare eller rulltrappa roletype.

-   **Produktionsledare som godkännare: attributbaserade valet för en OrgUnit** varje OrgUnit kan ha en eller flera attribut som anger alias för användare som kan godkänna rolltilldelningar för andra användare i OrgUnit. Dessa attribut kallas approver1, approver2 och så vidare. När en användare i OrgUnit begär en rolltilldelning, dirigerar BHOLD begäran (via FIM) till de användare som anges av OrgUnit godkännaren attribut. Om en OrgUnit inte har någon av dessa attribut kontrollerar BHOLD överordnade OrgUnits upp till roten OrgUnit.

-   **Rollhanteraren som godkännare: attributbaserade val för en roll** en roll kan ha en eller flera attribut (även namngivna approver1 och så vidare) som anger alias för användare som kan godkänna tilldelningen av rollen. När en användare begär som ska tilldelas en roll som har dessa godkännaren attribut, omdirigerar BHOLD begäran till de användare som anges av attribut.

Om en godkännare för en självbetjäningsrollen begäran inte har angetts av någon av följande metoder som standard tilldelar BHOLD automatiskt rollen utan att kräva godkännande. Därför bör direkt efter installation av BHOLD FIM-integrering, du konfigurera rot OrgUnit med alias för en godkännare, till exempel rotkontot. Detta förhindrar att en användare oavsiktligt beviljas en roll innan en mer omfattande godkännande-princip kan implementeras.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Konfigurera en godkännare för roten OrgUnit

1.  Logga in på BHOLD-Core-servern som administratör.

2.  Klicka på **starta**, och klicka sedan på **Internet Explorer**.

3.  I adressfältet i Internet Explorer skriver <http://localhost:5151/bhold/core>, och tryck sedan på RETUR-tangenten.

4.  På BHOLD-kärna startsidan, under **attributet def**, klickar du på **attributet typer**.

5.  På den **attributtyp** klickar du på **Lägg till**.

6.  På den **sidan Lägg till attributet**i **identitet**, Skriv approver1, i den **datatypen** klickar du på **alfanumeriskt**, i **Maxlängd**, ange 255, **lista med värden**, klickar du på **nr**i **engelska**, Skriv godkännaren 1, klicka på **OK**, och klicka sedan på **klar**.

7.  I den vänstra rutan under **attributet def** klickar du på **attributet typen anger**.

8.  På den **attributet typen anger** klickar du på **Lägg till**.

9.  På den **Lägg till uppsättning av attributet typen** sidan **beskrivning**skriver OrgUnit attribut och klicka sedan på **OK**.

10. På den **OrgUnit attribut** expanderar **attributet typer**, och klicka sedan på **ändra**.

11. I den **attributtyp** klickar du på **approver1**, klickar du på **Lägg till**, och klicka sedan på **klar**.

12. I den vänstra rutan klickar du på **objekttyper**.

13. På den **objekttyper** klickar du på **OrgUnit**.

14. På den **objekt av typen/OrgUnit** expanderar **attributet typen anger**, och klicka sedan på **ändra**.

15. På den **länka attributet typen set/OrgUnit** sidan **ordning**, Skriv 10, i den **typ attributuppsättning** klickar du på **OrgUnit attribut**, Klicka på **Lägg till**, och klicka sedan på **klar**.

16. I den vänstra rutan under **modellen**, klickar du på **organisationsenheter**.

17. På den **organisationsenheter** klickar du på **rot**.

18. På den **organisationens enhet/root** klickar du på **ändra**.

19. På den **ändra organisationsenhet attribut/root** sidan **godkännaren**, skriver du namnet för domänen och användarnamnet för den användare som kommer att godkänna begäranden för tilldelning av roller, i formatet  *\<domän\>*\\*\<användaren\>*, där *\<domän\>* är den NetBIOS (kort)-domännamn och *\<användaren\>* är användarens inloggningsnamn.
20. Klicka på **OK**.

> [!IMPORTANT]
> Namnet på domänen och användarnamnet måste matcha standardalias för en användare i databasen BHOLD-kärna.

Du kan ange en godkännare för föreslagna roller i BHOLD-kärna-databasen som ett alternativ till att ange en godkännare för organisationsenheter. För att göra det, skapa approver1 attribut, lägga till det till ett attribut utskrivna som är associerade med objektet rolltypen och ändrar varje föreslagna roll om du vill ange godkännaren.

För att ge bättre säkerhet för arbetsflöde, förutom godkännare, bör du ange ytterligare godkännande lägen och användare genom att skapa och fylla i följande attribut för OrgUnits och roller:

- rulltrappa<em>\<n\></em>

- ägare<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- meddelande<em>\<n\></em>

där *\<n\>* anger ett valfritt numeriskt suffix för att tillhandahålla flera attribut av samma typ.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Kontrollera godkännandearbetsflöden som konfigurerats i FIM-tjänsten

BHOLD FIM-integrering-installationen skapar uppsättningar, arbetsflödesdefinitioner och Hanteringsprincipregler (MPR) till FIM-tjänsten. Om du har anpassat FIM-distributionen om du vill ändra uppsättningar av administratörer eller uppsättningar med användare kan göra begäranden, bör du kontrollera att rätt användare anger hänvisar till Mpr.

> [!NOTE]
> Innan användare av FIM-portalen kan använda självbetjäning funktionerna som tillhandahålls av BHOLD, måste användarkonton synkroniseras till BHOLD-databas från FIM-synkroniseringstjänsten. I synnerhet måste det finnas en användarpost i BHOLD-kärna-databasen och i FIM-tjänstens databas för varje användare som kan göra en begäran om självbetjäning eller har angetts som en godkännare eller rulltrappa för självbetjäning begäranden.

## <a name="next-steps"></a>Nästa steg

- Information om hur du installerar FIM-portalen och andra FIM-funktioner finns i [planering och arkitektur](https://technet.microsoft.com/library/ee808044.aspx) i det tekniska biblioteket för Microsoft Forefront.
- [BHOLD-installationsguide](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
