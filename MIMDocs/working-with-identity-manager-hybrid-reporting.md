---
title: Arbeta med hybridrapportering i Azure med hjälp av Identity Manager 2016 | Microsoft Docs
description: Lär dig hur du sammanför lokala data och molndata i hybridrapporter i Azure och hur du hanterar och visar dessa rapporter.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 18e4127b1d854a53734142bb58442627619491ef
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358541"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Arbeta med hybridrapportering i Identity Manager

Den här artikeln beskrivs hur du sammanför lokala data och molndata i hybridrapporter i Azure och hur du hanterar och visar dessa rapporter.

## <a name="available-hybrid-reports"></a>Tillgängliga hybridrapporter
De första tre Microsoft Identity Manager-rapporterna i Azure Active Directory (Azure AD) är följande:

- **Återställningsaktivitet för lösenord**: Visar varje instans när en användare har genomfört med hjälp av lösenordsåterställning via självbetjäning (SSPR) och tillhandahåller gates eller metoder som används för autentisering.

- **Registrering för lösenordsåterställning**: Visar varje gång som en användare har registrerat sig för SSPR och de metoder som används för att autentisera. Exempel på metoder kan vara ett mobiltelefonnummer eller frågor och svar.
   > [!NOTE]
   > För *registrering för lösenordsåterställning* rapporterar, görs ingen skillnad mellan SMS-gate och MFA-gate. Båda betraktas mobiltelefon metoder.

