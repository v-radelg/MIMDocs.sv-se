---
title: BHOLD FIM/MIM integrering installation | Microsoft Docs
description: "BHOLD-integrationsmodul lägga till självbetjäning rollhantering i MIM och FIM"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-fimmim-integration-installation"></a>BHOLD FIM/MIM integrering Installation

BHOLD FIM integrationsmodulen lägger till självbetjäning rollhantering till Microsof Identity Manager, vilket gör det möjligt för användare att begära ytterligare roller och genomdriva som kan ha rollerna. BHOLD FIM integrationsmodulen utökar FIM-portalen för att göra det lättare att hantera användarnas roller som en del av övergripande FIM-administration. Det här avsnittet beskrivs hur du måste konfigurera nätverkets infrastruktur så att du kan installera och använda modulen BHOLD FIM-integrering. Den omfattar också hur du installerar BHOLD FIM integrationsmodul och konfigurationen som krävs när du har installerat modulen BHOLD FIM-integrering.

## <a name="bhold-fim-integration-installation-requirements"></a>Installationskrav för BHOLD FIM-integrering

BHOLD FIM integrationsmodulen utökar den FIM-portalen och FIM-tjänsten så att användarna för att hantera deras roller i FIM-portalen. Därför är det viktigt att modulen BHOLD kärnor och nödvändiga FIM-funktioner installeras och konfigureras innan du installerar modulen BHOLD FIM-integrering.
Följande är de programvarukomponenter som måste vara installerat på datorn innan du kan installera modulen BHOLD FIM-integrering:

- Microsoft Identity Manager 2016 portalen och tjänsten
- Microsoft Silverlight 3 eller senare
- Internet information Services och ASP.NET
- Microsoft Silverlight-verktyg

