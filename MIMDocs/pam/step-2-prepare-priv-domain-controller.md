---
title: Distribuera PAM steg 2 – PRIV DC | Microsoft Docs
description: Förbered PRIV-domänkontrollanten, som kommer att tillhandahålla skyddsmiljön där Privileged Access Management är isolerat.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 97b425fc4444b241ddce99e7d5e3abf564daf245
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043706"
---
# <a name="step-2---prepare-the-first-priv-domain-controller"></a>Steg 2 – Förbereda den första PRIV-domänkontrollanten

> [!div class="step-by-step"]
> [«Steg 1](step-1-prepare-corp-domain.md)
> [steg 3»](step-3-prepare-pam-server.md)

I det här steget ska du skapa en ny domän som tillhandahåller skyddsmiljön för autentisering av administratörer.  Den här skogen måste ha minst en domänkontrollant och minst en medlemsserver. Medlemsservern konfigureras i nästa steg.

## <a name="create-a-new-privileged-access-management-domain-controller"></a>Skapa en ny domänkontrollant för Privileged Access Management

I det här avsnittet konfigurerar du en virtuell dator som ska fungera som en domänkontrollant för en ny skog

### <a name="install-windows-server-2012-r2"></a>Installera Windows Server 2012 R2

Installera Windows Server 2012 R2 för att göra en dator "PRIVDC" på en annan ny virtuell dator utan någon programvara installerad.

1. Välj att utföra en anpassad (inte uppgraderad) installation av Windows Server. Vid installationen anger du **Windows Server 2012 R2 Standard (server med GUI) x64**. _Välj inte _ **Datacenter eller Server Core**.

2. Granska och godkänn licensvillkoren.

3. Eftersom disken är tom väljer du **Anpassad: Installera endast Windows** och Använd det oinitierade disk utrymmet.

4. När du har installerat operativsystemsversionen, loggar du in på den nya datorn som den nya administratören. Använd kontrollpanelen till att ge datorn namnet *PRIVDC*, ge den en statisk IP-adress på det virtuella nätverket och konfigurera DNS-servern som den för domänkontrollanten som installerades i föregående steg. Detta kräver en omstart av servern.

5. När servern har startats om loggar du in som administratör. Använd kontrollpanelen till att konfigurera datorn att söka efter uppdateringar och installera de uppdateringar som krävs. Du kan behöva starta om servern.

### <a name="add-roles"></a>Lägga till roller

Lägg till roller för Active Directory Domain Services (AD DS) och DNS-server.

1. Starta PowerShell som administratör.

2. Skriv följande kommandon för att förbereda installationen av Windows Server Active Directory.

   ```PowerShell
   import-module ServerManager

   Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
   ```

### <a name="configure-registry-settings-for-sid-history-migration"></a>Konfigurera registerinställningar för migrering av SID-historik

Starta PowerShell och skriv följande kommandon för att konfigurera källdomänen att tillåta RPC-åtkomst (Remote Procedure Call) till databasen för hanteraren för kontosäkerhet (SAM).

