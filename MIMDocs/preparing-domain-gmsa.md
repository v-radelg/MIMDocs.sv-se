---
title: Konfigurera en gMSAs för Microsoft Identity Manager 2016 | Microsoft Docs
description: Konfigurera grupphanterade tjänst konton i en domän för Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 32b346dd9cf99b617edfaca953389cba30d6681c
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043519"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Konfigurera en domän för gMSA-scenario (Group Managed Service accounts)

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

> [!IMPORTANT]
> Den här artikeln gäller endast MIM 2016 SP2.

Microsoft Identity Manager (MIM) fungerar med Active Directory-domänen (AD). Du bör redan ha en AD installerad och se till att du har en domänkontrollant i miljön för en domän du har administratörsbehörighet för.  Den här artikeln beskriver hur du konfigurerar grupphanterade tjänst konton i domänen för användning av MIM.

## <a name="overview"></a>Översikt

Grupphanterade tjänst konton eliminerar behovet av att regelbundet ändra lösen ord för tjänst konton. I och med lanseringen av MIM 2016 SP2 kan följande MIM-komponenter ha gMSA-konton konfigurerade för användning under installations processen:

-   MIM-synkroniseringstjänst (FIMSynchronizationService)
-   MIM-tjänst (FIMService)
-   Programpoolen MIM Password Registration-webbplats
-   Programpoolen MIM Password Reset-webbplats
-   Programpool för PAM REST API-webbplats
-   PAM Monitoring-tjänsten (PamMonitoringService)
-   PAM Component service (PrivilegeManagementComponentService)

Följande MIM-komponenter stöder inte körning som gMSA-konton:

