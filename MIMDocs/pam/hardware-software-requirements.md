---
title: PAM-programvarukrav | Microsoft Docs
description: Hitta kraven för maskinvara och programvara för distribution av Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 79f9eeabdd9ac9206c4232217c7cbcdf870c3a3a
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043995"
---
# <a name="hardware-and-software-requirements"></a>Maskin- och programvarukrav

Privileged Access Management har inga maskinvarukrav förutom för de underliggande programvaruplattformarna. Kontrollera att du har tillräckligt med minne och diskutrymme, och är ansluten till nätverket.

> [!IMPORTANT]
> Den här artikeln innehåller minimikraven för en grundläggande distribution. Den är inte avsedd att demonstrera prestanda, skalbarhet eller hög tillgänglighet. Den representerar inte en rekommenderad distributions topologi för stora företag eller produktions miljöer.

## <a name="installing-from-software-packages"></a>Installera från programvarupaket

Följande programvara kan laddas ned från TechNet Evaluation Center eller MSDN:

- Microsoft Identity Manager 2016
  - Tjänst och portal: innehåller installationsprogrammet för MIM-tjänsten, MIM-portalen och PAM-scenariot
  - Tillägg: innehåller installationsprogrammet för PowerShell-cmdletar för begäran

Följande programvara kan laddas ned från GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): innehåller ett exempel webb program för REST API

## <a name="required-software"></a>Programvara som krävs

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 eller SQL Server 2014

## <a name="evaluation-software"></a>Utvärderingsprogramvara

Om du inte har licenser för Windows, SQL Server eller Windows Server kan du ladda ned utvärderingsversioner.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Microsoft Download Center

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 och dess krav](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Maskinvarukrav

För PAM-komponenter hänvisas du till systemkraven för programvaruprodukterna.

För CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) eller tidigare

For CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

För PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

För PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) eller [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
