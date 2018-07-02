---
title: Lösenordshantering i Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 86b8b9bdf5c6441d0708cd874742fa48b65177fa
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289371"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Lösenordshantering i Microsoft Identity Manager 2016

Att hantera lösenord för flera användarkonton är en komplicerad del i en företagsmiljö med flera datakällor. Microsoft Identity Manager 2016 (MIM) har två lösningar för lösenordshantering:

-   Lösenordssynkronisering – använder tjänsten för meddelanden om lösenordsändring (PCNS) för att samla in lösenordsändringar från Active Directory och sprida dem till andra anslutna datakällor.

-   Användarbaserad hantering av lösenordsändring – använder Windows Management Instrumentation (WMI) genom program för webbaserad helpdesk och lösenordsåterställning via självbetjäning.

Med hjälp av lösenordssynkronisering och användarbaserad hantering av lösenordsändring kan du:

-   Minska antalet olika lösenord användare måste komma ihåg.

-   Samtidigt ange eller ändra lösenord i flera konton som tillhör en användare till samma lösenord.

-   Tillåta att användare ändrar sina egna lösenord i Active Directory och sprider lösenordsändringen till andra system.

-   Eliminera risken för att skapa ett ytterligare lösenord- eller autentiseringsuppgiftslager.

-   Synkronisera lösenord mellan flera datakällor med hjälp av Active Directory som auktoritativ källa.

-   Utföra åtgärder för lösenordshantering i realtid, oberoende av MIM-åtgärder.

## <a name="password-extensions"></a>Lösenordstillägg

Hanteringsagenter för katalogservrar stöder åtgärder för lösenordsändring och lösenordsinställning som standard. För filbaserade, databasanslutna och utökningsbara hanteringsagenter, som inte har stöd för åtgärder för lösenordsändring och lösenordsinställning som standard, kan du skapa ett dynamiskt länkat .NET-bibliotek (DLL) för lösenordstillägg.
.NET DLL för lösenordstillägg anropas varje gång ett anrop görs för ändring eller inställning av lösenord för någon av dessa hanteringsagenter. Inställningar för lösenordstillägg konfigureras för dessa hanteringsagenter i Hanteraren för synkroniseringstjänsten. Mer information om hur du konfigurerar lösenordstillägg finns i FIM för utvecklare.

