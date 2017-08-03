---
title: "CM REST API-tjänsten information | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>Information om CM REST API-tjänsten
I följande avsnitt beskrivs information om Microsoft Identity Manager (MIM) certifikat Management CM REST API.

## <a name="architecture"></a>Arkitektur 
MIM CM REST API-anrop som hanteras av domänkontrollanter. I följande tabell visas en fullständig lista över kontrollanter och exempel på kontexten där de kan användas.

Domänkontrollant| Exempel väg
----------|-------------
CertificateDataController| /API/v1.0/Requests/{RequestId}/certificatedata /
CertificateRequestGenerationOptionsController| /API/v1.0/Requests/{RequestId}/certificaterequestgenerationoptions
CertificatesController| /API/v1.0/SmartCards/{smartcardid}/Certificates <br/> /API/v1.0/Profiles/{profileID}/Certificates
OperationsController| /API/v1.0/SmartCards/{smartcardid}/Operations <br/> /API/v1.0/Profiles/{profileID}/Operations
PoliciesController| / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| / api/v1.0/profiles/{id} <br/> /API/v1.0/Profiles <br/> / api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| / api/v1.0/profiletemplates/{id} <br/> /API/v1.0/profiletemplates <br/> / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| / api/v1.0/requests/{id} <br/> /API/v1.0/Requests
SmartcardsController| /API/v1.0/Requests/{RequestId}/SmartCards/{ID}/diversifiedkey <br/> /API/v1.0/Requests/{RequestId}/SmartCards/{ID}/serverproposedpin <br/> /API/v1.0/Requests/{RequestId}/SmartCards/{ID}/authenticationresponse <br/> / api/v1.0/requests/{requestid}/smartcards/{id} <br/> / api/v1.0/smartcards/{id} <br/> /API/v1.0/SmartCards
SmartcardsConfigurationController| /API/v1.0/profiletemplates/{profiletemplateid}/Configuration/SmartCards
<br/>

## <a name="http-request-and-response-headers"></a>HTTP-begäran och svarshuvuden

HTTP-begäranden skickas till API: N bör innehålla följande huvuden (den här listan inte är fullständig):

Sidhuvud | Beskrivning
-------|------------
Auktorisering | Obligatoriskt. Innehållet beror på den autentiseringsmetod som kan konfigureras och kan baseras på WIA (Windows-integrerad autentisering) eller AD FS.
Innehållstyp | Krävs om begäran har en brödtext. Måste vara ”application/json”.
Content-Length | Krävs om begäran har en brödtext. 
Cookie | Sessions-cookie. Kan krävas beroende på vilken autentiseringsmetod du valde.
<br/>
HTTP-svar innehåller följande huvuden (den här listan inte är fullständig):

Sidhuvud | Beskrivning
-------|------------
Innehållstyp | API: et och returnerar ”application/json”.
Content-Length | Längden på begärantext om den finns, i byte.


## <a name="api-versioning"></a>API-version 
Den aktuella versionen av CM REST API är 1,0. Versionen som har angetts i segmentet direkt efter den `/api` segment i URI: `/api/v1.0`. I framtiden, versionsnumret ändras bör det finnas betydande ändringar i API-gränssnitt.


## <a name="enabling-the-api"></a>Aktivera API: et 
Den `<ClmConfiguration>` avsnitt i Web.config-filen har utökats med en ny nyckel:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Den här nyckeln avgör om CM REST API exponeras för klienter. Om nyckeln är inställd på ”false”, utförs inte vägen mappningen för API: et på Start av program. Det innebär att alla efterföljande begäranden till API-slutpunkter returnerar ett HTTP-fel 404. Nyckeln är standardinställningen på ”inaktiverad”.
När du har ändrat värdet ”true” Kom ihåg att köra **iisreset** på servern.

## <a name="enabling-tracing-and-logging"></a>Aktivera spårning och loggning 
MIM CM REST API avger spårningsdata för varje HTTP-begäran skickas till den. Du kan ange nivån av spårningsinformation orsakat genom att ange Konfigurationsvärdet för följande:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Felhantering och felsökning 
När undantag inträffar när en begäran bearbetades returnerar en HTTP-statuskod och webbklienten MIM CM REST API. Vanliga fel returnerar API: et lämplig HTTP-statuskoden och en felkod. 

Ohanterade undantag som konverteras till en `HttpResponseException` med HTTP-Status 500 (”internt fel”) och spåras i både i händelseloggen och spårningsfilen MIM CM. Varje undantag skrivs till händelseloggen med en motsvarande Korrelations-ID Korrelations-ID skickas även till konsumenten API i felmeddelandet. Åtgärda felet, kan en administratör söka motsvarande Korrelations-ID och fel information i händelseloggen.

På grund av säkerhetsfrågor skickas stackspår som motsvarar de fel som har genererats av förbrukar MIM CM REST API inte tillbaka till klienten.
