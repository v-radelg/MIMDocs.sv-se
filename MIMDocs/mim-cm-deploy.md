---
title: Distribuera Microsoft Identity Manager Certificate Manager | Microsoft Docs
description: Installera Microsoft Identity Manager 2016 Certificate Manager
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9a9e00f7dca118627a5140967a104d13273cbc26
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690805"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Distribuera Microsoft Identity Manager Certificate Manager 2016 (MIM CM)

Installationen av Microsoft Identity Manager Certificate Manager 2016 (MIM CM) omfattar ett antal steg. Som ett sätt att förenkla processen bryter vi ned saker. Det finns preliminärt steg som måste vidtas innan eventuella faktiska MIM CM-åtgärder. Utan preliminär arbetet troligen installationen att misslyckas.

Diagrammet nedan visar ett exempel på typ av miljö som kan användas. System med siffror som ingår i listan nedan i diagram och krävs för att slutföra stegen som beskrivs i den här artikeln. Slutligen används Windows 2016 Datacenter-servrar:

![Miljö-diagram](media/mim-cm-deploy/image001.png)

1. CORPDC – domänkontrollant
2. CORPCM – MIM CM-servern
3. CORPCA – certifikatutfärdare
4. CORPCMR – MIM CM Rest API webb – CM portalen-för Rest-API – används för senare
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – domänanslutna Windows 10

## <a name="deployment-overview"></a>Distributionsöversikt

