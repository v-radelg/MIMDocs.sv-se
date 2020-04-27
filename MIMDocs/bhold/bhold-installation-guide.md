---
title: Installation av BHOLD SP1 | Microsoft Docs
description: Dokumentation om installation av BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fb3cf6e5b00c1bd0c01d86aff474dc2ff28c2815
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042261"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Installations guide för Microsoft BHOLD Suite SP1 (6,0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) är en samling program som, när de används med Microsoft Identity Manager 2016 SP1 (MIM), lägger till effektiv roll hantering, analys och attestering till MIM. Microsoft BHOLD Suite SP1 består av följande moduler:

- BHOLD-kärna
- Åtkomst hanterings anslutning
- BHOLD FIM/MIM-integrering
- BHOLD modell Generator
- BHOLD-analys
- BHOLD-rapportering
- BHOLD-attestering


> [!NOTE]
> **Gäller för**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Vad det här dokumentet täcker

Det här dokumentet beskriver hur du planerar din BHOLD-distribution så att den uppfyller dina affärs behov och installerar varje BHOLD-modul. För varje modul är relevant maskin vara, infrastruktur och program varu krav, för konfigurering av nätverks konfiguration, information som krävs under installationen och postinstallation steg, om det finns några, detaljerad information.

## <a name="pre-requisite-knowledge"></a>Nödvändig kunskap

