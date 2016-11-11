---
title: Steg 7 Konfigurera SID-historik/SID-filtrering
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: a98d83a22c61ef534fcc02725e4cd500be10cc8a


---

# <a name="step-7-set-up-sid-historysid-filtering"></a>Steg 7 Konfigurera SID-historik/SID-filtrering

>[!div class="step-by-step"]
[« Steg 6](sp1-step6-setup-pam-trust.md)
[Steg 8 »](sp1-step8-pam-deployment-verification.md)

**Detta krävs inte för en miljö med endast PRIV** Logga in på PAMServer med MIMAdmin-kontot.

1. Logga in på CORP-domänkontrollanten som administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Välj menyalternativ 8 (konfigurera SID-historik/SID-filtrering)

När åtgärden har körts visas följande meddelanden:<br/></br>
För SID-filtrering: <br/></br>
”Anger att SID-filtrering inte ska användas i förtroendet” eller ”SID-filtrering har inte aktiverats för förtroendet”. </br></br>
För SID-historik: </br></br>
”SID-historik aktiveras för förtroendet” eller ”SD-historik har redan aktiverats för förtroendet”.

>[!div class="step-by-step"]
[« Steg 6](sp1-step6-setup-pam-trust.md)
[Steg 8 »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Nov16_HO2-->


