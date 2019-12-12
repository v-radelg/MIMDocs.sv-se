---
title: Konvertering av MIM-/regionsspecifika tjänster till gMSA | Microsoft Docs
description: Avsnitt som beskriver de grundläggande stegen för att konfigurera gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 96d375d82a71a21f0be444d628f387c4e1ffdd09
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/05/2019
ms.locfileid: "64520626"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Konvertering av MIM-/regionsspecifika tjänster till gMSA

Den här guiden vägleder dig genom de grundläggande stegen för att konfigurera gMSA för tjänster som stöds. Processen att konvertera till gMSA är enkel när du förkonfigurerar din miljö.

Snabb korrigering krävs: \<länk till senaste KB-\>

Stöds:

-   MIM-synkroniseringstjänst (FIMSynchronizationService)
-   MIM-tjänst (FIMService)
-   MIM-lösenords registrering
-   MIM-lösenord Återställ
-   PAM Monitoring-tjänsten (PamMonitoringService)
-   PAM Component service (PrivilegeManagementComponentService)

Stöds inte:

-   MIM-portalen stöds inte eftersom den ingår i SharePoint-miljön och du måste distribuera i Server grupps läge och [Konfigurera automatisk ändring av lösen ord i sharepointserver](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Alla hanterings agenter
-   Hantering av Microsoft-certifikat
-   BHOLD

<a name="general-information"></a>Allmän information 
--------------------

Läsning krävs för att slutföra installationen och förstå

-   [Översikt över grupphanterade tjänst konton](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Första steget på din Windows-domänkontrollant

1.  Skapa rot nyckeln Key Distribution Services (KDS) (endast en gång per domän) om det behövs. Rot nyckeln används av KDS-tjänsten i DCs (tillsammans med annan information) för att skapa lösen ord.

    -   Add-KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" innebär att vänta upp till \~10 timmar/behöver replikeras till alla domänkontrollanter. Detta var ungefär 1 timme för två domänkontrollanter.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synkroniseringstjänst
-----------------------

1.  Skapa en grupp med namnet "MIMSync_Servers" och Lägg till alla synkroniseringsenheter i den här gruppen.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Från Windows PowerShell kör du följande kommando som domän administratör med dator kontot som redan är anslutet till domänen

    -   New-ADServiceAccount-Name MIMSyncGMSAsvc-DNSHostName MIMSyncGMSAsvc.contoso.com-PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Hämta information om GSMA för synkronisering:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Om du kör PCNS-tjänsten måste du uppdatera delegeringen

    -   Set-ADServiceAccount-Identity MIMSyncGMSAsvc-ServicePrincipalNames \@{Add = "PCNSCLNT/mimsync. contoso. com"}

3. Härnäst på synkroniseringstjänsten måste du säkerhetskopiera krypterings nyckeln eftersom den kommer att begäras vid installation av ändrings läge

    -   På den server som synkroniseringstjänsten är installerad på Letar du reda på verktyget nyckel hantering för synkroniseringstjänst

    -   Som standard har **export nyckel uppsättningen** redan valts

    -   Klicka på **Nästa**

    -   Nu uppmanas du att ange den befintliga informationen om synkronisering av konto

    -   Ange och verifiera konto informationen för FIM-synkronisering

        -   Konto namn – konto namnet för det synkroniseringstjänst som används under den inledande installationen

        -   Lösen ord – lösen ord för kontot för synkroniseringstjänsten

        -   Domän-domän som kontot för synkroniseringstjänsten för tjänsten är skilt från

    -   Klicka på **Nästa**

    -   Om du har angett något felaktigt visas följande fel

    -   Nu när du har angett konto informationen visas ett alternativ för att ändra destinationen (platsen för export filen) för säkerhets kopieringens krypterings nyckel

        -   Som standard är export fil platsen **C:\\Windows\\system32**\\miiskeys-1. bin.

4. Installera Microsoft Identity Manager SP1-synkroniseringstjänsten för 4.4.1302.0. du hittar du på volym licens hämtnings Center eller MSDN hämtnings plats. När du har slutfört installationen ser du till att du sparar nyckel uppsättningen miiskeys. bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installera senaste [snabb korrigering 4.5. x. x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) eller senare.

- När korrigeringen stoppa FIM-synkroniseringstjänsten.
- Program och funktioner i kontroll panelen Microsoft Identity Manager
- Ändring av synkroniseringstjänsten-\> nästa\> konfigurera-\> nästa

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Rensa konto namnet
-  Skriv tjänst konto namnet **MIMSyncGMSA** med \$ symbol som på
- skärm bild. Lämna lösen ord tomt.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Nästa steg\> nästa steg\> installera
- Återställ nyckel uppsättningen från. bin-filen Sparad.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> En SQL-behörighet har lagts till kontot har skapats för inloggning. därför måste du tillåta att användaren använder behörigheten Ändra läge för att lägga till kontot och dbo i databasen för synkroniseringstjänsten

## <a name="mim-service"></a>MIM-tjänst
-----------

>[!IMPORTANT]
>Följande process måste användas när du först konverterar MIM-tjänstens relaterade konton till gMSA-konton. PowerShell-cmdletarna som anges i bilagan kan bara användas för att ändra konto informationen när den första konfigurationen har gjorts. *

1.  Skapa grupp hanterade konton för MIM-tjänsten, PAM REST API, PAM Monitoring service, PAM Component service, SSPR Registration Portal, SSPR reset Portal.

    -   Kontrol lera att du uppdaterar gMSA delegering och SPN
        -   Set-ADServiceAccount-Identity \<Account\>-ServicePrincipalNames \@{Add = "\<SPN\>"}
        -   Delegering
            -   Set-ADServiceAccount-Identity \<gsmaaccount\>-TrustedForDelegation \$True
        -   Begränsad delegering
            -   \$delspns = ' http/MIM ', ' http/mim. contoso. com '
            -   New-ADServiceAccount-Name \<gsmaaccount\>-DNSHostName \<gsmaaccount\>. contoso.com-PrincipalsAllowedToRetrieveManagedPassword \<Group\>-ServicePrincipalNames \$SPN-OtherAttributes \@{' msDS-AllowedToDelegateTo ' =\$delspns}

2.  Lägg till kontot för MIM-tjänsten i Sync-grupper. Det är nödvändigt för SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **Obs!**  Känt problem som tjänster som använder hanterat konto låser sig efter omstart av servern på grund av att Microsoft Key Distribution Service inte har startats efter omstart av Windows. Det gick inte att starta tjänsten och det gick inte att starta om Windows. Problemet kan reproduceras minst på Windows Server 2012 R2. Lösning för det här problemet är kör kommando 

-   **SC triggerinfo kdssvc start/networkon**

    Starta Microsoft Key Distribution Service när nätverket är på (normalt tidigt i Start cykeln).

    Se diskussionen om liknande problem: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Kör förhöjd MSI av MIM-tjänsten och välj Ändra.

5.  Markera kryss rutan Använd ett annat konto för Exchange (för hanterade konton) på sidan Konfigurera huvud server anslutning. Här kan du välja att använda det gamla kontot som har en post låda eller med hjälp av en moln post låda.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  På sidan "Konfigurera MIM-tjänstkontot" anger du tjänst kontot med \$ symbol i slutet. Skriv också lösen ordet för tjänsten e-postkontot. Lösen ordet för tjänst kontot bör inaktive ras.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Eftersom LogonUser-funktionen inte fungerar för hanterade konton visas nästa sida varning "kontrol lera om tjänst kontot är säkert i den aktuella konfigurationen".

![CID: image007. png\@01D36EB 7.562 E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  På sidan "Konfigurera Privileged Access Management REST API" anger du konto namnet för programpool med \$ symbol i slutet och lämnar fältet lösen ord tomt.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  På sidan Konfigurera PAM-komponentens tjänst konto namn med \$ symbol i fältet sluta och lämna lösen ordet tomt.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID: image010. png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  På sidan "konfigurera tjänsten för Privileged Access Management övervakning" anger du tjänst konto namnet med \$ symbol i slutet och lämnar fältet lösen ord tomt.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  På sidan "Konfigurera MIM-portalen för lösen ords registrering" på sidan Skriv konto namn med \$ symbol i fältet sluta och lämna lösen ordet tomt.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  På sidan "Konfigurera MIM-portalen för lösen ords återställning", sid typs konto namn med \$ symbol i fältet sluta och lämna lösen ordet tomt.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Slutför installationen.

Anm

-  Efter installationen skapas två nya nycklar i registret efter sökväg
    - "HKEY_LOCAL_MACHINE\\program vara\\Microsoft\\Forefront Identity
    - Manager\\2010\\-tjänsten "för att lagra krypterade Exchange-lösenord. En för
    - Exchange Online och en annan för lokal Exchange (en av dem bör vara
    - Tom).

![CID: image014. jpg\@01D36F 53.303 D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- För att uppdatera lösen ordet tillhandahöll vi ett skript som [hämtas här](microsoft-identity-manager-2016-gmsascript.md) så att kunden inte behöver köra ändrings läget

- För att kryptera Exchange-lösenordet skapar installations programmet ytterligare tjänst och
    - kör det under det hanterade kontot. Följande meddelanden kommer att läggas till i
    - Program händelse logg under installationen.

![CID: image016. jpg\@01D36F 53.303 D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
