---
title: Licensiering för Microsoft Identity Manager och hämtningsbara filer | Microsoft Docs
description: Den här artikeln beskrivs metoder för licensiering Microsoft Identity Manager (MIM) 2016 med hänvisning om var du hämtar programvaran.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 9e74b7ccf332c1572ce395fad18b8629673c756a
ms.sourcegitcommit: 4f0b2883922bcb8fbef6b4284c35c6ca62c11565
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56954551"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016 licensing och hämtningsbara filer

Den här artikeln beskrivs metoder för licensiering Microsoft Identity Manager (MIM) 2016 med hänvisning om var du hämtar programvaran.

## <a name="licensing-mim-for-your-organization"></a>Licensiering MIM för din organisation

Microsoft Identity Manager 2016 licensieras per användare.  Information om licensiering är omfattas av villkoren i produkten och relaterade dokument som kan laddas ned från den [licensvillkor](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) sidan.

### <a name="licensing-for-azure-ad-premium-customers"></a>Licensiering för Azure AD Premium-kunder

Microsoft Identity Manager 2016 ingår Azure Active Directory Premium P1 och P2, vilket är en del av Enterprise Mobility + Security.

Azure AD Premium är tillgängligt via en [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), [Open Volume License-programmet](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx), och [Cloud Solution Providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) program. Azure och Office 365-prenumeranter kan också köpa Azure Active Directory Premium P1 och P2 online.  Läs mer i [priser för Azure Active Directory](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>MIM för Fjärrskrivbordstjänster

Om du inte har Azure Active Directory Premium-prenumerationer för dina användare och använder flera MIM-funktioner utöver synkronisering, en [klientåtkomstlicens (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) krävs för varje användare vars identitet är hanterad i MIM. Om du vill att externa användare – till exempel affärspartner, externa leverantörer eller kunder – om du vill kunna få åtkomst till MIM kan du skaffa klientåtkomstlicenser för alla externa användare eller skaffa licenser för extern anslutning (EG). Microsoft Identity Manager 2016 klientåtkomstlicenser krävs inte för användare vars identitet är bara i Microsoft Identity Manager-synkroniseringstjänsten och hanteras inte i andra MIM-komponent.

### <a name="licenses-for-platform-components"></a>Licenser för Plattformskomponenter

En Windows Server-licens krävs för att använda Microsoft Identity Manager 2016 server-programvara som en Windows Server-tillägg. Och en MIM distribution kräver också en SQL Server-installation.  Windows Server och SQL Server-licenser ingår inte i MIM.

## <a name="obtaining-mim-software"></a>Hämta MIM-programvaran

Innan du startar en ny installation av MIM eller en uppgradering från en tidigare version, se till att du har de senaste versionerna.

Om du startar en ny installation, behöver du ladda ned installationsfilerna för varje MIM-komponent som är relevanta för scenariot. Sedan hämta eventuella uppdateringar för dessa filer och sedan ladda ned de ytterligare komponenter som separata nedladdningar från Download Center.


| Scenario | Komponent | Krävs för scenariot? | DVD ISO mappnamn | Kommentar |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synkronisering| Synkroniseringstjänsten (inklusive anslutning till AD) | Ja | `Synchronization Service` | |
| Synkronisering | PCNS | Nej | `Password Change Notification Service` |  Installeras på domänkontrollanter |
| Synkronisering | Anslutningsappar för LDAP, SQL, Web Services, PowerShell, Lotus Domino Graph | Nej | Gäller inte | Distribueras via Download Center |
| Privilegierad åtkomsthantering | MIM-tjänst | Ja | `Service and Portal` | |
| Självbetjäning | MIM Service, MIM Portal | Ja | `Service and Portal` | |
| Självbetjäning | Lägg till-tillägg | Nej | `Add-ins and extensions` | Installeras på slutanvändarnas datorer |
| Självbetjäning | SCSM-rapportering | Nej | `Data Warehouse Support Scripts` | |
| Självbetjäning | Hybridrapporteringsagenten | Nej | Gäller inte | Distribueras via Download Center |
| Självbetjäning | Språkpaket | Nej | `LANGUAGE Packs` | |
| Certifikathantering | CM | Ja | `Certificate Management` | |
| Certifikathantering | CM-Masskopieringsklienten | Nej | `CM Bulk Client` | |
| Certifikathantering | CM-klienten | Nej | `CM Client`  | |
| Certifikathantering | CM-appen för Windows | Nej | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Hämta Windows installer-paket

De flesta organisationer för en ny installation, ladda ned MIM installationspaket från den [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


DVD ISO-filen innehåller en mapp för varje MIM-komponent: Synkronisering av tjänsten, tjänst och Portal, osv. Om du ska installera programvara på en annan dator som du har hämtat den, kan du kopiera hela ISO-filen eller mappen för komponenten: Kopiera inte bara en MSI-fil från en mapp utan resten av filer och undermappar bara.

Om du inte har åtkomst till Volume Licensing Service Center kunder med en lämplig utvecklarprenumeration kan också ladda ned MIM 2016 SP1 som en ISO-fil från [Mina förmåner hämtningsfiler för Visual Studio](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Sök efter ”Microsoft Identity Manager 2016 Service Pack 1”.  

Om du inte har åtkomst till Volume Licensing Service Center och bara vill prova att använda MIM-programvaran under en begränsad tid kan du hämta en [utvärderingsversionen av MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Den här programvaran är inte avsedd för användning i produktion och kommer att upphöra att fungera 180 dagar efter första installationen och kan inte uppgraderas. Utvärderingsversionen kräver Windows Server 2008 R2, Windows Server 2012 eller Windows Server 2012 R2 för installation.  Om du är nybörjare på MIM- och tekniken, Kom ihåg att alla MIM-scenarier kräver en Active Directory-domän, en Windows Server och SQL Server måste finnas. Om du inte har Windows Server eller SQL Server som redan finns, kanske du vill prova [etablera en virtuell dator med SQL Server 2016 och Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Hämta uppdateringar

När du har installerat MIM från MSI, bör du sedan installera den nödvändiga snabbkorrigeringar.

Kontrollera den [Identity Manager versionshistorik](./reference/version-history.md) för den senaste versionen från uppdateringen, som har en länk till hämtningsplatsen för installer patch-filer.

Om du vill ta reda på vilken uppdateringsfiler krävs, är den här tabellen anger komponenterna och namnet på motsvarande patch (MSP)-fil i en uppdatering.

| Scenario | Komponent | DVD ISO mappnamn | Motsvarande update patch-filnamn |
|----------|-----------|-   |-------------------|----------|--------------|
|Synkronisering| Tjänsten för synkronisering | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Självbetjäning | MIM Service, MIM Portal | `Service and Portal` | `FIMService_x64*msp` |
| Självbetjäning | Lägg till-tillägg | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Självbetjäning | Språkpaket | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Åtkomsthantering (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Certifikathantering | CM |  `Certificate Management` | `FIMCM*.msp` |
| Certifikathantering | CM-Masskopieringsklienten |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Certifikathantering | CM-klienten | CM-klienten |`FIMCMClient*msp` |

Kontrollera att läsa alla viktig associeras med uppdateringen innan du installerar MSP-fil.

Uppdaterar till [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) distribueras inte som MSP-filer, endast som MSI-installationsprogram.

### <a name="additional-downloads"></a>Ytterligare hämtningar

Följande hämtningsbara filer kan även vara relevanta:

- [MIM Hybrid Reporting Agent](https://www.microsoft.com/download/details.aspx?id=55112)

- [Allmän LDAP Connector, allmän SQL-anslutning, Graph kontakt, Lotus Domino-Anslutningsapp, PowerShell-Anslutningsapp, Anslutningsappen för webbtjänster](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Anslutningsapp för SharePoint användarens profil Store](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Om du inte redan har en Active Directory-domän och ställer in en PAM-scenariot för experimentering, finns i den [MIM 2016 SP1 PAM-distributionsskript](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om scenarier som levereras i [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Läs den [kapacitetsplaneringsguiden](capacity-planning-guide.md).
- Distribuera MIM för en [synkroniseringsscenario](microsoft-identity-manager-deploy.md) eller [scenariot med privilegierad åtkomsthantering](./pam/privileged-identity-management-for-active-directory-domain-services.md).

