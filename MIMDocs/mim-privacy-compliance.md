---
title: Microsoft Identity Manager-datahantering | Microsoft Docs
description: Förstå hantera för att identifiera prenumeranten och rapportera om data i miljö, vidta åtgärder för Microsoft Identity Manager-data i den angivna system baserat på operativa funktioner och krav.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/22/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: e7134e060647153b5d525e9785139eb8c38f8569
ms.sourcegitcommit: 6b2c77788a69f751fc7f5c18e678aa1fabf23ebb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38952513"
---
# <a name="microsoft-identity-manager-data-handling"></a>Datahantering för Microsoft Identity Manager 

Den här artikeln innehåller anvisningar om hur organisationen kan genom sökning, ta bort, uppdatera och rapportera åtgärder beslut som din organisation behöver du öva eller implementera över många anslutna datakällor. Innan du bestämmer dig på din metod för att ta bort eller uppdatera förstå är aktuella design och konfiguration identity manager (MIM) viktigt. Nedan visas några scenarier kunder behöver tänka på och besvara följande frågor: 

- Vilka data du behöver för du Identitetshantering som underlättar affärsprocess?
- Där aktuella data ska lagras i MIM?
- Hur kommer du använder dessa data i systemet?
- Delar du dessa data med alla externa partners data sources(Exporting)
- Vad är den auktoritativa källan för data och bearbetning av dem?
- Vad kommer din datalagring och dataradering planera på plats?
- Har du identifierat den teknik du behöver för att bearbeta och hantera data?

