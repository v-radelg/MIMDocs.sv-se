---
title: Steg 3 Konfigurera SQL
description: "Förbered CORP-domänen med befintliga eller nya identiteter som ska hanteras av Privileged Identity Manager med hjälp av skript"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# Steg 3 Konfigurera SQL

Innan du fortsätter med stegen nedan kontrollerar du att du använder SQL Server 2012 SP1 eller senare, eller SQL Server 2014. För domänanslutna servrar loggar du in med MIMAdmin-kontot. Annars loggar du in som lokal administratör.
1. Kör PowerShell som administratör
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Välj menyalternativ 3 (SQL Server-konfiguration)

  Om servern inte är domänansluten till PRIV-domänen än uppmanas du att ange dina autentiseringsuppgifter och att ansluta servern till domänen.
  Datorn startas om efter domänanslutningen. Efter omstarten loggar du in på servern med MIMAdmin-kontot.

1. Kör PowerShell som administratör
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Välj menyalternativ 3 (SQL Server-konfiguration)

När du uppmanas att göra det anger du lösenordet för MIMAdmin-tjänstkontot och låter installationen fortsätta. När installationen är klar går du vidare till steg 4.



<!--HONumber=Sep16_HO4-->


