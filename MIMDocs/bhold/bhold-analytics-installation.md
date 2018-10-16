---
title: Installation av BHOLD-analys | Microsoft Docs
description: BHOLD-Analytics-modulen innehåller regelbaserad testning av åtkomst till data
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 5a31ab2ba6f8ff0a8388d4976e4c44adaf044d87
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332280"
---
# <a name="bhold-analytics-installation"></a>Installation av BHOLD-analys

BHOLD-Analytics-modulen innehåller regelbaserad testning av åtkomst till data så att din organisation kan ett effektivt sätt att styra åtkomsten till data och uppfyller kraven för intern och extern åtkomst. Den automatiska konsekvensanalys som genererats av modulen BHOLD-analys ger en översikt som visar antalet användare som kan påverkas av tillämpning av en föreslagen regel både personer som skulle vara kompatibel med regeln och de som skulle strida mot regeln. BHOLD-analysmodul kan också ge en detaljerad lista över de användare som vill följa regeln och de som skulle strida mot regeln.

## <a name="bhold-analytics-installation-requirements"></a>Installationskrav för BHOLD-analys

Innan du installerar modulen BHOLD-analys, måste du installera BHOLD-Core-modul på den server där du planerar att installera modulen BHOLD-analys. Mer information om installation av BHOLD-kärnmodul, finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera modulen BHOLD-analys, måste förberedas för att ange den information som installationsguiden för BHOLD Analytics kräver för att slutföra installationen. Följande kalkylblad beskriver hur du registrerar du den informationen så att du är redo att ange det när det behövs.

| **Objekt**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovider på domän/datorn** | Anger att Active Directory Domain Services-säkerhet ska styra åtkomst till BHOLD-kärna.                                                                                                                | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger domänen som innehåller det tjänstkonto som du skapade när du installerar BHOLD-kärna. Mer information finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om det är felaktigt. **Viktigt:** ange domännamnet genom att använda NetBIOS (kort)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om det fullständiga Domännamnet för domänen fabrikam.com, ange domännamnet som FABRIKAM. |
| **Användare**                                    | Logga in namnet på användarkontot för BHOLD-kärna.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Installation av BHOLD-analys

Logga in som medlem i gruppen Domänadministratörer för att installera modulen BHOLD-analys, hämta följande fil och köra den som administratör på den server som du planerar att installera modulen BHOLD-analys på:

- BholdAnalytics<em>\<Version\></em>\_Release.msi

Ersätt *\<Version\>* med versionsnumret för BHOLD-analys-versionen som du installerar.

Om du vill köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

# <a name="next-steps"></a>Nästa steg

- [BHOLD-Core-Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [BHOLD-installationsguide](bhold-installation-guide.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)