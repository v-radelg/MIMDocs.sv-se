---
title: "Vad är PAM för ADDS? | Microsoft Docs"
description: "Läs mer om Privileged Access Management och hur det kan hjälpa dig att hantera och skydda din Active Directory-miljö."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/27/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experiment_id: kgremban_images
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 7751722f4f75dcdd4af5f578060b4e1b144073ef

---

# <a name="privileged-access-management-for-active-directory-domain-services"></a>Privileged Access Management för Active Directory Domain Services
Privileged Access Management (PAM) hjälper organisationer att begränsa privilegierad åtkomst i befintliga Active Directory-miljöer.

![PAM-stegen: förbereda, skydda, använda, övervaka – diagram](media/MIM_PIM_SetupProcess.png)

Genom att fokusera på en cykel som förbereder, skyddar och övervakar miljön uppnår Privileged Access Management två mål:

- Återupprätta kontroll över en komprometterad Active Directory-miljö genom att hantera en separat skyddsmiljö som inte påverkas inte av skadliga attacker.  
- Minska risken för att autentiseringsuppgifter stjäls genom att isolera användningen av privilegierade konton.

> [!NOTE]
> PAM är en instans av [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) som implementeras med hjälp av Microsoft Identity Manager (MIM).

## <a name="what-problems-does-pam-help-solve"></a>Vilka problem löser PAM?
Idag är åtkomsten till resurser i en Active Directory-miljö är ett betydande problem för företag. Särskilt oroande är nyheter om sårbarheter, obehöriga privilegieutökningar och andra typer av obehörig åtkomst, som pass-the-hash, pass-the-ticket, spjutfiske och Kerberos-angrepp.

Idag är det alldeles för enkelt för angripare att få tag i domänadministratörers autentiseringsuppgifter och alldeles för svårt att upptäcka angreppen efteråt. Målet med PAM är att minska angriparnas tillfällen att ta sig in och att öka din kontroll och kunskap om miljön.

PAM gör det svårare för angripare att ta sig in i ett nätverk och få åtkomst till privilegierade konton. PAM lägger till skydd till privilegierade grupper som kontrollerar åtkomsten på många olika domänanslutna datorer och program på dessa datorer. Det ger också ökad övervakning, mer synlighet och mer detaljerade kontroller så att organisationer kan se vilka deras privilegierade administratörer är och vad de gör. Med PAM får organisationer större insyn i hur administratörskonton används i miljön.

