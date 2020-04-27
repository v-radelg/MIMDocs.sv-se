---
title: Distribuera PAM steg 1 – CORP-domän | Microsoft Docs
description: Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c21228dad923d80ab63c255c1184b7de04a0ff3d
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043740"
---
# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>Steg 1 – förbereda värden och CORP-domänen

> [!div class="step-by-step"]
> [Steg 2»](step-2-prepare-priv-domain-controller.md)

I det här steget förbereder du hanteringen av skyddsmiljön. Vid behov kan du också skapa en domänkontrollant och en medlemsarbetsstation i en ny domän och skog (*CORP*-skogen) med identiteter som ska hanteras av skyddsmiljön. Den här CORP-skogen simulerar en befintlig skog som har resurser som ska hanteras. I det här dokumentet finns en exempelresurs som ska skyddas, en filresurs.

Om du redan har en befintlig Active Directory-domän (AD) med en domänkontrollant som kör Windows Server 2012 R2 eller senare, i vilken du är domänadministratör, kan du använda den domänen i stället.  

## <a name="prepare-the-corp-domain-controller"></a>Förbereda CORP-domänkontrollanten

I det här avsnittet beskrivs hur du konfigurerar en domänkontrollant för en CORP-domän. I CORP-domänen hanteras de administrativa användare av skyddsmiljön. I det här exemplet används *contoso.local* som DNS-namn (Domain Name System) för CORP-domänen.

### <a name="install-windows-server"></a>Installera Windows Server

Installera Windows Server 2012 R2 eller Windows Server 2016 Technical Preview 4 eller senare på en virtuell dator för att skapa en dator som kallas *CORPDC*.

1. Välj **Windows Server 2012 R2 Standard (server med GUI) x64** eller **Windows Server 2016 Technical Preview (server med Skrivbordsmiljö)**.

2. Granska och godkänn licensvillkoren.

3. Eftersom disken är tom väljer du **Anpassad: Installera endast Windows** och Använd det oinitierade disk utrymmet.

4. Logga in på den nya datorn som administratör. Gå till Kontrollpanelen. Ange datornamnet till *CORPDC*, och tilldela en statisk IP-adress på det virtuella nätverket. Starta om servern.

5. När servern har startats om loggar du in som administratör. Gå till Kontrollpanelen. Konfigurera datorn att söka efter uppdateringar och installera de uppdateringar som krävs. Starta om servern.

### <a name="add-roles-to-establish-a-domain-controller"></a>Lägga till roller för att upprätta en domänkontrollant

I det här avsnittet lägger du till roller för Active Directory Domain Services (AD DS), DNS-server och filserver (i avsnittet fil- och lagringstjänster) och befordrar servern till en domänkontrollant för en ny contoso.local-skog.

