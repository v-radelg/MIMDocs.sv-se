---
title: "Installera Microsoft Identity Manager-tjänst och -portal | Microsoft Docs"
description: "Hämta anvisningarna för att konfigurera och installera MIM-tjänsten och -portalen för Microsoft Identity Manager 2016"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/16/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 03a0e6e90c1d0059872c4d37c88cb6f4f4095bac
ms.sourcegitcommit: 451c3ddda84758c87b8eb0359dac4043af52865f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/16/2017
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Installera MIM 2016: MIM-tjänst och -portal

>[!div class="step-by-step"]
[«Synkroniseringstjänst för MIM](install-mim-sync.md)
[Synkronisera databaser»](install-mim-sync-ad-service.md)

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Till exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – **Pass@word1**
> - Namn på tjänstkontot – **MIMService**

Om du inte installerade MIM-installationspaketet under det föregående steget ska du gå tillbaka och installera Microsoft Identity Manager 2016-komponenterna innan du fortsätter.


## <a name="configure-mim-service-and-portal-for-installation"></a>Konfigurera MIM-tjänsten och -portalen för installation

1. Kör **installationsprogrammet för MIM-tjänsten och -portalen** från den uppackade undermappen **Tjänst och portal**.

2. På välkomstskärmen klickar du på **Nästa**.

3. Läs licensavtalet och klicka på **Nästa** om du godkänner licensvillkoren.

4. På skärmen **MIM Customer Experience Improvement Program** klickar du på **Nästa**.

5. När du väljer komponentfunktioner för den här distributionen måste du vara noga med att inkludera funktionerna i MIM-tjänsten (utom MIM-rapportering) och MIM-portalen. Du kan även välja MIM-portalen för återställning av lösenord och MIM-tjänsten för meddelanden om lösenordsändring.

6. På sidan **Konfigurera MIM-databasanslutningen** väljer du **Skapa en ny databas**.

    ![Bild på Konfigurera MIM-databasanslutningen](media/MIM-Install10.png)

7. På sidan **Konfigurera anslutningen till e-postservern** anger du namnet på Exchange-servern som **e-postserver**. Om du inte har konfigurerat en e-postserver använder du **localhost** som namn på e-postservern och avmarkerar de två översta kryssrutorna. Klicka på **Nästa**.

    ![Bild på Konfigurera e-postserveranslutning](media/MIM-Install11.png)

8. Ange att du vill generera ett nytt självsignerat certifikat eller välj relevant certifikat.

9. Ange namnet på det tjänstkonto du ska använda, till exempel *MIMService*, lösenordet till tjänstkontot, till exempel *Pass@word1*, din tjänstkontodomän, till exempel *contoso*, samt tjänst-e-postkontot, till exempel *contoso*.

    ![Bild av Konfigurera MIM-tjänstkontot](media/MIM-Install12.png)

10. Observera att en varning kan komma att visas om att tjänstkontots nuvarande konfiguration inte är säker.

11. Godkänn standardinställningarna för Synkroniseringsserverns plats och ange kontot för MIM-hanteringsagenten som *contoso\MIMMA*.

    ![Bild på Konfigurera MIM-tjänsten och -portalen](media/MIM-Install13.png)

12. Ange *CORPIDM* (den här datorns namn) som MIM-tjänstens serveradress för MIM-portalen.

13. Ange *http://CorpIDM.contoso.local* som SharePoint-webbplatssamlingen.

14. Ange *http://CorpIDM.contoso.local:8080* som webbadress för Registrering av lösenord.

15. Ange *http://CorpIDM.contoso.local:8080* som webbadress för Återställning av lösenord.

16. Markera kryssrutan för att öppna portarna 5725 och 5726 i brandväggen, och kryssrutan för att bevilja alla autentiserade användare åtkomst till MIM-portalen.

## <a name="configure-mim-password-registration-portal"></a>Konfigurera MIM-portalen för registrering av lösenord

