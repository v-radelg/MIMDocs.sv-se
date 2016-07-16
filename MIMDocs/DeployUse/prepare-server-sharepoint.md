---
# required metadata

title: "Konfigurera en server för identitetshantering: SharePoint | Microsoft Identity Manager"
description: "Installera och konfigurera SharePoint Foundation så att den kan vara värd för MIM-portalsidan."
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Konfigurera en server för identitetshantering: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> Den här genomgången använder exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – **Pass@word1**


## Installera **SharePoint Foundation 2013 med SP1**

> [!NOTE]
> Internetanslutning krävs för att installationsprogrammet ska kunna hämta allt som behövs. Om datorn finns i ett virtuellt nätverk utan internetanslutning kan du lägga till ytterligare ett nätverksgränssnitt på den dator som har anslutning till Internet. Detta kan inaktiveras när installationen är klar.

Följ dessa steg om du vill installera SharePoint Foundation 2013 SP1. När du har slutfört installationen startar servern om.

1.  Starta **PowerShell** som domänadministratör.

    -   Ändra till katalogen där SharePoint packades upp.

    -   Skriv in följande kommando:

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Efter att alla nödvändiga **SharePoint**-komponenter har installerats installerar du **SharePoint Foundation 2013 med SP1** genom att skriva följande kommando:

    ```
    .\setup.exe
    ```

3.  Välj den fullständiga servertypen.

4.  Kör guiden när installationen har slutförts.

## Kör guiden för att konfigurera SharePoint

Följ stegen i **Konfigurationsguiden för SharePoint-produkter** för att konfigurera SharePoint att arbeta med MIM.

1. Ändra till att skapa en ny servergrupp på fliken **Anslut till en servergrupp**.

2. Ange den här servern som databasserver för konfigurationsdatabasen och *Contoso\SharePoint* som det konto för databasåtkomst som SharePoint ska använda.

3. Skapa ett lösenord för gruppens säkerhetslösenfras.

4. När konfigurationsguiden har slutfört konfigurationsåtgärd 10 av 10 klickar du på Slutför och en webbläsare öppnas.

5. I popup-fönstret i Internet Explorer autentiserar du som *Contoso\Administratör* (eller motsvarande administratörskonto i domänen) för att fortsätta.

6. Starta guiden (från webbappen) för att konfigurera SharePoint-servergruppen.

7. Välj alternativet för att använda det befintliga hanterade kontot (*Contoso\SharePoint*) och klicka på **Nästa**.

8. I fönstret **Skapa en webbplatssamling** klickar du på **Hoppa över**.  Klicka sedan på **Slutför**.

## Förbereda SharePoint som värd för MIM-portalen

> [!NOTE]
> SSL konfigureras inte första gången. Se till att konfigurera SSL eller motsvarande innan du aktiverar åtkomst till portalen.

1. Starta  **SharePoint 2013 Management Shell** och kör följande PowerShell-skript för att skapa ett **SharePoint Foundation 2013-webbprogram**.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
    -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE] Ett varningsmeddelande visas med information om att Windows Classic-autentiseringsmetoden används och att det kan ta flera minuter för kommandot att returneras. När du är klar visas den nya portalens URL som utdata. Håll **SharePoint 2013 Management Shell**-fönstret öppet för senare referens.

2. Starta SharePoint 2013 Management Shell och kör följande PowerShell-skript för att skapa en **SharePoint-webbplatssamling** som är associerad med det webbprogrammet.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE] Kontrollera att resultatet för *CompatibilityLevel* är ”14”. Om resultatet är ”15” skapades inte webbplatssamlingen för erfarenhetsversion 2010. Ta bort webbplatssamlingen och skapa den igen.

3. Inaktivera **Vytillstånd på SharePoint Server-sidan** och SharePoint-uppgiften ”Hälsoanalysjobb (Varje timme, Microsoft SharePoint Foundation-timer, alla servrar)” genom att köra följande PowerShell-kommandon i **SharePoint 2013 Management Shell**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Öppna en ny webbläsarflik på din server för identitetshantering, gå till http://localhost:82 och logga in som *contoso\Administratör*.  En tom SharePoint-sida med namnet *MIM-portal* visas.

    ![Bild av MIM-portalen på http://localhost:82/](media/MIM-DeploySP1.png)

5. Kopiera webbadressen och öppna sedan **Internetalternativ** i Internet Explorer, byt till fliken **Säkerhet**, välj **Lokalt intranät** och klicka på **Platser**.

    ![Bild av Internetalternativ](media/MIM-DeploySP2.png)

6. I fönstret **Lokalt intranät** klickar du på **Avancerat** och klistrar in den kopierade webbadressen i textrutan **Lägg till följande webbplats i zonen**. Klicka på **Lägg till** och stäng sedan fönstren.

7. Öppna programmet **Administrationsverktyg**, navigera till **Tjänster**, leta upp SharePoints Administrationstjänst och starta den om den inte redan körs.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)


<!--HONumber=Apr16_HO4-->


