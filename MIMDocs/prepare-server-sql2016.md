---
title: Konfigurera SQL Server för Microsoft Identity Manager 2016 SP2 | Microsoft Docs
description: Installera SQL Server 2016 eller 2017 som förberedelse för installationen av MIM 2016.
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
ms.openlocfilehash: 757e5c49b12e47f1b5610fd96d4be319696be113
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329399"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Konfigurera en server för identitets hantering: SQL Server 2016 eller 2017

> [!div class="step-by-step"]
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
> 

> [!NOTE]
Installations proceduren för SQL Server 2017 skiljer sig inte från installations proceduren för SQL Server 2016.

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Domänkontrollantens namn – **corpdc**
> - Domännamn – **contoso**
> - Server namn för MIM-tjänst – **corpservice**
> - MIM Sync Server-namn – **corpsync**
> - SQL Server namn – **corpsql**
> - Lösenord – <strong>Pass@word1</strong>

> [!IMPORTANT]
MIM 2016 SP2 har stöd för SQL AlwaysOn tillgänglighets grupp (AoAG) med alternativet *RegisterAllProvidersIP* inställt på 0, vilket innebär att SQL Server AoAG mellan under nätet inte stöds för närvarande.

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
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
