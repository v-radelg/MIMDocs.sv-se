---
title: "Distributionsskript för MIM2016 SP1 PAM"
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# Distributionsskript för MIM2016 SP1 PAM

Detta Service Pack innehåller en uppsättning distributionsskript som gör det lättare att distribuera PAM. Dessa skript är tillgänglig på Download Center. Innan du försöker använda skripten är det viktigt att du ser till att antagandena nedan gäller för din miljö.

Viktiga antaganden:
1. Operativsystemet på alla datorer är minst Windows Server 2012 R2. Om du provar Windows Server 2016 Technical Preview 5 måste PRIV-domänkontrollanten vara installerad med TP5-versionen.
2. DNS måste vara konfigurerat så att namnmatchningen mellan dina domänkontrollanter och komponentservrar är automatisk.
3. Binärfilerna för installationerna måste vara tillgängliga lokalt på de avsedda servrarna för installation av SQL, SharePoint och MIM.
4. Miljön har tre dedikerade (fysiska eller virtuella) datorer som kör CORPDC, PRIVDC och PAMSERVER oberoende av varandra.
5. För verifieringsalternativet förutsätts det att det finns en dedikerad klientdator som kan användas för att köra det här steget.

>[!NOTE] Om du får problem med skriptkörningen kan du behöva granska loggarna. Alla skriptloggar sparas i %AppData%\MIMPAMInstall. Komprimera mappen till en ZIP-fil och skicka den till mim2016@microsoft.com tillsammans med information om åtgärden och felet.



<!--HONumber=Sep16_HO4-->

