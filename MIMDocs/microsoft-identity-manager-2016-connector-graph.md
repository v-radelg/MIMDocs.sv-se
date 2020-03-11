---
title: Microsoft Identity Manager koppling för Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Identity Manager Connector för Microsoft Graph möjliggör hantering av livs cykeln för externa användares AD-konto. I det här scenariot har en organisation bjudit in gäster till sin Azure AD-katalog och vill ge gästarna till gång till lokala Windows-integrerade autentiseringar eller Kerberos-baserade program
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 462b649ca02519e5af5c3b1243506a74efa7052a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044267"
---
# <a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Microsoft Identity Manager koppling för Microsoft Graph


## <a name="summary"></a>Sammanfattning 


[Microsoft Identity Manager Connector för Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) möjliggör ytterligare integrerings scenarier för Azure AD Premium kunder.  IT-ytor i MIM Sync-metaversum har ytterligare objekt som hämtats från [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 och beta.

## <a name="scenarios-covered"></a>Scenarier som omfattas


### <a name="b2b-account-lifecycle-management"></a>Hantering av B2B-konto livs cykel


Det initiala scenariot för Microsoft Identity Manager Connector för Microsoft Graph är som en koppling för att automatisera AD DS-kontots livs cykel hantering för externa användare. I det här scenariot synkroniserar en organisation medarbetare till Azure AD från AD DS med hjälp av Azure AD Connect och har även bjudit in gäster i sin Azure AD-katalog. Att bjuda in ett gäst resultat i ett externt användar objekt som finns i den organisationens Azure AD-katalog, som inte finns i den organisationens AD DS. Sedan vill organisationen ge gästen åtkomst till den lokala Windows-integrerade autentiseringen eller Kerberos-baserade program, via [Azure AD-programproxyn](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) eller andra gateway-mekanismer. Azure AD-programproxyn kräver att varje användare har sitt eget AD DS-konto, i syfte att identifiera och delegering.  

Om du vill lära dig hur du konfigurerar MIM Sync för att automatiskt skapa och underhålla AD DS-konton för gäster, kan du läsa mer i artikeln [Azure AD Business-to-Business (B2B) tillsammans med MIM 2016 SP1 med Azure Application Proxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md)när du har läst instruktionerna i den här artikeln.  Den artikeln visar de synkroniseringsinställningar som krävs för anslutningen.

### <a name="other-identity-management-scenarios"></a>Andra scenarier för identitets hantering


Anslutningen kan användas för andra särskilda identitets hanterings scenarier som omfattar skapa, läsa, uppdatera och ta bort användar-, grupp-och kontakt objekt i Azure AD, utöver synkronisering av användare och grupper till Azure AD. När du utvärderar potentiella scenarier bör du tänka på att det inte går att använda den här anslutningen i ett scenario, vilket skulle resultera i ett data flöde som överlappar, faktisk eller potentiell synkroniseringskonflikt med en Azure AD Connect distribution.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) är den rekommenderade metoden för att integrera lokala kataloger med Azure AD genom att synkronisera användare och grupper från lokala kataloger till Azure AD.  Azure AD Connect har många fler synkroniseringsfunktionen och aktiverar scenarier som till exempel lösen ord och tillbakaskrivning av enheter, vilket inte är möjligt för objekt som skapats av MIM. Om data hämtas till AD DS kan du till exempel se till att den utesluts från Azure AD Connect försöker matcha objekten tillbaka till Azure AD-katalogen.  Den här kopplingen kan inte användas för att göra ändringar i Azure AD-objekt, som har skapats av Azure AD Connect.



## <a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Förbereder att använda anslutningen för Microsoft Graph

### <a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Auktorisera anslutningen för att hämta eller hantera objekt i Azure AD-katalogen


1.  Anslutningen kräver att ett webbapp/API-program skapas i Azure AD, så att den kan auktoriseras med lämpliga behörigheter för att använda Azure AD-objekt via Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Bild 1. Ny program registrering

2.  Öppna det skapade programmet i Azure Portal och spara program-ID: t, som ett klient-ID som du kan använda senare på sidan för MA: s anslutnings barhet:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Bild 2. Program-ID

3.  Generera en ny klient hemlighet genom att öppna alla inställningar-\> nycklar. Ange en nyckel beskrivning och välj Needful varaktighet. Spara ändringar. Ett hemligt värde är inte tillgängligt när du har lämnat sidan.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Bild 3. Ny klient hemlighet

4.  Lägg till "Microsoft Graph API" i programmet genom att öppna "nödvändiga behörigheter".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Bild 4. Lägg till nytt API

Följande behörighet ska läggas till i programmet så att den kan använda "Microsoft Graph API", beroende på scenariot:

