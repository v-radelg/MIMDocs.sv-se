---
title: "Konfigurera MIM 2016 för Privileged Access Management | Microsoft Docs"
description: "Översikt för att installera och MIM och konfigurering för Privileged Access Management."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 79de8ef08b49a41638a09e829efbd8c17301004b
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/01/2017
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Konfigurera MIM-miljö för Privileged Access Management

Det finns sju steg att slutföra när du konfigurerar miljön för åtkomst mellan skogar, installerar och konfigurerar Active Directory och Microsoft Identity Manager och demonstrerar en just-in-time-åtkomstbegäran.

De här stegen är uppbyggda så att du kan skapa en testmiljö från grunden. Om du vill tillämpa PAM på en befintlig miljö kan du använda dina egna domänkontrollanter eller användarkonton i stället för att skapa nya som matchar exemplen.

1. Förbered *CORPDC*-servern som en domänkontrollant och *CORPWKSTN* som en medlemsarbetsstation.

2. Förbered *PRIVDC*-servern som en domänkontrollant.

3.  Förbered *PAMSRV*-servern i *PRIV* skogen.

4.  Installera MIM-komponenter på *PAMSRV* och cmdletar på en arbetsstation för en medlem i *CONTOSO*-skogen och förbered dem för Privileged Access Management.

5.  Upprätta förtroende mellan *PRIV*- och *CONTOSO*-skogar.

6.  Förbered privilegierade säkerhetsgrupper med åtkomst till skyddade resurser och medlemskonton för Just-in-time-hantering av privilegierad åtkomst.

7.  Demonstrera begäran, mottagande och användning av privilegierad utökad åtkomst till en skyddad resurs.

>[!div class="step-by-step"]
[Start »](step-1-prepare-corp-domain.md)
