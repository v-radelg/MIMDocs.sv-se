---
title: "Hämta alternativen för certifikatförfrågan Generation | Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Hämta Generation alternativen för certifikatförfrågan

Hämtar parametrar för klientsidan Certifikatgenereringen begäran.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/CertificateManagement/API/v1.0/Requests/{RequestId}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>URL-parametrar
Parameter | Beskrivning
--------|--------------
begärande-ID| Obligatoriskt. GUID-identifierare för MIM CM-begäran för certifikatet generation parametrarna som ska hämtas.

###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="request-body"></a>Begärandetexten
Ingen.


##<a name="response"></a>Svar
###<a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
200 | OK
204 | Inget innehåll
403 | Tillåts inte
500 | Internt fel

###<a name="response-headers"></a>Svarsrubriker
Vanliga svarshuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="response-body"></a>Svarstexten
Returnerar listan med objekt som CertificateRequestGenerationOptions på åtgärden lyckades. Varje CertificateRequestGenerationOptions objekt motsvarar en enda certifikatbegäran att klienten har att generera och har följande egenskaper:

Egenskap| Beskrivning
--------|-----------
Exportera | Ett värde som anger om den privata nyckeln som skapats för begäran kan exporteras.
FriendlyName | Visningsnamnet för det registrerade certifikatet.
HashAlgorithmName | Hash-algoritmen som används när du skapar certifikatets begäran signatur.
KeyAlgorithmName | Nyckelalgoritmen.
KeyProtectionLevel | Andelen starkt nyckelskydd.
KeySize | Storlek i bitar av den privata nyckeln som ska genereras.
KeyStorageProviderNames | En lista över godkända nyckellagring (KSP) som kan användas för att generera den privata nyckeln. I fallet får att första KSP inte kan användas för att generera begäran cert någon av de angivna KSP: er inte användas tills ett lyckas.
KeyUsages | Åtgärden kan utföras av den privata nyckeln som skapats för denna begäran. Standardvärdet är signering.
Ärende | Ämnesnamnet.

**Obs**: Mer information om dessa egenskaper finns i den [Windows.Security.Cryptography.Certificates.CertificateRequestProperties klassen](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) beskrivning, men tänk på att det inte är en överensstämmelsen mellan den här klassen och CertificateRequestGenerationOptions objekt.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
