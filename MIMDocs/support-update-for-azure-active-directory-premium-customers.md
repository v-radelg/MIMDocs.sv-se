---
title: Support uppdatering för Azure AD Premium kunder som använder Microsoft Identity Manager | Microsoft Docs
description: I den här artikeln beskrivs hur Azure AD Premium kunder kan få support efter den 21 januari 2021.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 26dcaf121c4fd980d296ffee893af3ca66249c6c
ms.sourcegitcommit: ea16fea5d69aff58b862468d4bebfb05100d037a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/12/2020
ms.locfileid: "84749247"
---
# <a name="support-update-for-azure-ad-premium-customers-using-microsoft-identity-manager"></a>Support uppdatering för Azure AD Premium kunder som använder Microsoft Identity Manager

Gäller för: Azure AD Premium, Microsoft Identity Manager (MIM)

För Azure AD Premium kunder är standard supporten tillgänglig från juni 2020 och fortsätter efter 2021, för vissa komponenter i [Microsoft Identity Manager 2016 Service Pack 2](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016)eller senare Service Packs som möjliggör integrering med Azure AD. Detta är utöver det befintliga stödet för Microsoft Identity Manager som redan tillhandahålls genom den [fasta livs cykel policyn](https://docs.microsoft.com//lifecycle/policies/fixed) och planer för [Support för företag](https://support.microsoft.com/help/4341255).

De MIM-komponenter för vilka standard support är tillgängliga är:
- MIM-synkroniseringstjänsten och tjänsten för meddelanden om lösen ords ändring (PCNS)
- MIM-tjänst och Portal, tillägg och tillägg, support skript och språk paket för data lager
- MIM-kopplingar

Dessa MIM-komponenter fyller Active Directory och efter tillägg, Azure AD via Azure AD Connect, med användare och grupper etablerade från ett lokalt HR-system eller annat system av data källor. Detta säkerställer att kunder som använder Azure AD Premium med lokala system kan fortsätta att stödjas under migreringen av deras identitets hanterings scenarier från lokala system till Azure AD. 

## <a name="opening-a-support-request-in-the-azure-portal"></a>Öppna ett support ärende i Azure Portal

Som ett ytterligare support alternativ för Microsoft Identity Manager kan Azure AD Premium kunder begära stöd för ovanstående komponenter i Microsoft Identity Manager 2016 Service Pack 2 eller en senare snabb korrigering eller uppdatering via Azure Portal.

En kund kan skapa en support förfrågan för Azure med hjälp av anvisningarna i [så här skapar du en support förfrågan för Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request):
1. Välj *typ av problem: teknisk*
1. Växla till Visa *alla tjänster*
1. i listan tjänster under Azure Active Directory väljer du *användar etablering och synkronisering*
1. Välj *typ av problem: Microsoft Identity Manager (MIM)*
1. Välj *problem under typ*: *anslutningar*, *tjänst och Portal* eller *synkroniseringstjänst*

![Skapa MIM-Supportbegäran](media/azure-active-directory-new-support-request.png)

MIM-komponenterna visas som problem typer i *Azure Active Directory användar etablering och synkronisering* i Azure Portal.

För förfrågningar som öppnas via Azure Portal är standard support tillgängligt för Azure AD Premium kunder för följande komponenter i Microsoft Identity Manager 2016 Service Pack 2 eller en [senare snabb korrigering eller uppdatering](reference/version-history.md): SYNKRONISERINGSTJÄNST, PCNS (Password Change Notification Service), anslutningar, tjänst och Portal, tillägg och tillägg, support skript för informations lager och språk paket.

## <a name="other-support-options"></a>Andra supportalternativ

MIM 2016 SP2, build 4.6.34.0, släpptes i oktober 2019. Kunderna rekommenderas att stanna kvar på en fullständigt service pack för att se till att de är på den senaste och säkraste versionen av produkten. Mer information finns i princip för [Service Pack Lifecycle](https://support.microsoft.com/help/17138).

För kunder som fortfarande använder en äldre version av MIM, eller kunder som inte har Azure-support eller prenumeration på en svit som innehåller Azure Active Directory Premium, eller för problem med andra komponenter i MIM som inte anges ovan, är stödet fortfarande tillgängligt. Support policyn beskrivs i en [fast livs cykel policy](https://docs.microsoft.com/lifecycle/policies/fixed) med de angivna datumen vid [support livs cykeln för Microsoft Identity Manager 2016](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016).

Förutom support för Azure kan flera andra support alternativ organisationer använda för att få support. Om du till exempel har Microsoft Professional-support, kan du [skapa en ny support förfrågan](https://support.microsoft.com/supportforbusiness/productselection). Så här väljer du relevant MIM-komponent:
1. Välj *säkerhet* för produkt familj
1. Välj produkt *identitets hanterare 2016*
