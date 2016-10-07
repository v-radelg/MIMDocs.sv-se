---
title: "Steg 1 Konfigurera Priv-domänen"
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
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 37ac600701ed9d90790e557d2f282be45eed43b4


---
# Steg 1 Konfigurera Priv-domänen

1. Logga in på PRIVDC som administratör
  * Om det här är en PRIVOnly-miljö loggar du in på CORPDC
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Välj menyalternativ 1 (PRIV-skogskonfiguration)


Tjänstkontona som krävs för att hantera SQL/SharePoint och MIM skapas automatiskt om de inte redan finns i domänen. Du uppmanas att ange lösenorden för att skapa dessa tjänstkonton under skriptkörningen.
Om PRIV-domänen är Windows Server 2016, med funktionsnivån inställd på Windows Server 2016 Technical Preview 5, frågar skriptet om du vill aktivera den valfria Privileged Access Manager-funktionen för Active Directory som krävs av PAM. Bekräfta att du vill fortsätta genom att välja ”Yes”.
För funktionsnivåer under Windows Server 2016 ignorerar du varningen om att ingen ytterligare konfiguration kommer att utföras. Du måste köra PAMDeployment.ps1 och PAM-skogskonfigurationen igen när administratören höjer funktionsnivån till Windows Server 2016.

>[!Note] Följande steg är inte obligatoriska för PRIVOnly-konfigurationer

Kopiera SIDs.txt som skapas i $env:SYSTEMDRIVE\PAM till motsvarande mapp på CORPDC. Detta krävs av CORPDC för att konfigurera behörigheter för PRIV-användare så att de kan läsa CORP-användaregenskaper.
När skriptet har slutförts uppmanas du att starta om datorn för att ändringarna ska börja gälla.



<!--HONumber=Sep16_HO4-->


