---
# required metadata

title: Plattformar som stöds för MIM 2016 | Microsoft Identity Manager
description: Sök efter produkter och versioner som är kompatibla med respektive MIM 2016-komponent.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Plattformar som stöds för MIM 2016

| **MIM-komponent** | **Plattform** | **Version** |
|-------------------|--------------|-------------|
|**MIM Sync**|Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2|
||MIM Sync-databas |SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
||Active Directory för användaretablering, PCNS och GAL Sync (valfritt)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
||Exchange för etablering av postlådor och GAL Sync (valfritt)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| Utvecklingsmiljö (valfritt) | Visual Studio 2012<br/>Visual Studio 2013 |
|| Ytterligare anslutna system (valfritt) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 eller senare<br/>SharePoint Server 2013<br/>Andra produkter från tredje part |
| **MIM-tjänsten** (förutom PAM-scenario) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
|| MIM-tjänstdatabas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
|| Exchange för MIM-tjänstgodkännande och hantering av grupp-e-post (valfritt) | Exchange Server 2007 SP3 (där Exchange management console installerats)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
| **MIM-tjänst och -portal** (endast PAM-scenario)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 |
|| Active Directory för PAM-skog i bastionmiljö | Windows Server 2012 R2 |
|| Active Directory för befintliga skogar | Windows Server 2008 eller senare |
|| MIM-tjänstdatabas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
|| SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 |
|| E-postserver för MIM-tjänstgodkännande och hantering av grupp-e-post (valfritt) | Exchange Server 2007 SP3 (där Exchange management console installerats)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| Webbläsare | Internet Explorer 8, 9, 10 eller 11 |
| **MIM-tjänstrapportering** | Windows Server | Windows Server 2012 |
|| Informationslager | System Center 2012 Service Manager SP1 |
|| Informationslagerdatabas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
| **MIM-portalerna för återställning och registrering av lösenord** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
|| Webbläsare | Internet Explorer 7, 8, 9, 10 eller 11<br/>Andra webbläsare |
| **MIM-tillägg** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
|| Outlook-integrering (valfritt) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 |
|| PAM PowerShells begärande-cmdlet:ar (valfritt) | Windows 8.1<br/>Windows 10 |
| **Certifikathantering i MIM** (server och integrering med certifikatutfärdare (CA)) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| Certifikatutfärdare | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
|| MIM CM-databas | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
| **Certifikathantering i MIM** (program) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Certifikathantering i MIM** (klient och bulkklient) | Windows | Windows 7 |
| **MIM BHOLD-programsvit** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| BHOLD-databas | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
|| E-postserver (valfritt) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| Webbläsare | Internet Explorer 7, 8, 9, 10 eller 11 med Silverlight |


<!--HONumber=Apr16_HO4-->


