---
title: PAM-miljööversikt | Microsoft Docs
description: Hitta antalet av virtuella datorer och konfigurationen av virtuella datorer som krävs för att distribuera Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c758bd924c6f7272a44567c2e9dc919215cdd273
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044012"
---
# <a name="environment-overview"></a>Översikt över miljön

Privileged Access Management arbetar med virtuella datorer med separata enheter som är anslutna till varandra i ett delat nätverk. De här virtuella datorerna kan hanteras på Windows 8.1, Windows Server 2012 R2 och andra operativsystemsplattformar.

![PAM-servrar: relationer och plattformar som stöds – diagram](media/pam-test-lab-architecture.png)

Du behöver minst tre virtuella datorer.  Om du inte redan har en AD-domän för PAM att hantera behöver du en ytterligare virtuell dator för att fungera som en CORP-domänkontrollant.  Om du vill konfigurera den tidigare program varan för hög tillgänglighet behöver du två ytterligare virtuella datorer.

De enheter där de virtuella dator disk avbildningarna kommer att lagras måste vara minst 120 GB ledigt disk utrymme.  Om du planerar att distribuera för hög tillgänglighet ska du kontrollera att diskdelsystemet uppfyller kraven för SQL-delad lagring.  Det delade lagringsutrymmet kan bestå av WSFC-klusterdiskar (Windows Server Failover Clustering), diskar på ett SAN-nätverk (Storage Area Network) eller filer som delas på en SMB-server.

> [!IMPORTANT]
> Lagringen måste vara dedikerad i skydds-miljön. Delning av lagring med andra arbets belastningar utanför skydds-miljön rekommenderas inte eftersom det kan äventyra integriteten hos skydds-miljön.

## <a name="next-steps"></a>Nästa steg

- [Privileged Access Management för Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) är en översikt över pam och hur det fungerar.
- [Förstå komponenterna i pam](principles-of-operation.md) är en översikt över de olika komponenterna i pam.
