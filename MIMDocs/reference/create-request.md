---
title: "Skapa begäran | Microsoft Docs"
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Skapa begäran
Skapa en MIM CM-begäran.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
POST     |/CertificateManagement/API/v1.0/Requests

###<a name="url-parameters"></a>URL-parametrar
inget

###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="request-body"></a>Begärandetexten
Begärandetexten innehåller följande egenskaper.

Egenskap | Beskrivning
---------|-----------
profiletemplateuuid | Obligatoriskt. GUID för profilmallen som användaren skapar denna begäran.
datacollection | Obligatoriskt. En uppsättning namn / värde-par som representerar de data som ska tillhandahållas av denna registrerare. Insamling av nödvändiga data som måste anges kan hämtas från av Profilmall arbetsflöde principen. En tom samling kan anges.
mål | Valfritt. GUID för målanvändaren som begäran kommer att skapas för. Om inget annat anges, detta är som standard till den aktuella användaren.
typ | Obligatoriskt. Typ av begäran som håller på att skapas. Om tillgängliga begärandetyper är: ”registrera”, ”kopia”, ”OfflineUnblock”, ”OnlineUpdate”, ”förnya”, ”återställa”, ”RecoverOnBehalf”, ”återställa”, ”ta bort”, ”återkalla”, ”TemporaryCards” och ”avblockera”.<br/>**Obs**: stöds inte alla typer av begäran på alla profilmallar för. Du kan till exempel ange en avblockera åtgärd på en Profilmall för programvara.
kommentar | Obligatoriskt. Eventuella kommentarer som kan anges av användaren. Principen för arbetsflöde definierar om detta är nödvändigt. En tom sträng kan anges.
Prioritet | Valfritt. Prioritet för begäran. Om inget anges används begäran standardprioritet utifrån mallen profilinställningarna.


##<a name="response"></a>Svar
###<a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
201     | Skapad
403 | Tillåts inte
500 | Internt fel

###<a name="response-headers"></a>Svarsrubriker
Vanliga svarshuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="response-body"></a>Svarstexten
Returnerar URI: N för den nyligen skapade begäranden på åtgärden lyckades.
##<a name="example"></a>Exempel

###<a name="request-1"></a>Begäran 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Svar 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Förfrågan 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Svar 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Begäran 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>Se även

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll-metoden](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock-metoden](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover-metoden](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire-metoden](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock-metoden](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
