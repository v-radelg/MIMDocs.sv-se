---
title: "Arbeta med hybridrapportering i Azure med hjälp av MIM 2016 | Microsoft Docs"
description: "Lär dig hur du sammanför lokala data och molndata i hybridrapporter i Azure och hur du hanterar och visar dessa rapporter."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f16c3a054f0a2c59f118ba33bf64fca10034690
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.contentlocale: sv-se
ms.lasthandoff: 05/02/2017


---

# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>Arbeta med Identity Manager Hybrid Reporting - öppen förhandsversion (uppdatera)

## <a name="available-hybrid-reports"></a>Tillgängliga hybridrapporter
De första tre Microsoft Identity Manager (MIM)-rapporterna i Azure AD är **Återställningsaktivitet för lösenord**, **Registrering för lösenordsåterställning** och **Aktiviteter för självbetjäningsgrupper**.

-   Återställningsaktiviteten för lösenord visar alla tillfällen någon användare har genomfört en lösenordåterställningsaktivitet via SSPR och anger de portaler eller **metoder** som användes vid autentiseringen.

-   Registrering för lösenordsåterställning visar alla tillfällen någon användare har registrerat sig för SSPR och vilka **metoder** som använts för autentisering, exempelvis ett mobiltelefonnummer eller frågor och svar.
    Observera att vid registrering för lösenordsåterställning görs ingen skillnad mellan SMS-gate och MFA-gate, båda betraktas som **mobiltelefon**.

