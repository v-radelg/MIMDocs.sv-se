---
title: Installation av BHOLD access management | Microsoft Docs
description: BHOLD connector-modulen stöder initiala lösenord och fortlöpande synkronisering av data
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: d3a4ab5203e772db80c5345aab8f1c66731c8020
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333708"
---
# <a name="access-management-connector-installation"></a>Installation av hantering av åtkomst

Modulen BHOLD Suite Access Management-anslutningstjänsten har stöd för både första och pågående synkronisering av data till BHOLD. Management-anslutningstjänsten åtkomst fungerar med synkroniseringstjänsten för Microsoft Identity Manager (MIM) för att flytta data mellan BHOLD-kärna-databasen, FIM 2010 metaversum och målprogram och identitetslagringar. När du har installerat modulen Access Management-anslutningstjänsten, kommer du att kunna skapa FIM-Hanteringsagenter som styr dataflödet mellan BHOLD och MIM.

## <a name="access-management-connector-software-requirements"></a>Programvarukrav för Access Management-anslutningstjänsten

Innan du kan installera modulen Access Management-anslutningstjänsten måste installera du Microsoft .NET Framework 4. Mer information om .NET Framework 4 och instruktioner för installation finns den [startsidan för Microsoft .NET](http://www.microsoft.com/net).
Du måste installera Access Management-anslutningstjänsten på en dator som kör FIM-synkronisering Service av MIM.

## <a name="access-management-connector-setup"></a>Konfiguration av företagsåtkomst Management-anslutningstjänsten

Logga in som medlem i gruppen Domänadministratörer för att installera modulen åtkomstkontroll för hantering, hämta följande fil och köra den som administratör på den server som du planerar att installera modulen BHOLD FIM-integrering på:

- AccessManagementConnector.msi

Om du vill köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [Installation av BHOLD FIM-integrering](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) för att aktivera självbetjäning av slutanvändare av roller, kan du installera modulen BHOLD FIM-integrering
- [BHOLD-installationsguide](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)