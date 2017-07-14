---
title: "Distribuera PAM steg 6 – Flytta grupp | Microsoft Docs"
description: "Migrera en grupp i PRIV-skogen så att den kan hanteras med Privileged Access Management."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: aeffca2c4e5467ec039c2077a88f36a652493e90
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---

# Steg 6 – Överföra en grupp till Privileged Access Management
<a id="step-6--transition-a-group-to-privileged-access-management" class="xliff"></a>

>[!div class="step-by-step"]
[«Steg 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Steg 7»](step-7-elevate-user-access.md)

Privilegierade konto skapas i PRIV-skogen med PowerShell-cmdletar. Dessa cmdletar utför följande funktioner:

- Skapa en ny grupp i PRIV-skogen med samma säkerhetsidentifierare (SID) som en grupp i CORP-skogen.  
- Skapa ett objekt i MIM-tjänstens databas som motsvarar gruppen i PRIV-skogen.  
- För varje användarkonto skapar du två objekt i MIM-tjänstens databas ett för användaren i CORP-skogen och ett för det nya användarkontot i PRIV-skogen.  
- Skapa ett PAM-rollobjekt i MIM-tjänstens databas.  

Du måste köra cmdletarna en gång för varje grupp och en gång för varje medlem i en grupp. Migrerings-cmdletarna ändrar inga användare eller grupper i CORP-skogen: PAM-administratören gör det manuellt senare.

1. Logga in på PAMSRV, antingen direkt eller från en PRIV-arbetsstation, som *PRIV\MIMAdmin*.

2.  Starta PowerShell och skriv följande kommandon.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Skapa ett motsvarande användarkonto i PRIV för ett användarkonto i en befintlig skog i exempelsyfte.

    Skriv följande kommandon i PowerShell.  Om du inte använde namnet *Lisa* när du skapade användaren contoso.local tidigare, ändrar du parametrarna för kommandot efter behov. Lösenordet 'Pass@word1' är bara ett exempel och bör ändras till ett unikt lösenord.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Kopiera en grupp och dess medlem, Lisa, från CONTOSO till PRIV-domänen, i exempelsyfte.

    Kör följande kommandon och ange lösenordet för CORP-domänens administratör (CONTOSO\Administratör) när du uppmanas till det:

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Kommandot **New-PAMGroup** innehåller följande parametrar:

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Valfritt) Ta bort Lisas konto på CORPDC från gruppen **CONTOSO CorpAdmins** om det fortfarande finns kvar.  Det här krävs bara för att visa hur behörigheter kan associeras med konton som skapats i PRIV-skogen.

    1.  Logga in på CORPDC som *CONTOSO\Administratör*.

    2.  Starta PowerShell, kör följande kommando och bekräfta ändringen.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Om du vill visa att åtkomstbehörigheter mellan skogar gäller för användarens administratörskonto fortsätter du till nästa steg.

>[!div class="step-by-step"]
[«Steg 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Steg 7»](step-7-elevate-user-access.md)

