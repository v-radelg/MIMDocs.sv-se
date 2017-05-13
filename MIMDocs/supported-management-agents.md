---
title: "Stöd för anslutningsprogram | Microsoft Docs"
description: "Använd anslutningsprogram för att hantera dataöverföring mellan MIM och dina kataloger."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.lasthandoff: 05/02/2017


---

# <a name="connect-to-your-directories"></a>Anslut till katalogerna

Länk till anslutningsprogram för speciellt anslutna datakällor till Microsoft Identity Manager (MIM). Ett anslutningsprogram flyttar data från en ansluten datakälla till MIM. När data i MIM ändras kan anslutningsprogrammet även exportera data till den anslutna datakällan för att hålla den synkroniserad med MIM. I allmänhet finns det mist ett anslutningsprogram för varje ansluten katalog.

I Forefront Identity Manager kallades anslutningsprogram för hanteringsagenter. Termen används fortfarande i vissa artiklar eller delar av produkten, men nu hänvisar båda termerna till samma begrepp.

Den här artikeln omfattar anslutningsprogrammen som ingår i MIM, men anslutningsprogrammet för Extensible Connectivity 2.0 gör det möjligt att ansluta med ännu fler datakällor. Vissa partner har skapat sina egna anslutningsprogram på det här sättet. En fullständig lista finns i wikin [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: Hanteringsagenter från partner.)

## <a name="supported-connectors-in-mim-2016"></a>Anslutningsprogram som stöds i MIM 2016

| Namn | Versioner som stöds av den anslutna datakällan |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| ADLDS (Active Directory Lightweight Directory Services) | ADLDS (Active Directory Lightweight Directory Services) |
| Active Directory Global Address List (GAL) | Active Directory Global Address List (GAL) – Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Alla anropsbaserade eller filbaserade datakällor |
| MIM-tjänst | Microsoft Docs 2016 |
| IBM DB2 Universal Database | IBM DB2 version 9.1, 9.5 eller 9.7; IBM DB2 OLEDB v9.5 FP5 eller v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory version 8.7.3, 8.8.5 och 8.8.6 |
| Oracle Database | Oracle Database 10 g eller 11 g: 64-bitarsklient |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Oracle (tidigare Sun och Netscape) Directory Servers | Sun Directory Server 6.x, 7.x och Oracle 11 |
| [Windows PowerShell Connector för FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 eller bättre |
| [Microsoft Azure Active Directory Connector för FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Allmän LDAP Connector för FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3-server (RFC 4510-kompatibel) |
| [Connector för Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes, version v8.0.x eller v8.5.x |
| [SharePoint Services Connector för FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint Server 2013 eller 2016 med UPA (User Profile Service Application) |
| [Connector för Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 eller 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| Textfil med attributvärdespar | Textfiler med attributvärdespar |
| Avgränsad textfil | Avgränsade textfiler |
| DSML (Directory Services Mark-up Language) | DSML 2.0 (Directory Services Markup Language) |
| Textfil med fast bredd | Textfiler med fast bredd |
| LDAP Data Interchange Format (LDIF) | LDAP Data Interchange Format (LDIF) |

## <a name="related-topics"></a>Närliggande information

[Hanteringsagenter i FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
