---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "MIM innehåller åtkomsthanteringsfunktioner för FIM 2010 och hjälper dig att hantera användare, autentiseringsuppgifter, principer och åtkomst i din organisation."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 21bb12a70850a5f835ca6715d9683558ac6fad1d
ms.sourcegitcommit: f2778c5fa5f0cd04e8a74fc15fa340cd118dded5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Microsoft Identity Manager (MIM) 2016 bygger på funktionerna för identitets- och åtkomsthantering i [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Precis som föregångaren hjälper MIM dig att hantera användare, autentiseringsuppgifter, principer och åtkomst inom din organisation.  I MIM 2016 har man dessutom lagt till en hybridupplevelse, hanteringsfunktioner för privilegierad åtkomst och stöd för nya plattformar.

Förutom befintliga identity management-funktioner som ingår i [FIM](https://technet.microsoft.com/library/jj133868). MIM 2016 innehåller nya funktioner och förbättringar som:

- Privileged Identity Management
- Nya funktioner i Certifikathanteraren
  - [Referens för certifikathanterings-REST API](./reference/certificate-management-rest-api-reference.md)
  - Stöd för topologier med flera skogar.
  - En Windows-app för virtuella smartkort
  - Uppdaterade händelser samt funktioner för felsökning. 
- Självbetjäningsscenarier omfattar nu kontoupplåsning och Azure MFA-gate (multifaktorautentisering) för återställning av lösenord.

## <a name="hybrid-experience"></a>Hybridupplevelse

Microsoft Identity Manager 2016 fungerar tillsammans med [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) för att ge dig kontroll över hela miljön. Med hybridrapportering i Azure AD får du dina lokala data och dina data från molnet på en och samma plats. Dessutom den [lösenordsåterställning för Self Service portal](working-with-self-service-password-reset.md) stöder Azure multifaktorautentisering (MFA).

## <a name="privileged-identity-management"></a>Privileged Identity Management

Privileged Identity Management styr och hanterar administrativ åtkomst genom att tillhandahålla tillfällig och uppgiftsbaserad åtkomst till känsliga resurser. Det innebär att du bara behöver ge användare så mycket behörighet som krävs, vilket minskar risken för att en cyberangripare får fullständig administrativ åtkomst. Dessutom extraherar och isolerar Privileged Identity Management administrativa konton från befintliga Active Directory-skogar.

MIM har stöd för en lokal Privileged Identity Management-lösning för hantering av Active Directory. Kom igång genom att [använda Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Närliggande information

- Microsoft Identity Manager har fortfarande mycket gemensamt med föregångaren Forefront Identity Manager. Om du fortfarande använder FIM, eller om du vill få tillgång till ytterligare dokumentation, kan du ta en titt på [dokumentationsöversikten för FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Topologiöverväganden för distribution av MIM](topology-considerations.md) denna artikel presenterar flera olika topologier som du kan överväga att implementera.
- [Guide för kapacitetsplanering](capacity-planning-guide.md) du kan använda den här guiden, tillsammans med testmiljöerna, för att förstå lämplig omfattning för distributionen.