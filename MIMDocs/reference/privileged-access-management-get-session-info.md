---
title: "Att hämta PAM-Session information | Microsoft Docs"
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
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 150bc7e863fc679e785526935155611fb75cd69b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-session-info"></a>Få information om PAM-Session
Används av ett privilegierat konto för att hämta användarnamnet för det konto som är inloggade i sessionen.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `http://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/API/session/sessioninfo

###<a name="query-parameters"></a>Frågeparametrar
Parameter | Beskrivning
----------|--------------
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
Ett lyckat svar returnerar ett PAM-objekt med följande egenskaper.

Egenskap| Beskrivning
--------|-------------
Användarnamn| Användarnamnet för det konto som är inloggad i den här sessionen.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
