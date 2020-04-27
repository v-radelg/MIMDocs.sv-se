---
title: Installation av BHOLD-attestering | Microsoft Docs
description: Med modulen BHOLD attestering kan du utse granskare och utföra granskningar
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d7e136ee86272429ff16a49b3c1319a4543648f
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042329"
---
# <a name="bhold-attestation-installation"></a>Installation av BHOLD-attestering

I modulen BHOLD attestering kan du utse granskare och utföra återkommande granskningar av relationer mellan användare och behörigheter och konton per program.

## <a name="bhold-attestation-installation-requirements"></a>Installations krav för BHOLD-attestering

Innan du installerar modulen BHOLD attestering måste du installera modulen BHOLD Core på den server där du planerar att installera modulen BHOLD attestering. Information om hur du installerar BHOLD Core-modulen finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Eftersom BHOLD-modulen för attestering av kontakter skickar e-postmeddelanden till användare måste din miljö ha en Simple Mail Transfer Protocol (SMTP) e-postserver, till exempel Microsoft Exchange Server.

> [!IMPORTANT]
> Om du installerar både BHOLD-rapportering och BHOLD-attestering måste du installera BHOLD repor ting innan du installerar BHOLD-attestering.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera modulen BHOLD attestering måste du vara beredd på att tillhandahålla den information som installations guiden för BHOLD attestering kräver för att slutföra installationen. I följande kalkyl blad kan du registrera informationen så att du är redo att tillhandahålla den när den behövs.

| **Objekt**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovidern på domän/dator** | När du väljer det här anger du att Active Directory Domain Services säkerhet ska styra åtkomsten till BHOLD-kärnan.                                                                                                                | Markera kryssrutan. **Viktigt:** Installationen Miss kommer om den här kryss rutan inte är markerad.                                                                                                                                                                                                                   |
| **Domain**                                  | Anger den domän som innehåller det tjänst konto som du skapade när du installerade BHOLD-kärnan. Mer information finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domän namnet anges automatiskt av guiden. Ändra bara namnet om det är felaktigt. **Viktigt:** Ange domän namnet med hjälp av NetBIOS-namnet (Short), inte det fullständigt kvalificerade domän namnet (FQDN). Om FQDN för domänen till exempel är fabrikam.com anger du domän namnet som FABRIKAM. |
| **Användare**                                    | Anger inloggnings namnet för BHOLD Core Service-användarkontot.                                                                                                                                                          | Skriv användar kontots namn här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösen ordet för tjänstens användar konto.                                                                                                                                                                       | Skriv lösen ordet här: **viktigt:** se till att behålla det här lösen ordet på en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Installation av BHOLD-attestering

Om du vill installera modulen BHOLD attestering loggar du in som medlem i gruppen domän administratörer, laddar ned följande fil och kör den som administratör på den server som du tänker installera modulen BHOLD attestering på:

- BholdAttestation<em>\<version\></em>\_release. msi

Ersätt * \<version\> * med versions numret för BHOLD-attestering som du installerar.

Om du vill köra program filen som administratör högerklickar du på filen och klickar sedan på **Kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [Installations guide för BHOLD](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
