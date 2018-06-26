---
title: Microsoft Identity Manager datahantering | Microsoft Docs
description: Förstå Microsoft Identity Manager av data för att identifiera prenumeranten och rapportera om data i miljö vidta åtgärder i den angivna system baserat på operativa funktioner och krav.
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
ms.openlocfilehash: 6bcf9ab26ba38f3c6eefbdb315d4975320a597b9
ms.sourcegitcommit: d82c05aa814015fc6bdab37133eafec3366bea3b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/26/2018
ms.locfileid: "34449703"
---
# <a name="microsoft-identity-manager-data-handling"></a>Microsoft Identity Manager datahantering 

Den här artikeln får du veta hur organisationen kan genom Sök, ta bort, uppdatera och rapportera operations beslut som din organisation behöver öva eller implementerar över flera anslutna datakällor. Innan du beslutar om din metod för att ta bort eller uppdatera förstå är aktuella design och konfiguration identity manager (MIM) viktigt. Nedan visas några scenarier kunder måste överväga och besvara följande frågor: 

- Vilka data behöver du Identitetshantering att affärsprocess ha?
- Var aktuella data ska lagras i MIM?
- Hur ska du använder dessa data i systemet?
- Du dela informationen med alla externa partners data sources(Exporting)
- Vad är den auktoritära källan för data och bearbetning av det?
- Vad dina datalagring och Databorttagning planerar på plats?
- Har du identifierat den teknik som du måste bearbeta och hantera data?

