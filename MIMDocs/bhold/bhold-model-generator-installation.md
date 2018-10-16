---
title: BHOLD-modellgenerator | Microsoft Docs
description: BHOLD-modellen kan du strukturera data från olika källor
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: ddb49219b5f68ff060f9b15a9ab64cb85a035d98
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333487"
---
# <a name="bhold-model-generator-installation"></a>BHOLD-Modellgenerator

Du kan strukturera data från auktoritativa källor som innehåller användar- och organisationsinformation tillsammans med åtkomstkontrollistor (ACL) till en modell som kan användas i Administrera BHOLD med hjälp av BHOLD-modellen Generator-modulen.

## <a name="bhold-model-generator-installation-requirements"></a>BHOLD-modellen Generator installationskrav 

Innan du installerar modulen Generator för BHOLD-modellen, måste du installera följande:

1. BHOLD-Core-modul på den server där du planerar att installera modulen Generator för BHOLD-modellen. Mer information om installation av BHOLD-kärnmodul, finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. Microsoft OLE DB Provider för Microsoft Jet måste installeras. Mer information finns i [i den här artikeln](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Installera inte Generator för BHOLD-modellen i företagets nätverk. Generator för BHOLD-modellen är avsedd att användas offline i en mellanlagringsmiljö för att skapa en normaliserad roll-modell som du kan importera till din modell för enterprise-rollen. Köra Generator för BHOLD-modellen i företagets nätverk kan leda till förlust av din befintliga roll-modell.

## <a name="before-you-begin"></a>Innan du börjar

Innan du installerar modulen Generator för BHOLD-modellen, måste du vara beredd att ange information som installationsguiden för BHOLD-modellen Generator krävs för att slutföra installationen. Följande kalkylblad beskriver hur du registrerar du den informationen så att du är redo att ange det när det behövs. Du måste också se till att

Microsoft Access Database Engine 2010 Redistributable

 

*Från \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Kontoinställningar**

| **Objekt**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovider på domän/datorn** | Anger att Active Directory Domain Services-säkerhet ska styra åtkomst till BHOLD-kärna.                                                                                                                | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger domänen som innehåller det tjänstkonto som du skapade när du installerar BHOLD-kärna. Mer information finns i [Core-Installation av BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om det är felaktigt. **Viktigt:** ange domännamnet genom att använda NetBIOS (kort)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om det fullständiga Domännamnet för domänen fabrikam.com, ange domännamnet som FABRIKAM. |
| **Användare**                                    | Logga in namnet på användarkontot för BHOLD-kärna.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

**Säkerhetskopiera databasinställningar**

| Objekt                                        | Beskrivning                                                                                                                                                                                                                                                                                                                                                                                                                  | Värde                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda integrerad säkerhet**                 | Anger att Windows-autentisering används för att få åtkomst till databasen.                                                                                                                                                                                                                                                                                                                                                        | Markera kryssrutan om Windows-autentisering används för att ansluta till SQL Server. Avmarkera kryssrutan om du använder SQL Server-autentisering. Databasen måste ha skapats innan kör BHOLD Core installationen om SQL Server-autentisering används. **Obs:** om Windows-autentisering används behöver du måste vara inloggad med ett konto som har rollen sysadmin på databasservern. **Viktigt:** används SQL Server-autentisering bara i testmiljöer. Microsoft att rekommenderar du använder Windows-autentisering för produktionsdistributioner. |
| **Databasanvändare** och **lösenord för databas** | Anger användarnamn och lösenord för en användare med rollen sysadmin på databasservern. De här värdena tillhandahålls endast när SQL Server-autentisering används.                                                                                                                                                                                                                                                  | Skriv namnet här på SQL Server-användare: Skriv här användarlösenord SQL Server: </br></br> **Viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Databasserver** och **databasnamn**   | Anger NetBIOS-namnet på databasservern och namnet på säkerhetskopian av databasen som skapas av installationsprogrammet för BHOLD-modellen Generator. Om du inte använder standard-databasserverinstans, anger du instansen i formatet  *\<server\>*\\*\<instans\>* .  Microsoft rekommenderar att du namnger säkerhetskopierade databasen med namnet på databasen BHOLD-kärna följt av \_säkerhetskopiering, till exempel B1_BACKUP. | Skriv namn här server (eller server och instans): </br> Skriv namnet på databasen här:

## <a name="bhold-model-generator-setup"></a>Installation av BHOLD-modellen Generator

Logga in som medlem i gruppen Domänadministratörer för att installera modulen Generator för BHOLD-modellen, hämta följande fil och köra den som administratör på den server som du planerar att installera modulen BHOLD-kärnor på:

- BholdModelGenerator  *\<Version\>*\_Release.msi

Ersätt *\<Version\>* med det lägre versionsnumret för den modell-Generator för BHOLD-versionen som du installerar.

Om du vill köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- Information om hur du skapar indatafilerna [Microsoft BHOLD Suite Teknisk referens](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [BHOLD-installationsguide](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)