---
title: "Uppdatera Status för smartkort | Microsoft Docs"
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
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>Uppdatera Status för smartkort
Uppdaterar statusen för ett smartkort.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
## <a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>URL-parametrar
Parameter | Beskrivning
---------|------------
reqid | Obligatoriskt. Begärande-ID (MIM CM-specifik).
scid | Obligatoriskt. Smartkort identifierare (MIM CM-specifik). Det här är ”uuid”-egenskap i den [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objekt.

### <a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
### <a name="request-body"></a>Begärandetexten
Begärandetexten innehåller följande egenskaper.

Egenskap | Beskrivning
---------|-----------
status | Status till begäran. Till exempel inaktiverad ””.


## <a name="response"></a>Svar
### <a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
200     | OK
204 | Inget innehåll
403 | Tillåts inte
500 | Internt fel

### <a name="response-headers"></a>Svarsrubriker
Vanliga svarshuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
### <a name="response-body"></a>Svarstexten
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

## <a name="example"></a>Exempel

### <a name="request"></a>Begäran
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>Svar
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
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
## <a name="see-also"></a>Se även

- [Microsoft.Clm.Shared.Smartcards.Smartcard klassen](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
