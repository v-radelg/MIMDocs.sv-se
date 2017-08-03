---
title: Privileged Access Management REST API-referens | Microsoft Docs
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
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Privileged Access Management REST API-referens
Microsoft Identity Manager (MIM) 2016 lägger till ett nytt scenario kallas Privileged Access Management (PAM). PAM gör det möjligt för organisationer att ha mer kontroll över åtkomsträttigheter till känsliga resurser hög Privilegierade användarkonton, till exempel system eller tjänstadministratörer. PAM styr Privilegierade kontoåtkomst genom att tillhandahålla begränsad tid åtkomsträttigheter precis i tid (JIT) när åtkomstbehörigheter behövs.

En användare kan be MIM-tjänsten för privilegierad åtkomst (höjningen) i ett av två sätt:

- Med PAM REST-API
- Med hjälp av cmdleten New-PAMRequest för PAM PowerShell

I den här guiden beskrivs PAM REST-API. Mer information om hur du använder PowerShell cmdlet finns i Test Lab-Guide: demonstrera Privileged Access Management med hjälp av Microsoft Identity Manager, som finns på connect-webbplatsen.

##<a name="pam-rest-api-resources-and-operations"></a>PAM REST API-resurser och åtgärder
PAM REST-API som körs på följande resurser
- **PAM Role**: A PAM-roll associerar en samling användare med en samling åtkomsträttigheter. Åtkomstbehörigheter definieras av referens till säkerhetsgrupper.  Varje PAM-rollen har en lista över användarkonton, kallas kandidater som har rätt att upphöja till PAM-rollen. Du kan utföra följande åtgärder på PAM-roller:

    - [Hämta PAM-roller](privileged-access-management-get-roles.md)

- **PAM-Request**: en användare som vill höja till en åtkomsträttigheter för PAM-roll har att skicka en PAM-förfrågan och få godkännande för begäran för att höja. PAM-Request-objektet spårar livscykeln för den här förfrågan i MIM-tjänsten. Du kan utföra följande åtgärder för PAM-förfrågningar:

    - [Skapa PAM-begäran](privileged-access-management-create-request.md)
    - [Hämta PAM-begäranden](privileged-access-management-get-requests.md)
    - [Stäng PAM-begäran](privileged-access-management-close-request.md)

- **Väntande PAM Request**: används för att godkänna eller avvisa PAM-förfrågningar som har skickats av användare. Du kan utföra följande åtgärder på väntande begäranden om PAM:

    - [Hämta väntande PAM-begäranden](privileged-access-management-get-pending-requests.md)
    - [Godkänn eller avvisa en väntande PAM-begäran](privileged-access-management-approve-reject-pending-request.md)

- **PAM-Session**: när du använder PAM REST-API kan klienten (till exempel en webbläsare) har en session med PAM REST API-slutpunkt. Klienten är autentiserad till REST API-slutpunkt i den här sessionen. Du kan utföra följande åtgärder på PAM-sessioner:

     - [Hämta PAM-sessionsinformation](privileged-access-management-get-session-info.md)

Mer detaljerad information om tjänsten finns [information om PAM REST API-tjänsten](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>PAM-exempelportalen på GitHub
Ett sätt att lära dig hur du använder PAM REST API är med hjälp av PAM-exempelportalen en exempel-webbprogram som använder API: et. Du hittar koden för PAM Sample-portal i den [PAM exempel lagringsplats på GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Du kan lära dig hur du distribuerar exempelportalen i Test Lab-Guide för PAM.
