---
title: Förstå PAM-komponenterna | Microsoft Docs
description: Privileged Access Management delar vissa komponenter med MIM och har även några egna komponenter. Lär dig hur dessa fungerar tillsammans.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: accdda41616ba6c02959eda8549b6dbc322fc627
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043927"
---
# <a name="understand-the-components-of-pam"></a>Förstå komponenterna i PAM

Med Privileged Access Management separeras administrativ åtkomst från löpande användarkonton. Den här lösningen är beroende av parallella skogar:

- *CORP*: företagsskogen för allmänna uppgifter som innehåller en eller flera domäner. Även om du kan ha flera CORP-skogar utgår exemplen i de här artiklarna från en enkel skog med en enda domän.  
- *PRIV*: en särskild skog som skapats speciellt för det här PAM-scenariot. Den här skogen innehåller en domän för privilegierade grupper och konton som skuggas från en eller fler CORP-domäner.

MIM-lösningen som konfigurerats för PAM innehåller följande komponenter:  

- **MIM-tjänsten**: implementerar affärslogiken för att utföra identitets- och åtkomsthantering, inklusive hantering av privilegierade konton och utökningsbegäran.
- **MIM-portalen**: en SharePoint-baserad portal med SharePoint 2013 som värd, som tillhandahåller administratörer med ett användargränssnitt för hantering och konfiguration.
- **MIM-tjänstens databas**: lagras i SQL Server 2012 eller 2014 och innehåller identitetsdata och metadata som krävs för MIM-tjänsten.
- **PAM-övervakningstjänsten** och **PAM-komponenttjänsten**: två tjänster som hanterar livscykeln för privilegierade konton och stöder PRIV AD i gruppmedlemskapets livscykel.
- **PowerShell-cmdletar**: för att fylla MIM-tjänsten och PRIV AD med användare och grupper som motsvarar användarna och grupperna i CORP-skogen för PAM-administratörer, och för användare som begär just-in-time-åtkomst (JIT) till privilegier i ett administratörskonto.
- **PAM REST-API och exempelportal**: för utvecklare som integrerar MIM i PAM-scenariot med anpassade klienter för utökning utan att behöva använda PowerShell eller SOAP. Användning av REST API:t visas med en exempelwebbapp.

Efter installation och konfiguration är de grupper som skapats i migreringsproceduren i PRIV-skogen SIDHistory-baserade säkerhetsgrupper (eller externa huvudgrupper i en senare uppdatering med Windows Server vNext) som speglar SID-gruppen i den ursprungliga CORP-skogen. Dessutom tidsbegränsas medlemskapen när MIM-tjänsten lägger till medlemmar i dessa grupper i PRIV-skogen.

Det gör att när en användare begär utökning med PowerShell-cmdletarna och begäran har godkänts, lägger MIM-tjänsten till deras konton i PRIV-skogen i en grupp i PRIV-skogen. När användaren loggar in med sitt privilegierade konto innehåller användarens Kerberos-token en säkerhetsidentifierare (SID) som är identisk med SID för gruppen i CORP-skogen. Eftersom CORP-skogen har konfigurerats att lita på PRIV-skogen, visas det utökade kontot som används för att komma åt en resurs i CORP-skogen som en medlem av resursens säkerhetsgrupper för en resurs som kontrollerar Kerberos gruppmedlemskap. Detta tillhandahålls via Kerberos-autentisering mellan skogar.

Dessutom är dessa medlemskap tidsbegränsade. Det gör att användarens administratörskonto inte längre tillhör gruppen i PRIV-skogen efter en förinställd tidsperiod. Sedan kan kontot inte längre användas för att komma åt ytterligare resurser.