Du kan använda följande verktyg för att dokumentera din MIM-miljö eller att referera till implementering designdokument för att hjälpa dig att förstå aktuella MIM-miljö.
- [MIM Documentor - tillåter för att exportera aktuell konfiguration](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Söka efter och identifiera personliga data
Söka efter data i MIM är beroende av konfiguration och installation. De flesta miljöer är sammankopplade men för tydlighetens skull vi utbrott dem av övergripande komponenten.

### <a name="synchronization-service"></a>Synkroniseringstjänst

Alla data i MIM som relaterar till användare härleds från Active Directory (AD) och HR-datakällor. När du söker efter personliga data, är den första plats bör du överväga att söka i AD eller anslutna datakällor. 

Om du inte är säker källa för kan du spåra den här användaren från MIM Synchronization Service Manager-konsolen, klickar du på fältet Metaversumsökning om du vill visa identifierbar personliga data som lagras i databasen. Användare kan söka efter en viss användare eller ett attribut.

- Du utför en granskning eller Sök objekt användardata
    - Öppna tjänstklienten synkronisering
        - Med hjälp av metaversum designer om du vill se attributflöde importerar och prioritet.
![mim-sekretess-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Med hjälp av metaversumsökningen kan du söka på alla objekt och attribut i databasen ![mim-sekretess-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
När du har hittat objektet, öppnas klickar på objektet användarens profilsida. Information om objekt ger dig med detaljer om objektet, dess attribut, senast ändrad och auktoritetskälla och relaterade ansluten datakälla har härletts från management agent configuration exemplet nedan.

![mim-sekretess-efterlevnad. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM
Om du har en instans av tjänsten och portalen eller PAM installerat som är kan söka efter användare viktigt. 

Om du har installerat portalen kan du använda Användargränssnittet söka på alla attribut eller en fråga för en viss användare.

Om du bara har den tjänst server (utan portalens användargränssnitt) installerad du kan köra en Söksyntaxen baserat på [FIMAutomation PSSnapin] exempel finns [här](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM kan använda samma syntax som ovan eller du kan använda den [MIMPAM modulen](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifikt cmdleten get-pamuser att söka efter användare i PAM-miljön.

Andra reporting alternativ för att söka på tillgänglig data är i tjänsten och portalen.
- [Hybrid Reporting](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Rapportering med SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Bhold-Core-tjänst har ett gränssnitt som gör att du kan söka efter en användare eller attribut. 

![bhold-sökning](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Om du synkroniserar BHOLD med [åtkomst till management-anslutningstjänsten](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) för synkroniseringstjänsten kommer du att kunna se de anslutna objekt och attribut din skicka BHOLD-kärna.

Du kan också läsa in modulen BHOLD-rapportering.

- [BHOLD-rapportering](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Certifikathantering
Certifikatsökning management-tjänsten är inbyggd i Användargränssnittet. Administratören startar och välj ”hitta användare och visa eller hantera sin information'  

![cm-sökning](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportera personliga data
Eftersom de data som rör entiteter i MIM hämtas från flera källor, lagras de flesta data i databasen för synkroniseringstjänsten. Därför bör du exportera objekt-relaterade data från MIM Sync eller du kan fastställa ägaren av dessa data.

### <a name="synchronization-service"></a>Synkroniseringstjänst
Synkroniseringstjänster för export av data kan du bara väljer du data från sökningen Användargränssnittet och kopiera och klistra in i en csv- eller formatet du väljer. Ett annat sätt att exportera dessa data är att skapa en filbaserad MA för att ta bort aktuella data behövs om en flaggade användare av intresse. En exemplet för att använda filbaserad MA finns [här](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM
Tjänst och portal tillsammans med PAM som du kan exportera dessa data körs en Söksyntaxen baserat på [FIMAutomation PSSnapin] exempel finns [här](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) och skicka det till [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM kan använda samma syntax som ovan eller du kan använda den [MIMPAM modulen](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifikt den get-pamuser att söka efter användare i PAM-miljön och skicka det till en CSV-fil.

- [Exempel frågor till MIM-tjänsten med hjälp av PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Bhold-data kan exporteras med hjälp av bhold rapporterar modulen till formatet du väljer.

### <a name="certificate-management"></a>Certifikathantering
Certifikat management data som rör personliga data är ansluten till active directory. Administratörer kan exportera dessa data med hjälp av Active Directory powershell.

## <a name="updating-personal-data"></a>Uppdatera personliga data

Personliga data om användare eller objekt i MIM-lösningar vanligtvis härleds från användarens objekt i din organisations anslutna datakällor. Eftersom ändringar som görs till användarens profil i HR käll- eller en annan auktoritativ system för registrering, till exempel AD sedan avspeglas i MIM-synkroniseringstjänsten.

### <a name="synchronization-service"></a>Synkroniseringstjänst

För att kunna utföra hanteringsåtgärder, administratörer måste vara en del av synkroniseringsåtgärder eller administratörsdefinierad [här](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

Uppdatering av data görs genom att definiera regler för utfärdaren från källan. Konsolen Grupprinciphantering hjälper dig identifiera källan för att uppdatera den vid källan. Ett annat alternativ är att skapa synkroniseringsregel eller regel tillägg att styra data uppdateras om källan som HR data behöver fortfarande kvar. Det här är avialible stöds alternativ.

Mer information om olika sätt att uppdatera attribut finns nedan. 

- [Med tillägg för Etableringsregler](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Förstå datasynkronisering med externa system](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM

Tjänst och Portal för att inkludera PAM data kan uppdateras med hjälp av FIMAutomation eller PAM-cmdlets. Om du har på portalen kan du också direkt uppdatera genom att söka och ändra objektet. En sak till anteckning och, beroende på konfigurationen genom att uppdatera från portalen innebär inte det kommer att finnas. Eftersom auktoritetskälla är mycket beroende på övergripande konfiguration.

### <a name="bhold"></a>BHOLD

Användare kan uppdateras direkt med användargränssnittet för BHOLD-kärna eller access management-anslutningstjänsten.

### <a name="certificate-management"></a>Certifikathantering

Användare i den certifikatet management-tjänsten är en avbildning från active directory. Att uppdatera Använd Active Directory för att ändra information om objekt.

## <a name="deleting-personal-data"></a>Ta bort personliga data

>[!Note] 
> Den här artikeln innehåller råd om sätt att ta bort personliga data från Microsoft Identity Manager och kan användas för att stödja dina skyldigheter enligt GDPR. Om du letar efter allmän information om GDPR finns den i [GDPR-avsnittet i Service Trust-portalen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Data i MIM synkroniseras och uppdateras alltid från den anslutna datakällan. När ett objekt tas bort i målet, kan objektdata i MIM hanteras för säkerhetsundersökning. Ta bort objekt konfigureras per anslutna källregler eller regel extension(code) och/eller regler för borttagning av objekt.

### <a name="synchronization-service"></a>Synkroniseringstjänst
Synkronisering av tjänsten som många sätt att hantera data eller ta bort data, beroende på affärsprocesser. För att förstå visas nedan några artiklar för att förstå alternativen på Ta bort och uppdatera attribut: 

- [Förstå avetablering](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Med tillägg för Etableringsregler](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Metodtips för MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM

Vi rekommenderar för tjänsten och att du behåller standard 30 dagar-kvarhållningskonfiguration-portalen. Detta talar om för tjänsten när den tas bort, inte bara data för programbegäranden utan även objekt som måste tas bort från systemet. När den process alla data som är länkad till det här objektet tas bort detta omfattar alla data för SSPR-registrering. Det spelar i objektet tas bort konfigurationen ovan. Vi har en tabell har vi lagrar guid för objekten. Att minska den sammanlagda storleken på tabellen i build 4.4.1459 som vi har lagt till en process som kallas FIM_DeleteExpiredSystemObjectsJob information om den här processen finns [här](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-sekretess-efterlevnad-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold som de flesta system som är anslutna till tjänsten för synkronisering kan konfigureras för att ta bort när källobjektet som HR tas bort. Detta har konfigurerats på hanteringsagenten. och kontrolleras av objektborttagning regler som beskrivs under synkroniseringar service-funktioner.

Ett annat alternativ är att ta bort objektet direkt från BHOLD Core användargränssnittet. Beroende på inställningarna kan detta fungerar bra men Observera etablering logic kan återskapa den här användaren om det inte har tagits bort vid källan.
![mim-sekretess-efterlevnad-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Certifikathantering
Om du vill ta bort en användare från CM, finns ta bort användaren i active directory.

Certifikathantering som det lagras endast profil uid från Certifikattjänster med sAMAccountName i domänen. När användaren har tagits bort från användarcachen finns bara för certifikat AD ÄXLA som de har registrerat. Vi rekommenderar inte att ta bort något i databasen eftersom detta kan skada övergripande driften av miljön.

## <a name="opt-out-of-telemetry"></a>Avstår från telemetri
Tidigare versioner FIM/MIM används för att samlar in anonymiserade telemetri om varje distribution och överför dessa data via HTTPS till Microsoft-servrar. Dessa data har använts av Microsoft för att förbättra framtida versioner av FIM/MIM tidigare.

>[!Note] 
> I senare versioner av 4.5.x.x eller större data kommer samlingen att inaktiveras.

Inaktivera data samling i föregående versionen genom att köra ändra läge och avmarkera följande meddelande:

![mim-sekretess-efterlevnad-ceip. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

eller redigera registret och ange värdet till 0: (komponent) CEIP HKLM\SOFTWARE\Microsoft\Forefront identitet Manager\2010

![mim-sekretess-efterlevnad-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Nästa steg 
- [För SQL-relaterade sekretess vägledning](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [GDPR-delen av den Service Trust portalen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010-arkivet: Ramp Up - implementera Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
