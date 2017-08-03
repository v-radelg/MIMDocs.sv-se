---
title: "Hämta smartkort principen | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>Principen för smartkort

Hämtar princip för profilen mallar för angivet arbetsflöde. Dessa data används när begäran skapades. Arbetsflödet principen anger vilka data krävs av klienten för att skapa en förfrågan. Sådana data kan omfatta: olika datainsamlingsobjekt, begäran kommentarer och en tid lösenordsprincip.

**Obs**: URL: er som visas i det här avsnittet är i förhållande till värdnamnet valt under distributionen av API, till exempel: `https://api.contoso.com`.
##<a name="request"></a>Begäran


Metod  |URL-begäran  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

###<a name="url-parameters"></a>URL-parametrar
Parameter| Beskrivning
--------|-------------
ID| Obligatoriskt. GUID som motsvarar profilmallen som principen ska extraheras från.
typ| Obligatoriskt. Vilken typ av princip som begärs. Möjliga värden är: *registrera*, *duplicera*, *OfflineUnblock*, *OnlineUpdate*, *förnya*, *återställa*, *RecoverOnBehalf*, *återställa*, *pensionera*, *återkalla*, *TemporaryEnroll*, *avblockera*.

###<a name="request-headers"></a>Huvuden för begäran
Vanliga begärandehuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="request-body"></a>Begärandetexten
inget

##<a name="response"></a>Svar
###<a name="response-codes"></a>Svarskoder
Kod  |Beskrivning  
---------|---------
200     | OK
403 | Tillåts inte
204 | Inget innehåll
500 | Internt fel

###<a name="response-headers"></a>Svarsrubriker
Vanliga svarshuvuden finns [HTTP-begäran och svarshuvuden](certificate-management-rest-api-service-details.md#http-request-and-response-headers) i *CM REST API-tjänsteinformation*.
###<a name="response-body"></a>Svarstexten
Returnerar ett grupprincipobjekt baserat på om åtgärden lyckades kan en [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) objekt. Minst principobjektet innehåller egenskaper i tabellen nedan, men kan innehålla ytterligare egenskaper beroende på den princip som begärdes. Till exempel en begäran om en princip för registrera returneras en [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) objekt. Mer information finns i dokumentationen för principobjektet som är associerade med parametern {type} i begäran. I dokumentationen för de olika typerna av principobjekt finns under den [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) dokumentation.

Egenskap | Beskrivning
---------|------------
ApprovalsNeeded | Antal godkännanden som krävs för FIM CM-begäranden för principen.
AuthorizedApprover | Säkerhetsbeskrivningen för användare som har behörighet att godkänna FIM CM-begäranden för principen.
AuthorizedEnrollmentAgent | Säkerhetsbeskrivningen för användare som kan fungera som registreringsagenter för principen.
AuthorizedInitiator | Säkerhetsbeskrivningen för användare som kan initiera FIM CM-begäranden för principen.
CollectComments | Ett booleskt värde som anger om kommentar samlingen är aktiverad för FIM CM-begäranden för principen.
CollectRequestPriority | Ett booleskt värde som anger om prioritet request-samlingen för FIM CM-begäranden för principen är aktiverad.
DefaultRequestPriority | Standardprioritet för FIM CM-begäranden för principen.
Dokument | De principdokument som är konfigurerade för principen.
Aktiverad | Ett booleskt värde som anger om principen är aktiverad.
EnrollAgentRequired | Ett booleskt värde som anger om registreringsagenter krävs för FIM CM-begäranden för principen.
OneTimePasswordPolicy | Hämtar hur engångslösenord för FIM CM-begäranden för principen distribueras.
Anpassning | Smartkort anpassningsalternativ för principen.
PolicyDataCollection | Dataobjekt som är associerade med principen för samlingen.
SelfServiceEnabled | Ett booleskt värde som anger om självbetjäning initiering av FIM CM-begäranden är aktiverad för principen.

##<a name="example"></a>Exempel

###<a name="request-1"></a>Begäran 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Svar 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Förfrågan 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Svar 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Se även

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy-klass](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
