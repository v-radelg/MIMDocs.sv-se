---
title: Distribuera tjänsten för meddelanden om lösenordsändring | Microsoft Docs
description: Hämta anvisningarna för att installera och konfigurera MIM:s tjänst för meddelanden om lösenordsändring på domänkontrollanten.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f195a6db259bf0fefabcd05c8890ca65c9624314
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041972"
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>Distribuera MIM:s tjänst för meddelanden om lösenordsändring till en domänkontrollant<!--test-->

## <a name="install-the-password-change-notification-service"></a>Installera Tjänsten för meddelanden om lösenordsändring
Tjänsten för meddelanden om lösenordsändring (PCNS) är en tjänst som du installerar på domänkontrollanterna och som gör det möjligt för MIM att synkronisera lösenord med andra system, t.ex. en annan leverantörs katalogserver. Vid synkronisering av lösenord installerar du PCNS på alla domänkontrollantservrar.

1.  Logga in som domänadministratör på en server som körs på Windows Server med rollen Domäntjänster för Active Directory.

2.  Kopiera installationsmappen för Tjänsten för meddelanden om lösenordsändring till datorn.

3.  Leta upp filen *Tjänst för meddelanden om lösenordsändring.msi*, högerklicka på den och skapa en genväg.

4.  Leta upp genvägsfilen, högerklicka på den och öppna dess **Egenskaper**.

5.  I målfältet lägger du till prefixet *msiexec.exe /i* före sökvägen till msi-filen och suffixet *SCHEMAONLY=TRUE* efter msi-sökvägen. Om till exempel installationsmappen är *C:\PCNS* skulle körningskommandot se ut så här: (allt på en rad).

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Spara ändringarna i genvägsfilen.

7.  Kör genvägsfilen för att starta PCNS-installationen i läget schemautökning. När nästa skärm visas klickar du på **Nästa**.

8.  Du får ett meddelande om att installationen nu kommer att uppdatera Active Directory-schemat för Tjänsten för meddelanden om lösenordsändring. Klicka på **OK** för att gå vidare med schemauppdateringen.

9. När schemautökningen är klar och följande skärmbild visas klickar du på **Slutför**.

10. Kör filen *Tjänst för meddelanden om lösenordsändringen.msi* en gång till – den här gången direkt (ingen körningssträng krävs).  När nästa skärm visas klickar du på **Nästa**.

11. Godkänn licensavtalet och klicka på **Nästa**.

12. Klicka för att påbörja installationen.

13. När skärmen Installationen är klar visas klickar du på **Slutför**.

14. För att de konfigurationsändringar som gjorts i MIM:s Tjänst för meddelanden om lösenordsändring ska börja gälla måste du starta om datorn. Du kan göra det genom att klicka på **Ja** i popup-fönstret som visas. Du kan också starta om senare.

## <a name="configuring-the-password-change-notification-service"></a>Konfigurera Tjänsten för meddelanden om lösenordsändring
När du är återansluten till DC-servern som domänadministratör går du till *C:\Programfiler\Microsofts meddelanden om lösenordsändring.* Kör *pcnscfg.exe*.
