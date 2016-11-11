---
title: Steg 5 Installera/konfigurera PAM
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
ms.openlocfilehash: c641865548f753a609ccee8dbf12c329bb6a1c9f


---
# <a name="step-5-installingconfiguring-pam"></a>Steg 5 Installera/konfigurera PAM

>[!div class="step-by-step"]
[« Steg 4](sp1-step4-configuring-sharepoint.md)
[Steg 6 »](sp1-step6-setup-pam-trust.md)

För en domänansluten PAMServer loggar du in som MIMAdmin. Annars loggar du in som lokal administratör.
1. Kör PowerShell som administratör
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Välj menyalternativ 5 (MIM PAM-konfiguration)

>[!NOTE]
>Om datorn inte redan är domänansluten till PRIV uppmanas du att ange autentiseringsuppgifter. Datorn startas om efter domänanslutningen.

När PAMServer har startat om loggar du in på datorn med MIMAdmin-kontot igen.

1. Kör PowerShell som administratör
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Välj menyalternativ 5 (MIM PAM-konfiguration)

  När du uppmanas att göra det anger du lösenordet, MIM-övervakningskontot, MIM-komponentkontot, MIM MA-kontot, MIM-tjänstkontot, MIM-administratörskontot och SharePoint-kontot.
  Datorn startas om när installationen har slutförts.

>[!div class="step-by-step"]
[« Steg 4](sp1-step4-configuring-sharepoint.md)
[Steg 6 »](sp1-step6-setup-pam-trust.md)



<!--HONumber=Nov16_HO2-->


