---
title: Definiera privilegierade roller för PAM | Microsoft Docs
description: Bestäm vilka privilegierade roller som ska hanteras och ange hanteringsprincipen för varje roll.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f05769a7d1db38ecde200e18e45c6ca29a75b756
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044046"
---
# <a name="define-roles-for-privileged-access-management"></a>Definiera roller för Privileged Access Management

Med Privileged Access Management kan du tilldela användare privilegierade roller som de kan aktivera vid behov för just-in-time-åtkomst. Rollerna definieras manuellt och upprättas i skyddsmiljön. I den här artikeln får du vägledning genom processen att avgöra vilka roller som ska hanteras via PAM och hur du definierar dem med rätt behörighet och begränsningar.

Ett enkelt sätt att definiera roller för Privileged Access Management är att samla all information i ett kalkylblad. Lista roller i rollerna och använd kolumnerna till att identifiera styrningskrav och -behörigheter.

Styrnings kraven varierar beroende på befintliga identitets-och åtkomst principer eller krav för efterlevnad. De parametrar som ska identifieras för varje roll kan innehålla:

- Rollen ägare.
- Kandidat användare som kan ha rollen
- De autentiserings-, godkännande-eller meddelande kontroller som ska associeras med rollen.

Rollbehörigheterna beror på de program som hanteras. I den här artikeln används Active Directory som ett exempelprogram, och behörigheter delas upp i två kategorier:

- De som krävs för att hantera själva Active Directory-tjänsten (t.ex. konfigurera replikeringstopologin)

- De som krävs för att hantera data som lagras i Active Directory (t.ex. skapa användare och grupper)

## <a name="identify-roles"></a>Identifiera roller

Börja med att identifiera alla de roller som du kanske vill hantera med PAM. På kalkylbladet har varje potentiell roll en egen rad.

Titta närmare på varje program som ska hanteras för att hitta lämpliga roller:

- Är programmet i [nivå 0, nivå 1 eller nivå 2](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- Vilka är de behörigheter som påverkar programmets sekretess, integritet eller tillgänglighet?
- Har programmet beroenden på andra komponenter i systemet? Har det exempelvis beroenden för databaser, nätverk, säkerhets infrastruktur, virtualisering eller värd plattform?

Bestämma hur du vill gruppera de programbehoven. Du vill ha roller med tydliga gränser som bara ger tillräcklig behörighet att utföra vanliga administrativa uppgifter i programmet.

Du ska alltid skapa roller efter den uppgift som kräver lägst privilegium. Det kan baseras på aktuella (eller planerade) organisativa ansvarsområden för användare och omfattar den behörighet användarens uppgifter kräver. Det kan också omfatta de privilegier som förenklar driften utan att medföra risker.

Några andra saker du ska överväga när du anger behörighetsomfånget för en roll är:

- Hur många personer arbetar inom en viss roll? Om det är minst två personer kan den vara för snävt snävare definierad för att användbar eller så har du definierat en viss persons uppgifter.

- Hur många roller har en person? Kommer användarna att kunna välja rätt roll för uppgiften?

- Skulle användarna och hur de samverkar med program vara förenliga med hanteringen av privilegierad åtkomst?

- Går det att separera administration och granskning, så att en användare i en administrativ roll inte kan radera granskningsposter för sina åtgärder?

## <a name="establish-role-governance-requirements"></a>Upprätta krav för rollstyrning

När du identifierar kandidatroller börjar du med att fylla i kalkylbladet. Skapa kolumner för de krav som är relevanta för organisationen. Några krav du måste överväga är:

- Vem är rollägaren som ska ansvara för ytterligare definition av rollen, välja behörigheter och hantera styrningsinställningarna för rollen?

- Vilka är rollinnehavarna (användarna) som ska utföra rollens uppgifter?

- Vilken åtkomstmetod (beskrivs i nästa avsnitt) skulle vara lämplig för rollinnehavarna?

- Krävs manuellt godkännande av en rollägare när en användare aktiverar rollen?

- Krävs det meddelanden för när en användare aktiverar rollen?

- Bör det genereras en avisering eller ett meddelande i ett SIEM-system när rollen används i spårningssyfte?

- Är det nödvändigt att begränsa användarna som aktiverar rollen till att bara kunna logga in på datorer där åtkomst krävs för att utföra rollens uppgifter, och där värdens verifiering är tillräcklig för att skydda privilegierna/autentiseringsuppgifterna från missbruk?

- Måste rollinnehavarna tillhandahållas en dedikerad arbetsstation för administration?

- Vilka programbehörigheter (se listan med exempel för AD nedan) är associerade med rollen?

## <a name="select-an-access-method"></a>Välj en åtkomstmetod

Det kan finnas flera roller i ett Privileged Access Management-system med samma behörigheter som tilldelats dem. Detta kan inträffa om olika användar grupper har särskilda krav för åtkomst styrning. En organisation kan till exempel använda olika principer för sina heltidsanställda och för IT-anställda i en annan organisation.

I vissa fall kan en användare tilldelas permanent till en roll. I så fall behöver de inte begära eller aktivera en roll tilldelning. Exempel på scenarier med permanent tilldelning:

- Ett hanterat tjänstkonto i den befintliga skogen

- Ett användar konto i den befintliga skogen, med autentiseringsuppgifter som hanteras utanför PAM. Detta kan vara ett "Break glas"-konto. Break glas-kontot kan ha en roll som "domän/DC-underhåll" för att åtgärda problem, till exempel förtroende-och hälso problem i DOMÄNKONTROLLANTen. Som ett rast glas konto skulle den ha rollen permanent tilldelad med ett fysiskt skyddat lösen ord)

