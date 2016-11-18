---
title: "Distribuera PAM steg 5 – Forest Link | Microsoft Docs"
description: "Upprätta förtroende mellan skogarna PRIV och CORP så att privilegierade användare i PRIV fortfarande kan komma åt resurser i CORP."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 16208efe08c5a2c0f63ee121c64c45cad5a73909


---

# <a name="step-5-establish-trust-between-priv-and-corp-forests"></a>Steg 5 – upprätta förtroende mellan PRIV- och CORP-skogar

>[!div class="step-by-step"]
[« Steg 4](step-4-install-mim-components-on-pam-server.md)
[Steg 6 »](step-6-transition-group-to-pam.md)


För varje CORP-domän, till exempel contoso.local, måste domänkontrollanterna PRIV och CONTOSO vara bundna av ett förtroende. Det gör att användarna i PRIV-domänen kan komma åt resurser i CORP-domänen.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Koppla domänkontrollanterna till varandra

Innan du upprättar förtroende måste domänkontrollanterna konfigureras för DNS-namnmatchning för motparten, utifrån på den andra domänkontrollantens/DNS-serverns IP-adress.

1.  Om domänkontrollanterna eller servern med MIM-programvaran distribueras som virtuella datorer ska du se till att det inte finns några andra DNS-servrar som tillhandahåller dem med DNS-tjänster.
    - Om de virtuella datorerna har flera nätverksgränssnitt, inklusive nätverksgränssnitt som är anslutna till offentliga nätverk, kan du behöva inaktivera anslutningarna eller åsidosätta inställningarna för Windows-nätverksgränssnitt tillfälligt. Det är viktigt att du ser till att inga DHCP-angivna DNS-serveradresser används av några virtuella datorer.

2.  Kontrollera att alla befintliga CORP-domänkontrollanter kan vidarebefordra namn till PRIV-skogen. Starta PowerShell på varje domänkontrollant utanför PRIV-skogen, till exempel CORPDC, och skriv följande kommando:

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    Kontrollera att utdata visar en namnserverpost för PRIV-domänen med rätt IP-adress.

3.  Om domänkontrollanten inte kan vidarebefordra till PRIV-domänen använder du **DNS-hanteraren** (finns under **Start** > **Programverktyg** > **DNS**) till att konfigurera vidarebefordran av DNS-namn för PRIV-domänen till IP-adressen för PRIVDC. Om den är en överordnad domän (t.ex. contoso.local) expanderar du noderna för domänkontrollanten och dess domän, till exempel **CORPDC** > **Zoner för vanlig sökning** > **contoso.local** och ser till att det finns en nyckel med namnet **priv** som namnservertyp (NS).

    ![filstruktur för priv-nyckeln – skärmbild](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Upprätta förtroende på PAMSRV

Upprätta enkelriktat förtroende med varje domän på PAMSRV, till exempel CORPDC, så att CORP-domänkontrollanterna har förtroende för PRIV-skogen.

1. Logga in på PAMSRV som PRIV-domänadministratör (PRIV\Administratör).

2.  Starta PowerShell.

3.  Skriv följande PowerShell-kommandon för varje befintlig skog. Ange autentiseringsuppgifter för CORP-domänadministratören (CONTOSO\Administratör) när du uppmanas till det.

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Skriv följande PowerShell-kommandon för varje domän i de befintliga skogarna. Ange autentiseringsuppgifter för CORP-domänadministratören (CONTOSO\Administratör) när du uppmanas till det.

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Ge skogar läsbehörighet till Active Directory

Aktivera läsbehörighet till AD för PRIV-administratörer och övervakningstjänsten för varje befintlig skog.

1.  Logga in på CORP-skogens befintliga domänkontrollant (CORPDC) som domänadministratör på domänen på översta nivå i skogen (Contoso\Administratör).  
2.  Starta **Active Directory – användare och datorer**.  
3.  Högerklicka på domänen **contoso.local** och välj **Delegera kontroll**.  
4.  På fliken Valda användare och grupper klickar du på **Lägg till**.  
5.  I fönstret Välj användare, datorer eller grupper klickar du på **Platser** och ändrar platsen till *priv.contoso.local*.  På objektnamnet skriver du *Domänadministratörer* och klickar på **Kontrollera namn**. När ett popup-fönster visas anger du användarnamnet *priv\administratör* och lösenordet.  
6.  Efter Domänadministratörer lägger du till "*; MIMMonitor*". När namnen **Domänadministratörer** och **MIMMonitor** är understrukna klickar du på **OK** och sedan på **Nästa**.  
7.  I listan med vanliga uppgifter väljer du **Läsa all användarinformation** och klickar på **Nästa** och **Slutför**.  
8.  Stäng Active Directory – användare och datorer.

9.  Öppna ett PowerShell-fönster.  
10.  Se till att SID-historik är aktiverat och SID-filtrering är inaktiverat med hjälp av `netdom`. Typ:  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    Resultatet bör antingen ge **SID-historik aktiveras för förtroendet** eller **SID-historik har redan aktiverats för förtroendet**.

    Resultatet bör också ange **SID-filtrering har inte aktiverats för förtroendet**. Mer information finns i [Inaktivera SID-filterkarantän](http://technet.microsoft.com/library/cc772816.aspx).

## <a name="start-the-monitoring-and-component-services"></a>Starta övervaknings- och komponenttjänsterna

1.  Logga in på PAMSRV som PRIV-domänadministratör (PRIV\Administratör).

2.  Starta PowerShell.

3.  Skriv följande PowerShell-kommandon.

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

I nästa steg flyttar du en grupp till PAM.

>[!div class="step-by-step"]
[« Steg 4](step-4-install-mim-components-on-pam-server.md)
[Steg 6 »](step-6-transition-group-to-pam.md)



<!--HONumber=Nov16_HO2-->


