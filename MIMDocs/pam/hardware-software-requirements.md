---
title: Maskin- och programvarukrav | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 77e7174e94ea8032c4e57155db489f493ce18177


---

Inga maskinvarukrav utöver de för underliggande programvaruplattformar, tillräckligt minne eller diskutrymme och nätverksanslutning. Den här artikeln innehåller minimikraven för en grundläggande distribution. Den har inte som syfte att illustrera prestanda, skalbarhet och hög tillgänglighet och ska inte ses som en rekommenderad distributionstopologi för stora företag och produktionsmiljöer.

## Installera från programvarupaket

Följande programvara kan laddas ned från TechNet Evaluation Center eller MSDN:  
- Microsoft Identity Manager 2016
  - Tjänst och portal: innehåller installationsprogrammet för MIM-tjänsten, MIM-portalen och PAM-scenariot
  - Tillägg: innehåller installationsprogrammet för PowerShell-cmdletar för begäran

Följande programvara kan laddas ned från GitHub:  
- PAMSamplePortal: innehåller exempelwebbapp för REST-API:t

## Programvara som krävs

- Windows Server 2012 R2  
- Windows 8.1 Enterprise eller Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 eller SQL Server 2014  

## Utvärderingsprogramvara

Om du inte har licenser för Windows, SQL Server eller Windows Server kan du ladda ned utvärderingsversioner.

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8,1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft Download Center

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 och dess krav](https://www.microsoft.com/download/details.aspx?id=42039)

## Maskinvarukrav

För PAM-komponenter hänvisas du till systemkraven för programvaruprodukterna.

För CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) eller tidigare

For CORPWKSTN:  
- [Windows 8,1](http://windows.microsoft.com/windows-8/system-requirements)

För PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

För PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) eller [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jul16_HO2-->


