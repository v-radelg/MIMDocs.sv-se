---
title: Installation av BHOLD Analytics | Microsoft Docs
description: BHOLD Analytics modulen innehåller regelbaserad testning av åtkomst till data
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 2cff1f0b048a647198be6c05c658713750b41180
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2018
---
# <a name="bhold-analytics-installation"></a>BHOLD Analytics Installation

BHOLD Analytics-modulen innehåller regelbaserad testning av åtkomst till data så att organisationen effektivt kan styra åtkomsten till data och uppfyller kraven för intern och extern åtkomst. Automatisk konsekvensanalys som genererats av modulen BHOLD Analytics ger dig en översikt som visar antalet användare som kan påverkas av verkställandet av föreslagna regel både dem som skulle vara kompatibel med regeln och de som skulle bryter mot regeln. Modulen BHOLD Analytics kan också ge en detaljerad lista över de användare som skulle vara kompatibel med regeln och de som skulle bryter mot regeln.

## <a name="bhold-analytics-installation-requirements"></a>BHOLD Analytics installationskrav

Innan du installerar modulen BHOLD Analytics, måste du installera modulen BHOLD kärnor på den server där du tänker installera modulen BHOLD Analytics. Information om hur du installerar BHOLD-kärnmodul finns [BHOLD-kärninstallation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera modulen BHOLD Analytics måste förberedas för att ange den information som installationsguiden BHOLD Analytics kräver för att slutföra installationen. Följande kalkylblad hjälper dig att registrera informationen så att du är redo att leverera den när det behövs.

| **Objektet**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda Security Provider på domänen/dator** | Anger att Active Directory Domain Services-säkerhet kommer att styra åtkomsten till BHOLD-kärna.                                                                                                                | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger den domän som innehåller det tjänstkonto som du skapade när du installerar BHOLD kärnor. Mer information finns i [BHOLD-kärninstallation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om den är felaktig. **Viktigt:** ange domännamnet genom att använda NetBIOS (korta)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om FQDN för domänen är fabrikam.com, ange namnet på en domän som FABRIKAM. |
| **Användare**                                    | Anger användarkontot för BHOLD Core inloggningsnamn.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>BHOLD Analytics installation

Logga in som medlem i gruppen Domänadministratörer om du vill installera modulen BHOLD Analytics, hämta följande fil och kör det som administratör på den server som du tänker installera modulen BHOLD Analytics på:

- BholdAnalytics*\<Version\>*\_Release.msi

Ersätt *\<Version\>* med versionsnumret för BHOLD Analytics-versionen som du installerar.

För att köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

# <a name="next-steps"></a>Nästa steg

- [BHOLD Core-Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [BHOLD-installationsguiden](bhold-installation-guide.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)