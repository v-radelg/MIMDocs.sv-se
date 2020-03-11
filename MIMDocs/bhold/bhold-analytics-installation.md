---
title: BHOLD Analytics-installation | Microsoft Docs
description: BHOLD Analytics-modulen innehåller regelbaserade tester av data åtkomst
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7d26abb58fa976d40638b9512d5684d86f483378
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79041938"
---
# <a name="bhold-analytics-installation"></a>BHOLD Analytics-installation

BHOLD Analytics-modulen innehåller regelbaserade test av data åtkomst för att säkerställa att din organisation effektivt kan styra åtkomsten till data och uppfylla interna och externa åtkomst krav. Den automatiserade effekt analys som genereras av BHOLD Analytics-modulen ger dig en översikt som visar antalet användare som skulle påverkas av en föreslagen regels genomförande, både de som skulle följa regeln och de som skulle strida mot regeln. BHOLD Analytics-modulen kan också innehålla en detaljerad lista över de användare som skulle följa regeln och de som skulle kränka regeln.

## <a name="bhold-analytics-installation-requirements"></a>Installations krav för BHOLD Analytics

Innan du installerar BHOLD Analytics-modulen måste du installera BHOLD Core-modulen på den server där du planerar att installera BHOLD Analytics-modulen. Information om hur du installerar BHOLD Core-modulen finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera BHOLD Analytics-modulen måste du vara beredd på att tillhandahålla den information som installations guiden för BHOLD Analytics kräver för att slutföra installationen. I följande kalkyl blad kan du registrera informationen så att du är redo att tillhandahålla den när den behövs.

| **Konfigurationsobjektet**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovidern på domän/dator** | När du väljer det här anger du att Active Directory Domain Services säkerhet ska styra åtkomsten till BHOLD-kärnan.                                                                                                                | Markera kryss rutan. **Viktigt:** Installationen Miss kommer om den här kryss rutan inte är markerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger den domän som innehåller det tjänst konto som du skapade när du installerade BHOLD-kärnan. Mer information finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domän namnet anges automatiskt av guiden. Ändra bara namnet om det är felaktigt. **Viktigt:** Ange domän namnet med hjälp av NetBIOS-namnet (Short), inte det fullständigt kvalificerade domän namnet (FQDN). Om FQDN för domänen till exempel är fabrikam.com anger du domän namnet som FABRIKAM. |
| **Användare**                                    | Anger inloggnings namnet för BHOLD Core Service-användarkontot.                                                                                                                                                          | Skriv användar kontots namn här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösen ordet för tjänstens användar konto.                                                                                                                                                                       | Skriv lösen ordet här: **viktigt:** se till att behålla det här lösen ordet på en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>BHOLD Analytics-installation

Om du vill installera BHOLD Analytics-modulen loggar du in som medlem i gruppen domän administratörer, laddar ned följande fil och kör den som administratör på den server som du tänker installera BHOLD Analytics-modulen på:

- BholdAnalytics<em>\<Version\></em>\_release. msi

Ersätt *\<version\>* med versions numret för den BHOLD Analytics-version som du installerar.

Om du vill köra program filen som administratör högerklickar du på filen och klickar sedan på **Kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Installations guide för BHOLD](bhold-installation-guide.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
