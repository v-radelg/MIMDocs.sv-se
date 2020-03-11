---
title: Microsoft Identity Manager data hantering | Microsoft Docs
description: Förstå Microsoft Identity Manager data hantering för att identifiera prenumeranten och rapportera om data i miljön, vidta åtgärder i det aktuella systemet baserat på operativa funktioner och krav.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: e95cf26b62e582eaa3c07c40e551bc5930d3b1b0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044114"
---
# <a name="microsoft-identity-manager-data-handling"></a>Microsoft Identity Manager data hantering 

I den här artikeln får du vägledning om hur organisationer kan fatta beslut som kan tillämpas på många anslutna data källor.  Detta kan uppnås genom åtgärderna Sök, ta bort, uppdatera och rapportera.  Innan du bestämmer dig för att ta bort eller uppdatera är det viktigt att förstå den aktuella utformningen och konfigurationen av ditt Identity Manager-system (MIM). 

Nedan visas några scenarier som kunderna behöver tänka på och svara på följande frågor: 

- Vilka data behöver du för identitets hantering för att hjälpa till med affärs processen?
- Var kommer aktuella data att lagras i MIM?
- Hur kommer du att använda dessa data i systemet?
- Delar dessa data med alla externa partners data källor (export)
- Vad är den auktoritativa källan för data och bearbetning av den?
- Vad gör data lagringen och data borttagnings planen på plats?
- Har du identifierat all teknik du behöver för att bearbeta och hantera data?

