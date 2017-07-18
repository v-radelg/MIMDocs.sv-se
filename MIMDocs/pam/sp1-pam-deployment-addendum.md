---
title: "Tillägg"
description: "Detta är ett tillägg till de dokument som handlar om skriptbaserad PAM-distribution. Den beskriver hur du konfigurerar PRIV- och CORP-domänerna och hur du konfigurerar en klient så att den hanterar verifiering och information om hur du ber om hjälp."
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
ms.openlocfilehash: f69fe68dc63323c0945a4902e34ea8153f938c02
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Tillägg för PAM-distributionsskript:
<a id="pam-deployment-scripts-addendum" class="xliff"></a>

## Tillägg 1 Konfigurera PRIV-domänen
<a id="addendum-1-setting-up-the-priv-domain" class="xliff"></a>

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
<a id="addendum-2-setting-up-the-corp-domain" class="xliff"></a>

Om du nyss har börjat med PAM och vill konfigurera en testmiljö kan du även konfigurera en CORP-domän med hjälp av skriptet. När du har packat upp den komprimerade filen till mappen $env:SYSTEMDRIVE\PAM redigerar du PAMDeploymentConfig.xml genom att lägga till informationen om CORP-skogen. Uppdatera DNSName, NetbiosName, DC-namnet, sökvägen till databasen/loggarna och sökvägen till Sysvol. Funktionsnivån bör vara minst Windows Server 2012 R2.

1. Logga in på CORP-domänens domänkontrollant som administratör
2. Kör PowerShell som administratör
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Välj menyalternativ 10 (CORP-skogskonfiguration)

Domänkontrollanten startas om automatiskt när åtgärden har slutförts

## Tillägg 3 Konfigurera CORP-klienten att utföra verifieringen
<a id="addendum-3-setting-up-a-corp-client-to-do-the-validation" class="xliff"></a>

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
<a id="addendum-4-if-something-goes-wrong" class="xliff"></a>

Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och skicka den till [mim2016@microsoft.com](mailto:mim2016@microsoft.com) tillsammans med information om åtgärden och felet.
