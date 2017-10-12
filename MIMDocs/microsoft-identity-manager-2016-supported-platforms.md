---
title: "Programvaruplattformar som stöds | Microsoft Docs"
description: "Sök efter produkter och versioner som är kompatibla med respektive MIM 2016-komponent."
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 10/5/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: dd93c5bd4439aa6a485e1fcd6eaff970f07249b3
ms.sourcegitcommit: ce027818d34f170b7bd216337751c646e0eb8ced
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2017
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
| | Ytterligare anslutna system (valfritt) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 eller senare<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Andra produkter från tredje part |
| **MIM-tjänst och portal** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM-Scenario: Windows Server | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM-Scenario: Active Directory för skyddsmiljön PAM-skog | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM-Scenario: Active Directory för PAM scenariot befintliga (CORP) skogar | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM-tjänstdatabas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | E-postserver för MIM-tjänstgodkännande och hantering av grupp-e-post (valfritt) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (endast meddelande) |
| | Webbläsare | Alla större webbläsare * (endast de mobila enheter)|
| **MIM-tjänstrapportering** | Windows Server |  Windows Server 2008 R2 SP1<br/>Windows Server 2012 <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Informationslager | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (med 4.4.1459)<br/> [SQL Server-versionskompatibilitet för System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **MIM-portalerna för återställning och registrering av lösenord** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Webbläsare | Alla större webbläsare |
| **MIM-tillägg** | Windows | Windows 7<br/>Windows 8<br/>Windows 8,1<br/>Windows 10 |
| | Outlook-integrering (valfritt) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (i Windows 10) * |
| | PAM PowerShells begärande-cmdlet:ar (valfritt) | Windows 8,1<br/>Windows 10 |
| **Certifikathantering i MIM** (server och integrering med certifikatutfärdare (CA)) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Certifikatutfärdare | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM-databas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Certifikathantering i MIM** (program) | Windows | Windows 8<br/>Windows 8,1<br/>Windows 10 |
| **Certifikathantering i MIM** (Massredigera klienten) | Windows | Windows 7 |
| **Certifikathantering i MIM** (Client ActiveX baserat smartkort) | Windows | Windows 7 </br> Windows 8 </br> Windows 8,1 </br> Windows 10 |
| **MIM BHOLD-programsvit** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD-databas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | E-postserver (valfritt) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webbläsare | Internet Explorer 7, 8, 9, 10 eller 11 med Silverlight |
