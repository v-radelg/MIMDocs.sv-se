---
title: Distribuera Microsoft Identity Manager Certificate Manager | Microsoft Docs
description: Installera Microsoft Identity Manager 2016 Certificate Manager
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 25a511dc590b02019c65a688c9b2c8dc821fff50
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290092"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Distribuera Microsoft Identity Manager Certificate Manager 2016 (MIM CM)

Installationen av Microsoft Identity Manager Certificate Manager 2016 (MIM CM) omfattar ett antal steg. Som ett sätt att förenkla processen vi dela saker upp. Det finns förberedande steg som måste vidtas innan eventuella faktiska MIM CM-åtgärder. Utan den inledande kommer installationen troligen att misslyckas.

Diagrammet nedan visar ett exempel på typ av miljö som kan användas. System med siffror ingår i listan nedan i diagram och krävs för att slutföra stegen som beskrivs i den här artikeln. Slutligen används Windows 2016 Datacenter-servrar:

![Diagram för miljö](media/mim-cm-deploy/image001.png)

1. CORPDC – domänkontrollant
2. CORPCM – MIM CM-servern
3. CORPCA – certifikatutfärdare
4. CORPCMR – MIM CM Rest API Web – CM portalen-för Rest-API – används för senare
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – domänanslutna Windows 10

## <a name="deployment-overview"></a>Distributionsöversikt

- Installation av grundläggande operativsystem

    Labbet består av windows 2016 Datacenter.

    >[!NOTE]
    >För mer information om plattformar som stöds för MIM 2016 ta en titt på i artikeln [plattformar som stöds för MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)

