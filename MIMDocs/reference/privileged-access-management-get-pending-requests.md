---
title: "Hämta väntande PAM-förfrågningar | Microsoft Docs"
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Hämta väntande PAM-förfrågningar
Används av ett konto med privilegier för att returnera en lista över väntande begäranden som måste godkännas.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `http://api.contoso.com`.
##<a name="request"></a>Begäran

Metod  |URL-begäran  
---------|---------
GET     |/API/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Frågeparametrar
Parameter | Beskrivning
----------|--------------
$filter | Valfritt. Ange de Perding PAM Request egenskaper i ett filteruttryck för att returnera en filtrerad lista över svar. Mer information om stöds operatorer finns [filtrering i information om PAM REST API-tjänsten](privileged-access-management-rest-api-service-details.md#filtering)
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
Ett lyckat svar innehåller en lista över PAM begäran om godkännande objekt med följande egenskaper.

Egenskap | Beskrivning
---------|-------------
RoleName | Visningsnamnet för rollen som krävs för godkännande.
Begärande | Användarnamn för den begärande godkänns.
Motiveringen | Motiveringen som anges av användaren.
RequestedTTL | Den begärda giltighetstid tiden i sekunder.
RequestedTime | Den begärda tiden för höjning.
SkapadTid | Skapandetid för begäran.
FIMRequestID | Innehåller ett element, ”värde” med den unika identifieraren (GUID) för PAM-förfrågan.
RequestorID | Innehåller ett element, ”värde” med den unika identifieraren (GUID) för Active Directory-konto som skapade PAM-förfrågan.
ApprovalObjectID | Innehåller ett element, ”värde” med den unika identifieraren (GUID) för godkännande-objektet.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
