---
title: "Distribueringssteg för Microsoft Identity Manager 2016 | Microsoft Docs"
description: "Hämta hela listan med stegvisa instruktioner för att distribuera Microsoft Identity Manager 2016, från att förbereda miljön till att konfigurera portalerna."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 804f957d5cf9f5fb09efa65944983b1d5013fff3
ms.lasthandoff: 01/24/2017


---

# <a name="deploy-mim-2016"></a>Distribuera MIM 2016
Artiklarna i det här avsnittet ger stegvisa instruktioner för att distribuera Microsoft Identity Manager (MIM) 2016 vid självbetjäning för slutanvändare som använder en ny server där FIM eller MIM inte tidigare har distribuerats.

> [!NOTE]
> Topologin för distribution som beskrivs i det här avsnittet är endast avsedd som en hjälp att komma igång och lära dig mer om MIM.  [Kapacitetsplaneringsguiden](/microsoft-identity-manager/plan-design/capacity-planning-guide) innehåller mer information om topologier för produktionsdistribution.  Vi rekommenderar att du läser igenom den dokumentationen innan du distribuerar MIM för produktionsskala eller produktionsanvändning.

Scenariot med privilegierad åtkomsthantering distribueras på ett annorlunda sätt jämfört med andra MIM-scenarier, då det kräver en egen skyddsskogsmiljö.  Mer information om hur du distribuerar MIM för Privileged Identity Management finns i [Konfigurera MIM-miljö för Privileged Access Management](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam).

Distributionsprocessen för MIM 2016 påminner i mycket om samma process i föregångaren FIM 2010 R2. Om du vill läsa FIM-dokumentationen kan du söka rätt på [distributionsguiden för Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Steg ett: Förbered en domän
MIM fungerar med Active Directory (AD), så följ de här stegen för att konfigurera din AD-domänkontrollant.
- [Domänkonfiguration](preparing-domain.md)

## <a name="next-prepare-an-identity-management-server"></a>Nästa steg: Förbered en server för identitetshantering
När din domän är på plats och har konfigurerats ska du förbereda din server för hantering av företagsidentiteter. Detta omfattar att konfigurera:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQLServer 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (tillval)

## <a name="finally-install-microsoft-identity-manager-2016-components"></a>Slutligen: Installera komponenter för Microsoft Identity Manager 2016
När du har konfigurerat domänen och servern är du redo att installera MIM-komponenterna och konfigurera dem så att de synkroniseras med AD.
- [Synkroniseringstjänst för MIM](install-mim-sync.md)
- [MIM-tjänst och portal](install-mim-service-portal.md)
- [Synkronisera Active Directory och MIM-tjänstens databaser](install-mim-sync-ad-service.md)

