---
title: "Programvaruplattformar som stöds | Microsoft Docs"
description: "Sök efter produkter och versioner som är kompatibla med respektive MIM 2016-komponent."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f1faed3a09023e288a68a8950dae43725f19eb3e
ms.openlocfilehash: 33c84afa4d6fd2ed7bde33de39dd151f83f07fd4
ms.lasthandoff: 04/12/2017


---

# <a name="supported-platforms-for-mim-2016"></a>Plattformar som stöds för MIM 2016

I den här tabellen beskrivs de plattformar som stöds och versionen för respektive komponent i Microsoft Identity Manager 2016. Versioner som är märkta med * stöds endast i MIM 2016 Service Pack 1.


| **MIM-komponent** | **Plattform** | **Version** |
|-------------------|--------------|-------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | MIM Sync-databas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory för användaretablering, PCNS och GAL Sync (valfritt)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange för etablering av postlådor och GAL Sync (valfritt)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Utvecklingsmiljö (valfritt) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Ytterligare anslutna system (valfritt) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 eller senare<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Andra produkter från tredje part |
| **MIM-tjänsten** (förutom PAM-scenario) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM-tjänstdatabas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Exchange för MIM-tjänstgodkännande och hantering av grupp-e-post (valfritt) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (endast meddelande) |
| **MIM-tjänst och -portal** (endast PAM-scenario)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory för PAM-skog i bastionmiljö | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory för befintliga skogar | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM-tjänstdatabas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | E-postserver för MIM-tjänstgodkännande och hantering av grupp-e-post (valfritt) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (endast meddelande) |
| | Webbläsare | Alla större webbläsare |
| **MIM-tjänstrapportering** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | Informationslager | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (med 4.4.1459)<br/> [SQL Server-versionskompatibilitet för System Center 2016](https://technet.microsoft.com/en-us/system-center-docs/system-requirements/sql-server-version-compatibility)
 |
| **MIM-portalerna för återställning och registrering av lösenord** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Webbläsare | Alla större webbläsare |
| **MIM-tillägg** | Windows | Windows 7<br/>Windows 8<br/>Windows 8,1<br/>Windows 10 |
| | Outlook-integrering (valfritt) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (i Windows 10) * |
| | PAM PowerShells begärande-cmdlet:ar (valfritt) | Windows 8,1<br/>Windows 10 |
| **Certifikathantering i MIM** (server och integrering med certifikatutfärdare (CA)) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Certifikatutfärdare | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM-databas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Certifikathantering i MIM** (program) | Windows | Windows 8<br/>Windows 8,1<br/>Windows 10 |
| **Certifikathantering i MIM** (klient och bulkklient) | Windows | Windows 7 |
| **MIM BHOLD-programsvit** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD-databas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | E-postserver (valfritt) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webbläsare | Internet Explorer 7, 8, 9, 10 eller 11 med Silverlight |