```PowerShell
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## <a name="create-a-new-privileged-access-management-forest"></a>Skapa en ny skog för Privileged Access Management

Uppgradera sedan servern till domänkontrollant för en ny skog.

I det här dokumentet används namnet priv.contoso.local som domännamn för den nya skogen.  Namnet på skogen är inte viktigt och behöver inte vara underordnat namnet på en befintlig skog i organisationen. Både domän- och NetBIOS-namnet för den nya skogen måste dock vara unika och skilja sig från andra domännamn i organisationen.  

### <a name="create-a-domain-and-forest"></a>Skapa en domän och en skog

1. Skapa den nya domänen genom att skriva följande kommandon i ett PowerShell-fönster.  När du gör det skapas också en DNS-delegering i den överordnade domän (contoso.local) som skapades i föregående steg.  Om du vill konfigurera DNS senare kan du utelämna parametrarna `CreateDNSDelegation -DNSDelegationCredential $ca`.

   ```PowerShell
   $ca= get-credential
   Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
   ```

2. När popup-meddelandet visas anger du autentiseringsuppgifterna för CORP-skogens administratör (t.ex. användarnamnet CONTOSO\\Administratör och motsvarande lösenord från steg 1).

3. En uppmaning visas i PowerShell-fönstret om att du ska ange ett administratörslösenord för säkert läge. Ange ett nytt lösenord två gånger. Varningsmeddelanden för DNS-delegering och kryptografiinställningar visas. Det är normalt.

När skogen har skapats startas servern om automatiskt.

### <a name="create-user-and-service-accounts"></a>Skapa användar- och tjänstkonton

Skapa användar- och tjänstkonton för konfiguration av MIM-tjänsten och -portalen. Dessa konton hamnar i containern Användare i domänen priv.contoso.local.

1. När servern har startats om loggar du in på PRIVDC som domänadministratör (PRIV\\Administrator).

2. Starta PowerShell och skriv följande kommandon. Lösenordet 'Pass@word1' är bara ett exempel och du bör använda ett annat lösenord för konton.

   ```PowerShell
   import-module activedirectory

   $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   New-ADUser –SamAccountName MIMMA –name MIMMA

   Set-ADAccountPassword –identity MIMMA –NewPassword $sp

   Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

   Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

   Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

   Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

   Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMSync –name MIMSync

   Set-ADAccountPassword –identity MIMSync –NewPassword $sp

   Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMService –name MIMService

   Set-ADAccountPassword –identity MIMService –NewPassword $sp

   Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SharePoint –name SharePoint

   Set-ADAccountPassword –identity SharePoint –NewPassword $sp

   Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SqlServer –name SqlServer

   Set-ADAccountPassword –identity SqlServer –NewPassword $sp

   Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

   Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

   Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

   New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

   Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

   Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

   Add-ADGroupMember "Domain Admins" SharePoint

   Add-ADGroupMember "Domain Admins" MIMService
   ```

### <a name="configure-auditing-and-logon-rights"></a>Konfigurera rättigheter för granskning och inloggning

Du måste ställa in granskning för att PAM-konfigurationen ska upprättas mellan skogar.

1. Kontrollera att du är inloggad som domänadministratör (PRIV\\Administrator).

2. Gå till **Start** > **administrations verktyg** > **Grupprincip hantering**.

3. Navigera till **skog: Föreg. contoso. local** > **Domains** > Domains **. contoso. lokala** > **Domain Controllers** > domänkontrollanter är**standard domän**kontrollers princip för domänkontrollanter. En varning visas.

4. Högerklicka på **Standardprincip för domänkontrollanter** och välj **Redigera**.

5. I redigeraren Grupprinciphantering konsol trädet navigerar du till **dator konfiguration** > **principer** > **Windows-inställningar** > **säkerhets inställningar** > **lokala principer** > **gransknings princip**.

6. Högerklicka på **Granska kontohantering** i informationsfönstret och välj **Egenskaper**. Klicka på **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade**, klicka på **Tillämpa** och på **OK**.

7. Högerklicka på **Granska katalogtjänståtkomst** i informationsfönstret och välj **Egenskaper**. Klicka på **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade**, klicka på **Tillämpa** och på **OK**.

8. Gå till **dator konfiguration** > **principer** > **Windows-inställningar** > **säkerhets inställningar** > **konto principer** > **Kerberos-princip**.

9. I informationsfönstret högerklickar du på **Högsta livstid för användarbiljett** och väljer **Egenskaper**. Klicka på **Definiera följande principinställningar** ange antalet timmar till *1*, klicka på **Tillämpa** och på **OK**. Obs! Andra inställningar i fönstret ändras också.

10. I fönstret Grupprinciphantering väljer du på **Standarddomänprincip** och högerklickar och väljer **Redigera**.

11. Expandera **dator konfiguration** > **principer** > **Windows-inställningar** > **säkerhets inställningar** > **lokala principer** och välj **tilldelning av användar rättigheter**.

12. Högerklicka på **Neka inloggning som batchjobb** i informationsfönstret och välj **Egenskaper**.

13. Markera kryssrutan **Definiera följande principinställningar** och klicka på **Lägg till användare eller grupp**. I fältet Användar- och gruppnamn skriver du *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* och klickar på **OK**.

14. Stäng fönstret genom att klicka på **OK**.

15. Högerklicka på **Neka inloggning via Fjärrskrivbordstjänster** i informationsfönstret och välj **Egenskaper**.

16. Klicka på kryssrutan **Definiera följande principinställningar** och klicka på **Lägg till användare eller grupp**. I fältet Användar- och gruppnamn skriver du *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* och klickar på **OK**.

17. Stäng fönstret genom att klicka på **OK**.

18. Stäng fönstren Redigeraren Grupprinciphantering och Grupprinciphantering.

19. Öppna ett PowerShell-fönster som administratör och skriv följande kommando för att uppdatera domänkontrollanten i inställningarna för grupprincipen.

    ```cmd
    gpupdate /force /target:computer
    ```

    Efter en minut slutförs uppdateringen med meddelandet ”Uppdatering av grupprincip har slutförts”.


### <a name="configure-dns-name-forwarding-on-privdc"></a>Konfigurera vidarebefordran av DNS-namn på PRIVDC

Med PowerShell på PRIVDC konfigurerar du vidarebefordran av DNS-namn så att PRIV-domänen kan upptäcka andra befintliga skogar.

1. Starta PowerShell.

2. För varje domän som ligger högst upp i varje befintlig skog skriver du följande kommando, anger den befintliga DNS-domänen (t.ex. contoso.local) och IP-adressen för domänens huvudserver.  

   Om du har skapat en contoso.local-domän i det föregående steget, anger du *10.1.1.31* som IP-adress för CORPDC-datorns virtuella nätverk.

   ```PowerShell
   Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
   ```

> [!NOTE]
> De andra skogarna måste också kunna vidarebefordra DNS-frågor för PRIV-skogen till den här domänkontrollanten.  Om du har flera befintliga Active Directory-skogar måste du också lägga till en villkorlig DNS-vidarebefordrare för var och en av dessa skogar.

### <a name="configure-kerberos"></a>Konfigurera Kerberos

1. Med PowerShell lägger du till SPN-namn så att SharePoint, PAM REST-API:t och MIM-tjänsten kan använda Kerberos-autentisering.

   ```cmd
   setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
   setspn -S http/pamsrv PRIV\SharePoint
   setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
   setspn -S FIMService/pamsrv PRIV\MIMService
   ```

> [!NOTE]
> I nästa steg i det här dokumentet beskrivs hur du installerar MIM 2016 serverkomponenter på en enskild dator. Om du vill lägga till en annan server för hög tillgänglighet behöver du ytterligare Kerberos-konfigurationen enligt beskrivningen i [FIM 2010: Konfiguration av Kerberos-autentisering](https://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### <a name="configure-delegation-to-give-mim-service-accounts-access"></a>Konfigurera tilldelning för att ge åtkomst till MIM-tjänstkonton

Utför följande steg på PRIVDC som domänadministratör.

1. Starta **Active Directory – användare och datorer**.
2. Högerklicka på domänen **priv.contoso.local** och välj **Delegera kontroll**.
3. På fliken Valda användare och grupper klickar du på **Lägg till**.
4. I fönstret Välj användare, datorer eller grupper skriver du *mimcomponent; mimmonitor; mimservice* och klickar på **Kontrollera namn**. När namnen är understrukna klickar du på **OK** och sedan på **Nästa**.
5. I listan med vanliga aktiviteter väljer du **Skapa, ta bort och hantera användarkonton** och **Ändra medlemskap för en grupp** och klickar på **Nästa** och **Slutför**.

6. Högerklicka på domänen **priv.contoso.local** och välj **Delegera kontroll**.
7. På fliken Valda användare och grupper klickar du på **Lägg till**.  
8. I fönstret Välj användare, datorer eller grupper anger du *MIMAdmin* och klickar på **Kontrollera namn**. När namnen är understrukna klickar du på **OK** och sedan på **Nästa**.
9. Välj **anpassad aktivitet**, tillämpa på **den här mappen** med **allmänna behörigheter**.
10. I listan med behörigheter väljer du följande:
    - **Läsa**
    - **Skriva**
    - **Skapa alla underordnade objekt**
    - **Ta bort alla underordnade objekt**
    - **Läsa alla egenskaper**
    - **Skriva alla egenskaper**
    - **MIGRERA sid-historik** Klicka på **Nästa** och sedan på **Slutför**.

11. Högerklicka på domänen **priv.contoso.local** och välj **Delegera kontroll**.  
12. På fliken Valda användare och grupper klickar du på **Lägg till**.  
13. I fönstret Välj användare, datorer eller grupper anger du *MIMAdmin* och klickar på **Kontrollera namn**. När namnen är understrukna klickar du på **OK** och sedan på **Nästa**.  
14. Välj **anpassad aktivitet**, tillämpa på **den här mappen** och klicka på **endast användarobjekt**.    
15. I listan med behörigheter väljer du **Ändra lösenord** och **Återställ lösenord**. Klicka sedan på **Nästa** och på **Slutför**.  
16. Stäng Active Directory – användare och datorer.

17. Öppna en kommandotolk.  
18. Granska åtkomstkontrollistan i objektet AdminSDHolder i PRIV-domänerna. Om domänen till exempel är "priv.contoso.local" skriver du kommandot
    ```cmd
    dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
    ```
19. Uppdatera åtkomstkontrollistan vid behov för att säkerställa att MIM-tjänsten och MIM-komponenttjänsten kan uppdatera medlemskap i grupper som skyddas av denna ACL.  Skriv kommandot:

```cmd
dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"
dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
```

20. Starta om PRIVDC-servern så att ändringarna träder ikraft.

## <a name="prepare-a-priv-workstation"></a>Förbereda en PRIV arbetsstation

Om du inte redan har en arbetsstationsdator som ska anslutas till PRIV-domänen för att utföra underhåll av PRIV-resurser (som MIM), följer du de här anvisningarna för hur du förbereder en arbetsstation.  

### <a name="install-windows-81-or-windows-10-enterprise"></a>Installera Windows 8.1 eller Windows 10 Enterprise

På en annan virtuell dator utan installerad programvara installerar du Windows 8.1 Enterprise eller Windows 10 Enterprise om du vill göra en dator till *PRIVWKSTN*.

1. Använd standardinställningar under installationen.

2. Observera att installationen kanske inte kan ansluta till internet. Klicka på **Skapa ett lokalt konto**. Ange ett annat användarnamn. Använd inte "Administratör" eller "Lisa".

3. Använd kontrollpanelen till att ge datorn en statisk IP-adress på det virtuella nätverket och ange PRIVDC-servern som gränssnittets prioriterade DNS-server.

4. Använd kontrollpanelen och domänanslut PRIVWKSTN-datorn till domänen priv.contoso.local. För det måste du tillhandahålla autentiseringsuppgifterna före PRIV-domänens administratör. När det är klart startar du om datorn PRIVWKSTN.

Mer information finns i [Skydda arbetsstationer med privilegierad åtkomst](https://technet.microsoft.com/library/mt634654.aspx).

I nästa steg ska du förbereda en PAM-server.

> [!div class="step-by-step"]
> [«Steg 1](step-1-prepare-corp-domain.md)
> [steg 3»](step-3-prepare-pam-server.md)
