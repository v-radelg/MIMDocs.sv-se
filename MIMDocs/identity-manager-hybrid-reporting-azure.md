---
title: Vad är hybridrapportering i Azure AD? | Microsoft Docs
description: Hybrid aktivitet granskningsrapporter i Azure Active Directory kan du visa granskade händelser från molnet och lokalt.
keywords: ''
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/20/2018
ms.locfileid: "29370386"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Hybrididentitetshantering gransknings-och rapportering i Azure Active Directory
Med Azure Active Directory (AD Azure) audit aktivitetsrapportering, du kan övervaka aktivitet om Identitetshantering antingen lokalt eller i molnet. Genom att hantera alla identitets- och data i en rapport kan du spara tid och sänka övergripande kostnader.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Vad är hybridrapportering i Azure Active Directory?
Hybrid audit reporting hjälper IT-proffs utmaningarna vanliga identity management reporting, exempelvis:

* **Samla in aktiviteter om Identitetshantering från olika system**. Hybridrapporter visar aktivitet om identitetshantering från Azure AD och Identity Manager.

* **Exportera rapporteringsdata och skapa anpassade rapporter**. Förutom att visa rapporterna i Azure-portalen kan du exportera data för att skapa egna anpassade vyer.

* **Minska rapportsystemets infrastrukturkostnader**. Hybridrapportering i molnet innebär att du kan bidra till att eliminera kostnader som är associerade med din lokala, infrastrukturen för datalager.

## <a name="how-does-it-work"></a>Hur fungerar det?

För att samla in lokala data installerar du först en rapporteringsagent på Identity Manager 2016-servern. [Ladda ned Microsoft Identity Manager Hybrid Reporting Agent](https://www.microsoft.com/download/details.aspx?id=55112).

Hybridrapportering genomgår följande process:
1. När du har installerat rapporteringsagenten skickas aktivitetsdata Identity Manager till händelseloggen i Windows.
2. Rapporteringsagenten bearbetar händelserna delta var 10: e minut eller när händelseloggen i Windows-tjänsten startas om. Agenten överför sedan händelser till Azure-portalen.
3. Azure-portalen behandlar mottagna data inom en timme för att ta emot den.
4. Alla aktivitetsdata lagras i Azure i en månad.
5. Azure-portalen hämtar revisionen rapportdata och visas i fönstret Azure gransknings-och rapportering.

## <a name="next-steps"></a>Nästa steg
Läs mer om:
- [Arbeta med Identity Manager Hybrid Reporting](working-with-identity-manager-hybrid-reporting.md)
- [Granska aktivitetsrapporter i Azure Active Directory-portalen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Rapportering bevarandeprinciper](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure log-integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
