---
title: "Steg 6 Konfigurera PAM-förtroendet"
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: 52226bfa5742e39d834f80dac69317e10b6259c7


---

# Steg 6 Konfigurera PAM-förtroendet

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



<!--HONumber=Oct16_HO1-->


