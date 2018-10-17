---
title: Konfigurera Windows Server 2016 för MIM 2016 SP1 | Microsoft Docs
description: Hämta stegen och minimikraven för att förbereda Windows Server 2016 för att arbeta med MIM 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 49e549913a5fd87528df2205b8d5b0a83f3d2b24
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358252"
---
# <a name="set-up-an-identity-management-servers-windows-server-2016"></a>Konfigurera en identity-hanteringsservrar: Windows Server 2016

> [!div class="step-by-step"]
> [«Förbereda en domän](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
> 
> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Domänkontrollantens namn - **corpdc**
> - Domännamn – **contoso**
> - MIM-Tjänstserverns name - **corpservice**
> - Servernamnet för MIM Sync - **corpsync**
> - Namnet på SQL Server - **corpsql**
> - Lösenord – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Anslut Windows Server 2016 till din domän

Börja med en Windows Server 2016-dator, med minst 8 – 12 GB RAM-minne. Vid installationen anger du ”Windows Server 2016 Standard/Datacenter (Server med GUI) x 64” edition.

1. Logga in på den nya datorn som administratör.

2. Ge datorn en statisk IP-adress i nätverket med hjälp av Kontrollpanelen. Konfigurera nätverksgränssnittet för att skicka DNS-frågor till IP-adressen för domänkontrollanten i föregående steg och ange datornamnet till **CORPSERVICE**.  Detta kräver en omstart av servern.

3. Öppna Kontrollpanelen och Anslut datorn till domänen som du konfigurerade i det sista steget, *contoso.com*.  Detta omfattar att ange användarnamnet och autentiseringsuppgifterna för en domänadministratör, t.ex. *Contoso\Administratör*.  Efter att välkomstmeddelandet har visats kan du stänga dialogrutan och starta om servern igen.

4. Logga in på datorn *CORPSERVICE* som ett domänkonto med administratören för lokala datorer som *Contoso\MIMINSTALL*.


5. Öppna ett PowerShell-fönster som administratör och skriv följande kommando för att uppdatera datorn med inställningarna för grupprincipen.

    ```
    gpupdate /force /target:computer
    ```

    Inom mindre än en minut slutförs uppdateringen med meddelandet ”Uppdatering av grupprincip har slutförts”.

6. Lägg till roller för **Webbserver (IIS)** och **Programserver**, funktionerna för **.NET Framework** 3.5, 4.0 och 4.5 och **Active Directory-modulen för Windows PowerShell**.

    ![Bild av PowerShell-funktioner](media/MIM-DeployWS2.png)

7. Skriv följande kommandon i PowerShell. Observera att det kan vara nödvändigt att ange en annan plats för källfilerna för **.NET Framework** 3.5-funktionerna. De här funktionerna finns vanligtvis inte när du installerar Windows Server, men är tillgängliga i mappen sida-vid-sida (SxS) i mappen Sources på OS-installationsdisken, t.ex. ” \*d:\Sources\SxS\* ”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Konfigurera säkerhetsprinciper för servern

Konfigurera säkerhetsprincipen för servern så att de konton som nyligen skapats kan köras som tjänster.
> [!NOTE] 
> Beroende på konfigurationen baserat enkel server(all-in-one) eller server för distribuerad du behöver bara lägga till på datorn medlem som synkroniseringsserver. 

1. Starta programmet för lokala säkerhetsprinciper

2. Gå till **Lokala principer > Tilldelning av användarrättigheter**.

3. Högerklicka på **Logga in som en tjänst** i informationsfönstret och välj **Egenskaper**.

    ![Bild för Lokal säkerhetsprincip](media/MIM-DeployWS3.png)

4. Klicka på **lägga till användare eller grupp**, och i textrutan efter typen baserat på roll `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, klickar du på **Kontrollera namn**, och klicka på **OK**.

5. Klicka på **OK** för att stänga fönstret **Logga in som en tjänst Egenskaper**.

6.  I informationsfönstret, högerklickar du på **neka åtkomst till den här datorn från nätverket**, och välj **egenskaper**. >

[!NOTE] Om separata rollservrar det här steget bryts vissa funktioner som SSPR-funktionen.

7. Klicka på **Lägg till användare eller grupp**, skriv `contoso\MIMSync; contoso\MIMService` i textrutan och klicka på **OK**.

8. Klicka på **OK** för att stänga fönstret **Neka åtkomst till den här datorn från nätverket Egenskaper**.

9. Högerklicka på **Neka inloggning lokalt** i informationsfönstret och välj **Egenskaper**.

10. Klicka på **Lägg till användare eller grupp**, skriv `contoso\MIMSync; contoso\MIMService` i textrutan och klicka på **OK**.

11. Klicka på **OK** för att stänga fönstret **Neka inloggning lokalt Egenskaper**.

12. Stäng fönstret för lokala säkerhetsprinicper.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Ändra IIS-autentisering för Windows-läge om det behövs

1.  Öppna ett PowerShell-fönster.

2.  Avbryt IIS med kommandot *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Förbereda en domän](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
