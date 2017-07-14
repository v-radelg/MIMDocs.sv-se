---
title: "PAM-miljönivåmodell | Microsoft Docs"
description: "Läs mer om nivåmodellen som segregerar ditt system baserat på säkerhetsrelaterade risker."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 4c3b43e50403890572e77773191a821cf247269c
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---

# Nivåmodell för partitionering av administratörsbehörighet
<a id="tier-model-for-partitioning-administrative-privileges" class="xliff"></a>

I dagens hotmiljö är det inte en fråga om ifall en angripare kan få tillgång till systemet, utan när. Det innebär att den interna säkerheten är precis lika viktig som ett starkt perimeterskydd. I den här artikeln beskrivs en säkerhetsmodell som är avsedd att skydda mot rättighetsutökning genom att skilja uppgifter med hög behörighetsnivå från riskzoner. Den här modellen ger en bra användarupplevelse och följer metodtips och säkerhetsprinciper.

## Utöka privilegier i Active Directory-skogar
<a id="elevation-of-privilege-in-active-directory-forests" class="xliff"></a>

Användare, tjänster och programkonton som beviljas permanent administratörsbehörighet för skogar i Windows Server Active Directory (AD) medför betydande risker för organisationens mål och verksamhet. Dessa konton är ofta mål för angrepp i och med att ett lyckat angrepp ger angriparen behörighet att ansluta till andra servrar och program i domänen.

Med nivåmodellen delas administratörer upp utifrån vilka resurser de hanterar. Administratörer med kontroll över användararbetsstationer separeras från de som styr program eller hanterar företagsidentiteter. Läs mer om den här modellen i [referensmaterialet för att skydda privilegierad åtkomst](http://aka.ms/tiermodel).

## Begränsa exponeringen av autentiseringsuppgifter med inloggningsbegränsningar
<a id="restricting-credential-exposure-with-logon-restrictions" class="xliff"></a>

Vanligen måste man omforma de administrativa metoderna för att minska risken för attacker och minska risken för stöld av autentiseringsuppgifter för administratörskonton. Som ett första steg bör organisationer:

- Begränsa antalet värdar på vilka administrativa autentiseringsuppgifter exponeras.
- Begränsa rollprivilegier till den lägsta nivå som krävs.
- Se till att administrativa uppgifter inte utförs på värdar som används för standardanvändares aktiviteter (till exempel e-postanvändning och webbsurfande).

Nästa steg är att implementera inloggningsbegränsningar och aktivera processer och metoder för att uppfylla kraven för nivåmodellen. Helst bör exponering av autentiseringsuppgifter också reduceras till det lägsta privilegium som krävs för rollen inom varje nivå.

Inloggningsbegränsningar bör tillämpas för att säkerställa att konton med höga privilegier inte har åtkomst till mindre säkra resurser. Exempel:

- Domänadministratörer (nivå 0) kan inte logga in på företagsservrar (nivå 1) och standardanvändares arbetsstationer (nivå 2).
- Serveradministratörer (nivå 1) kan inte logga in på standardanvändare arbetsstationer (nivå 2).

>[!NOTE]
> Serveradministratörer bör inte ingå i gruppen för domänadministratörer. Medarbetare som har ansvar för hantering av både domänkontrollanter och företagsservrar bör tilldelas separata konton.

Inloggningsbegränsningar kan tillämpas med:

- Grupprincipbegränsningar för inloggningsrättigheter, bland annat:  
    - Neka tillgång till den här datorn från nätverket  
    - Neka inloggning som batchjobb  
    - Neka att logga in som en tjänst  
    - Neka lokal inloggning  
    - Neka inloggning via inställningar för fjärrskrivbord  
- Autentiseringsprinciper och -silon, om du använder Windows Server 2012 eller senare
- Selektiv autentisering om kontot är i en dedikerad administrativ skog

I nästa artikel [Planera en skyddsmiljö](planning-bastion-environment.md) får du veta hur du lägger till en dedikerad administrativ skog för Microsoft Identity Manager för att upprätta administratörskonton.

