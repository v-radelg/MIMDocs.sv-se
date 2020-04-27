---
title: Steg 3 Konfigurera SQL
description: Den här artikeln utgör steg 3 i den serie artiklar som beskriver hur du konfigurerar PIM med hjälp av skript och den beskriver de olika stegen för SQL-serverkonfiguration.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e9bc0345358a634adb0d7c0bdf9bd0f22ccea27e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043825"
---
# <a name="step-3-configuring-sql"></a>Steg 3 Konfigurera SQL

> [!div class="step-by-step"]
> [«Steg 2](sp1-step2-configuring-corp-domain.md)
> [steg 4»](sp1-step4-configuring-sharepoint.md)

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
> [«Steg 2](sp1-step2-configuring-corp-domain.md)
> [steg 4»](sp1-step4-configuring-sharepoint.md)