1. Fördistributionssteg

    - [Utökning av schemat](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Skapa tjänstkonton

    - [Skapa certifikatmallar](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Konfigurera Kerberos

    - Databasrelaterade steg

        - Krav för SQL-konfiguration

        - Databasbehörighet

2. distribution

## <a name="pre-deployment-steps"></a>Fördistributionssteg

Guiden för konfiguration av MIM CM kräver information ska tillhandahållas på vägen för att den ska slutföras.

![Diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Utökning av schemat

Processen med att utöka schemat är enkla men måste vara närmat sig med försiktighet på grund av dess oåterkallelig karaktär.

>[!NOTE]
>Det här steget kräver att det konto som används har administratörsrättigheter för schemat.

1. Bläddra till platsen för MIM-media och gå till \\certifikathantering\\x64 mapp.

2. Kopiera mappen schemat på CORPDC sedan navigera till den.

    ![Diagram](media/mim-cm-deploy/image005.png)

3. Kör skriptet resourceForestModifySchema.vbs enda skog scenario. Kör skripten för resursen skog scenariot:
   - DomänA – användare finns (userForestModifySchema.vbs)
   - ResourceForestB – platsen för CM-installationen (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Schemaändringar är en enkelriktad åtgärd och kräver en skog återställning för att återställa så se till att du har nödvändiga säkerhetskopior. För information om ändringar som görs i schemat genom att utföra den här åtgärden granska artikeln [schemaändringar för Forefront Identity Manager 2010 certifikat Management](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![Diagram](media/mim-cm-deploy/image007.png)

4. Kör skriptet, och du bör få lyckas visas en gång att skriptet har slutförts.

    ![Meddelande](media/mim-cm-deploy/image009.png)

I AD-schemat utökas nu för att stödja MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Skapa tjänstkonton och grupper

I följande tabell sammanfattas de konton och behörigheter som krävs av MIM CM. Du kan tillåta MIM CM skapa följande konton automatiskt eller så kan du skapa dem före installationen. De faktiska kontonamn kan ändras. Om du skapar konton själv, bör du namnge användarkonton i ett sådant direkt att det är enkelt att matcha namnet på användarkontot till sitt arbete.

Användare:

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Roll**                   | **Användaren loggar in namn** |
|----------------------------|---------------------|
| MIM CM-Agent               | MIMCMAgent          |
| MIM CM nyckelåterställningsagent  | MIMCMKRAgent        |
| MIM CM-Agent för auktorisering | MIMCMAuthAgent      |
| MIM CM CA Manager-agenten    | MIMCMManagerAgent   |
| MIM CM poolen Webbagent      | MIMCMWebAgent       |
| MIM CM registrerings-Agent    | MIMCMEnrollAgent    |
| MIM CM-uppdateringstjänst      | MIMCMService        |
| Kontot för MIM-installation        | MIMINSTALL          |
| Help Desk-Agent            | CMHelpdesk1 2       |
| CM-hanteraren                 | CMManager1 2        |
| Prenumeranten användare            | CMUser1 2           |

Grupper:

| **Roll**               | **Grupp**         |
|------------------------|-------------------|
| CM supportavdelningen medlemmar    | MIMCM supportavdelningen    |
| CM-Manager-medlemmar     | MIMCM-hanterare    |
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

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Uppdatera **CORPCM** Server lokal Policy för Agentkonton 

| **Användaren loggar in namn** | **Beskrivning och behörigheter**   |
|------|---------------------|
| MIMCMAgent          | Tillhandahåller följande tjänster: </br>-Hämtar krypterade privata nycklar från Certifikatutfärdaren. </br>-Skyddar smartkort PIN-kod informationen i FIM CM-databasen. </br>-Skyddar kommunikationen mellan FIM CM och CA: N. </br></br> Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br>-   **Tillåt lokal inloggning** användarrättigheten.</br>-   **Utfärda och hantera certifikat** användarrättigheten. </br>-Läs- och skrivbehörighet för system Temp-mappen på följande plats: % WINDIR %\\Temp.</br>– En digital signatur och kryptering certifikat utfärdas och installeras i användararkivet.
|MIMCMKRAgent        | Återställs arkiverade privata nycklar från Certifikatutfärdaren. Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br> -   **Tillåt lokal inloggning** användarrättigheten.</br>-Medlemskap i lokalt **administratörer** grupp. </br>– Registrera behörighet på den **KeyRecoveryAgent** certifikatmall. </br>-Nyckelåterställningsagenten certifikatet utfärdas och installeras i användararkivet. Certifikatet måste läggas till i listan över återställning av nyckel-agenterna på ca: N. </br>Kontrollera att de tre nya mallarna visas i den ```%WINDIR%\\Temp.```information rutan och Stäng certifikatutfärdare. ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | MIM CM-signering Det här användarkontot kräver följande inställningar för åtkomstkontroll: </br>Stäng alla öppna fönster och logga ut. </br> IIS-konfiguration             |
| MIMCMManagerAgent   | För att kunna vara värd för webbplatsen för CM, installera och konfigurera IIS. </br> Installera och konfigurera IIS        |
| MIMCMWebAgent       | Logga in på CORLog i som MIMINSTALL konto Kontot för installation av MIM ska vara en lokal administratör </br> Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br> Öppna PowerShell och kör följande kommando </br>-Medlemskap i lokalt **administratörer** grupp.</br>IIS-konfiguration </br>En webbplats med namnet Default Web Site installeras som standard i IIS 7. </br>Om platsen har bytt namn eller ta bort en webbplats med namnet måste Default Web Site finnas innan du kan installera MIM CM.</br>Kontot MIMCMWebAgent ska köras på MIM CM-portalen. </br>-   Beviljas läsbehörighet på den **HKEY_LOCAL_MACHINE\\programvara\\Microsoft\\CLM\\v1.0\\Server\\///användare** registernyckeln. </br>Du ska inaktivera Kerberos-autentisering för kernelläge och konfigurerar SPN för kontot MIMCMWebAgent i stället.|
| MIMCMEnrollAgent    | Vissa kommandot kräver förhöjd behörighet i active directory och CORPCM server. Det här användarkontot kräver följande inställningar för åtkomstkontroll:</br>Uppdatera IIS på CORPCM</br>-   **Tillåt lokal inloggning** användarrättigheten. </br>Du behöver lägga till en DNS A Record för ”cm.contoso.com” och peka på CORPCM IP                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Kräver SSL på MIM CM-portalen

Vi rekommenderar starkt att du kräva SSL på MIM CM-portalen.

Om du ännu inte guiden att varna dig för den.

- Registrera dig i certifikat för webbprogram för cm.contoso.com tilldela till standardwebbplatsen

- Öppna IIS-hanteraren och navigera till certifikathantering

- Dubbelklicka på SSL-inställningar i vyn funktioner.

Välj på sidan SSL-inställningar Kräv SSL. I åtgärdsfönstret klickar du på Verkställ.

Databaskonfiguration CORPSQL för MIM CM

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Se till att du är ansluten till CORPSQL01-servern.

1. Se till att du är inloggad som SQL DBA.

2. Kör följande T-SQL-skript för att tillåta CONTOSOS**MIMINSTALL-konto för att skapa databasen när vi går du till konfigurationssteg

3. Vi kommer att behöva gå tillbaka till SQL när vi är klara för modulen Avsluta & princip

4. MIM CM-konfiguration guiden felmeddelande

5. Distribution av Microsoft Identity Manager 2016 certifikathantering

    ![Kontrollera att du är ansluten till CORPCM-servern och att den MIMINSTALL kontot är medlem i den lokala administratörer grupp.](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >Se till att du är inloggad som ContosoMIMINSTALL. Montera ISO för Microsoft Identity Manager SP1. Se [V3 information Öppna](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) den certifikathanteringx64 directory. 

6. I den **x64** fönstret, högerklicka på **installationsprogrammet**, och klicka sedan på **kör som administratör**. Ändra den **giltighetsperioden** till **2 år**, och avmarkera sedan den **Publicera certifikatet i Active Directory** kryssrutan.

7. På den **hantering av begäranden** kontrollerar du att den **Tillåt att den privata nyckeln exporteras** kryssrutan är markerad och klicka sedan på **fliken kryptografi**.

8. I den **val av kryptografiska** dialogrutan inaktivera **Microsoft Enhanced kryptografiprovider v1.0**, aktivera **Microsoft Enhanced RSA och AES Cryptographic Provider**, och klicka sedan på **OK**.

9. På den **ämnesnamn** avmarkerar den **Inkludera e-postnamnet i ämnesnamnet** och **e-postnamn** kryssrutorna.

10. På den **tillägg** fliken den **tillägg som ingår i den här mallen** lista, se till att **användningsprinciper** är markerad och klicka sedan på **redigera** .

11. I den **Redigera tillägg för användningsprinciper** dialogrutan, Välj den **Krypterande filsystem** och **säker e-post** principer för program. Klicka på **ta bort**, och klicka sedan på **OK**.

12. På den **säkerhet** fliken utför följande steg:

    - Ta bort **administratör**.

    - Ta bort **Domänadministratörer**.

    - Ta bort **domänanvändare**.

    - Tilldela bara **Läs** och **skriva** behörighet att **Företagsadministratörer**.

    - Lägg till **MIMCMAgent.**

    - Tilldela **Läs** och **registrera** behörighet att **MIMCMAgent**.

13. I den **egenskaper för ny mall** dialogrutan klickar du på **OK**.

14. Lämna den **konsol för certifikatmallar** öppna.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Skapa MIM CM registreringsagent certifikatmall

1. I den **konsol för certifikatmallar**i den **information** rutan, markera och högerklicka på **registreringsagent**, och klicka sedan på **duplicera mall**.

2. Distribution av Microsoft Identity Manager 2016 certifikathantering

3. I den **egenskaper för ny mall** dialogrutan den **allmänna** fliken den **Mallens visningsnamn** skriver **MIM CM-registreringsagent**. Se till att den **giltighetsperioden** är **2 år**.

4. På den **hantering av begäranden** fliken, aktivera **Tillåt att den privata nyckeln exporteras**, och klicka sedan på **kryptografiproviders eller fliken kryptografi.**

5. I den **val av Kryptografiprovider** dialogrutan inaktivera **Microsoft grundläggande kryptografiprovider v1.0**, inaktivera **Microsoft Enhanced kryptografiprovider v1.0**, aktivera  **Microsoft Enhanced RSA och AES Cryptographic Provider**, och klicka sedan på **OK**.

6. På den **säkerhet** fliken utför följande:

    - Ta bort **administratör**.

    - Ta bort **Domänadministratörer**.

    - Tilldela bara **Läs** och **skriva** behörighet att **Företagsadministratörer**.

    - Lägg till **MIMCMEnrollAgent**.

    - Tilldela **Läs** och **registrera** behörighet att **MIMCMEnrollAgent**.

7. I den **egenskaper för ny mall** dialogrutan klickar du på **OK**.

8. Lämna den **konsol för certifikatmallar** öppna.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Skapa MIM CM nyckelåterställningsagenten certifikatmall

1. I den **certifikatmallar** konsolen i den **information** rutan, markera och högerklicka på **nyckelåterställningsagenten**, och klicka sedan på **duplicera mall**.

2. Distribution av Microsoft Identity Manager 2016 certifikathantering

3. I den **egenskaper för ny mall** dialogrutan den **allmänna** fliken den **Mallens visningsnamn** skriver **MIM CM nyckelåterställningsagenten**. Se till att den **giltighetsperioden** är **2 år** på den **fliken kryptografi.**

4. I den **Providers markeringen** dialogrutan inaktivera **Microsoft Enhanced kryptografiprovider v1.0**, aktivera **Microsoft Enhanced RSA och AES Cryptographic Provider**, och klicka sedan på **OK**.

5. På den **krav för utfärdande** kontrollerar du att **Certifikatutfärdarens certifikathanterare godkänner registreringen** är **inaktiveras**.

6. På den **säkerhet** fliken utför följande:

    - Ta bort **administratör**.

    - Ta bort **Domänadministratörer**.

    - Tilldela bara **Läs** och **skriva** behörighet att **Företagsadministratörer**.

    - Lägg till **MIMCMKRAgent**.

    - Tilldela **Läs** och **registrera** behörighet att **KRAgent**.

7. I den **egenskaper för ny mall** dialogrutan klickar du på **OK**.

8. Stäng **konsolen för certfikatmallar**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicera de nödvändiga certifikatmallarna på certifikatutfärdaren

1. Återställ den **certifikatutfärdare** konsolen.

2. I den **certifikatutfärdare** konsolen i konsolträdet, högerklicka på **certifikatmallar**, peka på **ny**, och klicka sedan på **certifikat Mallen ska utfärdas**.

3. I den **Aktivera certifikatmallar** dialogrutan **MIM CM registreringsagent**, **MIM CM nyckelåterställningsagenten**, och **MIM CM signering**. Klicka på **OK**.

4. I konsolträdet klickar du på **certifikatmallar**.

5. Välkommen till installationsguiden för Microsoft Identity Manager Certificate Management, klicka på **nästa.

    ![På sidan Licensavtal läsa avtalet, aktivera på jag accepterar villkoren i licensavtalet kryssrutan, och klicka sedan på Nästa.](media/mim-cm-deploy/image016.png)

6. Kontrollera på sidan anpassad installation av MIM CM-portalen och MIM CM-uppdateringstjänst komponenter är inställda på att installeras, och sedan Klicka på nästa.

### <a name="iis-configuration"></a>Kontrollera att namnet på virtuella mappen är på sidan virtuella webbmapp CertificateManagement, och sedan Klicka på nästa.

På sidan Installera Microsoft Identity Manager Certificate Management klickar du på Installera.

#### <a name="install-and-configure-iis"></a>På den slutförd installationsguiden för Microsoft Identity Manager Certificate Management-sidan Klicka på Slutför.

1. MIM CM guiden har slutförts

    >[!IMPORTANT]
    >Guiden för konfiguration av Microsoft Identity Manager 2016 certifikathantering

2. Innan du loggar in i CORPCM Lägg till MIMINSTALL till domain Admins, Schema-administratörer och lokala administratörer för konfigurationsguiden.

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Detta kan tas bort senare när konfigurationen är klar. Från den starta -menyn klickar du på certifikat Management konfigurationsguiden.

#### <a name="configuring-kerberos"></a>Konfigurera Kerberos

Och köra som administratör På den Välkommen till guiden för konfiguration av klickar du på nästa. På den certifikatutfärdarkonfiguration , se till att den markerade Certifikatutfärdaren är Contoso-CORPCA-CA, se till att den valda servern är CORPCA. CONTOSO.COM, och klicka sedan på nästa. På den ställa in Microsoft® SQL Server®-databas sidan den SQL Server skriver CORPSQL1 , aktivera den använda mina autentiseringsuppgifter för att skapa den databasen kryssrutan och klicka sedan på nästa.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**På den **databasinställningar** godkänner standardnamnet på databasen för FIMCertificateManagement, se till att SQL-integrerad autentisering väljs, och sedan Klicka på nästa.**

![Diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>På den konfigurera Active Directory , acceptera standardnamnet för tjänstanslutningspunkten och klicka sedan på nästa.

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>På den autentiseringsmetod Bekräfta windows-integrerad autentisering är markerad och klicka sedan på nästa.

På den agenter – FIM CM avmarkerar den använder standardinställningarna för FIM CM kryssrutan och klicka sedan på anpassad konton. I den agenter – FIM CM flera flikar i dialogrutan på varje flik, anger du följande information:

1. Användarnamn: **uppdatering

2. Lösenord: **skicka**word1**

3. Bekräfta lösenord: skickaword1

4. Använd en befintlig användare: **aktiverad

5. Dessa konton som vi skapade tidigare.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Kontrollera att procedurerna i steg 8 upprepas för alla sex agent konto flikar.

1. MIM CM-konton

2. När alla kontoinformation för agent är klar klickar du på OK.

3. På den \\agenter – MIM CM klickar du på nästa.

    >[!NOTE]
    >På den konfigurera servercertifikat sidan måste du aktivera följande certifikatmallar:

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Certifikatmall som ska användas för certifikat för dataåterställning agent nyckelåterställningsagenten: MIMCMKeyRecoveryAgent.](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Certifikatmall som ska användas för FIM CM-Agent certifikatet: MIMCMSigning.

1. Certifikatmall som ska användas för det registreringsagent-certifikatet: **FIMCMEnrollmentAgent**.

2. På den \\ställa in servercertifikat klickar du på nästa.

3. På den installationsprogrammet e-postservern, skriva ut dokumentet sidan den ange namnet på SMTP-servern som du vill använda i e-registrering meddelanden rutan och klicka på nästa.

4. **På den **redo att konfigurera** klickar du på **konfigurera\\.

5. I den **konfigurationsguiden – Microsoft Forefront Identity Manager 2010 R2** varning i dialogrutan klickar du på **OK** bekräftar att SSL inte är aktiverat på den virtuella IIS-katalogen.

6. Media/image17.png**

7. Klicka inte på knappen Slutför tills körningen av guiden är klar.

8. Loggning för guiden hittar du här: **% programfiles %** Microsoft Forefront Identity Management**2010**certifikathantering**config.log

9. Stäng alla öppna fönster.

10. Lägg till ** till zonen Lokalt intranät i webbläsaren.

11. Webbplats från servern CORPCM

![Kontrollera CNG Key Isolation-tjänst](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Från Administrationsverktygöppnar Services.

I den **information** rutan dubbelklickar du på CNG Key isolering. På den allmänna fliken, ändrar den starttyp till automatisk.

![Felmeddelande](media/mim-cm-deploy/image028.png)

1. På den **allmänna** fliken genom att starta tjänsten om den inte är i ett tillstånd som är igång. På den **allmänna** klickar du på OK.**

2. Installera och konfigurera modulerna som CA:

3. I det här steget ska vi installera och konfigurera Certifikatutfärdaren för FIM CM-moduler på certifikatutfärdaren.

4. Konfigurera FIM CM om du vill bara kontrollera användarbehörigheterna för hanteringsåtgärder

5. I den **C:** programfiler**Microsoft Forefront Identity Manager**2010**certifikathantering**web**och gör en kopia av** Web.config naming kopian web.1.config.

6. I den **Web** fönstret, högerklicka på **Web.config**, och klicka sedan på öppna.

7. Web.config-filen öppnas i anteckningar

8. Tryck på CTRL + F när filen öppnas.

9. I den **Sök och Ersätt** i dialogrutan den Sök efter skriver UseUser, och klicka sedan på Sök nästa tre gånger.

   - Stäng den **Sök och Ersätt** dialogrutan.**

   - Du bör vara på raden  **\@lägga till key="Clm.RequestSecurity.Flags” value = ”UseUser UseGroups” /**.**

   - Ändra raden till  **\@lägga till key="Clm.RequestSecurity.Flags” value = ”UseUser” /**.**

   - Stäng filen, spara alla ändringar.**

     >[!NOTE]
     >Skapa ett konto för CA-datorn med SQLServer skript Kontrollera att du är ansluten till den CORPSQL01 server.

     ![Se till att du är inloggad som DBA](media/mim-cm-deploy/image030.png)

10. Från den **starta** -menyn, starta SQL Server Management Studio.

11. I den **Anslut till Server** i dialogrutan den **servernamn** skriver CORPSQL01, och klicka sedan på Anslut.

12. I konsolträdet expanderar **säkerhet**, och klicka sedan på inloggningar.

    - Högerklicka på **inloggningar**, och klicka sedan på ny inloggning.

    - På den **allmänna** sidan den inloggningsnamnet skriver contosoCORPCA.

    - Välj **Windows-autentisering**.

13. Standarddatabasen är **FIMCertificateManagement**.

14. I den vänstra rutan, Välj **användarmappning**.**

15. I den högra rutan, klickar du på kryssrutan i den **kartan** kolumnen bredvid **FIMCertificateManagement**.

16. I den **rollmedlemskap för databasen: FIMCertificateManagement** och aktivera den **clmApp** roll.

    ![Stäng Microsoft SQL Server Management Studio.](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Installera FIM CM CA-moduler på certifikatutfärdaren Loggning för guiden hittar du här: **% programfiles%\\Microsoft Forefront Identity Management\\2010\\certifikathantering\\config.log**

17. Klicka på **Slutför**.

    ![Kontrollera CNG Key Isolation-tjänst](media/mim-cm-deploy/image033.png)

18. I den X64 windows, högerklicka på Setup.exe, och klicka sedan på kör som administratör.

19. På den https://cm.contoso.com/certificatemanagementVälkommen till installationsguiden för Microsoft Identity Manager Certificate Management klickar du på nästa.

20. På den https://cm.contoso.com/certificatemanagementlicensavtalet Läs avtalet.  

    ![Diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Välj den jag accepterar villkoren i licensavtalet kryssrutan och klicka sedan på nästa.

1. På den **anpassad installation** väljer **MIM CM-portalen**, och klicka sedan på funktionen blir inte tillgängliga.

2. På den **anpassad installation** väljer **MIM CM-uppdateringstjänst**, och klicka sedan på den här funktionen kommer inte att vara tillgänglig.

3. Detta lämnar MIM CM CA-filer som funktionen endast aktiverad för installationen.

4. På den **anpassad installation** klickar du på nästa.

5. På den **installera Microsoft Identity Manager Certificate Management** klickar du på **installera**.

### <a name="installing-and-configuring-the-ca-modules-"></a>På den slutföra Installationsguiden för Microsoft Identity Manager Certificate Management klickar du på Slutför.

Konfigurera MIM CM avsluta modul

1. I konsolträdet högerklickar du på contoso-CORPCA-CA, och klicka sedan på egenskaper.

2. På den **avsluta modul\\ väljer \\FIM CM avsluta modul\\, och klicka sedan på \\egenskaper\\.

3. I den **ange anslutningssträngen för databasen för CM** skriver **Connect Timeout = 15. Spara säkerhetsinformation = True; Integrerad säkerhet = sspi; Initial Catalog = FIMCertificateManagement; Data Source = CORPSQL01**.

    >[!Note]
    >Lämna den kryptera anslutningssträngen kryssrutan aktiverad och klicka sedan på OK.

4. I den Microsoft FIM certifikathantering meddelanderutan klickar du på OK.

5. I den **contoso-CORPCA-certifikatutfärdaregenskaper** dialogrutan klickar du på **OK**.

6. Högerklicka på **contoso-CORPCA-CA** , peka alla aktiviteter, och klicka sedan på stoppa tjänsten.

7. Du bör vara på raden **\<lägga till key="Clm.RequestSecurity.Flags” value = ”UseUser UseGroups” /\>**. Högerklicka på **contoso-CORPCA-CA\< \>,** peka alla aktiviteter, och klicka sedan på starta tjänsten.

8. Minimera den certifikatutfärdare konsolen.

9. Från \<Administrationsverktyg\>öppnar Loggboken.\>

10. I konsolträdet expanderar **program- och tjänstloggar**, och klicka sedan på FIM certifikathantering.

11. Kontrollera att göra de senaste händelserna i listan över händelser, **inte** innehålla varning eller fel händelser sedan den senaste omstarten av Certifikattjänster.**

12. Den sista händelsen bör ange att avsluta modulen lästes in med hjälp av inställningarna från:

13. Minimera den **Loggboken**.

14. Kopiera MIMCMAgent certifikatets tumavtryck till Windows® Urklipp

15. I konsolträdet expanderar **contoso-CORPCA-CA**, och klicka sedan på **utfärdade certifikat**.

16. I den **information** rutan dubbelklickar du på certifikatet med **CONTOSO**MIMCMAgent**i den \\beställarens namn\$ kolumn och med**FIM CM Signering i den certifikatmall kolumn. På den **information** väljer den tumavtrycket fältet. Välj tumavtrycket och tryck på CTRL + C.

17. Gör **inte** innehåller inledande blanksteg i listan över tumavtryck tecken. I den **certifikat** dialogrutan klickar du på **OK**. Från den **starta** menyn i den **Sök program och filer** skriver anteckningar, och tryck sedan på RETUR.

18. Klicka på **OK**.

19. I **anteckningar**, från den redigera -menyn klickar du på klistra in.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Från den redigera -menyn klickar du på ersätta.

1. I den **Sök efter** rutan, skriver du ett blanksteg och klicka sedan på Ersätt alla.

2. Detta tar bort alla blanksteg mellan tecken i certifikatets tumavtryck.

3. I den **ersätta** dialogrutan klickar du på **Avbryt**.

4. Välj den konverterade **thumbprintstring**, och tryck på CTRL + C. Stäng **anteckningar** utan att spara ändringarna.

5. Konfigurera principmodulen för FIM CM

6. Högerklicka på **contoso-CORPCA-CA**, och klicka sedan på **egenskaper**.

    >[!Note]
    >I den contoso-CORPCA-certifikatutfärdaregenskaper dialogrutan den principmodulen klickar du på egenskaper.

7. På den **allmänna** kontrollerar du att **skicka FIM CM-begäran till standardprincipmodulen för bearbetning** är markerad.

8. På den **signeringscertifikat** klickar du på **Lägg till**.

9. I dialogrutan certifikat högerklickar du på den **ange hex-kodat certifikat-hash** rutan och klicka på **klistra in**.

10. I den X64 windows, högerklicka på Setup.exe, och klicka sedan på kör som administratör.

### <a name="configure-the-mim-cm-exit-module"></a>Om den OK knappen inte är aktiverad kan du av misstag ingår ett dolt tecken i strängen tumavtrycket när du kopierade tumavtrycket från clmAgent certifikatet.

1. Se till att du är inloggad som SQL DBA.

2. Upprepa alla steg från **uppgift 4: kopiera MIMCMAgent tumavtrycket till Urklipp** i den här övningen.

3. I den **konfigurationsegenskaper** dialogrutan ser du till att tumavtrycket visas i den **giltigt signeringscertifikat** listan och klicka sedan på **OK**.

4. I den **FIM certifikathantering** meddelanderutan klickar du på **OK**. Stäng den **certifikatutfärdare** konsolen.
5. Stäng alla öppna fönster och sedan loggar ut.

6. Sista steget i distributionen** är vi vill försäkra att CONTOSO**MIMCM chefer kan distribuera och skapa mallar och konfigurera systemet utan att schemat och Domänadministratörer.

7. Nästa skriptet kommer ACL behörigheter till certifikatmallar med dsacls. Kör med ett konto som har fullständig behörighet för att ändra säkerhet Läs- och skrivbehörighet till varje certifikatmall i skogen.

8. Första stegen: **hur du konfigurerar tjänstanslutningspunkten och målgruppen behörigheter & delegera mallen profilhantering

9. Konfigurera behörigheter för tjänstanslutningspunkten (SCP).

10. Konfigurera delegerade profilhantering för mallen.

11. Inget skript

12. Kontrollera att du är ansluten till den *CORPDC* virtuell server.

    >[!NOTE] 
    >Logga in som `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`contosocorpadmin `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Från **Administrationsverktyg**öppnar Active Directory-användare och datorer.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Kopiera MIMCMAgent certifikatets tumavtryck till Windows® Urklipp

1. Återställ den **certifikatutfärdare** konsolen.

2. I konsolträdet expanderar **contoso-CORPCA-CA**, och klicka sedan på **utfärdade certifikat**.

3. I den **information** rutan dubbelklickar du på certifikatet med **CONTOSO\\MIMCMAgent** i den **beställarens namn** kolumn och med **FIM CM Signering** i den **certifikatmall** kolumn.

4. På den **information** väljer den **tumavtrycket** fältet.

5. Välj tumavtrycket och tryck på CTRL + C.

    >[!NOTE]
    >Gör **inte** innehåller inledande blanksteg i listan över tumavtryck tecken.

6. I den **certifikat** dialogrutan klickar du på **OK**.

7. Från den **starta** menyn i den **Sök program och filer** skriver **anteckningar**, och tryck sedan på RETUR.

8. I **anteckningar**, från den **redigera** -menyn klickar du på **klistra in**.

9. Från den **redigera** -menyn klickar du på **ersätta**.

10. I den **Sök efter** rutan, skriver du ett blanksteg och klicka sedan på **Ersätt alla**.

    >[!Note]
    >Detta tar bort alla blanksteg mellan tecken i certifikatets tumavtryck.

11. I den **ersätta** dialogrutan klickar du på **Avbryt**.

12. Välj den konverterade *thumbprintstring*, och tryck på CTRL + C.

13. Stäng **anteckningar** utan att spara ändringarna.

### <a name="configure-the-fim-cm-policy-module"></a>Konfigurera principmodulen för FIM CM

1. Återställ den **certifikatutfärdare** konsolen.

2. Högerklicka på **contoso-CORPCA-CA**, och klicka sedan på **egenskaper**.

3. I den **contoso-CORPCA-certifikatutfärdaregenskaper** dialogrutan den **principmodulen** klickar du på **egenskaper**.

    - På den **allmänna** kontrollerar du att **skicka FIM CM-begäran till standardprincipmodulen för bearbetning** är markerad.

    - På den **signeringscertifikat** klickar du på **Lägg till**.

    - I dialogrutan certifikat högerklickar du på den **ange hex-kodat certifikat-hash** rutan och klicka på **klistra in**.

    - I den **certifikat** dialogrutan klickar du på **OK**.

        >[!Note]
        >Om den **OK** knappen inte är aktiverad kan du av misstag ingår ett dolt tecken i strängen tumavtrycket när du kopierade tumavtrycket från clmAgent certifikatet. Upprepa alla steg från **uppgift 4: kopiera MIMCMAgent tumavtrycket till Urklipp** i den här övningen.

4. I den **konfigurationsegenskaper** dialogrutan ser du till att tumavtrycket visas i den **giltigt signeringscertifikat** listan och klicka sedan på **OK**.

5. I den **FIM certifikathantering** meddelanderutan klickar du på **OK**.

6. Sista steget i distributionen** är vi vill försäkra att CONTOSO**MIMCM chefer kan distribuera och skapa mallar och konfigurera systemet utan att schemat och Domänadministratörer.

7. Nästa skriptet kommer ACL behörigheter till certifikatmallar med dsacls.

8. Kör med ett konto som har fullständig behörighet för att ändra säkerhet Läs- och skrivbehörighet till varje certifikatmall i skogen.

9. Första stegen: **hur du konfigurerar tjänstanslutningspunkten och målgruppen behörigheter & delegera mallen profilhantering

10. Stäng den **certifikatutfärdare** konsolen.

11. Stäng alla öppna fönster och sedan loggar ut.

**Sista steget i distributionen** är vi vill försäkra att CONTOSO\\MIMCM chefer kan distribuera och skapa mallar och konfigurera systemet utan att schemat och Domänadministratörer. Nästa skriptet kommer ACL behörigheter till certifikatmallar med dsacls. I den Behörighetspost för CONTOSO i dialogrutan den gäller väljer underordnad användarobjekt och sedan aktivera den Tillåtkryssrutan för följande behörigheter:

I den **Behörighetspost för CONTOSO** dialogrutan klickar du på OK.**

1. I den avancerade säkerhetsinställningar för CONTOSO dialogrutan klickar du på Lägg till.

2. I den Behörighetspost för CONTOSO i dialogrutan den gäller väljer underordnad användarobjekt och välj sedan den Tillåtkryssrutan för följande behörigheter:

3. I den avancerade säkerhetsinställningar för CONTOSO dialogrutan klickar du på Lägg till. **\<I den **Behörighetspost för contoso\< dialogrutan klickar du på \>OK**.\>**

4.   Andra steg: **delegera hantering Certifikatmallsbehörighet** skript

5. Logga in som **contoso\\corpadmin**

6. Delegera behörigheter för behållaren OID.

7. Delegera behörigheter för befintliga certifikatmallarna.

8. I konsolträdet expanderar **Contoso.com** \| **System** \| **Microsoft** \| **certifikatets livscykel Manager**, och klicka sedan på **CORPCM**.

9. Återställ den **Active Directory-platser och tjänster** konsolen.

10. I konsolträdet expanderar **Services**, expandera **Public Key Services**, och klicka sedan på certifikatmallar.

    | Grupp          | Behörigheter      |
    |----------------|------------------|
    | I konsolträdet högerklickar du på certifikatmallar, och klicka sedan på delegera kontroll. | Läs </br> I den delegera kontroll klickar du på nästa.</br> På den användare eller grupper klickar du på Lägg till.</br> I den Välj användare, datorer eller grupper i dialogrutan den ange objektnamn att välja skriver mimcm chefer, och klicka sedan på OK.</br> På den användare eller grupper klickar du på nästa.</br> På den aktiviteter som ska delegeras klickar du på skapa en anpassad aktivitet och delegera, och klicka sedan på nästa.</br> På den Active Directory-objekttyp ser du till att den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen är markerad och klicka sedan på nästa. </br> FIM CM-begäran Avblockera smartkort |
    | mimcm supportavdelningen | Läs</br> På den användare eller grupper klickar du på Lägg till.</br> På den Active Directory-objekttyp ser du till att den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen är markerad och klicka sedan på nästa.</br> FIM CM-begäran Avblockera smartkort |

11. I den **CORPDC egenskaper** dialogrutan klickar du på **OK**.

12. Lämna **Active Directory-användare och datorer** öppna.

**Konfigurera behörigheter för de underordnade objekt**

1. Kontrollera att du är fortfarande i den **Active Directory-användare och datorer** konsolen.

2. I konsolträdet högerklickar du på **Contoso.com**, och klicka sedan på **egenskaper**.

3. På den **säkerhet** klickar du på **Avancerat**.

4. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **Lägg till**.

5. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm chefer**, och klicka sedan på **OK**.

6. I den **Behörighetspost för Contoso** i dialogrutan den **gäller** väljer **underordnad användarobjekt** och sedan aktivera den **Tillåt**kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **I den **delegera kontroll** klickar du på nästa.**

    - **På den **användare eller grupper** klickar du på Lägg till.**

    - **I den **Välj användare, datorer eller grupper** i dialogrutan den ange objektnamn att välja skriver mimcm chefer, och klicka sedan på OK.**

    - **På den **användare eller grupper** klickar du på nästa.**

    - **På den **aktiviteter som ska delegeras** klickar du på skapa en anpassad aktivitet och delegera, och klicka sedan på nästa.**

    - **På den **Active Directory-objekttyp** ser du till att den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen är markerad och klicka sedan på nästa.**

    - **FIM CM-begäran Avblockera smartkort**

7. I den **Behörighetspost för Contoso** dialogrutan klickar du på **OK**.

8. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **Lägg till**.

9. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm supportavdelningen**, och klicka sedan på **OK**.

10. I den **Behörighetspost för Contoso** i dialogrutan den **gäller** väljer **underordnad användarobjekt** och välj sedan den **Tillåt**kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **På den **användare eller grupper** klickar du på Lägg till.**

    - **På den **Active Directory-objekttyp** ser du till att den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen är markerad och klicka sedan på nästa.**

    - **FIM CM-begäran Avblockera smartkort**

11. I den **Behörighetspost för Contoso** dialogrutan klickar du på **OK**.

12. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **OK**.

13. I den **contoso.com egenskaper** dialogrutan klickar du på **OK**.

14. Lämna **Active Directory-användare och datorer** öppna.

**Konfigurera behörigheter för de underordnade användarobjekt \<skript\>**

1. Kontrollera att du är fortfarande i den **Active Directory-användare och datorer** konsolen.

2. I konsolträdet högerklickar du på **Contoso.com**, och klicka sedan på **egenskaper**.

3. På den **säkerhet** klickar du på **Avancerat**.

4. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **Lägg till**.

5. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm chefer**, och klicka sedan på **OK**.

6. I den **Behörighetspost för CONTOSO** i dialogrutan den **gäller** väljer **underordnad användarobjekt** och sedan aktivera den **Tillåt**kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **I den **delegera kontroll** klickar du på nästa.**

    - **På den **användare eller grupper** klickar du på Lägg till.**

    - **I den **Välj användare, datorer eller grupper** i dialogrutan den ange objektnamn att välja skriver mimcm chefer, och klicka sedan på OK.**

    - **På den **användare eller grupper** klickar du på nästa.**

    - **På den **aktiviteter som ska delegeras** klickar du på skapa en anpassad aktivitet och delegera, och klicka sedan på nästa.**

    - **På den **Active Directory-objekttyp** ser du till att den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen är markerad och klicka sedan på nästa.**

    - **FIM CM-begäran Avblockera smartkort**

7. I den **Behörighetspost för CONTOSO** dialogrutan klickar du på **OK**.

8. I den **avancerade säkerhetsinställningar för CONTOSO** dialogrutan klickar du på **Lägg till**.

9. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm supportavdelningen**, och klicka sedan på **OK**.

10. I den **Behörighetspost för CONTOSO** i dialogrutan den **gäller** väljer **underordnad användarobjekt** och välj sedan den **Tillåt**kryssrutan för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läsbehörighet**

    - **På den **användare eller grupper** klickar du på Lägg till.**

    - **På den **Active Directory-objekttyp** ser du till att den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen är markerad och klicka sedan på nästa.**

    - **FIM CM-begäran Avblockera smartkort**

11. I den **Behörighetspost för contoso** dialogrutan klickar du på **OK**.

12. I den **avancerade säkerhetsinställningar för Contoso** dialogrutan klickar du på **OK**.

13. I den **contoso.com egenskaper** dialogrutan klickar du på **OK**.

14. Lämna **Active Directory-användare och datorer** öppna.

Andra steg: **delegera hantering Certifikatmallsbehörighet \<skript\>**

- Delegera behörighet till behållaren för certifikatmallar.

- Delegera behörigheter för behållaren OID.

- Delegera behörigheter för befintliga certifikatmallarna.

Definiera behörigheter i behållare för certifikatmallar:

1. Återställ den **Active Directory-platser och tjänster** konsolen.

2. I konsolträdet expanderar **Services**, expandera **Public Key Services**, och klicka sedan på **certifikatmallar**.

3. I konsolträdet högerklickar du på **certifikatmallar**, och klicka sedan på **delegera kontroll**.

4. I den **delegera kontroll** klickar du på **nästa**.

5. På den **användare eller grupper** klickar du på **Lägg till**.

6. I den **Välj användare, datorer eller grupper** i dialogrutan den **ange objektnamn att välja** skriver **mimcm chefer**, och klicka sedan på **OK**.

7. På den **användare eller grupper** klickar du på **nästa**.

8. På den **aktiviteter som ska delegeras** klickar du på **skapa en anpassad aktivitet och delegera**, och klicka sedan på **nästa**.

9.  På den **Active Directory-objekttyp** ser du till att **den här mappen, befintliga objekt i den här mappen och skapa nya objekt i den här mappen** är markerad och klicka sedan på **nästa**.

10. På den **behörigheter** sidan den **behörigheter** väljer den **fullständig kontroll** kryssrutan och klicka sedan på **nästa**.

11. På den **Slutför guiden Delegera kontroll** klickar du på **Slutför**.

Definiera behörigheter för behållaren OID:

1. I konsolträdet högerklickar du på **OID**, och klicka sedan på **egenskaper**.

2. I den **OID egenskaper** dialogrutan den **säkerhet** klickar du på **Avancerat**.

3. I den **avancerade säkerhetsinställningar för OID** dialogrutan klickar du på **Lägg till**.

4. I den **Välj användare, dator, tjänstkonto eller grupp** i dialogrutan den **ange objektnamn att välja** skriver **mimcm chefer**, och klicka sedan på **OK**.

5. I den **Behörighetspost för OID** dialogrutan ser du till att behörigheterna som gäller för **objektet och alla underordnade objekt**, klickar du på **fullständig kontroll**, och klicka sedan på  **OK**.

6. I den **avancerade säkerhetsinställningar för OID** dialogrutan klickar du på **OK**.

7. I den **OID egenskaper** dialogrutan klickar du på **OK**.

8. Stäng **Active Directory-platser och tjänster**.

**Skript: Behörighet till behållaren OID, profilmallen och certifikatmallar**

![Diagram](media/mim-cm-deploy/image021.png)

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

![Diagram](media/mim-cm-deploy/image039.png)

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
