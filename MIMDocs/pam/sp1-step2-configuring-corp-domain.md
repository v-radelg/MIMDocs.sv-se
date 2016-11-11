---
title: "Steg 2 Konfigurera CORP-domänen"
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
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>Steg 2 Konfigurera CORP-domänen

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



<!--HONumber=Nov16_HO2-->


