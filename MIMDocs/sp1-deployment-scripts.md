---
title: "Distributionsskript för MIM2016 SP1 PAM"
description: "Den här sidan ingår i en artikelserie om hur du hanterar PIM med skript. Den innehåller en lista med antaganden om miljön."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 12c60e12dc5662ff0313e21bb9180b3709969af6
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---

# Distributionsskript för MIM2016 SP1 PAM
<a id="mim2016-sp1-pam-deployment-scripts" class="xliff"></a>

Detta Service Pack innehåller en uppsättning distributionsskript som gör det lättare att distribuera PAM. Dessa skript är tillgänglig på Download Center. Innan du försöker använda skripten är det viktigt att du ser till att antagandena nedan gäller för din miljö.

Viktiga antaganden:
1. Operativsystemet på alla datorer är minst Windows Server 2012 R2. Om du provar Windows Server 2016 Technical Preview 5 måste PRIV-domänkontrollanten vara installerad med TP5-versionen.
2. DNS måste vara konfigurerat så att namnmatchningen mellan dina domänkontrollanter och komponentservrar är automatisk.
3. Binärfilerna för installationerna måste vara tillgängliga lokalt på de avsedda servrarna för installation av SQL, SharePoint och MIM.
4. Miljön har tre dedikerade (fysiska eller virtuella) datorer som kör CORPDC, PRIVDC och PAMSERVER oberoende av varandra.
5. För verifieringsalternativet förutsätts det att det finns en dedikerad klientdator som kan användas för att köra det här steget.

>[!NOTE]
>Om du får problem med skriptkörningen kan du behöva granska loggarna. Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och skicka den till mim2016@microsoft.com tillsammans med information om åtgärden och felet.

Redo att sätta igång med PAM-distributionsskript? Börja med att [konfigurera PAM med skript](./pam/sp1-pam-configure-using-scripts.md).

