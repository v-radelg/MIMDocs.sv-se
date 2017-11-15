---
title: Installation av BHOLD access management | Microsoft Docs
description: "BHOLD-koppling modulen stöder inledande och pågående synkronisering av data"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/14/2017
---
# <a name="access-management-connector-installation"></a>Installation av Access Management

Modulen BHOLD Suite Access Management-anslutningstjänsten stöder både inledande och pågående synkronisering av data till BHOLD. Access Management-anslutningstjänsten fungerar med Microsoft Identity Manager (MIM)-synkroniseringstjänsten för att flytta data mellan BHOLD Core-databasen, FIM 2010 metaversum och målprogram och identitetslagringar. När du har installerat modulen Access Management-anslutningstjänsten, kommer du att kunna skapa FIM-Hanteringsagenter som styr dataflödet mellan BHOLD och MIM.

## <a name="access-management-connector-software-requirements"></a>Programvarukrav för Access Management-anslutningstjänsten

Innan du kan installera modulen Access Management-anslutningstjänsten måste du installera Microsoft .NET Framework 4. Mer information om .NET Framework 4 och installationsanvisningar finns i [startsida för Microsoft .NET](http://www.microsoft.com/net).
Du måste installera Access Management-anslutningstjänsten på en dator som kör FIM-synkronisering Service av MIM.

## <a name="access-management-connector-setup"></a>Inställningar för Access Management-anslutningstjänsten

Logga in som medlem i gruppen Domänadministratörer om du vill installera modulen åtkomstkontroll för hantering, hämta följande fil och kör det som administratör på den server som du tänker installera modulen BHOLD FIM-integrering på:

- AccessManagementConnector.msi

För att köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [BHOLD FIM-integrering installation](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) för att aktivera Självbetjäning för slutanvändare av roller, kan du installera modulen BHOLD FIM-integrering
- [BHOLD-installationsguiden](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)