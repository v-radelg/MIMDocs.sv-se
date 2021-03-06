---
title: Distribuera PAM steg 5 – Forest Link | Microsoft Docs
description: Upprätta förtroende mellan skogarna PRIV och CORP så att privilegierade användare i PRIV fortfarande kan komma åt resurser i CORP.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/29/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0cf952c93c0a7b95fd41939efc767e9e8c20be5e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043655"
---
# <a name="step-5--establish-trust-between-priv-and-corp-forests"></a>Steg 5 – upprätta förtroende mellan PRIV- och CORP-skogar

> [!div class="step-by-step"]
> [«Steg 4](step-4-install-mim-components-on-pam-server.md)
> [steg 6»](step-6-transition-group-to-pam.md)

För varje CORP-domän, till exempel contoso.local, måste domänkontrollanterna PRIV och CONTOSO vara bundna av ett förtroende. Det gör att användarna i PRIV-domänen kan komma åt resurser i CORP-domänen.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Koppla domänkontrollanterna till varandra

Innan du upprättar förtroende måste domänkontrollanterna konfigureras för DNS-namnmatchning för motparten, utifrån på den andra domänkontrollantens/DNS-serverns IP-adress.

1.  Om domänkontrollanterna eller servern med MIM-programvaran distribueras som virtuella datorer ska du se till att det inte finns några andra DNS-servrar som tillhandahåller dem med DNS-tjänster.
    - Om de virtuella datorerna har flera nätverksgränssnitt, inklusive nätverksgränssnitt som är anslutna till offentliga nätverk, kan du behöva inaktivera anslutningarna eller åsidosätta inställningarna för Windows-nätverksgränssnitt tillfälligt. Det är viktigt att du ser till att inga DHCP-angivna DNS-serveradresser används av några virtuella datorer.

2.  Kontrollera att alla befintliga CORP-domänkontrollanter kan vidarebefordra namn till PRIV-skogen. Starta PowerShell på varje domänkontrollant utanför PRIV-skogen, till exempel CORPDC, och skriv följande kommando:

    ```cmd
    nslookup -qt=ns priv.contoso.local.
    ```
    Kontrollera att utdata visar en namnserverpost för PRIV-domänen med rätt IP-adress.

3.  Om domänkontrollanten inte kan vidarebefordra till PRIV-domänen använder du **DNS-hanteraren** (finns under **Start** > **Programverktyg** > **DNS**) till att konfigurera vidarebefordran av DNS-namn för PRIV-domänen till IP-adressen för PRIVDC. Om det är en överordnad domän (t. ex. contoso. local) expanderar du noderna för den här domänkontrollanten och dess domän, till exempel **CORPDCs** > **Forward lookup Zones** > **contoso. local**och se till att en nyckel med namnet **föreg** finns som namnserver typ (namnserver).

    ![filstruktur för priv-nyckeln – skärmbild](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Upprätta förtroende på PAMSRV

Upprätta enkelriktat förtroende med varje domän på PAMSRV, till exempel CORPDC, så att CORP-domänkontrollanterna har förtroende för PRIV-skogen.

1. Logga in på PAMSRV som PRIV-domänadministratör (PRIV\Administratör).

2.  Starta PowerShell.

3.  Skriv följande PowerShell-kommandon för varje befintlig skog. Ange autentiseringsuppgifter för CORP-domänadministratören (CONTOSO\Administratör) när du uppmanas till det.

    ```PowerShell
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Skriv följande PowerShell-kommandon för varje domän i de befintliga skogarna. Ange autentiseringsuppgifter för CORP-domänadministratören (CONTOSO\Administratör) när du uppmanas till det.

    ```PowerShell
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Ge skogar läsbehörighet till Active Directory

Aktivera läsbehörighet till AD för PRIV-administratörer och övervakningstjänsten för varje befintlig skog.

1. Logga in på CORP-skogens befintliga domänkontrollant (CORPDC) som domänadministratör på domänen på översta nivå i skogen (Contoso\Administratör).  
2. Starta **Active Directory – användare och datorer**.  
3. Högerklicka på domänen **contoso.local** och välj **Delegera kontroll**.  
4. På fliken Valda användare och grupper klickar du på **Lägg till**.  
5. I fönstret Välj användare, datorer eller grupper klickar du på **Platser** och ändrar platsen till *priv.contoso.local*.  På objektnamnet skriver du *Domänadministratörer* och klickar på **Kontrollera namn**. När ett popup-fönster visas anger du användarnamnet *priv\administratör* och lösenordet.  
6. Efter Domänadministratörer lägger du till "*; MIMMonitor*". När namnen **Domänadministratörer** och **MIMMonitor** är understrukna klickar du på **OK** och sedan på **Nästa**.  
7. I listan med vanliga uppgifter väljer du **Läsa all användarinformation** och klickar på **Nästa** och **Slutför**.  
8. Stäng Active Directory – användare och datorer.

9. Öppna ett PowerShell-fönster.
10. Se till att SID-historik är aktiverat och SID-filtrering är inaktiverat med hjälp av `netdom`. Ange:
    ```cmd
    netdom trust contoso.local /quarantine:no /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    Resultatet bör antingen ge **SID-historik aktiveras för förtroendet** eller **SID-historik har redan aktiverats för förtroendet**.

    Resultatet bör också ange **SID-filtrering har inte aktiverats för förtroendet**. Mer information finns i [Inaktivera SID-filterkarantän](https://technet.microsoft.com/library/cc772816.aspx).

## <a name="start-the-monitoring-and-component-services"></a>Starta övervaknings- och komponenttjänsterna

1.  Logga in på PAMSRV som PRIV-domänadministratör (PRIV\Administratör).

2.  Starta PowerShell.

3.  Skriv följande PowerShell-kommandon.

    ```cmd
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

I nästa steg flyttar du en grupp till PAM.

> [!div class="step-by-step"]
> [«Steg 4](step-4-install-mim-components-on-pam-server.md)
> [steg 6»](step-6-transition-group-to-pam.md)
