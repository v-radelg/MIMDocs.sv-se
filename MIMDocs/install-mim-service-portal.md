---
title: Installera Microsoft Identity Manager-tjänst och -portal | Microsoft Docs
description: Hämta anvisningarna för att konfigurera och installera MIM-tjänsten och -portalen för Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/30/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fc70a0edb610fcb862d85902dd5e611e65f6e689
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825815"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Installera MIM 2016: MIM-tjänst och -portal

> [!div class="step-by-step"]
> [«Synkroniseringstjänst för MIM](install-mim-sync.md)
> [Synkronisera databaser»](install-mim-sync-ad-service.md)
> 
> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – <strong>Pass@word1</strong>
> - Namn på tjänstkontot – **MIMService**

Om du inte installerade MIM-installationspaketet under det föregående steget ska du gå tillbaka och installera Microsoft Identity Manager 2016-komponenterna innan du fortsätter.


## <a name="configure-mim-service-and-portal-for-installation"></a>Konfigurera MIM-tjänsten och -portalen för installation

1. Kör **installationsprogrammet för MIM-tjänsten och -portalen** från den uppackade undermappen **Tjänst och portal**.

2. På välkomstskärmen klickar du på **Nästa**.

3. Läs licensavtalet och klicka på **Nästa** om du godkänner licensvillkoren.

4. På skärmen **MIM Customer Experience Improvement Program** klickar du på **Nästa**.

5. När du väljer komponentfunktioner för den här distributionen måste du vara noga med att inkludera funktionerna i MIM-tjänsten (utom MIM-rapportering) och MIM-portalen. Du kan även välja MIM-portalen för återställning av lösenord och MIM-tjänsten för meddelanden om lösenordsändring.

6. På sidan **Konfigurera MIM-databasanslutningen** väljer du **Skapa en ny databas**.

    ![Bild på Konfigurera MIM-databasanslutningen](media/install-mim-service-portal/MIM_Install10.png)

7. På den **Konfigurera e-serveranslutning**, ange namnet på Exchange-servern som **e-postserver** eller så kan du använda **O365 postlåda**. Om du inte har konfigurerat en e-postserver använder du **localhost** som namn på e-postservern och avmarkerar de två översta kryssrutorna. Klicka på **Nästa**.

    ![Bild på Konfigurera e-postserveranslutning](media/install-mim-service-portal/MIM_Install11.png)

8. Ange att du vill generera ett nytt självsignerat certifikat eller välj relevant certifikat.

9. Ange namnet på det tjänstkonto du ska använda, till exempel *MIMService*, lösenordet till tjänstkontot, till exempel <em>Pass@word1</em>, din tjänstkontodomän, till exempel *contoso*, samt tjänst-e-postkontot, till exempel *contoso*.

    ![Bild av Konfigurera MIM-tjänstkontot](media/install-mim-service-portal/MIM_Install12.png)

10. Observera att en varning kan komma att visas om att tjänstkontots nuvarande konfiguration inte är säker.

11. Acceptera standardinställningarna för Synkroniseringsserverns plats och ange kontot för MIM-hanteringsagenten som *contoso\MIMMA*.

    ![Bild på Konfigurera MIM-tjänsten och -portalen](media/install-mim-service-portal/MIM_Install13.png)

12. Ange *CORPIDM* (den här datorns namn) som MIM-tjänstens serveradress för MIM-portalen.

13. Ange `*http://mim.contoso.com*` som SharePoint site URL: en samling.

14. Ange `*http://passwordregistration.contoso.com*` som lösenord Registreringswebbadress-port 80 rekommenderar att du uppdaterar senare med SSL-certifikat på 443.

15. Ange `*http://passwordreset.contoso.com*` som URL för återställning av lösenord-port 80 rekommenderar att du uppdaterar senare med SSL-certifikat på 443.

16. Markera kryssrutan för att öppna portarna 5725 och 5726 i brandväggen, och kryssrutan för att bevilja alla autentiserade användare åtkomst till MIM-portalen.

## <a name="configure-mim-password-registration-portal"></a>Konfigurera MIM-portalen för registrering av lösenord

1. Ange namnet på tjänstkontot för SSPR-registrering som *contoso\MIMSSPR* och lösenordet som <em>Pass@word1</em>.

2. Ange *passwordregistration.contoso.com* som värdnamn för registrering av lösenord i MIM och ange porten som **80**. Aktivera alternativet **Öppna port i brandväggen**.

   ![Bild på Ange konfigurationsinformation som används av IIS](media/install-mim-service-portal/MIM_Install14.png)

3. En varning visas. Läs den och klicka på **Nästa**.

4. I nästa MIM-portalen för Lösenordsregistrering skärm för konfiguration, ange *mim.contoso.com* som MIM-tjänstens serveradress för för portalen för registrering av lösenord.

## <a name="configure-mim-password-reset-portal"></a>Konfigurera MIM-portalen för återställning av lösenord

1. Ange namnet på tjänstkontot för SSPR-registrering *Contoso\MIMSSPR* och lösenordet som <em> Pass@word1 </em>.

2. Ange *passwordreset.contoso.com* som värdnamn för MIM lösenord portalen för återställning, och ange porten som **80**. Aktivera alternativet **Öppna port i brandväggen**.

   ![Bild på Ange konfigurationsinformation som används av IIS](media/install-mim-service-portal/MIM_Install15.png)

3. En varning visas. Läs den och klicka på **Nästa**.

4. I nästa MIM-portalen för Lösenordsregistrering skärm för konfiguration, ange *mim.contoso.com* som MIM-tjänstens serveradress för för i portalen för återställning.

## <a name="install-mim-service-and-portal"></a>Installera MIM-tjänsten och -portalen

När alla förberedande definitioner för installationen är klara klickar du på **Installera** för att starta installationen av de valda komponenterna i **Tjänsten och portalen**.

Verifiera att MIM-portalen är aktiv när installationen är klar.

1. Starta Internet Explorer och Anslut till MIM-portalen på * http://mim.contoso.com/identitymanagement *. Observera att det kan bli en kort fördröjning när du besöker sidan för första gången.

    - Om det behövs kan autentisera sig som *contoso\miminstall* till Internet Explorer.

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
> 
> [!div class="step-by-step"]  
> [«Synkroniseringstjänst för MIM](install-mim-sync.md)
> [Synkronisera databaser»](install-mim-sync-ad-service.md)