Det här dokumentet förutsätter att du har en grundläggande förståelse för hur du installerar program vara på Server datorer. Det förutsätter också att du har grundläggande kunskaper om Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) och Microsoft SQL Server 2012-databasprogram vara. En beskrivning av hur du konfigurerar och konfigurerar beroende tekniker, till exempel AD DS och FIM, är inte den här dokumentationens omfattning. Information om vilka funktioner som Microsoft BHOLD-moduler utför finns i [Microsoft BHOLD Suite Concept guide](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Målgrupp

Det här dokumentet är avsett för IT-planerare, system arkitekter, teknik besluts fattare, konsulter, infrastruktur planerare och IT-personal som planerar att distribuera Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>BHOLD infrastruktur överväganden

Oftast används BHOLD och FIM i en stor infrastruktur miljö. Du kan skräddarsy din BHOLD-och FIM-arkitektur för att uppfylla dina specifika affärs behov. Följande avsnitt innehåller några möjliga arkitektur lösningar. Den här översikten är inte en omfattande lista över alla möjliga alternativ, men du får förslag på hur du kan distribuera BHOLD i nätverket.
 
I det här avsnittet beskrivs följande ämnen:

- Arkitektur med en server
- Arkitektur med dubbla servrar
- Arkitektur på två nivåer
- Rekommendationer för SQL Server

### <a name="single-server-architecture"></a>Arkitektur med en server

För distribution i små organisationer eller i utvecklings syfte kan du installera BHOLD och FIM på samma server som SQL Server och AD DS, som du ser i följande bild.
 
![Arkitektur för enskild server](media/bhold-installation-guide/single.png)

När BHOLD Suite SP1 och FIM-portalen installeras tillsammans på en enskild server, måste du skapa olika värdnamn (CNAME eller A-poster) i DNS för BHOLD och för FIM. Detta gör att separata SPN-namn (Service Principal Name) kan skapas för BHOLD-och FIM-tjänsterna. Mer information finns i [BHOLD Core-installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Information om hur du installerar FIM i en konfiguration med en enda server finns i [gemensam konfiguration för komma igång guider](https://technet.microsoft.com/library/ff575965.aspx) i Microsoft TechNet-biblioteket.

### <a name="dual-server-architecture"></a>Arkitektur med dubbla servrar

Att installera BHOLD Core och FIM på separata servrar ger bättre prestanda och flexibilitet för medel stora organisationer som inte kräver en mer komplex distribution, t. ex. det som tillhandahålls av arkitekturer på flera nivåer. Följande bild visar BHOLD och FIM som är installerade på sina egna servrar. FIM-servern kör också SQL Server för att tillhandahålla databas tjänster till BHOLD och FIM. FIM-synkroniseringstjänsten som körs på FIM-servern synkroniserar ändringar mellan FIM-och BHOLD-databaserna. Observera att om självbetjäningen för slutanvändare krävs måste BHOLD FIM-integrering installeras på samma server som FIM-tjänsten och FIM-portalen. BHOLD FIM-integrering kräver att FIM-tjänsten och BHOLD FIM-integrerings modulen är installerade på samma server.

![Arkitektur med dubbla servrar](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> Rapporterings funktionen i modulen BHOLD FIM-integrering kräver att BHOLD-och FIM-databaser installeras på samma SQL Server instans och att BHOLD-tjänstkontot måste ha behörighet till FIM-tjänstedatabasen.

### <a name="two-tier-architecture"></a>Arkitektur på två nivåer

I de flesta miljöer, särskilt de där prestanda är viktiga, bör du köra BHOLD Suite SP1, FIM och SQL Server på separata servrar (arkitektur på två nivåer). Med en arkitektur på två nivåer är minnes-och processor resurser dedikerade för varje nivå. Följande bild visar ett möjligt sätt att konfigurera en arkitektur på två nivåer. FIM-synkroniseringstjänsten som körs på FIM-servern synkroniserar ändringar mellan FIM-och BHOLD-databaserna. Observera att om självbetjäningen för slutanvändare krävs måste BHOLD FIM-integrering installeras på samma server som FIM-tjänsten och-portalen.

![arkitektur på två nivåer](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Rekommendationer för SQL Server

Om du distribuerar BHOLD i en stor organisation rekommenderar vi starkt att du följer dessa rikt linjer för att konfigurera Microsoft SQL Server-databasen:

- Distribuera SQL Server på en server som är separat från alla FIM-eller BHOLD-tjänster.
- Isolera logg filen från data filen på den fysiska disk nivån.
- Om du använder RAID för att tillhandahålla redundans använder du RAID-nivå 10 (1 + 0). Använd inte RAID-nivå 5.
- Se till att konfigurera rätt inställningar när du använder mer än 2 GB fysiskt minne för servern som kör SQL Server.

Mer information om SQL Server bästa praxis finns i de [10 bästa praxisen för lagring](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) i Microsoft TechNet-biblioteket.

### <a name="trusted-certificates-list-update"></a>Uppdatering av lista över betrodda certifikat

Windows kan konfigureras för att verifiera certifikat kedjor innan en tjänst startas. På sådana system kan en tjänst inte starta om den körbara koden för tjänsten har signerats med ett certifikat som inte finns i listan över betrodda certifikat (TCL) på servern. Microsoft BHOLD Suite SP1-programmet är kod signerat med en kedja av kod signerings certifikat som kommer från certifikat utfärdaren Microsoft Root Certificate Authority 2010.
Windows kan konfigureras för att hämta rot certifikat från Microsoft via en Internet anslutning. På ett frånkopplat system innehåller dock Windows Server bara de certifikat som fanns i rot programmet vid en tidpunkt innan Windows släpptes. I versioner av Windows Server före Windows Server 2010 innehåller dessa certifikat inte det rot certifikat som krävs för att verifiera certifikat kedjan för kod signering i BHOLD Suite SP1. Om du planerar att installera en eller flera Microsoft BHOLD Suite SP1-moduler på ett system som kanske inte har en uppdaterad TCL måste du hämta och installera rot uppdaterings paketet, eller använda grupprincip för att installera rot uppdaterings paketet innan du installerar en BHOLD Suite SP1-modul. Mer information finns i [Windows Root Certificate program members](https://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Steg för att installera BHOLD Suite SP1 i Windows Server 2012/2016 

![IIS-installation BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Om du installerar BHOLD Suite SP1 på Windows Server 2012 eller 2016 kommer BHOLD-webbsidor inte att vara tillgängliga förrän du ändrar filen applicationHost. config som finns i ```C:\Windows\System32\inetsrv\config```. I ```<globalModules>``` avsnittet lägger ```preCondition="bitness64``` du till posten som börjar ```<add name="SPNativeRequestModule"``` så att den ser ut så här:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

När du har redigerat och sparat filen kör du kommandot iisreset för att återställa IIS-servern.


## <a name="upgrading-bhold-suite"></a>Uppgradera BHOLD Suite

Det går inte att uppgradera en befintlig installation av BHOLD Suite. I stället måste du avinstallera en befintlig installation av BHOLD Suite innan du kan uppdatera BHOLD-moduler. Om du har en befintlig BHOLD-roll modell kan du uppgradera BHOLD-databasen och använda den när du installerar den uppdaterade BHOLD Core-modulen. Mer information finns i [ersätta BHOLD Suite med BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Nästa steg

- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
