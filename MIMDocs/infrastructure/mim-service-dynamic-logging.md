---
title: MIM Service Dynamic Logging (Dynamisk loggning med MIM-tjänsten) | Microsoft Docs
description: Aktivera dynamisk loggning med MIM-tjänsten utan att behöva starta om hanteringstjänsten
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 69ebe774ddea0176fb26ef74b8f4352e4bb5d039
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042176"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Tjänsten MIM SP1 (4.4.1436.0) –  dynamisk loggning

Vi har introducerat en ny loggningsfunktion i 4.4.1436.0. Detta gör det möjligt för administratörer och supporttekniker att aktivera loggning utan att behöva starta om hanteringstjänsten.

När installationen är klar visas följande nya rad i Microsoft.ResourceManagement.Service.exe.config som heter

*   Rad 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Rad 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Rad 266 ``</system.diagnostics> ``

![Markerade avsnitt som visar de nya posterna med dynamisk loggning](media/mim-service-dynamic-logging/screen01.png)

Nivåerna för dynamisk loggning finns [här](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Critical (Kritisk) = standardnivåtjänst som endast skriver allvarliga händelser
- Uppdatera rad 8 (dynamicLogging mode="true" loggingLevel="Critical") med önskat loggningsvärde

Dynamisk loggnings konfiguration finns på rad 266: Microsoft. ResourceManagement. service. exe. config

![Markerade avsnitt visar rader med de olika tillgängliga loggningsområdena](media/mim-service-dynamic-logging/screen02.png)

Som standard är loggnings platsen på * * C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, FIM-tjänstkontot måste ha Skriv behörighet till den här platsen för att generera den dynamiska loggen.

![Mapplats för loggarna](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  I händelse av oväntade fel (syntaxfel i konfigurationsfilen Microsoft.ResourceManagement.Service.exe.config eller andra misstag) skrivs motsvarande felmeddelande i filen Microsoft.ResourceManagement.Service.exe_Emergency.log under följande sökväg %TMP% eller %TEMP% eller %USERPROFILE% (den första som finns).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Om du vill visa spårningen kan du använda [verktyget service spårnings visare](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Skärmbild av visningsverktyget för tjänstspårning](media/mim-service-dynamic-logging/screen04.png)

## <a name="updates-build-45xx-or-greater"></a>Uppdateringar: build 4.5. x. x eller senare

I build 4.5. x. x har vi ändrat loggnings funktionen för att ange standard loggnings nivån **"varning"**. Tjänsten skriver meddelanden i två filer ("00"-och "01"-index läggs till före tillägget). Filerna finns i katalogen C:\Program Files\Microsoft Forefront Identity Manager\2010\Service. När filen överskrider max storleken börjar tjänsten skriva i en annan fil. Om det finns en annan fil kommer den att skrivas över. Den maximala standard storleken för filen är 1 GB. Om du vill ändra den maximala standard storleken är det nödvändigt att lägga till parametern **"maxOutputFileSizeKB"** med värdet Max fil storlek i KB till lyssnare (se exemplet nedan) och starta om MIM-tjänsten. När tjänsten startas lägger den till loggar i mer nyligen använda fil (om gränsen för utrymme överskrids skrivs den äldsta filen över). 

> [!NOTE] 
> Eftersom fil storleken för tjänst kontrollen innan meddelandet skrivs, kan fil storleken vara större än max storleken för ett meddelandes storlek. som standard kan logg storleken vara cirka 6 GB (tre >lyssnare med två filer för en storlek på GB).

> [!NOTE] 
> Tjänst kontot måste ha behörighet att skriva i > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" >Directory. Om tjänst kontot inte har sådana rättigheter kommer >-filerna inte att skapas.

Exempel på hur du ställer in maximal fil storlek på 200 MB (200 * 1024 KB) för svclog-filer och 100 MB * (100 * 1024 KB) för txt-filer

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
