---
title: Konfigurera MIM 2016 för Privileged Access Management | Microsoft Docs
description: Översikt för att installera och MIM och konfigurering för Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e80786bc6d59eb5f8acc7ef7999ebc52ddc84d7b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044063"
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

> [!div class="step-by-step"]
> [Start »](step-1-prepare-corp-domain.md)
