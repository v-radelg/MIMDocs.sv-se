---
title: Konvertera Microsoft Identity Manager-/regionsspecifika tjänster till gMSA | Microsoft Docs
description: Den här artikeln innehåller förutsättningarna och grundläggande steg för att konfigurera ett grupphanterat tjänst konto (gMSA).
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 4586b9998a9526a867ffe7ace9489fe56fff146c
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044216"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Konvertera Microsoft Identity Manager-/regionsspecifika tjänster för att använda grupphanterade tjänst konton

Den här artikeln är en guide för att konfigurera Microsoft Identity Manager tjänster som stöds för att använda grupphanterade tjänst konton (gMSA). När du har förkonfigurerat din miljö är det enkelt att konvertera till gMSA.

## <a name="prerequisites"></a>Krav

- Hämta och installera följande nödvändiga snabb korrigering: [Microsoft Identity Manager 4.5.26.0 eller senare](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Tjänster som stöds:

    -   Microsoft Identity Manager synkroniseringstjänst (FIMSynchronizationService)
    -   Microsoft Identity Manager tjänst (FIMService)
    -   Microsoft Identity Manager lösen ords registrering
    -   Microsoft Identity Manager lösen ords återställning
    -   Privileged Access Management (PAM) övervaknings tjänsten (PamMonitoringService)
    -   PAM Component service (PrivilegeManagementComponentService)

    Tjänster som inte stöds:

    -   Microsoft Identity Manager-portalen stöds inte. Den ingår i SharePoint-miljön och du måste distribuera den i Server grupps läge och [Konfigurera automatisk ändring av lösen ord i SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Alla hanterings agenter förutom Microsoft Identity Manager tjänst hanterings agent
    -   Hantering av Microsoft-certifikat
    -   BHOLD

- Information om bakgrund och allmän referensinformation om hur du konfigurerar din miljö finns i: 

    -   [Översikt över grupphanterade tjänst konton](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Innan du börjar [skapar du nyckel distributions tjänstens rot nyckel](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) på Windows-domänkontrollanten. Tänk på följande:  

    - Rot nycklar används av KDS-tjänsten (Key Distribution Services) för att skapa lösen ord och annan information på domänkontrollanter.
    - Skapa bara en rot nyckel en gång per domän, om det behövs.  
    - Inkludera `Add-KDSRootKey –EffectiveImmediately`. "– EffectiveImmediately" innebär att det kan ta upp till 10 timmar att replikera rot nyckeln till alla domänkontrollanter. Det kan ta ungefär en timme att replikera till två domänkontrollanter. 
    ![Strängen "– EffectiveImmediately"](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>Åtgärder som ska köras på Active Directory-domän kontrollanten

1.  Skapa en grupp med namnet *MIMSync_Servers*och Lägg till alla synkroniseringsenheter till den.

    ![Skapa en MIMSync_Servers grupp](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Logga in på Windows PowerShell som *domän administratör* med ett konto som redan är anslutet till domänen och kör sedan följande kommando: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![Kommandot i PowerShell](media/f6deb0664553e01bcc6b746314a11388.png)

    - Visa information om gMSA för synkronisering:  
     ![Information om gMSA för synkronisering](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Om du kör PCNS (Password Change Notification Service) uppdaterar du delegeringen genom att köra följande kommando:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>Åtgärder som ska köras på den Microsoft Identity Manager-synkroniseringstjänsten

1. Säkerhetskopiera krypterings nyckeln i Synchronization Service Manager. Den kommer att begäras med installationen av ändrings läget. Gör följande:

    a. På den server som Synchronization Service Manager är installerad på Letar du reda på verktyget för nyckel hantering i synkroniseringstjänst. **Export nyckel uppsättningen** är redan markerad som standard.

    b. Välj **Nästa**. 
    
    c. I prompten anger och verifierar du Microsoft Identity Manager eller kontot för synkroniseringstjänsten i Forefront Identity Manager (FIM):

    -   **Konto namn**: namnet på kontot för synkroniseringstjänsten som används under den första installationen.  
    -   **Lösen ord**: lösen ordet för kontot för synkroniseringstjänsten.  
    -   **Domän**: den domän som kontot för synkroniseringstjänsten är en del av.

    d. Välj **Nästa**.

    Om du har angett konto informationen har du möjlighet att ändra målet eller exportera fil platsen för säkerhets kopieringens krypterings nyckel. Som standard är export filens plats *C:\Windows\system32\miiskeys-1.bin*.

1. Installera Microsoft Identity Manager SP1, som du hittar i Volume Licensing Service Center eller MSDN downloads site. När du har slutfört installationen sparar du nyckel uppsättningen *miiskeys. bin*.

   ![Installations förlopps fönstret för Microsoft Identity Manager-synkroniseringstjänsten](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Installera [Hotfix 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) eller senare.

1. När korrigeringen har installerats stoppar du FIM-synkroniseringstjänsten genom att göra följande:

   a. I kontroll panelen väljer du **program och funktioner** > **Microsoft Identity Manager**.  
   b. På sidan **synkroniseringstjänst** väljer du **ändra** > **Nästa**.  
   c. I fönstret **underhålls alternativ** väljer du **Konfigurera**.

   ![Fönstret underhålls alternativ](media/dc98c011bec13a33b229a0e792b78404.png)

   d. I fönstret **konfigurera Microsoft Identity Manager synkroniseringstjänst** rensar du standardvärdet i rutan **tjänst konto** och anger sedan **MIMSyncGMSA $**. Se till att ta med symbolen dollar tecken ($), som du ser i följande bild. Lämna rutan **lösen ord** tom.

   ![Fönstret Konfigurera Microsoft Identity Manager synkroniseringstjänst](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Välj **Nästa** > **Nästa** > **installation**.  
   f. Återställ nyckel uppsättningen från *miiskeys. bin* -filen som du sparade tidigare.

   ![Alternativ för att återställa nyckel uppsättnings konfigurationen](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![Listan med hanterings agenter i Synchronization Service Manager](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Microsoft Identity Manager tjänst

>[!IMPORTANT]
>Följ anvisningarna i det här avsnittet noggrant när du konverterar Microsoft Identity Manager tjänsterelaterade konton till gMSA-konton.

1. Skapa grupphanterade konton för Microsoft Identity Manager tjänst, PAM REST-API: et, PAM Monitoring-tjänsten, PAM Component service, SSPR-registrerings portalen (självbetjäning för återställning av lösen ord) och SSPR reset Portal.

    -   Uppdatera gMSA-delegering och tjänstens huvud namn (SPN):

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Vidare

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Begränsad delegering:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Lägg till ett konto för Microsoft Identity Manager-tjänsten i Sync-grupper. Det här steget är nödvändigt för SSPR.

    ![Fönstret Active Directory användare och datorer](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > Ett känt problem i Windows Server 2012 R2 är att tjänster som använder ett hanterat konto låser sig efter att servern har startats om, eftersom Microsoft Key Distribution Service inte har startats efter att Windows har startats om. Lösningen för det här problemet är att köra följande kommando: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > Kommandot startar Microsoft Key Distribution Service när nätverket är på (normalt tidigt i Start cykeln).
    >
    > En diskussion om ett liknande problem finns [AD FS Windows 2012 R2: adfssrv låser sig i Start läge](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS).

1.  Kör förhöjd MSI för Microsoft Identity Manager-tjänst och välj **ändra**.

1.  I fönstret **Konfigurera anslutning till e-postserver** markerar du kryss rutan **Använd annan användare för Exchange (för hanterade konton)** . Du får möjlighet att använda det aktuella Exchange-kontot eller moln post lådan.

    >[!NOTE]
    >Om du väljer alternativet **Använd Exchange Online** för att aktivera Microsoft Identity Manager tjänst för att bearbeta godkännande svar från Microsoft Identity Manager Outlook-tillägg, anger du register nyckeln **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** -värdet för *PollExchangeEnabled* till **1** efter installationen.
    
    ![Fönstret "Konfigurera anslutning till e-postserver"](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  I fönstret **Konfigurera MIM-tjänstkontot** , i rutan namn på **tjänst konto** , anger du namnet. Se till att ta med symbolen dollar tecken ($). Ange också ett lösen ord i rutan **lösen ord för e-postkonto för tjänst** . Rutan **lösen ord för tjänst konto** ska vara otillgänglig.

    ![Fönstret "Konfigurera MIM-tjänstkontot"](media/db0d543df6e1b0174a47135617c23fcb.png)

    Eftersom LogonUser-funktionen inte fungerar för hanterade konton visar nästa sida varningen "kontrol lera om tjänst kontot är säkert i den aktuella konfigurationen."

    ![Varnings fönster för konto säkerhet](media/d350bc13751b2d0a884620db072ed019.png)

1.  I fönstret **konfigurera Privileged Access Management REST API** , i rutan **programpoolens konto namn** , anger du konto namnet. Se till att ta med symbolen dollar tecken ($). Rutan lämna **lösen ord för programpoolen** är tom.

    ![Fönstret Konfigurera Privileged Access Management REST API](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  I fönstret **Konfigurera pam-Komponenttjänster** i rutan **namn på tjänst konto** anger du konto namnet. Se till att ta med symbolen dollar tecken ($). Lämna rutan **lösen ord för tjänst konto** tomt.

    ![Fönstret Konfigurera PAM-Komponenttjänster](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![Fönstret konto säkerhets varning](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  I fönstret **konfigurera Privileged Access Management övervaknings tjänst** skriver du namnet på tjänst kontot i rutan **namn på tjänst konto** . Se till att ta med symbolen dollar tecken ($). Lämna rutan **lösen ord för tjänst konto** tomt.

    ![Fönstret Konfigurera Privileged Access Management övervaknings tjänsten](media/d1e824248edf12a77fc9ffb011475164.png)

1.  I fönstret **Konfigurera MIM-portalen för lösen ords registrering** i rutan **konto namn** anger du konto namnet. Se till att ta med symbolen dollar tecken ($). Lämna rutan **lösen ord** tom.

    ![Fönstret Konfigurera MIM-portalen för lösen ords registrering](media/601e935cdfda298b61ae753a2a152996.png)

1.  I fönstret **Konfigurera MIM-portalen för lösen ords återställning** anger du kontots namn i rutan **konto namn** . Se till att ta med symbolen dollar tecken ($). Lämna rutan **lösen ord** tom.

    ![Fönstret Konfigurera MIM-portalen för lösen ords återställning](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Slutför installationen.

    > [!NOTE]
    > Under installationen skapas två nya nycklar i register Sök vägen **HKEY_LOCAL_MACHINE \Software\microsoft\forefront Identity Manager\2010\Service** för att lagra det krypterade Exchange-lösenordet. En post är för *ExchangeOnline*och den andra är för *ExchangeOnPremise*. Värdet i kolumnen **data** måste vara tomt för en av posterna.

    > ![Registereditorn](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

Om du vill uppdatera lösen ordet till dina lagrade konton utan att behöva köra ändrings läget, [laddar du ned det här PowerShell-skriptet](microsoft-identity-manager-2016-gmsascript.md).

För att kryptera Exchange-lösenordet skapar installations programmet en ytterligare tjänst och kör den under det hanterade kontot. Följande meddelanden läggs till i **program** händelse loggen under installationen:

![Fönstret Loggboken](media/95b315454705cd4d939b55ac5ad910f5.jpg)
