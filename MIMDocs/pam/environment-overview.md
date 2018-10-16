---
title: PAM-miljööversikt | Microsoft Docs
description: Hitta antalet av virtuella datorer och konfigurationen av virtuella datorer som krävs för att distribuera Privileged Access Management
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4e3a3fb2b23deced2202994463645521beef81e8
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332926"
---
# <a name="environment-overview"></a>Översikt över miljön

Privileged Access Management arbetar med virtuella datorer med separata enheter som är anslutna till varandra i ett delat nätverk. De här virtuella datorerna kan hanteras på Windows 8.1, Windows Server 2012 R2 och andra operativsystemsplattformar.

![PAM-servrar: relationer och plattformar som stöds – diagram](media/pam-test-lab-architecture.png)

Du behöver minst tre virtuella datorer.  Om du inte redan har en AD-domän som ska hanteras av PAM, måste en ytterligare virtuell dator som CORP-domänkontrollant.  Om du vill konfigurera PRIV-programvaran för hög tillgänglighet behöver du ytterligare två virtuella datorer.

Enheter där VM-diskavbildningar ska sparas måste ha minst 120 GB ledigt diskutrymme.  Om du planerar att distribuera för hög tillgänglighet ska du kontrollera att diskdelsystemet uppfyller kraven för SQL-delad lagring.  Det delade lagringsutrymmet kan bestå av WSFC-klusterdiskar (Windows Server Failover Clustering), diskar på ett SAN-nätverk (Storage Area Network) eller filer som delas på en SMB-server.

> [!IMPORTANT]
> Lagring måste vara dedikerad till skyddsmiljön. Lagringsutrymmet delas med andra arbetsbelastningar utanför skyddsmiljön rekommenderas inte eftersom det kan äventyra skyddsmiljöns integritet i skyddsmiljön.

## <a name="next-steps"></a>Nästa steg

- [Privileged Access Management för Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) är en översikt över PAM och hur det fungerar.
- [Förstå komponenterna i PAM](principles-of-operation.md) är en översikt över de olika komponenterna i PAM.