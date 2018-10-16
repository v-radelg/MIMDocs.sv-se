---
title: Tillägg
description: Detta är ett tillägg till de dokument som handlar om skriptbaserad PAM-distribution. Den beskriver hur du konfigurerar PRIV- och CORP-domänerna och hur du konfigurerar en klient så att den hanterar verifiering och information om hur du ber om hjälp.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: fb478c7f5df50cc4f4a8f9fb9fff1b3a0b431a14
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333929"
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
