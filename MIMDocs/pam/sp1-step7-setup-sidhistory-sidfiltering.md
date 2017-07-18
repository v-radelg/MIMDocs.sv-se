---
title: Steg 7 Konfigurera SID-historik/SID-filtrering
description: "Det här är steg 7 i beskrivningen av hur du konfigurerar PIM med hjälp av skript. I det här steget beskrivs hur du konfigurerar SID-historik/SID-filtrering."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: e608593f40759e3bc995daa56c4575510a71e987
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Steg 7 Konfigurera SID-historik/SID-filtrering
<a id="step-7-set-up-sid-historysid-filtering" class="xliff"></a>

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
