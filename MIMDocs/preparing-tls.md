---
title: Planera Microsoft Identity Manager 2016 i TLS 1,2-miljö | Microsoft Docs
description: Planera Microsoft Identity Manager 2016 i TLS 1,2-miljö
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4e208e2f42c206ae50febefb6a8206dc3823e084
ms.sourcegitcommit: c9f5f960fd39745bf5b57161a2fd0238c88d035a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133540"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>Planera MIM 2016 SP2 i miljöer med TLS 1,2 eller FIPS-läge


> [!IMPORTANT]
> Den här artikeln gäller endast MIM 2016 SP2

När du installerar MIM 2016 SP2 i den låsta miljön som har alla krypterings protokoll men TLS 1,2 inaktiverat gäller följande krav:
- För att upprätta säker TLS 1,2-anslutning MIM-komponenter krävs de senaste uppdateringarna för Windows Server och .NET Framework som aktiverar TLS 1,2-stöd i .NET 3,5 Framework för installation. Beroende på Server konfigurationen *kan* du behöva [aktivera SystemDefaultTlsVersions för .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) och/eller [inaktivera alla SCHANNEL-protokoll utom TLS 1,2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) i registret i *klient* -och *Server* under nycklar.

## <a name="mim-synchronization-service-sql-ma"></a>MIM-synkroniseringstjänst, SQL MA

- För att upprätta en säker TLS 1,2-anslutning med SQL Server krävs [SQL Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) eller senare i MIM-synkroniseringstjänsten och inbyggd SQL-hanteringsserver.

## <a name="mim-service"></a>MIM-tjänst
   >[!NOTE]
   >MIM 2016 SP2 obevakad installation Miss lyckas i miljön TLS 1,2. Installera MIM-tjänsten i interaktivt läge eller, om du installerar obevakad, se till att TLS 1,1 är aktiverat. När en obevakad installation har slutförts ska du framtvinga TLS 1,2 om det behövs.

- Självsignerade certifikat kan inte användas av MIM-tjänsten i endast en miljö med TLS 1,2. Välj starkt kryptering kompatibelt certifikat utfärdat av en betrodd certifikat utfärdare när du installerar MIM-tjänsten.
- Installations programmet för MIM-tjänsten kräver dessutom [OLE DB driv rutin för SQL Server version 18,2](https://www.microsoft.com/download/details.aspx?id=56730) eller senare.

## <a name="fips-mode-considerations"></a>Överväganden för FIPS-läge

Om du installerar MIM-tjänsten på en server där FIPS-läge är aktiverat måste du inaktivera verifiering av FIPS-principer för att tillåta att MIM-tjänstearbets flöden körs. Det gör du genom att lägga till *enforceFIPSPolicy Enabled = false* element i avsnittet *runtime* i *Microsoft.ResourceManagement.Service.exe.config* -filen mellan *runtime* -och *assemblyBinding* -avsnitten som visas nedan:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    
