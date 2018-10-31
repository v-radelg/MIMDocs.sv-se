---
title: Steg 7 Konfigurera SID-historik/SID-filtrering
description: Det här är steg 7 i beskrivningen av hur du konfigurerar PIM med hjälp av skript. I det här steget beskrivs hur du konfigurerar SID-historik/SID-filtrering.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: f85dd4eff32d5207948ec332bf2e9850b14a86fe
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379336"
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