För att hjälpa dig att förstå en aktuell MIM-miljö kan du använda följande verktyg för att dokumentera MIM-miljön, eller Överlåt dig till implementerings design dokumenten.
- [MIM-Dokumentor – tillåter export av den aktuella konfigurationen](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Söka efter och identifiera personliga data
Sökning av data i MIM är beroende av konfigurationen och konfigurationen. De flesta miljöer är sammankopplade, men för tydlighets skull kan vi dela upp dem med hög nivå komponent.

### <a name="synchronization-service"></a>Synkroniseringstjänst

Alla data i MIM som relaterar till användare härleds från Active Directory (AD) och data källor i HR. När du söker efter personliga data bör du överväga att söka är AD eller anslutna data källor. 

Om du inte är säker på källan till utfärdaren kan du spåra den här användaren från MIM Synchronization Service Manager-konsolen, klicka på Sök fältet metaversum för att visa de identifierbara personliga data som lagras i databasen. Användare kan söka efter en speciell användare eller ett attribut.

- Utföra en granskning eller sökning efter användar objekts data
    - Öppna tjänsten synkroniseringstjänst
        - Med hjälp av metaversum Designer kan du se import och prioritet för Attribute Flow.
![MIM-Privacy-compliance_1. PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Med hjälp av metaversum-sökningen kan du söka efter objekt och attribut i databasen ![MIM-Privacy-compliance_2. PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
När du har hittat objektet öppnas sidan användar profil när du klickar på objektet. Objekt informationen ger dig fullständig information om objektet, dess attribut, senast ändrad och källan till utfärdaren och relaterad ansluten data källa härledda från konfigurations exemplet för hanterings agenten nedan.

![MIM-sekretess-efterlevnad. KÄLLFIL](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Tjänst och Portal/PAM
Om du har en instans av tjänsten och portalen eller PAM installerat som kan söka efter användare är det viktigt. 

Om du har installerat portalen kan du använda användar gränssnittet för att söka efter attribut eller frågor för en viss användare.

Om du bara har installerat tjänst servern (utan Portal användar gränssnitt) kan du köra en söksyntax baserat på [FIMAutomation PSSnapin], exempel som du hittar [här](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM kan använda samma syntax ovan eller så kan du använda [MIMPAM-modulen](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) särskilt cmdleten Get-pamuser för att söka efter användaren i pam-miljön.

Andra rapporterings alternativ för att söka efter tillgängliga data finns i tjänsten och portalen.
- [Hybrid rapportering](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Rapportera med SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
BHOLD Core Service har ett användar gränssnitt som gör att du kan söka efter en användare eller attribut. 

![BHOLD-sökning](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Om du synkroniserar BHOLD med [Access Management Connector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) för synkroniseringstjänst kommer du att kunna se de anslutna användar objekten och de attribut som skickas till BHOLD-kärnan.

Du kan också läsa in modulen BHOLD rapportering.

- [BHOLD-rapportering](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Certifikathantering
Sök tjänsten för certifikat hantering är inbyggd i användar gränssnittet. Administratören startar och väljer "hitta användare och Visa eller hantera deras information"  

![cm-sökning](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportera personliga data
Eftersom data som är relaterade till entiteter i MIM härleds från flera källor lagras de flesta data i databasen för synkroniseringstjänsten. Därför bör du exportera objekt relaterade data från MIM-synkronisering eller så kan du bestämma vem som är ägare till dessa data.

### <a name="synchronization-service"></a>Synkroniseringstjänst
Med Sync Services för export av data väljer du bara data från Sök gränssnittet och kopierar och klistrar in dem i ett CSV-eller önskat format. Ett annat sätt att exportera dessa data är att skapa ett filbaserat MA för att släppa aktuella data som behövs för en flaggad användare av intresse. Det går att hitta en exemplet med hjälp av filbaserad [ma.](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/)


### <a name="service-and-portal--pam"></a>Tjänst och Portal/PAM
Tjänst och Portal tillsammans med PAM du kan exportera dessa data kör en söksyntax baserat på [FIMAutomation PSSnapin], exempel som du hittar [här](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) och pipe till [CSV](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM kan använda samma syntax ovan eller så kan du använda [MIMPAM-modulen](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) särskilt get-pamuser för att söka efter användaren i pam-miljön och skicka en pipe till en CSV.

- [Exempel som frågar MIM-tjänsten med PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
BHOLD-data kan exporteras med BHOLD-rapporterings modulen till önskat format.

### <a name="certificate-management"></a>Certifikathantering
Certifikat hanterings data relaterade till personliga data är anslutna till Active Directory. En administratör kan exportera dessa data med hjälp av Active Directory PowerShell.

## <a name="updating-personal-data"></a>Uppdatera personliga data

Person uppgifter om användare eller objekt i MIM-lösningar härleds vanligt vis från användarens objekt i organisationens anslutna data källor. Eftersom ändringar som görs i användar profilen i HR-källan, eller ett annat auktoritativt post system, till exempel AD, avspeglas i MIM-synkroniseringstjänsten.

### <a name="synchronization-service"></a>Synkroniseringstjänst

För att utföra hanterings åtgärder måste administratörer vara en del av de åtgärder för synkronisering eller administratör som definierats [här](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

Uppdatering av data görs genom att definiera regler från auktoritets källan. Hanterings konsolen hjälper till att identifiera källan till behörighet att uppdatera den vid källan. Ett annat alternativ är att skapa en Synkroniseringsregel eller regel omfattning för att kontrol lera att data uppdateras om källan som HR-data fortfarande måste vara kvar. Detta är avialible alternativ som stöds.

Mer information om olika sätt att uppdatera attribut finns nedan. 

- [Använda regel tillägg](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Förstå datasynkronisering med externa system](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Tjänst och Portal/PAM

Tjänsten och portalen för att inkludera PAM-data kan uppdateras med hjälp av FIMAutomation-eller PAM-cmdletar. Om du har portalen kan du också direkt uppdatera genom att söka efter och ändra objektet. En sak att notera och beroende på konfiguration som helt enkelt uppdaterar från portalen innebär inte att den är kvar. Som auktoritets källa är mycket beroende av den övergripande konfigurationen.

### <a name="bhold"></a>BHOLD

Användare kan uppdateras direkt med BHOLD Core-användargränssnittet eller åtkomst hanterings anslutningen.

### <a name="certificate-management"></a>Certifikathantering

Användare i certifikat hanterings tjänsten är en reflektion från Active Directory. Om du vill uppdatera använder Active Directory för att ändra objekt information.

## <a name="deleting-personal-data"></a>Ta bort personliga data

>[!Note] 
> Den här artikeln innehåller rikt linjer för hur du tar bort personliga data från Microsoft Identity Manager och som kan användas för att stödja dina skyldigheter enligt GDPR. Om du letar efter allmän information om GDPR finns den i [GDPR-avsnittet i Service Trust-portalen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Data i MIM synkroniseras och uppdateras alltid från den anslutna data källan. När ett objekt tas bort i mål, kan objektets data i MIM upprätthållas i samband med säkerhets undersökningen. Objekt borttagning har kon figurer ATS per ansluten data käll regler eller regel tillägg (kod) och/eller objekt borttagnings regler.

### <a name="synchronization-service"></a>Synkroniseringstjänst
Synkroniseringstjänsten på många sätt för att hantera data eller ta bort data beroende på affärs processer. För att förstå, nedan finns några artiklar som hjälper dig att förstå alternativen för att ta bort och uppdatera attribut: 

- [Förstå avetablering](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Använda regel tillägg](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Bästa praxis för MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Tjänst och Portal/PAM

Vi rekommenderar att du behåller tjänst & portalen att du behåller standard konfigurationen för 30 dagar system resurs kvarhållning. Detta meddelar tjänsten när den tas bort, inte bara begär ande data, utan även objekt som behöver rensas från systemet. När processen har tagits bort tas alla data som är länkade till objektet bort, inklusive alla SSPR registrerings data. Detta spelas upp i konfigurationen för borttagning av objekt ovan. Vi har en tabell som vi lagrar GUID för objekten. För att minska den totala storleken på tabellen i build-4.4.1459 vi har lagt till en process som heter FIM_DeleteExpiredSystemObjectsJob information om den här processen hittar du [här](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![MIM-Privacy-Compliance-SRRC. KÄLLFIL](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

BHOLD som de flesta system som är anslutna till synkroniseringstjänsten kan konfigureras att ta bort när källobjektet som HR tas bort. Detta är konfigurerat på hanterings agenten. och kontrol leras av reglerna för borttagning av objekt enligt beskrivningen i funktionerna i synkroniseringen.

Ett annat alternativ är att ta bort användar objekt direkt från BHOLD Core-användargränssnittet. Beroende på installationen kan detta fungera, men antecknings etablerings logiken kan återskapa den här användaren om den inte togs bort vid källan.
![MIM-Privacy-Compliance-bholdr. PNG-](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Certifikathantering
Ta bort användaren från CM genom att ta bort användaren i Active Directory.

Certifikat hantering eftersom den endast kommer att lagra profil-UID från certifikat tjänster med domän-sAMAccountName. När användaren har tagits bort från AD finns det bara användarens cacheminne för certifikaten äxla de har registrerats. Vi rekommenderar inte att du tar bort något i databasen eftersom det kan orsaka övergripande skada på driften av miljön.

## <a name="opt-out-of-telemetry"></a>Inaktivera telemetri
Tidigare versioner av FIM/MIM används för att samla in anonymiserats telemetri om varje distribution och överföring av data via HTTPS till Microsoft-servrar. Dessa data användes av Microsoft för att hjälpa till att förbättra framtida versioner av FIM/MIM tidigare.

>[!Note] 
> I senare versioner av 4.5. x. x eller en större data insamling inaktive ras.

Om du vill inaktivera data insamling i tidigare versioner kör du ändrings läge och avmarkerar du följande prompt:

![MIM-Privacy-Compliance-CEIP. KÄLLFIL](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

eller redigera registret och Ställ in värdet på 0: (komponent) CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![MIM-Privacy-Compliance-ceip2. KÄLLFIL](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Nästa steg 
- [Råd om SQL-relaterad sekretess](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Avsnittet GDPR i service Trust-portalen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010-Arkiv: ramp up – implementera Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
