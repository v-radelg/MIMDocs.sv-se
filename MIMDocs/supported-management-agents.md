---
title: Stöd för anslutningsprogram | Microsoft Docs
description: Använd kopplingar för att hantera data överföring mellan MIM och dina anslutna data källor.
keywords: ''
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2019
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b189d80053dd357874c57a461553205d3394ab04
ms.sourcegitcommit: 32c7a46b2f8ed3f2f9ebc6f79a4ecb0019fe62e0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77527913"
---
# <a name="connect-to-your-directories"></a>Anslut till katalogerna

Kopplingarna länkar vissa anslutna data källor till Microsoft Identity Manager SP1 (MIM). Ett anslutningsprogram flyttar data från en ansluten datakälla till MIM. När data i MIM ändras kan anslutningsprogrammet även exportera data till den anslutna datakällan för att hålla den synkroniserad med MIM. I allmänhet finns det mist ett anslutningsprogram för varje ansluten katalog.

I Forefront Identity Manager kallades anslutningsprogram för hanteringsagenter. Termen används fortfarande i vissa artiklar eller delar av produkten, men nu hänvisar båda termerna till samma begrepp.

Den här artikeln beskriver de anslutningar som ingår & som stöds i MIM, men anslutningen för utöknings bar anslutning 2,0 gör det möjligt att ansluta till ännu fler data källor. Vissa partner har skapat sina egna anslutningsprogram på det här sättet. En fullständig lista finns i wikin [FIM 2010: Management Agents from Partners](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: Hanteringsagenter från partner.)

## <a name="supported-connectors-in-mim-2016-sp1"></a>Anslutningar som stöds i MIM 2016 SP1

| Name | Versioner som stöds av den anslutna data källan & tekniska länkar |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory i Windows Server 2012-2019 |
| ADLDS (Active Directory Lightweight Directory Services) | ADLDS (Active Directory Lightweight Directory Services) |
| Active Directory Global Address List (GAL) | Active Directory global adress lista (GAL) – Exchange 2013-2019 |
| Extensible Connectivity 2.0 | Alla anropsbaserade eller filbaserade datakällor |
| FIM-tjänst | MIM-tjänst. Observera att MIM-synkroniseringstjänsten och MIM-tjänsten måste vara av samma version. |
| IBM DB2 Universal Database | IBM DB2 version 9,5 eller 9,7; IBM DB2 OLEDB v 9.5 FP5 eller v 9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory version 8.7.3, 8.8.5 och 8.8.6 |
| Oracle Database | Oracle Database 10 g eller 11 g: 64-bitarsklient |
| Microsoft SQL Server | SQL Server 2012-2017 |
| Oracle (tidigare Sun och Netscape) Directory Servers | Sun Directory Server 6.x, 7.x och Oracle 11 |
| [Windows PowerShell-anslutning](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 eller bättre |
| [Microsoft Azure Active Directory koppling](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (rekommenderas inte för nya distributioner) |
| [Allmän LDAP-anslutning](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3-Server (RFC 4510-kompatibel)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Allmän SQL-anslutning](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Anslutningen stöds med alla 64-bitars ODBC-drivrutiner](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Connector för Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes, version v 8.5. x, v 9.0. x |
| [SharePoint Services Connector-UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013-2019 med användar profil tjänst program (UPA) |
| [Connector för Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5,0 eller 6,0; Oracle, 9,1; Oracle eBusiness 12,1 och andra SOAP-och REST-API: er](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Text filen för attribut-värdepar](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Textfiler med attributvärdespar |
| [Avgränsad textfil](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Avgränsade textfiler |
| [DSML (Directory Services mark-up Language)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | DSML 2.0 (Directory Services Markup Language) |
| [Textfil med fast bredd](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Textfiler med fast bredd |
| [LDIF (LDAP Data Interchange Format)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Microsoft Graph koppling](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Närliggande information

[Hanteringsagenter i FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
