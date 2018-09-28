---
title: Konfigurera PAM med hjälp av skript
description: Den här artikeln ingår i en serie om att konfigurera PAM med hjälp av skript. Den beskriver hur du ändrar den XML-fil som ska användas av PAM-distributionsskripten.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/20/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: f5ae81fe8c0c695b26f2a28626512e056df4db8b
ms.sourcegitcommit: 46c68e2e0ebbf3cebae9fc04f1d2ba73bc987d5b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414983"
---
# <a name="configure-pam-using-scripts"></a>Konfigurera PAM med hjälp av skript

Om du väljer att installera SQL och SharePoint på separata servrar måste du konfigurera dem enligt anvisningarna nedan. Om SQL, SharePoint och PAM-komponenterna installeras på samma dator måste du utföra stegen nedan från den datorn.

Stegen nedan förutsätter att en PRIV-domän redan har konfigurerats. Anvisningar för hur du konfigurerar en PRIV-domän finns i tillägget i slutet av dokumentet.

Steg:

1. Hämta [Distributionsskript för PAM](https://www.microsoft.com/download/details.aspx?id=53941)
2. Packa upp den komprimerade filen ”PAMDeploymentScripts.zip” till mappen %SYSTEMDRIVE%\PAM på alla datorer.
3. Öppna filen **PAMDeploymentConfig.xml** på någon av datorerna och uppdatera informationen med hjälp av tabellen nedan eller vägledningen i själva XML-filen. Om CORP- och PRIV-skogarna redan har konfigurerats behöver du bara uppdatera **DNSName** och **NetBIOSName**.
4. I avsnittet Roles uppdaterar du **tjänstkontot**, **datorinformationen** och **platsen för binärfilerna för installationen** för SQL-, SharePoint- och MIM-rollerna.
    1. Platsen för MIM-binärfilen måste peka på katalogen som innehåller mappen Service and Portal. Platsen för klientens binärfil måste peka på katalogen som innehåller Add-ins och Extensions.msi.

5. Om det här är en PRIVOnly-miljö måste PRIVOnly-taggen tilldelas värdet True.
    1. För PRIVOnly-miljöer uppdaterar du PRIV-domänens **DNSName** och **NetbiosName** så att de matchar CORP-domänen. Kontrollera att datorsuffixen stämmer för datorer där SQL, SharePoint och MIM ska installeras eftersom standardmallfilen förutsätter en CORP- och PRIV-konfiguration.
    2. Klicka här om du vill ha mer information om PRIVOnly-miljöer.

6. Kopiera samma PAMDeploymentConfig.xml till mappen %SYSTEMDRIVE%\PAM på alla datorer, CORPDC, PRIVDC, PAM Server, SQL Server och SharePoint-servrar.


## <a name="deployment-worksheet"></a>Kalkylblad för distribution

Innan du fortsätter uppdaterar du PAMDeploymentConfig.xml och placerar den uppdaterade kopian på alla datorer.

### <a name="setup"></a>Setup

|Machine   | Vem du ska köra som   |Kommandon   |
|---|---|---|
|  PRIVDC |PRIV-domänadministratör   | .\PAMDeployment.ps1 Välj menyalternativ 1 (PRIV-skogskonfiguration)   |
|   |   |  Med ovanstående steg skapas en SIDs.txt. Den här filen måste kopieras till $envDrive:PAM på CORPDC innan du utför nästa steg. |
| CORPDC  |CORP-domänadministratör   | .\PAMDeployment.ps1 Välj menyalternativ 2 (CORP-skogskonfiguration)   |
| PAMServer (eller SQL Server)   |CORP-domänadministratör   |  .\PAMDeployment.ps1 Välj menyalternativ 2 (CORP-skogskonfiguration)  |
|  PAMServer |  Lokal administratör (MIM-administratör efter domänanslutning) |  .\PAMDeployment.ps1 Välj menyalternativ 4 (SharePoint-konfiguration)  |
| PAMServer  | Lokal administratör (MIM-administratör efter domänanslutning)  | .\PAMDeployment.ps1 Välj menyalternativ 5 (MIM PAM-konfiguration)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Välj menyalternativ 6 (PAM-förtroendekonfiguration) .\PAMDeployment.ps1 Välj menyalternativ 6 (PAM-förtroendekonfiguration) |

### <a name="validation"></a>Validering

|  Machine | Vem du ska köra som   | Kommandon   |
|---|---|---|
| CORPClient  | CORP-användare (lokal administratör)  |   .\PAMDeployment.ps1 Välj menyalternativ 7 (MIM PAM-klientkonfiguration)  |
| CORPDC  | CORP-domänadministratör   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | CORP-användare (lokal administratör)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>\PRIV.pamRequestor-användare och för PRIVOnly: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


> [!div class="step-by-step"]
> [Start »](sp1-step1-configuring-priv-domain.md)
