---
title: "PAM-miljööversikt | Microsoft Docs"
description: "Hitta antalet av virtuella datorer och konfigurationen av virtuella datorer som krävs för att distribuera Privileged Access Management"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e6c5a70c6b9ed140a56135676bbd14a84504317
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="environment-overview"></a>Översikt över miljön

Privileged Access Management arbetar med virtuella datorer med separata enheter som är anslutna till varandra i ett delat nätverk. De här virtuella datorerna kan hanteras på Windows 8.1, Windows Server 2012 R2 och andra operativsystemsplattformar.

![PAM-servrar: relationer och plattformar som stöds – diagram](media/pam-test-lab-architecture.png)

Du behöver minst tre virtuella datorer.  Om du inte redan har en AD-domän som ska hanteras av PAM behöver du en ytterligare virtuell dator som CORP-domänkontrollant.  Om du vill konfigurera PRIV-programvaran för hög tillgänglighet, behöver du ytterligare två virtuella datorer.

De enheter där de virtuella datorernas diskavbildningar ska sparas måste ha minst 120 GB ledigt diskutrymme för att ha plats för alla de virtuella datorerna.  Om du planerar att distribuera för hög tillgänglighet ska du kontrollera att diskdelsystemet uppfyller kraven för SQL-delad lagring.  Det delade lagringsutrymmet kan bestå av WSFC-klusterdiskar (Windows Server Failover Clustering), diskar på ett SAN-nätverk (Storage Area Network) eller filer som delas på en SMB-server. Obs! De måste vara tilldelade särskilt för skyddsmiljön. Vi rekommenderar inte att lagringsutrymmet delas med andra arbetsbelastningar utanför skyddsmiljön eftersom det kan äventyra skyddsmiljöns integritet.

> [!NOTE]
> Nuvarande CTP-version (customer technical preview) av MIM-CTP är inte kompatibel med innehållet i den föregående CTP-versionens databaser och kataloger. Om du har tidigare har testat MIM för PAM och andra scenarier bör du säkerhetskopiera och arkivera de virtuella datorer du använde i de testen och starta distributionen med nya avbildningar av virtuella datorer som inte använts tidigare i MIM-scenarier.
