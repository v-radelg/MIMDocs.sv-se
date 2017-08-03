---
title: Get-smartkort eller certifikat profil | Microsoft Docs
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
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 786562f1a6ffd0dcaefd7b11af8756b08727ecae
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-or-profile-certificates"></a>Get-smartkort eller certifikat profil
Hämtar en lista över certifikat som är associerade med viss profil för smartkort eller programvara.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles/{ID}/Certificates <br/>/CertificateManagement/API/v1.0/SmartCards/{ID}/Certificates

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
På lyckas, returneras en lista med JSON-serialiserad [Microsoft.Clm.Shared.Certificates.X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx) objekt med följande egenskaper:

Namn | Beskrivning
-----|------------
ArchivedOnCa | Ett booleskt värde som anger om certifikatet arkiveras på certifikatutfärdaren (CA).
CertificateType | Typ av certifikatet.
isKeyHistory | Ett booleskt värde som anger om certifikatet är ett certifikat för viktiga historik.
Serienummer | Certifikatets serienummer.
TemplateCommonName | Certifikatmallens vanliga namn för certifikatet.
Tumavtryck | Certifikatets tumavtryck.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       
##<a name="see-also"></a>Se även

- [Microsoft.Clm.Provision.FindOperations.FindCertificates-metoden](https://msdn.microsoft.com/library/microsoft.clm.provision.findoperations.findcertificates.aspx)
- [Microsoft.Clm.Shared.Certificates.X509ClmCertificate-klass](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx)
