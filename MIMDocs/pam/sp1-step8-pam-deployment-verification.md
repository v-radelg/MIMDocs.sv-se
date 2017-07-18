---
title: Steg 8 Verifiera PAM-distributionen
description: "Skriptdistributionen av PAM inkluderar verifieringsskript som kan köra ett PAM-scenario som verifierar att PAM-distributionen fungerar som förväntat."
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
ms.openlocfilehash: 2f4306dc50ecb869a3c917dfaf320ad80dddedd1
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Steg 8 Verifiera PAM-distributionen
<a id="step-8-pam-deployment-verification" class="xliff"></a>

>[!div class="step-by-step"]
[« Steg 7](sp1-step7-setup-sidhistory-sidfiltering.md)
[Tillägg »](sp1-pam-deployment-addendum.md)

Distributionspaketet innehåller verifieringsskript som kan köra ett PAM-scenario för att bekräfta att PAM-distributionen fungerar som förväntat.
Om du vill använda distributionsverifieringen redigerar du avsnittet <PamValidation/> i PAMDeploymentConfig.xml.

>[!NOTE]
>Verifieringen kräver en klientdator som är domänansluten till CORP-domänen där PAM-komponenterna på klientsidan är installerade. Skript för klientinstallationer finns i tillägget.

Klientdatorns namn måste uppdateras i <PAMValidationClient/>-taggen i PAMDeploymentConfig.xml. Resten av informationen i <PAMValidation/>-noden behöver bara ändras om den är i konflikt med befintliga användare/grupper eftersom verifieringen försöker skapa dem.
Utför verifieringen genom att följa dessa steg:

Steg 1:

1. Logga in på CORPDC som CORP-domänadministratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Nu skapas de grupper och användare som behövs för verifieringen

Steg 2:

1. Logga in på PAM-servern som MIMAdmin
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

I det här steget migreras användarna och grupperna till PAM-miljön

Steg 3:

1. Logga in på CORP-klienten som lokal administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


I det här steget uppmanas du att ange autentiseringsuppgifterna för CORPAdmin. När autentiseringsuppgifterna har angetts läggs nödvändiga användarna till i grupperna ”Användare av fjärrskrivbord” och ”Fjärrhanteringsanvändare”.
Använd följande kommando på CORP-klienten för att öppna PowerShell som den PRIV-användare som du verifierar. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
Skriv följande i PowerShell-fönstret:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Nu visas statusen för begäran.
  Till en början har användaren inte åtkomst till resursen. När användaren har lagts till i rollen med Just-In-Time beviljas användaren åtkomst. När varaktigheten för begäran går ut har användaren återigen ingen åtkomst.
  Skriptet använder standardvärdet (11 minuter) som varaktighet för begäran.

>[!div class="step-by-step"]
[« Steg 7](sp1-step7-setup-sidhistory-sidfiltering.md)
[Tillägg »](sp1-pam-deployment-addendum.md)
