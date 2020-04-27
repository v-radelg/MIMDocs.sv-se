---
title: Distribuera PAM steg 7 – användaråtkomst | Microsoft Docs
description: Som ett sista steg kan du ge en privilegierad användare tillfällig åtkomst för att se om distributionen av Privileged Access Management lyckades.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/17/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: 05e05966bf90700885e67ba16f10ab0d7864cf10
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043621"
---
# <a name="step-7--elevate-a-users-access"></a>Steg 7 – Utöka en användares behörighet

> [!div class="step-by-step"]
> [« Steg 6 ](step-6-transition-group-to-pam.md)


I det här steget kontrollerar du att en användare kan begära åtkomst till en roll via MIM.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Kontrollera att Lisa inte kan komma åt den privilegierade resursen

Utan utökad behörighet kan Lisa inte komma åt den privilegierade resursen i CORP-skogen.

1. Logga ut från CORPWKSTN för att ta bort eventuella cachelagrade öppna anslutningar.
2. Logga in på CORPWKSTN som *CONTOSO\Lisa* och växla till **skrivbordsvyn**.
3. Öppna en DOS-kommandotolk.
4. Skriv kommandot `dir \\corpwkstn\corpfs`. Felmeddelandet **Åtkomst nekas** bör visas.
5. Lämna kommandotolken öppen.

## <a name="request-privileged-access-from-mim"></a>Begär privilegierad åtkomst från MIM.

> [!NOTE]
> Vi rekommenderar att arbets stationen är en privilegie rad arbets Station (PAW).  Mer information finns i [Paw](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. På PRIVWKSTN loggar du in som PRIV\priv.jen.
2. Klicka på **Start**, **Kör**och ange **PowerShell. exe**.
3. Ange följande kommando.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. När du uppmanas anger du lösenordet för kontot PRIV.Lisa. Ett nytt kommandotolksfönster visas.
3. Skriv följande kommandon när PowerShell-fönstret visas.

    > [!NOTE]
    > Alla steg som följer efter att du har kört kommandona måste utföras inom en viss tid.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. När det är klart stänger du PowerShell-fönstret.
5. I DOS-kommandofönstret skriver du följande kommando

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Ange lösenordet för kontot PRIV.Lisa. Ett nytt kommandotolksfönster visas.

## <a name="validate-the-elevated-access"></a>Validera den utökade behörigheten.
I fönstret som öppnats skriver du följande kommandon.

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

Om kommandot dir misslyckas med felmeddelandet **Åtkomst nekas**, kontrollerar du förtroenderelationen igen.

## <a name="activate-the-privileged-role"></a>Aktivera den privilegierade rollen

Aktivera på begäran privilegierad åtkomst via PAM-exempelportalen.

1. Kontrollera att du är inloggad som CORP\Lisa på CORPWKSTN.
2. Skriv följande kommando i DOS-kommandofönstret.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. När du uppmanas anger du lösenordet för kontot PRIV.Lisa. Ett nytt webbläsarfönster visas.
4. Navigera till http://pamsrv.priv.contoso.local:8090 och kontrol lera att en webb sida från exempel portalen är synlig.
5. I Internet Explorer väljer du **verktyg** > **Internet alternativ** och klickar på fliken **säkerhet** .
6. Klicka på**platser** > i den **lokala intranät zonen** > **Avancerat** och Lägg sedan till webbplatsen i zonen.
7. Stäng dialogrutan **Internetalternativ**.
8. På den vänstra fliken klickar du på **Aktivera**. Välj **PAM-roll** och klicka på **Aktivera**.

> [!Note]
> I den här miljön kan du också lära dig hur du utvecklar program som använder PAM REST API, som beskrivs i [referensen för Privileged Access Management REST API](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## <a name="summary"></a>Sammanfattning

När du har slutfört stegen i den här genomgången kommer du att ha genomfört ett scenario för privilegierad åtkomsthantering. I det utökas användarens behörighet under en begränsad tid så att användaren kommer åt skyddade resurser med ett separat privilegierat konto. När den utökade sessionen upphör kommer det privilegierade kontot inte längre åt den skyddade resursen. PAM-administratören koordinerar beslutet om vilka säkerhetsgrupper som representerar privilegierade roller. När åtkomstbehörigheten har migrerats till Privileged Access Management-systemet, blir åtkomst som tidigare var möjlig med det ursprungliga användarkontot endast möjlig genom att logga in med ett särskilt privilegierat konto och görs tillgänglig på begäran. Gruppmedlemskap för mycket privilegierade grupper gäller därför bara under en begränsad tid.

> [!div class="step-by-step"]
> [« Steg 6 ](step-6-transition-group-to-pam.md)
