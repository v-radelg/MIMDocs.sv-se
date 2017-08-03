---
title: PAM-programvarukrav | Microsoft Docs
description: "Hitta kraven för maskinvara och programvara för distribution av Privileged Access Management"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2985215821db843d2f90d8a34250a8ca6a84b592
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="hardware-and-software-requirements"></a>Maskin- och programvarukrav

Privileged Access Management har inga maskinvarukrav förutom för de underliggande programvaruplattformarna. Kontrollera att du har tillräckligt med minne och diskutrymme, och är ansluten till nätverket.

Den här artikeln innehåller minimikraven för en grundläggande distribution. Den har inte som syfte att illustrera prestanda, skalbarhet och hög tillgänglighet och ska inte ses som en rekommenderad distributionstopologi för stora företag och produktionsmiljöer.

## <a name="installing-from-software-packages"></a>Installera från programvarupaket

Följande programvara kan laddas ned från TechNet Evaluation Center eller MSDN:  
- Microsoft Identity Manager 2016
  - Tjänst och portal: innehåller installationsprogrammet för MIM-tjänsten, MIM-portalen och PAM-scenariot
  - Tillägg: innehåller installationsprogrammet för PowerShell-cmdletar för begäran

Följande programvara kan laddas ned från GitHub:  
- PAMSamplePortal: innehåller exempelwebbapp för REST-API:t

## <a name="required-software"></a>Programvara som krävs

- Windows Server 2012 R2  
- Windows 8.1 Enterprise eller Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 eller SQL Server 2014  

## <a name="evaluation-software"></a>Utvärderingsprogramvara

Om du inte har licenser för Windows, SQL Server eller Windows Server kan du ladda ned utvärderingsversioner.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### <a name="microsoft-download-center"></a>Microsoft Download Center

- [SQLServer](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 och dess krav](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Maskinvarukrav

För PAM-komponenter hänvisas du till systemkraven för programvaruprodukterna.

För CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) eller tidigare

For CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

För PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

För PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) eller [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)
