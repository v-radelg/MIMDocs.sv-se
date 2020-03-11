---
title: Steg 1 Konfigurera Priv-domänen
description: Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript
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
ms.openlocfilehash: 4e24ac1b3f3f3d46aa5f67175dc4c4b8778bdb21
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043859"
---
# <a name="step-1-configuring-the-priv-domain"></a>Steg 1 Konfigurera Priv-domänen

> [!div class="step-by-step"]
> [Steg 2 »](sp1-step2-configuring-corp-domain.md)

1. Logga in på PRIVDC som administratör
   * Om det här är en PRIVOnly-miljö loggar du in på CORPDC
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Välj menyalternativ 1 (PRIV-skogskonfiguration)


Tjänstkontona som krävs för att hantera SQL/SharePoint och MIM skapas automatiskt om de inte redan finns i domänen. Du uppmanas att ange lösenorden för att skapa dessa tjänstkonton under skriptkörningen.
Om PRIV-domänen är Windows Server 2016, med funktionsnivån inställd på Windows Server 2016 Technical Preview 5, frågar skriptet om du vill aktivera den valfria Privileged Access Manager-funktionen för Active Directory som krävs av PAM. Bekräfta att du vill fortsätta genom att välja ”Yes”.
För funktionsnivåer under Windows Server 2016 ignorerar du varningen om att ingen ytterligare konfiguration kommer att utföras. Du måste köra PAMDeployment.ps1 och PAM-skogskonfigurationen igen när administratören höjer funktionsnivån till Windows Server 2016.

>[!NOTE]
>Följande steg är inte obligatoriska för PRIVOnly-konfigurationer

Kopiera SIDs.txt som skapas i $env:SYSTEMDRIVE\PAM till motsvarande mapp på CORPDC. Detta krävs av CORPDC för att konfigurera behörigheter för PRIV-användare så att de kan läsa CORP-användaregenskaper.
När skriptet har slutförts uppmanas du att starta om datorn för att ändringarna ska börja gälla.

> [!div class="step-by-step"]
> [Steg 2 »](sp1-step2-configuring-corp-domain.md)
