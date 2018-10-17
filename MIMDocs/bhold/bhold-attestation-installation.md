---
title: Installation av BHOLD-attestering | Microsoft Docs
description: BHOLD-attestering modulen kan du ange granskare och utföra granskningar
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358116"
---
# <a name="bhold-attestation-installation"></a>Installation av BHOLD-attestering

BHOLD-attestering modulen kan du ange granskare och utföra återkommande granskningar av relationerna mellan användare och per programbehörigheter och konton.

## <a name="bhold-attestation-installation-requirements"></a>Installationskrav för BHOLD-attestering

Innan du installerar modulen BHOLD-attestering måste du installera BHOLD-Core-modul på den server där du planerar att installera modulen BHOLD-attestering. Mer information om installation av BHOLD-kärnmodul, finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Eftersom e-postmeddelandet BHOLD-attestering modulen kontakter skickar meddelanden till användare, måste din miljö ha en Simple Mail Transfer Protocol (SMTP) e-postserver, till exempel Microsoft Exchange Server.

> [!IMPORTANT]
> Om du installerar både BHOLD-rapportering och BHOLD-attestering måste du installera BHOLD-rapportering för innan du installerar BHOLD-attestering.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar att installera modulen BHOLD-attestering måste förberedas för att ange information som installationsguiden för BHOLD-attestering krävs för att slutföra installationen. Följande kalkylblad beskriver hur du registrerar du den informationen så att du är redo att ange det när det behövs.

| **Objekt**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovider på domän/datorn** | Anger att Active Directory Domain Services-säkerhet ska styra åtkomst till BHOLD-kärna.                                                                                                                | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger domänen som innehåller det tjänstkonto som du skapade när du installerar BHOLD-kärna. Mer information finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om det är felaktigt. **Viktigt:** ange domännamnet genom att använda NetBIOS (kort)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om det fullständiga Domännamnet för domänen fabrikam.com, ange domännamnet som FABRIKAM. |
| **Användare**                                    | Logga in namnet på användarkontot för BHOLD-kärna.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Installation av BHOLD-attestering

För att installera modulen BHOLD-attestering måste logga in som medlem i gruppen Domänadministratörer, hämta följande fil och köra den som administratör på den server som du planerar att installera modulen BHOLD-attestering på:

- BholdAttestation<em>\<Version\></em>\_Release.msi

Ersätt *\<Version\>* med versionsnumret för BHOLD-attestering versionen som du installerar.

Om du vill köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [BHOLD-installationsguide](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
