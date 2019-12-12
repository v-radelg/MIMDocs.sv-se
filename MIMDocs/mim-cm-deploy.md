---
title: Distribuera Microsoft Identity Manager Certificate Manager | Microsoft Docs
description: Installera Microsoft Identity Manager 2016 certifikat hanterare
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9a9e00f7dca118627a5140967a104d13273cbc26
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/05/2019
ms.locfileid: "67690805"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Distribuera Microsoft Identity Manager Certificate Manager 2016 (MIM CM)

Installationen av Microsoft Identity Manager Certificate Manager 2016 (MIM CM) omfattar ett antal steg. Ett sätt att förenkla processen är att dela upp saker. Det finns preliminära steg som måste vidtas före eventuella faktiska MIM CM steg. Utan det förberedande arbetet kommer installationen sannolikt att Miss Miss förväntat.

Diagrammet nedan visar ett exempel på vilken miljö typ som kan användas. System med siffror ingår i listan nedanför diagrammet och krävs för att slutföra de steg som beskrivs i den här artikeln. Slutligen används Windows 2016 Data Center-servrar:

![Miljö diagram](media/mim-cm-deploy/image001.png)

1. CORPDC – domänkontrollant
2. CORPCM – MIM CM Server
3. CORPCA – certifikat utfärdare
4. CORPCMR – MIM CM REST API Web – CM-portalen för REST API – används för senare
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10-domänanslutna

## <a name="deployment-overview"></a>Distributions översikt

