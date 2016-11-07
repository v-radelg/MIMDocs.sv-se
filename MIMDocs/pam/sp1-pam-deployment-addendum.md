---
title: "Tillägg"
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: cdd859ceb13d187af3303235c0fe1e496f2bfb6e


---
# <a name="pam-deployment-scripts-addendum"></a>Tillägg för PAM-distributionsskript:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Tillägg 1 Konfigurera PRIV-domänen

När du har packat upp den komprimerade filen till mappen $env:SYSTEMDRIVE\PAM redigerar du PAMDeploymentConfig.xml genom att lägga till informationen om PRIV-skogen. Uppdatera DNSName, NetbiosName, DC-namnet, sökvägen till databasen/loggarna och sökvägen till Sysvol. Uppdatera också DomainMode och ForestMode. Om du testar Windows Server Technical Preview 5 anger du DomainMode och ForestMode till WinThreshold.

1. Logga in på PRIV-domänens domänkontrollant som administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Välj menyalternativ 9 (Priv-skogskonfiguration)


Domänkontrollanten startas om automatiskt när åtgärden har slutförts. Administratörslösenordet för återställningsläge för katalogtjänster (DSRM) måste uppfylla följande krav:

  * Lösenordet måste innehålla minst 15 tecken
  * Lösenordet måste innehålla minst en gemen
  * Lösenordet måste innehålla minst en versal
  * Lösenordet måste innehålla minst en siffra eller ett specialtecken

## <a name="addendum-2-setting-up-the-corp-domain"></a>Tillägg 2 Konfigurera CORP-domänen

Om du nyss har börjat med PAM och vill konfigurera en testmiljö kan du även konfigurera en CORP-domän med hjälp av skriptet. När du har packat upp den komprimerade filen till mappen $env:SYSTEMDRIVE\PAM redigerar du PAMDeploymentConfig.xml genom att lägga till informationen om CORP-skogen. Uppdatera DNSName, NetbiosName, DC-namnet, sökvägen till databasen/loggarna och sökvägen till Sysvol. Funktionsnivån bör vara minst Windows Server 2012 R2.

1. Logga in på CORP-domänens domänkontrollant som administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Välj menyalternativ 10 (CORP-skogskonfiguration)

Domänkontrollanten startas om automatiskt när åtgärden har slutförts

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Tillägg 3 Konfigurera CORP-klienten att utföra verifieringen

ClientBinaryLocation i konfigurationsfilen måste peka på platsen där setup.exe finns.
Logga in på klienten som lokal administratör och kör följande kommandon i en upphöjd PowerShell-kommandotolk:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Välj menyalternativ 7 (MIM PAM-klientkonfiguration)


Om datorn inte är domänansluten uppmanas du att ange autentiseringsuppgifterna för CORP för att upprätta en domänanslutning. Datorn måste startas om efter domänanslutningen. Logga in på klienten igen som lokal administratör och kör följande kommandon från en upphöjd PowerShell-kommandotolk:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Välj menyalternativ 7 (MIM PAM-klientkonfiguration)

Fortsätt med steg 8 (se ovan).

## <a name="addendum-4-if-something-goes-wrong"></a>Tillägg 4 Om det uppstår problem

Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och skicka den till [mim2016@microsoft.com](mailto:mim2016@microsoft.com) tillsammans med information om åtgärden och felet.



<!--HONumber=Oct16_HO1-->


