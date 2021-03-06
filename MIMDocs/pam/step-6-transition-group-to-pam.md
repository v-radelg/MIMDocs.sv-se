---
title: Distribuera PAM steg 6 – Flytta grupp | Microsoft Docs
description: Migrera en grupp i PRIV-skogen så att den kan hanteras med Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e88407ceb1c7ac99f1746f453b7e4a7a5d296e5a
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043638"
---
# <a name="step-6--transition-a-group-to-privileged-access-management"></a>Steg 6 – Överföra en grupp till Privileged Access Management

> [!div class="step-by-step"]
> [«Steg 5 ](step-5-establish-trust-between-priv-corp-forests.md) 
>  [steg 7»](step-7-elevate-user-access.md)

Privilegierade konto skapas i PRIV-skogen med PowerShell-cmdletar. Dessa cmdletar utför följande funktioner:

- Skapa en ny grupp i PRIV-skogen med samma säkerhetsidentifierare (SID) som en grupp i CORP-skogen.  
- Skapa ett objekt i MIM-tjänstens databas som motsvarar gruppen i PRIV-skogen.  
- För varje användarkonto skapar du två objekt i MIM-tjänstens databas ett för användaren i CORP-skogen och ett för det nya användarkontot i PRIV-skogen.  
- Skapa ett PAM-rollobjekt i MIM-tjänstens databas.  

Du måste köra cmdletarna en gång för varje grupp och en gång för varje medlem i en grupp. Migrerings-cmdletarna ändrar inga användare eller grupper i CORP-skogen: PAM-administratören gör det manuellt senare.

1. Logga in på PAMSRV, antingen direkt eller från en PRIV-arbetsstation, som *PRIV\MIMAdmin*.

2.  Starta PowerShell och skriv följande kommandon.

```PowerShell
   Import-Module MIMPAM
   Import-Module ActiveDirectory
```

3. Skapa ett motsvarande användarkonto i PRIV för ett användarkonto i en befintlig skog i exempelsyfte.

   Skriv följande kommandon i PowerShell.  Om du inte använde namnet *Lisa* när du skapade användaren contoso.local tidigare, ändrar du parametrarna för kommandot efter behov. Lösenordet 'Pass@word1' är bara ett exempel och bör ändras till ett unikt lösenord.

   ```PowerShell
       $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
       $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
       Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
       Set-ADUser –identity priv.Jen –Enabled 1
   ```

4. Kopiera en grupp och dess medlem, Lisa, från CONTOSO till PRIV-domänen, i exempelsyfte.

    Kör följande kommandon och ange lösenordet för CORP-domänens administratör (CONTOSO\Administratör) när du uppmanas till det:

   ```PowerShell
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
   ```

    Kommandot **New-PAMGroup** innehåller följande parametrar:

     -   CORP-skogens domän namn i NetBIOS-form  
     -   Namnet på den grupp som ska kopieras från domänen  
     -   CORP-skogens domän domänkontrollant NetBIOS-namn  
     -   Autentiseringsuppgifterna för en domän administratörs användare i CORP-skogen  

5. (Valfritt) Ta bort Lisas konto på CORPDC från gruppen **CONTOSO CorpAdmins** om det fortfarande finns kvar.  Det här krävs bara för att visa hur behörigheter kan associeras med konton som skapats i PRIV-skogen.

   1.  Logga in på CORPDC som *CONTOSO\Administratör*.

   2.  Starta PowerShell, kör följande kommando och bekräfta ändringen.

       ```PowerShell
       Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
       ```


Om du vill visa att åtkomstbehörigheter mellan skogar gäller för användarens administratörskonto fortsätter du till nästa steg.

> [!div class="step-by-step"]
> [«Steg 5 ](step-5-establish-trust-between-priv-corp-forests.md) 
>  [steg 7»](step-7-elevate-user-access.md)