-   Aktiviteter för självbetjäningsgrupper visar alla försök av någon att lägga till eller ta bort sig själv från en grupp och försök att skapa en grupp.

    ![Hybridrapportering i Azure – bild för återställningsaktivitet för lösenord](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> Rapporterna visar i nuläget data för upp till en månad bakåt i tiden.</br>
> Du måste avinstallera den tidigare hybridagenten</br>
> Om du vill avinstallera hybridrapporter ska du avinstallera agenten MIMreportingAgent.msi.

## <a name="prerequisites"></a>Förutsättningar

1.  Installera Microsoft Identity Manager 2016 RTM- eller SP1 MIM-tjänsten.

2.  Säkerställ att du har en Azure AD Premium-klient med en behörig administratör i din katalog.

3.  Se till att du har en utgående internetanslutning från Microsoft Identity Manager-servern till Azure.

## <a name="requirements"></a>Krav
Följande tabell innehåller en lista över kraven för att använda Microsoft Identity Manager Hybrid Reporting.

| Krav | Beskrivning |
| --- | --- |
| Azure AD Premium | Hybrid Reporting är en Azure AD Premium-funktion som kräver Azure AD Premium. </br></br>Mer information finns i [Komma igång med Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium) </br>Om du vill starta en kostnadsfri 30-dagars utvärderingsversion, se [Starta en utvärderingsversion.](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Du måste vara en global administratör i din Azure AD för att komma igång |Endast globala administratörer kan som standard, installera och konfigurera agenter för att komma igång, få åtkomst till portalen och utföra åtgärder i Azure. </br></br>**Viktigt:** det konto som används när du installerar agenter måste vara ett arbets- eller skolkonto. Det kan inte vara ett Microsoft-konto. Mer information finns i [Registrera dig för Azure som en organisation](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization) |
| Microsoft Identity Manager Hybrid Agent installeras på varje riktad MIM Service-server | Hybrid-rapportering kräver att agenterna ska installeras och konfigureras på utvalda servrar för att ta emot data och innehåller funktioner för övervakning och analys </br>|
| Utgående anslutning till Azure-tjänstslutpunkter | Agenten kräver anslutning till Azure-tjänstslutpunkter under installation och körning. Om den utgående anslutningen blockeras med brandväggar, kontrollerar du att följande slutpunkter läggs till i listan över tillåtna: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Utgående anslutningar baserat på IP-adresser | För filtrering för brandväggar baserat på IP-adress, se [Azure IP-intervall](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| SSL-kontroll för utgående trafik filtreras eller är inaktiverad | Stegen för agentregistrering eller dataöverföringar kan misslyckas om SSL-kontroll eller avslutning används för utgående trafik på nätverksnivå. |
| Portar i brandväggen på servern som kör agenten. |Agenten kräver att följande brandväggsportar är öppna för att agenten ska kunna kommunicera med Azure-tjänstslutpunkter.</br></br><li>TCP port 443</li><li>TCP port 5671</li> |
| Tillåt följande webbplatser om Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat |Om Förbättrad säkerhetskonfiguration i Internet Explorer är aktiverat, måste följande webbplatser tillåtas på den server där agenten ska installeras.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Federationsservern i din organisation som är betrodd av Azure Active Directory. Exempel: https://sts.contoso.com</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>Installera Microsoft Identity Manager Reporting Agent i Azure AD
Efter att rapportagenten har installerats exporteras data från aktiviteten i Microsoft Identity Manager från MIM till Windows händelselogg. MIM-rapportagenten bearbetar händelserna och överför dem till Azure. I Azure parsas, dekrypteras och filtreras händelser för de begärda rapporterna.

1.  Installera Microsoft Identity Manager 2016.

2.  Hämta rapportagenter för Microsoft Identity Manager:

    1.  Logga in på hanteringsportalen för Azure AD och klicka på Active Directory-ikonen.

    2.  Dubbelklicka på katalogen för vilken du är global administratör och har ett Azure AD Premium-abonnemang.

    3.  Klicka på **Konfiguration** och hämta rapportagenten.

3.  Installera rapportagenten för Microsoft Identity Manager:

    1.  Ladda ned [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) till Microsoft Identity Manager Service-servern.
    2.  Kör `MIMHReportingAgentSetup.exe` 
    3.  Kör installationsprogrammet för agenten.

    4.  Se till att MIM-rapportagenttjänsten körs

    5.  Starta om MIM-tjänsten.

4.  Validera att Microsoft Identity Manager Reporting arbetar i Azure.

    Du kan generera rapportdata genom att använda portalen för Lösenordsåterställning via självbetjäning för Microsoft Identity Manager för att återställa en användares lösenord. Kontrollera att lösenordsåterställningen lyckades och kontrollera sedan att dessa data visas i hanteringsportalen för Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Visa hybridrapporter i Azure Portal

1.  Logga in på [Azure Portal](https://portal.azure.com/) med ditt globala administratörskonto för klienten.

2.  Klicka på **Azure Active Directory**-ikonen.

3.  Välj klientkatalogen i listan över tillgängliga kataloger för ditt abonnemang.

4.  Klicka på **Granskningsloggar**.

5.  Se till att du markerar **MIM-tjänst** på den nedrullningsbara menyn Kategori.

> [!WARNING]
> Det kan ta lite tid innan Microsoft Identity Manager-granskningsdata visas i Azure Portal.

## <a name="stop-creating-hybrid-reports"></a>Avbryta generering av hybridrapporter
Om du vill avbryta uppladdningen av rapportgranskningsdata från Microsoft Identity Manager till Azure Active Directory ska du avinstallera hybridrapporteringsagenten. Använd Windows-verktyget **Lägg till eller ta bort program** för att avinstallera Microsoft Identity Manager Hybrid Reporting.

## <a name="windows-events-used-for-hybrid-reporting"></a>Windows-händelser som används för hybridrapportering
Händelser som genererats av Microsoft Identity Manager loggas i Windows händelselogg och visas i Loggboken under Program- och tjänstloggar &gt; **Begärandelogg för Identity Manager**. Varje MIM-begäran exporteras som en händelse i Windows händelselogg i JSON-strukturen. Denna kan exporteras till din SIEM.

|Händelsetyp|ID|Händelseinformation|
|--------------|------|-----------------|
|Information|4121|MIM-händelsedata som inkluderar alla begärandedata.|
|Information|4137|Förlängning av MIM-händelse 4121, om det finns för många data för en enskild händelse. Rubriken för den här händelsen är i följande format: `"Request: <GUID> , message <xxx> out of <xxx>`|
