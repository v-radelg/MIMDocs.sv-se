---
title: Arbeta med självbetjäning i portalen för återställning av lösenord | Microsoft Docs
description: Se vad som är nytt med självbetjäning för återställning av lösenord i MIM 2016, inklusive hur SSPR fungerar med multifaktorautentisering.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e2c252771bd114a3c70b900c8c089e09eff8cd23
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2018
---
>[!IMPORTANT]
På grund av ett meddelande om utfasning av Azure Multi-Factor Authentication SDK. Azure MFA SDK ska ha stöd för befintliga kunder fram till datumet för tillbakadragandet av 14 November 2018. Nya kunder och aktuella kunder kommer inte att hämta SDK längre via den klassiska Azure-portalen. Om du vill hämta du behöver nå ut till Azure kundsupport för att ta emot dina autentiseringsuppgifter för MFA-paketet. <br> Utvecklingsgruppen Microsoft arbetar på Planering av MFA ändringar genom att integrera med MFA-serverns SDK. Detta tas med i kommande snabbkorrigeringen i tidig 2018.

# <a name="working-with-self-service-password-reset"></a>Arbeta med självbetjäning för återställning av lösenord
Microsoft Identity Manager 2016 innehåller ytterligare funktioner för återställning av lösenord via självbetjäning. Den här funktionen har förbättrats med flera viktiga funktioner:

-   Portalen för självbetjäning för återställning av lösenord och Windows inloggningssida ger nu användarna möjlighet att låsa upp sina konton utan att ändra lösenord eller kontakta support. Det finns många legitima orsaker till att användare spärras, som att de anger ett gammalt lösenord, använder en dator eller ett tangentbord med fel språk inställt eller att de försöker logga in på en delad arbetsstation där någon annan redan är inloggad.

-   En ny autentiseringsgate, telefonporten, har lagts till. Den ger möjlighet till användarautentisering via ett telefonsamtal.

-   Det finns numera även stöd för tjänsten Microsoft Azure Multi-Factor Authentication (MFA) Den kan användas antingen för den befintliga SMS-gaten för engångslösenord eller den nya telefonporten.

## <a name="azure-for-multi-factor-authentication"></a>Azure för multifaktorautentisering
Microsoft Azure Multi-Factor Authentication är en autentiseringstjänst där användare måste bekräfta sina inloggningsförsök via en mobilapp, ett telefonsamtal eller SMS. Den är tillgänglig för användning med Microsoft Azure Active Directory och som en tjänst för molnprogram och lokala företagsprogram.

Azure MFA ger en extra autentiseringsmetod som kan förstärka befintliga autentiseringsprocesser, t.ex. den som utförs av MIM för hjälp vid inloggning via självbetjäning.

När Azure MFA används autentiseras användare i systemet för att verifiera deras identitet när de försöker att komma åt sina konton och sina resurser. Autentisering kan genomföras via SMS eller telefon.   Ju starkare autentisering, desto troligare är det att den som begär åtkomst verkligen är den användare som äger identiteten. Användaren kan välja ett nytt lösenord för att ersätta det gamla när autentiseringen är klar.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Krav för att konfigurera upplåsning och lösenordsåterställning för självbetjäningskonton med MFA
Det här avsnittet förutsätter att du har hämtat och slutfört distributionen av Microsoft Identity Manager 2016, inklusive följande komponenter och tjänster:

-   En Windows Server 2008 R2 eller senare som har konfigurerats som en Active Directory-server med AD-domäntjänster och domänkontrollant som har en angiven domän (en företagsdomän)

-   En grupprincip har definierats för kontoutelåsning

-   Synkroniseringstjänsten (Synk) för MIM 2016 har installerats och körs på en server som är domänansluten till AD-domänen

-   MIM 2016-tjänstens &amp; portaler, inklusive SSPR-portalen för registrering och SSPR-portalen för återställning, är installerade och körs på en server (kan samordnas genom Synk)

-   MIM-synkronisering har konfigurerats för AD-MIM-identitetssynkronisering, inklusive:

    -   Konfigurering av hanteringsagenten för Active Directory (ADMA) för anslutning till AD DS och möjlighet att importera identitetsdata från, och exportera dem till, Active Directory.

    -   Konfigurering av MIM Management Agent (MIM MA) för anslutning till FIM-tjänstdatabasen och möjlighet att importera identitetsdata från, och exportera dem till, FIM-databasen.

    -   Konfigurering av synkroniseringsregler i MIM-portalen för att tillåta synkronisering av användardata och förenkla synkroniseringsbaserade aktiviteter i MIM-tjänsten.

