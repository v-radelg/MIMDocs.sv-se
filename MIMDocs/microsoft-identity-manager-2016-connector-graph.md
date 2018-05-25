---
title: Microsoft Identity Manager hanteringsagenten för Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Graph (förhandsversion) är externa användare Livscykelhantering för AD-kontot. I det här scenariot en organisation har bjudits in gäster i sina Azure AD-katalog och vill ge dessa gäster tillgång till lokala Windows-integrerad autentisering och Kerberos-baserade program
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2018
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>Microsoft Identity Manager hanteringsagenten för Microsoft Graph (förhandsversion)
=======================================================================================

<a name="summary"></a>Sammanfattning 
=======

Den [Microsoft Identity Manager hanteringsagent för Microsoft Graph (förhandsgranskning)](http://go.microsoft.com/fwlink/?LinkId=717495) aktiverar ytterligare integrationsscenarier för Azure AD Premium-kunder.

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integrerar lokala kataloger med Azure AD och garanterar att användarna har en gemensam identitet och konsekvent autentisering över AD DS, Office 365, Azure och SaaS program som är integrerade med Azure AD genom att synkronisera användare och grupper från lokala kataloger till Azure AD.   Den här hanteringsagenten kan distribueras för särskilda identitet och åtkomst hanteringsåtgärder utöver användare och Gruppsynkronisering till Azure AD.  Den här hanteringsagenten hämtar i MIM sync metaversum ytterligare objekten från den [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 och beta. 

<a name="scenarios-covered"></a>Scenarier som tas upp
=================

<a name="b2b-account-lifecycle-management"></a>Livscykelhantering för B2B-konto
--------------------------------

Första scenariot i förhandsvisning för Microsoft Identity Manager management agent för Microsoft Graph (förhandsversion) är Livscykelhantering för extern användare AD-kontot. I det här scenariot en organisation har bjudits in gäster i sina Azure AD-katalog och vill ge dessa gäster tillgång till lokala Windows-integrerad autentisering och Kerberos-baserade program den [Azure AD-program](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy eller andra mekanismer för gateway. Azure AD application proxy måste varje användare ha sina egna AD DS-konto för identifiering och delegering

Fler scenarier kan läggas till i framtiden och [dokumenteras här](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Fastställa din distributionstopologi
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Förbereder för att använda hantering av Agent(MA) för Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Auktorisera MA att hantera Azure AD-katalogen
----------------------------------------------------

1.  Diagrammet hanteringsagenten kräver webbapp / API-program som kan skapas i AzureAD.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Bild 1. Ny appregistrering

2.  Öppna programmet skapas och använda program-ID som klient-Id på sidan för den MA-anslutningen:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Bild 2. Program-ID

2.  Generera en ny Klienthemlighet genom att öppna alla inställningar-\> nycklar. Ange en beskrivning av nyckeln och markera needful varaktighet. Spara ändringarna. Hemligt värde blir inte tillgängliga när du lämnar sidan.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Bild 3. Nya Klienthemligheten

3.  Lägga till ”Microsoft Graph API” i programmet genom att öppna ”behörigheter som krävs”.

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Bild 4. Lägga till nya API

Följande behörighet bör läggas till ”Microsoft Graph API”:

| Igen med objekt | Behörigheten som krävs                                                                  | Behörighetstyp |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importera grupp          | Group.Read.All eller Group.ReadWrite.All                                                | Programmet     |
| Importera användare           | User.Read.All eller User.ReadWrite.All eller Directory.Read.All eller Directory.ReadWrite.All | Programmet     |

Mer information om behörigheter som krävs kunde hittas [här](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  Skapa koppling med program-ID och genererade klienten Secret.Each hanteringsagenten ska ha sin egen program i AzureAD för att undvika att köra import parallellt för samma program. Diagrammet connector stöder följande lista över objekt av typen:

-   Användare

    -   Fullständig/Deltaimport

    -   Exportera (lägga till, uppdatera, ta bort)

-   Grupp

    -   Fullständig/Deltaimport

    -   Exportera (lägga till, uppdatera, ta bort)


Lista över attributtyper som stöds:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (sträng i anslutningsplatsen)

-   microsoft.graph.directoryObject (referens i anslutningsplatsen till någon av objekt som stöds)

-   Microsoft.Graph.Contact

Flervärdesattribut (insamling) stöds även för ett formulär av typen listan tidigare.

Diagram-anslutningen använder id-attribut för fästpunkten och DN för alla objekt.

Namnbyte stöds inte för tillfället eftersom GraphAPI inte tillåter att ändra objektet till id-attribut för sparats objektet.

<a name="access-token-lifetime"></a>Livslängd för åtkomst-token
=====================

Ett diagram program kräver en åtkomst-token för åtkomst till GraphAPI. En koppling begär en ny åtkomsttoken för varje iteration import (importera iteration beror på storleken). Exempel:

-   AzureAD innehåller 10000 objekt

-   Sidstorlek som konfigurerats i connector är 5 000

I det här fallet finns två iterationer under importen, var och en av dem kommer att returnera 5 000 objekt ska synkroniseras. En ny åtkomsttoken kommer därför att begäran två gånger.

Observera att en ny åtkomsttoken under exporten ska begäras för varje objekt som ska läggas till eller uppdateras/tas bort.

<a name="installing-the-connector"></a>Installerar connector
========================

Innan du använder anslutningen, kontrollera att du har följande på synkroniseringsservern: 4.5.2 för Microsoft .NET Framework eller senare Microsoft Identity Manager 2016 SP1 måste du använda snabbkorrigering 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) eller senare.

Kopplingar för Microsoft Identity Manager 2016 SP1 Graph-kopplingen är tillgänglig för hämtning från den [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Kopplingskonfiguration
=======================

Sida för anslutningen:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Bild 5. Anslutningen sida

Sidan anslutningar (bild 1) innehåller Graph API-version som används och klient namn. Klient-Id och Klienthemlighet representerar program-ID och nyckelvärde för WebAPI-program som måste skapas i AzureAD.

Globala parametrar sida:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Bild 6. Sidan globala parametrar

Globala parametrar sidan innehåller följande inställningar:

DateTime-format-format som används för ett attribut med Edm.DateTimeOffset typen. Alla datum konverteras till en sträng med formatet under importen. Ange formatet används för alla attribut som sparar datum.

HTTP-tidsgräns (sekunder) – tidsgräns i sekunder som ska användas under varje HTTP-anrop till WebAPI-programmet.

Tvingad ändra lösenordet för skapade användaren vid nästa inloggning – det här alternativet används för nya användare som kommer att skapas under exporten. Om alternativet är aktiverat sedan [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) egenskap anges true, annars kommer vara falskt.

<a name="troubleshooting"></a>Felsökning
===============

**Aktivera loggar**

Om det uppstår några problem i diagram, kan sedan loggar användas för att lokalisera problemet. Diagram-kopplingen använder samma källa som alla generiska kopplingar. Därför spårningar kan aktiveras i [på samma sätt som för allmänna kopplingar finns i wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Eller bara genom att lägga till följande miiserver.exe.config (i avsnittet system.diagnostics/sources):

\<datakällans namn = ”ConnectorsLog” switchValue = ”utförlig”\>

\<lyssnare\>

>   \<Lägg till initializeData = ”ConnectorsLog” type="System.Diagnostics.EventLogTraceListener, System, Version = 4.0.0.0-prestandaräknare kultur = neutral, PublicKeyToken = b77a5c561934e089” name = ”ConnectorsLogListener” traceOutputOptions = ”LogicalOperationStack,   DateTime, Timestamp, anropsstacken ”/\>

\<ta bort namn = ”standard” /\>

\</Listeners\>

\</ Source\>

Observera: om 'Kör den här hanteringsagenten i en separat process-aktiverat sedan dllhost.exe.config ska användas i stället för miiserver.exe.config.

**Åtkomst-token har upphört att gälla fel**

Anslutningen kan returnera HTTP 401 obehöriga, felmeddelandet ”åtkomst-token har upphört att gälla”.:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Bild 7. ”Åtkomst-token har upphört att gälla”. Fel

Orsaken till problemet kanske konfigurationen av livslängd för token för åtkomst från Azure sida. Som standard upphör åtkomst-token efter en timme. Om du vill öka förfallotid finns [i den här artikeln](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Exempel på detta med hjälp av [Azure AD PowerShell-modulen offentliga förhandsversionen](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Nya AzureADPolicy-Definition \@('{”TokenLifetimePolicy”: {”Version”: 1, **”AccessTokenLifetime” ”: 5: 00:00”**}}') - DisplayName ”OrganizationDefaultPolicyScenario” - IsOrganizationDefault \$true - typ ”TokenLifetimePolicy”

<a name="next-steps"></a>Nästa steg
----------
- [Diagrammet Explorer (bra för felsökning av problem med HTTP-anrop)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versionshantering, support och dela ändra principer för Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Ladda ned Microsoft Identity Manager management agent för Microsoft Graph (förhandsgranskning)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Scenariot specifika stöds stödlinjer
----------------------------------
[Distribution av MIM B2B slutpunkt till slutpunkt]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
