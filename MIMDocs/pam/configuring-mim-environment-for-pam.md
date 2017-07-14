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
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: ce1ce0c67dfd39433ff01dabd542e862c557c787
ms.contentlocale: sv-se
ms.lasthandoff: 07/10/2017


---

# Konfigurera MIM-miljö för Privileged Access Management
<a id="configure-the-mim-environment-for-privileged-access-management" class="xliff"></a>
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

