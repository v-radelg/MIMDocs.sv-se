---
title: Distributionsskript för MIM2016 SP1 PAM
description: Den här sidan ingår i en artikelserie om hur du hanterar PIM med skript. Den innehåller en lista med antaganden om miljön.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: eeee6473f7471d4c961a4f4d3113d1af73ddaffe
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044420"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Distributionsskript för MIM2016 SP1 PAM

Detta Service Pack innehåller en uppsättning distributionsskript som gör det lättare att distribuera PAM. Dessa skript är tillgänglig på Download Center. Innan du försöker använda skripten är det viktigt att du uppfyller kraven nedan:

1. Operativ systemet på alla servrar är minst Windows Server 2012 R2.
2. DNS måste konfigureras för att aktivera namn matchning mellan domän kontrol Lanterna och komponent servrar.
3. Binärfilerna för installationerna måste vara tillgängliga lokalt på de avsedda servrarna för installation av SQL, SharePoint och MIM.
4. Miljön har tre dedikerade (fysiska eller virtuella) datorer som kör CORPDC, PRIVDC och PAMSERVER oberoende av varandra.
5. För verifierings alternativet måste du ha en dedikerad arbets Station.

>[!NOTE]
>Om du får problem med skriptkörningen kan du behöva granska loggarna. Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och ta med den och information om åtgärden och felet i supportärendet.

Redo att sätta igång med PAM-distributionsskript? Börja med att [konfigurera PAM med skript](./pam/sp1-pam-configure-using-scripts.md).
