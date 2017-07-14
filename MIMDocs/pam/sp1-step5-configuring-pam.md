---
title: Steg 5 Installera/konfigurera PAM
description: "Detta är steg 5 i beskrivningen av hur du konfigurerar PAM med hjälp av skript, och här beskrivs de olika distributionsstegen på PAM-servern."
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
ms.translationtype: MT
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 862f62ab9bac87bcee31c35e249db34740e9fb14
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---
# Steg 5 Installera/konfigurera PAM
<a id="step-5-installingconfiguring-pam" class="xliff"></a>

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

