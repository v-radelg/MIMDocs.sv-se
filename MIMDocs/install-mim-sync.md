---
title: "Installera synkroniseringstjänsten för Microsoft Identity Manager | Microsoft Docs"
description: "Kom igång med MIM 2016-komponenterna genom att installera och konfigurera Synkroniseringstjänsten."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 396c7066275db6123f15312cb8f0bc50d544275e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Installera MIM 2016: MIM-synkroniseringstjänsten
<a id="install-mim-2016-mim-synchronization-service" class="xliff"></a>

>[!div class="step-by-step"]
[«Exchange Server](prepare-server-exchange.md)
[MIM-tjänst och portal»](install-mim-service-portal.md)

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Till exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – **Pass@word1**

För att kunna installera Microsoft Identity Manager 2016-komponenterna måste du först installera installationspaketet.

1. Logga in som *contoso\Administrator* på den server som du använder för identitetshantering.

2. Packa upp installationspaketet MIM eller starta MIM-DVD:n.

## Installera MIM 2016-synkroniseringstjänsten
<a id="install-mim-2016-synchronization-service" class="xliff"></a>

1. Packa upp MIM-installationsmappen och navigera till mappen **Synkroniseringstjänst**.

2. Kör **installationsprogrammet för MIM-synkroniseringstjänsten**. Följ riktlinjerna i installationsprogrammet och slutför installationen.

3. På välkomstskärmen klickar du på **Nästa**.

    ![Bild på Välkommen till guiden för MIM-installationsprogrammet](media/MIM-Install1.png)

4. Läs igenom licensvillkoren och godkänn dem genom att klicka på **Nästa**.

5. På skärmen **Anpassad installation** klickar du på **Nästa**.

    ![Bild på anpassad installation](media/MIM-Install2.png)

6.  På skärmen för konfiguration av synkroniseringstjänstens databas väljer du:

    1.  SQL Server finns på: **Den här datorn**.

    2.  SQL Server-instansen är: **Standardinstansen**.

    ![Bild av databasanslutning](media/MIM-Install3.png)

7.  Konfigurera synkroniseringstjänstkontot enligt de konton du tidigare skapat:

    1.  Tjänstkonto: *MIMSync*

    2.  Lösenord: *Pass@word1*

    3.  Tjänstkontodomän eller namnet på den lokala datorn: *contoso*

    ![Bild av tjänstkonto](media/MIM-Install4.png)

8.  Ange de relevanta säkerhetsgrupperna i installationsprogrammet för MIM-synkroniseringstjänsten:

    1. Administratör = *contoso\MIMSyncAdmins*

    2. Operatör = *contoso\MIMSyncOperators*

    3. Sammanbindning = *contoso\MIMSyncJoiners*

    4. Bläddringsbehörighet för koppling = *contoso\MIMSyncBrowse*

    5. WMI-lösenordshantering= *contoso\MIMSyncPasswordReset*

    ![Bild på säkerhetsgrupper](media/MIM-Install5.png)

9. På skärmen för säkerhetsinställningar markerar du **Aktivera brandväggsregler för inkommande RPC-kommunikation** och klickar på **Nästa**.

10. Klicka på **Installera** för att påbörja installationen av MIM-synkroniseringstjänsten.

    1. Det kan hända att en varning om MIM-synkroniseringstjänstkontot visas. Klicka i så fall på **OK**.

    2. MIM-synkroniseringstjänsten kommer nu att installeras.

    3. Ett meddelande om att skapa en säkerhetskopia för krypteringsnyckeln visas – klicka på **OK**, välj sedan en mapp där säkerhetskopian av krypteringsnyckeln ska lagras.

        ![Bild av meddelande om att skapa en säkerhetskopia av krypteringsnyckeln för MIM-synkronisering](media/MIM-Install7.png)

    4. När installationsprogrammet har slutfört installationen klickar du på **Slutför**.

    5. Du måste logga ut och sedan logga in igen för att ändringarna av gruppmedlemskap ska börja gälla. Logga ut genom att klicka på **Ja**.

>[!div class="step-by-step"]  
[«Exchange Server](prepare-server-exchange.md)
[MIM-tjänst och portal»](install-mim-service-portal.md)
