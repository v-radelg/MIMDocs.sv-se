---
title: BHOLD attestering installation | Microsoft Docs
description: "BHOLD attestering modulen kan du ange granskare och utföra granskningar"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 93d0b9a17d82911b71b1b220465b6d637687444b
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-attestation-installation"></a>BHOLD attestering installation

Attesteringen av BHOLD-modulen kan du ange granskare och utför återkommande granskning av relationerna mellan användare och per program behörigheter och konton.

## <a name="bhold-attestation-installation-requirements"></a>BHOLD attestering installationskrav

Innan du installerar modulen BHOLD attestering, måste du installera modulen BHOLD kärnor på den server som du tänker installera modulen BHOLD attestering. Information om hur du installerar BHOLD-kärnmodul finns [BHOLD-kärninstallation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Eftersom e-postmeddelandet BHOLD attestering modulen kontakter skickar meddelanden till användare, måste din miljö ha en SMTP Simple Mail Transfer Protocol () e-postserver, till exempel Microsoft Exchange Server.

>[!IMPORTANT]
Om du installerar både BHOLD rapportering och BHOLD attestering, måste du installera Reporting BHOLD för innan du installerar BHOLD attestering.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera modulen BHOLD attestering, måste förberedas för att ange information som installationsguiden BHOLD attestering krävs för att slutföra installationen. Följande kalkylblad hjälper dig att registrera informationen så att du är redo att leverera den när det behövs.

| **Objektet**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda Security Provider på domänen/dator** | Anger att Active Directory Domain Services-säkerhet kommer att styra åtkomsten till BHOLD-kärna.                                                                                                                | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger den domän som innehåller det tjänstkonto som du skapade när du installerar BHOLD kärnor. Mer information finns i [BHOLD-kärninstallation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om den är felaktig. **Viktigt:** ange domännamnet genom att använda NetBIOS (korta)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om FQDN för domänen är fabrikam.com, ange namnet på en domän som FABRIKAM. |
| **Användare**                                    | Anger användarkontot för BHOLD Core inloggningsnamn.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Attesteringen av BHOLD-installation

Logga in som medlem i gruppen Domänadministratörer om du vill installera modulen BHOLD attestering, hämta följande fil och köra den som administratör på den server som du tänker installera modulen BHOLD attestering på:

- BholdAttestation*\<Version\>*\_Release.msi

Ersätt * \<Version\> * med versionsnumret för attestering av BHOLD-versionen som du installerar.

För att köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [BHOLD-installationsguiden](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)