-   MIM 2016-tillägg och förlängningar, inklusive den klient som är integrerad med SSPR för Windows-inloggning, distribueras på servern eller på en separat klientdator.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Förbereda MIM för integrering med multifaktorautentisering
Konfigurera MIM-synkronisering för att ge stöd för funktionerna lösenordsåterställning och kontoupplåsning. Mer information finns i [Installera FIM-tillägg och förlängningar](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [ Installera FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR-autentiseringsgater](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) och [SSPR Test Lab-guide](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

I nästa avsnitt ställer du in Azure MFA-leverantören i Microsoft Azure Active Directory. Som en del av detta genererar du en fil som innehåller de autentiseringsuppgifter MFA kräver för att kunna kontakta Azure MFA.  För att kunna fortsätta behöver du ett Azure-abonnemang.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registrera din leverantör av multifaktorautentiseringstjänsten i Azure

1.  Skapa en [MFA-leverantören](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Öppna ett supportärende och begära direkt SDK för ASP.net 2.0 C#. SDK endast anges till användare av MIM med MFA eftersom direkt SDK är föråldrad. Nya kunder bör anta nästa version av MIM som kan integreras med MFA-servern.

3.  Klicka på **App Services &gt; Active Directory &gt; Leverantör av multifaktorautent. &gt; Snabbregistrering**.

![Azure portaler snabbt skapa bild av MFA](media/MIM-SSPR-Azureportal.png)

4.  I fältet **Namn** anger du **SSPRMFA** och klickar sedan på **Skapa**.

![Bild av MFA i Azure-portalen](media/MIM-SSPR-Azureportal-2.png)

5.  Klicka på **Active Directory** på den klassiska portalmenyn i Azure och klicka sedan på fliken **Leverantör av multifaktorautent.** .

6.  Klicka på **SSPRMFA** och sedan på **Hantera** längst ner på skärmen.

    ![Hanteringsikon för Azure-portalen](media/MIM-SSPR-ManageButton.png)

7.  I vänsterpanelen i det nya fönstret, under **Konfigurera** klickar du på **Inställningar**.

8.  Under **bedrägeriförsök**, avmarkera ** Blockera användare när bedrägeri rapporteras. Syftet med detta är att förhindra att hela tjänsten spärras.

9. I det **Azure Multi-Factor Authentication**-fönster som öppnas klickar du på **SDK** under **Hämtade filer** på menyn till vänster.

10. Klicka på länken **Hämta** i ZIP-kolumnen för filen med språket **SDK för ASP.net 2.0 C#**.

    ![ZIP-filsbild för hämtning i Azure MFA](media/MIM-SSPR-Azure-MFA.png)

11. Kopiera den skapade ZIP-filen till alla system där MIM-tjänsten är installerad.  Observera att ZIP-filen innehåller nyckeluppgifter som används vid autentisering via Azure MFA-tjänsten.

### <a name="update-the-configuration-file"></a>Uppdatera konfigurationsfilen

1. Logga in på datorn där MIM-tjänsten finns installerad som den användare som installerade MIM.

2. Skapa en ny katalogmapp direkt under den katalog där MIM-tjänsten installerades, t.ex. **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Använd Windows Explorer och navigera till mappen **\pf\certs** i ZIP-filen som hämtades i föregående avsnitt och kopiera filen **cert_key.p12** till den nya katalogen.

4.  Gå till mappen **\pf** i SDK-zipfilen och öppna filen **pf_auth.cs**.

5.  Leta upp de här tre parametrarna: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![pf_auth.cs code image](media/MIM-SSPR-pFile.png)

6.  I **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** öppnar du filen: **MfaSettings**.xml.

7.  Kopiera värdena från `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`-parametrarna i filen pf_aut.cs till deras respektive xml-element i filen MfaSettings.xml.

8.  Extrahera filen **cert_key.p12** under \pf\certs i SDK-zipfilen och ange den fullständiga sökvägen till den i `<CertFilePath>` xml-elementet i filen MfaSettings.xml.

9. Ange ett valfritt användarnamn i elementet `<username>`.

10. Ange din standardlandskod i elementet `<DefaultCountryCode>`. Om telefonnummer registreras för användare utan landskod kommer de tilldelas denna landskod. Om en användare har en internationell landskod ska denna inkluderas i det registrerade telefonnumret.

11. Spara filen MfaSettings.xml med samma namn och på samma plats.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Konfigurera telefonporten eller SMS-gaten för engångslösenord

1.  Starta Internet Explorer och navigera till MIM-portalen, autentisera dig som MIM-administratör och klicka sedan på **Arbetsflöden** i det vänstra navigeringsfältet.

    ![Bild av navigering i MIM-portalen](media/MIM-SSPR-workflow.jpg)

2.  Markera **Återställning av lösenord för AuthN-arbetsflöde**.

    ![Bild av Arbetsflöden i MIM-portalen](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Klicka på fliken **Aktiviteter** och bläddra till **Lägg till aktivitet**.

4.  Välj **Telefonport** eller **SMS-gate för engångslösenord**, klicka på **Välj** och sedan på **OK**.

Användare i din organisation kan nu registrera sig för att få sina lösenord återställda.  Som en del av denna process får de ange sitt telefonnummer till arbetet eller sitt mobiltelefonnummer så att systemet vet hur det ska ringa upp dem (eller skicka SMS).

#### <a name="register-users-for-password-reset"></a>Registrera användare för lösenordsåterställning

1.  En användare kommer att starta en webbläsare och navigera till MIM återställa Lösenordsregistreringsportal.  (Normalt konfigureras den här portalen med Windows-autentisering).  I portalen får de uppge sitt användarnamn och lösenord igen för att bekräfta sin identitet.

    De måste gå till portalen för lösenordsregistrering och autentiseras med sitt användarnamn och lösenord

2.  I fältet **Telefonnummer** eller **Mobiltelefon** anger de en landskod, mellanslag och sedan telefonnumret och klickar på **Nästa**.

    ![Bild av MIM telefonverifiering](media/MIM-SSPR-PhoneVerification.JPG)

    ![Bild av MIM mobiltelefonsverifiering](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Hur fungerar det för användarna?
Nu när allting har konfigurerats och körs kanske du vill se vilken procedur användarna får gå igenom om de återställer sitt lösenord strax innan en ledighet och upptäcker att de glömt sitt lösenord när de kommer tillbaka från ledigheten.

Användare kan komma åt funktionerna för lösenordsåterställning och kontoupplåsning på två sätt, antingen genom Windows inloggningssida eller via självbetjäningsportalen.

Genom att installera MIM-tillägg och -förlängningar på en domänansluten dator som är ansluten till MIM-tjänsten via din organisations nätverk, kan användare återställa ett lösenord de har glömt från platsen för inloggning till skrivbordsmiljön.  Följande steg hjälper dig genom processen.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Windows lösenordsåterställning vid inloggning till skrivbordsmiljön

1.  Om användarna anger fel lösenord flera gånger kan de klicka på **Problem att logga in?** .

    ![Bild av inloggningssidan](media/MIM-SSPR-problemsloggingin.JPG)

    Genom att klicka på den här länken kommer de vidare till sidan för MIM lösenordsåterställning där de kan ändra sitt lösenord eller låsa upp sitt konto.

    ![Bild av MIM lösenordsåterställning](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  Användaren styrs till att autentiseras. Om MFA har konfigurerats får användaren ett telefonsamtal.

3.  I bakgrunden är vad som händer att Azure MFA platser ett telefonsamtal till det nummer användaren gav när de registrerat dig för tjänsten.

4.  Användare som svarar i telefonen blir ombedda att trycka på fyrkant, #, på telefonen. Användaren klickar sedan på **Nästa** i portalen.

    Om du även konfigurerar andra portar uppmanas användaren att ange mer information på de efterföljande sidorna.

    > [!NOTE]
    > Autentiseringen misslyckas om användare råkar trycka på **Nästa** innan de har tryckt på fyrkant, #.

5.  Efter en lyckad autentisering får användaren två alternativ, antingen att låsa upp kontot och behålla det nuvarande lösenordet eller att ange ett nytt lösenord.

6.  Användaren får sedan ange sitt nya lösenord två gånger och lösenordet återställs.

#### <a name="access-from-the-self-service-portal"></a>Åtkomst från självbetjäningsportalen

1.  Användaren kan öppna en webbläsare, navigera till **portalen för återställning av lösenord**, ange sitt lösenord och klicka på **Nästa**.

    Om MFA har konfigurerats får användaren ett telefonsamtal. I bakgrunden är vad som händer att Azure MFA platser ett telefonsamtal till det nummer användaren gav när de registrerat dig för tjänsten.

    Användare som svarar i telefonen uppmanas att trycka på fyrkant, #, på telefonen. Användaren klickar sedan på **Nästa** i portalen.

2.  Om du även konfigurerar andra portar uppmanas användaren att ange mer information på de efterföljande sidorna.

    > [!NOTE]
    > Autentiseringen misslyckas om användare råkar trycka på **Nästa** innan de har tryckt på fyrkant, #.

3.  Användaren måste välja mellan att återställa sitt lösenord eller låsa upp kontot. Om han eller hon väljer att låsa upp sitt konto, blir kontot upplåst.

    ![Bild av MIM inloggningsassistent för kontoupplåsning](media/MIM-SSPR-accountUnlock.JPG)

4.  Efter en lyckad autentisering har användaren två alternativ: antingen att behålla det nuvarande lösenordet eller ange ett nytt.

5.  ![Bild av lyckad kontoupplåsning](media/MIM-SSPR-account-unlock.JPG)

6.  Om användare väljer att återställa sitt lösenord måste de skriva in sitt nya lösenord två gånger och klicka på **Nästa** för att ändra lösenordet.

    ![Bild av MIM inloggningsassistent för lösenordsåterställning](media/MIM-SSPR-PR1.JPG)
