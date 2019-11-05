---
title: Installera synkroniseringstjänsten för Microsoft Identity Manager | Microsoft Docs
description: Kom igång med MIM 2016-komponenterna genom att installera och konfigurera Synkroniseringstjänsten.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: b9bc580710a13b3a0868b0580d55469a753e7f33
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568089"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Installera MIM 2016: MIM-synkroniseringstjänsten

> [!div class="step-by-step"]
> [«Exchange Server](prepare-server-exchange.md)
> [MIM-tjänst och portal»](install-mim-service-portal.md)
 
> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Domänkontrollantens namn – **corpdc**
> - Domännamn – **contoso**
> - Server namn för MIM-tjänst – **corpservice**
> - MIM Sync Server-namn – **corpsync**
> - SQL Server namn – **corpsql**
> - Lösenord – <strong>Pass@word1</strong>

För att kunna installera Microsoft Identity Manager 2016-komponenterna måste du först installera installationspaketet.

1. Logga in som *contoso\miminstall* på den server som du använder för synkroniseringen Server **corpsync**i.

2. Packa upp installationspaketet MIM eller starta MIM-DVD:n.  Om du inte har den här DVD-skivan kan du läsa [Microsoft Identity Manager licensiering och nedladdningar](microsoft-identity-manager-licensing.md).

## <a name="install-mim-2016-sp1-synchronization-service"></a>Installera MIM 2016 SP1-synkroniseringstjänsten

1. Packa upp MIM-installationsmappen och navigera till mappen **Synkroniseringstjänst**.

2. Kör **installationsprogrammet för MIM-synkroniseringstjänsten**. Följ riktlinjerna i installationsprogrammet och slutför installationen.

3. På välkomstskärmen klickar du på **Nästa**.

    ![Bild på Välkommen till guiden för MIM-installationsprogrammet](media/install-mim-sync/MIM_Install1.png)

4. Läs igenom licensvillkoren och godkänn dem genom att klicka på **Nästa**.

5. På skärmen **Anpassad installation** klickar du på **Nästa**.

    ![Bild på anpassad installation](media/install-mim-sync/MIM_Install2.png)

6. På skärmen för konfiguration av synkroniseringstjänstens databas väljer du:

   1.  SQL Server finns på: **en fjärran sluten dator** som heter **corpsql.contoso.com**.

   2.  SQL Server-instansen är: **Standardinstansen**

   ![Bild av databasanslutning](media/install-mim-sync/MIM_Install3.png)

    3. *MIM 2016 SP2 och senare*: Konfigurera databas namnet för MIM-synkroniseringstjänsten

7. Konfigurera synkroniseringstjänstkontot enligt de konton du tidigare skapat:

   1. Tjänstkonto: *MIMSync*

   2. Lösenord: <em>Pass@word1</em>

   3. Tjänstkontodomän eller namnet på den lokala datorn: *contoso*

    >[!NOTE]
    >MIM 2016 SP2 och senare: om du vill gruppera hanterade tjänst konton ser du till att **$** -tecknen är i slutet av tjänst konto namnet, t. ex. MIMSync $, och lämnar fältet lösen ord tomt.

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
