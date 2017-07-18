---
title: "Distributionsskript för MIM2016 SP1 PAM"
description: "Den här sidan ingår i en artikelserie om hur du hanterar PIM med skript. Den innehåller en lista med antaganden om miljön."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: ae8f6a87f57c95e073b40d3cda944c71f1bf7247
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/14/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Distributionsskript för MIM2016 SP1 PAM

Detta Service Pack innehåller en uppsättning distributionsskript som gör det lättare att distribuera PAM. Dessa skript är tillgänglig på Download Center. Innan du försöker använda skripten är det viktigt att du ser till att antagandena nedan gäller för din miljö.

Viktiga antaganden:
1. Operativsystemet på alla datorer är minst Windows Server 2012 R2. Om du provar Windows Server 2016 Technical Preview 5 måste PRIV-domänkontrollanten vara installerad med TP5-versionen.
2. DNS måste vara konfigurerat så att namnmatchningen mellan dina domänkontrollanter och komponentservrar är automatisk.
3. Binärfilerna för installationerna måste vara tillgängliga lokalt på de avsedda servrarna för installation av SQL, SharePoint och MIM.
4. Miljön har tre dedikerade (fysiska eller virtuella) datorer som kör CORPDC, PRIVDC och PAMSERVER oberoende av varandra.
5. För verifieringsalternativet förutsätts det att det finns en dedikerad klientdator som kan användas för att köra det här steget.

>[!NOTE]
>Om du får problem med skriptkörningen kan du behöva granska loggarna. Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och ta med den och information om åtgärden och felet i supportärendet.

Redo att sätta igång med PAM-distributionsskript? Börja med att [konfigurera PAM med skript](./pam/sp1-pam-configure-using-scripts.md).
