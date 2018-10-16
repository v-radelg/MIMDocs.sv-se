---
title: Vad är hybridrapportering i Azure AD? | Microsoft Docs
description: Hybrid granskning av aktivitetsrapporter i Azure Active Directory kan du visa granskade händelser i både molnet och lokalt.
keywords: ''
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: 459e1c0c04f28b1ecc1c74f5f672c6318684cc90
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332841"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Hybrididentitetshantering gransknings-och rapportering i Azure Active Directory
Med Azure Active Directory (Azure AD) audit aktivitetsrapportering, du kan övervaka aktivitet om Identitetshantering antingen lokalt eller i molnet. Genom att hantera alla identitets- och data i en enda rapport kan du spara tid och minska övergripande kostnader.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Vad är hybridrapportering i Azure Active Directory?
Hybrid audit rapportering hjälper IT-proffs kan du hantera vanliga Identitetshantering reporting utmaningar, som:

* **Samla in aktiviteter om Identitetshantering från olika system**. Hybridrapporter visar aktivitet om identitetshantering från Azure AD och Identity Manager.

* **Exportera rapporteringsdata och skapa anpassade rapporter**. Förutom att visa rapporterna i Azure-portalen, kan du exportera data och skapa egna anpassade vyer.

* **Minska rapportsystemets infrastrukturkostnader**. Hybridrapportering i molnet innebär att du kan undvika de kostnader som är associerade med din lokala, infrastrukturen för datalager.

## <a name="how-does-it-work"></a>Hur fungerar det?

För att samla in lokala data installerar du först en rapporteringsagent på Identity Manager 2016-servern. [Ladda ned Microsoft Identity Manager Hybrid Reporting Agent](https://www.microsoft.com/download/details.aspx?id=55112).

Hybridrapporteringen görs på följande sätt:
1. När du har installerat rapporteringsagenten skickas aktivitetsdata i Identity Manager till händelseloggen i Windows.
2. Rapporteringsagenten bearbetar händelserna var tionde minut eller när tjänsten Windows startas om. Agenten överför sedan händelser till Azure-portalen.
3. Azure-portalen behandlar mottagna data inom en timme från mottagandet.
4. Alla aktivitetsdata lagras i Azure i en månad.
5. Azure-portalen hämtar rapporteringsdata och visar det i fönstret granska Reporting i Azure.

## <a name="next-steps"></a>Nästa steg
Läs mer om:
- [Arbeta med Identity Manager Hybrid Reporting](working-with-identity-manager-hybrid-reporting.md)
- [Granska aktivitetsrapporter i Azure Active Directory-portalen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Rapportering bevarandeprinciper](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure-loggintegrering (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
