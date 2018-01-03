---
title: "Vad är hybridrapportering | Microsoft Docs"
description: "Med aktivitetsrapporter för hybridgranskning i Azure Active Directory kan du se händelser som granskats såväl i molnet som lokalt."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: ffe372c273aae55278f9b18b45b65425734aa6f7
ms.sourcegitcommit: e52bab207117390997c6fa8450de24335b502673
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/20/2017
---
# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Granskningsrapporter för hybrididentitetshantering i Azure Active Directory - öppen förhandsversion (uppdatera)
Med granskningsaktivitetsrapporter i Azure Active Directory (AD) kan du visa en enstaka rapport för att övervaka aktivitet om identitetshantering som sker antingen lokalt eller i molnet. Med den här funktionen kan du hantera alla identitets- och åtkomstdata på en plats, vilket sparar tid och minskar totalkostnaderna.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Vad är hybridrapportering i Azure Active Directory?
Hybridgranskningsrapporteringen hjälper IT-proffs att hantera vanliga problem som gäller rapportering av identitetshantering.

1. **Samla in aktiviteter om identitetshantering från olika system.** Hybridrapporter visar aktivitet om identitetshantering från Azure AD och Identity Manager.

2. **Exportera rapporteringsdata och skapa anpassade rapporter.** Förutom att visa rapporterna i Azure-portalen kan du också exportera dina data och skapa egna anpassade vyer.

3. **Minska rapportsystemets infrastrukturkostnader.** Hybridrapportering i molnet gör att du kan du eliminera infrastrukturen för datalager för lokal rapportering.

## <a name="how-does-it-work"></a>Hur fungerar det?

För att samla in lokala data installerar du först en rapporteringsagent på Identity Manager 2016-servern. Rapporteringsagenten laddas ned från Microsofts nedladdningssida [här](https://www.microsoft.com/download/details.aspx?id=55112).

Hybridrapporteringen följer de här stegen:
1. När rapporteringsagenten har installerats skickas aktivitetsdata i Identity Manager till händelseloggen i Windows.
2. Rapporteringsagenten bearbetar händelserna i händelseloggen i Windows var 10:e minut eller när tjänsten startar om och överför dem till Azure Portal.
3. Azure-portalen behandlar mottagna data inom 1 timme från mottagandet
4. Alla aktivitetsdata lagras i Azure i en månad.
5. Azure Portal hämtar dina granskningsrapporteringsdata och återger dem i form av granskningen i granskningsrapportbladet i Azure.

## <a name="next-steps"></a>Nästa steg
- Läs mer om [Arbeta med hybridrapportering i Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- Läs mer om [granskningsaktivitetsrapporter i Azure Active Directory-portalen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- Mer information om Erhåll [rapportera bevarandeprinciper](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- Mer information om Erhåll [Microsoft Azure log-integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- Mer information om Erhåll [Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
