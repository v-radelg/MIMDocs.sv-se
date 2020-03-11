---
title: Arbeta med hybrid rapportering i Azure med hjälp av Identity Manager 2016 | Microsoft Docs
description: Lär dig hur du sammanför lokala data och molndata i hybridrapporter i Azure och hur du hanterar och visar dessa rapporter.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: fd0efd3e3d5c42f4b67d0abd42f6dab8254573e5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044352"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Arbeta med hybrid rapportering i Identity Manager

Den här artikeln beskriver hur du kombinerar lokala och molnbaserade data till hybrid rapporter i Azure och hur du hanterar och visar dessa rapporter.

## <a name="available-hybrid-reports"></a>Tillgängliga hybridrapporter
De första tre Microsoft Identity Manager-rapporterna som är tillgängliga i Azure Active Directory (Azure AD) är följande:

- **Aktivitet för lösen ords återställning**: visar varje instans när en användare utförde lösen ords återställning med självbetjäning för återställning av lösen ord (SSPR) och tillhandahåller de portar eller metoder som används för autentisering.

- **Registrering av lösen ords återställning**: visar varje tillfälle som en användare registrerar sig för SSPR och de metoder som används för att autentisera. Exempel på metoder kan vara ett mobiltelefon nummer eller frågor och svar.
   > [!NOTE]
   > För *registrerings rapporter för lösen ords återställning* görs ingen DIFFERENTIERING mellan SMS-grinden och MFA-porten. Båda betraktas som mobil telefon metoder.

