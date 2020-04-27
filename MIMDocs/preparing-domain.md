---
title: Konfigurera en domän för Microsoft Identity Manager 2016 | Microsoft Docs
description: Skapa en Active Directory-domänkontrollant innan du installerar MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 70ac40d38af06e6be98e00caf1dc73630b62334e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043485"
---
# <a name="set-up-a-domain"></a>Konfigurera en domän

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

Microsoft Identity Manager (MIM) fungerar med Active Directory-domänen (AD). Du bör redan ha en AD installerad och se till att du har en domänkontrollant i miljön för en domän du har administratörsbehörighet för.

Den här artikeln vägleder dig igenom stegen för att förbereda domänen så att den fungerar tillsammans med MIM.

## <a name="create-user-accounts-and-groups"></a>Skapa användarkonton och grupper

Alla komponenter i MIM-distributionen behöver ha egna identiteter i domänen. Detta omfattar MIM-komponenter som Service och Sync, samt SharePoint och SQL.

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Ett exempel:
> - Domänkontrollantens namn – **corpdc**
> - Domännamn – **contoso**
> - Server namn för MIM-tjänst – **corpservice**
> - MIM Sync Server-namn – **corpsync**
> - SQL Server namn – **corpsql**
> - Ords<strong>Pass@word1</strong>

1. Logga in på domänkontrollanten som domänadministratör (*t.ex. Contoso\Administratör*).

2. Skapa följande användarkonton för MIM-tjänster. Starta PowerShell och skriv följande PowerShell-skript för att uppdatera domänen.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMINSTALL
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMpool –name MIMpool
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Skapa säkerhetsgrupper för alla grupper.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  Lägg till SPN-namn om du vill aktivera Kerberos-autentisering för tjänstkonton

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  Under installationen måste du lägga till följande DNS-poster för korrekt namn matchning

- mim.contoso.com pekar på corpservice fysiska IP-adress
- passwordreset.contoso.com pekar på corpservice fysiska IP-adress
- passwordregistration.contoso.com pekar på corpservice fysiska IP-adress

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)