## <a name="how-is-pam-set-up"></a>Hur konfigureras PAM?
PAM bygger på principen för just-in-time-administration, som är relaterat till [JEA (just enough administration)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA är ett Windows PowerShell-verktyg som definierar en uppsättning kommandon för att utföra privilegierade aktiviteter och en slutpunkt där administratörer kan få behörighet att köra dessa kommandon. I JEA beslutar administratör att användare med en viss behörighet kan utföra en viss uppgift. Varje gång en behörig användare behöver utföra aktiviteten aktiveras behörigheten. Behörigheten upphör efter en angiven tidsperiod, så att obehöriga användare inte kan få åtkomst.

PAM-installationen och -driften består av fyra steg.


1.  **Förbereda**: identifiera vilka grupper i den befintliga skogen som har betydande behörighet. Återskapa dessa grupper utan medlemmar i skyddsskogen.

2.  **Skydda**: Konfigurera livscykel- och autentiseringsskydd, till exempel Multi-Factor Authentication (MFA), för när användare begär just-in-time-administration. MFA hjälper till att förhindrar programmässiga attacker från skadlig programvara och påföljande stölder av autentiseringsuppgifter.

3.  **Använda**: när autentiseringskraven har uppfyllts och en begäran har godkänts, läggs ett användarkonto tillfälligt till i en privilegierad grupp i skyddsskogen. Under en förinställd tidsperiod har administratören alla privilegier och åtkomstbehörigheter som är tilldelade till gruppen. Därefter tas kontot bort från gruppen.

4.  **Övervaka**: PAM lägger till granskningar, aviseringar och rapporter för begäran om privilegierad åtkomst. Du kan granska historiken för privilegierad åtkomst och se vem som utfört uppgiften. Du kan bestämma om aktiviteten är giltig eller inte och enkelt identifiera obehörig aktivitet, som försök att lägga till användare direkt i en privilegierad grupp i den ursprungliga skogen. Den här åtgärden är inte bara viktig för att upptäcka skadliga program utan också för att spåra interna angrepp.

## <a name="how-does-pam-work"></a>Hur fungerar PAM?
PAM är baserat på nya funktionerna i AD DS, särskilt de för domänautentisering och -auktorisering, och nya funktioner i Microsoft Identity Manager. Med PAM separeras behöriga konton från en befintlig Active Directory-miljö. När ett privilegierat konto behöver användas, måste det först begäras och sedan godkännas. När det godkänts ges det privilegierade kontot behörighet via en extern huvudgrupp i en ny skyddsskog i stället för i användarens eller programmets aktuella skog. Genom att använda en skyddsskog får organisationen större kontroll, till exempel över när en användare kan vara medlem i en privilegierad grupp och hur användaren måste autentiseras.

Active Directory, MIM-tjänsten och andra delar av den här lösningen kan också distribueras i en konfiguration med hög tillgänglighet.

I följande exempel visas hur PIM fungerar i detalj.

![PIM-process och deltagare – diagram](media/MIM_PIM_howitworks.png)

Skyddsskogen utfärdar tidsbegränsade gruppmedlemskap, vilka i sin tur utfärdar tidsbegränsade biljettbeviljande biljetter. Kerberos-baserade program och tjänster kan respektera och tillämpa dessa biljettbeviljande biljetter om apparna och tjänsterna finns i skogar som litar på skyddsskogen.

Löpande användarkonton måste inte flyttas till en ny skog. Det gäller också för datorer, program och deras grupper. De förblir där de är i en befintlig skog. Studera exemplet med en organisation som är bekymrad över dagens cybersäkerhetsproblemen, men inte har några omedelbara planer på att uppgradera serverinfrastrukturen till nästa version av Windows Server. Den organisationen kan fortfarande dra nytta av den här kombinerade lösningen genom att använda MIM och en ny skyddsskog, och får bättre kontroll över åtkomsten till befintliga resurser.

PAM ger följande fördelar:

-   **Isolera/ange omfång för behörigheter**: användare har inte behörighet för konton som också används för icke-privilegierade uppgifter som e-post- och internetanvändning. Användarna måste begära behörighet. Begäranden godkänns och nekas utifrån MIM-principer som definierats av en PAM-administratör. Privilegierad åtkomst är inte tillgänglig förrän en begäran har godkänts.

-   **Step-up och proof-up**: Nya autentiserings- och auktoriseringsutmaningar för att hantera livscykeln för olika administratörskonton. Användaren kan begära att ett administratörskonto utökas och den begäran går igenom MIM-arbetsflöden.

-   **Ytterligare loggning**: förutom de inbyggda MIM-arbetsflödena finns det ytterligare loggning för PAM som identifierar begäran, hur den auktoriserades och eventuella händelser efter godkännandet.

-   **Anpassningsbart arbetsflöde**: MIM-arbetsflöden kan konfigureras för olika scenarier och flera olika arbetsflöden kan användas baserat på parametrarna för användaren eller begärda roller.

## <a name="how-do-users-request-privileged-access"></a>Hur begär användare privilegierad åtkomst?
Det finns några olika sätt en användare kan skicka en begäran på, bland annat via:  
- API:t för MIM-tjänsternas webbtjänster  
- En REST-slutpunkt  
- Windows PowerShell (`New-PAMRequest`)

## <a name="what-workflows-and-monitoring-options-are-available"></a>Vilka arbetsflöden och övervakningsalternativ finns?
Om vi till exempel antar att en användare var medlem av en administrativ grupp innan PIM konfigurerades. Som en del av PIM-konfigurationen tas användaren bort från den administrativa gruppen och en princip skapas i MIM. Principen anger att om användaren begär administratörsbehörighet och autentiseras av MFA godkänns begäran och ett särskilt konto för användaren läggs till i den privilegierade gruppen i skyddsskogen.

Förutsatt att begäran godkänns kommunicerar åtgärdsarbetsflödet direkt med skyddsskogens Active Directory för att placera användaren i en grupp. När Lisa till exempel begär att administrera HR-databasen, läggs Lisas administratörskonto till i den privilegierade gruppens skyddsskog inom några sekunder. Hennes administratörskontos medlemskap i gruppen går ut efter en begränsad tid. Med Windows Server Technical Preview associeras medlemskapet med en tidsgräns i Active Directory. Med Windows Server 2012 R2 i skyddsskogen tvingas tidsgränsen av MIM.

> [!NOTE]
> När du lägger till en ny användare i en grupp måste ändringen replikeras till andra domänkontrollanter i skyddsskogen. Replikeringsfördröjning kan påverka möjligheten för användare att komma åt resurser. Mer information om replikeringsfördröjning finns i [Så här fungerar replikeringstopologin i Active Directory](https://technet.microsoft.com/library/cc755994.aspx).
>
> Som kontrast utvärderas utgångna länkar i realtid av Security Accounts Manager (SAM). Även om tillägg av medlemmar måste replikeras av domänkontrollanten som tar emot åtkomstbegäran, utvärderas borttagning av medlemmar omedelbart på vilken domänkontrollant som helst.

Det här arbetsflödet är speciellt avsett för dessa administratörskonton. Administratörer (och skript) som bara behöver tillfällig åtkomst för privilegierade grupper kan begära precis den åtkomsten. MIM loggar begäran och ändringarna i Active Directory, och du kan visa dem i loggboken eller skicka data till företagets övervakningslösningar, till exempel System Center 2012 – gransknings- och insamlingstjänsten för Operations Manager och andra verktyg från tredje part.



<!--HONumber=Nov16_HO2-->


