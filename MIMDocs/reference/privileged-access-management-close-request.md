---
title: "Stänga PAM-förfrågan | Microsoft Docs"
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
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 181bf1a2953e461925d1b7efc5da4a2fa0c86914
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="close-pam-request"></a>Stänga PAM-förfrågan
Används av ett konto med privilegier för att stänga en begäran har påbörjats för att höja till en PAM-roll.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `http://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
POST     |/API/pamresources/pamrequests({RequestId)/Close

###<a name="url-parameters"></a>URL-parametrar
Parameter | Beskrivning
----------|-----------
begärande-ID | Identifierare (GUID) för PAM-förfrågan för att stänga anges enligt följande:`guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

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
inget
##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

```       
