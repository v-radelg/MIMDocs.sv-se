---
title: Distributionsskript för MIM2016 SP1 PAM
description: Den här sidan ingår i en artikelserie om hur du hanterar PIM med skript. Den innehåller en lista med antaganden om miljön.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: d9ed4414a3c4087b3ca7ec3709893ba1f8814d0a
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333198"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Distributionsskript för MIM2016 SP1 PAM

Detta Service Pack innehåller en uppsättning distributionsskript som gör det lättare att distribuera PAM. Dessa skript är tillgänglig på Download Center. Innan du försöker använda skripten är det viktigt att du ser till att du uppfyller kraven nedan:

1. Operativsystemet på alla servrar som är minst Windows Server 2012 R2.
2. DNS måste konfigureras så att namnmatchningen mellan dina domänkontrollanter och komponentservrar.
3. Binärfilerna för installationerna måste vara tillgängliga lokalt på de avsedda servrarna för installation av SQL, SharePoint och MIM.
4. Miljön har tre dedikerade (fysiska eller virtuella) datorer som kör CORPDC, PRIVDC och PAMSERVER oberoende av varandra.
5. Du måste ha en dedikerad arbetsstation för verifieringsalternativet.

>[!NOTE]
>Om du får problem med skriptkörningen kan du behöva granska loggarna. Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och ta med den och information om åtgärden och felet i supportärendet.

Redo att sätta igång med PAM-distributionsskript? Börja med att [konfigurera PAM med skript](./pam/sp1-pam-configure-using-scripts.md).
