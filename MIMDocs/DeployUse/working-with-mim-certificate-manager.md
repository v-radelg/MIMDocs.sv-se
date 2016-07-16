---
title: Arbeta med MIM Certificate Manager| Microsoft Identity Manager
description: "Läs mer om hur du distribuerar Certificate Manager-appen, så att  användarna kan hantera sina egna åtkomsträttigheter."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: 3e0e6cea0b268836bb6347e81694deec93320ce3


---

# Arbeta med MIM Certificate Manager
När du har börjat köra MIM 2016 och Certificate Manager kan du distribuera Windows Store-appen för MIM Certificate Manager så att användarna kan hantera sina fysiska smartkort, virtuella smartkort och programcertifikat. Distribution av MIM CM-appen genomförs i följande steg:

1.  Skapa en certifikatmall.

2.  Skapa en profilmall.

3.  Förbered appen.

4.  Distribuera appen via SCCM eller Intune.

## Skapa en certifikatmall
Du skapar en certifikatmall för CM-appen på samma sätt som du normalt skapar en certifikatmall, förutom att du måste se till att certifikatmallen är av version 3 eller senare.

1.  Logga in på den server som kör AD CS (certifikatservern).

2.  Öppna MMC.

3.  Klicka på **Arkiv &gt; Lägg till/ta bort snapin-modul**.

4.  I listan Tillgängliga snapin-moduler klickar du på **Certifikatmallar** och sedan på **Lägg till**.

5.  Du ser nu **Certifikatmallar** under **Konsolrot** i MMC. Dubbelklicka på den för att se alla tillgängliga certifikatmallar.

6.  Högerklicka på mallen **Smartkortsinloggning** och klicka på **Kopiera mall**.

7.  Välj Windows Server 2008 på fliken Kompatibilitet under certifikatutfärdare, och välj Windows 8.1/Windows Server 2012 R2 under certifikatmottagare.
    Det här steget är mycket viktigt eftersom det säkerställer att du har en certifikatmall av version 3 (eller senare), och det är endast version 3 som fungerar med appen för certifikathantering. Om du inte skapade certifikatmallen på detta sätt går det, på grund av att versionen konfigureras första gången du skapar och sparar certifikatmallen, inte att ändra den till korrekt version och du måste skapa en ny innan du fortsätter.

8.  På fliken **Allmänt** i fältet **Visningsnamn** anger du det namn du vill ska visas i appens användargränssnitt, till exempel **Inloggning för virtuellt smartkort**.

9. På fliken **Hantering av begäranden** ska **Syfte** ställas in på **Signatur och kryptering** under **Gör följande...** väljer du **Ställ frågor till användaren när registrering sker**.

10. På fliken **Kryptografi** under **Leverantörskategori** väljer du **Nyckellagringsprovider och Begäranden kan använda valfri provider som finns på användarens dator**.

    > [!NOTE]
    > Nyckellagringsprovider visas endast som alternativ om din mall är av version 3. Om du kan se den beror det förmodligen på att du inte skapade certifikatmallen på ett korrekt sätt med rätt version. Börja om från steg 5 ovan.

11. På fliken **Säkerhet** lägger du till säkerhetsgruppen som du vill ge åtkomsten **Registrera**. Om du till exempel vill ge alla användare åtkomst väljer du gruppen **Autentiserade användare** och sedan **Registreringsbehörigheter** för dessa.

12. Klicka på **OK** för att slutföra ändringarna och skapa den nya mallen. Du ska se din nya mall i listan med certifikatmallar.

13. Välj **Arkiv** och klicka på **Lägg till/ta bort snapin-modulen** för att lägga till snapin-modulen för certifikatutfärdaren till MMC-konsolen. När du tillfrågas om vilken dator du vill hantera väljer du **Lokal dator**.

14. Expandera **Certifikatutfärdare (lokal)** i rutan till vänster i MMC och expandera sedan din certifikatutfärdare i listan med certifikatutfärdare.

15. Högerklicka på **Certifikatmallar**, klicka på **Ny &gt; certifikatmall** för att utfärda.

16. Välj den nya mallen du har skapat i listan och klicka på **OK**.

## Skapa en profilmall
När du skapar en profilmall ska du se till att du konfigurerar den att skapa/ta bort det virtuella smartkortet och att ta bort insamlade data. CM-appen kan inte hantera insamlade data, så det är viktigt att inaktivera den. Det gör du så här:

1.  Logga in på CM-portalen som en användare med administratörsbehörighet.

2.  Gå till Administration &gt; Hantera profilmallar och markera rutan bredvid MIM CM Exempelprofilmall för smartkort och klicka sedan på Kopiera en markerad profilmall.

3.  Skriv in profilmallens namn och klicka på **OK**.

4.  På nästa skärm klickar du på **Lägg till ny certifikatmall**. Se till att markera kryssrutan bredvid namnet på Certifikatutfärdaren.

5.  Markera rutan bredvid namnet på profilmallen **Inloggning** och klicka på **Lägg till**.

