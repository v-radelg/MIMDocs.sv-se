---
title: Distribuera PAM steg 3 – PAM-server | Microsoft Docs
description: Förbered en PAM-server som ska vara värd för både SQL och SharePoint för distribution av Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3098816d07603384a28db47c0bc60ac92a340e8f
ms.sourcegitcommit: 80507a128d2bc28ff3f1b96377c61fa97a4e7529
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/13/2020
ms.locfileid: "83279988"
---
# <a name="step-3--prepare-a-pam-server"></a>Steg 3 – Förbereda en PAM-server

> [!div class="step-by-step"]
> [«Steg 2](step-2-prepare-priv-domain-controller.md) 
>  [Steg 4»](step-4-install-mim-components-on-pam-server.md)

## <a name="install-windows-server-2012-r2"></a>Installera Windows Server 2012 R2

På en tredje virtuell dator installerar du Windows Server 2012 R2, mer specifikt Windows Server 2012 R2 Standard (server med GUI) x64, för att skapa *PAMSRV*. Eftersom SQL Server och SharePoint 2013 kommer att installeras på den här datorn, krävs det minst 8 GB RAM-minne.

1. Välj **Windows Server 2012 R2 Standard (server med GUI) x64**.

    ![Välja Windows Server Standard med GUI – skärmbild](media/PAM_GS_Select_WS2012.png)

2. Granska och godkänn licensvillkoren.

3.  Eftersom disken är tom markerar du **Anpassad: Installera bara Windows** och använd **ej initierat diskutrymme**.

4.  Logga in på den nya datorn som administratör.  Använd kontrollpanelen och ge den en statisk IP-adress på det virtuella nätverket, konfigurera nätverksgränssnittet att skicka DNS-frågor till IP-adressen för PRIVDC och ange datornamnet till *PAMSRV*.  Detta kräver en omstart av servern.

5.  Om det virtuella nätverket inte har internetanslutning kan du lägga till ytterligare ett nätverksgränssnitt på den dator som har anslutning till Internet.  Det för SharePoint-installationen och kan inte inaktiveras när åtgärden har slutförts.

6.  När servern har startats om loggar du in som administratör. Använd kontrollpanelen till att konfigurera datorn att söka efter uppdateringar och installera de uppdateringar som krävs.  Du kan behöva starta om servern.

7.  När servern startas om loggar du in som administratör, öppnar kontrollpanelen och ansluter PAMSRV till PRIV-domänen (priv.contoso.local).  När du gör det måste du ange användarnamnet och autentiseringsuppgifterna för en PRIV-domänadministratör, (PRIV\Administratör). Efter att välkomstmeddelandet har visats kan du stänga dialogrutan och starta om servern.


### <a name="add-the-web-server-iis-and-application-server-roles"></a>Lägg till rollerna för webbserver (IIS) och programserver

Lägg till roller för webbserver (IIS) och programserver, .NET Framework 3.5-funktioner, Active Directory-modulen för Windows PowerShell och andra funktioner som krävs för SharePoint.

1.  Logga in som PRIV-domänadministratör (PRIV\Administratör) och starta PowerShell.

