---
title: "Tilldela en begäran om smartkort | Microsoft Docs"
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Tilldela en begäran om smartkort
Binder angivna smartkortet till den angivna begäranden. En gång bunden kan begäran endast köras med det här kortet.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
POST     |/CertificateManagement/API/v1.0/SmartCards

###<a name="url-parameters"></a>URL-parametrar
Ingen.
###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="request-body"></a>Begärandetexten
Begärandetexten innehåller följande egenskaper.

Egenskap | Beskrivning
---------|-----------
begärande-ID | ID för begäran som smartkortet ska bindas till.
av CardId | Av cardid för smartkortet.
ATR | Strängen som smartkort svar till återställning (ATR).


##<a name="response"></a>Svar
###<a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
201     | Skapad
204 | Inget innehåll
403 | Tillåts inte
500 | Internt fel

###<a name="response-headers"></a>Svarsrubriker
Vanliga svarshuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="response-body"></a>Svarstexten
På lyckades, returnerar du en URI till det nyligen skapade smartkort-objektet.
##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Svar
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Se även

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard-metoden (String, String, begäran)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
