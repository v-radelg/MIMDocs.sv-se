---
title: Konfigurera SQL Server för Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Installera SQL Server 2016 som förberedelse för installationen av MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d32638bda8fd757233af0c697ea3d1ac9eb47eb9
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701377"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>Konfigurera en server för identitets hantering: SQL Server 2016

> [!div class="step-by-step"]
> [«Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
> 
> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Domänkontrollantens namn – **corpdc**
> - Domännamn – **contoso**
> - Server namn för MIM-tjänst – **corpservice**
> - MIM Sync Server-namn – **corpsync**
> - SQL Server namn – **corpsql**
> - Lösenord – <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Installera **SQL Server 2016 standard/Enterprise Edition**

1. Starta **PowerShell** som domänadministratör.

2. Ändra till den katalog där installationsprogrammet för SQL Server finns.

3. Skriv in följande kommandon:

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Mer information SQL-distributionspaket och-tjänster finns [här](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS ingår inte längre i SQL 2016. Information om hämtning hittar du [här](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)
> 
> [!div class="step-by-step"]  
> [«Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
