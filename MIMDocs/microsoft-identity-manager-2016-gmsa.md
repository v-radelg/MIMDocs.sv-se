---
title: Konvertering av specifika MIM-tjänster till gMSA | Microsoft Docs
description: Avsnitt som beskriver de grundläggande stegen för att konfigurera gMSA.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: 090e82cac6c734beb9767e2d2e6230320e44c26f
ms.sourcegitcommit: c6cb2556bb9f2256b959a3c95db7ca5bbfc2b437
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37065150"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Konvertering av specifika MIM-tjänster till gMSA

Den här guiden kommer att gå igenom de grundläggande stegen för att konfigurera gMSA för tjänster som stöds. Processen för att konvertera till gMSA är enkelt när du konfigurera din miljö.

Snabbkorrigering som krävs: \<länk till senaste KB\>

Stöds:

-   MIM-synkronisering service(FIMSynchronizationService)
-   MIM Service(FIMService)
-   Registrering av MIM-lösenord
-   MIM lösenordsåterställning
-   PAM övervakning Service(PamMonitoringService)
-   PAM Component-tjänsten (PrivilegeManagementComponentService)

Stöds inte:

-   MIM-portalen stöds inte den ingår i sharepoint-miljön och du vill distribuera i servergruppsläge och [konfigurera automatisk ändring av lösenord i SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Alla Hanteringsagenter
-   Hantering av Microsoft-certifikat
-   BHOLD

<a name="general-information"></a>Allmän information 
--------------------

Läsning som behövs för att slutföra installationen och förstå

-   [Översikt över Grupphanterade tjänstkonton](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/en-us/library/jj128430(v=ws.11).aspx>

Första gången steg på din windows-domänkontrollant

1.  Skapa Distribution Services(KDS) rotnyckeln (endast en gång per domän) om det behövs. Rotnyckeln används av tjänsten KDS på domänkontrollanter (tillsammans med annan information) för att generera lösenord.

    -   Lägg till KDSRootKey – EffectiveImmediately

    -   ”– EffectiveImmediately” betyder att vänta i upp till \~10 timmar / måste replikeras till alla domänkontrollanter. Detta var ungefär en timme för två domänkontrollanter.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synkroniseringstjänst
-----------------------

1.  Fist steget skapar ett grupp-anrop ”MIMSync_Servers” och lägga till alla synkroniseringsservrar i den här gruppen.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Från windows PowerShell kör du under kommandot som domänadministratör med datorkontot redan anslutits till domänen

    -   Nya-ADServiceAccount-Name MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword ”MIMSync_Servers”

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Hämta information om GSMA för synkronisering:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Om PCNS-tjänsten körs, behöver du uppdatera delegeringen

    -   Set-ADServiceAccount-identitet MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com”}

3. Nästa vid synkronisering bör tjänster du säkerhetskopiera krypteringsnyckeln som kommer att begäras vid ändra läge installation

    -   Hitta hanteringsverktyg för synkronisering Service nyckeln på den Server som synkroniseringstjänsten är installerad på

    -   Som standard den ** Export viktiga set ** är redan markerat

    -   Klicka på **nästa**

    -   Nu uppmanas du att ange den befintliga kontoinformationen för synkronisering

    -   Ange och Bekräfta kontoinformationen för FIM-synkronisering

        -   Kontonamn - kontonamnet för synkroniseringstjänsten kontot som används under den inledande installationen

        -   Lösenord - lösenord för tjänstkonto för synkronisering

        -   Domän - domän som kontot synkronisering från varandra i

    -   Klicka på **nästa**

    -   Om du angett något fel får du följande felmeddelande

    -   Nu du har angett kontoinformationen, visas med ett alternativ för att ändra mål (plats för exportfilen) av krypteringsnyckeln för säkerhetskopiering

        -   Plats för exportfilen är som standard **C:\\Windows\\system32**\\miiskeys 1.bin.

4. Installera Microsoft Identity Manager SP1-synkroniseringstjänsten skapa 4.4.1302.0. Du hittar på volym licens Download Center eller MSDN hämta plats. När du har slutfört installationen Kontrollera kan du spara nyckeluppsättningen miiskeys.bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installera senaste [snabbkorrigering 4.5.x.x](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history) eller senare.

- En gång korrigeras stoppa FIM-synkroniseringstjänsten.
- Styra Kontrollpanelen program och funktioner Microsoft Identity Manager
- Synkroniseringstjänsten ändra -\> nästa -\> konfigurera -\> nästa

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Ta bort namnet på kontot
-  Skriv namnet på tjänstkontot **MIMSyncGMSA** med \$ symbol som på den
- skärmbild. Lämna lösenordet tomt.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Nästa -\> nästa -\> installera
- Återställa nyckeluppsättningen från .bin-filen sparas.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> SQL-behörighet som lagts till är kontot har skapats för inloggningen därför måste du tillåta användaren tillämpa ändringen läge behörighet att lägga till kontot och dbo på tjänstdatabasen synkronisering

## <a name="mim-service"></a>MIM-tjänst
-----------

>[!IMPORTANT]
>Följande process måste användas när först konvertera MIM-tjänsten relaterade konton ska gMSA-konton. PowerShell-cmdlets som anges i tillägget kan bara användas för att ändra kontoinformationen när den inledande konfigurationen har done.*

1.  Skapa grupp hanterade konton för MIM-tjänsten, PAM Rest API, PAM-övervakningstjänsten, PAM Component-tjänsten, SSPR-Registreringsportalen och SSPR Återställ Portal.

    -   Kontrollera att du uppdaterar gMSA delegering och SPN
        -   Set-ADServiceAccount-identitet \<konto\> - ServicePrincipalNames \@{Lägg till = ”\<SPN\>”}
        -   Delegering
            -   Set-ADServiceAccount-identitet \<gsmaaccount\> - TrustedForDelegation \$SANT
        -   Begränsad delegering
            -   \$delspns = ”http/mim', 'http/mim.contoso.com'
            -   Nya ADServiceAccount-namnet \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<grupp\> - ServicePrincipalNames \$SPN - OtherAttributes \@{'msDS-AllowedToDelegateTo' =\$delspns}

2.  Lägg till kontot för MIM-tjänsten i synkroniseringsgrupper. Det är nödvändigt för SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **OBS**.  Kända problem som tjänster som använder hanterade kontot låser sig efter omstart av servern på grund av Microsoft Key Distribution Service är inte startad efter omstart av Windows. Det gick inte att starta tjänsten och Windows kunde inte startas för. Problemet är reproduceras minst på Windows Server 2012 R2. Lösning på problemet är att köra kommando 

-   **SC triggerinfo kdssvc start/networkon**

    Starta Microsoft Key Distribution Service när nätverket är på (vanligtvis tidig start cykel).

    Visa diskussion om liknande problem: <https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Kör utökade MSI av MIM-tjänsten och välj ändring.

5.  I ”Konfigurera huvudserver anslutningssidan” kryssrutan ”Använd annat konto för Exchange (för hanterade tjänstkonton)”. Här har du möjlighet att använda det gamla kontot som har en postlåda eller använda molnet postlåda.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  På sidan ”Konfigurera MIM-tjänstens konto” typ tjänstkontot med \$ symbolen i slutet. Också ange lösenord för tjänstkonto e-post. Lösenord för tjänstkonto ska inaktiveras.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Eftersom LogonUser funktionen inte fungerar för hanterade konton, kommer nästa sida att varning ”kontrollera om kontot är säker i dess aktuella konfiguration”.

![CID:image007.PNG\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Ange namn på programpoolskonto med på ”Konfigurera Privileged Access Management REST-API” \$ symbol i slutet och lämna fältet tomt.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  På sidan ”Konfigurera PAM-Komponenttjänsten” skriver du namnet på tjänstkontot med \$ symbol i slutet och lämna fältet tomt.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.PNG\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  På sidan ”Konfigurera Privileged Access Management övervakning tjänst” skriver du namnet på tjänstkontot med \$ symbol i slutet och lämna fältet tomt.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  På sidan ”Konfigurera MIM portalen för Lösenordsregistrering” Ange kontonamn med \$ symbol i slutet och lämna fältet tomt.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Skriv på Konfigurera MIM lösenord återställa-Portal ”kontonamn med \$ symbol i slutet och lämna fältet tomt.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Slutför installationen.

Obs!

-  Efter installationen skapas två nya nycklar i registret med sökväg
    - ”HKEY_LOCAL_MACHINE\\programvara\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service ”för att lagra krypterade Exchange-lösenord. En för
    - Exchange Online och en annan för Exchange lokalt (en av dem ska vara
    - tom).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Om du vill uppdatera lösenordet vi tillhandahöll ett skript [hämta här](microsoft-identity-manager-2016-gmsascript.md) så kunden inte behöver köra ändra läge

- För att kryptera Exchange lösenord installationsprogrammet skapar ytterligare tjänst och
    - körs det under det hanterade kontot. Följande meddelanden kommer att läggas till i
    - Programhändelseloggen under installationen.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
