---
title: Konfigurera Windows Server | Microsoft Docs
description: "Hämta stegen och minimikraven för att förbereda Windows Server 2012 RS för att arbeta med MIM 2016."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 08/11/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 06d2365808858dc09d04ab37eafa9d6e3212cbcf


---

# <a name="set-up-an-identity-management-server-windows-server-2012-r2"></a>Konfigurera en server för identitetshantering: Windows Server 2012 R2

>[!div class="step-by-step"]
[« Förbereda en domän](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Till exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – **Pass@word1**

## <a name="join-windows-server-2012-r2-to-your-domain"></a>Ansluta Windows Server 2012 R2 till din domän

Börja med en dator med Windows Server 2012 R2, med minst 8 GB RAM-minne. Vid installationen anger du version ”Windows Server 2012 R2 Standard (Server med GUI) x 64”.

1. Logga in på den nya datorn som administratör.

2. Ge datorn en statisk IP-adress i nätverket med hjälp av Kontrollpanelen. Konfigurera nätverksgränssnittet så att DNS-frågor skickas till domänkontrollantens IP-adress i föregående steg och ange datornamnet **CORPIDM**.  Detta kräver en omstart av servern.

3. Öppna Kontrollpanelen och anslut datorn till domänen du konfigurerade i det sista steget, *contoso.local*.  Detta omfattar att ange användarnamnet och autentiseringsuppgifterna för en domänadministratör, t.ex. *Contoso\Administratör*.  Efter att välkomstmeddelandet har visats kan du stänga dialogrutan och starta om servern igen.

4. Logga in på datorn *CorpIDM* som domänadministratör, t.ex. *Contoso\Administratör*.

5. Öppna ett PowerShell-fönster som administratör och skriv följande kommando för att uppdatera datorn med inställningarna för grupprincipen.

    ```
    gpupdate /force /target:computer
    ```

    Inom mindre än en minut slutförs uppdateringen med meddelandet ”Uppdatering av grupprincip har slutförts”.

6. Lägg till roller för **Webbserver (IIS)** och **Programserver**, funktionerna för **.NET Framework** 3.5, 4.0 och 4.5 och **Active Directory-modulen för Windows PowerShell**.

    ![Bild av PowerShell-funktioner](media/MIM-DeployWS2.png)

7. Skriv följande kommandon i PowerShell. Observera att det kan vara nödvändigt att ange en annan plats för källfilerna för **.NET Framework** 3.5-funktionerna. De här funktionerna finns vanligtvis inte när du installerar Windows Server, men är tillgängliga i mappen sida-vid-sida (SxS) i mappen Sources på OS-installationsdisken, t.ex. ”* d:\Sources\SxS\\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Konfigurera säkerhetsprinciper för servern

Konfigurera säkerhetsprincipen för servern så att de konton som nyligen skapats kan köras som tjänster.

1. Starta programmet för lokala säkerhetsprinciper

2. Gå till **Lokala principer > Tilldelning av användarrättigheter**.

3. Högerklicka på **Logga in som en tjänst** i informationsfönstret och välj **Egenskaper**.

    ![Bild för Lokal säkerhetsprincip](media/MIM-DeployWS3.png)

4. Klicka på **Lägg till användare eller grupp** och skriv `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR` i textrutan, klicka på **Kontrollera namn** och klicka på **OK**.

5. Klicka på **OK** för att stänga fönstret **Logga in som en tjänst Egenskaper**.

6.  Högerklicka på **Neka åtkomst till den här datorn från nätverket** i informationsfönstret och välj **Egenskaper**.

7. Klicka på **Lägg till användare eller grupp**, skriv `contoso\MIMSync; contoso\MIMService` i textrutan och klicka på **OK**.

8. Klicka på **OK** för att stänga fönstret **Neka åtkomst till den här datorn från nätverket Egenskaper**.

9. Högerklicka på **Neka inloggning lokalt** i informationsfönstret och välj **Egenskaper**.

10. Klicka på **Lägg till användare eller grupp**, skriv `contoso\MIMSync; contoso\MIMService` i textrutan och klicka på **OK**.

11. Klicka på **OK** för att stänga fönstret **Neka inloggning lokalt Egenskaper**.

12. Stäng fönstret för lokala säkerhetsprinicper.


## <a name="change-the-iis-windows-authentication-mode"></a>Ändra Windows-autentiseringsläget för IIS

1.  Öppna ett PowerShell-fönster.

2.  Avbryt IIS med kommandot *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« Förbereda en domän](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)



<!--HONumber=Nov16_HO2-->


