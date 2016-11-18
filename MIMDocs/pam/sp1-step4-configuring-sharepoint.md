---
title: Steg 4 Konfigurera SharePoint
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
ms.openlocfilehash: 76696f7dce3d79a845c2a8ba9ae8d284012a0df7


---

# <a name="step-4-configuring-sharepoint"></a>Steg 4 Konfigurera SharePoint

>[!div class="step-by-step"]
[« Steg 3](sp1-step3-installing-configuring-sql.md)
[Steg 5 »](sp1-step5-configuring-pam.md)

SharePoint måste vara SharePoint Foundation 2013 med SP1.

För domänanslutna servrar loggar du in som MIMAdmin

1. Kör PowerShell som administratör
2.  .\PAMDeployment.ps1
3.  Välj menyalternativ 4 (SharePoint-konfiguration)


För arbetsgruppsservrar

1. Kör PowerShell som administratör
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. Välj menyalternativ 4 (SharePoint-konfiguration)

Datorn startas om flera gånger när SharePoint installeras. Varje gång måste SharePoint-installationsprogrammet köras igen. Var noga med att logga in med MIMAdmin-kontot.
Om datorn som installerar SharePoint inte har någon Internetanslutning för att hämta de komponenter som krävs kan dessa laddas ned separat och placeras i en lokal mapp. **Den här sökvägen till den lokala mappen måste uppdateras i <PrerequisitesBinaryLocation/> i filen PAMConfiguration.xml.** Tillägg 5 innehåller länkar där du kan ladda ned filerna.
Efter installationen öppnas det grafiska användargränssnittet för SharePoint-konfigurationen och vägleder dig genom stegen för att slutföra SharePoint-installationen. Välj alternativet för hela servern och gå igenom resten av användargränssnittet. Efter installationen uppmanas du att köra konfigurationsguiden. Utför stegen genom att följa anvisningarna nedan.

1. Ändra till **Skapa en ny servergrupp** på fliken **Anslut till en servergrupp**.
2. Ange **SQLServer** som databasservern för konfigurationsdatabasen och **SharePoint ServiceAccount** som det konto för databasåtkomst som SharePoint ska använda.
3. Ange ett lösenord som lösenfrasen för att skydda servergruppen **(det används inte senare)**.
4. Acceptera resten av standardinställningarna i konfigurationsguiden för SharePoint för att skapa en servergrupp med en enda server.

Mer information finns i avsnittet **Konfigurera SharePoint** i [Steg 3 – Förbereda en PAM-server](/microsoft-identity-manager/pam/step-3-prepare-pam-server). När konfigurationsguiden har slutförts kör du skriptet ”.\PAMDeployment.ps1” igen och väljer alternativ 4 (SharePoint-konfiguration) för att slutföra det här steget.

>[!div class="step-by-step"]
[« Steg 3](sp1-step3-installing-configuring-sql.md)
[Steg 5 »](sp1-step5-configuring-pam.md)



<!--HONumber=Nov16_HO2-->


