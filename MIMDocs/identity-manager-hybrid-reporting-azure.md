---
title: Vad är hybrid rapportering i Azure AD? | Microsoft Docs
description: Med aktivitets rapporter för Hybrid granskning i Azure Active Directory kan du Visa granskade händelser både i molnet och lokalt.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: dd87f00fb3faded60671a47a0ba1dab7e4c2a531
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516765"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Gransknings rapportering för Hybrid identitets hantering i Azure Active Directory
Med Azure Active Directory (Azure AD) rapportering av gransknings aktiviteter kan du övervaka identitets hanterings aktiviteter antingen lokalt eller i molnet. Genom att hantera alla dina identitets-och åtkomst data i en enda rapport kan du spara tid och minska totalkostnaden.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Vad är hybridrapportering i Azure Active Directory?
Hybrid gransknings rapportering hjälper IT-proffs att hantera vanliga rapporterings utmaningar för identitets hantering, till exempel:

* **Samla in identitets hanterings aktiviteter i olika system**. Hybridrapporter visar aktivitet om identitetshantering från Azure AD och Identity Manager.

* **Exportera rapporterings data och skapa anpassade rapporter**. Förutom att visa dina rapporter i Azure Portal kan du exportera dem för att skapa dina egna anpassade vyer.

* **Minska kostnaderna för rapporterings system infrastrukturen**. Hybrid rapportering i molnet innebär att du kan eliminera de kostnader som är associerade med din lokala infrastruktur för data lager.

## <a name="how-does-it-work"></a>Hur fungerar det?

För att samla in lokala data installerar du först en rapporteringsagent på Identity Manager 2016-servern. [Hämta Microsoft Identity Manager hybrid repor ting agent](https://www.microsoft.com/download/details.aspx?id=55112).

Hybrid rapportering sker i följande steg:
1. När du har installerat rapport agenten skickas aktivitets data i Identity Manager till Windows-händelseloggen.
2. Rapport agenten bearbetar delta händelser var tionde minut eller när tjänsten Windows händelse logg startas om. Agenten överför sedan händelserna till Azure Portal.
3. Azure Portal bearbetar mottagna data inom en timme från det att den tagit emot den.
4. Alla aktivitetsdata lagras i Azure i en månad.
5. Azure Portal hämtar gransknings rapporterings data och visar dem i fönstret Azure Audit repor ting.

## <a name="next-steps"></a>Nästa steg
Läs mer om:
- [Arbeta med Identity Manager hybrid repor ting](working-with-identity-manager-hybrid-reporting.md)
- [Granska aktivitetsrapporter i Azure Active Directory-portalen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Rapportera bevarande principer](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure log integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [API för Azure Active Directory rapportering](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