- Grundläggande operativsystem installation

    Labbet består av windows 2016 Datacenter-servrar.

    >[!NOTE]
    >För mer information om plattformar som stöds för MIM 2016 ta en titt på artikeln [plattformar som stöds för MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Fördistributionssteg

    - [Utöka schemat](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Skapa tjänstkonton

    - [Skapa certifikatmallar](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Konfigurera Kerberos

    - Databasrelaterade steg

        - Krav för SQL-konfiguration

        - Databasbehörighet

2. Distribution

## <a name="pre-deployment-steps"></a>Fördistributionssteg

Guiden för konfiguration av MIM CM kräver information som anges på vägen för att den har slutförts.

![diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Utöka schemat

Processen med att utöka schemat är enkelt, men måste vara närmat sig med försiktighet eftersom det inte går att ångra handlar.

>[!NOTE]
>Det här steget kräver att det konto som används har administratörsrättigheter för schemat.

1. Bläddra till platsen för MIM-media och gå till \\certifikathantering\\x64 mapp.

2. Kopiera mappen schemat till CORPDC och gå sedan till den.

    ![diagram](media/mim-cm-deploy/image005.png)

3. Köra skript resourceForestModifySchema.vbs enkel skog scenariot. Kör skripten för resursen skog scenariot:
   - DomainA – Users located (userForestModifySchema.vbs)
   - ResourceForestB – platsen för CM-installation (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Schemaändringar är en enkelriktad åtgärd och kräver en skog återställning för att återställa så se till att du har nödvändiga säkerhetskopior. För information om ändringar som görs i schemat genom att utföra den här åtgärden granska artikeln [schemaändringar för Forefront Identity Manager 2010 certifikat Management](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![diagram](media/mim-cm-deploy/image007.png)

4. Kör skriptet och du bör få lyckas visas en gång att skriptet har körts.

    ![Meddelande om att något har slutförts](media/mim-cm-deploy/image009.png)

Schemat i AD nu har utökats för att stödja MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Skapa service-konton och grupper

I följande tabell sammanfattas de konton och behörigheter som krävs av MIM CM. Du kan tillåta MIM CM skapa följande konton automatiskt eller kan du skapa dem före installationen. De faktiska kontonamn som kan ändras. Om du skapar konton själv, Överväg att namnge användarkonton i, till exempel direkt att det är enkelt att matcha namnet på användarkontot framtagen för dess funktion.

Användare:

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Roll**                   | **Användarens inloggningsnamn** |
|----------------------------|---------------------|
| MIM CM-Agent               | MIMCMAgent          |
| MIM CM nyckelåterställningsagenten  | MIMCMKRAgent        |
| MIM CM-Agent för auktorisering | MIMCMAuthAgent      |
| MIM CM CA Manager-agenten    | MIMCMManagerAgent   |
| MIM CM Pool Webbagent      | MIMCMWebAgent       |
| MIM CM-registreringsagent    | MIMCMEnrollAgent    |
| MIM CM-uppdateringstjänst      | MIMCMService        |
| Kontot för MIM-installation        | MIMINSTALL          |
| Help Desk-agenten            | CMHelpdesk1-2       |
| CM-Manager                 | CMManager1-2        |
| Prenumerant användare            | CMUser1-2           |

Grupper:

| **Roll**               | **Grupp**         |
|------------------------|-------------------|
| CM-supportavdelningen medlemmar    | MIMCM-Helpdesk    |
| CM Manager medlemmar     | MIMCM-Managers    |
| CM-prenumeranter medlemmar | MIMCM-prenumeranter |

PowerShell: Agentkonton:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Uppdatera **CORPCM** Server lokala principen om Agentkonton 

| **Användarens inloggningsnamn** | **Beskrivning och behörigheter**   |
|------|---------------------|
| MIMCMAgent          | Tillhandahåller följande tjänster: </br>-Hämtar krypterad privata nycklar från Certifikatutfärdaren. </br>-Skyddar information om smartkort PIN-kod i FIM CM-databasen. </br>-Skyddar kommunikationen mellan FIM CM och CA: N. </br></br> Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br>-   **Tillåt lokal inloggning** användarrättigheten.</br>-   **Utfärda och hantera certifikat** användarrättigheten. </br>-Läs- och skrivbehörighet för system Temp-mappen på följande plats: % WINDIR %\\Temp.</br>– En digital signatur och kryptering certifikat utfärdas och installeras i användararkivet.
|MIMCMKRAgent        | Återställs arkiverade privata nycklar från Certifikatutfärdaren. Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br> -   **Tillåt lokal inloggning** användarrättigheten.</br>-Medlemskap i lokalt **administratörer** grupp. </br>-Behörighet för registrering på den **KeyRecoveryAgent** certifikatmall. </br>-Nyckelåterställningsagenten certifikatet utfärdas och installeras i användararkivet. Certifikatet måste läggas till i listan över återställning av nyckel-agenterna på ca: N. </br>-Läsbehörighet och skrivbehörighet för system Temp-mappen på följande plats: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Anger rättigheter och behörigheter för användare och grupper. Det här användarkontot kräver följande inställningar för åtkomstkontroll: </br>-Medlemskap i gruppen Kompatibel åtkomst innan Windows 2000-domän. </br> -Beviljas den **skapa säkerhetsgranskningar** användarrättigheten.             |
| MIMCMManagerAgent   | Utför hanteringsaktiviteter för CA: N. </br> Den här användaren måste tilldelas behörighet för att hantera Certifikatutfärdare.        |
| MIMCMWebAgent       | Ger identiteten för IIS-programpoolen. FIM CM körs inom en Microsoft Win32® application programming interface process som använder den här användarens autentiseringsuppgifter. </br> Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br> -Medlemskap i lokalt **IIS_WPG, windows 2016 = IIS_IUSRS** grupp. </br>-Medlemskap i lokalt **administratörer** grupp.</br>-Beviljas den **skapa säkerhetsgranskningar** användarrättigheten. </br>-Beviljas den **agera som del av operativsystemet** användarrättigheten. </br>-Beviljas den **Ersätt processnivå-token** användarrättigheten.</br>-Tilldelade som identitet för IIS-programpoolen **CLMAppPool**. </br>-   Beviljas läsbehörighet på den **HKEY_LOCAL_MACHINE\\programvara\\Microsoft\\CLM\\v1.0\\Server\\///användare** registernyckeln. </br>– Det här kontot måste också vara betrodd för delegering.|
| MIMCMEnrollAgent    | Utför registreringen för en användares räkning. Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br>– Ett registreringsagent-certifikat som utfärdas och installeras i användararkivet.</br>-   **Tillåt lokal inloggning** användarrättigheten. </br>-Behörighet för registrering på den **registreringsagent** certifikatmall (eller den anpassade mallen, om sådan används).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Skapa mallar för MIM CM-tjänstkonton

Tre av tjänstkonton används av MIM CM kräver ett certifikat och guiden för konfiguration av kräver att du anger namnet på mallarna certifikat som den ska använda för att begära certifikat för dessa.

Tjänstkonton som kräver certifikat är:

- MIMCMAgent: Det här kontot måste ha ett användarcertifikat

- MIMCMEnrollAgent: Det här kontot måste ha ett registreringsagent-certifikat

- MIMCMKRAgent: Det här kontot måste en **nyckelåterställningsagenten** certifikat

Det finns mallar som redan finns i AD, men vi måste skapa egna versioner för att arbeta med MIM CM. Eftersom vi behöver göra ändringar från de ursprungliga baslinje-mallarna.

Alla tre av ovanstående konton kommer ha utökade rättigheter i din organisation och bör hanteras försiktigt.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Skapa certifikatmall MIM CM-signering

1. Från **Administrationsverktyg**öppnar **certifikatutfärdare**.

2. I den **certifikatutfärdare** i konsolträdet i konsolen expanderar **Contoso CorpCA**, och klicka sedan på **certifikatmallar**.

3. Högerklicka på **certifikatmallar**, och klicka sedan på **hantera**.

4. I den **konsol för certifikatmallar**i den **information** rutan, markera och högerklicka på **användaren**, och klicka sedan på **duplicera mall** .

5. I den **kopiera mallen** dialogrutan **Windows Server 2003 Enterprise**, och klicka sedan på **OK**.

    ![Visa resulterande ändringar](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM fungerar inte med certifikat baserat på version 3 certifikatmallar. Du måste skapa en certifikatmall för Windows Server® 2003 Enterprise (version 2). Se [V3 information Öppna](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) den certifikathanteringx64 directory.

6. I den **egenskaper för ny mall** dialogrutan den **Allmänt** fliken den **mallvisningsnamnet** skriver **MIM CM-signering**. Ändra den **giltighetsperioden** till **2 år**, och avmarkera de **Publicera certifikatet i Active Directory** markerar du kryssrutan.

7. På den **hantering av begäranden** fliken, se till att den **Tillåt att den privata nyckeln exporteras** kryssrutan är markerad och klicka sedan på **fliken kryptografi**.

8. I den **kryptografi val av** dialogrutan inaktivera **Microsoft förbättrad kryptografiprovider v1.0**, aktivera **Microsoft Enhanced RSA och AES kryptografiprovider**, och klicka sedan på **OK**.

9. På den **ämnesnamn** fliken rensa den **Inkludera e-postnamnet i ämnesnamnet** och **e-postnamn** kryssrutorna.

10. På den **tillägg** fliken den **tillägg som ingår i den här mallen** Kontrollera att **användningsprinciper** är markerad och klicka sedan på **redigera** .

11. I den **Redigera tillägg för användningsprinciper** dialogrutan, Välj den **Krypterande filsystem** och **säker e-post** principer för program. Klicka på **ta bort**, och klicka sedan på **OK**.

12. På den **Security** fliken utför följande steg:

    - Ta bort **administratör**.

    - Ta bort **Domänadministratörer**.

    - Ta bort **domänanvändare**.

    - Tilldela bara **Läs** och **skriva** behörighet att **Företagsadministratörer**.

    - Lägg till **MIMCMAgent.**

    - Tilldela **Läs** och **registrera** behörighet att **MIMCMAgent**.

13. I den **egenskaper för ny mall** dialogrutan klickar du på **OK**.

14. Lämna den **konsol för certifikatmallar** öppna.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Skapa MIM CM-registreringsagent certifikatmall

1. I den **konsol för certifikatmallar**i den **information** rutan, markera och högerklicka på **registreringsagent**, och klicka sedan på **duplicera mall**.

2. I den **kopiera mallen** dialogrutan **Windows Server 2003 Enterprise**, och klicka sedan på **OK**.

3. I den **egenskaper för ny mall** dialogrutan den **Allmänt** fliken den **mallvisningsnamnet** skriver **MIM CM-registreringsagent**. Se till att den **giltighetsperioden** är **2 år**.

4. På den **hantering av begäranden** fliken måste du aktivera **Tillåt att den privata nyckeln exporteras**, och klicka sedan på **CSP: er eller fliken kryptografi.**

5. I den **val av Kryptografiprovider** dialogrutan inaktivera **Microsoft grundläggande kryptografiprovider v1.0**, inaktivera **Microsoft förbättrad kryptografiprovider v1.0**, aktivera  **Microsoft Enhanced RSA och AES kryptografiprovider**, och klicka sedan på **OK**.

6. På den **Security** fliken gör du följande:

    - Ta bort **administratör**.

    - Ta bort **Domänadministratörer**.

    - Tilldela bara **Läs** och **skriva** behörighet att **Företagsadministratörer**.

    - Lägg till **MIMCMEnrollAgent**.

    - Tilldela **Läs** och **registrera** behörighet att **MIMCMEnrollAgent**.

7. I den **egenskaper för ny mall** dialogrutan klickar du på **OK**.

8. Lämna den **konsol för certifikatmallar** öppna.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Skapa MIM CM nyckelåterställningsagenten certifikatmall

1. I den **certifikatmallar** konsolen på den **information** rutan, markera och högerklicka på **nyckelåterställningsagenten**, och klicka sedan på **duplicera mall**.

2. I den **kopiera mallen** dialogrutan **Windows Server 2003 Enterprise**, och klicka sedan på **OK**.

3. I den **egenskaper för ny mall** dialogrutan den **Allmänt** fliken den **mallvisningsnamnet** skriver **MIM CM nyckelåterställningsagenten**. Se till att den **giltighetsperioden** är **2 år** på den **fliken kryptografi.**

4. I den **Providers val av** dialogrutan inaktivera **Microsoft förbättrad kryptografiprovider v1.0**, aktivera **Microsoft Enhanced RSA och AES kryptografiprovider**, och klicka sedan på **OK**.

5. På den **krav för utfärdande** fliken, se till att **Certifikatutfärdarens certifikathanterare godkänner registreringen** är **inaktiverat**.

6. På den **Security** fliken gör du följande:

    - Ta bort **administratör**.

    - Ta bort **Domänadministratörer**.

    - Tilldela bara **Läs** och **skriva** behörighet att **Företagsadministratörer**.

    - Lägg till **MIMCMKRAgent**.

    - Tilldela **Läs** och **registrera** behörighet att **KRAgent**.

7. I den **egenskaper för ny mall** dialogrutan klickar du på **OK**.

8. Stäng **konsolen för certfikatmallar**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicera de nödvändiga certifikatmallarna på certifikatutfärdaren

1. Återställa den **certifikatutfärdare** konsolen.

2. I den **certifikatutfärdare** -konsolen i konsolträdet högerklickar du på **certifikatmallar**, peka på **New**, och klicka sedan på **certifikat Mallen ska utfärdas**.

3. I den **Aktivera certifikatmallar** dialogrutan **MIM CM-registreringsagent**, **MIM CM nyckelåterställningsagenten**, och **MIM CM signering**. Klicka på **OK**.

4. I konsolträdet klickar du på **certifikatmallar**.

5. Kontrollera att de tre nya mallarna visas i den **information** fönstret och stäng sedan **certifikatutfärdare**.

    ![MIM CM-signering](media/mim-cm-deploy/image016.png)

6. Stäng alla öppna windows och logga ut.

### <a name="iis-configuration"></a>IIS-konfiguration

För att kunna vara värd för webbplatsen för CM, installera och konfigurera IIS.

#### <a name="install-and-configure-iis"></a>Installera och konfigurera IIS

1. Logga in på **CORLog i** som **MIMINSTALL** konto

    >[!IMPORTANT]
    >Kontot för MIM-installation bör vara lokal administratör

2. Öppna PowerShell och kör följande kommando

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>En plats med namnet Default Web Site installeras som standard med IIS 7. Om platsen har bytt namn eller ta bort en webbplats med namnet måste Default Web Site finnas innan du kan installera MIM CM.

#### <a name="configuring-kerberos"></a>Konfigurera Kerberos

Kontot MIMCMWebAgent körs MIM CM-portalen. Som standard i IIS och konfigurera kernel används läge-autentisering i IIS som standard. Du kan inaktivera Kerberos-autentisering för kernelläge och konfigurera SPN för kontot MIMCMWebAgent i stället. Vissa kommandot kräver förhöjd behörighet i active directory och CORPCM server.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Uppdatera IIS på CORPCM**

![diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Du måste lägga till en DNS A Record för ”cm.contoso.com” och peka på CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>SSL-kravet på MIM CM-portalen

Vi rekommenderar starkt att du kräva SSL på MIM CM-portalen. Om du ännu inte guiden att varna dig om den.

1. Registrera dig i certifikat för webbprogram för **cm.contoso.com** tilldela till standardwebbplatsen

2. Öppna **IIS-hanteraren** och navigera till **certifikathantering**

3. Dubbelklicka på SSL-inställningar i funktioner visas.

4. På sidan SSL-inställningar väljer **Kräv SSL**.

5. I åtgärdsfönstret klickar du på **tillämpa.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Databaskonfiguration **CORPSQL** för MIM CM

1. Se till att du är ansluten till CORPSQL01-servern.

2. Se till att du är inloggad som SQL DBA.

3. Kör följande T-SQL-skript för att tillåta CONTOSOS\\MIMINSTALL-konto för att skapa databasen när vi går du till konfigurationssteg

    >[!NOTE]
    >Vi kommer att behöva gå tillbaka till SQL när vi är redo för Avsluta & princip-modulen

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![MIM CM-konfiguration guiden felmeddelande](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Distribution av Microsoft Identity Manager 2016-certifikathantering

1. Kontrollera att du är ansluten till servern CORPCM och som den **MIMINSTALL** kontot är medlem i den **lokala administratörer** grupp.

2. Se till att du är inloggad som Contoso\\MIMINSTALL.

3. Montera ISO för Microsoft Identity Manager SP1.

4. **Öppna** den **certifikathantering\\x64** directory.

5. I den **x64** fönstret högerklickar du på **installationsprogrammet**, och klicka sedan på **kör som administratör**.

6. Välkommen till installationsguiden för Microsoft Identity Manager Certificate Management, klicka på **nästa.**

7. På sidan Licensavtal läsa avtalet, aktivera den jag accepterar villkoren i licensavtalet **kryssrutan**, och klicka sedan på Nästa.

8. Kontrollera på sidan anpassad installation den **MIM CM-portalen** och **MIM CM-uppdateringstjänst komponenter** är inställda på att installeras, och sedan **Klicka på nästa**.

9. På sidan virtuella webbmapp att se till att namnet på virtuella mappen är **CertificateManagement**, och sedan **Klicka på nästa**.

10. På sidan Installera Microsoft Identity Manager Certificate Management **klickar du på Installera**.

11. På den **slutförd** installationsguiden för Microsoft Identity Manager Certificate Management-sidan **Klicka på Slutför**.

![MIM CM-guiden har slutförts](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Guiden för konfiguration av Microsoft Identity Manager 2016-certifikathantering

Innan du loggar in till CORPCM Lägg till MIMINSTALL till **domän administratörer, Schemaadministratörer och lokala administratörer** för konfigurationsguiden. Detta kan tas bort senare när konfigurationen är klar.

![Felmeddelande](media/mim-cm-deploy/image028.png)

1. Från den **starta** -menyn klickar du på **guiden för certifikat Management Config**. Och köra som **administratör**

2. På den **Välkommen till guiden för konfiguration av** klickar du på **nästa**.

3. På den **certifikatutfärdarkonfiguration** kontrollerar du att den valda Certifikatutfärdaren är **Contoso-CORPCA-CA**, se till att den valda servern är **CORPCA. CONTOSO.COM**, och klicka sedan på **nästa**.

4. På den **ställa in Microsoft® SQL Server®-databas** sidan den **SQL Server** skriver **CORPSQL1** , aktivera den **använda min inloggningsinformation för att skapa den databasen** och klicka sedan på **nästa**.

5. På den **databasinställningar** godkänner standarddatabasnamnet för **FIMCertificateManagement**, se till att **SQL-integrerad autentisering** väljs, och sedan Klicka på **nästa**.

6. På den **konfigurera Active Directory** , Godkänn standardnamnet för tjänstanslutningspunkten, och klicka sedan på **nästa**.

7. På den **autentiseringsmetod** Bekräfta **windows-integrerad autentisering** är markerad och klicka sedan på **nästa**.

8. På den **agenter – FIM CM** avmarkerar den **använda standardinställningarna för FIM CM** och klicka sedan på **anpassade konton**.

9. I den **agenter – FIM CM** flera flikar dialogrutan på varje flik, skriver du följande information:

   - Användarnamn: **Uppdatera**

   - Lösenord: **Skicka\@word1**

   - Bekräfta lösenord: **Skicka\@word1**

   - Använd en befintlig användare: **Aktiverad**

     >[!NOTE]
     >Dessa konton som vi skapade tidigare. Se till att procedurerna i steg 8 upprepas för alla sex agenten konto flikar.

     ![MIM CM-konton](media/mim-cm-deploy/image030.png)

10. När alla kontoinformation för agenten är klar klickar du på **OK**.

11. På den **agenter – MIM CM** klickar du på **nästa**.

12. På den **ställa in servercertifikat** kan du aktivera följande certifikatmallar:

    - Certifikatmall som ska användas för återställning nyckelåterställningsagenten agentcertifikatet: **MIMCMKeyRecoveryAgent**.

    - Certifikatmall som ska användas för FIM CM-Agent-certifikat: **MIMCMSigning**.

    - Certifikatmall som ska användas för registreringscertifikat för agenten: **FIMCMEnrollmentAgent**.

13. På den **ställa in servercertifikat** klickar du på **nästa**.

14. På den **installationsprogrammet e-postservern, skriva ut dokumentet** sidan den **anger du namnet på SMTP-servern som du vill använda till e-post registrering meddelanden** rutan och klicka sedan på **nästa.**

15. På den **redo att konfigurera** klickar du på **konfigurera**.

16. I den **konfigurationsguiden – Microsoft Forefront Identity Manager 2010 R2** varning dialogrutan klickar du på **OK** att bekräfta att SSL inte är aktiverat på den virtuella katalogen för IIS.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Klicka inte på Slutför tills körningen av guiden har slutförts. Loggning för guiden hittar du här: **% programfiles%\\Microsoft Forefront Identity Management\\2010\\certifikathantering\\config.log**

17. Klicka på **Slutför**.

    ![MIM CM-guiden har slutförts](media/mim-cm-deploy/image033.png)

18. Stäng alla öppna fönster.

19. Lägg till `https://cm.contoso.com/certificatemanagement` till zonen Lokalt intranät i webbläsaren.

20. På den `https://cm.contoso.com/certificatemanagement`licensavtalet Läs avtalet.  

    ![diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Kontrollera CNG Key Isolation-tjänst

1. Från **Administrationsverktyg**öppnar **Services**.

2. I den **information** fönstret dubbelklickar du på **CNG Key Isolation**.

3. På den **Allmänt** fliken, ändrar den **starttyp** till **automatisk**.

4. På den **Allmänt** fliken genom att starta tjänsten om den inte är i tillståndet igång.

5. På den **Allmänt** fliken **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Installera och konfigurera CA-moduler:

I det här steget ska vi installera och konfigurera FIM CM CA-moduler på certifikatutfärdaren.

1. Konfigurera FIM CM om du vill bara kontrollera användarbehörigheterna för hanteringsåtgärder

2. I den **C:\\programfiler\\Microsoft Forefront Identity Manager\\2010\\certifikathantering\\web** fönstret gör en kopia av  **Web.config** naming kopian **web.1.config**.

3. I den **Web** fönstret högerklickar du på **Web.config**, och klicka sedan på **öppna**.

    >[!Note]
    >Web.config-filen öppnas i anteckningar

4. Tryck på CTRL + F när filen öppnas.

5. I den **Sök och Ersätt** i dialogrutan den **Sök efter** skriver **UseUser**, och klicka sedan på **Sök nästa** tre gånger.

6. Stäng den **Sök och Ersätt** dialogrutan.

7. Du bör vara på raden **\<lägga till key="Clm.RequestSecurity.Flags” value = ”UseUser UseGroups” /\>** . Ändra raden till  **\<lägga till key="Clm.RequestSecurity.Flags” värde = ”UseUser” /\>** .

8. Stäng filen, sparar alla ändringar.

9. Skapa ett konto för CA-datorn på SQLServer \<inget skript\>

10. Kontrollera att du är ansluten till den **CORPSQL01** server.

11. Se till att du är inloggad som **DBA**

12. Från den **starta** menyn, starta **SQL Server Management Studio**.

13. I den **Anslut till Server** i dialogrutan den **servernamn** skriver **CORPSQL01,** och klicka sedan på **Connect**.

14. I konsolträdet expanderar **Security**, och klicka sedan på **inloggningar**.

15. Högerklicka på **inloggningar**, och klicka sedan på **ny inloggning**.

16. På den **Allmänt** sidan den **inloggningsnamn** skriver **contoso\\CORPCA\$** . Välj **Windows-autentisering**. Standarddatabasen är **FIMCertificateManagement**.

17. I den vänstra rutan väljer **användarmappning**. I den högra rutan klickar du på kryssrutan i den **kartan** kolumnen bredvid **FIMCertificateManagement**. I den **databas rollmedlemskap för: FIMCertificateManagement** listan måste du aktivera den **clmApp** roll.

18. Klicka på **OK**.

19. Stäng **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Installera FIM CM CA-moduler på certifikatutfärdaren

1. Kontrollera att du är ansluten till den **CORPCA** server.

2. I den **X64** windows, högerklicka på **Setup.exe**, och klicka sedan på **kör som administratör**.

3. På den **Välkommen till installationsguiden för Microsoft Identity Manager Certificate Management** klickar du på **nästa**.

4. På den **licensavtalet för slutanvändare** läser avtalet. Välj den **jag accepterar villkoren i licensavtalet** och klicka sedan på **nästa**.

5. På den **anpassad installation** väljer **MIM CM-portalen**, och klicka sedan på **den här funktionen är inte tillgängliga**.

6. På den **anpassad installation** väljer **MIM CM-uppdateringstjänst**, och klicka sedan på **den här funktionen inte är tillgänglig**.

    >[!Note]
    >Detta lämnar MIM CM CA-filer som funktionen endast är aktiverad för installationen.

7. På den **anpassad installation** klickar du på **nästa**.

8. På den **installera Microsoft Identity Manager Certificate Management** klickar du på **installera**.

9. På den **slutföra Installationsguiden för Microsoft Identity Manager Certificate Management** klickar du på **Slutför**.

10. Stäng alla öppna fönster.

### <a name="configure-the-mim-cm-exit-module"></a>Konfigurera MIM CM stängningsmodul

1. Från **Administrationsverktyg**öppnar **certifikatutfärdare**.

2. I konsolträdet högerklickar du på **contoso-CORPCA-CA**, och klicka sedan på **egenskaper**.

3. På den **Avslutsmodulen** fliken **FIM CM-Avslutsmodulen**, och klicka sedan på **egenskaper**.

4. I den **ange anslutningssträngen för databasen för CM** skriver **Connect Timeout = 15. Spara säkerhetsinformation = True; Integrerad säkerhet = sspi; Initial Catalog = FIMCertificateManagement; Data Source = CORPSQL01**. Lämna den **kryptera anslutningssträngen** kryssrutan aktiverad och klicka sedan på **OK**.
5. I den **Microsoft FIM-certifikathantering** meddelanderutan, klickar du på **OK**.

6. I den **contoso-CORPCA-certifikatutfärdaregenskaper** dialogrutan klickar du på **OK**.

7. Högerklicka på **contoso-CORPCA-CA** *,* pekar **alla uppgifter**, och klicka sedan på **stoppa tjänsten**. Vänta tills Active Directory Certificate Services stoppas.

8. Högerklicka på **contoso-CORPCA-CA** *,* pekar **alla uppgifter**, och klicka sedan på **starta tjänsten**.

9. Minimera den **certifikatutfärdare** konsolen.

10. Från **Administrationsverktyg**öppnar **Loggboken**.

11. I konsolträdet expanderar **program- och tjänstloggar**, och klicka sedan på **FIM-certifikathantering**.

12. I listan över händelser, kontrollerar du att de senaste händelserna gör *inte* ta med eventuella **varning** eller **fel** händelser sedan den senaste omstarten av Certificate Services.

    >[!NOTE] 
    >Den sista händelsen bör tillstånd att Avslutsmodulen in med hjälp av inställningarna från: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimera den **Loggboken**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Kopiera MIMCMAgent certifikatets tumavtryck till Windows® Urklipp

1. Återställa den **certifikatutfärdare** konsolen.

2. I konsolträdet expanderar **contoso-CORPCA-CA**, och klicka sedan på **utfärdade certifikat**.

3. I den **information** fönstret dubbelklickar du på certifikatet med **CONTOSO\\MIMCMAgent** i den **namn på beställare** kolumn och med **FIM CM Signering** i den **certifikatmall** kolumn.

4. På den **information** fliken den **tumavtryck** fält.

5. Välj tumavtrycket och tryck på CTRL + C.

    >[!NOTE]
    >Gör **inte** innehåller inledande blanksteg i listan över tumavtryck tecken.

6. I den **certifikat** dialogrutan klickar du på **OK**.

7. Från den **starta** menyn i den **Sök program och filer** skriver **anteckningar**, och tryck sedan på RETUR.

8. I **anteckningar**, från den **redigera** -menyn klickar du på **klistra in**.

9. Från den **redigera** -menyn klickar du på **Ersätt**.

10. I den **Sök efter** rutan, skriver du ett blanksteg och klickar sedan på **Ersätt alla**.

    >[!Note]
    >Detta tar bort alla blanksteg mellan tecken i certifikatets tumavtryck.

11. I den **Ersätt** dialogrutan klickar du på **Avbryt**.

12. Välj den konverterade *thumbprintstring*, och tryck på CTRL + C.

13. Stäng **anteckningar** utan att spara ändringarna.

### <a name="configure-the-fim-cm-policy-module"></a>Konfigurera principmodulen för FIM CM

1. Återställa den **certifikatutfärdare** konsolen.

2. Högerklicka på **contoso-CORPCA-CA**, och klicka sedan på **egenskaper**.

3. I den **contoso-CORPCA-certifikatutfärdaregenskaper** dialogrutan den **principmodulen** fliken **egenskaper**.

    - På den **Allmänt** fliken, se till att **skickar FIM CM-begäranden till standard-principmodulen för bearbetning** har valts.

    - På den **signeringscertifikat** fliken **Lägg till**.

    - I dialogrutan certifikat högerklickar du på den **ange hex-kodat certifikat-hash** rutan och klicka sedan på **klistra in**.

    - I den **certifikat** dialogrutan klickar du på **OK**.

        >[!Note]
        >Om den **OK** knappen är inte aktiverad, du av misstag med ett dolda tecken i tumavtryckssträngen när du har kopierat tumavtrycket från clmAgent certifikatet. Upprepa alla steg som startar från **uppgift 4: Kopiera MIMCMAgent certifikatets tumavtryck till Windows Urklipp** i den här övningen.

4. I den **konfigurationsegenskaper** dialogrutan kontrollerar du att tumavtrycket visas i den **giltiga certifikat för signering** listan och klicka sedan på **OK**.

5. I den **FIM-certifikathantering** meddelanderutan, klickar du på **OK**.

6. I den **contoso-CORPCA-certifikatutfärdaregenskaper** dialogrutan klickar du på **OK**.

7. Högerklicka på **contoso-CORPCA-CA** *,* pekar **alla uppgifter**, och klicka sedan på **stoppa tjänsten**.

8. Vänta tills Active Directory Certificate Services stoppas.

9. Högerklicka på **contoso-CORPCA-CA** *,* pekar **alla uppgifter**, och klicka sedan på **starta tjänsten**.

10. Stäng den **certifikatutfärdare** konsolen.

11. Stäng alla öppna fönster och sedan loggar ut.

**Sista steget i distributionen** är vi vill vara säker på att CONTOSO\\MIMCM-hanterare kan distribuera och skapa mallar och konfigurera systemet utan att vara schemat och Domänadministratörer. Nästa skriptet kommer ACL behörighet att använda dsacls certifikatmallarna. Kör med konto som har fullständig behörighet för att ändra säkerhet Läs- och skrivbehörighet till varje befintlig certifikatmall i skogen.

Första stegen: **Hur du konfigurerar tjänstanslutningspunkten och målgruppen behörigheter & delegera mall profilhantering**

1. Konfigurera behörigheter för tjänstanslutningspunkt (SCP).

2. Konfigurera delegerade profilhantering för mallen.

3. Konfigurera behörigheter för tjänstanslutningspunkt (SCP). **\<Inget skript\>**

4.   Kontrollera att du är ansluten till den **CORPDC** virtuell server.

5. Logga in som **contoso\\corpadmin**

6. Från **Administrationsverktyg**öppnar **Active Directory-användare och datorer**.

7. I **Active Directory-användare och datorer**på den **visa** menyn, se till att **avancerade funktioner** är aktiverad.

8. I konsolträdet expanderar **Contoso.com** \| **System** \| **Microsoft** \| **certifikatets livscykel Manager**, och klicka sedan på **CORPCM**.

9. Högerklicka på **CORPCM**, och klicka sedan på **egenskaper**.

10. I den **CORPCM egenskaper** dialogrutan den **Security** fliken Lägg till följande grupper med motsvarande behörigheter:

    | Grupp          | Behörigheter      |
    |----------------|------------------|
    | mimcm-Managers | Läsa </br> FIM CM-granskning</br> FIM CM-registreringsagent</br> Registrera för FIM CM-begäran</br> Återställ för FIM CM-begäran</br> Förnya FIM CM-begäran</br> FIM CM-begäran återkalla </br> FIM CM-begäran Avblockera smartkort |
    | mimcm-HelpDesk | Läsa</br> FIM CM-registreringsagent</br> FIM CM-begäran återkalla</br> FIM CM-begäran Avblockera smartkort |

11. I den **CORPDC egenskaper** dialogrutan klickar du på **OK**.

12. Lämna **Active Directory-användare och datorer** öppna.

**Konfigurera behörigheter för de underordnade objekt**

1. Se till att du fortfarande är i den **Active Directory-användare och datorer** konsolen.

2. I konsolträdet högerklickar du på **Contoso.com**, och klicka sedan på **egenskaper**.

3. På den **Security** fliken **Avancerat**.

4. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **Lägg till**.

5. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm-chefer**, och klicka sedan på **OK**.

6. I den **Behörighetspost för Contoso** i dialogrutan den **avser** väljer **underordnad användarobjekt** och sedan aktivera den **Tillåt**markerar du kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **FIM CM-granskning**

    - **FIM CM-registreringsagent**

    - **Registrera för FIM CM-begäran**

    - **Återställ för FIM CM-begäran**

    - **Förnya FIM CM-begäran**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran Avblockera smartkort**

7. I den **Behörighetspost för Contoso** dialogrutan klickar du på **OK**.

8. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **Lägg till**.

9. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm supportavdelningen**, och klicka sedan på **OK**.

10. I den **Behörighetspost för Contoso** i dialogrutan den **avser** väljer **underordnad användarobjekt** och välj sedan den **Tillåt**markerar du kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **FIM CM-registreringsagent**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran Avblockera smartkort**

11. I den **Behörighetspost för Contoso** dialogrutan klickar du på **OK**.

12. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **OK**.

13. I den **contoso.com egenskaper** dialogrutan klickar du på **OK**.

14. Lämna **Active Directory-användare och datorer** öppna.

**Konfigurera behörigheter för de underordnade objekt \<inget skript\>**

1. Se till att du fortfarande är i den **Active Directory-användare och datorer** konsolen.

2. I konsolträdet högerklickar du på **Contoso.com**, och klicka sedan på **egenskaper**.

3. På den **Security** fliken **Avancerat**.

4. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **Lägg till**.

5. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm-chefer**, och klicka sedan på **OK**.

6. I den **Behörighetspost för CONTOSO** i dialogrutan den **avser** väljer **underordnad användarobjekt** och sedan aktivera den **Tillåt**markerar du kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **FIM CM-granskning**

    - **FIM CM-registreringsagent**

    - **Registrera för FIM CM-begäran**

    - **Återställ för FIM CM-begäran**

    - **Förnya FIM CM-begäran**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran Avblockera smartkort**

7. I den **Behörighetspost för CONTOSO** dialogrutan klickar du på **OK**.

8. I den **avancerade säkerhetsinställningar för CONTOSO** dialogrutan klickar du på **Lägg till**.

9. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm supportavdelningen**, och klicka sedan på **OK**.

10. I den **Behörighetspost för CONTOSO** i dialogrutan den **avser** väljer **underordnad användarobjekt** och välj sedan den **Tillåt**markerar du kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **FIM CM-registreringsagent**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran Avblockera smartkort**

11. I den **Behörighetspost för contoso** dialogrutan klickar du på **OK**.

12. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **OK**.

13. I den **contoso.com egenskaper** dialogrutan klickar du på **OK**.

14. Lämna **Active Directory-användare och datorer** öppna.

Andra steg: **Delegera hantering för Certifikatmallsbehörigheter \<skript\>**

- Delegera behörigheter för behållaren för certifikatmallar.

- Delegera behörigheter för OID-behållaren.

- Delegera behörigheter för befintliga certifikatmallarna.

Definiera behörigheter för behållaren för certifikatmallar:

1. Återställa den **Active Directory-platser och tjänster** konsolen.

2. I konsolträdet expanderar **Services**, expandera **Public Key Services**, och klicka sedan på **certifikatmallar**.

3. I konsolträdet högerklickar du på **certifikatmallar**, och klicka sedan på **delegera kontroll**.

4. I den **delegera kontroll** guiden klickar du på **nästa**.

5. På den **användare eller grupper** klickar du på **Lägg till**.

6. I den **Välj användare, datorer eller grupper** i dialogrutan den **ange objektnamn att välja** skriver **mimcm-chefer**, och klicka sedan på **OK**.

7. På den **användare eller grupper** klickar du på **nästa**.

8. På den **aktiviteter som ska delegeras** klickar du på **skapa en anpassad aktivitet och delegera**, och klicka sedan på **nästa**.

9.  På den **Active Directory-objekttyp** kontrollerar du att **den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen** är markerad och klicka sedan på **nästa**.

10. På den **behörigheter** sidan den **behörigheter** väljer den **fullständig kontroll** och klicka sedan på **nästa**.

11. På den **du har slutfört guiden Delegera kontroll** klickar du på **Slutför**.

Definiera behörigheter för OID-behållaren:

1. I konsolträdet högerklickar du på **OID**, och klicka sedan på **egenskaper**.

2. I den **OID egenskaper** dialogrutan den **Security** fliken **Avancerat**.

3. I den **avancerade säkerhetsinställningar för OID** dialogrutan klickar du på **Lägg till**.

4. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm-chefer**, och klicka sedan på **OK**.

5. I den **Behörighetspost för OID** dialogrutan ser du till att behörigheterna som tillämpas på **objektet och alla underordnade objekt**, klickar du på **fullständig kontroll**, och klicka sedan på  **OK**.

6. I den **avancerade säkerhetsinställningar för OID** dialogrutan klickar du på **OK**.

7. I den **OID egenskaper** dialogrutan klickar du på **OK**.

8. Stäng **Active Directory-platser och tjänster**.

**Skript: Behörigheter för behållaren OID, Profilmall & certifikatmallar**

![diagram](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Skript: Delegera behörigheter för befintliga certifikatmallarna.**  

![diagram](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
