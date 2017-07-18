---
title: "Uppgradera från FIM 2010 R2 till Microsoft Identity Manager 2016 | Microsoft Docs"
description: "Lär dig hur du uppgraderar FIM 2010 R 2-komponenterna och sedan installerar de komponenter som är nya i MIM 2016."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 02/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 20e733f17d6ed590844c526888b649eb6bf5f322
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# Uppgradera från Forefront Identity Manager 2010 R2
<a id="upgrade-from-forefront-identity-manager-2010-r2" class="xliff"></a>

Använd den här artikeln som vägledning om du har en Forefront Identity Manager (FIM) 2010 R2-miljö och vill prova Microsoft Identity Manager (MIM) 2016. Det finns tre faser i uppgraderingen:

1.  Installera MIM-synkroniseringstjänsten (Sync) på en server som är domänansluten till Active Directory-domänen (AD). Det här ersätter FIM 2010 R2-instansen av Sync.

2.  Installera MIM-tjänsten och -portalen. Du kan nu även välja att installera registreringsportalen och tjänsteportalen för lösenordsåterställning via självbetjäning (SSPR). och exklusive funktionsuppsättningen för privilegierad åtkomst, som sedan installeras.

3.  Distribuera MIM-tillägg på en separat klientdator. Detta omfattar den integrerade klienten för SSPR Windows-inloggning.


Den här guiden förutsätter att du redan har konfigurerat följande:
- FIM 2010 R2 distribuerat i en testmiljö
- Servrar som kör Windows Server 2012, Windows Server 2012 R2 eller Windows Server 2008 R2
- Lokala och miljörelaterade krav (SQL Server, Exchange Server, SharePoint Services osv.) som har konfigurerats för FIM 2010 R2.


## Förberedelse
<a id="preparation" class="xliff"></a>

1.  Säkerhetskopiera FIM-tjänstdatabasen, FIM-synkroniseringsdatabasen, FIM-synkronisering och tjänstkonfiguration och programvara.

2.  På varje server där FIM 2010 R2-komponenterna är installerade – till exempel *CORPIDM* – loggar du in som Contoso\Administrator. I den här exempeldistributionen behövs administratörsbehörighet för att uppgradera FIM 2010 R2 till **MIM**.

3.  Hämta eller packa upp MIM-programvaran.

## Uppgradera synkroniseringstjänsten
<a id="upgrade-the-synchronization-service" class="xliff"></a>

1.  Logga in som administratör på en server där FIM 2010 R2-synkroniseringstjänsten (”Sync”) har distribuerats.

2.  Se till att säkerhetskopiera databasen innan du påbörjar den här proceduren.

3.  Öppna konsolen **Tjänster**, leta upp **Forefront Identity Manager-synkroniseringstjänsten** och stoppa den.

    ![Bild av tjänstkonsolen](media/MIM-UpgFIM1.PNG)

4.  Kör **installationsprogrammet för MIM-synkroniseringstjänsten**. Installationsprogrammet identifierar den befintliga Sync-versionen och föreslår en uppgradering. Klicka på knappen **Uppdatera** för att gå vidare.

5.  Om du godkänner licensvillkoren klickar du på **Nästa** för att gå vidare.

6.  Ange lösenordet för tjänstkontot som Sync använder och klicka på **Nästa**.

    ![Bild av Konfigurera tjänstkonto för MIM-synkronisering](media/MIM-UpgFIM3.png)

7.  Verifiera att namnen på säkerhetsgrupperna är korrekta och klicka på **Nästa**.

    ![Bild av Konfigurera säkerhetsgrupper i MIM-synkronisering](media/MIM-UpgFIM4.png)

8.  Låt kryssrutan för brandväggsregler för inkommande RPC-kommunikation vara oförändrad.

9. Installationsprogrammet är redo att uppgradera Sync från FIM 2010 R2 till MIM. Klicka på **Uppgradera** för att starta uppgraderingsprocessen.

10. Uppgraderingen pågår nu. Avsluta inte installationsprogrammet och starta inte om datorn medan uppgraderingen pågår.

    ![Bild av installationsstatus för MIM Sync](media/MIM-UpgFIM7.png)

