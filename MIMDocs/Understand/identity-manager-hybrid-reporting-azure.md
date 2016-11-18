---
title: "Vad är hybridrapportering | Microsoft Docs"
description: "Med hybridrapportering i Azure Active Directory kan du skapa anpassade rapporter som omfattar både lokala händelser och händelser i molnet."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f21c15fdaa5fba9176cfc60a3c49017fa97fa935


---

# <a name="hybrid-identity-management-reports-in-azure"></a>Rapporter för hybrididentitetshantering i Azure
Med Azure Active Directory (AD) kan du skapa en enstaka rapport för att övervaka aktivitet om identitetshantering som sker antingen lokalt eller i molnet. Med den här funktionen kan du hantera alla identitets- och åtkomstdata på en plats, vilket sparar tid och minskar totalkostnaderna.

## <a name="what-is-azure-ad-hybrid-reporting"></a>Vad är Azure AD-hybridrapportering?
Hybridrapporteringen hjälper IT-proffs att hantera vanliga problem vad gäller rapportering av identitetshantering.

1. **Samla in aktiviteter om identitetshantering från olika system.** Hybridrapporter visar aktivitet om identitetshantering från Azure AD och Identity Manager.

2. **Exportera rapporteringsdata och skapa anpassade rapporter.** Förutom att visa rapporterna i Azure-portalen kan du också exportera dina data och skapa egna anpassade vyer.

3. **Minska rapportsystemets infrastrukturkostnader.** Hybridrapportering i molnet gör att du kan du eliminera infrastrukturen för datalager för lokal rapportering

## <a name="how-does-it-work"></a>Hur fungerar det?

För att samla in lokala data installerar du först en rapporteringsagent på Identity Manager-servern. Rapporteringsagenten hämtas från konfigurationssidan i din katalog i den [klassiska Azure-portalen](https://manage.windowsazure.com/).

Hybridrapporteringen följer de här stegen:
1. När rapporteringsagenten har installerats skickas aktivitetsdata i Identity Manager till händelseloggen i Windows.
2. Rapporteringsagenten bearbetar händelserna i händelseloggen i Windows och överför dem till Azure.
3. Alla aktivitetsdata lagras i Azure i en månad.
4. När du begär en rapport parsas och filtreras aktivitetshändelserna för den rapport du har begärt.
5. Den klassiska Azure-portalen hämtar rapporteringsdata och återger dem i form av en aktivitetsrapport.

## <a name="see-also"></a>Se även
- Läs mer om [Arbeta med hybridrapportering i Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Nov16_HO2-->


