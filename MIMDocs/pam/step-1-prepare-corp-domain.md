---
title: "Distribuera PAM steg 1 – CORP-domän | Microsoft Docs"
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 1164e7efb70d911497b08248b68f8d929bc6d3fb
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---

# Steg 1 – förbereda värden och CORP-domänen
<a id="step-1---prepare-the-host-and-the-corp-domain" class="xliff"></a>

>[!div class="step-by-step"]
[Steg 2 »](step-2-prepare-priv-domain-controller.md)


I det här steget förbereder du hanteringen av skyddsmiljön. Vid behov kan du också skapa en domänkontrollant och en medlemsarbetsstation i en ny domän och skog (*CORP*-skogen) med identiteter som ska hanteras av skyddsmiljön. Den här CORP-skogen simulerar en befintlig skog som har resurser som ska hanteras. I det här dokumentet finns en exempelresurs som ska skyddas, en filresurs.

Om du redan har en befintlig Active Directory-domän (AD) med en domänkontrollant som kör Windows Server 2012 R2 eller senare, i vilken du är domänadministratör, kan du använda den domänen i stället.  

## Förbereda CORP-domänkontrollanten
<a id="prepare-the-corp-domain-controller" class="xliff"></a>

I det här avsnittet beskrivs hur du konfigurerar en domänkontrollant för en CORP-domän. I CORP-domänen hanteras de administrativa användare av skyddsmiljön. I det här exemplet används *contoso.local* som DNS-namn (Domain Name System) för CORP-domänen.

### Installera Windows Server
<a id="install-windows-server" class="xliff"></a>

Installera Windows Server 2012 R2 eller Windows Server 2016 Technical Preview 4 eller senare på en virtuell dator för att skapa en dator som kallas *CORPDC*.

1. Välj **Windows Server 2012 R2 Standard (server med GUI) x64** eller **Windows Server 2016 Technical Preview (server med Skrivbordsmiljö)**.

2. Granska och godkänn licensvillkoren.

3. Eftersom disken är tom markerar du **Anpassad: Installera bara Windows** och använd diskutrymmet som inte initierats.

4. Logga in på den nya datorn som administratör. Gå till Kontrollpanelen. Ange datornamnet till *CORPDC*, och tilldela en statisk IP-adress på det virtuella nätverket. Starta om servern.

5. När servern har startats om loggar du in som administratör. Gå till Kontrollpanelen. Konfigurera datorn att söka efter uppdateringar och installera de uppdateringar som krävs. Starta om servern.

### Lägga till roller för att upprätta en domänkontrollant
<a id="add-roles-to-establish-a-domain-controller" class="xliff"></a>

I det här avsnittet lägger du till roller för Active Directory Domain Services (AD DS), DNS-server och filserver (i avsnittet fil- och lagringstjänster) och befordrar servern till en domänkontrollant för en ny contoso.local-skog.

