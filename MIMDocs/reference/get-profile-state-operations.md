---
title: "Get-profil tillstånd Operations | Microsoft Docs"
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e8428984404b2aebda8f53e4f7841b699a5d23a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-state-operations"></a>Get-profil tillstånd åtgärder
Hämtar en lista över möjliga åtgärder som kan utföras av den aktuella användaren på den angivna profilen. Sedan kan du initiera en begäran för någon av de angivna åtgärderna.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles/{ID}/Operations <br/>/CertificateManagement/API/v1.0/SmartCards/{ID}/Operations

###<a name="url-parameters"></a>URL-parametrar
Parameter | Beskrivning
---------|------------
ID | Identifierare (GUID) för profilen eller smartkort.

###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="request-body"></a>Begärandetexten
inget

##<a name="response"></a>Svar
###<a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
200     | OK
204 | Inget innehåll
403 | Tillåts inte
500 | Internt fel

###<a name="response-headers"></a>Svarsrubriker
Vanliga svarshuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="response-body"></a>Svarstexten
Returnerar en lista över möjliga åtgärder som kan utföras av användare på smartkortet på åtgärden lyckades. Den här listan kan innehålla valfritt antal följande: *OnlineUpdate*, *förnya*, *återställa*, *RecoverOnBehalf*, *pensionera*, *återkalla*, och *avblockera*.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
