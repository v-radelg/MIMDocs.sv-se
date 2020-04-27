---
title: Uppgradera från FIM 2010 R2 och MIM 2016 SP2 till Microsoft Identity Manager 2016 Service Pack 2 | Microsoft Docs
description: Lär dig hur du uppgraderar dina FIM 2010 R2-eller MIM 2016 SP2-komponenter och sedan installerar de komponenter som är nya i MIM 2016.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bfe604795d44cdea61fe0d10bc2943f9b8433784
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044163"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>MIM 2016 SP2-uppgradering från Forefront Identity eller Microsoft Identity Manager

Organisationer kan uppgradera till Microsoft Identity Manager 2016 SP2 från tidigare versioner av Microsoft Identity Manager eller Forefront Identity Manager.  I varje avsnitt i den här artikeln beskrivs en uppgraderings väg som stöds.

Det finns flera tillgängliga uppgraderings alternativ. Om du redan kör MIM 2016 och inte behöver uppdatera den underliggande plattformen (Windows Server, SQL, SharePoint, SCSM DW) eller köra MIM-tjänster med hjälp av grupphanterade tjänst konton och du inte använder MIM-språkpaket, är det enklaste alternativet uppgradering/Hotfix (. msp) på plats. Annars rekommenderas den fullständiga installationen.

> [!IMPORTANT]
> Kontrol lera avsnittet uppdaterade [program varu krav](prepare-server-ws2016.md#software-prerequisites) innan du installerar MIM 2016 SP2

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>Uppgradera från FIM 2010 R2 SP1 eller senare FIM-versioner

> [!NOTE]
> Den minimala versionen av Forefront Identity Manager som kan uppgraderas direkt till MIM 2016 SP2 är FIM 2010 R2 SP1 (build 4.1.3419.0). Direkt uppgradering till MIM 2016 SP2 från tidigare versioner av FIM stöds inte. Om du kör FIM-versioner som är äldre än 4.1.3419.0 måste du uppgradera till FIM 2010 R2 SP1 innan du uppgraderar till MIM 2016 SP2.

1. **Alternativ 1: fullständig installation med befintliga databaser**
    1. Gör en säkerhets kopia av dina FIMSynchronizationService-och FIMService-databaser.
    1. Exportera alla FIM-RCDC objekt och RCDC resurs strängar som du har gjort ändringar i.
    1. Exportera krypterings nycklar för synkroniseringstjänsten.
    1. Backup MIISServer. exe. config, Sync Server "Extensions"-mappen, Microsoft. ResourceManagement. service. exe. config som MIM Installer kan skriva över anpassade ändringar i konfigurationsfiler.
    1. Avinstallera **alla** FIM-komponenter, inklusive språk paket (som ska avinstalleras först).
    1. *Valfritt:* Flytta FIM-databaser till en annan SQL Server. Vi rekommenderar att du skapar SQL-alias på MIM-servrar och använder dessa alias i stället för verkliga SQL Server-namn för att under lätta MIM databaser-migreringen i framtiden.
    1. Installera MIM 2016 SP2 från. ISO-installationsmedia på samma eller på en annan server som följer [MIM Deployment Guide](microsoft-identity-manager-deploy.md), Välj att använda befintliga databaser när du uppmanas till det, ange tidigare exporterade krypterings nycklar för synkroniseringstjänsten.
    1. Granska filerna MIISServer. exe. config och Microsoft. ResourceManagement. service. exe. config för saknade .net-omdirigeringar eller anpassade avsnitt som du har lagt till.
    1. Installera MIM 2016 SP2-språk paket om det behövs.
    1. Skicka anpassningar på nytt till RCDC-objekt och RCDC-resurs strängar och lokaliseringar om det behövs.
    1. Uppgradera FIM-tilläggen och lösen ords återställnings klienter, ange namnet på den nya MIM-servern om MIM-tjänstens Server namn har ändrats.
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>Uppgradera från tidigare MIM 2016-versioner
1. Gör en säkerhets kopia av dina FIMSynchronizationService-och FIMService-databaser.
1. Exportera alla anpassade FIM-tjänstelokaliseringar som görs till RCDC-objekt och RCDC-resurs strängar.
1. Exportera krypterings nycklar för synkroniseringstjänsten.
1. Backup MIISServer. exe. config, Sync Server "Extensions"-mappen, Microsoft. ResourceManagement. service. exe. config som MIM Installer kan skriva över anpassade ändringar i konfigurationsfiler.
1. Avinstallera MIM-språkpaket om det används.
1. Stoppa MIM-tjänster.
1. **Alternativ 1: uppgradering på plats – installation av snabb korrigeringar**
    1. Installera [snabb korrigering](https://www.microsoft.com/download/details.aspx?id=100412) för MIM 2016 SP2-synkroniseringstjänsten
    1. Installera MIM 2016 SP2 service [Hotfix](https://www.microsoft.com/download/details.aspx?id=100412)
    1. Granska filerna MIISServer. exe. config och Microsoft. ResourceManagement. service. exe. config för saknade .net-omdirigeringar eller anpassade avsnitt som måste läggas till.
    1. Installera MIM 2016 SP2-språk paket om det behövs.
    1. Skicka anpassningar på nytt till RCDC-objekt och RCDC-resurs strängar och lokaliseringar om det behövs.
    1. Uppgradera MIM 2016-tillägg och lösen ords återställnings klienter.
1. **Alternativ 2: fullständig installation med befintliga databaser**
    1. Avinstallera **alla** MIM-komponenter.
    1. *Valfritt:* Flytta FIM-databaser till en annan SQL Server. Vi rekommenderar att du skapar SQL-alias på MIM-servrar och använder dessa alias i stället för verkliga SQL Server-namn för att under lätta MIM databaser-migreringen i framtiden.
    1. Installera MIM 2016 SP2 från. ISO-installationsmedia på samma eller på en annan server som följer [MIM Deployment Guide](microsoft-identity-manager-deploy.md), Välj att använda befintliga databaser när du uppmanas till det, ange tidigare exporterade krypterings nycklar för synkroniseringstjänsten.
    1. Granska filerna MIISServer. exe. config och Microsoft. ResourceManagement. service. exe. config för saknade .net-omdirigeringar eller anpassade avsnitt som måste läggas till.
    1. Installera MIM 2016 SP2-språk paket om det behövs.
    1. Skicka anpassningar på nytt till RCDC-objekt och RCDC-resurs strängar och lokaliseringar om det behövs.
    1. Uppgradera MIM 2016-tillägg och lösen ords återställnings klienter, ange ett nytt MIM-servernamn om MIM-tjänstens Server namn har ändrats.

> [!NOTE]
> Uppdateringar av språk paket efter MIM 2016 SP2 distribueras som snabb korrigeringar (. msp-filer), vilket eliminerar behovet av att avinstallera/ominstallera språk paket.

Mer detaljerad information om säkerhets kopierings procedurerna för uppgradering och databaser finns i artikeln [Uppgradera till fim 2010 R2](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29) , som gäller för uppgraderings processen för FIM eller mim.
