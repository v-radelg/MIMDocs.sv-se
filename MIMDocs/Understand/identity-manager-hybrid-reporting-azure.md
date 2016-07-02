---
title: "Rapporter för hybrididentitetshantering | Microsoft Identity Manager"
description: "Med hybridrapportering i Azure Active Directory kan du skapa anpassade rapporter som omfattar både lokala händelser och händelser i molnet."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 7e61e201b277a2e8ec9fee785e9e34fca3b1cb29
ms.openlocfilehash: b3f3982ade46932b18fb730fe5c16d52cde188a1


---

# Rapporter för hybrididentitetshantering i Azure
Med Azure Active Directory (AD) kan du skapa en enstaka rapport för att övervaka aktivitet om identitetshantering som sker antingen lokalt eller i molnet. Med den här funktionen kan du hantera alla identitets- och åtkomstdata på en plats, vilket sparar tid och minskar totalkostnaderna.

## Vad är Azure AD-hybridrapportering?
Hybridrapporteringen hjälper IT-proffs att hantera vanliga problem vad gäller rapportering av identitetshantering.

1. **Samla in aktiviteter om identitetshantering från olika system.** Hybridrapporter visar aktivitet om identitetshantering från Azure AD och Identity Manager.

2. **Exportera rapporteringsdata och skapa anpassade rapporter.** Förutom att visa rapporterna i Azure-portalen kan du också exportera dina data och skapa egna anpassade vyer.

3. **Minska rapportsystemets infrastrukturkostnader.** Hybridrapportering i molnet gör att du kan du eliminera infrastrukturen för datalager för lokal rapportering

## Hur fungerar det?

För att samla in lokala data installerar du först en rapporteringsagent på Identity Manager-servern. Rapporteringsagenten hämtas från konfigurationssidan i din katalog i den [klassiska Azure-portalen](https://manage.windowsazure.com/).

Hybridrapporteringen följer de här stegen:
1. När rapporteringsagenten har installerats skickas aktivitetsdata i Identity Manager till händelseloggen i Windows.
2. Rapporteringsagenten bearbetar händelserna i händelseloggen i Windows och överför dem till Azure.
3. Alla aktivitetsdata lagras i Azure i en månad.
4. När du begär en rapport parsas och filtreras aktivitetshändelserna för den rapport du har begärt.
5. Den klassiska Azure-portalen hämtar rapporteringsdata och återger dem i form av en aktivitetsrapport.

## Se även
- Läs mer om [Arbeta med hybridrapportering i Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Jun16_HO4-->