6.  Ta bort mallen för smartkortsinloggning genom att markera rutan bredvid den och klicka på **Ta bort markerade certifikatmallar** och sedan på **OK**.

7.  Bläddra ned till slutet av sidan och klicka på **Ändra inställningar**.

8.  Markera kryssrutorna bredvid **Skapa/ta bort virtuellt smartkort** och ** Distribuera admin-nyckel**.

9. Under **Princip för användar-PIN** väljer du **Anges av användaren**.

10. I den vänstra rutan klickar du på **Förnya princip &gt; Ändra allmänna inställningar**. Välj **Återanvänd kort vid förnyelse** och klicka på **OK**.

11. Du måste inaktivera datainsamlingsobjekt för varje princip genom att klicka på principen i den vänstra rutan, markera kryssrutan bredvid **Exempeldataobjekt** och sedan klicka på **Ta bort datainsamlingsobjekt**. Klicka sedan på **OK**.

## Förbereda CM-appen för distribution

1.  Kör följande kommando i kommandotolken för att packa upp appen och extrahera innehållet i en ny undermapp med namnet appx och skapa en kopia så att du inte ändrar den ursprungliga filen.

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  Ändra namnet på den fil som heter CustomDataExample.xml till Custom.data i mappen appx

3.  Öppna Custom.data-filen och ändra parametrarna efter behov.

    |||
    |-|-|
    |URL för MIMCM|FQDN för portalen som du använde för att konfigurera CM. Till exempel https://mimcmServerAddress/certificatemanagement|
    |URL för ADFS|Ange URL:en för din AD FS om du tänker använda AD FS. Till exempel https://adfsServerSame/adfs|
    |PrivacyUrl|Du kan inkludera en webbadress till en sida som förklarar hanteringen av användarinformation som samlas in för certifikatregistrering.|
    |SupportMail|Du kan ange en e-postadress för supportfrågor.|
    |LobComplianceEnable|Du kan ställa in den här variabeln på sant eller falskt. Som standard är den inställd på sant.|
    |MinimumPinLength|Som standard är den inställd på 6.|
    |NonAdmin|Du kan ställa in den här variabeln på sant eller falskt. Som standard är den inställd på falskt. Ändra bara det här om du vill att användare som inte är administratörer på sina datorer ska kunna registrera och förnya certifikat.|

4.  Spara filen och avsluta redigeraren.

5.  Vid signeringen av paketet skapas en signeringsfil, så du måste ta bort den ursprungliga signeringsfilen med namnet AppxSignature.p7x.

6.  Filen AppxManifest.xml anger signeringscertifikatets ämnesnamn. Öppna filen för att redigera den.

7.  Du måste skaffa ett signeringscertifikat innan du startar det här avsnittet. Se steg 1 nedan, Aktivering av smartkort-förnyelse för icke-administratörer i MIM 2016 Certificate Manager.

8.  I elementet &lt;Identitet&gt; ändrar du värdet för attributet Publisher, så att det blir identiskt med ämnet som anges i signeringscertifikatet, till exempel ”CN = ÄMNE”.

9. Spara filen och avsluta redigeraren.

10. I Kommandotolken kör du följande kommando för att packa och signera .appx-filen på nytt,

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    där appaketets namn är samma namn du använde när du skapade kopian.

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Detta ger den nya .appx-filen. Pfx-filen innehåller en plats för signeringscertifikatet och ett lösenord för PFX-filen.

11. Arbeta med AD FS-autentisering:

    -   Starta appen för det virtuella smartkortet. På så sätt blir det lättare att hitta de värden som behövs för nästa steg.

    -   För att lägga till appen som en klient på AD FS-servern och konfigurera CM på servern öppnar du Windows PowerShell på AD FS-servern och kör kommandot `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`

        Följande är ConfigureMimCMClientAndRelyingParty.ps1-skriptet:

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   Uppdatera värdena för redirectUri och serverFQDN.

    -   Öppna den virtuella smartkortappens inställningssida för att hitta redirectUri genom att klicka på **Inställningar** och hitta omdirigerings-URI i listan under AD FS-serveradressfältet. URI:n visas endast om en adress för AD FS-servern har konfigurerats.

    -   serverFQDN kan endast vara MIMCM-serverns fullständiga datornamn.

    -   För att få hjälp med skriptet **ConfigureMIimCMClientAndRelyingParty.ps1** kör `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`

## Distribuera appen
När du konfigurerar CM-appen hämtar du filen MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip i Download Center och extraherar allt dess innehåll. .appx-filen är installationsprogrammet. Du kan distribuera den på samma sätt du normalt distribuerar appar från Windows Store, genom att använda [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx) eller [Intune](https://technet.microsoft.com/library/dn613839.aspx) för att läsa in appen separat så att användare kan få åtkomst till den genom företagsportalen eller genom att den överförs direkt till deras datorer.



<!--HONumber=Jun16_HO4-->


