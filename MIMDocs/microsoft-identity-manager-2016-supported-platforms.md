---
title: Programvaruplattformar som stöds | Microsoft Docs
description: Sök efter produkter och versioner som är kompatibla med respektive MIM 2016-komponent.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: d4cb70ae60d23049251caa121a1834eeacb05677
ms.sourcegitcommit: 4c4bc7aa42cd5984c838abdd302490355ddcb4ea
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238920"
---
# <a name="supported-platforms-for-mim-2016"></a>Plattformar som stöds för MIM 2016

I den här tabellen beskrivs de plattformar som stöds och versionen för respektive komponent i Microsoft Identity Manager 2016. De versioner som marker ATS med en * stöds bara i MIM 2016 service pack 1 med den senaste snabb korrigeringen.  De versioner som har marker ATS med "NR", som inte rekommenderas, stöds men rekommenderas inte om du startar en ny distribution av plattformen för MIM.


| **MIM-komponent** | **Plattform** | **Version** |
|-------------------|--------------|--------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Active Directory funktions nivå för användar etablering, PCNS och GAL-synkronisering | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | MIM Sync-databas | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| | Active Directory för användar etablering, PCNS och GAL-synkronisering (valfritt)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange för etablering av postlådor och GAL Sync (valfritt)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 * |
| | Utvecklingsmiljö (valfritt) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Ytterligare anslutna system (valfritt) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 eller senare<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Andra produkter från tredje part |
| **MIM-tjänst och portal** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM-scenario:  Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |PAM-scenario: Active Directory för PAM-skog i bastionmiljö | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |PAM-scenario: Active Directory för befintliga (CORP) skogar för PAM-scenario | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM-tjänstdatabas | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016 * |
| | E-postserver för MIM-tjänstgodkännande och hantering av grupp-e-post (valfritt) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (endast meddelande innan build- [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | Alla huvud webbläsare som stöds * (begränsat mobila enheter)|
| **MIM-tjänstrapportering** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Informationslager | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (med 4.4.1459)<br/> [SQL Server-versionskompatibilitet för System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016) |
| **MIM-portalerna för återställning och registrering av lösenord** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Webbläsare | Alla huvud webbläsare som stöds |
| **MIM-tillägg** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook-integrering (valfritt) | Outlook 2010 (i Windows, förutom Klicka-och-kör)<br/>Outlook 2013 (i Windows, förutom Klicka-och-kör) <br/> Outlook 2016 (i Windows 10, förutom Klicka-och-kör) * |
| | PAM PowerShells begärande-cmdlet:ar (valfritt) | Windows 8.1<br/>Windows 10 |
| **Certifikathantering i MIM** (server och integrering med certifikatutfärdare (CA)) | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Certifikatutfärdare | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM-databas | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| **Certifikathantering i MIM** (program) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Hantering av MIM-certifikat** (Bulk-klient) | Windows | Windows 7 |
| **Hantering av MIM-certifikat** (Klientens ActiveX-baserade smartkort) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD-programsvit** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD-databas | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | E-postserver (valfritt) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Webbläsare | Webbläsare som stöds i Internet Explorer med Silverlight |
