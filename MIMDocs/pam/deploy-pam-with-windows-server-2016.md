---
title: Distribuera MIM Privileged Access Management med Windows Server 2016 | Microsoft Docs
description: "Lär dig mer om hur du distribuerar Privileged Access Management med server 2016"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/08/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: fbdebd59249667a0e60d3a248f183bcb6a75085a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Distribuera MIM PAM med Windows Server 2016
<a id="deploy-mim-pam-with-windows-server-2016" class="xliff"></a>


I det här scenariot kan MIM 2016 SP1 använda funktioner i Windows Server 2016 som domänkontrollant ”PRIV”-skogen.  När scenariot har konfigurerats tidsbegränsas en användares Kerberos-biljett till den återstående tiden för användarens rollaktivering. 

>[!Note]
Tidigare tekniska förhandsvisningar av Windows Server 2016 före teknisk förhandsvisning 5 inte kan användas med den här versionen av MIM.

## Förberedelse
<a id="preparation" class="xliff"></a>

Det krävs minst två virtuella datorer för laboratoriemiljön:

-   En virtuell dator är värd för PRIV-domänkontrollanten som kör Windows Server 2016

-   En virtuell dator är värd för MIM-tjänsten som kör Windows Server 2016 (rekommenderas) eller Windows Server 2012 R2

>[!NOTE]
Om du inte redan har en ”CORP”-domän i laboratoriemiljön krävs ytterligare en domänkontrollant i domänen. ”CORP”-domänkontrollanten kan antingen köra Windows Server 2016 eller Windows Server 2012 R2.


Genomför installationen enligt beskrivningen i guiden [för att komma igång](privileged-identity-management-for-active-directory-domain-services.md), **med undantag av vad som anges nedan**:

