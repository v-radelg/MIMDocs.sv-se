---
title: Microsoft Identity Manager-anslutningen för Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Identity Manager connector för Microsoft Graph kan externa användare Livscykelhantering för AD-konto. I det här scenariot kan en organisation har bjudit in gäster i sin Azure AD-katalog och vill ge dessa gäster åtkomst till lokal Windows-Integrated autentisering eller Kerberos-baserade program
keywords: ''
ms.author: billmath
manager: bhu
ms.date: 10/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 09052bc9f5cecd0a599e9a93ee43cc44ce435671
ms.sourcegitcommit: 032b3cdd8a88b1ccfb30c0070f216020feee6293
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045657"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Microsoft Identity Manager-anslutningen för Microsoft Graph
=======================================================================================

<a name="summary"></a>Sammanfattning 
=======

Den [Microsoft Identity Manager-anslutningen för Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) aktiverar ytterligare integrationsscenarier för Azure AD Premium-kunder.  Den hämtar i MIM sync metaversum ytterligare objekten hämtas från den [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1- och betaversioner.

<a name="scenarios-covered"></a>Du lär dig
=================

<a name="b2b-account-lifecycle-management"></a>Livscykelhantering för B2B-konto
--------------------------------

Det första scenariot för Microsoft Identity Manager-anslutningen för Microsoft Graph är som en anslutning för att automatisera Livscykelhantering för AD DS-konto för externa användare. I det här scenariot kan en organisation synkroniserar anställda till Azure AD från AD DS med Azure AD Connect och även har bjudit in gäster i sin Azure AD-katalog. Bjuda in en gäst resulterar i ett användarobjekt för externa i den organisationens Azure AD-katalog, som inte är i den organisationen AD DS. Sedan organisationen vill ge dessa gäster tillgång till den lokala Windows-integrerad autentisering och Kerberos-baserade program den [Azure AD-programproxyn](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) och andra mekanismer som gateway. Azure AD-programproxy måste varje användare ha sina egna AD DS-kontot för identifiering och delegering.  

Om du vill veta hur du konfigurerar MIM-synkronisering för att automatiskt skapa och hantera AD DS-konton för gäster, när du har läst anvisningarna i den här artikeln, Fortsätt att läsa i artikeln [Azure AD-business-to-business (B2B) samarbete med MIM 2016 SP1 med Azure Application Proxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Den här artikeln visar sync-regler som behövs för anslutningen.

<a name="other-identity-management-scenarios"></a>Andra scenarier för hantering av identitet
---------------

Anslutningen kan användas för andra specifika Identitetshantering scenarier som rör skapa, läsa, uppdatera och ta bort användare, grupp och kontakta objekt i Azure AD, utöver synkronisering av användare och grupper till Azure AD. När du utvärderar möjliga scenarier, ha i åtanke: den här anslutningen kan inte användas i ett scenario, vilket skulle resultera i ett dataflöde överlappar faktiska eller potentiella synkronisering står i konflikt med en Azure AD Connect-distribution.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) är den rekommenderade metoden att integrera lokala kataloger med Azure AD genom att synkronisera användare och grupper från lokala kataloger till Azure AD.  Azure AD Connect har många fler synkroniseringsfunktioner och möjliggör scenarier, till exempel tillbakaskrivning av lösenord och enheten, som inte är möjligt för objekt som skapas av MIM. Om data hämtas till AD DS, till exempel se till att den inte med i Azure AD Connect försöker matcha objekten tillbaka till Azure AD-katalog.  Inte heller kan den här anslutningen användas för att göra ändringar i Azure AD-objekt som har skapats av Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Förbereder för att använda anslutningstjänsten för Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Auktorisera anslutningen att hämta eller hantera objekt i din Azure AD-katalog
----------------------------------------------------

1.  Anslutningen kräver en webbapp / API-program som ska skapas i Azure AD så att den kan auktoriseras med rätt behörighet för att arbeta med Azure AD-objekt via Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Bild 1. Ny programregistrering

2.  Öppna skapade programmet i Azure-portalen och spara program-ID som en klient-ID för att använda senare på sidan för den MA-anslutning:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Bild 2. Program-ID

3.  Generera nya Klienthemligheten genom att öppna alla inställningar-\> nycklar. Ange vissa nyckel beskrivning och välj needful varaktighet. Spara ändringarna. Hemligt värde kan inte tillgängliga efter att ha lämnat sidan.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Bild 3. Nya Klienthemlighet

4.  Lägga till ”Microsoft Graph API” i programmet genom att öppna ”behörigheter som krävs”.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Bild 4. Lägg till nytt API

Följande behörighet ska läggas till programmet så att det för att använda ”Microsoft Graph API”, beroende på scenario:

| Åtgärden med objekt | Behörighet krävs                                                                  | Behörighetstyp |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Import-grupp          | `Group.Read.All` eller `Group.ReadWrite.All`                                                | Programmet     |
| Importera användare           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` eller `Directory.ReadWrite.All` | Programmet     |

Mer information om behörigheter som krävs kunde hittas [här](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Bevilja behörigheterna som krävs för programmet.


<a name="installing-the-connector"></a>Installera anslutningstjänsten
========================

6.  Innan du installerar anslutningen måste du kontrollera att du har följande på synkroniseringsserver: 

 - 4.5.2 för Microsoft .NET Framework eller senare
 - Microsoft Identity Manager 2016 SP1 och måste använda snabbkorrigering 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) eller senare.

7. Anslutningsappen för Microsoft Graph, utöver andra anslutningsappar för Microsoft Identity Manager 2016 SP1 är tillgänglig för hämtning från den [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Starta om MIM-synkroniseringstjänsten.
 
<a name="connector-configuration"></a>Kopplingskonfiguration
=======================


9.  Välj i Synchronization Service Manager-UI **Anslutningsappar** och **skapa**.
Välj **Graph (Microsoft)** , skapar du en koppling och ge den ett beskrivande namn.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. Ange program-ID i MIM-synkroniseringstjänsten Användargränssnittet, och genereras Klienthemlighet. Varje hanteringsagent som konfigurerats i MIM-synkronisering bör ha sin egen program i Azure AD för att undvika att köra import parallellt för samma program.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Bild 5. Sidan anslutning

Sidan anslutningar (bild 5) innehåller Graph API-versionen som används och klientorganisationens namn. Klient-ID och Klienthemlighet representerar program-ID och nyckel-värde för WebAPI-program som måste skapas i Azure AD.

11. Gör nödvändiga ändringar på sidan globala parametrar:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Bild 6. Globala parametrar sidan

Globala parametrar innehåller följande inställningar:

- DateTime-format – format som används för attribut med Edm.DateTimeOffset typen. Alla datum konverteras till sträng med formatet under importen. Set-format används för alla attribut, vilket sparar datum.

 - HTTP-tidsgräns (sekunder) – tidsgräns i sekunder som ska användas under varje HTTP-anrop till WebAPI-program.

 - Framtvinga ändra lösenordet för användaren som skapades vid nästa inloggning – det här alternativet används för nya användare som kommer att skapas under exporten. Om alternativet är aktiverat sedan [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) egenskapen anges till SANT, annars blir det FALSKT.

<a name="configuring-the-connector-schema-and-operations"></a>Konfigurera anslutningsschema och åtgärder
=========================

12.   Konfigurera schemat.  Anslutningen stöder följande lista över objekt av typen:

-   Användare

    -   Fullständig/Deltaimport

    -   Exportera (lägga till, uppdatera, ta bort)

-   Grupp

    -   Fullständig/Deltaimport

    -   Exportera (lägga till, uppdatera, ta bort)


Lista över typer som stöds:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (sträng i anslutarplatsen)

-   `microsoft.graph.directoryObject` (refererar till i anslutarplatsen till någon av objekt som stöds)

-   `microsoft.graph.contact`

Flervärdesattribut (samling) har också stöd för någon av en typ i listan ovan.

Anslutningen används den ”`id`”-attributet för fästpunkten och unikt namn för alla objekt.  Därför är Byt namn på inte nödvändigt, eftersom Graph API inte tillåter ett objekt för att ändra dess id-attribut.


<a name="access-token-lifetime"></a>Livslängd för åtkomst-token
=====================

Ett diagram program kräver en åtkomsttoken för att komma åt Graph API. En anslutning kommer att begära en ny åtkomsttoken för varje import iteration (importera iteration beror på sidstorlek). Exempel:

-   Azure AD innehåller 10000 objekt

-   Sidstorlek som konfigurerats i connector är 5 000

I det här fallet utgår två iterationer under importen, var och en av dem returnerar 5000 objekt ska synkroniseras. En ny åtkomsttoken kommer därför att begäran två gånger.

En ny åtkomsttoken kommer att begäras för varje objekt som måste vara har lagts till/uppdaterade/borttagna under exporten.

<a name="troubleshooting"></a>Felsökning
===============

**Aktivera loggar**

Om det finns problem i diagrammet, skulle loggar kunna användas för att hitta problemet. Därför spårningar kan aktiveras på [samma sätt som för allmän kopplingar](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Eller bara genom att lägga till följande för att `miiserver.exe.config` (inuti `system.diagnostics/sources` avsnitt):


\<Källnamn = ”ConnectorsLog” switchValue = ”utförlig”\>

\<lyssnare\>

>   \<Lägg till initializeData = ”ConnectorsLog” type="System.Diagnostics.EventLogTraceListener, System, Version = 4.0.0.0 Culture = neutral, PublicKeyToken = b77a5c561934e089” name = ”ConnectorsLogListener” traceOutputOptions = ”LogicalOperationStack,   DateTime, tidsstämpel, anropsstacken ”/\>

\<ta bort namn = ”standard” /\>

\</Listeners\>

\</ Source\>

>[!NOTE]
>Om ”kör den här hanteringsagenten i en separat process” är aktiverad, sedan `dllhost.exe.config` ska användas i stället för `miiserver.exe.config`.

**Åtkomst-token har upphört att gälla-fel**

Anslutningen kan returnera HTTP 401 obehörig, felmeddelandet ”åtkomst-token har gått ut”.:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Bild 7. ”Åtkomst-token har gått ut”. Fel

Orsaken till problemet kanske konfigurationen av livslängd för åtkomst-token från Azure-sidan. Som standard upphör åtkomsttoken efter 1 timme. Om du vill öka förfallotid, se [i den här artikeln](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Exempel på detta med hjälp av [Azure AD PowerShell-modulen offentliga förhandsversionen](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Ny AzureADPolicy-Definition \@('{”TokenLifetimePolicy”: {”Version”: 1, **”AccessTokenLifetime” ”: 5: 00:00”**}}') - DisplayName ”OrganizationDefaultPolicyScenario” - IsOrganizationDefault \$true - typen ”TokenLifetimePolicy”

<a name="next-steps"></a>Nästa steg
----------
- [Graph-testaren bra för att felsöka HTTP anropa problem]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versionshantering, support och icke-bakåtkompatibel ändringsprinciper för Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Ladda ned Microsoft Identity Manager-anslutningen för Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>Scenariospecifika guider
----------------------------------
[MIM B2B från slutpunkt till slutpunkt-distributionen]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
