---
title: "Avbryt, Avbryt eller avsluta en begäran om | Microsoft Docs"
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>Avbryt, Avbryt eller slutföra en begäran
Markera en begäran om MIM CM slutfört, avbrutet eller avbrutna.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
PLACERA     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>URL-parametrar
Egenskap| Beskrivning
---------|--------
ID| Obligatoriskt. GUID för att slutföra begäran.


###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="request-body"></a>Begärandetexten
Begärandetexten innehåller följande egenskaper.

Egenskap | Beskrivning
---------|-----------
status | Status till begäran. Möjliga värden är: ”slutförd”, ”avbruten” eller ”Abandoned”.


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
Om åtgärden lyckades kan returnerar en [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) objekt som beskriver den MIM CM-begäran som har markerats som slutförd med följande egenskaper:

Namn | Beskrivning
-----|------------
Kommentar | Kommentaren som är associerad med MIM CM-begäran.
Slutfört | Den tid som MIM CM-begäran slutfördes.
DataCollection | Dataobjekt som är associerade med MIM CM-begäran för samlingen.
DataCollectionFlags | Alternativ för datainsamling för MIM CM-begäran.
Flaggor | De alternativ som är associerade med MIM CM-begäran.
IsDataCollectionComplete | Ett booleskt värde som anger om datainsamlingen har slutförts för MIM CM-begäran.
IsEnrollmentAgent | Ett booleskt värde som anger om en registreringsagent krävs för att köra MIM CM-begäran.
IsSmartcard | Ett booleskt värde som anger om MIM CM-begäran är en begäran om smartkort eller en programvarubegäran profil.
NewProfileUuid | Identiteten för den nya profilen för programvara som produceras av MIM CM-begäran.
NewSmartcardUuid | Identiteten för den nya smartkort som tilldelats av MIM CM-begäran.
OldProfileUuid | Identiteten för programvara profilen som MIM CM-begäran har skapats.
OldSmartcardUuid | Identitet för smartkortet som MIM CM-begäran har skapats.
OriginatorUserUuid | Identiteten för den användare som skickade begäran MIM CM
Prioritet | MIM CM-begäran prioritet.
ProfileTemplateUuid | Identitet av Profilmall som MIM CM-begäran har skapats.
RequestType | Typ av MIM CM-begäran.
Adtagent | Säkerhetsbeskrivningen för MIM CM-begäran.
Status | Status för MIM CM-begäran.
Har skickats | Den tid som MIM CM-begäran har skickats.
TargetUserUuid | Identiteten för målanvändaren för MIM CM-begäran.
UUID | Identifierare för MIM CM-begäran.

##<a name="example"></a>Exempel

###<a name="request"></a>Begäran
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>Svar
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>Se även

- [Microsoft.Clm.Provision.ExecuteOperations.Complete-metoden](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
