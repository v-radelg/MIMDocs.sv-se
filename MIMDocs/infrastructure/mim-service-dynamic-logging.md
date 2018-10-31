---
title: MIM Service Dynamic Logging (Dynamisk loggning med MIM-tjänsten) | Microsoft Docs
description: Aktivera dynamisk loggning med MIM-tjänsten utan att behöva starta om hanteringstjänsten
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.openlocfilehash: e5d8bcc640ad77b71a515b13bcb3bcf6985654f5
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380093"
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

Dynamisk loggning finns på rad 266: Microsoft.ResourceManagement.Service.exe.config

![Markerade avsnitt visar rader med de olika tillgängliga loggningsområdena](media/mim-service-dynamic-logging/screen02.png)

Som standard den loggningsplatsen på den ** C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, The FIM-tjänstens konto behöver skrivbehörighet för den här platsen att generera den dynamiska loggen.

![Mapplats för loggarna](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  I händelse av oväntade fel (syntaxfel i konfigurationsfilen Microsoft.ResourceManagement.Service.exe.config eller andra misstag) skrivs motsvarande felmeddelande i filen Microsoft.ResourceManagement.Service.exe_Emergency.log under följande sökväg %TMP% eller %TEMP% eller %USERPROFILE% (den första som finns).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Om du vill visa spårningen kan du använda den [visningsverktyget för tjänstspårning](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Skärmbild av visningsverktyget för tjänstspårning](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Uppdateringar: Skapa 4.5.x.x eller större

I bygger 4.5.x.x som vi har ändrat loggningsfunktionen om du vill ange Standardloggning är **”varning”**. Tjänsten skriver meddelanden i två filer (”00” och ”01” index har lagts till innan tillägget). Filerna som finns i ”C:\Program Files\Microsoft Forefront Identity Manager\2010\Service” katalog. När filen är större än maxstorleken skriva startar tjänsten i en annan fil. Om det finns en annan fil, kommer att skrivas över. Maximal standardstorlek för filen är 1 GB. Om du vill ändra Maximal standardstorlek, är det nödvändigt att lägga till **”maxOutputFileSizeKB”** parametern med värdet för maximal filstorlek i KB till lyssnare (se exemplet nedan) och starta om MIM-tjänsten. När tjänsten startas, läggs loggar i nyare fil (om gränsen utrymme överskrids den skriva över den äldsta filen). 

> [!NOTE] 
> Som tjänsten kontrollera filstorleken innan meddelandet skrivs kan storleken på filen vara större än maxstorleken för storleken på ett meddelande. som standard storleken på loggarna kan vara ungefär 6 GB (tre > lyssnare med två filen för en GB storlek).

> [!NOTE] 
> Kontot ska ha behörighet att skriva i > ”C:\Program Files\Microsoft Forefront Identity Manager\2010\Service” > directory. Om kontot inte har sådana rättigheter den > kommer inte att skapa filer.

Exempel hur du ställer in maximala filstorleken till 200 MB (200 * 1024 KB) för svclog filer och 100 MB * (100 * 1024 KB) för txt-filer

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
