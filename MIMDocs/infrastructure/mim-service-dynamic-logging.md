---
title: "MIM Service Dynamic Logging (Dynamisk loggning med MIM-tjänsten) | Microsoft Docs"
description: "Aktivera dynamisk loggning med MIM-tjänsten utan att behöva starta om hanteringstjänsten"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: 1e2fb9a9ae508ab601ebad1dec7acc21dc44d13e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
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

Konfigurationsfilen med dynamisk loggning finns på rad 266 : Microsoft.ResourceManagement.Service.exe.config

![Markerade avsnitt visar rader med de olika tillgängliga loggningsområdena](media/mim-service-dynamic-logging/screen02.png)

Som standard finns loggningsplatsen på **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**. FIM-tjänstkontot måste skriva behörighet till den här platsen för att skapa den dynamiska loggen.

![Mapplats för loggarna](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 I händelse av oväntade fel (syntaxfel i konfigurationsfilen Microsoft.ResourceManagement.Service.exe.config eller andra misstag) skrivs motsvarande felmeddelande i filen Microsoft.ResourceManagement.Service.exe_Emergency.log under följande sökväg %TMP% eller %TEMP% eller %USERPROFILE% (den första som finns).  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Om du vill visa spårningen kan du använda [visningsverktyget för tjänstspårning](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Skärmbild av visningsverktyget för tjänstspårning](media/mim-service-dynamic-logging/screen04.png)
