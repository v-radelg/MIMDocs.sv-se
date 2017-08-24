---
title: "Steg 6 Konfigurera PAM-förtroendet"
description: "Steg 6 i beskrivningen av hur du konfigurerar PAM med hjälp av skript. I det här avsnittet beskrivs hur du konfigurerar det nödvändiga förtroendet mellan företagsdomäner och privata domäner"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: a24b2a5a196e633379b696ee82e9428dd67454ca
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/19/2017
---
# <a name="step-6-set-up-the-pam-trust"></a>Steg 6 Konfigurera PAM-förtroendet

>[!div class="step-by-step"]
[« Steg 5](sp1-step5-configuring-pam.md)
[Steg 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Detta krävs inte för en miljö med endast PRIV** Logga in på PAMServer med MIMAdmin-kontot.

1. Logga in på PAMServer med MIMAdmin-kontot
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Välj menyalternativ 6 (PAM-förtroendekonfiguration)

  När du uppmanas att göra det anger du autentiseringsuppgifterna för CORP-administratörskontot. När du har angett autentiseringsuppgifterna upprättas förtroendet och konfigurationen är klar.

>[!div class="step-by-step"]
[« Steg 5](sp1-step5-configuring-pam.md)
[Steg 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
