---
title: "Tillägg"
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 482cfbbac3ea668ca4bf9d8a4a45469e61634f98


---
# Tillägg:

## Tillägg 1 Konfigurera PRIV-domänen

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

## Tillägg 2 Konfigurera CORP-domänen

Om du nyss har börjat med PAM och vill konfigurera en testmiljö kan du även konfigurera en CORP-domän med hjälp av skriptet. När du har packat upp den komprimerade filen till mappen $env:SYSTEMDRIVE\PAM redigerar du PAMDeploymentConfig.xml genom att lägga till informationen om CORP-skogen. Uppdatera DNSName, NetbiosName, DC-namnet, sökvägen till databasen/loggarna och sökvägen till Sysvol. Funktionsnivån bör vara minst Windows Server 2012 R2.

1. Logga in på CORP-domänens domänkontrollant som administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Välj menyalternativ 10 (CORP-skogskonfiguration)

Domänkontrollanten startas om automatiskt när åtgärden har slutförts

## Tillägg 3 Konfigurera CORP-klienten att utföra verifieringen

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

## Tillägg 4 Om det uppstår problem

Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och skicka den till [mim2016@microsoft.com](mim2016@microsoft.com) tillsammans med information om åtgärden och felet.



<!--HONumber=Sep16_HO4-->