11. Under uppgraderingen visas en varning om uppgraderingen av Sync-databasen Det rekommenderas att databasen har säkerhetskopierats innan detta.

12. När uppgraderingen är klar klickar du på **Slutför**.

    ![Bild som visar att installationen av MIM Sync har slutförts](media/MIM-UpgSP1.png)

13. Observera att **Synkroniseringstjänsten** har startats om.

## Uppgradera tjänsten och portalen
<a id="upgrade-the-service-and-portal" class="xliff"></a>

1.  Logga in som administratör på en server där FIM 2010 R2-tjänsten och -portalen har distribuerats.

2.  Öppna konsolen **Tjänster**, leta upp **Forefront Identity Manager-tjänsten** och stoppa den.

    ![Bild av tjänstkonsolen](media/MIM-UpgFIM9.PNG)

3.  Kör installationsprogrammet för MIM-tjänsten och -portalen. Klicka på knappen **Installera** för att gå vidare.

    ![Bild av Installera MIM-tjänsten och portalen](media/MIM-UpgSP2.png)

4.  Om du godkänner licensvillkoren klickar du på **Nästa** för att gå vidare.

5.  På skärmen MIM Customer Experience Improvement Program klickar du på **Nästa** för att gå vidare.

6.  Välj de MIM-funktioner och -komponenter du vill installera och klicka  på **Nästa** när du är klar.

    ![Bild på anpassad installation](media/MIM-UpgSP4.png)

    1.  **MIM-tjänsten:** den här funktionen måste finnas på minst en server och kräver en SQL Server-databasserver som antingen är samordnad eller finns på en annan server.

    2.  **MIM-portalen:** den här funktionen måste finnas på minst en server och kräver SharePoint 2013 Foundation.

    3.  **MIM-portal för lösenordsregistrering:** den här funktionen krävs för återställning av lösenord på egen hand.

    4.  **MIM-portal för återställning av lösenord:** den här funktionen krävs för återställning av lösenord.

7.  Ange information om den SQL Server som används för FIM-tjänstdatabasen. Välj alternativet att återanvända den befintliga databasen och spara dessa data. Klicka på **Nästa** för att gå vidare.

8. Om alternativet att återanvända den befintliga databasen har valts visas en påminnelse om att säkerhetskopiera databasen.

9. Ange information om e-postservern. Om e-postservern finns på den aktuella servern anger du ”localhost” som plats för e-postservern. Klicka på **Nästa** för att gå vidare.

    ![Bild på Konfigurera e-postserveranslutning](media/MIM-UpgSP6.png)

10. Välj ett certifikat för tjänsten som ska användas för att validera klienter. Du bör använda det befintliga certifikatet från det lokala certifikatarkivet som tidigare användes av FIM-tjänsten.

    ![Bild av Konfigurera tjänstcertifikat](media/MIM-UpgSP7.png)

    1.  Om alternativet lokalt certifikatarkiv har valts klickar du på knappen **Välj certifikat** och väljer ett certifikat i listan i popup-fönstret. Klicka på **OK** och sedan på **Nästa**.

        ![Bild på popup-fönster med certifikat](media/MIM-UpgSP8.PNG)

11. Konfigurera autentiseringsuppgifterna för tjänstkontot för MIM-tjänsten. Observera att tjänstkontot inte får vara samma tjänstkonto som används av synkroniseringstjänsten. Det måste vara samma konto som användes av FIM-tjänsten.

    ![Bild av Konfigurera MIM-tjänstkontot](media/MIM-UpgSP9.png)

12. Konfigurera uppgifterna om MIM Sync-servern enligt distributionen av MIM-tjänsten som du konfigurerade i ett föregående steg.

    ![Bild av Konfigurera MIM-tjänsten och portalen](media/MIM-UpgSP10.png)

13. När du installerar MIM-portalen ska du ange MIM-tjänstserverns adress. Klicka på **Nästa**.

14. När du installerar MIM-portalen ska du ange webbadressen för den SharePoint-webbplatssamling som för närvarande är värd för FIM-portalen. Klicka på **Nästa**.

