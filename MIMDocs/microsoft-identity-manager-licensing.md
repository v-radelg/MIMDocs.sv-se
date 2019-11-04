---
title: Microsoft Identity Manager licensiering och nedladdningar | Microsoft Docs
description: Den här artikeln beskriver metoderna för licens Microsoft Identity Manager (MIM) 2016, med pekare på var du kan ladda ned program varan.
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: femila
ms.date: 10/18/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: e0bfd868345b8e7dcc6a02e745d3ccbf632a6c58
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329291"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016-licensiering och nedladdningar

Den här artikeln beskriver metoderna för licens Microsoft Identity Manager (MIM) 2016, med pekare på var du kan ladda ned program varan.

## <a name="licensing-mim-for-your-organization"></a>Licensiering av MIM för din organisation

Microsoft Identity Manager 2016 licensieras per användare.  Informationen om licensiering ingår i produkt villkoren och relaterade dokument, som kan hämtas från sidan [licens villkor](https://www.microsoft.com/licensing/product-licensing/products.aspx) .

### <a name="licensing-for-azure-ad-premium-customers"></a>Licensiering för Azure AD Premium kunder

Microsoft Identity Manager 2016 ingår i Azure Active Directory Premium (P1 och P2), som är en del av Enterprise Mobility + Security.

Azure AD Premium är tillgängligt via en [Microsoft Enterprise-avtal](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx), [Open Volume License-programmet](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)och [Cloud solution providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) -programmet. Azure-och Office 365-prenumeranter kan också köpa Azure Active Directory Premium P1 och P2 online.  Läs mer på [Azure Active Directory prissättning](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="mim-cals"></a>MIM CAL

Om du inte har Azure Active Directory Premium prenumerationer för dina användare och använder fler MIM-funktioner utöver synkroniseringen krävs en [klient åtkomst licens (CAL)](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx) för varje användare vars identitet hanteras i mim. Om du vill att externa användare, t. ex. affärs partner, externa entreprenörer eller kunder, ska kunna komma åt MIM kan du hämta CAL för var och en av dina externa användare eller skaffa externa anslutnings licenser (EC). Microsoft Identity Manager 2016-CAL krävs inte för användare vars identitet endast finns i Microsoft Identity Manager synkroniseringstjänst och inte hanteras i någon annan MIM-komponent.

### <a name="licenses-for-platform-components"></a>Licenser för plattforms komponenter

En Windows Server-licens krävs för att använda Microsoft Identity Manager 2016: s serverprogram vara som ett Windows Server-tillägg. En MIM-distribution kräver också en SQL Server-installation.  Windows Server-och SQL Server-licenser ingår inte i MIM.

## <a name="obtaining-mim-software"></a>Hämta MIM-programvara

Se till att du har de senaste versionerna innan du påbörjar en ny installation av MIM eller en uppgradering från en tidigare version.

Om du startar en ny installation måste du ladda ned installationsfilerna för varje MIM-komponent som är relevant för ditt scenario. Hämta sedan alla uppdateringar för dessa filer och hämta eventuella ytterligare komponenter som är separata hämtningar från Download Center.


| Scenario | Komponent | Krävs för scenariot? | Namn på DVD ISO-mapp | Kommentar |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synkronisering| Sync service (inklusive anslutning till AD) | Ja | `Synchronization Service` | |
| Synkronisering | PCNS | Nej | `Password Change Notification Service` |  Installeras på domänkontrollanter |
| Synkronisering | Anslutningar för LDAP, SQL, Web Services, PowerShell, Lotus Domino, Graph | Nej | E.t. | Distribuerad via Download Center |
| Privilegierad åtkomsthantering | MIM-tjänst | Ja | `Service and Portal` | |
| Självbetjäning | MIM-tjänsten, MIM-portalen | Ja | `Service and Portal` | |
| Självbetjäning | Tillägg och tillägg | Nej | `Add-ins and extensions` | Installeras på slut användar datorer |
| Självbetjäning | SCSM-rapportering | Nej | `Data Warehouse Support Scripts` | |
| Självbetjäning | Hybrid rapporterings agent | Nej | E.t. | Distribuerad via Download Center |
| Självbetjäning | Språk paket | Nej | `LANGUAGE Packs` | |
| Certifikathantering | 210 | Ja | `Certificate Management` | |
| Certifikathantering | CM Mass klient | Nej | `CM Bulk Client` | |
| Certifikathantering | CM-klient | Nej | `CM Client`  | |
| Certifikathantering | CM-app för Windows | Nej | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Hämta Windows Installer-paket

För en ny installation laddar de flesta organisationer över MIM-installations paketen från [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


DVD-ISO-filen innehåller en mapp för varje MIM-komponent: synkroniseringstjänst, tjänst och Portal osv. Om du kommer att installera program varan på en annan dator än den som du laddade ned till, måste du kopiera antingen hela ISO-filen eller mappen för komponenten: Kopiera inte bara en MSI-fil från en mapp utan resten av filerna och undermapparna.

Om du inte har åtkomst till Volume Licensing Service Center kan kunder med en lämplig prenumeration på utvecklare också hämta MIM 2016 SP2 som en ISO-fil från [Visual Studio mina förmåner hämtningar](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=).  Sök efter "Microsoft Identity Manager 2016 med Service Pack 2".  

Om du inte har åtkomst till Volume Licensing Service Center och bara vill testa MIM-programvaran under en begränsad tid kan du ladda ned en [utvärderings version av MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Den här program varan är inte avsedd för användning av produktion och upphör att fungera 180 dagar efter den första installationen och kan inte uppgraderas. Utvärderings versionen kräver Windows Server 2008 R2, Windows Server 2012 eller Windows Server 2012 R2 för installation.  Om du är nybörjare på MIM och lär dig att lära dig tekniken bör du tänka på att alla MIM-scenarier kräver en Active Directory domän, en Windows-Server och SQL Server som ska finnas. Om du inte har Windows Server eller SQL Server redan finns kan du försöka att [tillhandahålla en virtuell dator med SQL Server 2016 och Windows Server 2016](https://azure.microsoft.com/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Hämtar uppdateringar

När du har installerat MIM från MSI bör du installera de nödvändiga snabb korrigeringarna härnäst.

Kontrol lera [versions historiken för Identity Manager](./reference/version-history.md) för den senaste uppdaterings versionen, som innehåller en länk till nedladdnings platsen för installations korrigerings filen.

För att avgöra vilka uppdateringsfiler som behövs, visar den här tabellen komponenterna och namnet på motsvarande korrigerings fil (MSP) i en uppdatering.

| Scenario | Komponent | Namn på DVD ISO-mapp | Namn på motsvarande uppdaterings korrigerings fil |
|----------|-----------|-   |-------------------|----------|--------------|
|Synkronisering| Synkroniseringstjänsten | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| Självbetjäning | MIM-tjänsten, MIM-portalen | `Service and Portal` | `MIMService_x64*msp` |
| Självbetjäning | Tillägg och tillägg | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| Självbetjäning | Språk paket | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Åtkomst hantering (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Certifikathantering | 210 |  `Certificate Management` | `MIMCM*.msp` |
| Certifikathantering | CM Mass klient |  `CM Bulk Client` |`MIMCMBulkClient*msp` |
| Certifikathantering | CM-klient | `CM Client` |`MIMCMClient*msp` |

Se till att läsa eventuella versions anmärkningar som är associerade med uppdateringen innan du installerar MSP-filen.

Uppdateringar av [BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) distribueras inte som MSP-filer, endast som MSI-installationer.

### <a name="additional-downloads"></a>Ytterligare hämtningar

Följande hämtningar kan också vara relevanta:

- [MIM hybrid repor ting agent](https://www.microsoft.com/download/details.aspx?id=55112)

- [Allmän LDAP-koppling, allmän SQL-anslutning, Graph Connector, Lotus Domino-koppling, PowerShell-koppling, webb tjänst koppling](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Användar profil Arkiv för SharePoint-anslutning](https://www.microsoft.com/download/details.aspx?id=41164)

- Om du inte redan har en Active Directory domän och ställer in ett PAM-scenario för experimentering, se skript för [MIM-distribution i MIM 2016 SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Nästa steg

- Läs mer om scenarier som levereras i [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Läs [guiden för kapacitets planering](capacity-planning-guide.md).
- Distribuera MIM för ett [synkroniseringsförsök](microsoft-identity-manager-deploy.md) eller ett [scenario för privilegie rad åtkomst hantering](./pam/privileged-identity-management-for-active-directory-domain-services.md).