- Ett användar konto i den administrativa skogen som autentiserar med ett lösen ord. Detta kan vara en användare som behöver permanent administratörs behörighet på dygnet och logga in från en enhet som inte stöder stark autentisering.

- Ett användarkonto i den administrativa skogen, med ett smartkort eller virtuellt smartkort (till exempel ett konto med ett offlinesmartkort som behövs för sällsynta underhållsuppgifter)

I guiden [Aktivera med hjälp av Azure MFA](use-azure-mfa-for-activation.md) hittar organisationer som är oroliga för stöld och missbruk av autentiseringsuppgifter anvisningar om hur de konfigurerar MIM till att kräva ytterligare out-of-band-kontroller vid rollaktivering.

## <a name="delegate-active-directory-permissions"></a>Tilldela Active Directory-behörigheter

Windows Server skapar automatiskt standardgrupper som "Domänadministratörer" när nya domäner skapas. Med de här grupperna blir det enklare att komma igång och de kan vara lämpliga för mindre organisationer. Större organisationer, eller de som kräver mer isolering av administrativa privilegier, bör tömma grupperna och ersätta dem med grupper som ger detaljerade behörigheter.

En begränsning med gruppen Domänadministratörer är att den inte kan ha medlemmar från en extern domän. En annan begränsning är att den ger behörighet till tre olika funktioner:

- Hantera själva tjänsten Active Directory
- Hantera de data som lagras i Active Directory
- Aktivera fjärrinloggning till domänanslutna datorer.

I stället för standard grupper som domän administratörer skapar du nya säkerhets grupper som endast tillhandahåller de behörigheter som krävs. Du bör sedan använda MIM för att dynamiskt tillhandahålla administratörs konton med dessa grupp medlemskap.

### <a name="service-management-permissions"></a>Behörighet för tjänsthantering

Följande tabell innehåller exempel på behörigheter som är relevanta att inkludera i roller för att hantera AD.

| Roll | Beskrivning |
| ---- | ---- |
| Hantering av domäner/domänkontrollanter | Medlemskap i gruppen Domain\Administrators gör det möjligt att felsöka och ändra domänkontrollantens operativ system. Åtgärder som att befordra en ny domänkontrollant till en befintlig domän i skogens och AD-rollens delegering.
|Hantera virtuella domänkontrollanter | Hantera domänkontrollantens virtuella datorer med hjälp av programvara för hantering av virtualisering. Det här privilegiet kan tilldelas via fullständig kontroll över alla virtuella datorer i hanteringsverktyget och i funktionerna för rollbaserad åtkomstkontroll. |
| Utöka schemat | Hantera schemat, inklusive att lägga till nya objektdefinitioner, ändra behörigheter till schemaobjekt och ändra schemats standardbehörigheter för objekttyper |
| Säkerhetskopiera Active Directory-databasen | Gör en säkerhetskopia av Active Directory-databasen i sin helhet, inklusive alla hemligheter som tilldelats domänkontrollanten och domänen. |
| Hantera förtroenden och funktionsnivåer | Skapa och ta bort förtroenden för externa domäner och skogar. |
| Hantera platser, undernät och replikering | Hantera replikeringstopologiobjekten i Active Directory och ändra platser, undernät och platslänkobjekt samt starta replikeringsåtgärder |
| Hantera grupprincipobjekt | Skapa, ta bort och ändra grupprincipobjekt för hela domänen |
| Hantera zoner | Skapa, ta bort och ändra DNS-zoner och objekt i Active Directory |
| Ändra organisationsenheter på nivå 0 | Ändra organisationsenheter på nivå 0 och ingående objekt i Active Directory |