- **Aktivitet för självbetjänings grupper**: visar varje försök som gjorts av någon för att lägga till eller ta bort honom från en grupp och grupp skapas.

    ![Hybridrapportering i Azure – bild för återställningsaktivitet för lösenord](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Rapporterna visar för närvarande data för upp till en månads aktivitet.
> * Den tidigare hybrid rapporterings agenten måste avinstalleras.
> * Avinstallera hybrid rapporter genom att avinstallera MIMreportingAgent. msi-agenten.

## <a name="prerequisites"></a>Förutsättningar

* Identity Manager 2016 SP1 Identity Manager-tjänsten, rekommenderad build- [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* En Azure AD Premium klient med en licensierad administratör i din katalog.

* Utgående Internet anslutning från Identity Manager-servern till Azure.

## <a name="requirements"></a>Krav
Kraven för att använda hybrid rapportering i Identity Manager visas i följande tabell:


|                                         Krav                                         |                                                                                                                                                                                                                                                                                    Beskrivning                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Hybrid rapportering är en Azure AD Premium-funktion och kräver Azure AD Premium. </br>Mer information finns i [komma igång med Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Få en [kostnads fri 30-dagars utvärderings version av Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Du måste vara global administratör för din Azure AD                     |                                                                   Som standard kan endast globala administratörer installera och konfigurera agenterna för att komma igång, få åtkomst till portalen och utföra alla åtgärder i Azure. </br>**Viktigt**: det konto som du använder när du installerar agenterna måste vara ett arbets-eller skol konto. Det kan inte vara ett Microsoft-konto. Mer information finns i [Registrera dig för Azure som en organisation](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Identity Manager-hybrid agenten installeras på varje mål server för identitets hanterare |                                                                                                                                                                                                       För att ta emot data och tillhandahålla funktioner för övervakning och analys kräver hybrid rapportering att agenterna installeras och konfigureras på mål servrar.  </br>                                                                                                                                                                                                       |
|                    Utgående anslutning till Azure-tjänstslutpunkter                     | Agenten kräver anslutning till Azure-tjänstslutpunkter under installation och körning. Om utgående anslutning blockeras av brand väggar kontrollerar du att följande slut punkter har lagts till i listan över tillåtna:<ul><li>\*. blob.core.windows.net </li><li>\*. servicebus.windows.net-port: 5671 </li><li>\*. adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Utgående anslutningar baserat på IP-adresser                         |                                                                                                                                                                                                                      För IP-adresser baserade på filtrering av brand väggar, se [Azure IP-intervallen](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 SSL-kontroll för utgående trafik filtreras eller inaktive ras                 |                                                                                                                                                                                                               Det kan hända att agent registrerings steget eller data överförings åtgärderna Miss söker om SSL-kontroll eller avslutning av utgående trafik på nätverks nivå.                                                                                                                                                                                                                |
|                      Brand Väggs portar på servern som kör agenten                       |                                                                                                                                                                                                          För att kommunicera med Azure-tjänstens slut punkter kräver agenten att följande brand Väggs portar är öppna:<ul><li>TCP port 443</li><li>TCP port 5671</li></ul>                                                                                                                                                                                                          |
|          Tillåt vissa webbplatser om förbättrad säkerhet i Internet Explorer är aktiverat           |                                                                                Om förbättrad säkerhet i Internet Explorer är aktiverat måste följande webbplatser tillåtas på den server där agenten är installerad:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>Federations servern för din organisation är betrodd av Azure Active Directory (till exempel <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Installera rapporterings agenten för Identity Manager i Azure AD
När rapport agenten har installerats exporteras data från Identity Manager-aktiviteten från Identity Manager till Windows-händelseloggen. Identity Manager repor ting agent bearbetar händelserna och laddar upp dem till Azure. I Azure parsas, dekrypteras och filtreras händelser för de begärda rapporterna.

1.  Installera Identity Manager 2016.

2.  Hämta rapport agenten för Identity Manager och gör sedan följande:

    a. Logga in på hanterings portalen för Azure AD och välj sedan **Active Directory**.

    b. Dubbelklicka på den katalog som du är global administratör för och be en Azure AD Premium prenumeration.

    c. Välj **konfiguration**och sedan ladda ned rapport agent.

3.  Installera rapport agenten genom att göra följande:

    a.  Hämta [filen MIMHReportingAgentSetup. exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) för Identity Manager-hanteringsservern.

    b.  Kör `MIMHReportingAgentSetup.exe`. 

    c.  Kör installationsprogrammet för agenten.

    d.  Kontrol lera att Identity Manager repor ting Agent-tjänsten körs.

    e.  Starta om tjänsten Identity Manager.

4.  Kontrol lera att Identity Manager repor ting agent fungerar i Azure.

    Du kan skapa rapport data med hjälp av självbetjänings portalen för lösen ords återställning i identitets hanteraren för att återställa en användares lösen ord. Kontrol lera att lösen ords återställningen har slutförts och kontrol lera sedan att data visas i hanterings portalen för Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Visa hybrid rapporter i Azure Portal

1.  Logga in på [Azure Portal](https://portal.azure.com/) med ditt globala administratörs konto för klienten.

2.  Välj **Azure Active Directory**.

3.  Välj klient katalogen i listan över tillgängliga kataloger för din prenumeration.

4.  Välj **gransknings loggar**.

5.  I list rutan **kategori** kontrollerar du att **MIM-tjänsten** är vald.

> [!IMPORTANT]
> Det kan ta lite tid för gransknings data i identitets hanteraren att visas i Azure Portal.

## <a name="stop-creating-hybrid-reports"></a>Avbryta generering av hybridrapporter
Avinstallera hybrid rapporterings agenten om du vill avbryta uppladdningen av rapport gransknings data från Identity Manager till Azure AD. Använd verktyget Windows Lägg till eller ta bort program för att avinstallera hybrid rapportering i Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Windows-händelser som används för hybridrapportering
Händelser som genereras av identitets hanteraren lagras i händelse loggen i Windows. Du kan visa händelserna i **Loggboken** genom att välja **program-och tjänst loggar** > **identitets hanterarens begär ande logg**. Varje begäran om identitets hanteraren exporteras som en händelse i händelse loggen i Windows i JSON-strukturen. Du kan exportera resultatet till SIEM-systemet (Security information and Event Management).

|Händelsetyp|ID|Händelseinformation|
|--------------|------|-----------------|
|Information|4121|Händelse data i identitets hanteraren som innehåller alla data för begäran.|
|Information|4137|Identity Manager Event 4121-tillägget, om det finns för mycket data för en enskild händelse. Rubriken i den här händelsen visas i följande format: `"Request: <GUID> , message <xxx> out of <xxx>`.|