## Installera MIM-portalen för lösenordsregistrering
<a id="install-the-mim-password-registration-portal" class="xliff"></a>

1. Om du installerar MIM-portalen för lösenordsregistrering ska du ange den webbadress som krävs för portalen för lösenordsregistrering. Klicka på **Nästa**.

2. Konfigurera funktionerna för klienter och slutanvändare så att de kan använda tjänsten och portalen.

    1.  Markera om du vill **Öppna portarna 5725 och 5726 i brandväggen**.

    2.  Markera om du vill **Bevilja autentiserade användare åtkomst till MIM-portalwebbplatsen**.

    3.  Klicka på **Nästa**.

3. Ange åtkomstuppgifter och autentiseringsuppgifter för MIM-portalen för registrering av lösenord.

    ![Bild av Konfigurera MIM-portalen för registrering av lösenord](media/MIM-UpgSP15.png)

    1.  Ange namnet på tjänstkontot (inklusive domän) och lösenordet för MIM-portalen för registrering av lösenord.

    2.  Ange uppgifter om värden – namn och port (t.ex. 8080) – för portalen för registrering av lösenord.

    3.  Markera alternativet **Öppna port i brandväggen**.

    4.  Klicka på **Nästa**.

4. På nästa konfigurationsskärm för MIM-portalen för registrering av lösenord:

    1.  Ange i MIM-portalen för registrering av lösenord var MIM-tjänsten är installerad, vanligtvis i samma system.

    2.  Ange om den här portalen ska kunna nås av både extranät- och intranätanvändare, eller endast av intranätanvändare, enligt den tidigare konfigurationen för återställning av lösenord i FIM.

## Installera MIM-portalen för återställning av lösenord
<a id="install-the-mim-password-reset-portal" class="xliff"></a>

1. Om du installerar MIM-portalen för återställning av lösenord ska du ange åtkomstuppgifter och autentiseringsuppgifter för återställning av lösenord i MIM.

    ![Bild av Konfigurera MIM-portalen för återställning av lösenord](media/MIM-UpgSP17.png)

    1.  Ange tjänstkontots namn (inklusive domän) och lösenordet för återställning av lösenord i MIM.

    2.  Ange uppgifter om värden – namn och port (t.ex. 8088) – för portalen för återställning av lösenord.

    3.  Markera alternativet **Öppna port i brandväggen**.

    4.  Klicka på **Nästa**.

2. På nästa konfigurationsskärm för återställning av lösenord i MIM:

    1.  Ange i MIM-portalen för återställning av lösenord var MIM-tjänsten är installerad.

    2.  Ange om den här portalen ska kunna nås av både extranät- och intranetanvändare, eller endast av intranetanvändare.

## Slutför installationen och uppgraderingen
<a id="finish-installation-and-upgrade" class="xliff"></a>

1. När alla definitioner för konfiguration är klara visas en installationssida. Klicka på **Installera** för att påbörja installationen och uppgraderingen av MIM-tjänsten och -portalen.

2. Installation och uppgradering av MIM-tjänsten och -portalen pågår nu. Avbryt inte installationsprogrammet och starta inte om datorn under installationen.

3. När installationen (uppgraderingen) av MIM-tjänsten och -portalen är klar visas en bekräftelseskärm. Klicka på **Slutför** för att slutföra installationen och avsluta installationsprogrammet.

4. Observera att **Forefront Identity Manager-tjänsten** har startats om.

Obs: Om FIM-tilläggen för närvarande finns distribuerade på användarens dator för SSPR ska du inte konfigurera de nya MFA-telefongrindarna för återställning av lösenord förrän alla FIM-tillägg har uppgraderats till MIM 2016.  Eftersom FIM 2010- och FIM 2010 R2-tilläggen inte kan identifiera de nya grindarna orsakar de ett fel och användaren kan inte slutföra  återställningen av lösenordet.

Riktlinjer för uppdatering av Microsoft Identity Manager 2016 SP1 finns i [Uppdateringspaketet för Microsoft Identity Manager 2016 Service Pack 1](https://blogs.technet.microsoft.com/iamsupport/2016/11/08/microsoft-identity-manager-2016-service-pack-1-update-package/)