### <a name="data-management-permissions"></a>behörighet för datahantering

Följande tabell innehåller exempel på behörigheter som är relevanta att inkludera i roller för att hantera eller använda data som lagras i AD.

| Roll | Beskrivning |
| ---- | ---- |
| Ändra organisationsenheten för administration på nivå 1                 | Ändra organisationsenheter som innehåller administrationsobjekt på nivå 1 i Active Directory |
| Ändra organisationsenheten för administration på nivå 2                 | Ändra organisationsenheter som innehåller administrationsobjekt på nivå 2 i Active Directory |
| Kontohantering: Skapa/Ta bort/Flytta | Ändra standardanvändarkonton                                      |
| Kontohantering: Återställa, låsa upp       | Återställa lösenord och låsa upp konton                                  |
| Säkerhetsgrupp: Skapa, ändra          | Skapa och ändra säkerhetsgrupper i Active Directory              |
| Säkerhetsgrupp: Ta bort                 | Ta bort säkerhetsgrupper i Active Directory                         |
| Hantering av grupprincipobjekt                         | Hantera alla grupprincipobjekt i domänen/skogen som inte påverkar servrar på nivå 0             |
| Ansluta till dator/lokal administration                    | Lokala administrativa rättigheter för alla arbetsstationer                               |
| Ansluta till server/lokal administration                   | Lokala administrativa rättigheter för alla servrar                                    |

## <a name="example-role-definitions"></a>Exempel på rolldefinitioner

Valet av roll definitioner beror på vilken nivå av servrar som hanteras. Detta beror också på valet av program som hanteras. Program som Exchange eller tredje parts företags produkter som SAP kommer ofta att ta med sina egna ytterligare roll definitioner för delegerad administration.

Följande avsnitt innehåller exempel för vanliga företagsscenarier.

### <a name="tier-0---administrative-forest"></a>Nivå 0 – Administrativ skog

Roller som är lämpliga för konton i skyddsmiljön kan vara:

- Nödåtkomst till den administrativa skogen
- Nödfallsadministratörer: användare som är administratörer för den administrativa skogen
- Användare som är administratörer för produktionsskogen
- Användare som är tilldelade begränsad administrativ behörighet till program i produktionsskogen

### <a name="tier-0---enterprise-production-forest"></a>Nivå 0 – Företagsproduktionsskog

Roller som är lämpliga för att hantera produktionsskogskonton och -resurser på nivå 0 kan vara:

- Nödåtkomst till produktionsskogen
- Administratörer av grupprinciper
- DNS-administratörer
- PKI-administratörer
- Administratörer för AD-topologi och -replikering
- Virtualiseringsadministratörer för nivå 0-servrar
- Lagringsadministratörer
- Administratörer av program mot skadlig kod för nivå 0-servrar
- SCCM-administratörer för SCCM på nivå 0
- SCOM-administratörer för SCOM på nivå 0
- Reservadministratörer för nivå 0
- Användare av out-of-band-styrenheter och styrenheter för huvudkortshantering (för KVM eller lights-out-hantering) som är anslutna till värdar på nivå 0

### <a name="tier-1"></a>Nivå 1

Roller för hantering och säkerhetskopiering av servrar på nivå 1 kan vara:

- Serverunderhåll
- Virtualiseringsadministratörer för servrar på nivå 1
- Säkerhetsskannerkonto
- Administratörer av program mot skadlig kod för nivå 1-servrar
- SCCM-administratörer för SCCM på nivå 1
- SCOM-administratörer för SCOM på nivå 1
- Reservadministratörer för servrar på nivå 1
- Användare av out-of-band-styrenheter och styrenheter för huvudkortshantering (för KVM eller lights-out-hantering) till värdar på nivå 1

Rollerna för att hantera företagsprogram på nivå 1 kan också omfatta:

- DHCP-administratörer
- Exchange-administratörer
- Administratörer för Skype för företag
- Administratörer för SharePoint-grupper
- Administratörer för en molntjänst, t.ex. ett företags webbplats eller ett offentligt DNS
- Administratörer för HCM-system, finansiella och juridiska system

### <a name="tier-2"></a>Nivå 2

Roller för hantering av icke-administrativa användare och datorer kan vara:

- Kontoadministratörer
- Supportavdelningen
- Administratörer för säkerhetsgrupper
- Kontorssupport för arbetsstationer

## <a name="next-steps"></a>Nästa steg

- [Referensmaterial för skydd av privilegierad åtkomst](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Aktivera med hjälp av Azure MFA](use-azure-mfa-for-activation.md)
