---
title: Steg 4 Konfigurera SharePoint
description: Det här är steg 4 i beskrivningen av hur du konfigurerar PAM med hjälp av skript. I det här steget konfigurerar du SharePoint så att det kan användas som en del av din PAM-distribution.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 17776b882b6a3f67313e2e41b424cbdaf22b6a44
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043808"
---
# <a name="step-4-configuring-sharepoint"></a>Steg 4 Konfigurera SharePoint

> [!div class="step-by-step"]
> [« Steg 3](sp1-step3-installing-configuring-sql.md)
> [Steg 5 »](sp1-step5-configuring-pam.md)

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
3. Ange ett lösenord som lösenfrasen för att skydda servergruppen **(det används inte senare)** .
4. Acceptera resten av standardinställningarna i konfigurationsguiden för SharePoint för att skapa en servergrupp med en enda server.

Mer information finns i avsnittet **Konfigurera SharePoint** i [Steg 3 – Förbereda en PAM-server](/microsoft-identity-manager/pam/step-3-prepare-pam-server). När konfigurationsguiden har slutförts kör du skriptet ”.\PAMDeployment.ps1” igen och väljer alternativ 4 (SharePoint-konfiguration) för att slutföra det här steget.

> [!div class="step-by-step"]
> [« Steg 3](sp1-step3-installing-configuring-sql.md)
> [Steg 5 »](sp1-step5-configuring-pam.md)
