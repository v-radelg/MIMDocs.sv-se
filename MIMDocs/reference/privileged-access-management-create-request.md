---
title: "Skapa PAM-förfrågan | Microsoft Docs"
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>Skapa PAM-förfrågan
Används av ett konto med privilegier för att upphöja till en PAM-roll.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `http://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
POST     |/API/pamresources/pamrequests

###<a name="query-parameters"></a>Frågeparametrar
Parameter | Beskrivning
--------|-------------
Motiveringen | Valfritt. Användaren har angett orsaken för höjning begäran.
RoleId| Obligatoriskt. Den unika identifieraren (GUID) för PAM-roll att upphöja till.
RequestedTTL| Obligatoriskt. Den begärda förfallotiden, i sekunder.
RequestedTime | Optoinal. Tid att upphöja behörighet.  
v | Valfritt. API-versionen. Om inte ingår, används den aktuella (mest nyligen utgivna) versionen av API: et. Mer information finns i [versionshantering i information om PAM REST API-tjänsten](privileged-access-management-rest-api-service-details.md#versioning)

**Obs**: du kan ange den *motivering*, *RoleId*, *RequestedTTL*, och *RequestedTime* parametrar som egenskaper i frågans brödtext i stället för som Frågeparametrar. Den *v* parameteer kan bara anges som en frågeparameter.

###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) i *information om PAM REST API-tjänsten*.
###<a name="request-body"></a>Begärandetexten
Valfritt. Som nämnts ovan är de *motivering*, *RoleId*, *RequestedTTL*, och *RequestedTime* parametrar kan anges som egenskaperna för en begärantext i stället för att ange dem i URL: en frågesträng.

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
Ett lyckat svar innehåller en PAM-begäran med följande egenskaper.

Egenskap | Beskrivning
--------|-------------
Begärande-ID | Den unika identifieraren (GUID) för PAM-förfrågan.
CreatorID | Den unika identifieraren (GUID) i MIM-tjänsten för det konto som skapats av begäran.
Motiveringen | Orsak för höjning.
SkapadTid | Skapandetid för begäran.
CreationMethod | Den metod som används för att skapa begäran.
ExpirationTime | Förfallotid för begäran.
RoleID| Den unika identifieraren (GUID) för PAM-roll.
RequestedTTL | Den begärda giltighetstid tidsgränsen i sekunder.
RequestedTime | Den begärda tiden för höjning.
RequestStatus | Status för begäran. Möjliga värden är: ”bearbetning”, ”aktiv”, ”stängd”, ”Closing”, ”upphört att gälla”, ”väntar på godkännande”, ”PendingMFA” och ”avvisade”.

##<a name="example"></a>Exempel

###<a name="request-1"></a>Begäran 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Svar 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Förfrågan 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Svar 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
