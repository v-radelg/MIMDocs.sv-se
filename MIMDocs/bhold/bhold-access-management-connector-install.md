---
title: Installation av BHOLD Access Management Connector | Microsoft Docs
description: BHOLD Connector-modulen stöder inledande och pågående synkronisering av data
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ae9cc0bb4c63089c6733c06b7b035b2b9566fdd0
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041871"
---
# <a name="access-management-connector-installation"></a>Installation av åtkomst hanterings anslutning

BHOLD Suite Access Management Connector-modulen stöder både inledande och pågående synkronisering av data till BHOLD. Access Management-anslutningsprogrammet fungerar med synkroniseringstjänsten för Microsoft Identity Manager (MIM) för att flytta data mellan BHOLD Core-databasen, FIM 2010-metaversum och mål program och identitets lager. När du har installerat Access Management Connector-modulen kommer du att kunna skapa FIM-hanterings agenter som styr data flödet mellan BHOLD och MIM.

## <a name="access-management-connector-software-requirements"></a>Program varu krav för åtkomst hanterings anslutning

Innan du kan installera Access Management Connector-modulen måste du installera Microsoft .NET Framework 4. Mer information om .NET Framework 4 och installations anvisningar finns på [Start sidan för Microsoft .net](https://www.microsoft.com/net).
Du måste installera Access Management Connector på en dator som kör FIM-synkroniseringstjänsten för MIM.

## <a name="access-management-connector-setup"></a>Installation av åtkomst hanterings anslutning

För att installera modulen för åtkomst hanterings kontroll loggar du in som medlem i gruppen domän administratörer, laddar ned följande fil och kör den som administratör på den server som du vill installera BHOLD FIM-integrering på:

- AccessManagementConnector. msi

Om du vill köra program filen som administratör högerklickar du på filen och klickar sedan på **Kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [Installation av BHOLD FIM-integrering](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Om du vill aktivera självbetjäning av roller för slutanvändare kan du installera BHOLD FIM-integrering
- [Installations guide för BHOLD](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