| Åtgärd med objekt | Behörighet krävs                                                                  | Behörighets typ |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importera grupp          | `Group.Read.All` eller `Group.ReadWrite.All`                                                | Program     |
| Importera användare           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` eller `Directory.ReadWrite.All` | Program     |

Mer information om de behörigheter som krävs finns [här](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Ge programmet de behörigheter som krävs.


## <a name="installing-the-connector"></a>Installerar anslutningen


6.  Kontrol lera att du har följande på synkroniseringstjänsten innan du installerar anslutningen: 

 - Microsoft .NET 4.5.2-ramverk eller senare
 - Microsoft Identity Manager 2016 SP1 och måste använda Hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) eller senare.

7. Anslutnings programmet för Microsoft Graph, förutom andra anslutningar för Microsoft Identity Manager 2016 SP1, är tillgängligt som en nedladdning från [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Starta om MIM-synkroniseringstjänsten.
 
## <a name="connector-configuration"></a>Anslutnings konfiguration



9.  I Synchronization Service Manager användar gränssnitt väljer du **kopplingar** och **skapa**.
Välj **Graph (Microsoft)**  , skapa en koppling och ge den ett beskrivande namn.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. I användar gränssnittet för MIM-synkroniseringstjänsten anger du program-ID och genererad klient hemlighet. Varje hanterings agent som kon figurer ATS i MIM Sync bör ha sitt eget program i Azure AD för att undvika att köra import parallellt för samma program.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Bild 5. Anslutnings sida

Sidan anslutning (bild 5) innehåller den Graph API-version som används och klient namn. Klient-ID och klient hemlighet representerar program-ID och nyckel värde för det WebAPI-program som måste skapas i Azure AD.

11. Gör nödvändiga ändringar på sidan globala parametrar:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Bild 6. Sidan globala parametrar

Sidan globala parametrar innehåller följande inställningar:

- DateTime-format – format som används för alla attribut med EDM. DateTimeOffset-typ. Alla datum konverteras till String med det formatet under importen. Set format används för alla attribut, vilket sparar datum.

 - HTTP-tidsgräns (sekunder) – tids gräns i sekunder som ska användas vid varje HTTP-anrop till WebAPI-program.

 - Framtvinga ändrings lösen ord för skapad användare vid nästa inloggning – det här alternativet används för nya användare som skapas under exporten. Om alternativet är aktiverat anges egenskapen [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) till true, annars kommer den att vara falsk.

## <a name="configuring-the-connector-schema-and-operations"></a>Konfigurera anslutnings schema och-åtgärder


12.   Konfigurera schemat.  Anslutnings programmet stöder följande lista över objekt typer:

-   Användare

    -   Fullständig/delta-import

    -   Exportera (Lägg till, uppdatera, ta bort)

-   Grupp

    -   Fullständig/delta-import

    -   Exportera (Lägg till, uppdatera, ta bort)


Listan över attributtyper som stöds:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (sträng i kopplings området)

-   `microsoft.graph.directoryObject` (referens i kopplings utrymme till något av de objekt som stöds)

-   `microsoft.graph.contact`

Flervärdesattribut (mängd) stöds också för någon av typerna i listan ovan.

Anslutningen använder attributet "`id`" för ankare och DN för alla objekt.  Därför behövs inte Rename, eftersom Graph API inte tillåter att ett objekt ändrar sitt ID-attribut.


## <a name="access-token-lifetime"></a>Livstid för åtkomsttoken


Ett diagram program kräver en åtkomsttoken för att få åtkomst till Graph API. En anslutning begär en ny åtkomsttoken för varje import upprepning (import upprepning är beroende av sid storlek). Till exempel:

-   Azure AD innehåller 10000 objekt

-   Sid storleken som kon figurer ATS i koppling är 5000

I det här fallet kommer det att finnas två iterationer under importen, där var och en av dem returnerar 5000 objekt som ska synkroniseras. Därför kommer en ny åtkomsttoken att begära två gånger.

Under exporten begärs en ny åtkomsttoken för varje objekt som måste läggas till/uppdateras/tas bort.

## <a name="troubleshooting"></a>Felsökning


**Aktivera loggar**

Om det finns några problem i grafen kan loggarna användas för att lokalisera problemet. Spårningen kan därför aktive ras på [samma sätt som för allmänna anslutningar](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Eller bara genom att lägga till följande i `miiserver.exe.config` (i `system.diagnostics/sources` avsnittet):

```
\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

\<add initializeData="ConnectorsLog"
type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,
Culture=neutral, PublicKeyToken=b77a5c561934e089"
name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,
DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>
```
>[!NOTE]
>Om "kör den här hanterings agenten i en separat process" är aktive rad ska `dllhost.exe.config` användas i stället för `miiserver.exe.config`.

**Förfallo fel för åtkomsttoken**

Anslutningen kan returnera HTTP-fel 401 obehörig, meddelandet "åtkomsttoken har upphört att gälla.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Bild 7. "Åtkomsttoken har upphört att gälla." Fel

Orsaken till det här problemet kan vara konfiguration av livs längd för åtkomsttoken från Azure-sidan. Som standard upphör åtkomsttoken att gälla efter 1 timme. Läs [den här artikeln](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)för att öka förfallo tiden.

Exempel på detta med [Azure AD PowerShell module offentlig för hands version](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy-definition \@({"TokenLifetimePolicy": {"version": 1, **"AccessTokenLifetime": "5:00:00"** }})-DisplayName "OrganizationDefaultPolicyScenario"-IsOrganizationDefault \$True-Type "TokenLifetimePolicy"

## <a name="next-steps"></a>Nästa steg

- [Graph Explorer, perfekt för fel sökning av HTTP-anrops problem]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versions hantering, support och avbrytande ändrings principer för Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Hämta Microsoft Identity Manager Connector för Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
[MIM B2B-distribution till slut punkt]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