-   Om du skapar en ny domän i CORP när du följer instruktionerna i [Steg 1 - Förbereda CORP-domänkontrollanten](step-1-prepare-corp-domain.md) kan du också konfigurera CORP- domänens funktionsnivå för Windows Server 2016. **Om du väljer det här alternativet kan du göra följande justeringar**:

    -   Om du använder Windows Server 2016 media kallas installationsalternativet Windows Server 2016 (Server med skrivbordsmiljö).

    -   Du kan ange funktionsnivån för Windows Server 2016 för CORP-skogen och -domänen genom att ange 7 som versionsnummer för domänen och skogen i argumentet för kommandot Install-ADDSForest enligt följande:
     ```
        Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
        ```
    -   I ”Skapa nya användare och grupper” är det slutliga kommandot (nytt ADGroup-namnet ”CONTOSO\$\$\$'...) **inte nödvändigt när både CORP- och PRIV-domänkontrollanternas funktionsnivåer är konfigurerade för Windows Server 2016**.

    -   Ändringarna som beskrivs i ”Konfigurera granskning”(objekt #8) och ”Konfigurera registerinställningar” (objekt #10) **rekommenderas men är inte obligatoriska** när både CORP- och PRIV-domänkontrollanternas funktionsnivåer är konfigurerade för Windows Server 2016.

-   Om du väljer att använda Windows Server 2012 R2 som operativsystem för CORPDC, måste du installera snabbkorrigeringar 2919442, 2919355, [och uppdatera 3155495](http://support.microsoft.com/kb/3156418) på CORPDC.

-   Följ instruktionerna i [Steg 2 - Förbereda PRIV-domänkontrollant](step-2-prepare-priv-domain-controller.md) med undantag av dessa justeringar:

    -   Installera med hjälp av Windows Server 2016 media. Installationsalternativet kallas Windows Server 2016 (Server med skrivbordsmiljö).

    -   I anvisningarna ”Lägg till roller” (objekt #4) **måste du ange 7 som versionsnummer för domänen och skogen i den fjärde raden i PowerShell-kommandon** för att tillåta att nedanstående Windows Server AD-funktioner är aktiverade.

        ```
        Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
        ```  

    -   När du konfigurerar gransknings- och inloggningsrättigheter bör du notera att programmet för grupprinciphantering finns i mappen Administrationsverktyg för Windows.

    -   Du behöver inte konfigurera registerinställningarna som behövs för att flytta SID-historiken (objekt #8) **när PRIV-domänens funktionsnivå är konfigurerad för Windows Server 2016**.

    -   När du har konfigurerat delegering och innan du startar om servern ska du aktivera funktionen privilegierad åtkomsthantering i Windows Server 2016 Active Directory genom att starta ett PowerShell-fönster som administratör och sedan skriva följande kommandon.

    ```
    $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
    Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
    ```

  -   När du har konfigurerat delegering och innan du startar om servern bör du ge behörighet till MIM-administratörer och MIM-tjänstkontot för att skapa och uppdatera skuggprinciper.

     a. Starta powershell-fönstret och Skriv ADSIEdit.

     b. I menyn Åtgärder klickar du på ”Anslut till”. Ändra namngivningskontexten från ”Standardbaserad namngivningskontext” till ”Configuration” i inställningen Anslutning och klicka på OK.

     c. Expandera noden Configuration att se när du har anslutit till vänster i fönstret nedanför ”ADSI-redigering” för att visa ”CN = Configuration, DC = priv,...”. Expandera CN = Configuration och expandera sedan CN = Services.

     d. Högerklicka på ”CN = Shadow Principal Configuration” och klicka på Egenskaper. Gå till säkerhetsfliken när dialogrutan Egenskaper visas.

     e. Klicka på Lägg till.... Ange kontona ”MIMService”, samt andra MIM-administratörer som senare kommer att utföra New-PAMGroup för att skapa ytterligare PAM-grupper. För varje användare i behörighetslistan över tillåtna lägger du till ”Skriva”, ”Skapa alla underordnade objekt” och ”Ta bort alla underordnade objekt”. Lägg till behörigheterna.

     f. Ändra till avancerade säkerhetsinställningar. Klicka på Redigera på den rad som tillåter åtkomst till MIMService. Ändra inställningen ”Gäller” till ”För det här objektet och alla underordnade objekt”. Uppdatera behörighetsinställningen och stäng dialogrutan säkerhet.

     g. Stäng ADSI-redigering.

 -   När du har konfigurerat delegering och innan du startar om servern bör du ge behörighet till MIM-administratörer för att skapa och uppdatera behörighetsprinciper.

     a.  Starta en upphöjd **kommandotolk**, skriv följande kommandon och ersätt namnet på ditt MIM-administratörskonto med ”mimadmin” på var och en av de fyra raderna:
    ```
       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicy

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


-   Följ instruktionerna i [Steg 3 - Förbered en PAM-server](step-3-prepare-pam-server.md) med dessa ändringar.

    -   Om du installerar på Windows Server 2016 är rollen ”ApplicationServer” inte tillgänglig.

    -   Om du installerar MIM på Windows Server 2016 **går det inte att installera SharePoint 2013**.

-   Följ instruktionerna i [Steg 4 – Installera MIM-komponenter på PAM-server och -arbetsstation](step-4-install-mim-components-on-pam-server.md) med dessa ändringar.

    -   Användaren som installerar MIM-tjänsten och PAM-komponenter **måste ha skrivbehörighet till PRIV-domänen i AD** eftersom MIM-installationen skapar en ny AD-organisationsenhet: ”PAM objects”.

    -   Installera inte MIM-portalen om SharePoint inte har installerats.

-   Följ instruktionerna i [Steg 5 - Upprätta förtroende](step-5-establish-trust-between-priv-corp-forests.md) med dessa ändringar:

    -   När du etablerar envägsförtroenden ska du endast utföra de första två PowerShell-kommandona (get-credential och New-PAMTrust) **och inte kommandot New-PAMDomainConfiguration**.

    -   När du har etablerat ett förtroende ska du logga in på PRIVDC som PRIV\\-administratör, starta PowerShell och ange följande kommandon:
  ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1

     netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
     /usero:contoso\\administrator /passwordo:Pass\@word1  

     netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1
  ```

-   Objekt #5 (verifiering av förtroende) **behövs inte när både CORP- och PRIV-domänernas funktionsnivå är konfigurerade för Windows Server 2016**.

## Mer information
<a id="more-information" class="xliff"></a>

- [Privileged Access Management för Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md)
- [Konfigurera MIM-miljö för Privileged Access Management](configuring-mim-environment-for-pam.md)
- [Konfigurera PAM med hjälp av skript](sp1-pam-configure-using-scripts.md)