> [!NOTE]  
> Om du redan har en domän som ska användas som CORP-domän, och domänen använder Windows Server 2012 R2 eller senare som domänkontrollant, kan du gå vidare till [Skapa ytterligare användare och grupper i exempelsyfte](#create-additional-users-and-groups-for-demonstration-purposes).

1. När du är inloggad som administratör startar du PowerShell.

2. Skriv in följande kommandon:

   ```PowoerShell
   import-module ServerManager

   Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

   Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
   ```

   En uppmaning visas om att du ska ange ett administratörslösenord för säkert läge. Obs! Varningsmeddelanden för DNS-delegering och kryptografiinställningar visas. Det är normalt.

3. När du har skapat skogen loggar du ut. Servern kommer att startas om automatiskt.

4. När servern har startats om logga du in på CORPDC som administratör för domänen. Det är vanligtvis användaren CONTOSO\\Administratör som har lösenordet som skapades när du installerade Windows på CORPDC.

### <a name="create-a-group"></a>Skapa en grupp

Skapa en grupp för granskning av Active Directory om gruppen inte redan finns. Namnet på gruppen måste vara NetBIOS-domännamnet följt av tre dollartecken, till exempel *CONTOSO$$$*.

Logga in på en domänkontrollant som domänadministratör för varje domän och utför följande steg:

1. Starta PowerShell.

2. Skriv följande kommandon, men ersätt "CONTOSO" med NetBIOS-namnet på din domän.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
   ```

I vissa fall kan gruppen redan finnas. Det är normalt om domänen också har använts i AD-migreringsscenarier.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>Skapa ytterligare användare och grupper i exempelsyfte

Om du har skapat en ny CORP-domän bör du skapa ytterligare användare och grupper för att visa PAM-scenariot. De användare och grupper du skapar i exempelsyfte bör inte vara domänadministratörer eller styras av inställningarna för adminSDHolder i AD.

> [!NOTE]
> Om du redan har en domän som du tänker använda som CORP-domän och har en användare och en grupp som du kan använda som exempel kan du gå vidare till avsnittet [Konfigurera granskning](#configure-auditing).

Vi ska skapa en säkerhetsgrupp med namnet *CorpAdmins* och en användare med namnet *Lisa*. Du kan använda olika namn om du vill.

1. Starta PowerShell.

2. Skriv in följande kommandon: Ersätt lösenordet ”Pass@word1” med ett annat lösenord.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

   New-ADUser –SamAccountName Jen –name Jen

   Add-ADGroupMember –identity CorpAdmins –Members Jen

   $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   Set-ADAccountPassword –identity Jen –NewPassword $jp

   Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
   ```

### <a name="configure-auditing"></a>Konfigurera granskning

Du måste aktivera granskning i befintliga skogar för att upprätta PAM-konfigurationen på de skogarna.  

Logga in på en domänkontrollant som domänadministratör för varje domän och utför följande steg:

1. Gå till **Start** > **administrations verktyg** (eller, på Windows Server 2016, **Windows administrations verktyg**) och starta **Grupprincip hantering**.

2. Gå till domänens princip för domänkontrollanter.  Om du har skapat en ny domän för contoso. local navigerar du till **skog: contoso. local** > **Domains** > Domains**contoso. lokala** > **Domain Controllers** > domänkontrollanter**standard domän kontroller**. Ett informationsmeddelande visas.

3. Högerklicka på **Standardprincip för domänkontrollanter** och välj **Redigera**. Ett nytt fönster visas.

4. I fönstret Redigeraren Grupprinciphantering, under princip trädet standard domänkontrollanter, navigerar du till **dator konfiguration** > **principer** > **Windows-inställningar** > **säkerhets inställningar** > **lokala principer** > **gransknings princip**.

5. Högerklicka på **Granska kontohantering** i informationsfönstret och välj **Egenskaper**. Välj **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade** och klicka på **Tillämpa** och **OK**.

6. Högerklicka på **Granska katalogtjänståtkomst** i informationsfönstret och välj **Egenskaper**. Välj **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade** och klicka på **Tillämpa** och **OK**.

7. Stäng fönstren Redigeraren Grupprinciphantering och Grupprinciphantering.

8. Tillämpa granskningsinställningarna genom att öppna ett PowerShell-fönster och skriva:

   ```cmd
   gpupdate /force /target:computer
   ```

Meddelandet **Uppdatering av grupprincip har slutförts** visas efter några minuter.

### <a name="configure-registry-settings"></a>Konfigurera registerinställningar

I det här avsnittet ska du konfigurera de registerinställningar som krävs för migrering av SID-historik och som används till att skapa Privileged Access Management-grupper.

1. Starta PowerShell.

2. Skriv följande kommandon för att konfigurera källdomänen att tillåta RPC-åtkomst (Remote Procedure Call) till databasen för hanteraren för kontosäkerhet (SAM).

   ```PowerShell
   New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

   Restart-Computer
   ```

Domänkontrollanten CORPDC startas om. Mer information om den här registerinställningen finns i [Felsöka sIDHistory-migrering mellan skogar med ADMTv2](https://support.microsoft.com/kb/322970).

## <a name="prepare-a-corp-workstation-and-resource"></a>Förbereda en CORP-arbetsstation och -resurs

Om du inte redan har kopplat en arbetsstationsdator till domänen följer du de här anvisningarna för hur du förbereder en.  

> [!NOTE]
> Om du redan har en arbetsstation kopplad till domänen går du vidare till [Skapa en resurs i exempelsyfte](#create-a-resource-for-demonstration-purposes).

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Installera Windows 8.1 eller Windows 10 Enterprise som en virtuell dator

På en annan virtuell dator utan installerad programvara installerar du Windows 8.1 Enterprise eller Windows 10 Enterprise om du vill göra en dator till *CORPWKSTN*.

1. Använd standardinställningar under installationen.

2. Observera att installationen kanske inte kan ansluta till internet. Välj **Skapa ett lokalt konto**. Ange ett annat användarnamn. Använd inte "Administratör" eller "Lisa".

3. Använd kontrollpanelen till att ge datorn en statisk IP-adress på det virtuella nätverket och ange CORPDC-servern som gränssnittets prioriterade DNS-server.

4. Använd kontrollpanelen och domänanslut CORPWKSTN-datorn till domänen contoso.local. Du måste ange autentiseringsuppgifterna för Contoso-domänens administratör. När det är klart startar du om datorn CORPWKSTN.

### <a name="create-a-resource-for-demonstration-purposes"></a>Skapa en resurs i exempelsyfte

Du behöver en resurs för att visa säkerhetsgruppsbaserad åtkomst med PAM.  Om du inte redan har en resurs kan du använda en filmapp i exempelsyfte.  Här används AD-objekten "Lisa" och "CorpAdmins" som du skapade i domänen contoso.local.

1. Anslut till arbetsstationen CORPWKSTN. Klicka på ikonen **Växla användare** och sedan på **Annan användare**. Kontrollera att användaren CONTOSO\\Lisa kan logga in på CORPWKSTN.

2. Skapa en ny mapp med namnet *CorpFS* och dela den med gruppen *CorpAdmins*.

3. Öppna PowerShell som administratör.

4. Skriv in följande kommandon:

   ```PowerShell
   mkdir c:\corpfs

   New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

   $acl = Get-Acl c:\corpfs

   $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

   $acl.SetAccessRule($car)

   Set-Acl c:\corpfs $acl
   ```

I nästa steg förbereder du PRIV-domänkontrollanten.

> [!div class="step-by-step"]
> [Steg 2»](step-2-prepare-priv-domain-controller.md)
