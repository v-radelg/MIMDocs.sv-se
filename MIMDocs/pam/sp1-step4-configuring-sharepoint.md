---
title: Steg 4 Konfigurera SharePoint
description: "Det här är steg 4 i beskrivningen av hur du konfigurerar PAM med hjälp av skript. I det här steget konfigurerar du SharePoint så att det kan användas som en del av din PAM-distribution."
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
ms.openlocfilehash: 12eb9a00584f72b9c628e870562a743fb603d4a3
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---

# Steg 4 Konfigurera SharePoint
<a id="step-4-configuring-sharepoint" class="xliff"></a>

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

