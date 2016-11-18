---
title: Konfigurera SQL Server | Microsoft Docs
description: "Installera SQL Server 2014 inför installationen av MIM 2016."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 4ecb80282591ae5e4e52124637ab0e2edf0809b3


---

# <a name="set-up-an-identity-management-server-sql-server-2014"></a>Konfigurera en server för identitetshantering: SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Till exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – **Pass@word1**

## <a name="install-sql-server-2014-standard-edition"></a>Installera **SQL Server 2014 Standard Edition**

1. Starta **PowerShell** som domänadministratör.

2. Ändra till den katalog där installationsprogrammet för SQL Server finns.

3. Skriv in följande kommandon:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)



<!--HONumber=Nov16_HO2-->


