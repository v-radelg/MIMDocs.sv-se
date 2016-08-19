---
title: "Distribuera PAM steg 7 – användaråtkomst | Microsoft Identity Manager"
description: "Som ett sista steg kan du ge en privilegierad användare tillfällig åtkomst för att se om distributionen av Privileged Access Management lyckades."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9b5b7460e6307ab38b1b9356a638eb0200fd97d1
ms.openlocfilehash: 009091a65dba31de2066e45930e438442fcd89a0


---

# Steg 7 – Utöka en användares behörighet

>[!div class="step-by-step"]
[« Steg 6 ](step-6-transition-group-to-pam.md)


I det här steget kontrollerar du att en användare kan begära åtkomst till en roll via MIM.

## Kontrollera att Lisa inte kan komma åt den privilegierade resursen
Utan utökad behörighet kan Lisa inte komma åt den privilegierade resursen i CORP-skogen.

1. Logga ut från CORPWKSTN för att ta bort eventuella cachelagrade öppna anslutningar.
2. Logga in på CORPWKSTN som *CONTOSO\Lisa* och växla till **skrivbordsvyn**.
3. Öppna en DOS-kommandotolk.
4. Skriv kommandot `dir \\corpwkstn\corpfs`. Felmeddelandet **Åtkomst nekas** bör visas.
5. Lämna kommandotolken öppen.

## Begär privilegierad åtkomst från MIM.
1. Skriv följande kommando på CORPWKSTN, fortfarande som CONTOSO\Lisa.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. När du uppmanas anger du lösenordet för kontot PRIV.Lisa. Ett nytt kommandotolksfönster visas.
3. Skriv följande kommandon när PowerShell-fönstret visas.

    > [!NOTE]
    > Alla steg som följer efter att du har kört kommandona måste utföras inom en viss tid.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. När det är klart stänger du PowerShell-fönstret.
5. I DOS-kommandofönstret skriver du följande kommando

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Ange lösenordet för kontot PRIV.Lisa. Ett nytt kommandotolksfönster visas.

## Validera den utökade behörigheten.
I fönstret som öppnats skriver du följande kommandon.

```
whoami /groups
dir \\corpwkstn\corpfs
```

Om kommandot dir misslyckas med felmeddelandet **Åtkomst nekas**, kontrollerar du förtroenderelationen igen.

## Aktivera den privilegierade rollen
Aktivera på begäran privilegierad åtkomst via PAM-exempelportalen.

1. Kontrollera att du är inloggad som CORP\Lisa på CORPWKSTN.
2. Skriv följande kommando i DOS-kommandofönstret.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. När du uppmanas anger du lösenordet för kontot PRIV.Lisa. Ett nytt webbläsarfönster visas.
4. Gå till http://pamsrv.priv.contoso.local:8090 och kontrollera att en webbsida från exempelportalen visas.
5. I Internet Explorer väljer du **Verktyg** > **Internetalternativ** och klicka på fliken **Säkerhet**.
6. Klicka på **Zonen Lokalt nätverk** > **Platser** > **Avancerat** och lägg till webbplatsen i zonen.
7. Stäng dialogrutan **Internetalternativ**.
8. På den vänstra fliken klickar du på **Aktivera**. Välj **PAM-roll** och klicka på **Aktivera**.

> [!Note]
> I den här miljön kan du också lära dig hur du utvecklar program som använder PAM REST API, som beskrivs i [referensen för Privileged Access Management REST API](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## Sammanfattning
När du har slutfört stegen i den här genomgången kommer du att ha genomfört ett scenario för privilegierad åtkomsthantering. I det utökas användarens behörighet under en begränsad tid så att användaren kommer åt skyddade resurser med ett separat privilegierat konto. När den utökade sessionen upphör kommer det privilegierade kontot inte längre åt den skyddade resursen. PAM-administratören koordinerar beslutet om vilka säkerhetsgrupper som representerar privilegierade roller. När åtkomstbehörigheten har migrerats till Privileged Access Management-systemet, blir åtkomst som tidigare var möjlig med det ursprungliga användarkontot endast möjlig genom att logga in med ett särskilt privilegierat konto och görs tillgänglig på begäran. Gruppmedlemskap för mycket privilegierade grupper gäller därför bara under en begränsad tid.

>[!div class="step-by-step"]
[« Steg 6 ](step-6-transition-group-to-pam.md)



<!--HONumber=Jul16_HO4-->


