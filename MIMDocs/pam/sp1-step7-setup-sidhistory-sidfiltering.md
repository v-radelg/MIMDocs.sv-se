---
title: Steg 7 Konfigurera SID-historik/SID-filtrering
description: Det här är steg 7 i beskrivningen av hur du konfigurerar PIM med hjälp av skript. I det här steget beskrivs hur du konfigurerar SID-historik/SID-filtrering.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: cfb49dd3a15fd115021ab58605d5aeee4d73a5c7
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043723"
---
# <a name="step-7-set-up-sid-historysid-filtering"></a>Steg 7 Konfigurera SID-historik/SID-filtrering

> [!div class="step-by-step"]
> [« Steg 6](sp1-step6-setup-pam-trust.md)
> [Steg 8 »](sp1-step8-pam-deployment-verification.md)

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

> [!div class="step-by-step"]
> [« Steg 6](sp1-step6-setup-pam-trust.md)
> [Steg 8 »](sp1-step8-pam-deployment-verification.md)