1.  Ange namnet på tjänstkontot för SSPR-registrering som *contoso\MIMSSPR* och lösenordet som *Pass@word1*.

2.  Ange *CORPIDM* som värdnamn för registrering av lösenord i MIM och ange porten som **8080**. Aktivera alternativet **Öppna port i brandväggen**.

    ![Bild på Ange konfigurationsinformation som används av IIS](media/MIM-Install14.png)

3.  En varning visas. Läs den och klicka på **Nästa**.

4. På nästa skärm för konfiguration av MIM-portalen för registrering av lösenord anger du *http://CorpIDM.contoso.local* som MIM-tjänstens serveradress till portalen för registrering av lösenord.

## <a name="configure-mim-password-reset-portal"></a>Konfigurera MIM-portalen för återställning av lösenord

1.  Ange namnet på tjänstkontot för SSPR-registrering som *Contoso\MIMSSPRService* och lösenordet som *Pass@word1*.

2.  Ange *CORPIDM* som värdnamn på portalen för återställning av lösenord i MIM och ange porten som **8088**. Aktivera alternativet **Öppna port i brandväggen**.

    ![Bild på Ange konfigurationsinformation som används av IIS](media/MIM-Install15.png)

3.  En varning visas. Läs den och klicka på **Nästa**.

4. På nästa skärm för konfiguration av MIM-portalen för registrering av lösenord anger du *CorpIDname http://CorpIDname.domain.local* som MIM-tjänstens serveradress till portalen för att återställa lösenord.

## <a name="install-mim-service-and-portal"></a>Installera MIM-tjänsten och -portalen

När alla förberedande definitioner för installationen är klara klickar du på **Installera** för att starta installationen av de valda komponenterna i **Tjänsten och portalen**.

Verifiera att MIM-portalen är aktiv när installationen är klar.

1. Starta Internet Explorer och Anslut till MIM-portalen på *http://corpidm.contoso.local/identitymanagement*. Observera att det kan bli en kort fördröjning när du besöker sidan för första gången.

    - Om det blir nödvändigt autentiserar du som *contoso\Administrator* i Internet Explorer.

2. I Internet Explorer går du till **Internetalternativ**, sedan till fliken **Säkerhet** och lägger till webbplatsen i zonen **Lokalt nätverk** om den inte redan finns där.  Stäng dialogrutan **Internetalternativ**.

3. Tillåt användare att visa sina egna poster i MIM.

    1.  Gå till **MIM-portalen** i Internet Explorer och klicka på **Hanteringsprincipregler (MPR)**.

    2.  Sök efter hanteringsprincipregeln **Användarhantering: användare får läsa sina egna attribut**.

    3.  Välj den här hanteringsprincipregeln och avmarkera **Hanteringsprincipregeln (MPR) är inaktiverad**.

    4.  Klicka på **OK** och därefter på **Skicka**.

4.  Verifiera att brandväggen tillåter inkommande anslutningar till TCP-portarna 5725 och 5726.

    1.  Starta **Administrationsverktyg » Windows-brandväggen** med **avancerad säkerhet**.

    2.  Klicka på **Regler för inkommande trafik**.

    3.  Verifiera att följande två regler visas:

        -   Forefront Identity Manager-tjänsten (STS).

        -   Forefront Identity Manager-tjänsten (Webbtjänst).

    4.  Slutför guiden och stäng programmet **Windows-brandvägg**.

    5.  Starta **Kontrollpanelen » Nätverk och Internet » Visa nätverksstatus och -åtgärder**.

    6.  Verifiera att det finns ett aktivt nätverk listat som contoso.local som ett domännätverk.

    7.  Stäng **kontrollpanelen**.

> [!NOTE]
> Valfritt: nu kan du installera MIM-tillägg.

>[!div class="step-by-step"]  
[«Synkroniseringstjänst för MIM](install-mim-sync.md)
[Synkronisera databaser»](install-mim-sync-ad-service.md)
