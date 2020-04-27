---
title: PAM-miljönivåmodell | Microsoft Docs
description: Läs mer om nivåmodellen som segregerar ditt system baserat på säkerhetsrelaterade risker.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/30/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4fb5689400d170adc19f15cbbc2d45915cb39fe3
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043604"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>Nivåmodell för partitionering av administratörsbehörighet

I den här artikeln beskrivs en säkerhetsmodell som är avsedd att skydda mot rättighetsutökning genom att skilja uppgifter med hög behörighetsnivå från riskzoner. Den här modellen ger en bra användarupplevelse och följer metodtips och säkerhetsprinciper.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Utöka privilegier i Active Directory-skogar

Användare, tjänster och programkonton som beviljas permanent administratörsbehörighet för skogar i Windows Server Active Directory (AD) medför betydande risker för organisationens mål och verksamhet. Dessa konton är ofta riktade mot angripare eftersom angriparen har behörighet att ansluta till andra servrar eller program i domänen.

Med nivåmodellen delas administratörer upp utifrån vilka resurser de hanterar. Administratörer med kontroll över användararbetsstationer separeras från de som styr program eller hanterar företagsidentiteter. Läs mer om den här modellen i [referensmaterialet för att skydda privilegierad åtkomst](https://aka.ms/tiermodel).

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Begränsa exponeringen av autentiseringsuppgifter med inloggningsbegränsningar

Vanligen måste man omforma de administrativa metoderna för att minska risken för attacker och minska risken för stöld av autentiseringsuppgifter för administratörskonton. Som ett första steg bör organisationer:

- Begränsa antalet värdar på vilka administrativa autentiseringsuppgifter exponeras.
- Begränsa rollprivilegier till den lägsta nivå som krävs.
- Se till att administrativa uppgifter inte utförs på värdar som används för standardanvändares aktiviteter (till exempel e-postanvändning och webbsurfande).

Nästa steg är att implementera inloggningsbegränsningar och aktivera processer och metoder för att uppfylla kraven för nivåmodellen. Helst bör exponering av autentiseringsuppgifter också reduceras till det lägsta privilegium som krävs för rollen inom varje nivå.

Inloggningsbegränsningar bör tillämpas för att säkerställa att konton med höga privilegier inte har åtkomst till mindre säkra resurser. Ett exempel:

- Domänadministratörer (nivå 0) kan inte logga in på företagsservrar (nivå 1) och standardanvändares arbetsstationer (nivå 2).
- Serveradministratörer (nivå 1) kan inte logga in på standardanvändare arbetsstationer (nivå 2).

>[!NOTE]
> Serveradministratörer bör inte ingå i gruppen för domänadministratörer. Medarbetare som har ansvar för hantering av både domänkontrollanter och företagsservrar bör tilldelas separata konton.

Inloggningsbegränsningar kan tillämpas med:

- Grupprincipbegränsningar för inloggningsrättigheter, bland annat:
    - Neka åtkomst till den här datorn från nätverket
    - Neka inloggning som batchjobb
    - Neka att logga in som en tjänst
    - Neka lokal inloggning
    - Neka inloggning via inställningar för fjärrskrivbord  
- Autentiseringsprinciper och -silon, om du använder Windows Server 2012 eller senare
- Selektiv autentisering om kontot är i en dedikerad administrativ skog

## <a name="next-steps"></a>Nästa steg

- I nästa artikel [Planera en skyddsmiljö](planning-bastion-environment.md) får du veta hur du lägger till en dedikerad administrativ skog för Microsoft Identity Manager för att upprätta administratörskonton.
- [Till gångs arbets stationer](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) tillhandahåller ett dedikerat operativ system för känsliga uppgifter som skyddas från Internet attacker och hot vektorer.