Du kan använda följande verktyg för att dokumentera din MIM-miljö eller skjuta upp till din implementering designdokument som hjälper dig att förstå aktuella MIM-miljö.
- [MIM Documentor - kan du exportera konfigurationen som aktuella](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Söker efter och identifiera personliga data
Söka efter data i MIM är beroende av konfiguration och installation. De flesta miljöer är sammankopplade men för tydlighetens skull vi utbrott dem av övergripande komponenten.

### <a name="synchronization-service"></a>Synkroniseringstjänst

Alla data i MIM som relaterar till användare härleds från Active Directory (AD) och HR-datakällor. När du söker efter personliga data, är den första plats bör du söker AD eller anslutna datakällor. 

Om du inte är säker på att källan för utfärdare du spårar den här användaren från MIM Synchronization Service Manager-konsolen, klickar du på fältet metaversumsökningen om du vill visa identifierbar personlig information som lagras i databasen. Användare kan söka efter en viss användare eller ett attribut.

- Att utföra en granskning eller Sök objekt användardata
    - Öppna tjänstklienten synkronisering
        - Med hjälp av metaversumsökningen designer kan du se attributflöde importerar och prioritet.
![mim-sekretess-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Med hjälp av metaversumsökningen kan du söka på alla objekt och attribut i databasen ![mim-sekretess-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
När du har hittat objektet, öppnas klickar på objektet användarprofilsidan. Information om objekt ger du med omfattande information om objektet i dess attribut senast ändrad och källan för utfärdare och relaterade anslutna datakällan har härletts från management agent configuration exemplet nedan.

![mim-sekretess-efterlevnad. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM
Om du har en instans av tjänsten och portalen eller PAM installerat som är kan söka efter användare viktigt. 

Du kan använda Användargränssnittet för att söka efter alla attribut eller en fråga för en viss användare om du har installerat portalen.

Om du har den tjänst server (utan portalens användargränssnitt) installerad du kan köra en sökning syntax baserat på [FIMAutomation PSSnapin] exempel hitta [här](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM kan använda samma syntax ovan eller du kan använda den [MIMPAM modulen](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifikt cmdlet get-pamuser söka efter användare i PAM-miljön.

Andra reporting alternativ för att söka efter tillgängliga data är i tjänsten och portalen.
- [Hybrid Reporting](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Rapportering med SCSM](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Bhold kärntjänsten har ett gränssnitt som gör att du kan söka efter en användare eller attribut. 

![bhold-sökning](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Om du vill synkronisera BHOLD med [åtkomst till management-anslutningstjänsten](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-access-management-connector-install) för synkroniseringstjänsten kommer du att kunna se anslutna användarobjekt och attribut din skickas till BHOLD-kärna.

Du kan också ladda BHOLD rapportmodulen.

- [BHOLD-rapportering](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Certifikathantering
Certifikatsökning management-tjänsten är inbyggd i Användargränssnittet. Administratören kommer att starta och välj den 'hitta användare och visa eller hantera information om deras'  

![cm-sökning](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportera personliga data
Eftersom de data som rör entiteter i MIM hämtas från flera källor, lagras de flesta data i databasen för synkroniseringstjänsten. Därför ska du exportera objekt-relaterade data från MIM Sync eller du kan ange ägare av den här informationen.

### <a name="synchronization-service"></a>Synkroniseringstjänst
Synkroniseringstjänster för export av data markerar data från sökningen UI och kopiera och klistra in i ett CSV- eller önskade format. Ett annat sätt att exportera dessa data är att skapa en filbaserad MA för att ta bort aktuella data som behövs om en flaggade användare av intresse. En exmaple med filbaserad MA hittar [här](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM
Tjänsten och portalen tillsammans med PAM som du kan exportera den här informationen att köra en söksyntax baserat på [FIMAutomation PSSnapin] exempel hitta [här](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) och skicka det till [csv](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM kan använda samma syntax ovan eller du kan använda den [MIMPAM modulen](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifikt den get-pamuser att söka efter användare i PAM-miljön och skicka det till en CSV-fil.

- [Exempel frågar MIM-tjänsten med hjälp av PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Bhold-data kan exporteras med bhold rapporterar modulen till önskat format.

### <a name="certificate-management"></a>Certifikathantering
Certifikat management data som rör personliga data är ansluten till active directory. En administratör kan exportera dessa data med hjälp av Active Directory powershell.

## <a name="updating-personal-data"></a>Uppdaterar personliga data

Personliga data om användare eller objekt i MIM-lösningar vanligen härleds från användarobjektet i din organisation anslutna datakällor. Eftersom alla ändringar i användarprofilen i HR käll- eller en annan auktoritativ system för posten, till exempel AD visas sedan i MIM-synkroniseringstjänsten.

### <a name="synchronization-service"></a>Synkroniseringstjänst

För att kunna utföra hanteringsåtgärder administratörer måste vara en del av synkroniseringsåtgärder eller administratör som har definierats för [här](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10)).

Uppdatering av data görs genom att definiera regler för utfärdare från källan. Hanteringskonsolen identifierar källan för att uppdatera den vid källan. Ett annat alternativ är att skapa synkroniseringsregel eller regel-tillägget för att kontrollera data uppdateras om datakällan som HR data behöver fortfarande kvar. Dessa är avialible stöd för alternativ.

Mer information om olika sätt att uppdatera attributet finns nedan. 

- [Med hjälp av regler tillägg](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Förstå datasynkronisering med externa system](https://docs.microsoft.com/en-us/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM

Tjänsten och portalen för att inkludera PAM data kan uppdateras med hjälp av FIMAutomation eller PAM-cmdlets. Om du har portalen kan du också direkt uppdatera genom att söka och ändra objektet. En sak att Observera och beroende på konfiguration uppdateras bara från portalen innebär inte kommer att finnas kvar. Som källa för utfärdare är mycket beroende av övergripande konfiguration.

### <a name="bhold"></a>BHOLD

Användare kan uppdateras direkt med BHOLD Core användargränssnittet eller access management-anslutningstjänsten.

### <a name="certificate-management"></a>Certifikathantering

Användare i tjänsten för hantering av certifikat är en reflection från active directory. Uppdatera Använd Active Directory för att ändra objektet uppgifter.

## <a name="deleting-personal-data"></a>Ta bort personliga data

>[!Note] 
> Den här artikeln innehåller information om olika sätt att ta bort personliga data från Microsoft Identity Manager och kan användas för att stödja din skyldigheter enligt BNPR. Om du letar efter allmän information om BNPR finns i [BNPR avsnitt av tjänsten förtroende portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Data i MIM synkroniseras och alltid uppdateras från den anslutna datakällan. När ett objekt tas bort i mål-kan objektets data i MIM underhållas för tillämpning av säkerhet undersökning. Ta bort objekt konfigureras per ansluten data källregler eller regel extension(code) och/eller regler för borttagning av objekt.

### <a name="synchronization-service"></a>Synkroniseringstjänst
Synkronisering tjänsten så många sätt att hantera data eller ta bort data beroende på affärsprocesser. Förstå finns nedan att, vissa artiklar för att förstå alternativ för borttagning och uppdatering attribut: 

- [Förstå avetablering](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Med hjälp av regler tillägg](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Metodtips för MIM](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Tjänst och Portal / PAM

Det rekommenderas för tjänsten & Portal att du behåller standard 30 dagar resurs kvarhållning systemkonfigurationen. Detta talar om tjänsten när den tas bort, inte bara data för begäran utan även alla objekt som ska tas bort från systemet. När den process raderas alla data som är länkade till objektet Detta omfattar alla data för SSPR-registrering. Det spelar i objektet borttagning konfigurationen ovan. Vi har en tabell har vi lagrar guid för objekt. Att minska den totala storleken på tabellen i build 4.4.1459 som vi har lagt till en process som kallas FIM_DeleteExpiredSystemObjectsJob information på den här processen kan hittas [här](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-sekretess-kompatibilitet-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold som de flesta system som är ansluten till synkroniseringstjänsten kan konfigureras för att ta bort när källobjektet som HR tas bort. Detta är konfigurerat på hanteringsagenten. och kontrolleras av objektborttagning regler som beskrivs under synkroniseringar tjänstens funktioner.

Ett annat alternativ är att ta bort användarobjektet direkt från BHOLD Core användargränssnittet. Beroende på installationsprogrammet kan detta fungerar men Observera etablering logik kan återskapa den här användaren om inte bort vid källan.
![mim-sekretess-kompatibilitet-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Certifikathantering
Om du vill ta bort en användare från CM är ta bort användaren i active directory.

Certifikathantering som det lagras endast profil uid från Certifikattjänster med domänen sAMAccountName. När användaren har tagits bort från användarens cache finns bara för certifikat AD växla som de har registrerats. Vi rekommenderar inte ingenting tas bort i databasen som detta kan skada övergripande driften av miljön.

## <a name="opt-out-of-telemetry"></a>CEIP telemetri
Tidigare versioner av FIM/MIM används för att samlar in anonymiserade telemetri om varje distribution och överför dessa data via HTTPS till Microsoft-servrar. Dessa data används av Microsoft för att förbättra kommande versioner av FIM/MIM tidigare.

>[!Note] 
> Samlingen i senare versioner av 4.5.x.x eller större data kommer att inaktiveras.

Inaktivera data-samlingen i tidigare versioner kör ändra läge och avmarkerar du följande fråga:

![mim-sekretess-kompatibilitet-ceip. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

Redigera registret och ange värdet till 0: (komponent) CEIP HKLM\SOFTWARE\Microsoft\Forefront identitet Manager\2010

![mim-sekretess-kompatibilitet-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Nästa steg 
- [För SQL-relaterade sekretess vägledning](https://docs.microsoft.com/en-us/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [BNPR avsnitt av tjänsten förtroende-portalen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 Arkiv: Ramp Up - implementera Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)