---
title: Installation av BHOLD-rapportering | Microsoft Docs
description: Med BHOLD repor ting module kan du generera rapporter om roller och Auktoriseringsprinciper
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7bac40105aa53add914599c59e812587b6be9585
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042041"
---
# <a name="bhold-reporting-installation"></a>Installation av BHOLD-rapportering

BHOLD repor ting-modulen ger dig möjlighet att generera rapporter om roller och andra auktoriseringsprinciper i BHOLD. Dessa rapporter är ofta användbara för granskning eller för att demonstrera efterlevnad av myndighets krav. Dessutom utökar den här modulen din möjlighet att hantera auktorisering i organisationen genom att ge användarna den information de behöver för att analysera medlemskapet för deras roller. Rapporterna kan ha begränsade vyer som säkerställer att de användare som skapar rapporter endast visar den information som de får se.

## <a name="bhold-reporting-installation-requirements"></a>Installations krav för BHOLD-rapportering

Innan du installerar BHOLD repor ting-modulen måste du installera BHOLD Core-modulen på den server där du planerar att installera modulen BHOLD rapportering. Information om hur du installerar BHOLD Core-modulen finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Om du installerar både BHOLD-rapportering och BHOLD-attestering måste du installera BHOLD repor ting innan du installerar BHOLD-attestering.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera BHOLD repor ting-modulen måste du vara beredd på att tillhandahålla den information som krävs för att slutföra installationen av installations guiden för BHOLD-rapportering. I följande kalkyl blad kan du registrera informationen så att du är redo att tillhandahålla den när den behövs.

| **Konfigurationsobjektet**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovidern på domän/dator** | När du väljer det här anger du att Active Directory Domain Services säkerhet ska styra åtkomsten till BHOLD-kärnan.                                                                                                                | Markera kryss rutan. </br>**Viktigt:** Installationen Miss kommer om den här kryss rutan inte är markerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger den domän som innehåller det tjänst konto som du skapade när du installerade BHOLD-kärnan. Mer information finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domän namnet anges automatiskt av guiden. Ändra bara namnet om det är felaktigt. **Viktigt:** Ange domän namnet med hjälp av NetBIOS-namnet (Short), inte det fullständigt kvalificerade domän namnet (FQDN). Om FQDN för domänen till exempel är fabrikam.com anger du domän namnet som FABRIKAM. |
| **Användare**                                    | Anger inloggnings namnet för BHOLD Core Service-användarkontot.                                                                                                                                                          | Skriv användar kontots namn här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösen ordet för tjänstens användar konto.                                                                                                                                                                       | Skriv lösen ordet här: </br>**Viktigt:** Se till att behålla det här lösen ordet på en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Installation av BHOLD-rapportering

Om du vill installera BHOLD repor ting-modulen loggar du in som medlem i gruppen domän administratörer, laddar ned följande fil och kör den som administratör på den server som du tänker installera BHOLD repor ting module på:

- BholdReporting<em>\<Version\></em>\_release. msi

Ersätt *\<version\>* med versions numret för den BHOLD-rapport version som du installerar.

Om du vill köra program filen som administratör högerklickar du på filen och klickar sedan på **Kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [Installations guide för BHOLD](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