- **Aktiviteter för självbetjäningsgrupper**: Visar varje försök av någon att lägga till eller ta bort sig själv från en grupp och skapa en grupp.

    ![Hybridrapportering i Azure – bild för återställningsaktivitet för lösenord](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Rapporter för närvarande finns data i upp till en månad aktivitet.
> * Föregående Hybrid Reporting Agent måste avinstalleras.
> * Om du vill avinstallera hybridrapporter avinstallera agenten mimreportingagent.msi.

## <a name="prerequisites"></a>Krav

* Identity Manager 2016 SP1 Identity Manager-tjänsten, rekommenderas build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* En Azure AD Premium-klient med en behörig administratör i din katalog.

* Utgående Internetanslutning från Identity Manager-servern till Azure.

## <a name="requirements"></a>Krav
Kraven för att använda Identity Manager hybrid reporting finns i följande tabell:


|                                         Krav                                         |                                                                                                                                                                                                                                                                                    Beskrivning                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Hybrid reporting är en Azure AD Premium-funktion som kräver Azure AD Premium. </br>Mer information finns i [komma igång med Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Hämta en [kostnadsfri 30-dagars utvärderingsversion av Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Du måste vara en global administratör för din Azure AD                     |                                                                   Som standard kan endast globala administratörer installera och konfigurera agenter för att komma igång, få åtkomst till portalen och utföra åtgärder i Azure. </br>**Viktiga**: kontot du använder när du installerar agenter måste vara ett arbets- eller skolkonto konto. Det kan inte vara ett Microsoft-konto. Mer information finns i [registrera dig för Azure som en organisation](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Identity Manager Hybrid Agent installeras på varje målserver Identity Manager-tjänsten |                                                                                                                                                                                                       Om du vill ta emot data och tillhandahålla funktioner för övervakning och analys, kräver hybrid-rapportering att agenterna ska installeras och konfigureras på utvalda servrar.  </br>                                                                                                                                                                                                       |
|                    Utgående anslutning till Azure-tjänstslutpunkter                     | Agenten kräver anslutning till Azure-tjänstslutpunkter under installation och körning. Om utgående anslutningar har blockerats av brandväggar, kontrollerar du att följande slutpunkter har lagts till listan över tillåtna:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Utgående anslutningar baserat på IP-adresser                         |                                                                                                                                                                                                                      IP-adresser baserat filtrering för brandväggar, finns det [IP-intervallen för Azures](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 SSL-kontroll för utgående trafik filtreras eller inaktiveras                 |                                                                                                                                                                                                               De agenten steg eller data överför registreringsåtgärder kan misslyckas om det finns SSL-kontroll eller avslutning för utgående trafik på nätverksnivå.                                                                                                                                                                                                                |
|                      Portar i brandväggen på den server som kör agenten                       |                                                                                                                                                                                                          Agenten kräver att följande brandväggsportar är öppna för att kommunicera med Azure-tjänstslutpunkter:<ul><li>TCP port 443</li><li>TCP port 5671</li></ul>                                                                                                                                                                                                          |
|          Tillåt vissa webbplatser om Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat           |                                                                                Om Internet Explorer förbättrad säkerhet är aktiverat, följande webbplatser måste tillåtas på servern som agenten är installerad:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>Federationsservern för din organisation är betrott av Azure Active Directory (till exempel <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Installera Identity Manager Reporting Agent i Azure AD
När du har installerat Reporting Agent exporteras från Identity Manager-aktivitet data från Identity Manager till händelseloggen i Windows. Identity Manager Reporting Agent bearbetar händelserna och överför dem till Azure. I Azure parsas, dekrypteras och filtreras händelser för de begärda rapporterna.

1.  Installera Identity Manager 2016.

2.  Ladda ned Identity Manager Reporting Agent och gör sedan följande:

    a. Logga in på Azure AD-hanteringsportalen och väljer sedan **Active Directory**.

    b. Dubbelklicka på den katalog som du är en global administratör och har en Azure AD Premium-prenumeration.

    c. Välj **Configuration**, och sedan ladda ned Agent för rapportering.

3.  Installera Reporting Agent genom att göra följande:

    a.  Ladda ned den [MIMHReportingAgentSetup.exe filen](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) för Identity Manager-tjänsten server.

    b.  Kör `MIMHReportingAgentSetup.exe`. 

    c.  Kör installationsprogrammet för agenten.

    d.  Kontrollera att Identity Manager Reporting Agent-tjänsten körs.

    e.  Starta om tjänsten Identity Manager.

4.  Kontrollera att Identity Manager Reporting Agent fungerar i Azure.

    Du kan skapa rapport data med hjälp av lösenord för självbetjäning i Identity Manager återställa portalen om du vill återställa en användares lösenord. Kontrollera att lösenordsåterställningen har slutförts och kontrollera sedan att informationen som visas i Azure AD-hanteringsportalen.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Visa hybridrapporter i Azure portal

1.  Logga in på den [Azure-portalen](https://portal.azure.com/) med ditt globala administratörskonto för klienten.

2.  Välj **Azure Active Directory**.

3.  Välj klientkatalogen i listan över tillgängliga kataloger för din prenumeration.

4.  Välj **granskningsloggar**.

5.  I den **kategori** nedrullningsbara lista, se till att **MIM-tjänsten** har valts.

> [!IMPORTANT]
> Det kan ta lite tid för Identity Manager granskningsdata visas i Azure-portalen.

## <a name="stop-creating-hybrid-reports"></a>Avbryta generering av hybridrapporter
Om du vill avbryta uppladdningen av rapportgranskningsdata från Identity Manager till Azure AD kan du avinstallera Hybrid Reporting Agent. Använd verktyget Windows lägga till eller ta bort program för att avinstallera Identity Manager hybrid reporting.

## <a name="windows-events-used-for-hybrid-reporting"></a>Windows-händelser som används för hybridrapportering
Händelser som genereras av Identity Manager lagras i Windows-händelseloggen. Du kan visa händelser i den **Loggboken** genom att välja **loggar för program och tjänster** > **Begärandelogg för Identity Manager**. Varje Identity Manager-begäran exporteras som en händelse i Windows händelselogg i JSON-strukturen. Du kan exportera resultatet till din säkerhet och händelsehantering (SIEM) hanteringssystem.

|Händelsetyp|ID|Händelseinformation|
|--------------|------|-----------------|
|Information|4121|Identity Manager händelsedata som inkluderar alla begärandedata.|
|Information|4137|Identity Manager händelse 4121 tillägget, om det finns för många data för en enskild händelse. Rubriken för den här händelsen visas i följande format: `"Request: <GUID> , message <xxx> out of <xxx>`.|
