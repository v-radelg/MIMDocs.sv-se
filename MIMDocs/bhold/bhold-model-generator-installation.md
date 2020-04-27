---
title: Installation av BHOLD modell Generator | Microsoft Docs
description: Med BHOLD-modellen kan du strukturera data från olika källor
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 6f7e0979246eb2124604f594c57b40ec11cc7140
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042024"
---
# <a name="bhold-model-generator-installation"></a>Installation av BHOLD-modell Generator

Med hjälp av modulen BHOLD modell Generator kan du strukturera data från auktoritativa källor som innehåller användar-och organisationsinformation tillsammans med åtkomst kontrol listor (ACL) i en modell som kan användas för att administrera BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Installations krav för BHOLD Model Generator 

Innan du installerar BHOLD modell Generator-modulen måste du installera följande:

1. BHOLD Core-modul på den server där du planerar att installera modulen BHOLD modell Generator. Information om hur du installerar BHOLD Core-modulen finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. Microsoft OLE DB-providern för Microsoft Jet måste vara installerad. Mer information finns i [den här artikeln](https://support.microsoft.com/kb/271908).

> [!WARNING]
> Installera inte BHOLD Model Generator i produktions nätverket. BHOLD modell Generator är avsedd att användas offline i en utvecklings miljö för att skapa en normaliserad roll modell som du kan importera till din företags roll modell. Om du kör BHOLD Model Generator i produktions nätverket kan din befintliga roll modell försvinna.

## <a name="before-you-begin"></a>Innan du börjar

Innan du installerar BHOLD modell Generator-modulen måste du vara beredd att tillhandahålla den information som installations guiden för BHOLD modell Generator kräver för att slutföra installationen. I följande kalkyl blad kan du registrera informationen så att du är redo att tillhandahålla den när den behövs. Du måste också se till att

Microsoft Access Database Engine 2010 Redistributable

 

*Som\<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Kontoinställningar**

| **Objekt**                                    | **Beskrivning**                                                                                                                                                                                                           | **Värde**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd säkerhetsprovidern på domän/dator** | När du väljer det här anger du att Active Directory Domain Services säkerhet ska styra åtkomsten till BHOLD-kärnan.                                                                                                                | Markera kryssrutan. **Viktigt:** Installationen Miss kommer om den här kryss rutan inte är markerad.                                                                                                                                                                                                                   |
| **Domain**                                  | Anger den domän som innehåller det tjänst konto som du skapade när du installerade BHOLD-kärnan. Mer information finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Domän namnet anges automatiskt av guiden. Ändra bara namnet om det är felaktigt. **Viktigt:** Ange domän namnet med hjälp av NetBIOS-namnet (Short), inte det fullständigt kvalificerade domän namnet (FQDN). Om FQDN för domänen till exempel är fabrikam.com anger du domän namnet som FABRIKAM. |
| **Användare**                                    | Anger inloggnings namnet för BHOLD Core Service-användarkontot.                                                                                                                                                          | Skriv användar kontots namn här:                                                                                                                                                                                                                                                                                    |
| **Lösenord**                                | Anger lösen ordet för tjänstens användar konto.                                                                                                                                                                       | Skriv lösen ordet här: **viktigt:** se till att behålla det här lösen ordet på en dold, säker plats.                                                                                                                                                                                                                  |

**Säkerhetskopiera databas inställningar**

| Objekt                                        | Beskrivning                                                                                                                                                                                                                                                                                                                                                                                                                  | Värde                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Använd integrerad säkerhet**                 | Anger att Windows-autentisering används för att komma åt databasen.                                                                                                                                                                                                                                                                                                                                                        | Markera kryss rutan om Windows-autentisering används för att ansluta till SQL Server. Avmarkera kryss rutan om SQL Server autentisering används. Databasen måste ha skapats innan du kör BHOLD Core-installationen om SQL Server autentisering används. **Obs:** Om Windows-autentisering används måste du vara inloggad med ett konto som har rollen sysadmin-server på databas servern. **Viktigt:** Använd endast SQL Server autentisering i test miljöer. Microsoft rekommenderar starkt att du använder Windows-autentisering i produktions distributioner. |
| **Databas användare** och **databas lösen ord** | Anger användar namn och lösen ord för en användare med Server rollen sysadmin på databas servern. Dessa värden anges bara när SQL Server autentisering används.                                                                                                                                                                                                                                                  | Skriv SQL Server användar namn här: Skriv SQL Server användar lösen ord här: </br></br> **Viktigt:** Se till att behålla det här lösen ordet på en dold, säker plats.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Databas server** och **databas namn**   | Anger NetBIOS-namnet på databas servern och namnet på den säkerhets kopierings databas som skapas av installations programmet för BHOLD modell Generator. Om du inte använder standard instansen av databas servern anger du databas Server instansen i * \<form\>Server*\\*\<-instansen\>*.  Microsoft rekommenderar att du namnger säkerhets kopierings databasen med namnet på BHOLD Core-databasen följt av \_backup, till exempel B1_BACKUP. | Skriv Server namnet (eller server och instans) här: </br> Skriv databas namnet här:

## <a name="bhold-model-generator-setup"></a>Installation av BHOLD modell Generator

Om du vill installera modulen BHOLD modell Generator loggar du in som medlem i gruppen domän administratörer, laddar ned följande fil och kör den som administratör på den server som du vill installera BHOLD Core-modulen på:

- BholdModelGenerator * \<version\>*\_release. msi

Ersätt * \<version\> * med versions numret för den version av BHOLD modell generator som du installerar.

Om du vill köra program filen som administratör högerklickar du på filen och klickar sedan på **Kör som administratör**.

## <a name="next-steps"></a>Nästa steg

- Information om hur du skapar indatafiler [Microsoft BHOLD Suite Technical Reference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Installations guide för BHOLD](bhold-installation-guide.md)
- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
