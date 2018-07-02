---
title: BHOLD SP1-installationen | Microsoft Docs
description: BHOLD SP1 dokumentationen
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 11cde4e3b2779f9c32d9849a47713acf5f120b3c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289704"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Installationsguiden för Microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) är en uppsättning program som, när den används med Microsoft Identity Manager 2016 SP1 (MIM), lägger till effektiva rollhantering, analyser och attestering i MIM. Microsoft BHOLD-programsvit SP1 består av följande moduler:

- BHOLD kärnor
- Access Management-anslutningstjänsten
- BHOLD FIM/MIM-integrering
- BHOLD modellen Generator
- BHOLD Analytics
- BHOLD-rapportering
- BHOLD Hälsoattestering


> [!NOTE]
> **Gäller för**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Det här dokumentet beskriver

Det här dokumentet beskriver hur du planerar distributionen BHOLD för att uppfylla företagets behov och installera varje BHOLD-modul. För varje modul, relevanta maskinvara, infrastruktur och programvarukrav, preinstallation nätverkskonfiguration, information som krävs under installationen och viktig, eventuella detaljerade instruktioner finns.

## <a name="pre-requisite-knowledge"></a>Förutvärdering knowledge

Det här dokumentet förutsätter att du har en grundläggande förståelse för hur du installerar programvara på server-datorer. Det förutsätts även att du har grundläggande kunskaper i Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) och Microsoft SQL Server 2008 databasprogram. En beskrivning av hur du skapar och konfigurerar beroende tekniker, till exempel AD DS och FIM ligger utanför omfånget för den här dokumentationen. Information om funktioner som Microsoft BHOLD-moduler utför finns [Microsoft BHOLD suite begrepp guiden](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Målgrupp

Det här dokumentet är avsett för IT-planerare systemarkitekter, beslutsfattare teknik, konsulter, infrastruktur planerare och IT-personal som planerar att distribuera Microsoft BHOLD-programsvit.

## <a name="bhold-infrastructure-considerations"></a>Överväganden för BHOLD-infrastruktur

BHOLD och FIM används oftast i en stor infrastruktur-miljö. Du kan anpassa BHOLD- och FIM-arkitekturen för att uppfylla dina specifika affärsbehov. Följande avsnitt innehåller några arkitektur lösningar. Den här översikten är inte en omfattande lista över alla möjliga alternativ men förslag om hur du kan distribuera BHOLD i nätverket.
 
Det här avsnittet beskrivs i följande avsnitt:

- Arkitektur för enskild server
- Dubbla serverarkitektur
- Tvålagers-arkitektur
- SQL Server-rekommendationer

### <a name="single-server-architecture"></a>Arkitektur för enskild server

För distribution i mindre organisationer eller för utveckling, kan du installera BHOLD och FIM på samma server som SQL Server och AD DS som visas i följande bild.
 
![Arkitektur för enskild server](media/bhold-installation-guide/single.png)

När BHOLD-programsvit SP1 och FIM-portalen har installerats tillsammans på en enskild server, måste du skapa olika värden alias (CNAME-post eller A-poster) i DNS för BHOLD och FIM. Detta gör att separat tjänstens huvudnamn (SPN) som ska skapas för BHOLD och FIM-tjänster. Mer information finns i [BHOLD-kärninstallation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Anvisningar om hur du installerar FIM i en enstaka serverkonfiguration finns [vanliga konfigurationer för komma-igång guider](https://technet.microsoft.com/library/ff575965.aspx) i Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Dubbla serverarkitektur

Installera BHOLD kärnor och FIM på separata servrar ger bättre prestanda och flexibilitet för medelstora organisationer som inte kräver en mer komplex distribution, till exempel som tillhandahålls av flera skikt arkitekturerna. Följande bild visar BHOLD och FIM installerad på sina egna servrar. FIM-servern även kör SQL Server för att förse BHOLD och FIM databastjänster. FIM-synkroniseringstjänsten körs på FIM-servern synkroniserar ändringar mellan FIM och BHOLD-databaser. Observera att om slutanvändaren självbetjäning krävs BHOLD FIM integrationsmodulen måste installeras på samma server som FIM-tjänsten och FIM-portalen. BHOLD FIM integrationsmodulen kräver att FIM-tjänsten och modulen BHOLD FIM-integrering är installerade på samma server.

![Dubbla serverarkitektur](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> Rapporteringsfunktionen för modulen BHOLD FIM-Integration kräver BHOLD och FIM-databaserna installeras på samma SQL Server-instansen och BHOLD-kontot måste ha åtkomsträttigheter till FIM-tjänstdatabasen.

### <a name="two-tier-architecture"></a>Tvålagers-arkitektur

I de flesta miljöer, särskilt de där prestanda är viktigt, bör du köra BHOLD-programsvit SP1, FIM och SQL Server på separata servrar (tvålagers-arkitektur). Med en tvålagers-arkitektur dedikerat minne och processorresurser för varje nivå. Följande bild visar ett sätt att konfigurera en tvålagers-arkitektur. FIM-synkroniseringstjänsten körs på FIM-servern synkroniserar ändringar mellan FIM och BHOLD-databaser. Observera att om slutanvändaren självbetjäning krävs BHOLD FIM integrationsmodulen måste installeras på samma server som FIM-tjänsten och portalen.

![tvålagers-arkitektur](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server-rekommendationer

Om du distribuerar BHOLD i en stor organisation rekommenderar att du följer dessa riktlinjer för att konfigurera Microsoft SQL Server-databasen:

- Distribuera SQL Server på en server som är separat från FIM eller BHOLD-tjänster.
- Isolera loggfilen från datafilen på nivån fysisk disk.
- Om du använder RAID för att tillhandahålla redundans för lagring, använder du RAID-nivå 10 (1 + 0). Använd inte RAID-nivå 5.
- Se till att konfigurera rätt inställningar när du använder mer än 2 GB fysiskt minne för den server som kör SQL Server.
- För optimala prestanda för BHOLD, använder du Microsoft SQL Server 2008 R2 eller senare.

Mer information om metodtips för SQL Server finns [lagring Top 10 bästa praxis](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) i Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Betrodda certifikat listan uppdatering

Windows kan konfigureras för att verifiera certifikatkedjor innan du startar en tjänst. En tjänst kan inte starta om tjänsten körbara koden har signerats med ett certifikat som inte finns med i listan över betrodda certifikat (TCL) på servern på sådana system. Microsoft BHOLD-programsvit SP1 programvaran är kod som signeras med hjälp av en certifikatkedja som har sitt ursprung med Microsoft Root Certificate myndigheten 2010-certifikat för kodsignering.
Windows kan konfigureras för att hämta rotcertifikat från Microsoft via en Internet-anslutning. På en frånkopplad, men Windows Server innehåller bara de certifikat som fanns i root program samtidigt innan Windows släpptes. I versioner av Windows Server före Windows Server 2010 innehåller certifikaten inte rotcertifikat som behövs för att verifiera certifikatkedja BHOLD-programsvit SP1 kodsignering. Om du planerar att installera en eller flera Microsoft BHOLD-programsvit SP1-moduler på ett system som inte kanske har en uppdaterad TCL, måste du hämta och installera rot-uppdateringspaketet eller använda grupprincipinställningar för att installera rot uppdateringspaketet innan du installerar en BHOLD-programsvit SP1 modul. Mer information finns i [Windows root certificate program medlemmar](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Installera BHOLD Suite SP1 på Windows Server 2012/2016 nödvändiga steg 

![IIS installerar BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Om du installerar BHOLD Suite SP1 på Windows Server 2012 eller 2016 webbsidor BHOLD blir inte tillgängliga förrän du ändra filen applicationHost.config i ```C:\Windows\System32\inetsrv\config```. I den ```<globalModules>``` lägger du till ```preCondition="bitness64``` till posten som börjar ```<add name="SPNativeRequestModule"``` så att det står på följande sätt:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

När du redigerar och sparar filen, kör iisreset-kommando för att återställa IIS-servern.


## <a name="upgrading-bhold-suite"></a>Uppgradera BHOLD-programsvit

Du kan inte uppgradera en befintlig installation av BHOLD-programsvit. I stället måste du avinstallera en befintlig installation av BHOLD-programsvit innan du kan uppdatera BHOLD-moduler. Om du har en befintlig BHOLD roll modell kan du använda den när du installerar uppdaterade BHOLD-kärnmodul uppgradera BHOLD-databas. Mer information finns i [ersätter BHOLD-programsvit med BHOLD-programsvit SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Nästa steg

- [Referens för BHOLD-utvecklare](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD-versionshistorik](../reference/version-bhold-history.md)
