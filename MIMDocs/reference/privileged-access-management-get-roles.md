---
title: "Hämta PAM-roller | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>Hämta PAM-roller
Används av ett privilegierat konto för att visa en lista med PAM-roller som kontot är en kandidat.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `http://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/API/pamresources/pamroles

###<a name="query-parameters"></a>Frågeparametrar
Parameter | Beskrivning
----------|--------------
$filter | Valfritt. Ange någon av egenskaperna för PAM Role i ett filteruttryck för att returnera en filtrerad lista över svar. Mer information om stöds operatorer finns [filtrering i information om PAM REST API-tjänsten](privileged-access-management-rest-api-service-details.md#filtering)
v | Valfritt. API-versionen. Om inte ingår, används den aktuella (mest nyligen utgivna) versionen av API: et. Mer information finns i [versionshantering i information om PAM REST API-tjänsten](privileged-access-management-rest-api-service-details.md#versioning)
###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) i *information om PAM REST API-tjänsten*.
###<a name="request-body"></a>Begärandetexten
inget

##<a name="response"></a>Svar
###<a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
200 | OK
401 | Obehörig
403 | Tillåts inte
408 | Tidsgräns för förfrågan   
500 | Internt serverfel
503 | Tjänsten är inte tillgänglig

###<a name="response-headers"></a>Svarsrubriker
Vanliga svarshuvuden finns [HTTP-begäran och svarshuvuden](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) i *information om PAM REST API-tjänsten*.
###<a name="response-body"></a>Svarstexten
Ett lyckat svar innehåller en samling av en eller flera PAM-roller, som har följande egenskaper.

Egenskap | Beskrivning
--------|-------------
RoleID | Den unika identifieraren (GUID) för PAM-roll.
DisplayName | Visningsnamn för PAM-roll i MIM-tjänsten.
Beskrivning | Beskrivning av PAM-roll i MIM-tjänsten.
TTL | Rollens åtkomstbehörigheter maximala giltighetstid tidsgräns i sekunder.
AvailableFrom | Den tidigaste tidpunkten på dagen som en requst aktiveras.
AvailableTo | Senaste tid på dagen som en begäran aktiveras.
MFAEnabled | Ett booleskt värde som anger om aktiveringsbegäranden för den här rollen kräver MFA-kontrollen.
ApprovalEnabled | Ett booleskt värde som anger om aktiveringsbegäranden för den här rollen kräver godkännande av en rollägare.
AvailibitlyWindowEnabled | Ett booleskt värde som anger om rollen som bara kan aktiveras under ett angivet tidsintervall.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