-   MIM-portalen. Detta beror på att MIM-portalen är en del av SharePoint-miljön. I stället kan du distribuera SharePoint i Server grupps läge och [Konfigurera automatisk ändring av lösen ord i SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Alla hanterings agenter
-   Hantering av Microsoft-certifikat
-   BHOLD


Mer information om gMSA finns i följande artiklar:
-   [Översikt över grupphanterade tjänstkonton](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [Skapa KDS-rotnyckeln för nyckeldistributionstjänster](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Skapa användarkonton och grupper

Alla komponenter i MIM-distributionen behöver ha egna identiteter i domänen. Detta omfattar MIM-komponenter som Service och Sync, samt SharePoint och SQL.


> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Ett exempel:
> - Domänkontrollantens namn- **DC**
> - Domännamn – **contoso**
> - Server namn för MIM-tjänst – **mimservice**
> - MIM Sync Server-namn – **mimsync**
> - SQL Server namn – **SQL**
> - Ords<strong>Pass@word1</strong>

1. Logga in på domänkontrollanten som domänadministratör (*t.ex. Contoso\Administratör*).

2. Skapa följande användarkonton för MIM-tjänster. Starta PowerShell och skriv följande PowerShell-skript för att skapa nya AD-domän användare (alla konton är inte obligatoriska, även om skriptet endast tillhandahålls i informations syfte, är det bäst att använda ett dedikerat *MIMAdmin* -konto för MIM-och SharePoint-installationen).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Skapa säkerhetsgrupper för alla grupper.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Lägg till SPN-namn om du vill aktivera Kerberos-autentisering för tjänstkonton

    ```PowerShell
    setspn -S http/mim.contoso.com contoso\svcMIMAppPool
    ```

5.  Se till att registrera följande DNS "A"-poster för korrekt namn matchning (förutsatt att MIM-tjänsten, MIM-portalen, lösen ords återställning och lösen ords registrering webbplatser kommer att finnas på samma dator)

    - mim.contoso.com-peka på MIM-tjänsten och Portal serverns fysiska IP-adress
    - passwordreset.contoso.com-peka på MIM-tjänsten och Portal serverns fysiska IP-adress
    - passwordregistration.contoso.com-peka på MIM-tjänsten och Portal serverns fysiska IP-adress

## <a name="create-key-distribution-service-root-key"></a>Skapa nyckel distributions tjänstens rot nyckel

Se till att du är inloggad på domänkontrollanten som administratör för att förbereda Group Key Distribution Service.

Om det redan finns en rot nyckel för domänen (Använd **Get-KdsRootKey** för att kontrol lera) fortsätter du till nästa avsnitt.

6.  Skapa rot nyckeln Key Distribution Services (KDS) (endast en gång per domän) om det behövs. Rot nyckeln används av KDS-tjänsten på domänkontrollanter (tillsammans med annan information) för att skapa lösen ord. Skriv följande PowerShell-kommando som domän administratör:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *– EffectiveImmediately* kan kräva en fördröjning på upp till \~10 timmar eftersom den måste replikeras till alla domänkontrollanter. Den här fördröjningen var ungefär 1 timme för två domänkontrollanter.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >I labb-eller test miljö kan du undvika replikerings fördröjning på 10 timmar genom att köra följande kommando i stället:
    ><br/>
    >Add-KDSRootKey-EffectiveTime ((Hämta datum). AddHours (-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>Skapa MIM-synkroniseringstjänst, grupp och tjänstens huvud namn
-----------------------

Se till att alla dator konton för datorer där MIM-programvaran ska installeras redan är anslutna till domänen.  Utför sedan de här stegen i PowerShell som domän administratör.

7.  Skapa en grupp *MIMSync_Servers* och Lägg till alla MIM-synkroniseringsenheter i den här gruppen.
    Skriv följande för att skapa en ny AD-grupp för MIM-synkroniseringsenheter. Sedan kan du lägga till MIM-synkroniseringstjänsten Active Directory dator konton, t. ex. *contoso\MIMSync $*, i den här gruppen.

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  Skapa MIM-gMSA för synkroniseringstjänst. Skriv följande PowerShell.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Kontrol lera informationen om de GSMA som har skapats genom att köra *Get-ADServiceAccount PowerShell-* kommandot:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Om du planerar att köra tjänsten för meddelanden om lösen ords ändring måste du registrera tjänstens huvud namn genom att köra det här PowerShell-kommandot:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Starta om MIM-synkroniseringstjänsten för att uppdatera en Kerberos-token som är associerad med servern eftersom grupp medlemskapet "MIMSync_Server" har ändrats.

## <a name="create-mim-service-management-agent-service-account"></a>Skapa tjänst konto för MIM Service Management Agent

11. Normalt när du installerar MIM-tjänsten skapar du ett nytt konto för MIM-tjänstens hanterings agent (MIM MA-konto).  Med gMSA finns det två tillgängliga alternativ:

- Använd MIM-synkroniseringstjänsten för grupphanterade tjänst konton och skapa inte ett separat konto

    Du kan hoppa över skapandet av tjänst kontot för MIM-tjänstehanterings agenten. I det här fallet använder du MIM-gMSA namn, t. ex. *contoso\MIMSyncGMSAsvc $*, i stället för MIM ma-kontot när du installerar MIM-tjänsten. Senare i konfiguration av MIM-tjänstens hanterings agent aktiverar du alternativet *Använd MIMSync konto* .

    Aktivera inte "Neka inloggning från nätverket för MIM-synkroniseringstjänsten gMSA som MIM MA-kontot kräver behörigheten Tillåt nätverks inloggning.

- Använd ett reguljärt tjänst konto för tjänst kontot för MIM Service Management-agenten

    Starta PowerShell som domän administratör och skriv följande för att skapa en ny AD-domän användare:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Aktivera inte "Neka inloggning från nätverket" för MIM MA-kontot eftersom det kräver behörigheten Tillåt nätverks inloggning.

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>Skapa MIM-tjänstekonton, grupper och tjänstens huvud namn

Fortsätt att använda PowerShell som domän administratör.
   
12. Skapa en grupp *MIMService_Servers* och Lägg till alla MIM-tjänsteservrar i den här gruppen.  Skriv följande PowerShell för att skapa en ny AD-grupp för MIM-tjänsteservrar och Lägg till MIM-tjänsteserver Active Directory dator konto, t. ex. *contoso\MIMPortal $*, i den här gruppen.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  Skapa MIM-gMSA.

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Registrera tjänstens huvud namn och aktivera Kerberos-delegering genom att köra det här PowerShell-kommandot:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  För SSPR-scenarier måste du kunna kommunicera med MIM-synkroniseringstjänsten, vilket innebär att MIM-tjänstkontot måste antingen vara medlem i MIMSyncAdministrators eller MIM synkronisera lösen ords återställning och bläddra bland grupper:

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Starta om MIM-tjänsteservern för att uppdatera en Kerberos-token som är associerad med servern eftersom grupp medlemskapet "MIMService_Servers" har ändrats.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>Skapa andra MIM-konton och grupper vid behov

Om du konfigurerar MIM SSPR följer du samma rikt linjer som beskrivits ovan för MIM-synkroniseringstjänsten och MIM-tjänsten. du kan skapa andra gMSA för:

- Programpoolen MIM Password Reset-webbplats
- Programpoolen MIM Password Registration-webbplats

Om du konfigurerar MIM PAM följer du samma rikt linjer som beskrivits ovan för MIM-synkroniseringstjänsten och MIM-tjänsten. du kan skapa andra gMSA för:

- Programpool för MIM PAM-REST API webbplats
- MIM PAM Component service
- MIM PAM Monitoring-tjänsten

## <a name="specifying-a-gmsa-when-installing-mim"></a>Ange en gMSA när du installerar MIM

Som en allmän regel, i de flesta fall när du använder ett MIM-installationsprogram, för att ange att du vill använda en gMSA i stället för ett vanligt konto, lägger du till ett dollar tecken till gMSA namn, t. ex. **contoso\MIMSyncGMSAsvc $**, och lämnar fältet lösen ord tomt. Ett undantag är verktyget *miisactivate. exe* som godkänner gMSA namn utan dollar tecken.
<br/>

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)
