---
title: "Hämta nyckel för smartkort diversifierat Admin | Microsoft Docs"
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
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 57a8481b2a4f976717b07061e96ccb041164a5a6
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-diversified-admin-key"></a>Hämta smartkort diversifierat Admin-nyckel
Hämtar diversifierat admin-nyckel för det angivna smartkortet.

**Obs**: admin-nyckel som bara behöver vara diversifierade om mallen profilprincip anger att det ska vara.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/CertificateManagement/API/v1.0/Requests/{reqid}/SmartCards/{scid}/diversifiedkey

###<a name="url-parameters"></a>URL-parametrar
Parameter | Beskrivning
---------|------------
reqid | Obligatoriskt. Begärande-ID (MIM CM-specifik).
scid | Obligatoriskt. Smartkort identifierare (MIM CM-specifik). Från den [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objekt.
###<a name="query-parameters"></a>Frågeparametrar
Parameter | Beskrivning
---------|------------
ATR | Valfritt. Strängen som smartkort svar till återställning (ATR).
av CardId | Obligatoriskt. Kort-id.

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
Returnerar en byte BLOB som representerar diversifierat admin-nyckel på åtgärden lyckades.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##<a name="see-also"></a>Se även

- [Metoden Microsoft.Clm.Provision.RequestOperations.CreateSmartcard-metoden (String, String, begäran)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
