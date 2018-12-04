---
title: Konvertering av MIM specifika tjänster till gMSA | Microsoft Docs
description: Avsnittet som beskriver de grundläggande stegen för att konfigurera gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 96d375d82a71a21f0be444d628f387c4e1ffdd09
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825781"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Konvertering av MIM specifika tjänster till gMSA

Den här guiden kommer att gå igenom de grundläggande stegen för att konfigurera gMSA för tjänster som stöds. Konvertera till gMSA är lätt att när du redan har konfigurerat din miljö.

Snabbkorrigering som krävs: \<länk till senaste KB\>

Stöds:

-   MIM-synkronisering service(FIMSynchronizationService)
-   MIM Service(FIMService)
-   Registrering av MIM-lösenord
-   MIM lösenordsåterställning
-   PAM övervakning Service(PamMonitoringService)
-   PAM Component-tjänsten (PrivilegeManagementComponentService)

Stöds inte:

-   MIM-portalen finns inte stöd för den ingår i sharepoint-miljön och du behöver distribuera i läget för servergruppen och [konfigurera automatisk ändring av lösenord i SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Alla Hanteringsagenter
-   Microsoft-certifikathantering
-   BHOLD

<a name="general-information"></a>Allmän information 
--------------------

Information som behövs för att slutföra installationen och förstå

-   [Översikt över Grupphanterade tjänstkonton](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Första steget på din windows-domänkontrollant

1.  Skapa Key Distribution Services(KDS) rotnyckeln (bara en gång per domän) om det behövs. Rotnyckeln används av tjänsten KDS på domänkontrollanter (tillsammans med annan information) för att generera lösenord.

    -   Lägg till-KDSRootKey – EffectiveImmediately

    -   ”– EffectiveImmediately” innebär att vänta upp till \~10 timmar / måste replikeras till alla domänkontrollanter. Detta var ungefär en timma för två domänkontrollanter.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synkroniseringstjänst
-----------------------

1.  Skapa en grupp med namnet ”MIMSync_Servers” och lägga till alla synkroniseringsservrar i den här gruppen.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Från windows PowerShell kör du kommandot nedan som domänadministratör med datorkontot redan anslutits till domänen

    -   Ny-ADServiceAccount – namnge MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword ”MIMSync_Servers”

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Hämta information om GSMA för synkronisering:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Om PCNS-tjänsten körs, kommer du behöva uppdatera delegeringen

    -   Set-ADServiceAccount-identitet MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com”}

3. Därefter vid synkronisering måste tjänster du säkerhetskopiera krypteringsnyckeln som kommer att begäras vid ändra läge installation

    -   Leta upp verktyget synkronisering tjänsten Key Management på den Server som synkroniseringstjänsten är installerad på

    -   Som standard den **exportera nyckeluppsättning** har redan valts

    -   Klicka på **nästa**

    -   Du kommer nu att uppmanas att ange befintliga kontoinformation för synkronisering

    -   Ange och kontrollera informationen FIM-synkronisering

        -   Kontonamn - kontonamnet för synkroniseringstjänsten kontot som används under den inledande installationen

        -   Lösenord: lösenordet för tjänstkontot för synkronisering

        -   Domän - domän som Synchronization Service-kontot från varandra i

    -   Klicka på **nästa**

    -   Om du har angett något felaktigt, får du följande felmeddelande

    -   Nu du har angett informationen, visas med ett alternativ för att ändra målsökvägen (plats för exportfilen) av krypteringsnyckeln för säkerhetskopiering

        -   Plats för exportfilen är som standard **C:\\Windows\\system32**\\miiskeys 1.bin.

4. Installera synkroniseringstjänsten för Microsoft Identity Manager SP1 build 4.4.1302.0. Du hittar på Volume License Download Center eller hämta MSDN-webbplatsen. När du har slutfört installationen kan du se till att, sparar du keyset miiskeys.bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installera senaste [snabbkorrigering 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) eller senare.

- En gång korrigerade stoppa FIM-synkroniseringstjänsten.
- Styra panelen program och funktioner Microsoft Identity Manager
- Synkroniseringstjänsten ändringen -\> nästa -\> konfigurera -\> nästa

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Rensa kontonamnet
-  Skriv namnet på tjänstkontot **MIMSyncGMSA** med \$ symbolen som på den
- skärmbild. Lämna lösenordet tomt.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Nästa -\> nästa -\> installera
- Återställa keyset från .bin-filen sparas.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> SQL-behörighet har lagts till är att kontot har skapats för inloggningen därför måste du tillåta användaren tillämpa ändringen läge behörighet att lägga till kontot och dbo på synkronisering service-databas

## <a name="mim-service"></a>MIM-tjänst
-----------

>[!IMPORTANT]
>Följande process måste användas när du först konvertera MIM-tjänsten relaterade konton för att vara gMSA-konton. PowerShell-cmdletar som anges i tillägget kan bara användas för att ändra informationen när den inledande konfigurationen har done.*

1.  Skapa grupp hanterade konton för MIM-tjänsten, PAM Rest API, PAM-övervakningstjänsten, PAM Component-tjänsten, SSPR-Registreringsportalen och SSPR återställa portalen.

    -   Kontrollera att du uppdaterar gMSA delegering och SPN
        -   Set-ADServiceAccount-identitet \<konto\> - ServicePrincipalNames \@{Lägg till = ”\<SPN\>”}
        -   Delegering
            -   Set-ADServiceAccount-identitet \<gsmaaccount\> - TrustedForDelegation \$SANT
        -   Begränsad delegering
            -   \$delspns = ”http/mim', 'http/mim.contoso.com'
            -   Ny-ADServiceAccount-namnet \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<grupp\> - ServicePrincipalNames \$SPN - OtherAttributes \@{'msDS-AllowedToDelegateTo' =\$delspns}

2.  Lägg till konto för MIM-tjänsten i synkroniseringsgrupper. Det är nödvändigt för SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **OBS**.  Kända problem som tjänster som använder de hanterade kontona som låser sig efter omstart av servern på grund av Microsoft Key Distribution Service har inte startats efter omstart i Windows. Det gick inte att starta tjänsten och Windows kunde inte startas för. Problemet är reproducerbar minst på Windows Server 2012 R2. Lösning för det här problemet är att köra kommandot 

-   **SC triggerinfo kdssvc start/networkon**

    Starta Microsoft Key Distribution Service när nätverket är på (vanligtvis tidigt i Start-cykel).

    Se diskussion om liknande problem: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Kör upphöjd MSI för MIM-tjänsten och väljer ändringen.

5.  På ”Konfigurera huvudservern anslutningssidan” Markera kryssrutan ”Använd annat konto för Exchange (för hanterade konton)”. Här har du möjlighet att använda det gamla kontot som har en postlåda eller använda cloud-postlåda.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  På sidan ”Konfigurera MIM-tjänstens konto” typen tjänstkontot med \$ symbolen i slutet. Även skriva lösenordet för e-post. Lösenordet ska vara inaktiverat.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Eftersom LogonUser funktionen inte fungerar för hanterade konton, kommer nästa sida att varning ”kontrollera om är säker tjänstkontots nuvarande konfiguration”.

![CID:image007.PNG\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Ange namn på programpoolskonto med på ”Konfigurera Privileged Access Management REST API” \$ symbolen i slutet och lämna lösenordsfältet tomt.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  På ”konfigurera PAM-Komponenttjänsten”-sidan skriver du namnet på tjänstkontot med \$ symbolen i slutet och lämna lösenordsfältet tomt.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.PNG\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  På ”Konfigurera Privileged Access Management övervakningstjänsten”-sidan skriver du namnet på tjänstkontot med \$ symbolen i slutet och lämna lösenordsfältet tomt.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  På sidan ”Konfigurera MIM-portalen för Lösenordsregistrering” Skriv namnet på kontot med \$ symbolen i slutet och lämna lösenordsfältet tomt.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  På sidan ”Konfigurera MIM lösenord portalen för återställning” Skriv namnet på kontot med \$ symbolen i slutet och lämna lösenordsfältet tomt.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Slutför installationen.

Obs!

-  Efter installationen skapas två nya nycklar i registret med sökväg
    - ”HKEY_LOCAL_MACHINE\\programvara\\Microsoft\\Forefront Identity
    - Manager\\2010\\tjänst ”för att lagra krypterade Exchange-lösenord. En för
    - Exchange Online och en annan för Exchange lokalt (en av dem bör vara
    - tom).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Om du vill uppdatera lösenordet vi lagt till ett skript [ladda ned här](microsoft-identity-manager-2016-gmsascript.md) så att kunden inte behöver köra ändra läge

- För att kryptera Exchange lösenord installationsprogrammet skapar ytterligare service och
    - Kör den under kontot hanterade. Följande meddelanden kommer att läggas till i
    - Programmets händelselogg under installationen.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
