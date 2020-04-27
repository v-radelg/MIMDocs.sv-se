---
title: Steg 2 Konfigurera CORP-domänen
description: I den här artikeln beskrivs det andra steg som krävs för att du ska kunna konfigurera CORP-domänen, vilket involverar körning av ett skript efter det att sids.txt har kopierats CORPDC
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
ms.openlocfilehash: 1215e0be4d978e879ebc09ecdd99223fb3667a85
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043842"
---
# <a name="step-2-configuring-the-corp-domain"></a>Steg 2 Konfigurera CORP-domänen

> [!div class="step-by-step"]
> [«Steg 1](sp1-step1-configuring-priv-domain.md)
> [steg 3»](sp1-step3-installing-configuring-sql.md)

När SIDs.txt har kopierats till CORPDC **krävs inte för PRIVOnly-distributioner**

1. Logga in på CORPDC som administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Välj menyalternativ 2 (CORP-skogskonfiguration)

> [!div class="step-by-step"]
> [«Steg 1](sp1-step1-configuring-priv-domain.md)
> [steg 3»](sp1-step3-installing-configuring-sql.md)
