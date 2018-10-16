---
title: BHOLD-rapportering Installation | Microsoft Docs
description: BHOLD rapportmodulen kan du generera rapporter om roller och auktoriseringsprinciper
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 7c95529bf1cd227bee8f1dde11c94a810b44fc8c
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332263"
---
# <a name="bhold-reporting-installation"></a>BHOLD-rapportering Installation

BHOLD-rapportmodul ger dig möjlighet att generera rapporter om roller och andra auktoriseringsprinciper i BHOLD. De här rapporterna är ofta användbara för granskning eller för att visa efterlevnad av regelkrav. Den här modulen utökar dessutom dina möjligheter att hantera auktorisering i din organisation genom att ge användare information de behöver för att analysera medlemskapet för deras roller. Rapporterna kan ha begränsad vyer som se till att de användare som skapar rapporter visas endast informationen som de har behörighet att visa.

## <a name="bhold-reporting-installation-requirements"></a>BHOLD-rapportering installationskrav

Innan du installerar rapportmodulen för BHOLD, måste du installera BHOLD-Core-modul på den server där du planerar att installera modulen BHOLD-rapportering. Mer information om installation av BHOLD-kärnmodul, finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Om du installerar både BHOLD-rapportering och BHOLD-attestering måste du installera BHOLD-rapportering för innan du installerar BHOLD-attestering.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar installera rapportmodulen för BHOLD, måste förberedas för att ange information som installationsguiden för BHOLD Reporting krävs för att slutföra installationen. Följande kalkylblad beskriver hur du registrerar du den informationen så att du är redo att ange det när det behövs.

| **Objekt**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovider på domän/datorn** | Anger att Active Directory Domain Services-säkerhet ska styra åtkomst till BHOLD-kärna.                                                                                                                | Markera kryssrutan. </br>**Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger domänen som innehåller det tjänstkonto som du skapade när du installerar BHOLD-kärna. Mer information finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om det är felaktigt. **Viktigt:** ange domännamnet genom att använda NetBIOS (kort)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om det fullständiga Domännamnet för domänen fabrikam.com, ange domännamnet som FABRIKAM. |
| **Användare**                                    | Logga in namnet på användarkontot för BHOLD-kärna.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: </br>**Viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Installation av BHOLD-rapportering

Logga in som medlem i gruppen Domänadministratörer för att installera rapportmodulen för BHOLD, hämta följande fil och köra den som administratör på den server som du planerar att installera modulen BHOLD-rapportering på:

- BholdReporting<em>\<Version\></em>\_Release.msi

Ersätt *\<Version\>* med versionsnumret för BHOLD-rapportering versionen som du installerar.

Om du vill köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- [BHOLD-installationsguide](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)