2.  Skriv in följande kommandon: Observera att du kan behöva ange en annan plats för källfilerna för .NET Framework 3.5-funktionerna. De här funktionerna finns vanligtvis inte när du installerar Windows Server, men är tillgängliga i mappen sida-vid-sida (SxS) i mappen Sources på OS-installationsdisken, t.ex. ”d:\Sources\SxS\”.

    ```PowerShell
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### <a name="configure-the-server-security-policy"></a>Konfigurera säkerhetsprinciper för servern

Konfigurera säkerhetsprincipen för servern så att de konton som nyligen skapats kan köras som tjänster.

1.  Starta programmet för **lokal säkerhetsprincip**.   
2.  Navigera till **lokala principer**  >  **tilldelning av användar rättigheter**.  
3.  Högerklicka på **Logga in som en tjänst** i informationsfönstret och välj **Egenskaper**.  
4.  Klicka på **Lägg till användare eller grupp** och skriv *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer* i Användar- och gruppnamn. Klicka på **Kontrollera namn** och klicka på **OK**.  

5.  Stäng Fönstret Egenskaper genom att klicka på **OK** .
6.  Högerklicka på **Neka åtkomst till den här datorn från nätverket** i informationsfönstret och välj **Egenskaper**.  
7.  Klicka på **Lägg till användare eller grupp** och skriv*priv\mimmonitor; priv\MIMService; priv\mimcomponent* i Användar- och gruppnamn. Klicka på **OK**.  
8.  Klicka på **OK** för att stänga egenskapsfönstret.

9. Högerklicka på **Neka inloggning lokalt** i informationsfönstret och välj **Egenskaper**.  
10. Klicka på **Lägg till användare eller grupp** och skriv*priv\mimmonitor; priv\MIMService; priv\mimcomponent* i Användar- och gruppnamn. Klicka på **OK**.  
11. Klicka på **OK** för att stänga egenskapsfönstret.  
12. Stäng fönstret för lokala säkerhetsprinicper.  

13. Öppna kontrollpanelen och växla till **Användarkonton**.
14. Klicka på **Ge andra användare åtkomst till den här datorn**.
15. Klicka på **Lägg till** och ange användaren *MIMADMIN* i domänen *PRIV*. På nästa skärm i guiden klickar du på **Lägg till användaren som administratör**.  
16. Klicka på **Lägg till** och ange användaren *SharePoint* i domänen *PRIV*. På nästa skärm i guiden klickar du på **Lägg till användaren som administratör**.  
17. Stäng kontrollpanelen.

### <a name="change-the-iis-configuration"></a>Ändra IIS-konfigurationen

Det finns två sätt att ändra IIS-konfigurationen så att program kan använda läget för Windows-autentisering. Kontrollera att du är inloggad som MIMAdmin och följ sedan något av följande alternativ.

Om du vill använda PowerShell:

1. Högerklicka på PowerShell och välj **Kör som administratör**.
2. Stoppa IIS och lås upp programvärdinställningarna med dessa kommandon
   ```CMD
   iisreset /STOP
   C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
   iisreset /START
   ```

Om du vill använda en textredigerare, t.ex. Notepad:

1. Öppna filen **C:\Windows\System32\inetsrv\config\applicationHost.config**
2. Bläddra ner till rad 82 i filen. Taggvärdet för **overrideModeDefault** ska vara **<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. Ändra värdet för **overrideModeDefault** till *Tillåt*  
4. Spara filen och starta om IIS med PowerShell-kommandot `iisreset /START`

## <a name="install-sql-server"></a>Installera SQL Server

Om SQL Server inte är i skyddsmiljön, installerar du antingen SQL Server 2012 (Service Pack 1 eller senare) eller SQL Server 2014. Följande steg gäller för SQL 2014.

1. Kontrollera att du är inloggad som MIMAdmin.
2. Högerklicka på PowerShell och välj **Kör som administratör**.
3. Gå till den katalog där installationsprogrammet för SQL Server finns.  
4. Ange följande kommando.  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## <a name="install-sharepoint-foundation-2013"></a>Installera SharePoint Foundation 2013

Med SharePoint Foundation 2013 med SP1-installationsprogrammet installerar du SharePoints programvarukrav på PAMSRV.

> [!NOTE]
> Internetanslutning krävs för att installationsprogrammet ska kunna ladda ned allt som behövs. När det har installerats startas servern om.

1. Högerklicka på PowerShell och välj **Kör som administratör**.  
2. Ändra till katalogen där SharePoint packades upp.  
3. Skriv kommandot `.\prerequisiteinstaller.exe`.

När alla nödvändiga SharePoint-komponenter har installerats installerar du SharePoint Foundation 2013 med SP1.

1.  Högerklicka på PowerShell och välj **Kör som administratör**.  
2.  Ändra till katalogen där SharePoint packades upp.  
3.  Skriv kommandot `.\setup.exe`.  
4.  Välj typen **fullständig server**.  
5.  När installationen har slutförts väljer du att köra guiden.  

### <a name="configure-sharepoint"></a>Konfigurera SharePoint

Konfigurera SharePoint genom att köra Konfigurationsguiden för SharePoint-produkter.

1.  På fliken Anslut till en Server grupp ändrar du för att **skapa en ny server grupp**.  
2.  Ange **PAMSRV** som databasserver för konfigurationsdatabasen och **PRIV\SharePoint** som det konto för databasåtkomst som SharePoint ska använda.  
3.  Ange ett lösenord som lösenfras för gruppsäkerhet (du kommer inte att använda igen under genomgången).  
4.  I det här läget accepterar du resten av standardinställningarna i konfigurationsguiden för SharePoint för att skapa en enskild server-grupp.    
5.  När konfigurations guiden Slutför konfigurations åtgärd 10 av 10 klickar du på **Slutför** och en webbläsare öppnas.  
6.  I popup-fönstret i Internet Explorer autentiserar du dig som domänadministratör (PRIV\MIMAdmin) för att kunna fortsätta.  
7.  Starta guiden i webbappen för att konfigurera SharePoint-gruppen.  
8.  Välj att använda det befintliga hanterade kontot (PRIV\SharePoint), avmarkera kryssrutan för att inaktivera alla valfria tjänster och klicka på **Nästa**.  
9. När fönstret Skapa en webbplatssamling visas klickar du på **Hoppa över** och **Slutför**.  

## <a name="create-a-sharepoint-foundation-2013-web-application"></a>Skapa en SharePoint Foundation 2013-webbapp

När guiden har slutförts använder du PowerShell till att skapa en SharePoint Foundation 2013-webbapp som värd för MIM-portalen. Eftersom den här genomgången bara är ett exempel aktiveras inte SSL.

1.  Högerklicka på SharePoint 2013 Management Shell, välj **Kör som administratör** och kör följande PowerShell-skript:

    ```PowerShell
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Ett varningsmeddelande visas med information om att Windows Classic-autentiseringsmetoden används och att det kan ta flera minuter för kommandot att returneras.  När det är klart klar visas den nya portalens URL som utdata.

