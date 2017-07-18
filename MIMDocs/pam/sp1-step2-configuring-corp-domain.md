---
title: "Steg 2 Konfigurera CORP-domänen"
description: "I den här artikeln beskrivs det andra steg som krävs för att du ska kunna konfigurera CORP-domänen, vilket involverar körning av ett skript efter det att sids.txt har kopierats CORPDC"
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
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Steg 2 Konfigurera CORP-domänen
<a id="step-2-configuring-the-corp-domain" class="xliff"></a>

>[!div class="step-by-step"]
[« Steg 1](sp1-step1-configuring-priv-domain.md)
[Steg 3 »](sp1-step3-installing-configuring-sql.md)

När SIDs.txt har kopierats till CORPDC **krävs inte för PRIVOnly-distributioner**

1. Logga in på CORPDC som administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Välj menyalternativ 2 (CORP-skogskonfiguration)

>[!div class="step-by-step"]
[« Steg 1](sp1-step1-configuring-priv-domain.md)
[Steg 3 »](sp1-step3-installing-configuring-sql.md)
