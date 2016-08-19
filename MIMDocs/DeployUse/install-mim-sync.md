---
title: "Installera MIM-synkroniseringstjänst | Microsoft Identity Manager"
description: "Kom igång med MIM 2016-komponenterna genom att installera och konfigurera Synkroniseringstjänsten."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 2dfc89d32ef3b615201f1eb57ed3b8f5daed513e


---

# Installera MIM 2016: MIM-synkroniseringstjänsten

>[!div class="step-by-step"]
[«Exchange Server](prepare-server-exchange.md)
[MIM-tjänst och portal»](install-mim-service-portal.md)

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – **Pass@word1**

För att kunna installera Microsoft Identity Manager 2016-komponenterna måste du först installera installationspaketet.

1. Logga in som *contoso\Administrator* på den server som du använder för identitetshantering.

2. Packa upp installationspaketet MIM eller starta MIM-DVD:n.

## Installera MIM 2016-synkroniseringstjänsten

1. Packa upp MIM-installationsmappen och navigera till mappen **Synkroniseringstjänst**.

2. Kör **installationsprogrammet för MIM-synkroniseringstjänsten**. Följ riktlinjerna i installationsprogrammet och slutför installationen.

3. På välkomstskärmen klickar du på **Nästa**.

    ![Bild på Välkommen till guiden för MIM-installationsprogrammet](media/MIM-Install1.png)

4. Läs igenom licensvillkoren och godkänn dem genom att klicka på **Nästa**.

5. På skärmen **Anpassad installation** klickar du på **Nästa**.

    ![Bild på anpassad installation](media/MIM-Install2.png)

6.  På skärmen Konfiguration av synkroniseringsdatabasen väljer du:

    1.  SQL Server finns på: **Den här datorn**.

    2.  SQL Server-instansen är: **Standardinstansen**.

    ![Bild av databasanslutning](media/MIM-Install3.png)

7.  Konfigurera synkroniseringstjänstkontot enligt de konton du tidigare skapat:

    1.  Tjänstkonto: *MIMSync*

    2.  Lösenord: *Pass@word1*

    3.  Tjänstkontodomän eller namnet på den lokala datorn: *contoso*

    ![Bild av tjänstkonto](media/MIM-Install4.png)

8.  Ange de relevanta säkerhetsgrupperna i installationsprogrammet för MIM-synkronisering:

    1. Administratör = *contoso\MIMSyncAdmins*

    2. Operatör = *contoso\MIMSyncOperators*

    3. Sammanbindning = *contoso\MIMSyncJoiners*

    4. Bläddringsbehörighet för koppling = *contoso\MIMSyncBrowse*

    5. WMI-lösenordshantering= *contoso\MIMSyncPasswordReset*

    ![Bild på säkerhetsgrupper](media/MIM-Install5.png)

9. På skärmen för säkerhetsinställningar markerar du **Aktivera brandväggsregler för inkommande RPC-kommunikation** och klickar på **Nästa**.

10. Klicka på **Installera** för att starta installationen av MIM-synkronisering.

    1. Det kan hända att en varning om MIM-synkroniseringstjänstkontot visas. Klicka i så fall på **OK**.

    2. MIM-synkronisering installeras.

    3. Ett meddelande om att skapa en säkerhetskopia för krypteringsnyckeln visas – klicka på **OK**, välj sedan en mapp där säkerhetskopian av krypteringsnyckeln ska lagras.

        ![Bild av meddelande om att skapa en säkerhetskopia av krypteringsnyckeln för MIM-synkronisering](media/MIM-Install7.png)

    4. När installationsprogrammet har slutfört installationen klickar du på **Slutför**.

    5. Du måste logga ut och sedan logga in igen för att ändringarna av gruppmedlemskap ska börja gälla. Logga ut genom att klicka på **Ja**.

>[!div class="step-by-step"]  
[«Exchange Server](prepare-server-exchange.md)
[MIM-tjänst och portal»](install-mim-service-portal.md)



<!--HONumber=Jul16_HO3-->


