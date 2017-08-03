---
title: "PAM REST API-tjänsten information | Microsoft Docs"
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>Information om PAM REST API-tjänsten
I följande avsnitt beskrivs information om Microsoft Identity Manager (MIM) Privileged Access Management (PAM) REST API.

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

## <a name="versioning"></a>Versionshantering 
Den aktuella versionen av API: et är 1. API-versionen kan anges via en frågeparameter i begärande-URL, som i följande exempel: `http://localhost:8086/api/pamresources/pamrequests?v=1` om versionen inte har angetts i begäran, begäran körs mot den nyligen lanserade versionen av API: et. 

## <a name="security"></a>Säkerhet 
Åtkomst till API kräver Windows IWA (Integrated Authentication). Detta ska konfigureras manuellt i IIS innan du installerar Microsoft Identity Manager (MIM).

HTTPS (TLS) stöds, men måste konfigureras manuellt i IIS. Mer information finns i: **implementera Secure Sockets Layer (SSL) för FIM-portalen** i [steg 9: utföra FIM 2010 R2 åtgärder efter Installation] (https://technet.microsoft.com/library/hh322875 (v=ws.10%29.aspx) i installera FIM 2010 R2 Test Lab-guiden. 

Du kan generera ett nytt SSL-servercertifikat genom att köra följande kommando i Visual Studio-kommandotolk:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Detta kommando skapar ett självsignerat certifikat som kan användas för att testa ett webbprogram som använder SSL på en webbserver vars URL är test.cwap.com. OID - eku-alternativ identifierar certifikaten som SSL-servercertifikat. Certifikatet lagras i den mitt Arkiv och är tillgänglig på datornivå, så kan du exportera den från snapin-modulen certifikat i mmc.exe

## <a name="cross-domain-access-cors"></a>Mellan åtkomst till domänen (CORS) 
CORS stöds, men måste konfigureras manuellt i IIS. Lägg till följande element i filen web.config distribuerade API för att konfigurera API för att tillåta anrop över domäner: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>Felhantering 
API: N returnerar HTTP-felsvar för att ange felvillkor. Fel är OData-kompatibel. I följande tabell visas felkoder som kan returneras till en klient.

HTTP-statuskod | Beskrivning
-----------------|------------
401 | Obehörig 
403 | Tillåts inte 
408 | Tidsgräns för förfrågan   
500 | Internt serverfel 
503 | Tjänsten är inte tillgänglig 
<br/>

## <a name="filtering"></a>Filtrering 
PAM REST API-begäranden kan innehålla filter för att ange egenskaper som ska inkluderas i svaret. Filtersyntaxen baseras på OData-uttryck.

Filter kan ange någon av egenskaperna för PAM-förfrågningar, PAM-roller. eller väntande PAM-förfrågningar. Till exempel: *ExpirationTime*, *DisplayName*, eller någon annan giltig egenskap för en PAM-Request PAM Role eller väntande begäran.

API: et stöder följande operatorer i filteruttryck: *och*, *lika*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*, och *LessThanOrEqual*. 

Följande exempel-begäranden är filter:

- Denna begäran returnerar alla PAM-förfrågningar mellan specifika datum:`http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- Denna begäran returnerar Pam Role med namnet ”SQL filåtkomst”:`http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
