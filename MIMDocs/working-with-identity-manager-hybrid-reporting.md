---
title: "Arbeta med hybridrapportering i Azure med hjälp av Identity Manager 2016 | Microsoft Docs"
description: "Lär dig hur du sammanför lokala data och molndata i hybridrapporter i Azure och hur du hanterar och visar dessa rapporter."
keywords: 
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a96d79d6773a72c813d0cd76de26ea40d28769e1
ms.sourcegitcommit: 3d8a2493eae1218bfdb75a399ffa4adc8c2a8fdf
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/20/2018
---
# <a name="work-with-hybrid-reporting-in-identity-manager-public-preview-refresh"></a>Arbeta med hybridrapportering i Identity Manager Public Preview uppdatering

Den här artikeln beskrivs hur du sammanför lokala data och molndata i hybridrapporter i Azure och hur du hanterar och visar dessa rapporter.

## <a name="available-hybrid-reports"></a>Tillgängliga hybridrapporter
De första tre Microsoft Identity Manager-rapporterna i Azure Active Directory (AD Azure) är följande:

- **Lösenordsåterställningsaktivitet**: Visar varje instans när en användare har genomfört en lösenordsåterställning med hjälp av lösenordsåterställning via självbetjäning (SSPR) och tillhandahåller portar eller metoder som används för autentisering.

- **Registrering för lösenordsåterställning**: visas varje gång en användare som registrerar för SSPR och de metoder som används för att autentisera. Exempel på metoder kan vara ett mobiltelefonnummer eller frågor och svar.
   > [!NOTE]
   > För *registrering för lösenordsåterställning* rapporterar görs ingen skillnad mellan SMS-gate och MFA-gate. Båda betraktas mobiltelefon metoder.

