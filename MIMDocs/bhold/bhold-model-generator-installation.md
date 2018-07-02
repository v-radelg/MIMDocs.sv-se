---
title: BHOLD modellen generator installation | Microsoft Docs
description: BHOLD-modell kan du strukturera data från olika källor
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 90e7da2a1e39b802723ff0714bd0caccf9649440
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289143"
---
# <a name="bhold-model-generator-installation"></a>BHOLD modellen Generator Installation

Med hjälp av modulen BHOLD modellen Generator strukturerar du data från auktoritära källor som innehåller användar- och organisationsinformation tillsammans med åtkomstkontrollistor (ACL) till en modell som kan användas i Administrera BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>BHOLD modellen Generator installationskrav 

Innan du installerar modulen BHOLD modellen Generator, måste du installera följande:

1. BHOLD-kärnmodul på servern som du tänker installera modulen BHOLD modellen Generator. Information om hur du installerar BHOLD-kärnmodul finns [BHOLD-kärninstallation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. Microsoft OLE DB Provider för Microsoft Jet måste installeras. Mer information finns i [i den här artikeln](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Installera inte BHOLD modellen Generator i produktionsnätverket. BHOLD modellen Generator är avsedd att användas offline i en fristående miljö för att skapa en normaliserade roll-modell som du kan importera till din modell för enterprise-rollen. Kör BHOLD modellen Generator i produktionsnätverket kan resultera i förlust av din befintliga roll-modell.

## <a name="before-you-begin"></a>Innan du börjar

Innan du installerar modulen BHOLD modellen Generator måste förberedas för att ange information som installationsguiden BHOLD modellen Generator krävs för att slutföra installationen. Följande kalkylblad hjälper dig att registrera informationen så att du är redo att leverera den när det behövs. Du måste också kontrollera

Microsoft Access Database Engine 2010 Redistributable

 

*Från \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Inställningarna för kontot**

| **Objektet**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda Security Provider på domänen/dator** | Anger att Active Directory Domain Services-säkerhet kommer att styra åtkomsten till BHOLD-kärna.                                                                                                                | Markera kryssrutan. **Viktigt:** installationen misslyckas om den här kryssrutan är avmarkerad.                                                                                                                                                                                                                   |
| **Domän**                                  | Anger den domän som innehåller det tjänstkonto som du skapade när du installerar BHOLD kärnor. Mer information finns i [BHOLD-kärninstallation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domännamnet anges automatiskt av guiden. Ändra namnet endast om den är felaktig. **Viktigt:** ange domännamnet genom att använda NetBIOS (korta)-namn, inte det fullständigt kvalificerade domännamnet (FQDN). Till exempel om FQDN för domänen är fabrikam.com, ange namnet på en domän som FABRIKAM. |
| **Användare**                                    | Anger användarkontot för BHOLD Core inloggningsnamn.                                                                                                                                                          | Skriv namnet på användarkontot här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösenordet för tjänstkontot för användaren.                                                                                                                                                                       | Skriv lösenordet här: **viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                  |

**Inställningar för säkerhetskopiering**

| Objekt                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                  | Värde                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använda integrerad säkerhet**                 | Anger att Windows-autentisering används för att få åtkomst till databasen.                                                                                                                                                                                                                                                                                                                                                        | Markera kryssrutan om Windows-autentisering används för att ansluta till SQL Server. Avmarkera kryssrutan om SQL Server-autentisering används. Databasen måste ha skapats innan du kör BHOLD Core installation om SQL Server-autentisering används. **Obs:** om Windows-autentisering används, du måste vara inloggad med ett konto som har serverrollen sysadmin på databasservern. **Viktigt:** används SQL Server-autentisering bara i testmiljöer. Microsoft att rekommenderar du använder Windows-autentisering för produktionsdistributioner. |
| **Databasanvändare** och **lösenord för databas** | Anger användarnamn och lösenord för en användare med serverrollen sysadmin på databasservern. Dessa värden anges endast när SQL Server-autentisering används.                                                                                                                                                                                                                                                  | Skriv namnet här på SQL Server-användare: skriva SQL Server användarens lösenord här: </br></br> **Viktigt:** se till att det här lösenordet i en dold, säker plats.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Databasserver** och **databasnamn**   | Anger namnet på säkerhetskopian av databasen som skapas av installationsprogrammet för BHOLD modellen Generator och NetBIOS-namnet på databasservern. Om du inte använder standard-databasserverinstans, anger du databasserverinstansen i formatet  *\<server\>*\\*\<instans\>* .  Microsoft rekommenderar att du namnger säkerhetskopierade databasen med namnet på databasen BHOLD Core följt av \_säkerhetskopiering, till exempel B1_BACKUP. | Skriv namnet här server (eller server och instans): </br> Skriv namnet på databasen här:

## <a name="bhold-model-generator-setup"></a>BHOLD modellen Generator installationen

Logga in som medlem i gruppen Domänadministratörer om du vill installera modulen BHOLD modellen Generator, hämta följande fil och kör det som administratör på den server som du tänker installera modulen BHOLD kärnor på:

- BholdModelGenerator  *\<Version\>*\_Release.msi

Ersätt *\<Version\>* med versionsnumret för den BHOLD modellen Generator-versionen som du installerar.

För att köra programfilen som administratör, högerklicka på filen och klicka sedan på **kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- Information om hur du skapar indatafiler [Teknisk referens för Microsoft BHOLD-paket](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [BHOLD-installationsguiden](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)