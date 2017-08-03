---
title: "Hämta Profilmallar | Microsoft Docs"
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Hämta Profilmallar
Hämtar en lista med profilmallar som kan registrera den angivna användaren. Den här metoden returnerar en begränsad vy av av Profilmall. Mallen profildata returnerade ska vara tillräcklig för att aktivera användaren att bestämma vilka Profilmall eventuella de behöver för att registrera sig för. Om inga arbetsflödet och behörighet har angetts, returneras alla profilmallar visas för användaren.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates? \[målanvändare\] 

###<a name="url-parameters"></a>URL-parametrar
Parameter| Beskrivning
--------|-------------
målanvändare| Valfritt. Anger de mål för att returnera profilmallar för. Om inget anges används identiteten för den aktuella användaren. <br/>**Obs**: för närvarande stöds endast den aktuella användaren.

###<a name="request-headers"></a>Huvuden för begäran
Finns i avsnittet service för vanliga begärandehuvuden
###<a name="request-body"></a>Begärandetexten
inget

##<a name="response"></a>Svar
###<a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
200     | OK
204 | Inget innehåll
500 | Internt fel

###<a name="response-headers"></a>Svarsrubriker
Avsnittet service för vanliga svarshuvuden.
###<a name="response-body"></a>Svarstexten
Returnerar en lista över ProfileTemplateLimitedView objekt med följande egenskaper på lyckades.

Egenskap| Typ| Beskrivning
--------|-----|--------
Namn| sträng| Visa namnet på profilmallen.
Beskrivning| sträng| Beskrivningen av Profilmall.
UUID| GUID| Identifiering av Profilmall.
IsSmartcardProfileTemplate| bool| Anger om mallen är en Profilmall för smartkort.
IsVirtualSmartcardProfileTemplate| bool| Anger om profilmallen kräver ett virtuellt smartkort.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