- **Aktiviteter för självbetjäningsgrupper**: Visar alla försök av någon att lägga till eller ta bort honom eller sig själv från en grupp och skapa gruppen.

    ![Hybridrapportering i Azure – bild för återställningsaktivitet för lösenord](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Rapporterna visar i nuläget data i upp till en månad aktivitet.
> * Du måste avinstallera den tidigare Hybrid Reporting Agent.
> * Om du vill avinstallera hybridrapporter avinstallera agenten mimreportingagent.msi.

## <a name="prerequisites"></a>Krav

* Identity Manager 2016 RTM eller SP1 Identity Manager-tjänsten.

* En Azure AD Premium-klient med en behörig administratör i din katalog.

* Utgående Internetanslutning från Identity Manager-servern till Azure.

## <a name="requirements"></a>Krav
Kraven för att använda Identity Manager hybrid reporting finns i följande tabell:

| Krav | Description |
| --- | --- |
| Azure AD Premium | Hybridrapportering är en Azure AD Premium-funktion som kräver Azure AD Premium. </br>Mer information finns i [komma igång med Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Hämta en [kostnadsfri 30-dagars utvärderingsversion av Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Du måste vara en global administratör i din Azure AD |Som standard endast globala administratörer installera och konfigurera agenter för att komma igång, åtkomst till portalen och utföra åtgärder i Azure. </br>**Viktiga**: kontot du använder när du installerar agenter måste vara ett arbets- eller skolkonto konto. Det kan inte vara ett Microsoft-konto. Mer information finns i [registrera dig för Azure som en organisation](https://docs.microsoft.com/azure/active-directory/sign-up-organization). |
| Identity Manager Hybrid Agent installeras på varje målserver Identity Manager-tjänsten | För att ta emot data och innehåller funktioner för övervakning och analyser, kräver hybrid reporting agenter som ska installeras och konfigureras på målservrar.  </br>|
| Utgående anslutning till Azure-tjänstslutpunkter | Agenten kräver anslutning till Azure-tjänstslutpunkter under installation och körning. Om utgående anslutning har blockerats av brandväggar, kontrollerar du att följande slutpunkter har lagts till i listan över tillåtna:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li></ul> |
|Utgående anslutning baserat på IP-adresser | För IP-adresser baserat filtrering för brandväggar, referera till den [Azure IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653).|
| SSL-kontroll för utgående trafik filtreras eller inaktiverad | Agenten registrering steg data överför åtgärderna eller kan misslyckas om det finns SSL-kontroll eller avslutning för utgående trafik på nätverksnivå. |
| Brandväggsportar på servern som kör agenten | Agenten kräver följande brandväggsportar måste vara öppna för att kommunicera med Azure-Tjänsteslutpunkter:<ul><li>TCP port 443</li><li>TCP port 5671</li></ul> |
| Tillåt vissa webbplatser om Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat |Om Internet Explorer förbättrad säkerhet är aktiverat, på följande webbplatser måste tillåtas på servern som agenten har installerats:<ul><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Federationsservern för din organisation som är betrodd av Azure Active Directory (till exempel https://sts.contoso.com).</li></ul> |
</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Installera Identity Manager Reporting Agent i Azure AD
När du har installerat Reporting Agent exporteras data från Identity Manager aktivitet från Identity Manager till händelseloggen i Windows. Identity Manager Reporting Agent bearbetar händelserna och överför dem till Azure. I Azure parsas, dekrypteras och filtreras händelser för de begärda rapporterna.

1.  Installera Identity Manager 2016.

2.  Ladda ned Identity Manager Reporting Agent och gör sedan följande:

    a. Logga in på hanteringsportalen för Azure AD och välj sedan **Active Directory**.

    b. Dubbelklicka på katalogen som är en global administratör och har en Azure AD Premium-prenumeration.

    c. Välj **Configuration**, och sedan ladda ned Agent för rapportering.

3.  Installera Reporting agenten genom att göra följande:

    a.  Hämta den [MIMHReportingAgentSetup.exe filen](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) för Identity Manager-tjänsten-servern.

    b.  Kör `MIMHReportingAgentSetup.exe`. 

    c.  Kör installationsprogrammet för agenten.

    d.  Kontrollera att Identity Manager Reporting Agent-tjänsten körs.

    e.  Starta om tjänsten Identity Manager.

4.  Kontrollera att Identity Manager Reporting Agent fungerar i Azure.

    Du kan skapa rapporten data med Identity Manager självbetjäning lösenord återställa portalen om du vill återställa en användares lösenord. Kontrollera att lösenordsåterställningen har slutförts och kontrollera för att säkerställa att informationen som visas i hanteringsportalen för Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Visa hybridrapporter i Azure-portalen

1.  Logga in på den [Azure-portalen](https://portal.azure.com/) med ditt globala administratörskonto för klienten.

2.  Välj **Azure Active Directory**.

3.  Välj klientkatalogen i listan över tillgängliga kataloger för din prenumeration.

4.  Välj **granskningsloggar**.

5.  I den **kategori** nedrullningsbara listan, kontrollerar du att **MIM-tjänsten** har valts.

> [!IMPORTANT]
> Det kan ta lite tid för Identity Manager granskningsdata ska visas i Azure-portalen.

## <a name="stop-creating-hybrid-reports"></a>Avbryta generering av hybridrapporter
Om du vill avbryta uppladdningen reporting granskningsdata från Identity Manager till Azure AD, avinstallera Hybrid Reporting Agent. Använda verktyget Windows Lägg till eller ta bort program om du vill avinstallera Identity Manager hybrid reporting.

## <a name="windows-events-used-for-hybrid-reporting"></a>Windows-händelser som används för hybridrapportering
Händelser som genereras av Identity Manager lagras i Windows-händelseloggen. Du kan visa händelser i den **Loggboken** genom att välja **program och tjänster loggar** > **Begärandelogg för Identity Manager**. Varje Identity Manager-begäran exporteras som en händelse i händelseloggen i JSON-strukturen. Du kan exportera resultatet till din säkerhet information och händelse (SIEM) hanteringssystem.

|Händelsetyp|ID|Händelseinformation|
|--------------|------|-----------------|
|Information|4121|Identity Manager händelsedata som inkluderar alla begärandedata.|
|Information|4137|Identity Manager händelse 4121 filnamnstillägget, om det finns för många data för en enskild händelse. Rubriken för den här händelsen visas i följande format: `"Request: <GUID> , message <xxx> out of <xxx>`.|
