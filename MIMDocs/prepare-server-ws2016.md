---
title: Konfigurera Windows Server 2016 eller 2019 för MIM 2016 SP2 | Microsoft Docs
description: Hämta stegen och minimi kraven för att förbereda Windows Server 2016 eller 2019 för att arbeta med MIM 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c6d5d5081f0e932b9c60d8f2025b54e47dc352d5
ms.sourcegitcommit: d98a76d933d4d7ecb02c72c30d57abe3e7f5d015
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853558"
---
# <a name="set-up-an-identity-management-server-windows-server-2016-or-2019"></a>Konfigurera en server för identitets hantering: Windows Server 2016 eller 2019

> [!div class="step-by-step"]
> [«Förbereda en domän](preparing-domain.md)
> [SQL Server»](prepare-server-sql2016.md)
> 

> [!NOTE]
> Installations proceduren för Windows Server 2019 skiljer sig inte från installations proceduren för Windows Server 2016.


> [!NOTE]
> Den här genomgången använder exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Till exempel:
> - Domänkontrollantens namn – **corpdc**
> - Domännamn – **contoso**
> - Server namn för MIM-tjänst – **corpservice**
> - MIM Sync Server-namn – **corpsync**
> - SQL Server namn – **corpsql**
> - Lösenord – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Anslut Windows Server 2016 till din domän

Börja med en Windows Server 2016-dator med minst 8 – 12 GB RAM-minne. När du installerar anger du "Windows Server 2016 standard/Data Center (Server med GUI) x64" Edition.

1. Logga in på den nya datorn som administratör.

2. Ge datorn en statisk IP-adress i nätverket med hjälp av Kontrollpanelen. Konfigurera nätverks gränssnittet för att skicka DNS-frågor till domänkontrollantens IP-adress i föregående steg och ange dator namnet till **CORPSERVICE**.  Den här åtgärden kräver att servern startas om.

3. Öppna kontroll panelen och Anslut datorn till den domän som du konfigurerade i det sista steget, *contoso.com*.  Den här åtgärden omfattar att ange användar namn och autentiseringsuppgifter för en domän administratör, till exempel *contoso\administrator*.  Efter att välkomstmeddelandet har visats kan du stänga dialogrutan och starta om servern igen.

4. Logga in på datorn *CORPSERVICE* som ett domän konto med lokal dator administratör, till exempel *Contoso\MIMINSTALL*.


5. Öppna ett PowerShell-fönster som administratör och skriv följande kommando för att uppdatera datorn med inställningarna för grupprincipen.

    ```
    gpupdate /force /target:computer
    ```

    Inom mindre än en minut slutförs uppdateringen med meddelandet ”Uppdatering av grupprincip har slutförts”.

6. Lägg till roller för **Webbserver (IIS)** och **Programserver**, funktionerna för **.NET Framework** 3.5, 4.0 och 4.5 och **Active Directory-modulen för Windows PowerShell**.

    ![Bild av PowerShell-funktioner](media/MIM-DeployWS2.png)

7. Skriv följande kommandon i PowerShell. Observera att det kan vara nödvändigt att ange en annan plats för källfilerna för **.NET Framework** 3.5-funktionerna. De här funktionerna finns vanligtvis inte när du installerar Windows Server, men är tillgängliga i mappen sida-vid-sida (SxS) i mappen Sources på OS-installationsdisken, t.ex. ” *d:\Sources\SxS\* ”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Konfigurera säkerhetsprinciper för servern

Konfigurera säkerhetsprincipen för servern så att de konton som nyligen skapats kan köras som tjänster.
> [!NOTE] 
> Beroende på din konfiguration, en enskild server (alla-i-ett) eller distribuerade servrar behöver du bara lägga till, baserat på roll för medlems datorn, till exempel synkroniseringstjänst. 

1. Starta programmet för lokala säkerhetsprinciper

2. Gå till **Lokala principer > Tilldelning av användarrättigheter**.

3. I informations fönstret högerklickar du på **Logga in som en tjänst**och väljer **Egenskaper**.

    ![Bild för Lokal säkerhetsprincip](media/MIM-DeployWS3.png)

4. Klicka på **Lägg till användare eller grupp**, och i text rutan skriver du följande baserat på roll `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, klicka på **kontrol lera namn**och klicka på **OK**.

5. Klicka på **OK** för att stänga fönstret **Logga in som en tjänst Egenskaper**.

6.  I informations fönstret högerklickar **du på neka åtkomst till den här datorn från nätverket**och väljer **Egenskaper**. >

7. Klicka på **Lägg till användare eller grupp**, skriv `contoso\MIMSync; contoso\MIMService` i textrutan och klicka på **OK**.

8. Klicka på **OK** för att stänga fönstret **Neka åtkomst till den här datorn från nätverket Egenskaper**.

9. I informations fönstret högerklickar du på **Neka inloggning lokalt**och väljer **Egenskaper**.

10. Klicka på **Lägg till användare eller grupp**, skriv `contoso\MIMSync; contoso\MIMService` i textrutan och klicka på **OK**.

11. Klicka på **OK** för att stänga fönstret **Neka inloggning lokalt Egenskaper**.

12. Stäng fönstret för lokala säkerhetsprinicper.

## <a name="software-prerequisites"></a>Program varu krav

Innan du installerar MIM 2016 SP2-komponenter måste du kontrol lera att du installerar alla program varu krav:

13. Installera [Visual C++ 2013 Redistributable-paket](https://www.microsoft.com/download/details.aspx?id=40784).

14. Installera .NET Framework 4,6.

15. På den server som ska vara värd för MIM-synkroniseringstjänsten måste du [SQL Server Native Client](https://www.microsoft.com/download/details.aspx?id=50402)i MIM-synkroniseringstjänsten.

16. MIM-tjänsten kräver .NET Framework 3,5 på den server som ska vara värd för MIM-tjänsten.

17. Om du använder TLS 1,2 eller FIPS-läge, se [MIM 2016 SP2 i miljöer med endast TLS 1,2 eller FIPS-läge](preparing-tls.md).

## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Ändra läget IIS Windows-autentisering om det behövs

1.  Öppna ett PowerShell-fönster.

2.  Avbryt IIS med kommandot *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Förbereda en domän](preparing-domain.md)
> [SQL Server»](prepare-server-sql2016.md)
