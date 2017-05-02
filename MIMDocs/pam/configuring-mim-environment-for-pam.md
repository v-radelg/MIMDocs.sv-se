---
title: "Konfigurera MIM 2016 för Privileged Access Management | Microsoft Docs"
description: "Översikt för att installera och MIM och konfigurering för Privileged Access Management."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 32815c4ddc51fb9c9187c9fc9a1710239faf7935
ms.lasthandoff: 01/24/2017


---

# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Konfigurera MIM-miljö för Privileged Access Management
Det finns sju steg att slutföra när du konfigurerar miljön för åtkomst mellan skogar, installerar och konfigurerar Active Directory och Microsoft Identity Manager och demonstrerar en just-in-time-åtkomstbegäran.

De här stegen är uppbyggda så att du kan skapa en testmiljö från grunden. Om du vill tillämpa PAM på en befintlig miljö kan du använda dina egna domänkontrollanter eller användarkonton i stället för att skapa nya som matchar exemplen.

1.  Förbered *CORPDC*-servern som en domänkontrollant och *CORPWKSTN* som en medlemsarbetsstation.

2.  Förbered *PRIVDC*-servern som en domänkontrollant.

3.  Förbered *PAMSRV*-servern i *PRIV* skogen.

4.  Installera MIM-komponenter på *PAMSRV* och cmdletar på en arbetsstation för en medlem i *CONTOSO*-skogen och förbered dem för Privileged Access Management.

5.  Upprätta förtroende mellan *PRIV*- och *CONTOSO*-skogar.

6.  Förbered privilegierade säkerhetsgrupper med åtkomst till skyddade resurser och medlemskonton för Just-in-time-hantering av privilegierad åtkomst.

7.  Demonstrera begäran, mottagande och användning av privilegierad utökad åtkomst till en skyddad resurs.

>[!div class="step-by-step"]
[Start »](step-1-prepare-corp-domain.md)

