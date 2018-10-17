---
title: Distribueringssteg för Microsoft Identity Manager 2016 | Microsoft Docs
description: Hämta hela listan med stegvisa instruktioner för att distribuera Microsoft Identity Manager 2016, från att förbereda miljön till att konfigurera portalerna.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fdf3745979be2d911c9cc1c245149328311e7ad8
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358150"
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>Distribuera Microsoft Identity Manager 2016 SP1
Artiklarna i det här avsnittet ger stegvisa instruktioner för att distribuera Microsoft Identity Manager (MIM) 2016 vid självbetjäning för slutanvändare som använder en ny server där FIM eller MIM inte tidigare har distribuerats.

> [!NOTE]
> Topologin för distribution som beskrivs i det här avsnittet är endast avsedd som en hjälp att komma igång och lära dig mer om MIM.  [Kapacitetsplaneringsguiden](capacity-planning-guide.md) innehåller mer information om topologier för produktionsdistribution.  Vi rekommenderar att du läser igenom den dokumentationen innan du distribuerar MIM för produktionsskala eller produktionsanvändning.

Scenariot med privilegierad åtkomsthantering distribueras på ett annorlunda sätt jämfört med andra MIM-scenarier, då det kräver en egen skyddsskogsmiljö.  Mer information om hur du distribuerar MIM för Privileged Identity Management finns i [Konfigurera MIM-miljö för Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

Processen för distribution av MIM är mycket lik en process i föregångaren FIM 2010 R2. Om du vill läsa FIM-dokumentationen kan du söka rätt på [distributionsguiden för Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Steg ett: Förbered en domän
MIM fungerar med Active Directory (AD), så följ de här stegen för att konfigurera din AD-domänkontrollant.
- [Domänkonfiguration](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>Nästa steg: Förbered en identitet hanteringsservrar
När din domän är på plats och har konfigurerats ska du förbereda din server för hantering av företagsidentiteter. Detta omfattar att konfigurera:
- [Windows Server 2016](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (tillval)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>Slutligen: Installera Microsoft Identity Manager 2016 SP1-komponenter
När du har konfigurerat domänen och servern är du redo att installera MIM-komponenterna och konfigurera dem så att de synkroniseras med AD.
- [Synkroniseringstjänst för MIM](install-mim-sync.md)
- [MIM-tjänst och portal](install-mim-service-portal.md)
- [Synkronisera Active Directory och MIM-tjänstens databaser](install-mim-sync-ad-service.md)
- [Plattformar som stöds för MIM 2016 eller senare](microsoft-identity-manager-2016-supported-platforms.md)
