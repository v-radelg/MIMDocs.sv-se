---
title: Steg 3 Konfigurera SQL
description: Den här artikeln utgör steg 3 i den serie artiklar som beskriver hur du konfigurerar PIM med hjälp av skript och den beskriver de olika stegen för SQL-serverkonfiguration.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 1196d16e03eed13056877584626e3ea7ab4739d6
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289255"
---
# <a name="step-3-configuring-sql"></a>Steg 3 Konfigurera SQL

> [!div class="step-by-step"]
> [« Steg 2](sp1-step2-configuring-corp-domain.md)
> [Steg 4 »](sp1-step4-configuring-sharepoint.md)

Innan du fortsätter med stegen nedan kontrollerar du att du använder SQL Server 2012 SP1 eller senare, eller SQL Server 2014. För domänanslutna servrar loggar du in med MIMAdmin-kontot. Annars loggar du in som lokal administratör.
1. Kör PowerShell som administratör
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Välj menyalternativ 3 (SQL Server-konfiguration)

   Om servern inte är domänansluten till PRIV-domänen än uppmanas du att ange dina autentiseringsuppgifter och att ansluta servern till domänen.
   Datorn startas om efter domänanslutningen. Efter omstarten loggar du in på servern med MIMAdmin-kontot.

5. Kör PowerShell som administratör
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Välj menyalternativ 3 (SQL Server-konfiguration)

När du uppmanas att göra det anger du lösenordet för MIMAdmin-tjänstkontot och låter installationen fortsätta. När installationen är klar går du vidare till steg 4.

> [!div class="step-by-step"]
> [« Steg 2](sp1-step2-configuring-corp-domain.md)
> [Steg 4 »](sp1-step4-configuring-sharepoint.md)
