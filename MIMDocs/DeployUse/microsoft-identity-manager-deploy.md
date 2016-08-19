---
title: Distribuera MIM 2016 | Microsoft Identity Manager
description: "Hämta hela listan med stegvisa instruktioner för att distribuera Microsoft Identity Manager 2016, från att förbereda miljön till att konfigurera portalerna."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: db062cf8dafe0b480db06cd8d913583c5b709246


---

# Distribuera MIM 2016
Artiklarna i det här avsnittet ger stegvisa instruktioner för att distribuera Microsoft Identity Manager (MIM) 2016 vid självbetjäning för slutanvändare som använder en ny server där FIM eller MIM inte tidigare har distribuerats.

> [!NOTE]
> Topologin för distribution som beskrivs i det här avsnittet är endast avsedd som en hjälp att komma igång och lära dig mer om MIM.  [Kapacitetsplaneringsguiden](/microsoft-identity-manager/plan-design/capacity-planning-guide) innehåller mer information om topologier för produktionsdistribution.  Vi rekommenderar att du läser igenom den dokumentationen innan du distribuerar MIM för produktionsskala eller produktionsanvändning.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## Steg ett: Förbered en domän
MIM fungerar med Active Directory (AD), så följ de här stegen för att konfigurera din AD-domänkontrollant.
- [Domänkonfiguration](preparing-domain.md)

## Nästa steg: Förbered en server för identitetshantering
När din domän är på plats och har konfigurerats ska du förbereda din server för hantering av företagsidentiteter. Detta omfattar att konfigurera:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (tillval)

## Slutligen: Installera komponenter för Microsoft Identity Manager 2016
När du har konfigurerat domänen och servern är du redo att installera MIM-komponenterna och konfigurera dem så att de synkroniseras med AD.
- [Synkroniseringstjänst för MIM](install-mim-sync.md)
- [MIM-tjänst och portal](install-mim-service-portal.md)
- [Synkronisera Active Directory och MIM-tjänstens databaser](install-mim-sync-ad-service.md)



<!--HONumber=Jul16_HO3-->