- Grundläggande operativ system installation

    Labbet består av Windows 2016 Data Center-servrar.

    >[!NOTE]
    >Mer information om vilka plattformar som stöds för MIM 2016 ta en titt på artikeln med de [plattformar som stöds för mim 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. För distributions steg

    - [Utöka schemat](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Skapa tjänst konton

    - [Skapa certifikatmallar](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Konfigurera Kerberos

    - Databas-relaterade steg

        - Krav för SQL-konfiguration

        - Databas behörigheter

2. Distribution

## <a name="pre-deployment-steps"></a>För distributions steg

Konfigurations guiden för MIM CM kräver information som ska tillhandahållas på vägen för att den ska kunna slutföras.

![Venndiagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Utöka schemat

Processen för att utöka schemat är enkel men måste följas av försiktighet på grund av dess irreversibla natur.

>[!NOTE]
>Det här steget kräver att kontot som används har schema administratörs behörighet.

1. Bläddra till platsen för MIM-mediet och navigera till \\certifikat hantering\\x64-mappen.

2. Kopiera mappen schema till CORPDC och navigera sedan till den.

    ![Venndiagram](media/mim-cm-deploy/image005.png)

3. Kör scenariot för en enda skog med skriptet resourceForestModifySchema. vbs. Kör skripten för resurs skogens scenario:
   - DomänA – användare Funna (userForestModifySchema. vbs)
   - ResourceForestB – plats för CM-installation (resourceForestModifySchema. vbs).

     >[!NOTE]
     >Schema ändringar är en enkelriktad åtgärd och kräver en skogs återställning för att återställa, så se till att du har nödvändiga säkerhets kopior. Mer information om de ändringar som gjorts i schemat genom att utföra den här åtgärden finns i artikeln [Forefront Identity Manager 2010 certifikat hantering schema ändringar](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![Venndiagram](media/mim-cm-deploy/image007.png)

4. Kör skriptet och du får ett meddelande när skriptet har slutförts.

    ![Meddelande om att något har slutförts](media/mim-cm-deploy/image009.png)

Schemat i AD har nu utökats till stöd för MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Skapa tjänst konton och grupper

I följande tabell sammanfattas de konton och behörigheter som krävs för MIM CM. Du kan tillåta att MIM CM skapar följande konton automatiskt eller så kan du skapa dem före installationen. De faktiska konto namnen kan ändras. Om du skapar konton själv, bör du överväga att namnge användar kontona i så fall att det är enkelt att matcha användar kontots namn med dess funktion.

Användare

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Roll**                   | **Användar inloggnings namn** |
|----------------------------|---------------------|
| MIM CM agent               | MIMCMAgent          |
| MIM CM nyckel återställnings agent  | MIMCMKRAgent        |
| MIM CM Authorization agent | MIMCMAuthAgent      |
| Agent för MIM CM CA Manager    | MIMCMManagerAgent   |
| MIM CM Web pool-agent      | MIMCMWebAgent       |
| MIM CM registrerings agent    | MIMCMEnrollAgent    |
| MIM CM uppdaterings tjänst      | MIMCMService        |
| MIM-installations konto        | MIMINSTALL          |
| Support agent            | CMHelpdesk1-2       |
| CM-chef                 | CMManager1-2        |
| Prenumerantens användare            | CMUser1-2           |

Användargrupp

| **Roll**               | **Grupp**         |
|------------------------|-------------------|
| CM-helpdesk-medlemmar    | mimcm – supportavdelningen    |
| CM-chef medlemmar     | MIMCM – chefer    |
| CM-prenumerant medlemmar | MIMCM-prenumeranter |

PowerShell Agent konton:

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

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Uppdatera **CORPCM** -serverns lokala princip för agent konton 

| **Användar inloggnings namn** | **Beskrivning och behörigheter**   |
|------|---------------------|
| MIMCMAgent          | Tillhandahåller följande tjänster: </br>– Hämtar krypterade privata nycklar från certifikat utfärdaren. </br>– Skyddar PIN-information för smartkort i FIM CM-databasen. </br>– Skyddar kommunikationen mellan FIM CM och CA: n. </br></br> Det här användar kontot kräver följande inställningar för åtkomst kontroll:</br>-   **Tillåt inloggning lokalt** användar rättigheter.</br>-   **utfärda och hantera certifikat** användar rättighet. </br>-Läs-och Skriv behörighet för systemets Temp-mapp på följande plats:% WINDIR%\\temp.</br>– En digital signatur och ett krypterings certifikat utfärdat och installeras i användar arkivet.
|MIMCMKRAgent        | Återställer arkiverade privata nycklar från certifikat utfärdaren. Det här användar kontot kräver följande inställningar för åtkomst kontroll:</br> -   **Tillåt inloggning lokalt** användar rättigheter.</br>– Medlemskap i den lokala **Administratörs** gruppen. </br>-Registrera behörighet för certifikat mal len **KeyRecoveryAgent** . </br>– Certifikat för nyckel återställnings agent utfärdas och installeras i användar arkivet. Certifikatet måste läggas till i listan över nyckel återställnings agenter på CA: n. </br>-Läs behörighets-och Skriv behörighet för mappen system Temp på följande plats: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Fastställer användar rättigheter och behörigheter för användare och grupper. Det här användar kontot kräver följande inställningar för åtkomst kontroll: </br>– Medlemskap i domän gruppen för kompatibel åtkomst för Windows 2000. </br> -Beviljas användar rättigheten **generera säkerhets granskningar** .             |
| MIMCMManagerAgent   | Utför hanterings aktiviteter för CA. </br> Den här användaren måste tilldelas behörigheten Hantera certifikat utfärdare.        |
| MIMCMWebAgent       | Tillhandahåller identiteten för IIS-programpoolen. FIM CM körs i en Microsoft Win32-® application programming interface process som använder den här användarens autentiseringsuppgifter. </br> Det här användar kontot kräver följande inställningar för åtkomst kontroll:</br> – Medlemskap i den lokala **IIS_WPG, windows 2016 = IIS_IUSRS-** gruppen. </br>– Medlemskap i den lokala **Administratörs** gruppen.</br>-Beviljas användar rättigheten **generera säkerhets granskningar** . </br>– Beviljas **åtgärden som en del av användar rättigheten för operativ systemet** . </br>-Beviljas användar rättigheten **Ersätt process nivå** .</br>– Tilldelas som identitet för IIS-programpoolen **CLMAppPool**. </br>-   Beviljas läsbehörighet på den **HKEY_LOCAL_MACHINE\\programvara\\Microsoft\\CLM\\v1.0\\Server\\///användare** registernyckeln. </br>– Det här kontot måste också vara betrott för delegering.|
| MIMCMEnrollAgent    | Utför registrering för en användares räkning. Det här användar kontot kräver följande inställningar för åtkomst kontroll:</br>– Ett certifikat för registrerings agent som utfärdas och installeras i användar arkivet.</br>-   **Tillåt inloggning lokalt** användar rättigheter. </br>-Registrera behörighet i certifikat mal len för **registrerings agenten** (eller den anpassade mallen om en sådan används).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Skapa certifikatmallar för MIM CM tjänst konton

Tre av de tjänst konton som används av MIM CM kräver ett certifikat och konfigurations guiden kräver att du anger namnet på de certifikatmallar som ska användas för att begära certifikat för dem.

Tjänst kontona som kräver certifikat är:

- MIMCMAgent: Det här kontot behöver ett användar certifikat

- MIMCMEnrollAgent: Det här kontot behöver ett registrerings agent certifikat

- MIMCMKRAgent: Det här kontot måste ha ett certifikat för **nyckel återställnings agenten**

Det finns redan mallar i AD, men vi behöver skapa våra egna versioner för att arbeta med MIM CM. När vi behöver göra ändringar från de ursprungliga bas linje mallarna.

Alla tre av kontona ovan har utökade rättigheter i din organisation och bör hanteras noggrant.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Skapa mallen för MIM CM signerings certifikat

1. Från **administrations verktyg**öppnar du **certifikat utfärdare**.

2. I konsolen **certifikat utfärdare** , i konsol trädet, expanderar du **contoso-CorpCA**och klickar sedan på **certifikatmallar**.

3. Högerklicka på **certifikatmallar**och klicka sedan på **Hantera**.

4. I **konsolen Certifikatmallar**, i **informations** fönstret, väljer du och högerklickar på **användare**och klickar sedan på **Duplicera mall**.

5. I dialog rutan **Duplicera mall** väljer du **Windows Server 2003 Enterprise**och klickar sedan på **OK**.

    ![Visa resulterande ändringar](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM fungerar inte med certifikat som baseras på Certifikatmallar i version 3. Du måste skapa en certifikatmall för Windows Server® 2003 Enterprise (version 2). Se [V3 information Öppna](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) den certifikathanteringx64 directory.

6. I dialog rutan **Egenskaper för ny mall** går du till fliken **allmänt** och skriver **MIM cm signering**i rutan **mallens visnings namn** . Ändra **giltighets perioden** till **två år**och avmarkera sedan kryss rutan **publicera certifikat i Active Directory** .

7. På fliken **hantering av begär Anden** kontrollerar du att kryss rutan Tillåt att den **privata nyckeln exporteras** är markerad och klickar sedan på **fliken Kryptografi**.

8. I dialog rutan **kryptografi val** inaktiverar du **Microsoft Enhanced Cryptographic Provider v 1.0**, aktiverar **Microsoft Enhanced RSA och AES Cryptographic Provider**och klickar sedan på **OK**.

9. På fliken **namn på certifikat mottagare** avmarkerar du kryss rutorna **Inkludera e-postnamn i namn på certifikat mottagare** och **e-postnamn** .

10. På fliken **tillägg** , i listan **tillägg som ingår i den här mallen** , se till att **användnings principer** är markerat och klicka sedan på **Redigera**.

11. I dialog rutan **Redigera tillägg för användnings principer** väljer du både **Krypterande filsystem** och principer för **säker e-** postprogram. Klicka på **ta bort**och sedan på **OK**.

12. På fliken **säkerhet** utför du följande steg:

    - Ta bort **administratör**.

    - Ta bort **domän administratörer**.

    - Ta bort **domän användare**.

    - Tilldela endast **Läs** -och **Skriv** behörighet till **företags administratörer**.

    - Lägg till **MIMCMAgent.**

    - Tilldela **Läs** -och **registrerings** behörigheter till **MIMCMAgent**.

13. I dialog rutan **Egenskaper för ny mall** klickar du på **OK**.

14. Lämna **konsolen Certifikatmallar** öppen.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Skapa certifikat mal len för MIM CM registrerings agenten

1. I **konsolen Certifikatmallar**i **informations** fönstret markerar du och högerklickar på **registrerings agenten**och klickar sedan på **Duplicera mall**.

2. I dialog rutan **Duplicera mall** väljer du **Windows Server 2003 Enterprise**och klickar sedan på **OK**.

3. I dialog rutan **Egenskaper för ny mall** går du till fliken **allmänt** och skriver **MIM cm registrerings agent**i rutan **mallens visnings namn** . Se till att **giltighets perioden** är **2 år**.

4. På fliken **hantering av begär Anden** aktiverar du Tillåt att den **privata nyckeln exporteras**och klickar sedan på **fliken CSP eller kryptografi.**

5. I dialog rutan **Val av kryptografiprovider** inaktiverar du **Microsoft Base Cryptographic Provider v 1.0**, inaktiverar **Microsoft Enhanced Cryptographic Provider v 1.0**, aktiverar **Microsoft Enhanced RSA och AES Cryptographic Provider**och klickar sedan på **OK**.

6. På fliken **säkerhet** gör du följande:

    - Ta bort **administratör**.

    - Ta bort **domän administratörer**.

    - Tilldela endast **Läs** -och **Skriv** behörighet till **företags administratörer**.

    - Lägg till **MIMCMEnrollAgent**.

    - Tilldela **Läs** -och **registrerings** behörigheter till **MIMCMEnrollAgent**.

7. I dialog rutan **Egenskaper för ny mall** klickar du på **OK**.

8. Lämna **konsolen Certifikatmallar** öppen.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Skapa certifikat mal len för MIM CM nyckel återställnings agenten

1. I konsolen **certifikatmallar** i **informations** fönstret markerar du och högerklickar på **nyckel återställnings agenten**och klickar sedan på **Duplicera mall**.

2. I dialog rutan **Duplicera mall** väljer du **Windows Server 2003 Enterprise**och klickar sedan på **OK**.

3. I dialog rutan **Egenskaper för ny mall** går du till fliken **allmänt** och skriver **MIM cm nyckel återställnings agent**i rutan **mallens visnings namn** . Se till att **giltighets perioden** är **2 år** på **fliken Kryptografi.**

4. I dialog **rutan Val av leverantörer** inaktiverar du **Microsoft Enhanced Cryptographic Provider v 1.0**, aktiverar **Microsoft Enhanced RSA och AES Cryptographic Provider**och klickar sedan på **OK**.

5. På fliken **krav för utfärdande** kontrollerar du att **certifikat utfärdarens certifikat hanterare** har **inaktiverats**.

6. På fliken **säkerhet** gör du följande:

    - Ta bort **administratör**.

    - Ta bort **domän administratörer**.

    - Tilldela endast **Läs** -och **Skriv** behörighet till **företags administratörer**.

    - Lägg till **MIMCMKRAgent**.

    - Tilldela **Läs** -och **registrerings** behörigheter till **KRAgent**.

7. I dialog rutan **Egenskaper för ny mall** klickar du på **OK**.

8. Stäng **konsolen för certfikatmallar**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicera nödvändiga certifikatmallar på certifikat utfärdaren

1. Återställa konsolen för **certifikat utfärdare** .

2. I konsolen **certifikat utfärdare** , i konsol trädet, högerklickar du på **certifikatmallar**, pekar på **ny**och klickar sedan på **certifikatmall som ska utfärdas**.

3. I dialog rutan **Aktivera certifikatmallar** väljer du **MIM cm registrerings agent**, **MIM cm nyckel återställnings agent**och **MIM cm signering**. Klicka på **OK**.

4. I konsol trädet klickar du på **certifikatmallar**.

5. Kontrol lera att de tre nya mallarna visas i **informations** fönstret och stäng sedan **certifikat utfärdaren**.

    ![MIM CM signering](media/mim-cm-deploy/image016.png)

6. Stäng alla öppna fönster och logga ut.

### <a name="iis-configuration"></a>IIS-konfiguration

Installera och konfigurera IIS för att vara värd för webbplatsen för CM.

#### <a name="install-and-configure-iis"></a>Installera och konfigurera IIS

1. Logga in på **CORLog i** som **MIMINSTALL** -konto

    >[!IMPORTANT]
    >Installations kontot för MIM måste vara en lokal administratör

2. Öppna PowerShell och kör följande kommando

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>En webbplats med namnet standard webbplats installeras som standard med IIS 7. Om webbplatsen har bytt namn eller tagits bort, måste en webbplats med namnet standard webbplats vara tillgänglig innan MIM CM kan installeras.

#### <a name="configuring-kerberos"></a>Konfigurera Kerberos

MIMCMWebAgent-kontot kommer att köra MIM CM-portalen. Som standard används autentisering i IIS och kernel-läge i IIS som standard. Du inaktiverar Kerberos kernel mode-autentisering och konfigurerar SPN på MIMCMWebAgent-kontot i stället. Vissa kommandon kräver förhöjd behörighet i Active Directory och CORPCM Server.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Uppdatera IIS på CORPCM**

![Venndiagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Du måste lägga till en DNS A-post för "cm.contoso.com" och peka på CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Kräver SSL på MIM CM Portal

Vi rekommenderar starkt att du kräver SSL på MIM CM-portalen. Om du inte gör det kommer guiden att varna dig om den.

1. Registrera i webb certifikat för **cm.contoso.com** tilldela till standard plats

2. Öppna **IIS-hanteraren** och navigera till **certifikat hantering**

3. I Visa funktioner, dubbelklicka på SSL-inställningar.

4. På sidan SSL-inställningar väljer du **KRÄV SSL**.

5. I rutan åtgärder klickar du på **tillämpa.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>**CORPSQL** för databas konfiguration för MIM cm

1. Se till att du är ansluten till CORPSQL01-servern.

2. Se till att du är inloggad som SQL DBA.

3. Kör följande T-SQL-skript för att låta CONTOSO\\MIMINSTALL-kontot skapa databasen när vi går till konfigurations steget

    >[!NOTE]
    >Vi kommer att behöva komma tillbaka till SQL när vi är redo för att avsluta & principmodulen

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Fel meddelande för MIM CM konfigurations guiden](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Distribution av Microsoft Identity Manager 2016 certifikat hantering

1. Se till att du är ansluten till CORPCM-servern och att **MIMINSTALL** -kontot är medlem i den **lokala gruppen Administratörer** .

2. Se till att du är inloggad som contoso\\MIMINSTALL.

3. Montera Microsoft Identity Manager SP1 ISO.

4. **Öppna** **certifikat hanterings\\x64** -katalogen.

5. I **x64** -fönstret högerklickar du på **installation**och klickar sedan på **Kör som administratör**.

6. Klicka på Nästa på sidan Välkommen till installations guiden för Certifikathantering för Microsoft Identity Manager **.**

7. På sidan licens avtal för slutanvändare läser du avtalet, aktiverar **kryss rutan**jag accepterar villkoren i licens avtalet och klickar sedan på Nästa.

8. På sidan anpassad installation ser du till att **MIM cm Portal** och **MIM cm uppdaterings tjänst komponenter** är inställda på att installeras och klickar sedan **på nästa**.

9. På sidan virtuell webbmapp ser du till att namnet på den virtuella mappen är **CertificateManagement**och **klickar sedan på nästa**.

10. På sidan Installera Certifikathantering för Microsoft Identity Manager klickar du **på installera**.

11. **Klicka på Slutför**på sidan **Slutför** installations guiden för certifikathantering för Microsoft Identity Manager.

![Guiden MIM CM slutförd](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Konfigurations guiden för Microsoft Identity Manager 2016 certifikat hantering

Innan du loggar in på CORPCM kan du lägga till MIMINSTALL i **domän administratörer, schema administratörer och lokala administratörs** grupper för konfigurations guiden. Detta kan tas bort senare när konfigurationen är klar.

![Felmeddelande](media/mim-cm-deploy/image028.png)

1. Från **Start** -menyn klickar du på **konfigurations guiden för certifikat hantering**. Och kör som **administratör**

2. Klicka på **Nästa**på sidan **Välkommen till konfigurations guiden** .

3. På sidan **ca-konfiguration** ser du till att den valda certifikat utfärdaren är **contoso-CORPCA-ca**, se till att den valda servern är **CORPCA. CONTOSO.COM**och klickar sedan på **Nästa**.

4. På sidan **Konfigurera Microsoft® SQL Server® databas** i rutan **namn på SQL Server** skriver du **CORPSQL1** , aktiverar kryss rutan **Använd mina autentiseringsuppgifter för att skapa databasen** och klickar sedan på **Nästa**.

5. På sidan **databas inställningar** godkänner du standard databas namnet för **FIMCertificateManagement**, kontrollerar att SQL- **integrerad autentisering** är markerat och klickar sedan på **Nästa**.

6. På sidan **konfigurera Active Directory** godkänner du det standard namn som angetts för tjänst anslutnings punkten och klickar sedan på **Nästa**.

7. Bekräfta att **Windows-integrerad autentisering** är markerat på sidan **autentiseringsmetod** och klicka sedan på **Nästa**.

8. På sidan **agenter – FIM cm** avmarkerar du kryss rutan **Använd STANDARDINSTÄLLNINGARNA för FIM cm** och klickar sedan på **anpassade konton**.

9. I dialog rutan **agenter – FIM cm** multi-tabbed skriver du följande information på varje flik:

   - Användarnamn: **Uppdatera**

   - Lösenord: **Pass\@word1**

   - Bekräfta lösen ord: **Pass\@word1**

   - Använd en befintlig användare: **Aktiverad**

     >[!NOTE]
     >De här kontona har skapats tidigare. Se till att procedurerna i steg 8 upprepas för alla sex agent konto flikar.

     ![MIM CM konton](media/mim-cm-deploy/image030.png)

10. När all agent konto information är klar klickar du på **OK**.

11. På sidan **agenter – MIM cm** klickar du på **Nästa**.

12. På sidan **Konfigurera Server certifikat** aktiverar du följande certifikatmallar:

    - Certifikatmall som ska användas för återställnings agent certifikatet för agent nyckel: **MIMCMKeyRecoveryAgent**.

    - Certifikatmall som ska användas för FIM CM-agent certifikatet: **MIMCMSigning**.

    - Certifikatmall som ska användas för registrerings agent certifikatet: **FIMCMEnrollmentAgent**.

13. På sidan **Konfigurera Server certifikat** klickar du på **Nästa**.

14. På sidan Skriv **ut e-postserver, dokument utskrift** , i rutan **Ange namnet på den SMTP-server som du vill använda för meddelanden om registrering av e-post** och klicka sedan på **Nästa.**

15. På sidan **klar att konfigurera** klickar du på **Konfigurera**.

16. I dialog rutan **konfigurations guiden – Microsoft Forefront Identity Manager 2010 R2** , klickar du på **OK** för att bekräfta att SSL inte är aktiverat på den virtuella IIS-katalogen.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Klicka inte på knappen Slutför förrän körningen av konfigurations guiden har slutförts. Loggning för guiden hittar du här: **% programfiles%\\Microsoft Forefront Identity Management\\2010\\certifikathantering\\config.log**

17. Klicka på **Slutför**.

    ![Guiden MIM CM slutförd](media/mim-cm-deploy/image033.png)

18. Stäng alla öppna fönster.

19. Lägg till `https://cm.contoso.com/certificatemanagement` i zonen Lokalt intranät i webbläsaren.

20. På den `https://cm.contoso.com/certificatemanagement`licensavtalet Läs avtalet.  

    ![Venndiagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verifiera isolerings tjänsten för CNG-nyckeln

1. Från **administrations verktyg**öppnar du **tjänster**.

2. I **informations** fönstret dubbelklickar du på **CNG-nyckel isolering**.

3. På fliken **Allmänt** ändrar du **Start metod** till **Automatisk**.

4. På fliken **Allmänt** startar du tjänsten om den inte är i ett start läge.

5. På fliken **Allmänt** klickar du på **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Installera och konfigurera CA-modulerna:

I det här steget ska vi installera och konfigurera FIM CM CA-moduler på certifikat utfärdaren.

1. Konfigurera FIM CM för att endast granska användar behörigheter för hanterings åtgärder

2. I mappen **C:\\Program\\Microsoft Forefront Identity Manager\\2010\\certifikat hantering\\Web** , gör du en kopia av **Web. config** och kopierar filen **Web. 1. config**.

3. Högerklicka på **Web. config**i **webb** fönstret och klicka sedan på **Öppna**.

    >[!Note]
    >Filen Web. config öppnas i anteckningar

4. När filen öppnas trycker du på CTRL + F.

5. I dialog rutan **Sök och ersätt** i rutan **Sök efter** skriver du **UseUser**och klickar sedan på **Sök nästa** tre gånger.

6. Stäng dialog rutan **Sök och ersätt** .

7. Du bör vara på raden **\<lägga till key="Clm.RequestSecurity.Flags” value = ”UseUser UseGroups” /\>** . Ändra raden för att läsa **\<Lägg till nyckel = "CLM. RequestSecurity. Flags" value = "UseUser"/\>** .

8. Stäng filen och spara alla ändringar.

9. Skapa ett konto för CA-datorn på SQL-servern \<inget skript\>

10. Se till att du är ansluten till **CORPSQL01** -servern.

11. Kontrol lera att du är inloggad som **DBA**

12. Starta **SQL Server Management Studio**på **Start** -menyn.

13. I rutan **Server namn** i dialog rutan **Anslut till Server** skriver du **CORPSQL01** och klickar sedan på **Anslut**.

14. I konsol trädet expanderar du **säkerhet**och klickar sedan på **inloggningar**.

15. Högerklicka på **inloggningar**, och klicka sedan på **ny inloggning**.

16. Skriv **contoso\\CORPCA\$** i rutan **inloggnings namn** på sidan **Allmänt** . Välj **Windows-autentisering**. Standard databasen är **FIMCertificateManagement**.

17. Välj **användar mappning**i det vänstra fönstret. I den högra rutan klickar du på kryss rutan i **kart** kolumnen bredvid **FIMCertificateManagement**. I medlemskapet i **databas roll för: Aktivera **clmApp** -rollen FIMCertificateManagement** List.

18. Klicka på **OK**.

19. Stäng **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Installera FIM CM CA-modulerna på certifikat utfärdaren

1. Se till att du är ansluten till **CORPCA** -servern.

2. I **x64** -Windows högerklickar du på **Setup. exe**och klickar sedan på **Kör som administratör**.

3. Klicka på **Nästa**på sidan **Välkommen till installations guiden för certifikathantering för Microsoft Identity Manager** .

4. Läs avtalet på sidan **licens avtal för slutanvändare** . Markera kryss rutan **Jag accepterar villkoren i licens avtalet** och klicka sedan på **Nästa**.

5. På sidan **anpassad installation** väljer du **MIM cm Portal**och klickar sedan på **den här funktionen kommer inte att vara tillgänglig**.

6. På sidan **anpassad installation** väljer du **MIM cm uppdaterings tjänst**och klickar sedan på **den här funktionen kommer inte att vara tillgänglig**.

    >[!Note]
    >Detta lämnar MIM CM CA-filer som den enda funktionen som är aktive rad för installationen.

7. Klicka på **Nästa**på sidan **anpassad installation** .

8. På sidan **installera certifikathantering för Microsoft Identity Manager** klickar du på **Installera**.

9. Klicka på **Slutför**på sidan **Slutför installations guiden för certifikathantering för Microsoft Identity Manager** .

10. Stäng alla öppna fönster.

### <a name="configure-the-mim-cm-exit-module"></a>Konfigurera MIM CM stängningsmoduler-modulen

1. Från **administrations verktyg**öppnar du **certifikat utfärdare**.

2. I konsol trädet högerklickar du på **contoso-CORPCA-ca**och klickar sedan på **Egenskaper**.

3. På fliken **Avsluta modul** väljer du **FIM cm-stängningsmodul**och klickar sedan på **Egenskaper**.

4. Skriv Connect timeout = 15 i rutan **Ange anslutnings sträng för cm-databas** **. Behåll säkerhets information = sant; Integrated Security = SSPI; initial catalog = FIMCertificateManagement; data source = CORPSQL01**. Låt kryss rutan **kryptera anslutnings strängen** vara aktive rad och klicka sedan på **OK**.
5. I rutan **Microsoft FIM certifikat hanterings** meddelande klickar du på **OK**.

6. I dialog rutan **contoso-CORPCA-ca-egenskaper** klickar du på **OK**.

7. Högerklicka på **contoso-CORPCA-ca** *,* peka på **alla aktiviteter**och klicka sedan på **stoppa tjänst**. Vänta tills Active Directory Certificate Services stoppas.

8. Högerklicka på **contoso-CORPCA-ca** *,* peka på **alla aktiviteter**och klicka sedan på **Starta tjänst**.

9. Minimera konsolen för **certifikat utfärdare** .

10. Från **administrations verktyg**öppnar du **Loggboken**.

11. I konsol trädet expanderar du **program-och tjänst loggar**och klickar sedan på **FIM-certifikat hantering**.

12. I listan över händelser kontrollerar du att de senaste händelserna *inte* innehåller några **varnings** -eller **fel** händelser sedan den senaste omstarten av Certificate Services.

    >[!NOTE] 
    >Den sista händelsen ska ange att stängningsmodulen som läses in med inställningar från: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimera **Loggboken**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Kopiera MIMCMAgent-certifikatets tumavtryck till Windows® Clipboard

1. Återställa konsolen för **certifikat utfärdare** .

2. I konsol trädet expanderar du **contoso-CORPCA-ca**och klickar sedan på **utfärdade certifikat**.

3. I **informations** fönstret dubbelklickar du på certifikatet med **contoso\\MIMCMAgent** i kolumnen **beställarens namn** och med **FIM cm-signering** i kolumnen **certifikatmall** .

4. Välj fältet **tumavtryck** på fliken **information** .

5. Välj tumavtrycket och tryck på CTRL + C.

    >[!NOTE]
    >Ta **inte** med det inledande utrymmet i listan över tumavtryck.

6. Klicka på **OK**i dialog rutan **certifikat** .

7. Från **Start** -menyn i rutan **Sök efter program och filer** skriver du **anteckningar**och trycker sedan på RETUR.

8. I **anteckningar**, på **Redigera** -menyn, klickar du på **Klistra in**.

9. I **Redigera** -menyn klickar du på **Ersätt**.

10. I rutan **Sök efter** skriver du ett blank stegs tecken och klickar sedan på **Ersätt alla**.

    >[!Note]
    >Detta tar bort alla blank steg mellan tecknen i tumavtrycket.

11. Klicka på **Avbryt**i dialog rutan **Ersätt** .

12. Välj den konverterade *thumbprintstring*och tryck sedan på CTRL + C.

13. Stäng **anteckningar** utan att spara ändringarna.

### <a name="configure-the-fim-cm-policy-module"></a>Konfigurera FIM CM-principmodulen

1. Återställa konsolen för **certifikat utfärdare** .

2. Högerklicka på **contoso-CORPCA-ca**och klicka sedan på **Egenskaper**.

3. I dialog rutan **contoso-CORPCA-ca-egenskaper** klickar du på **Egenskaper**på fliken **principmodul** .

    - På fliken **Allmänt** kontrollerar du att **skicka icke-FIM cm-begäranden till standard-principmodulen för bearbetning** är markerat.

    - Klicka på **Lägg till**på fliken **signerings certifikat** .

    - Högerklicka på rutan **Ange hex-kodat certifikat-hash** i dialog rutan certifikat och klicka sedan på **Klistra in**.

    - Klicka på **OK**i dialog rutan **certifikat** .

        >[!Note]
        >Om knappen **OK** inte är aktive rad inkluderade du oavsiktligt ett dolt tecken i tumavtrycket när du kopierade tumavtrycket från clmAgent-certifikatet. Upprepa alla steg som börjar med **aktivitet 4: Kopiera MIMCMAgent-certifikatets tumavtryck till Urklipp i Windows** i den här övningen.

4. I dialog rutan **konfigurations egenskaper** kontrollerar du att tumavtrycket visas i listan **giltiga signerings certifikat** och klickar sedan på **OK**.

5. I rutan **FIM-certifikat hanterings** meddelande klickar du på **OK**.

6. I dialog rutan **contoso-CORPCA-ca-egenskaper** klickar du på **OK**.

7. Högerklicka på **contoso-CORPCA-ca** *,* peka på **alla aktiviteter**och klicka sedan på **stoppa tjänst**.

8. Vänta tills Active Directory Certificate Services stoppas.

9. Högerklicka på **contoso-CORPCA-ca** *,* peka på **alla aktiviteter**och klicka sedan på **Starta tjänst**.

10. Stäng konsolen för **certifikat utfärdare** .

11. Stäng alla öppna fönster och logga sedan ut.

**I det sista steget i distributionen** vill vi se till att contoso\\MIMCM-chefer kan distribuera och skapa mallar och konfigurera systemet utan att vara schema-och domän administratörer. Nästa skript kommer att ACL: er behörighet till certifikatmallarna med hjälp av dsacls. Kör med kontot som har fullständig behörighet att ändra säkerhets Läs-och skriv behörigheter till varje befintlig certifikatmall i skogen.

Första stegen: **Konfigurera tjänst anslutnings punkt och mål grupps behörigheter & att delegera hantering av profil mal len**

1. Konfigurera behörigheter för tjänst anslutnings punkten (SCP).

2. Konfigurera hantering av delegerad profil mall.

3. Konfigurera behörigheter för tjänst anslutnings punkten (SCP). **\<saknar skript\>**

4.   Se till att du är ansluten till den virtuella **CORPDC** -servern.

5. Logga in som **contoso\\corpadmin**

6. Från **administrations verktyg**öppnar **du Active Directory användare och datorer**.

7. I **Active Directory användare och datorer**, i menyn **Visa** , se till att **avancerade funktioner** är aktiverade.

8. I konsolträdet expanderar **Contoso.com** \| **System** \| **Microsoft** \| **certifikatets livscykel Manager**, och klicka sedan på **CORPCM**.

9. Högerklicka på **CORPCM**och klicka sedan på **Egenskaper**.

10. I dialog rutan **Egenskaper för CORPCM** , på fliken **säkerhet** , lägger du till följande grupper med motsvarande behörigheter:

    | Grupp          | Behörigheter      |
    |----------------|------------------|
    | mimcm – chefer | Läsa </br> FIM CM-granskning</br> FIM CM-registrerings agent</br> Registrera FIM CM-begäran</br> FIM CM-begäran återställning</br> Förnya FIM CM-begäran</br> FIM CM-begäran återkalla </br> FIM CM-begäran avblockera smartkort |
    | mimcm – supportavdelningen | Läsa</br> FIM CM-registrerings agent</br> FIM CM-begäran återkalla</br> FIM CM-begäran avblockera smartkort |

11. I dialog rutan **Egenskaper för CORPDC** klickar du på **OK**.

12. Lämna **Active Directory användare och datorer** öppna.

**Konfigurera behörigheter för de underordnade användar objekten**

1. Se till att du fortfarande befinner dig i konsolen **Active Directory användare och datorer** .

2. I konsol trädet högerklickar du på **contoso.com**och klickar sedan på **Egenskaper**.

3. På fliken **säkerhet** klickar du på **Avancerat**.

4. I dialog rutan **avancerade säkerhets inställningar för Contoso** klickar du på **Lägg till**.

5. I dialog rutan **Välj användare, dator, tjänst konto eller grupp** , i rutan **Ange det objekt namn som ska väljas** , skriver du **mimcm-Managers**och klickar sedan på **OK**.

6. I dialog rutan **behörighets post för Contoso** väljer du **underordnade användar objekt** i listan **tillämpa på** och aktiverar sedan kryss rutan **Tillåt** för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läs behörigheter**

    - **FIM CM-granskning**

    - **FIM CM-registrerings agent**

    - **Registrera FIM CM-begäran**

    - **FIM CM-begäran återställning**

    - **Förnya FIM CM-begäran**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran avblockera smartkort**

7. I dialog rutan **behörighets post för Contoso** klickar du på **OK**.

8. I dialog rutan **avancerade säkerhets inställningar för Contoso** klickar du på **Lägg till**.

9. I dialog rutan **Välj användare, dator, tjänst konto eller grupp** , i rutan **Ange det objekt namn som ska väljas** , skriver du **mimcm-helpdesk**och klickar sedan på **OK**.

10. I dialog rutan **behörighets post för Contoso** väljer du **underordnade användar objekt** i listan **tillämpa på** och markerar sedan kryss rutan **Tillåt** för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läs behörigheter**

    - **FIM CM-registrerings agent**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran avblockera smartkort**

11. I dialog rutan **behörighets post för Contoso** klickar du på **OK**.

12. I dialog rutan **avancerade säkerhets inställningar för Contoso** klickar du på **OK**.

13. I dialog rutan **Egenskaper för contoso.com** klickar du på **OK**.

14. Lämna **Active Directory användare och datorer** öppna.

**Konfigurera behörigheter för underordnade användar objekt \<inga skript\>**

1. Se till att du fortfarande befinner dig i konsolen **Active Directory användare och datorer** .

2. I konsol trädet högerklickar du på **contoso.com**och klickar sedan på **Egenskaper**.

3. På fliken **säkerhet** klickar du på **Avancerat**.

4. I dialog rutan **avancerade säkerhets inställningar för Contoso** klickar du på **Lägg till**.

5. I dialog rutan **Välj användare, dator, tjänst konto eller grupp** , i rutan **Ange det objekt namn som ska väljas** , skriver du **mimcm-Managers**och klickar sedan på **OK**.

6. I dialog rutan **behörighets post för Contoso** väljer du **underordnade användar objekt** i listan **tillämpa på** och aktiverar sedan kryss rutan **Tillåt** för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läs behörigheter**

    - **FIM CM-granskning**

    - **FIM CM-registrerings agent**

    - **Registrera FIM CM-begäran**

    - **FIM CM-begäran återställning**

    - **Förnya FIM CM-begäran**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran avblockera smartkort**

7. I dialog rutan **behörighets post för Contoso** klickar du på **OK**.

8. I dialog rutan **avancerade säkerhets inställningar för Contoso** klickar du på **Lägg till**.

9. I dialog rutan **Välj användare, dator, tjänst konto eller grupp** , i rutan **Ange det objekt namn som ska väljas** , skriver du **mimcm-helpdesk**och klickar sedan på **OK**.

10. I dialog rutan **behörighets post för Contoso** väljer du **underordnade användar objekt** i listan **tillämpa på** och markerar sedan kryss rutan **Tillåt** för följande **behörigheter**:

    - **Läsa alla egenskaper**

    - **Läs behörigheter**

    - **FIM CM-registrerings agent**

    - **FIM CM-begäran återkalla**

    - **FIM CM-begäran avblockera smartkort**

11. I dialog rutan **behörighets post för Contoso** klickar du på **OK**.

12. I dialog rutan **avancerade säkerhets inställningar för Contoso** klickar du på **OK**.

13. I dialog rutan **Egenskaper för contoso.com** klickar du på **OK**.

14. Lämna **Active Directory användare och datorer** öppna.

Andra steg: **Delegera hanterings behörigheter för certifikat mal len \<skript\>**

- Delegera behörigheter för behållaren certifikatmallar.

- Delegerar behörigheter för OID-behållaren.

- Delegera behörigheter för befintliga certifikatmallar.

Definiera behörigheter för behållaren certifikatmallar:

1. Återställa konsolen **Active Directory platser och tjänster** .

2. I konsol trädet expanderar du **tjänster**, expanderar **tjänster för offentliga nycklar**och klickar sedan på **certifikatmallar**.

3. I konsol trädet högerklickar du på **certifikatmallar**och klickar sedan på **delegera kontroll**.

4. I guiden **delegera kontroll** klickar du på **Nästa**.

5. På sidan **användare eller grupper** klickar du på **Lägg till**.

6. I dialog rutan **Välj användare, datorer eller grupper** i rutan **Ange de objekt namn som ska väljas skriver du** **mimcm-Managers**och klickar sedan på **OK**.

7. På sidan **användare eller grupper** klickar du på **Nästa**.

8. På sidan **aktiviteter som ska delegeras** klickar **du på skapa en anpassad aktivitet som ska delegeras**och klickar sedan på **Nästa**.

9.  På sidan **Active Directory objekt typ** kontrollerar du att **mappen, befintliga objekt i den här mappen och skapandet av nya objekt i den här mappen** är markerad och klickar sedan på **Nästa**.

10. På sidan **behörigheter** i listan **behörigheter** markerar du kryss rutan **fullständig behörighet** och klickar sedan på **Nästa**.

11. Klicka på **Slutför**på sidan **Slutför guiden Delegera kontroll** .

Definiera behörigheter för OID-behållaren:

1. I konsol trädet högerklickar du på **OID**och klickar sedan på **Egenskaper**.

2. I dialog rutan **OID-egenskaper** klickar du på **Avancerat**på fliken **säkerhet** .

3. I dialog rutan **avancerade säkerhets inställningar för OID** klickar du på **Lägg till**.

4. I dialog rutan **Välj användare, dator, tjänst konto eller grupp** , i rutan **Ange det objekt namn som ska väljas** , skriver du **mimcm-Managers**och klickar sedan på **OK**.

5. I dialog rutan **behörighets post för OID** kontrollerar du att behörigheterna gäller för **det här objektet och alla underordnade objekt**, klickar du på **fullständig behörighet**och klickar sedan på **OK**.

6. I dialog rutan **avancerade säkerhets inställningar för OID** klickar du på **OK**.

7. I dialog rutan **OID-egenskaper** klickar du på **OK**.

8. Stäng **Active Directory-platser och-tjänster**.

**skript: Behörigheter för mappen OID, Profile & certifikat mal len behållare**

![Venndiagram](media/mim-cm-deploy/image021.png)

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

**skript: Delegera behörigheter för befintliga certifikatmallar.**  

![Venndiagram](media/mim-cm-deploy/image039.png)

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
