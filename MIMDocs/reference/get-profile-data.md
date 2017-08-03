---
title: "Hämta Data för användarprofiler | Microsoft Docs"
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Hämta Data för användarprofiler
Hämtar en lista över en användares programvara certifikatprofiler med en lista över möjliga åtgärder som kan utföras av den aktuella användaren. Sedan kan du initiera en begäran för någon av de angivna åtgärderna.

**Obs**: servern ställer in PIN-koden endast om mallen profilprincip anger att det ska göras. I annat fall måste användaren ange den.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles<br/>/ CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/API/v1.0/Requests/{RequestId}/Profiles

###<a name="url-parameters"></a>URL-parametrar
Parameter | Beskrivning
---------|------------
ID | Identifierare (GUID) för profilen som ska returneras.
begärande-ID | Identifierare för att returnera profiler för begäran.

###<a name="query-parameters"></a>Frågeparametrar
Parameter | Beskrivning
---------|------------
status | Valfritt. Anger status för profiler för att hämta data. För möjliga statustyper är: ”aktiv”, ”godkänd”, ”avbrutits”, ”slutfört”, ”nekad”, ”körs”, ”misslyckades”, ”None” och ”väntande”. <br/>Om ingen status anges returneras alla profiler, oavsett status.
###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="request-body"></a>Begärandetexten
inget

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
På lyckas, returneras en lista med JSON-serialiserad [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) objekt med följande egenskaper:

Egenskap | Beskrivning
---------|------------
AssignedUserUuid | Identifierare för den användare som tilldelats profilen.
Kommentar | Kommentar som beskriver profilen.
Flaggor | Flaggor som beskriver profilen.
ParentProfileUuid | Identifierare för den gamla profilen profilen har ersatt.
PrimaryProfileUuid | Identifierare för den primära profilen.
ProfileOperations | Lista över möjliga åtgärder som kan utföras av den aktuella användaren på profilen.
ProfileTemplateUuid | Identifierare av Profilmall som innehåller de principer och inställningar som styr profilen.
ProfileTemplateVersion | Versionen av Profilmall när profilen har skapats.
Status | Status för profilen.
UUID | Profilens identifierare.


##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>Se även

- [Microsoft.Clm.Shared.Profiles.Profile-klass](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
