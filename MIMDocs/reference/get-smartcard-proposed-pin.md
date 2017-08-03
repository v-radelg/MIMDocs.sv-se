---
title: "Hämta smartkort föreslagna PIN-kod | Microsoft Docs"
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Hämta smartkort föreslagna PIN-kod
Hämtar servergenererade användaren PIN-kod.

**Obs**: servern ställer in PIN-koden endast om mallen profilprincip anger att det ska göras. I annat fall måste användaren ange den.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/CertificateManagement/API/v1.0/SmartCards/{ID}/serverproposedpin

###<a name="url-parameters"></a>URL-parametrar
Parameter | Beskrivning
---------|------------
ID | Smartkort identifierare (MIM CM-specifik). Hämtas från Microsft.Clm.Shared.Smartcard-objektet.
###<a name="query-parameters"></a>Frågeparametrar
Parameter | Beskrivning
---------|------------
ATR | Kort atr.
av CardId | Kort-id.
utmaning | En Base64-kodad sträng som representerar anrop utfärdat av smartkortet.

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
Returnerar en sträng som representerar PIN-koden föreslagna av servern på åtgärden lyckades.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

... body coming soon
```       