Dessutom modulerna BHOLD kärnor och Access Management-anslutningstjänsten måste redan distribueras på en server i miljön och FIM måste konfigureras med en eller flera BHOLD-hanteringsagenter. Information om installation och konfiguration av BHOLD-kärnmodul finns [BHOLD-kärninstallation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Information om hur du installerar och använder modulen Access Management-anslutningstjänsten finns [Access Management Kopplingsinstallationen](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx) och [Test Lab-Guide: BHOLD Access Management-anslutningstjänsten](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

>[!IMPORTANT]
Namnet på FIM-tjänstdatabasen måste vara FIM-tjänst. BHOLD FIM integrerade installationen misslyckas om FIM inte installerades med FIM-tjänsten standardnamnet på databasen.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera modulen BHOLD FIM-integrering, måste du skapa en BHOLD-katalog i rotkatalogen på enhet C: disk (C:\BHOLD).

Dessutom måste du vara beredd att ange den information som installationsguiden BHOLD FIM-Integration kräver för att slutföra installationen. Följande kalkylblad hjälper dig att registrera informationen så att du är redo att leverera den när det behövs.

### <a name="bholdfim-account-settings"></a>Inställningar för BHOLDFim

| **Objektet**                            | **Beskrivning**                                                                                                                                                                                                               | **Värde**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda Security Provider på domänen** | Anger att Active Directory Domain Services-säkerhet kommer att styra åtkomsten till BHOLD-kärna.                                                                                                                    | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                          | Anger den domän som innehåller den **tjänstkonto** som du skapade när du installerar BHOLD kärnor. Mer information finns i [BHOLD-kärninstallation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om den är felaktig. **Viktigt:** ange domännamnet genom att använda NetBIOS (korta)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om FQDN för domänen är fabrikam.com, ange namnet på en domän som FABRIKAM. |
| **Användarnamn**                        | Anger användarkontot för BHOLD Core inloggningsnamn.                                                                                                                                                              | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                        | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                           | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Inställningar för FIM-tjänsten

| **Objektet**            | **Beskrivning**                                                                                                                                                                                                                               | **Värde**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Användare**            | Anger inloggningsnamn för ett konto med administratörsbehörighet för FIM. Microsoft rekommenderar att du inte använder det konto som är associerade med rotanvändaren i BHOLD kärnor (som standard det konto som används för att installera BHOLD Core). | Skriv namnet på användarkontot här:                                                                   |
| **Lösenord**        | Anger lösenordet för FIM-administratörskonto.                                                                                                                                                                                 | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats. |
| **FIM-databasen**    | Anger namnet på FIM-tjänstdatabasen.                                                                                                                                                                                               | FIM-tjänst                                                                                          |
| **Webbplatsen IP och Port** | Anger namn eller IP-adressen för servern för FIM-portalen och webbplatsens portnummer.                                                                                                                                                               | Skriv servernamnet eller adressen och porten här:                                                     |

### <a name="bhold-core-connection"></a>BHOLD-Core-anslutning

| **Objektet**               | **Beskrivning**                                                                                                                                                                                                                                                                                                                                                                               | **Värde**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domän**             | Anger namnet på domänen för kontot som angavs i **användaren**, nedan. Ange domänen i (korta) NetBIOS-format.                                                                                                                                                                                                                                                                   | Skriv namnet på användarkontot domän här?                                                            |
| **Användare**               | Anger inloggningsnamnet på kontot för **BHOLD användare som är en ansvarig** över alla användare och roller och har behörighet att länka och Avlänka användarroller. Microsoft rekommenderar att du inte använder det konto som är associerade med rotanvändaren i BHOLD kärnor (som standard det konto som används för att installera BHOLD Core). Det här kontot kan vara samma konto som används för att ansluta till FIM | Skriv namnet på användarkontot här:                                                                   |
| **Lösenord**           | Anger lösenordet för det användarkonto som anges i **användaren**.                                                                                                                                                                                                                                                                                                                             | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats. |
| **IP/datorn adress** | Anger IP-adressen för BHOLD Core-webbplatsservern. Använd inte namnet på servern.                                                                                                                                                                                                                                                                                                        | Skriva den här IP-adressen:                                                                          |
| **Portnummer**        | Anger du portnumret för webbplatsen BHOLD kärnor.                                                                                                                                                                                                                                                                                                                                          | Skriv portnumret här:                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM integrerade installationen

Logga in som medlem i gruppen Domänadministratörer om du vill installera modulen BHOLD FIM-integrering, hämta följande fil och kör det som administratör på den server som du tänker installera modulen BHOLD FIM-integrering på:

- BholdFIMIntegration*\<Version\>*\_Release.msi

Ersätt * \<Version\> * med versionsnumret för integrering av BHOLD FIM-versionen som du installerar.

För att köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

![Kör msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Uppgifter efter installationen

När du har installerat BHOLD FIM-integrering, måste du konfigurera Microsoft SharePoint om du vill ge de BHOLD webbplatsägare behörigheterna för kontot. Om FIM-portalen har konfigurerats för att använda Secure Sockets Layer (SSL)-säkerhet, måste du ändra filer som innehåller referenser till adresser BHOLD-sidor till FIM-portalen.

### <a name="configuring-sharepoint"></a>Konfigurera SharePoint

BHOLD FIM-integrering kräver BHOLD-tjänstkonto som har behörigheter för plats-medlem i Microsoft SharePoint ska fungera ordentligt.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>För att ge plats medlem behörigheter för BHOLD-tjänstkontot

1.  Logga in på den server som kör BHOLD FIM-integrering med administratörsbehörighet.

2.  Klicka på **starta**, och klicka sedan på **Internet Exporer**.

3.  I adressfältet skriver <https://localhost> om SharePoint är konfigurerad för att använda SSL-säkerhet, annars skriver <http://localhost>.

4.  På vänster sida av den **gruppwebbplats** klickar du på **personer och grupper**.

5.  Under **grupper** klickar du på **plats gruppmedlemmar**, och klicka på i verktygsfältet i mitten av fönstret **ny**, och klicka sedan på **Lägg till användare**.

6.  På den **Lägg till användare: gruppwebbplats** sidan **användare eller grupper**BHOLDApplicationGroup och skriv sedan på knappen Kontrollera namn under den **användare eller grupper** rutan. Gruppnamnet måste matcha inkludera domännamnet.

7.  Klicka på **ge användare behörighet direkt**väljer **fullständig kontroll – har fullständig kontroll**, och klicka sedan på **OK**.

8.  Kontrollera att BHOLDApplicationGroup finns i **behörigheter: gruppwebbplats**, och stäng sedan Internet Explorer.

![Kör msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Konfigurera BHOLD att stödja SSL

Om FIM-portalen är konfigurerad för att använda SSL-säkerhet, måste du ändra filer på FIM-servern så att länkar till BHOLD sidor öppnas. Filerna finns i följande mapp: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

I följande tabell visas filerna och de ursprungliga och de ändrade versionerna av strängar som ska redigeras.

| **Filen**                  | **Ursprungliga strängen**                                                                                                                   | **Ändrade sträng**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint = http: / /\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint.svc TransportMode = Transport | RoleExchangePoint = https: / /\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint / BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Där:

-   *\<BHOLD_Server\> * anger namnet på BHOLD-server som hittades i den ursprungliga versionen av filen

-   *\<MIM_Server\> * anger namnet på FIM-server som hittades i den ursprungliga versionen av filen

-   *\<BHOLD_Server_FQDN\> * anger fullständigt kvalificerade domännamnet (FQDN) för BHOLD-servern

-   *\<MIM_Port\> * anger du portnumret för FIM-server som hittades i den ursprungliga versionen av filen

-   *\<MIM_Server_FQDN\> * Anger FQDN för FIM-server

-   *\<MIM_SSL_Port\> * anger en annan port för användning med SSL på FIM-servern

### <a name="enable-approval-workflows-in-bhold-core"></a>Aktivera godkännandearbetsflöden i BHOLD kärnor

När FIM och BHOLD integrerad för självbetjäning, kör arbetsflöden för godkännande i FIM-tjänsten. Detta liknar modellen arbetsflöde för begäranden som har sitt ursprung i FIM-portalen, till exempel när en användare skickar en begäran om att ansluta till en distributionslista. Det finns viktiga skillnader mellan BHOLD roll arbetsflöden och andra arbetsflöden finns i FIM-tjänst men. När det gäller BHOLD kommer arbetsflödesparametrar som anger vilka användare måste godkänna begäran om en roll i BHOLD i stället för att lagras i arbetsflödesdefinitioner i FIM-tjänstdatabasen. Dessa parametrar tillhandahålls av BHOLD FIM-tjänsten när den första begäranden görs och en åtgärdsarbetsflödet resultaten tillbaka till BHOLD kärnor.

BHOLD väljer godkännare för en begäran med självbetjäning på något av tre sätt:

-   **Linjehanteraren som godkännare: rollbaserad val för en organisationsenhet (OrgUnit)** om en roll har ett attribut med namnet roletype som har angetts till godkännare eller rulltrappa och rollen som är kopplad till en eller flera användare i samband med en OrgUnit förfrågningar från användare i den OrgUnit måste godkännas av en användare som är kopplad till rollen godkännare eller rulltrappa roletype.

-   **Linjehanteraren som godkännare: attributbaserade urvalet för en OrgUnit** varje OrgUnit kan ha en eller flera attribut som anger de användare som kan godkänna rolltilldelningar för andra användare i OrgUnit-alias. Dessa attribut är namngivna approver1, approver2 och så vidare. När en användare i OrgUnit begär en rolltilldelning, dirigerar BHOLD begäran (via FIM) till de användare som anges av OrgUnit godkännare attribut. Om en OrgUnit inte har någon av dessa attribut kontrollerar BHOLD överordnade OrgUnits upp till roten OrgUnit.

-   **Rollhanteraren som godkännare: attributbaserade urvalet för en roll** en roll kan ha en eller flera attribut (även namngiven approver1 och så vidare) som anger alias för användare som kan godkänna tilldelningen av rollen. När en användare begär tilldelas en roll som har dessa godkännare attribut, dirigerar BHOLD begäran till de användare som anges av attribut.

Om godkännare för en begäran för självbetjäning roll inte anges med en av dessa metoder som standard tilldelas BHOLD automatiskt rollen utan att kräva godkännande. Därför bör direkt efter installation BHOLD FIM-integrering, du konfigurera rot OrgUnit med alias för godkännare, till exempel rotkontot. Detta förhindrar att en användare oavsiktligt beviljas en roll innan en mer omfattande godkännande princip kan genomföras.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Så här konfigurerar du godkännare för roten OrgUnit

1.  Logga in på BHOLD Core-server som administratör.

2.  Klicka på **starta**, och klicka sedan på **Internet Explorer**.

3.  Ange i adressfältet i Internet Explorer <core-http://localhost:5151/bhold>, och tryck på RETUR.

4.  Om BHOLD startsidan, under **attributet def**, klickar du på **attributet typer**.

5.  På den **attributtyp** klickar du på **Lägg till**.

6.  På den **Lägg till attributet sidan**i **identitet**, Skriv approver1, i den **datatyp** klickar du på **alfanumeriskt**, i **Maxlängd**, Skriv 255, i **lista med värden**, klickar du på **nr**i **engelska**, Skriv godkännare 1, klicka på **OK**, och klicka sedan på **klar**.

7.  I den vänstra rutan under **attributet def** klickar du på **attributet typen anger**.

8.  På den **attributet typen anger** klickar du på **Lägg till**.

9.  På den **lägga till attributet typ inställt** sidan **beskrivning**OrgUnit attribut och skriv sedan på **OK**.

10. På den **OrgUnit attribut** expanderar **attributet typer**, och klicka sedan på **ändra**.

11. I den **attributtyp** klickar du på **approver1**, klickar du på **Lägg till**, och klicka sedan på **klar**.

12. I den vänstra rutan klickar du på **objekttyper**.

13. På den **objekttyper** klickar du på **OrgUnit**.

14. På den **objekt av typen/OrgUnit** expanderar **attributet typen anger**, och klicka sedan på **ändra**.

15. På den **länka attributet typen set/OrgUnit** sidan **ordning**, skriver du 10, i den **typen attributuppsättningen** klickar du på **OrgUnit attribut**, Klicka på **Lägg till**, och klicka sedan på **klar**.

16. I den vänstra rutan under **modellen**, klickar du på **organisationsenheter**.

17. På den **organisationsenheter** klickar du på **rot**.

18. På den **organisatorisk enhet/root** klickar du på **ändra**.

19. På den **ändra organisationsenhet attribut/root** sidan **godkännare**, typnamn domänen och användarnamnet för användaren som ska godkänna rolltilldelning förfrågningar, i formatet * \<domän\>*\\*\<användare\>*, där * \<domän\> * är den Domännamn för NetBIOS (korta) och * \<användare\> * är användarens inloggningsnamn.
20. Klicka på **OK**.

>[!IMPORTANT]
Namnet på domänen och användarnamnet måste matcha standardalias för en användare i databasen BHOLD kärnor.

Du kan ange godkännare för föreslagna roller i Core BHOLD-databas som ett alternativ till att ange godkännare för organisationsenheter. Gör du genom skapa attributet approver1, lägger du till på den till ett attribut typeset som är kopplade till objekttypen roll och ändra varje föreslagna roll om du vill ange godkännaren.

För att ge bättre säkerhet för arbetsflödet, utöver godkännare, ska du ange extra godkännande lägen och användare genom att skapa och fylla i följande attribut för OrgUnits och roller:

- rulltrappa*\<n\>*

- ägare*\<n\>*

- securityOfficer*\<n\>*

- meddelande*\<n\>*

där * \< n \> * anger ett numeriskt suffix för att tillhandahålla flera attribut av samma typ som valfria.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Kontrollera godkännandearbetsflöden som konfigurerats i FIM-tjänsten

BHOLD FIM-integrering installationen skapar uppsättningar, arbetsflödesdefinitioner och Hanteringsprincipregler (MPR) till FIM-tjänsten. Om du har anpassat FIM-distribution om du vill ändra uppsättningar av administratörer eller uppsättningar med användare som kan skapa förfrågningar, bör du kontrollera att MPR refererar till rätt användare anger.

>[!NOTE]
Innan användare av FIM-portalen kan använda självbetjäning funktionerna som tillhandahålls av BHOLD, måste de användarkonton synkroniseras till BHOLD-databas från FIM-synkroniseringstjänsten. I synnerhet måste det finnas en användarpost i databasen BHOLD kärnor och FIM-tjänstdatabasen för varje användare som kan göra en självbetjäning begäran eller anges som godkännare eller rulltrappa för självservicebegäranden.

## <a name="next-steps"></a>Nästa steg

- Information om hur du installerar FIM-portalen och andra FIM-funktioner finns i [planering och arkitektur](https://technet.microsoft.com/library/ee808044.aspx) i det tekniska biblioteket för Microsoft Forefront.
- [BHOLD-installationsguiden](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
