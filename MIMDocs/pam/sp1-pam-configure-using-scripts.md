---
title: "Konfigurera PAM med hjälp av skript"
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 96c734ade75f5c206858387cf45106761bc0a881
ms.openlocfilehash: a1e4e5561bf8d38c56c3d27249d94f4bf7103b8c


---

# Konfigurera PAM med hjälp av skript

Om du väljer att installera SQL och SharePoint på separata servrar måste de konfigureras med hjälp av anvisningarna nedan. Om SQL, SharePoint och PAM-komponenterna installeras på samma dator måste du utföra stegen nedan från den datorn.

Stegen nedan förutsätter att en PRIV-domän redan har konfigurerats. Anvisningar för hur du konfigurerar en PRIV-domän finns i tillägget i slutet av dokumentet.

Steg:

1. Packa upp den komprimerade filen ”PAMDeploymentScripts.zip” till mappen %SYSTEMDRIVE%\PAM på alla datorer.
2. Öppna filen **PAMDeploymentConfig.xml** på någon av datorerna och uppdatera informationen med hjälp av tabellen nedan eller vägledningen i själva XML-filen. Om CORP- och PRIV-skogarna redan har konfigurerats behöver du bara uppdatera **DNSName** och **NetBIOSName**.
3. I avsnittet Roles uppdaterar du **tjänstkontot**, **datorinformationen** och **platsen för binärfilerna för installationen** för SQL-, SharePoint- och MIM-rollerna.
    1. Platsen för MIM-binärfilen måste peka på katalogen som innehåller mappen Service and Portal. Platsen för klientens binärfil måste peka på katalogen som innehåller Add-ins och Extensions.msi.

4. Om det här är en PRIVOnly-miljö måste PRIVOnly-taggen tilldelas värdet True.
    1. För PRIVOnly-miljöer uppdaterar du PRIV-domänens **DNSName** och **NetbiosName** så att de matchar CORP-domänen. Kontrollera att datorsuffixen stämmer för datorer där SQL, SharePoint och MIM ska installeras eftersom standardmallfilen förutsätter en CORP- och PRIV-konfiguration.
    2. Klicka här om du vill ha mer information om PRIVOnly-miljöer.

5. Kopiera samma PAMDeploymentConfig.xml till mappen %SYSTEMDRIVE%\PAM på alla datorer, CORPDC, PRIVDC, PAM Server, SQL Server och SharePoint-servrar.


## Kalkylblad för distribution

Innan du fortsätter uppdaterar du PAMDeploymentConfig.xml och placerar den uppdaterade kopian på alla datorer.

### Setup

|Machine   | Vem du ska köra som   |Kommandon   |
|---|---|---|
|  PRIVDC |PRIV-domänadministratör   | .\PAMDeployment.ps1 Välj menyalternativ 1 (PRIV-skogskonfiguration)   |
|   |   |  Med ovanstående steg skapas en SIDs.txt. Den här filen måste kopieras till $envDrive:PAM på CORPDC innan du utför nästa steg. |
| CORPDC  |CORP-domänadministratör   | .\PAMDeployment.ps1 Välj menyalternativ 2 (CORP-skogskonfiguration)   |
| PAMServer (eller SQL Server)   |CORP-domänadministratör   |  .\PAMDeployment.ps1 Välj menyalternativ 2 (CORP-skogskonfiguration)  |
|  PAMServer |  Lokal administratör (MIM-administratör efter domänanslutning) |  .\PAMDeployment.ps1 Välj menyalternativ 4 (SharePoint-konfiguration)  |
| PAMServer  | Lokal administratör (MIM-administratör efter domänanslutning)  | .\PAMDeployment.ps1 Välj menyalternativ 5 (MIM PAM-konfiguration)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Välj menyalternativ 6 (PAM-förtroendekonfiguration) .\PAMDeployment.ps1 Välj menyalternativ 6 (PAM-förtroendekonfiguration) |

### Validering

|  Machine | Vem du ska köra som   | Kommandon   |
|---|---|---|
| CORPClient  | CORP-användare (lokal administratör)  |   .\PAMDeployment.ps1 Välj menyalternativ 7 (MIM PAM-klientkonfiguration)  |
| CORPDC  | CORP-domänadministratör   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | CORP-användare (lokal administratör)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>\PRIV.pamRequestor-användare och för PRIVOnly: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |



<!--HONumber=Sep16_HO4-->


