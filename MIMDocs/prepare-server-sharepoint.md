---
title: Konfigurera SharePoint för Microsoft Identity Manager 2016 | Microsoft Docs
description: Installera och konfigurera SharePoint Foundation så att den kan vara värd för MIM-portalsidan.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5beee58388c12abbe8e3245ff610d3528b03786a
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690757"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Konfigurera en server för Identitetshantering: SharePoint

> [!div class="step-by-step"]
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 
> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Domänkontrollantens namn - **corpdc**
> - Domännamn – **contoso**
> - MIM-Tjänstserverns name - **corpservice**
> - Servernamnet för MIM Sync - **corpsync**
> - Namnet på SQL Server - **corpsql**
> - Lösenord – <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Installera **SharePoint 2016**

> [!NOTE]
> Internetanslutning krävs för att installationsprogrammet ska kunna hämta allt som behövs. Om datorn finns i ett virtuellt nätverk utan internetanslutning kan du lägga till ytterligare ett nätverksgränssnitt på den dator som har anslutning till Internet. Detta kan inaktiveras när installationen är klar.

Följ dessa steg om du vill installera SharePoint 2016. När du har slutfört installationen startar servern om.

1.  Starta **PowerShell** som ett domänkonto med lokal administratör på den **corpservice** och **sysadmin** på SQL-databasservern som vi använder ut **contoso\ miminstall**.

    -   Ändra till katalogen där SharePoint packades upp.

    -   Skriv följande kommando.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Efter **SharePoint** nödvändiga komponenter har installerats, installerar **SharePoint 2016** genom att skriva följande kommando:

    ```
    .\setup.exe
    ```

3.  Välj den fullständiga servertypen.

4.  Kör guiden när installationen har slutförts.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Kör guiden för att konfigurera SharePoint

Följ stegen i **Konfigurationsguiden för SharePoint-produkter** för att konfigurera SharePoint att arbeta med MIM.

1. Ändra till att skapa en ny servergrupp på fliken **Anslut till en servergrupp**.

2. Ange den här servern som databasserver som **corpsql** för konfigurationsdatabasen och *Contoso\SharePoint* som kontot för SharePoint ska använda databasåtkomst.
3. Skapa ett lösenord för gruppens säkerhetslösenfras.

4. Vi rekommenderar att du väljer i konfigurationsguiden [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) typ av **frontend**

5. När konfigurationsguiden har slutfört konfigurationsåtgärd 10 av 10, klickar du på Slutför och en webbläsare öppnas...

6. Om du uppmanas popup-fönstret för Internet Explorer autentiserar du som *Contoso\miminstall* (eller motsvarande administratörskonto) att fortsätta.

7. I guiden (från webbappen) klickar du på **Avbryt/hoppa över**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Förbereda SharePoint som värd för MIM-portalen

> [!NOTE]
> SSL konfigureras inte första gången. Se till att konfigurera SSL eller motsvarande innan du aktiverar åtkomst till portalen.

1. Starta **hanteringsgränssnittet för SharePoint 2016** och kör följande PowerShell-skript för att skapa en **webbprogram för SharePoint 2016**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Ett varningsmeddelande visas med information om att Windows Classic-autentiseringsmetoden används och att det kan ta flera minuter för kommandot att returneras. När du är klar visas den nya portalens URL som utdata. Behåll den **hanteringsgränssnittet för SharePoint 2016** fönstret öppet för senare referens.

2. Starta hanteringsgränssnittet för SharePoint 2016 och kör följande PowerShell-skript för att skapa en **SharePoint-webbplatssamling** som är associerade med det webbprogrammet.

   ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```

   > [!NOTE]
   > Kontrollera att resultatet av den *CompatibilityLevel* variabeln är ”15”. Om resultatet är än ”15”, sedan skapades webbplatssamlingen inte korrekt upplevelse-version. ta bort webbplatssamlingen och skapa den på nytt.

3. Inaktivera **SharePoint serversidan Viewstate** och SharePoint-uppgiften ”Hälsoanalysjobb (varje timme, Microsoft SharePoint Foundation-Timer, alla servrar)” genom att köra följande PowerShell-kommandon i den  **Hanteringsgränssnittet för SharePoint 2016**:

   ```
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. Öppna en ny webbläsarflik på din server för Identitetshantering, gå till http://mim.contoso.com/ och logga in som *contoso\miminstall*.  En tom SharePoint-sida med namnet *MIM-portal* visas.

    ![MIM-portalen på http://mim.contoso.com/ bild](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Kopiera webbadressen och öppna sedan **Internetalternativ** i Internet Explorer, byt till fliken **Säkerhet**, välj **Lokalt intranät** och klicka på **Platser**.

    ![Bild av Internetalternativ](media/MIM-DeploySP2.png)

6. I fönstret **Lokalt intranät** klickar du på **Avancerat** och klistrar in den kopierade webbadressen i textrutan **Lägg till följande webbplats i zonen**. Klicka på **Lägg till** och stäng sedan fönstren.

7. Öppna programmet **Administrationsverktyg**, navigera till **Tjänster**, leta upp SharePoints Administrationstjänst och starta den om den inte redan körs.

> [!div class="step-by-step"]  
> [«SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
