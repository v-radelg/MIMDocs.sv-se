---
title: Konfigurera SharePoint för Microsoft Identity Manager 2016 | Microsoft Docs
description: Installera och konfigurera SharePoint Foundation så att den kan vara värd för MIM-portalsidan.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7fb65eec7a42da94c4f27a30e59c09739279e882
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043536"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Konfigurera en server för identitetshantering: SharePoint

> [!div class="step-by-step"]
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 

> [!NOTE]
> Installations proceduren för SharePoint Server 2019 skiljer sig inte från installations proceduren för SharePoint Server 2016 **förutom** ett extra steg som måste utföras för att AVblockera ASHX-filer som används av MIM-portalen.

> [!NOTE]
> Den här genomgången använder exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Till exempel:
> - Domänkontrollantens namn – **corpdc**
> - Domännamn – **contoso**
> - Server namn för MIM-tjänst – **corpservice**
> - MIM Sync Server-namn – **corpsync**
> - SQL Server namn – **corpsql**
> - Lösenord – <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Installera **SharePoint 2016**

> [!NOTE]
> Internetanslutning krävs för att installationsprogrammet ska kunna hämta allt som behövs. Om datorn finns i ett virtuellt nätverk utan internetanslutning kan du lägga till ytterligare ett nätverksgränssnitt på den dator som har anslutning till Internet. Detta kan inaktiveras när installationen är klar.

Följ de här stegen för att installera SharePoint 2016. När du har slutfört installationen startar servern om.

1.  Starta **PowerShell** som ett domän konto med lokal administratör på **corpservice** och **sysadmin** på SQL Database-Server vi ska använda **contoso\miminstall**.

    -   Ändra till katalogen där SharePoint packades upp.

    -   Skriv in följande kommando:
    ```
    .\prerequisiteinstaller.exe
    ```

2.  När du har installerat **SharePoint** -förutsättningarna installerar du **SharePoint 2016** genom att skriva följande kommando:

    ```
    .\setup.exe
    ```

3.  Välj den fullständiga servertypen.

4.  Kör guiden när installationen har slutförts.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Kör guiden för att konfigurera SharePoint

Följ stegen i **Konfigurationsguiden för SharePoint-produkter** för att konfigurera SharePoint att arbeta med MIM.

1. Ändra till att skapa en ny servergrupp på fliken **Anslut till en servergrupp**.

2. Ange den här servern som databas server som **corpsql** för konfigurations databasen och *Contoso\SharePoint* som det konto för databas åtkomst som SharePoint ska använda.
3. Skapa ett lösenord för gruppens säkerhetslösenfras.

4. I konfigurations guiden rekommenderar vi att du väljer [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) -typ för **klient delen**

5. När konfigurations guiden Slutför konfigurations åtgärd 10 av 10 klickar du på Slutför och en webbläsare öppnas.

6. Om du uppmanas att ange popup-fönstret för Internet Explorer autentiserar du som *Contoso\miminstall* (eller motsvarande administratörs konto) för att fortsätta.

7. Klicka på **Avbryt/hoppa över**i webb guiden (i webbappen).


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Förbereda SharePoint som värd för MIM-portalen

> [!NOTE]
> SSL konfigureras inte första gången. Se till att konfigurera SSL eller motsvarande innan du aktiverar åtkomst till portalen.

1. Starta **sharepoint 2016 Management Shell** och kör följande PowerShell-skript för att skapa ett **SharePoint 2016-webbprogram**.

    ```PowerShell
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Ett varningsmeddelande visas med information om att Windows Classic-autentiseringsmetoden används och att det kan ta flera minuter för kommandot att returneras. När du är klar visas den nya portalens URL som utdata. Låt **SharePoint 2016 Management Shell** -fönstret vara öppet för senare bruk.

2. Starta SharePoint 2016 Management Shell och kör följande PowerShell-skript för att skapa en **SharePoint** -webbplatssamling som är associerad med webb programmet.
    ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
    ```
    > [!NOTE]
    > Kontrol lera att resultatet av *CompatibilityLevel* -variabeln är "15". Om resultatet är annat än "15" skapades inte webbplats samlingen med rätt version. ta bort webbplats samlingen och skapa den igen.

    > [!IMPORTANT]
    > SharePoint Server 2019 använder olika webb programs egenskaper för att behålla en lista över blockerade fil namns tillägg. För att avblockera. ASHX-filer som används av MIM-portalen tre extra-kommandon måste köras manuellt från hanterings gränssnittet för SharePoint.
    <br/>
    **Kör endast följande tre kommandon för SharePoint 2019:**

    ```PowerShell
    $w.BlockedASPNetExtensions.Remove("ashx")
    $w.Update()
    $w.BlockedASPNetExtensions
    ```
   > [!NOTE]
   > Kontrol lera att *BlockedASPNetExtensions* -listan inte innehåller fler än ASHX-tillägg flera MIM-portaler kan inte återges på rätt sätt.


3. Inaktivera **ViewState för SharePoint på Server sidan** och SharePoint-uppgiften "hälso analys jobb (varje timme, Microsoft SharePoint Foundation timer, alla servrar)" genom att köra följande PowerShell-kommandon i **hanterings gränssnittet för SharePoint 2016**:

   ```PowerShell
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. Öppna en ny webbläsare-flik på din Identity Management-Server och navigera till http://mim.contoso.com/ och logga in som *contoso\miminstall*.  En tom SharePoint-sida med namnet *MIM-portal* visas.

    ![MIM-portalen på http://mim.contoso.com/ avbildning](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Kopiera webbadressen och öppna sedan **Internetalternativ** i Internet Explorer, byt till fliken **Säkerhet**, välj **Lokalt intranät** och klicka på **Platser**.

    ![Bild av Internetalternativ](media/MIM-DeploySP2.png)

6. I fönstret **Lokalt intranät** klickar du på **Avancerat** och klistrar in den kopierade webbadressen i textrutan **Lägg till följande webbplats i zonen**. Klicka på **Lägg till** och stäng sedan fönstren.

7. Öppna programmet **Administrationsverktyg**, navigera till **Tjänster**, leta upp SharePoints Administrationstjänst och starta den om den inte redan körs.

> [!div class="step-by-step"]  
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
