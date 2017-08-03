---
title: "Hämta Data för smartkort | Microsoft Docs"
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
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>Hämta Data för smartkort
Hämtar en lista med profiler smartkort med en lista över möjliga åtgärder som kan utföras av den aktuella användaren. Sedan kan du initiera en begäran för någon av de angivna åtgärderna.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/CertificateManagement/API/v1.0/SmartCards <br/> / CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###<a name="url-parameters"></a>URL-parametrar
Egenskap| Beskrivning
---------|--------
smartcarduuid | Valfritt. Smartkort UUID med MIM CM. Detta är det ”uuid” i den [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objekt.

###<a name="query-parameters"></a>Frågeparametrar
Egenskap| Beskrivning
---------|--------
av CardId | Valfritt. Smartkort UUID med MIM CM. Detta är det ”uuid” i den [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objekt.


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
Returnerar lyckades, en JSON-serialiserad [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objekt med följande egenskaper:

Namn | Beskrivning
-----|-----------
AssignedUserUuid | Identifierare för den användare som tilldelats smartkortet.
ATR | Smartkort svar till återställning (ATR) strängen för kortet som för närvarande på att initieras.
Kommentar | Kommentar som beskriver smartkortet.
Flaggor | Flaggor som beskriver smartkortet.
Mellanprogram | Mellanprogram för smartkortet.
ParentSmartcardUuid | Identifierare för det gamla smartkort som smartkortet har ersatt.
PermanentSmartcardUuid | Identifierare för permanenta smartkortet som associeras med smartkortet.
PrimarySmartcardUuid | Identifierare för primära smartkortet.
ProfileTemplateUuid | Identifierare av Profilmall som innehåller de principer och inställningar som styr smartkortet.
ProfileTemplateVersion | Versionen av Profilmall vid den tidpunkt då smartkort profilen har skapats.
Serienummer | Serienumret för det smartkortet.
Status | Status för smartkortet.
UUID | Identifierare för smartkort-profilen.

##<a name="example"></a>Exempel

###<a name="request-1"></a>Begäran 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>Svar 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###<a name="request-2"></a>Förfrågan 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>Svar 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
##<a name="see-also"></a>Se även

-[Microsoft.Clm.Shared.Smartcards.Smartcard-klass](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