| Lösenordshantering stöds som standard i hanteringsagenter för: | Genom att använda ett lösenordstillägg stöds lösenordshantering även i hanteringsagenterna för: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Textfiler med attributvärdespar                                                                    |
| ADLDS (Active Directory Lightweight Directory Services)                   | Avgränsade textfiler                                                                               |
| IBM Directory Server                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Textfiler med fast bredd                                                                             |
| Sun och Netscape Directory Servers                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | LDAP Data Interchange Format (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Oracle Database                                                                                    |

## <a name="password-synchronization"></a>Lösenordssynkronisering


Lösenordssynkronisering fungerar med tjänsten om meddelanden för lösenordsändring (PCNS) på en Active Directory-domän, och tillåter att lösenordsändringar som kommer från Active Directory sprids automatiskt till andra anslutna datakällor. MIM åstadkommer detta genom att köras som en RPC-server (Remote Procedure Call) som lyssnar efter en avisering om lösenordsändring från en Active Directory-domänkontrollant. När begäran om lösenordsändring tas emot och autentiseras bearbetas den av MIM och sprids till lämpliga hanteringsagenter.

> [!IMPORTANT]
> Dubbelriktad lösenordssynkronisering stöds inte av MIM. Konfiguration av dubbelriktad lösenordssynkronisering kan skapa en slinga, som förbrukar serverresurser och har en potentiellt negativ effekt på både Active Directory och MIM.

PCNS körs på varje Active Directory-domänkontrollant. System som tar emot lösenordsaviseringar kallas mål. MIM-servern måste konfigureras som ett PCNS-mål i Active Directory innan lösenordsaviseringar skickas. PCNS-konfigurationen måste definiera en inkluderingsgrupp och eventuellt en undantagsgrupp. Dessa grupper används för att begränsa flödet av känsliga lösenord från domänen. Om du till exempel vill skicka lösenord för alla användare, men inte vill skicka administrativa lösenord, kan du välja att använda Domänanvändare som inkluderingsgrupp och Domänadminstratörer som undantagsgrupp. Mer information om att konfigurera tjänsten för meddelanden om lösenordsändring finns i [Using Password Synchronization](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx) (Använda lösenordssynkronisering)

Komponenterna som ingår i processen för lösenordssynkronisering är:

-   **Tjänsten för meddelanden om lösenordsändring (Pcnssvc.exe)** – Tjänsten för meddelanden om lösenordsändring körs på en domänkontrollant och ansvarar för att ta emot aviseringar om lösenordsändring från det lokala lösenordsfiltret, placera dem i kö för målservern som kör MIM och använda RPC för att leverera aviseringarna. Tjänsten krypterar lösenordet och ser till att lösenordet förblir skyddat tills det har levererats helt till målservern som kör MIM.

-   **Tjänstens huvudnamn (SPN)** – SPN är en egenskap för kontoobjektet i Active Directory som används av Kerberos-protokollet för att ömsesidigt autentisera PCNS och målet. SPN säkerställer att PCNS autentiserar till rätt server som kör MIM och att ingen annan tjänst kan ta emot aviseringarna om lösenordsändring. SPN skapas och tilldelas med hjälp av verktyget setspn.exe. Mer information om att konfigurera SPN finns i Using Password Synchronization (Använda lösenordssynkronisering).

-   **Filter för avisering om lösenordsändring (Pcnsflt.dll)** – Lösenordsfiltret används för att hämta lösenord i klartext från Active Directory. Det här filtret läses in av den lokala säkerhetskontrollen (LSA) på varje Windows Server-domänkontrollant som deltar i lösenordsdistributionen till en målserver som kör MIM. När filtret har installerats och domänkontrollanten har startats om börjar filtret ta emot aviseringar om lösenordsändring för lösenordsändringar som har sitt ursprung på den domänkontrollanten. Filtret för lösenordsavisering körs samtidigt med andra filter som körs på domänkontrollanten.

-   **Konfigurationsverktyg för tjänsten för meddelanden om lösenordsändring (Pcnscfg.exe)** – Verktyget pcnscfg.exe används för att hantera och underhålla konfigurationsparametrarna för tjänsten för meddelanden om lösenordsändring som lagras i Active Directory. Dessa konfigurationsparametrar, t.ex. definition av målservrar, omförsöksintervall för lösenordskön och aktivering eller inaktivering av en målserver, används när lösenordsmeddelanden autentiseras och skickas till målservern som kör MIM.
    Tjänstkonfigurationen lagras i Active Directory, så konfigurationen behöver bara uppdateras på en domänkontrollant. Active Directory replikerar ändringen till alla andra domänkontrollanter.

-   **RPC-servern (Remote Procedure Call) på servern som kör MIM** – När lösenordssynkronisering aktiveras startar RPC-servern på den server som kör MIM, vilket gör det möjligt för den att ta emot aviseringar från tjänsten för meddelanden om lösenordsändring. RPC väljer dynamiskt ett portintervall som ska användas. Om du vill att MIM ska kommunicera med Active Directory-skogen via en brandvägg måste du öppna en serie portar.

-   **DLL för lösenordstillägg** – DLL för lösenordstillägg ger ett sätt att implementera åtgärder för lösenordsändring eller lösenordsinställning med hjälp av ett regeltillägg för valfri databas, Extensible Connectivity eller filbaserad hanteringsagent.
    Det går att uppnå genom att skapa ett krypterat attributnamn endast för export med namnet "export_password" som inte finns rent faktiskt i den anslutna katalogen men som går att komma åt och ange i tillägg för etableringsregler eller som kan användas under exportattributflödet. Mer information om hur du konfigurerar lösenordstillägg finns i [FIM för utvecklare](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## <a name="preparing-for-password-synchronization"></a>Förbereda för lösenordssynkronisering

Kontrollera följande innan du konfigurerar lösenordssynkronisering för MIM- och Active Directory-miljön:

-   MIM har installerats enligt installationsanvisningarna.

-   Hanteringsagenterna för de anslutna datakällorna som ska hanteras för lösenordssynkronisering har redan skapats och objekten kopplas och synkroniseras.

Konfigurera lösenordssynkronisering:

-   Utöka Active Directory-schemat för att lägga till de klasser och attribut som krävs för att installera och köra tjänsten för meddelanden om lösenordsändring (PCNS).

-   Installera PCNS på varje domänkontrollant.

-   Konfigurera tjänstens huvudnamn (SPN) i Active Directory för MIM-tjänstkontot.

-   Konfigurera PCNS för att kommunicera med MIM-tjänsten som är mål.

-   Konfigurera hanteringsagenterna för anslutna datakällor som ska hanteras för lösenordssynkronisering.

-   Aktivera lösenordssynkronisering på MIM.

Mer information om att konfigurera lösenordssynkronisering finns i Using Password Synchronization (Använda lösenordssynkronisering).

## <a name="password-synchronization-process"></a>Processen för lösenordssynkronisering

Processen för synkronisering av en begäran om lösenordsändring från en Active Directory-domänkontrollant till andra anslutna datakällor visas i följande diagram:

1.  Användaren initierar begäran om lösenordsändring genom att trycka på Ctrl+Alt+Del. Begäran om lösenordsändring, inklusive det nya lösenordet, skickas till närmaste domänkontrollant.

2.  Domänkontrollanten registrerar begäran om lösenordsändring och meddelar filtret för meddelande om lösenordsändring (Pcnsflt.dll).

3.  Filtret för meddelande om lösenordsändring skickar begäran till tjänsten för meddelanden om lösenordsändring (PCNS).

4.  PCNS verifierar begäran om lösenordsändring och autentiserar sedan tjänstens huvudnamn (SPN) med hjälp av Kerberos och vidarebefordrar begäran om lösenordsändring i krypterad RPC till MIM-målservern.

5.  MIM validerar källdomänkontrollanten och använder sedan domännamnet för att hitta hanteringsagenten som betjänar den domänen, och använder informationen om användarkonto i begäran om lösenordsändring för att hitta motsvarande objekt i anslutarplatsen.

6.  MIM använder informationen om kopplingstabell för att fastställa de hanteringsagenter som tar emot lösenordsändringen och skickar ut lösenordsändringen till dem.

## <a name="password-synchronization-security"></a>Säkerhet för lösenordssynkronisering

Följande säkerhetsproblem med lösenordssynkronisering har åtgärdats:

-   Autentisering från lösenordskällan – när meddelandet om lösenordsändring tas emot görs Kerberos-autentisering av både MIM och källdomänkontrollanten för att säkerställa att både mottagare och avsändare är giltiga. När ett meddelande om lösenordsändring tas emot ser MIM till att den som anropar har ett konto i domänkontrollantcontainern för den domän den tillhör.

-   Misslyckad lösenordssynkronisering till en måldatakälla till följd av en osäker anslutning – synkroniseringen misslyckas om hanteringsagenten har konfigurerats för att kräva en säker anslutning men det inte går att hitta en sådan.
    Synkroniseringen sker fortfarande om hanteringsagenten har konfigurerats för att tillåta ej säkra anslutningar. Ej säkra anslutningar bör endast tillåtas efter att riskerna med detta har undersökts och förståtts.

-   Säker lagring av lösenord – MIM lagrar bara krypterade lösenord tillfälligt. Alla lösenord som tas emot av MIM under en åtgärd för meddelande om lösenordsändring krypteras så snart de går in i MIM-processen.
    De krypteras när de skickas framgångsrikt till den anslutna måldatakällan och minnet som lagrar lösenordet rensas omedelbart. Om åtgärden inte lyckas skriva till den anslutna måldatakällan lagras det krypterade lösenordet tills alla omförsök har gjorts och rensas sedan från minnet.

-   Säkra lösenordsköer – lösenord som lagras i PCNS-lösenordsköer krypteras tills de levereras.

## <a name="password-synchronization-error-recovery-scenarios"></a>Återställningsscenarier vid fel med lösenordssynkronisering

Helst synkroniseras ändringen utan fel när en användare ändrar ett lösenord. Följande scenarier beskriver hur MIM återställer efter vanliga synkroniseringsfel:

-   **Misslyckat lösenordsmeddelande från Active Directory till MIM** – Detta kan inträffa om nätverket ligger nere eller om servern som kör MIM inte är tillgänglig. PCNS behåller meddelandet om lösenordsändring i kön lokalt på domänkontrollanten. PCNS försöker skicka meddelandet igen enligt konfigurationen för återförsöksintervall.

-   **Lösenordssynkronisering till en måldatakälla misslyckades** – Detta kan också inträffa om nätverket ligger nere eller om måldatakällan inte är tillgänglig.
    Meddelandet om lösenordsändring placeras i kö och nya försök görs enligt hanteringsagentens konfiguration för återförsöksintervall. Alla lösenord krypteras när de lagras för återförsök och raderas när åtgärden lyckas eller gränsen för återförsök uppnås.

-   **Aktivering av en varm standby-server som kör MIM efter ett fel** – Om det blir fel på den primära servern som kör MIM kan du konfigurera en varm standby-server för lösenordssynkronisering och aktivera den utan att några lösenordsändringar går förlorade. Mer information finns i [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx) (MIISactivate: serveraktiveringsverktyg)

En del fel är så allvarliga att det inte är sannolikt att åtgärden lyckas oavsett hur många återförsök som genomförs. I dessa fall loggas en felhändelse och processen stoppas. Nya försök görs inte för följande händelser:

| Händelse | Allvarlighetsgrad    | Description                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Information | En åtgärd för lösenordssynkronisering utfördes inte på grund av att tidsstämpeln var för gammal.                                                                      |
| 6921  | Fel       | Åtgärden för lösenordssynkronisering bearbetades inte på grund av att lösenordshantering inte har aktiverats på hanteringsagenten som är mål.                                |
| 6922  | Fel       | Åtgärden för lösenordssynkronisering bearbetades inte på grund av att lösenordshantering inte har konfigurerats på hanteringsagenten som är mål.                             |
| 6923  | Varning     | Åtgärden för lösenordssynkronisering bearbetades eftersom anslutarplatsen som är mål inte gick att hitta i den anslutna katalogen.                  |
| 6927  | Fel       | Åtgärden för synkronisering av lösenord misslyckades eftersom lösenordet inte uppfyller målsystemets lösenordsprincip.                                      |
| 6928  | Fel       | Åtgärden för synkronisering av lösenord misslyckades eftersom lösenordstillägget för målhanteringsagenten inte är konfigurerad för att ha stöd för åtgärder för lösenordsinställning. |

## <a name="user-based-password-change-management"></a>Användarbaserad hantering av lösenordsändring

MIM tillhandahåller två webbprogram som använder Windows Management Instrumentation (WMI) för att återställa lösenord. Liksom med lösenordssynkronisering aktiverar du lösenordshantering när du konfigurerar hanteringsagenten i hanteringsagentens designer. Information om lösenordshantering och WMI finns i MIM för utvecklare.

MIM skapar två säkerhetsgrupper under installationen som specifikt stöder lösenordshanteringsåtgärderna:

-   FIMSyncBrowse – Medlemmar i den här gruppen har behörighet att samla in information om en användares konton under sökåtgärder med WMI-frågor.

-   FIMSyncPasswordSet – Medlemmar i den här gruppen har behörighet att utföra åtgärder för kontosökning, lösenordsinställning och lösenordsändring med hjälp av gränssnitten för lösenordshantering med WMI.
