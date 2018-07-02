---
title: Installera synkroniseringstjänsten för Microsoft Identity Manager | Microsoft Docs
description: Kom igång med MIM 2016-komponenterna genom att installera och konfigurera Synkroniseringstjänsten.
keywords: ''
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 05/01/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c68b33b2ff28d75b6f4e63fa8caf0c87727a5927
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289405"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Installera MIM 2016: MIM-synkroniseringstjänsten

> [!div class="step-by-step"]
> [«Exchange Server](prepare-server-exchange.md)
> [MIM-tjänst och portal»](install-mim-service-portal.md)
> 
> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Domänkontrollantens namn - **corpdc**
> - Domännamn – **contoso**
> - MIM-tjänsten Server name - **corpservice**
> - Servernamnet för MIM Sync - **corpsync**
> - Namnet på SQL Server - **corpsql**
> - Lösenord – <strong>Pass@word1</strong>

För att kunna installera Microsoft Identity Manager 2016-komponenterna måste du först installera installationspaketet.

1. Logga in som *contoso\miminstall* till servern som du använder för server för Identitetshantering synkroniseringen **corpsync**.

2. Packa upp installationspaketet MIM eller starta MIM-DVD:n.

## <a name="install-mim-2016-sp1-synchronization-service"></a>Installera MIM 2016-synkroniseringstjänsten med SP1

1. Packa upp MIM-installationsmappen och navigera till mappen **Synkroniseringstjänst**.

2. Kör **installationsprogrammet för MIM-synkroniseringstjänsten**. Följ riktlinjerna i installationsprogrammet och slutför installationen.

3. På välkomstskärmen klickar du på **Nästa**.

    ![Bild på Välkommen till guiden för MIM-installationsprogrammet](media/install-mim-sync/MIM_Install1.png)

4. Läs igenom licensvillkoren och godkänn dem genom att klicka på **Nästa**.

5. På skärmen **Anpassad installation** klickar du på **Nästa**.

    ![Bild på anpassad installation](media/install-mim-sync/MIM_Install2.png)

6. På skärmen för konfiguration av synkroniseringstjänstens databas väljer du:

   1.  SQL Server finns på: **A fjärrdatorn** kallas **corpsql.contoso.com**.

   2.  SQL Server-instansen är: **Standardinstansen**

   ![Bild av databasanslutning](media/install-mim-sync/MIM_Install3.png)

7. Konfigurera synkroniseringstjänstkontot enligt de konton du tidigare skapat:

   1. Tjänstkonto: *MIMSync*

   2. Lösenord: <em>Pass@word1</em>

   3. Tjänstkontodomän eller namnet på den lokala datorn: *contoso*

   ![Bild av tjänstkonto](media/install-mim-sync/MIM_Install4.png)

8. Ange de relevanta säkerhetsgrupperna i installationsprogrammet för MIM-synkroniseringstjänsten:

   1. Administratör = *contoso\MIMSyncAdmins*

   2. Operatör = *contoso\MIMSyncOperators*

   3. Sammanbindning = *contoso\MIMSyncJoiners*

   4. Bläddringsbehörighet för koppling = *contoso\MIMSyncBrowse*

   5. WMI-lösenordshantering= *contoso\MIMSyncPasswordReset*

   ![Bild på säkerhetsgrupper](media/install-mim-sync/MIM_Install5.png)

9. På skärmen för säkerhetsinställningar markerar du **Aktivera brandväggsregler för inkommande RPC-kommunikation** och klickar på **Nästa**.

10. Klicka på **Installera** för att påbörja installationen av MIM-synkroniseringstjänsten.

    1. Det kan hända att en varning om MIM-synkroniseringstjänstkontot visas. Klicka i så fall på **OK**.

    2. MIM-synkroniseringstjänsten kommer nu att installeras.

    3. Ett meddelande om att skapa en säkerhetskopia för krypteringsnyckeln visas – klicka på **OK**, välj sedan en mapp där säkerhetskopian av krypteringsnyckeln ska lagras.

        ![Bild av meddelande om att skapa en säkerhetskopia av krypteringsnyckeln för MIM-synkronisering](media/MIM-Install7.png)

    4. När installationsprogrammet har slutfört installationen klickar du på **Slutför**.

    5. Du måste logga ut och sedan logga in igen för att ändringarna av gruppmedlemskap ska börja gälla. Logga ut genom att klicka på **Ja**.

> [!div class="step-by-step"]  
> [«Exchange Server](prepare-server-exchange.md)
> [MIM-tjänst och portal»](install-mim-service-portal.md)
