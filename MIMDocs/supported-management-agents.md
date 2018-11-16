---
title: Stöd för anslutningsprogram | Microsoft Docs
description: Använd anslutningsprogram för att hantera dataöverföring mellan MIM och dina anslutna datakällor.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 4c39b5e3ba651464989bd0bc1f600629c7e89a0f
ms.sourcegitcommit: 3b514aba69af203f176b40cdb7c2a51c477c944a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/16/2018
ms.locfileid: "51718240"
---
# <a name="connect-to-your-directories"></a>Anslut till katalogerna

Länk till anslutningsprogram för speciellt anslutna datakällor till Microsoft Identity Manager SP1 (MIM). Ett anslutningsprogram flyttar data från en ansluten datakälla till MIM. När data i MIM ändras kan anslutningsprogrammet även exportera data till den anslutna datakällan för att hålla den synkroniserad med MIM. I allmänhet finns det mist ett anslutningsprogram för varje ansluten katalog.

I Forefront Identity Manager kallades anslutningsprogram för hanteringsagenter. Termen används fortfarande i vissa artiklar eller delar av produkten, men nu hänvisar båda termerna till samma begrepp.

Den här artikeln omfattar anslutningsprogrammen som ingår & stöds i MIM, men anslutningen för Extensible Connectivity 2.0 gör det möjligt att ansluta med ännu fler datakällor. Vissa partner har skapat sina egna anslutningsprogram på det här sättet. En fullständig lista finns i wikin [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: Hanteringsagenter från partner.)

## <a name="supported-connectors-in-mim-2016-sp1"></a>Anslutningsprogram som stöds i MIM 2016 SP1

| Namn | Versioner som stöds av den anslutna datakällan & teknisk länkar |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory-2012, 2016 |
| ADLDS (Active Directory Lightweight Directory Services) | ADLDS (Active Directory Lightweight Directory Services) |
| Active Directory Global Address List (GAL) | Active Directory globala adresslistan (GAL) – Exchange 2013, 2016 |
| Extensible Connectivity 2.0 | Alla anropsbaserade eller filbaserade datakällor |
| FIM-tjänst | FIM-tjänstens hanteringsagent (synkroniseringstjänsten) måste finnas på samma version av ”Forefront Identity Manager-tjänsten” installerat |
| IBM DB2 Universal Database | 9.5 eller 9.7; IBM DB2-version IBM DB2 OLEDB v9.5 FP5 eller v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory version 8.7.3, 8.8.5 och 8.8.6 |
| Oracle Database | Oracle Database 10 g eller 11 g: 64-bitarsklient |
| Microsoft SQL Server | SQLServer 2012, 2014, 2016 |
| Oracle (tidigare Sun och Netscape) Directory Servers | Sun Directory Server 6.x, 7.x och Oracle 11 |
| [Windows PowerShell Connector för FIM 2010 R2](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 eller bättre |
| [Microsoft Azure Active Directory Connector för FIM 2010 R2](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Allmän LDAP Connector för FIM 2010 R2](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-server (RFC 4510-kompatibel)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Allmän SQL Connector för FIM 2010 R2 / MIM](./reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Anslutningen stöds med alla 64-bitars ODBC-drivrutiner](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql.md) |
| [Connector för Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes-versionen v8.5.x |
| [SharePoint Services Connector UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013 eller 2016 med UPA (User Profile Service Application) |
| [Connector för Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 eller 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Textfil med Attributvärdespar](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Textfiler med attributvärdespar |
| [Avgränsad textfil](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Avgränsade textfiler |
| [Directory Services Mark-up Language (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | DSML 2.0 (Directory Services Markup Language) |
| [Textfil med fast bredd](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Textfiler med fast bredd |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |

## <a name="related-topics"></a>Närliggande information

[Hanteringsagenter i FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
