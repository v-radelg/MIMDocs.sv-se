---
title: Steg 5 Installera/konfigurera PAM
description: Detta är steg 5 i beskrivningen av hur du konfigurerar PAM med hjälp av skript, och här beskrivs de olika distributionsstegen på PAM-servern.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 839cb4fa69aefa8024d38aeff0ebae96599aefb3
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288851"
---
# <a name="step-5-installingconfiguring-pam"></a>Steg 5 Installera/konfigurera PAM

> [!div class="step-by-step"]
> [« Steg 4](sp1-step4-configuring-sharepoint.md)
> [Steg 6 »](sp1-step6-setup-pam-trust.md)

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

> [!div class="step-by-step"]
> [« Steg 4](sp1-step4-configuring-sharepoint.md)
> [Steg 6 »](sp1-step6-setup-pam-trust.md)
