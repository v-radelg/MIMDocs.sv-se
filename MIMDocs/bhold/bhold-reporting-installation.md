---
title: BHOLD reporting Installation | Microsoft Docs
description: BHOLD rapportmodulen kan du generera rapporter om roller och auktoriseringsprinciper
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-reporting-installation"></a>BHOLD reporting Installation

BHOLD-rapportmodulen ger dig möjlighet att generera rapporter om roller och andra auktoriseringsprinciper i BHOLD. Dessa rapporter är ofta användbar för granskning eller för att visa efterleva krav. Den här modulen utökar också möjligheten att hantera auktorisering inom din organisation genom att ge användarna den information de behöver att analysera medlemskap i deras roller. Rapporterna kan ha begränsad vyer som se till att de användare som skapar rapporter visas enbart informationen som de kan se.

## <a name="bhold-reporting-installation-requirements"></a>BHOLD Reporting installationskrav

Innan du installerar rapportmodulen BHOLD, måste du installera modulen BHOLD kärnor på den server där du planerar att installera rapportmodulen för BHOLD. Information om hur du installerar BHOLD-kärnmodul finns [BHOLD-kärninstallation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
Om du installerar både BHOLD rapportering och BHOLD attestering, måste du installera Reporting BHOLD för innan du installerar BHOLD attestering.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera rapportmodulen BHOLD måste förberedas för att ange den information som installationsguiden BHOLD-rapportering kräver för att slutföra installationen. Följande kalkylblad hjälper dig att registrera informationen så att du är redo att leverera den när det behövs.

| **Objektet**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda Security Provider på domänen/dator** | Anger att Active Directory Domain Services-säkerhet kommer att styra åtkomsten till BHOLD-kärna.                                                                                                                | Markera kryssrutan. </br>**Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger den domän som innehåller det tjänstkonto som du skapade när du installerar BHOLD kärnor. Mer information finns i [BHOLD-kärninstallation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om den är felaktig. **Viktigt:** ange domännamnet genom att använda NetBIOS (korta)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om FQDN för domänen är fabrikam.com, ange namnet på en domän som FABRIKAM. |
| **Användare**                                    | Anger användarkontot för BHOLD Core inloggningsnamn.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: </br>**Viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>BHOLD Reporting-installation

Logga in som medlem i gruppen Domänadministratörer för att installera rapportmodulen BHOLD, hämta följande fil och kör det som administratör på den server som du tänker installera rapportmodulen BHOLD på:

- BholdReporting*\<Version\>*\_Release.msi

Ersätt  *\<Version\>*  med versionsnumret för rapportering av BHOLD-versionen som du installerar.

För att köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [BHOLD-installationsguiden](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)