> [!NOTE]
> Lämna fönstret SharePoint 2013 Management Shell öppet. Det ska användas i nästa steg.

## <a name="create-a-sharepoint-site-collection"></a>Skapa en SharePoint-webbplatssamling

Sedan skapar du en SharePoint-webbplatssamling som är kopplade till den webbappen som värd för MIM-portalen.

1.  Öppna fönstret **SharePoint 2013 Management Shell** om det inte redan är öppet och kör följande PowerShell-skript

    ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Kontrollera att variabeln **CompatibilityLevel** är inställd på *14*. Om den returnerar *15* skapades inte webbplatssamlingen för 2010-versionen. Ta bort webbplatssamlingen och skapa den igen.

2.  Kör följande PowerShell-kommandon i **SharePoint 2013 Management Shell**. Detta inaktiverar det ViewState för SharePoint på Server sidan och SharePoint-uppgiftens **hälso analys jobb (varje timme, Microsoft SharePoint Foundation timer, alla servrar)**.

    ```PowerShell
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## <a name="change-update-settings"></a>Ändra uppdateringsinställningar

1. Öppna kontrollpanelen, gå till **Windows Update** och klicka på att **ändra inställningar**.  
2. Ändra inställningarna för att ta emot uppdateringar via Windows Update och andra produkter från Microsoft Updates.  
3. Sök efter nya uppdateringar och se till att alla väntande viktiga uppdateringar har installerats innan du fortsätter.

## <a name="set-the-website-as-the-local-intranet"></a>Ange webbplatsen som lokalt intranät

1. Starta Internet Explorer och öppna en ny webbläsarflik
2. Navigera till `http://pamsrv.priv.contoso.local:82/` och logga in som PRIV\MIMAdmin.  En tom SharePoint-webbplats med namnet MIM-portal visas.  
3. I Internet Explorer öppnar du **Internetalternativ**, byter till fliken **Säkerhet**, väljer **Lokalt intranät** och lägger till webbadressen `http://pamsrv.priv.contoso.local:82/`.

Om inloggningen misslyckas kan du behöva uppdatera de Kerberos SPN du skapade i [steg 2](step-2-prepare-priv-domain-controller.md).

## <a name="start-the-sharepoint-administration-service"></a>Starta SharePoints administrationstjänst

Gå till **Tjänster** (i Administrationsverktyg) och starta **SharePoints administrationstjänst** om den inte redan körs.

I steg 4 börjar du installera MIM-komponenterna på PAM-servern.

> [!div class="step-by-step"]
> [«Steg 2](step-2-prepare-priv-domain-controller.md) 
>  [Steg 4»](step-4-install-mim-components-on-pam-server.md)
