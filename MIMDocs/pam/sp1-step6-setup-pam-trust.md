---
title: Steg 6 Konfigurera PAM-förtroendet
description: Steg 6 i beskrivningen av hur du konfigurerar PAM med hjälp av skript. I det här avsnittet beskrivs hur du konfigurerar det nödvändiga förtroendet mellan företagsdomäner och privata domäner
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
ms.openlocfilehash: ad1482718693c9ae7004a71334013de68f7c20da
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518300"
---
# <a name="step-6-set-up-the-pam-trust"></a>Steg 6 Konfigurera PAM-förtroendet

> [!div class="step-by-step"]
> [« Steg 5](sp1-step5-configuring-pam.md)
> [Steg 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Detta krävs inte för en miljö med endast PRIV** Logga in på PAMServer med MIMAdmin-kontot.

1. Logga in på PAMServer med MIMAdmin-kontot
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Välj menyalternativ 6 (PAM-förtroendekonfiguration)

   När du uppmanas att göra det anger du autentiseringsuppgifterna för CORP-administratörskontot. När du har angett autentiseringsuppgifterna upprättas förtroendet och konfigurationen är klar.

> [!div class="step-by-step"]
> [« Steg 5](sp1-step5-configuring-pam.md)
> [Steg 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