> [!NOTE]  
> Om du redan har en domän som ska användas som CORP-domän, och domänen använder Windows Server 2012 R2 eller senare som domänkontrollant, kan du gå vidare till [Skapa ytterligare användare och grupper i exempelsyfte](#create-additional-users-and-groups-for-demonstration-purposes).

1. När du är inloggad som administratör startar du PowerShell.

2. Skriv in följande kommandon:

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  En uppmaning visas om att du ska ange ett administratörslösenord för säkert läge. Obs! Varningsmeddelanden för DNS-delegering och kryptografiinställningar visas. Det är normalt.

3. När skogen har skapats loggar du ut. Servern startar om automatiskt.

4. När servern har startats om logga du in på CORPDC som administratör för domänen. Det är vanligtvis användaren CONTOSO\\Administratör som har lösenordet som skapades när du installerade Windows på CORPDC.

### Skapa en grupp
<a id="create-a-group" class="xliff"></a>

Skapa en grupp för granskning av Active Directory om gruppen inte redan finns. Namnet på gruppen måste vara NetBIOS-domännamnet följt av tre dollartecken, till exempel *CONTOSO$$$*.

Logga in på en domänkontrollant som domänadministratör för varje domän och utför följande steg:

1. Starta PowerShell.

2. Skriv följande kommandon, men ersätt "CONTOSO" med NetBIOS-namnet på din domän.

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

I vissa fall kan gruppen redan finnas. Det är normalt om domänen också har använts i AD-migreringsscenarier.

### Skapa ytterligare användare och grupper i exempelsyfte
<a id="create-additional-users-and-groups-for-demonstration-purposes" class="xliff"></a>

Om du har skapat en ny CORP-domän bör du skapa ytterligare användare och grupper för att visa PAM-scenariot. De användare och grupper du skapar i exempelsyfte bör inte vara domänadministratörer eller styras av inställningarna för adminSDHolder i AD.

> [!NOTE]
> Om du redan har en domän som du tänker använda som CORP-domän och har en användare och en grupp som du kan använda som exempel kan du gå vidare till avsnittet [Konfigurera granskning](#configure-auditing).

Vi ska skapa en säkerhetsgrupp med namnet *CorpAdmins* och en användare med namnet *Lisa*. Du kan använda olika namn om du vill.

1. Starta PowerShell.

2. Skriv in följande kommandon: Ersätt lösenordet ”Pass@word1” med ett annat lösenord.

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### Konfigurera granskning
<a id="configure-auditing" class="xliff"></a>

Du måste aktivera granskning i befintliga skogar för att upprätta PAM-konfigurationen på de skogarna.  

Logga in på en domänkontrollant som domänadministratör för varje domän och utför följande steg:

1. Gå till **Start** > **Administrationsverktyg** (på Windows Server 2016 **Administrationsverktyg för Windows** ) och starta **Grupprinciphantering**.

2. Gå till domänens princip för domänkontrollanter.  Om du har skapat en ny domän för contoso.local går du till **Skog: contoso.local** > **Domäner** > **contoso.local** > **Domänkontrollanter** > **Standardprincip för domänkontrollanter**. Ett informationsmeddelande visas.

3. Högerklicka på **Standardprincip för domänkontrollanter** och välj **Redigera**. Ett nytt fönster visas.

4. I fönstret Redigeraren Grupprinciphantering under trädet Standardprincip för domänkontrollanter går du till **Datorkonfiguration** > **Principer** > **Windows-inställningar** > **Säkerhetsinställningar** > **Lokala principer** > **Granskningsprincip**.

5. Högerklicka på **Granska kontohantering** i informationsfönstret och välj **Egenskaper**. Välj **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade** och klicka på **Tillämpa** och **OK**.

6. Högerklicka på **Granska katalogtjänståtkomst** i informationsfönstret och välj **Egenskaper**. Välj **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade** och klicka på **Tillämpa** och **OK**.

7. Stäng fönstren Redigeraren Grupprinciphantering och Grupprinciphantering.

8. Tillämpa granskningsinställningarna genom att öppna ett PowerShell-fönster och skriva:

  ```
  gpupdate /force /target:computer
  ```

Meddelandet **Uppdatering av grupprincip har slutförts** visas efter några minuter.

### Konfigurera registerinställningar
<a id="configure-registry-settings" class="xliff"></a>

I det här avsnittet ska du konfigurera de registerinställningar som krävs för migrering av SID-historik och som används till att skapa Privileged Access Management-grupper.

1. Starta PowerShell.

2. Skriv följande kommandon för att konfigurera källdomänen att tillåta RPC-åtkomst (Remote Procedure Call) till databasen för hanteraren för kontosäkerhet (SAM).

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

Domänkontrollanten CORPDC startas om. Mer information om den här registerinställningen finns i [Felsöka sIDHistory-migrering mellan skogar med ADMTv2](http://support.microsoft.com/kb/322970).

## Förbereda en CORP-arbetsstation och -resurs
<a id="prepare-a-corp-workstation-and-resource" class="xliff"></a>

Om du inte redan har kopplat en arbetsstationsdator till domänen följer du de här anvisningarna för hur du förbereder en.  

> [!NOTE]
> Om du redan har en arbetsstation kopplad till domänen går du vidare till [Skapa en resurs i exempelsyfte](#create-a-resource-for-demonstration-purposes).

### Installera Windows 8.1 eller Windows 10 Enterprise som en virtuell dator
<a id="install-windows-81-or-windows-10-enterprise-as-a-vm" class="xliff"></a>

På en annan virtuell dator utan installerad programvara installerar du Windows 8.1 Enterprise eller Windows 10 Enterprise om du vill göra en dator till *CORPWKSTN*.

1. Använd standardinställningar under installationen.

2. Observera att installationen kanske inte kan ansluta till internet. Välj **Skapa ett lokalt konto**. Ange ett annat användarnamn. Använd inte "Administratör" eller "Lisa".

3. Använd kontrollpanelen till att ge datorn en statisk IP-adress på det virtuella nätverket och ange CORPDC-servern som gränssnittets prioriterade DNS-server.

4. Använd kontrollpanelen och domänanslut CORPWKSTN-datorn till domänen contoso.local. Du måste ange autentiseringsuppgifterna för Contoso-domänens administratör. När det är klart startar du om datorn CORPWKSTN.

### Skapa en resurs i exempelsyfte
<a id="create-a-resource-for-demonstration-purposes" class="xliff"></a>

Du behöver en resurs för att visa säkerhetsgruppsbaserad åtkomst med PAM.  Om du inte redan har en resurs kan du använda en filmapp i exempelsyfte.  Här används AD-objekten "Lisa" och "CorpAdmins" som du skapade i domänen contoso.local.

1. Anslut till arbetsstationen CORPWKSTN. Klicka på ikonen **Växla användare** och sedan på **Annan användare**. Kontrollera att användaren CONTOSO\\Lisa kan logga in på CORPWKSTN.

2. Skapa en ny mapp med namnet *CorpFS* och dela den med gruppen *CorpAdmins*.

3. Öppna PowerShell som administratör.

4. Skriv in följande kommandon:

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

I nästa steg förbereder du PRIV-domänkontrollanten.

>[!div class="step-by-step"]
[Steg 2 »](step-2-prepare-priv-domain-controller.md)

