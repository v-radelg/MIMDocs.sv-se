---
title: "Vad är hybridrapportering | Microsoft Docs"
description: "Med aktivitetsrapporter för hybridgranskning i Azure Active Directory kan du se händelser som granskats såväl i molnet som lokalt."
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Granskningsrapporter för hybrididentitetshantering i Azure Active Directory
Med granskningsaktivitetsrapporter i Azure Active Directory (AD) kan du visa en enstaka rapport för att övervaka aktivitet om identitetshantering som sker antingen lokalt eller i molnet. Med den här funktionen kan du hantera alla identitets- och åtkomstdata på en plats, vilket sparar tid och minskar totalkostnaderna.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Vad är hybridrapportering i Azure Active Directory?
Hybridgranskningsrapporteringen hjälper IT-proffs att hantera vanliga problem som gäller rapportering av identitetshantering.

1. **Samla in aktiviteter om identitetshantering från olika system.** Hybridrapporter visar aktivitet om identitetshantering från Azure AD och Identity Manager.

2. **Exportera rapporteringsdata och skapa anpassade rapporter.** Förutom att visa rapporterna i Azure-portalen kan du också exportera dina data och skapa egna anpassade vyer.

3. **Minska rapportsystemets infrastrukturkostnader.** Hybridrapportering i molnet gör att du kan du eliminera infrastrukturen för datalager för lokal rapportering

## <a name="how-does-it-work"></a>Hur fungerar det?

För att samla in lokala data installerar du först en rapporteringsagent på Identity Manager 2016-servern. Rapporteringsagenten laddas ned från Microsofts nedladdningssida [här](https://www.microsoft.com/en-us/download/details.aspx?id=####/).

Hybridrapporteringen följer de här stegen:
1. När rapporteringsagenten har installerats skickas aktivitetsdata i Identity Manager till händelseloggen i Windows.
2. Rapporteringsagenten bearbetar händelserna i händelseloggen i Windows och överför dem till Azure Portal.
3. Alla aktivitetsdata lagras i Azure i en månad.
4. När du begär en rapport parsas och filtreras aktivitetshändelserna för den rapport du har begärt.
5. Azure Portal hämtar dina granskningsrapporteringsdata och återger dem i form av granskningen i aktivitetsrapporten.

## <a name="see-also"></a>Se även
- Läs mer om [Arbeta med hybridrapportering i Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)
- Läs mer om [granskningsaktivitetsrapporter i Azure Active Directory-portalen](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)

