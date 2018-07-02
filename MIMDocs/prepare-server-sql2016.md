---
title: Konfigurera SQLServer för Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Installera SQL Server 2016 inför installationen av MIM 2016.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 915bf316fad2278ca1f62a9c2efd5850039d17a4
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289388"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>Konfigurera en server för Identitetshantering: SQL Server 2016

> [!div class="step-by-step"]
> [«Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
> 
> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Domänkontrollantens namn - **corpdc**
> - Domännamn – **contoso**
> - MIM-tjänsten Server name - **corpservice**
> - Servernamnet för MIM Sync - **corpsync**
> - Namnet på SQL Server - **corpsql**
> - Lösenord – <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Installera **SQL Server 2016 Standard/Enterprise Edition**

1. Starta **PowerShell** som domänadministratör.

2. Ändra till den katalog där installationsprogrammet för SQL Server finns.

3. Skriv in följande kommandon:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"

More info SQL deployment accounts and services can be found [here](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS is no longer included in SQL 2016 downlaod details can be found [here](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)    ```
> 
> [!div class="step-by-step"]  
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
