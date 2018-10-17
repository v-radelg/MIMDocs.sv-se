---
title: Arbeta med lösenordsåterställning via självbetjäning | Microsoft Docs
description: Se vad som är nytt med självbetjäning för återställning av lösenord i MIM 2016, inklusive hur SSPR fungerar med multifaktorautentisering.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/30/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 3a86569a8de77f4cf4d5aeafe0cd01dab40232b3
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358474"
---
# <a name="self-service-password-reset-deployment-options"></a>Självbetjäning för återställning av lösenord distributionsalternativ

För nya kunder som är [licens för Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing), bör du använda [återställning av lösenord för självbetjäning i Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks.md) att tillhandahålla slutanvändarens upplevelse.  Lösenord för självbetjäning av Azure AD lösenordsåterställning innehåller både en webb- och Windows-integrerad upplevelse för en användare att återställa sina egna lösenord och stöder många av funktionerna som MIM, inklusive alternativ e-postadress och Q & A portar.  När du distribuerar Azure AD via Självbetjäning för återställning av lösenord, Azure AD Connect stöder [skriver tillbaka de nya lösenorden till AD DS](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md), och MIM [lösenordet ändra Notification Service](deploying-mim-password-change-notification-service-on-domain-controller.md) kan användas för att vidarebefordra den lösenord till andra system, t.ex en annan leverantörs katalogserver, samt.  Distribuera MIM för [lösenordshantering](infrastructure/mim2016-password-management.md) kräver inte MIM-tjänsten eller lösenord för självbetjäning MIM portaler återställning eller registrering som ska distribueras.  I stället kan du följa dessa steg:

- Första, om du vill skicka lösenord till kataloger än Azure AD och AD DS, distribuera MIM-synkronisering med anslutningsprogram till Active Directory Domain Services och eventuella ytterligare målsystem, konfigurera MIM för [lösenordshantering](infrastructure/mim2016-password-management.md) och distribuera den [lösenordet ändra meddelandetjänsten](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Sedan, om du vill skicka lösenord till kataloger än Azure AD kan konfigurera Azure AD Connect för [skriver tillbaka de nya lösenorden till AD DS](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md).
- Du kan också [föranmäl dig användare](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md).
- Slutligen [lanseras Azure AD lösenordsåterställning via Självbetjäning för dina slutanvändare](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment.md).

För befintliga kunder som hade tidigare har distribuerat Forefront Identity Manager (FIM) för återställning av lösenord nollställa och är licensierade för Azure Active Directory Premium, rekommenderar vi att planera övergången till Azure AD-självbetjäning lösenord återställa.  Du kan migrera användare till Azure AD lösenordsåterställning via självbetjäning utan att behöva dem registrera, av [eller genom att ange via PowerShell en användares alternativa e-postadress eller telefonnummer för mobila nummer](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md). När användare är registrerad för lösenord för självbetjäning i Azure AD återställa, FIM-återställningsportal av lösenord kan inaktiveras.

För kunder som inte har distribuerat Azure AD lösenordsåterställning via Självbetjäning för sina användare, dessutom MIM lösenordsåterställning via självbetjäning portaler.  MIM 2016 innehåller jämfört med FIM, följande ändringar:

- MIM självbetjäningsportalen för lösenordsåterställning portalen och Windows-inloggningsskärmen Låt användare låsa upp sina konton utan att ändra sina lösenord.
- En ny autentiseringsgate, Telefonporten, har lagts till MIM. Detta ger möjlighet till användarautentisering via ett telefonsamtal via tjänsten Microsoft Azure Multi-Factor Authentication (MFA).

MIM 2016 versionen versioner upp till version 4.5.26.0 förlita sig på kunden att ladda ned Azure Multi-Factor Authentication Software Development Kit (SDK för Azure MFA).  Den SDK är föråldrad och Azure MFA SDK kommer att ha stöd för befintliga kunder bara fram till datumet för tillbakadragandet för den 14 November 2018. Fram till detta datum kommer måste kunder kontakta Azure kundsupporten om du vill ta emot din genererade autentiseringsuppgifter MFA-paketet, eftersom de kommer inte att hämta SDK: N för Azure MFA. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>Nytt! Uppdatera aktuell Azure MFA-konfigurationen till Azure Multi-Factor Authentication Server

Detta [artikeln](working-with-mfaserver-for-mim.md) beskriver hur du uppdaterar din distribution av MIM portalen för återställning av lösenord för självbetjäning och PAM-konfigurationen med hjälp av Azure Multi-Factor Authentication-servern för multi-Factor authentication.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Distribuera MIM självbetjäningsportalen för lösenordsåterställning med hjälp av Azure MFA för Multifaktorautentisering

I följande avsnitt beskrivs hur du distribuerar MIM självbetjäning lösenord för återställningsportal, med hjälp av Azure MFA för multifaktorautentisering.  Dessa åtgärder krävs endast för kunder som inte använder Azure AD lösenordsåterställning via Självbetjäning för sina användare.

Microsoft Azure Multi-Factor Authentication är en autentiseringstjänst där användare måste bekräfta sina inloggningsförsök via en mobilapp, ett telefonsamtal eller SMS. Den är tillgänglig för användning med Microsoft Azure Active Directory och som en tjänst för molnprogram och lokala företagsprogram.

Azure MFA ger en extra autentiseringsmetod som kan förstärka befintliga autentiseringsprocesser, t.ex. den som utförs av MIM för hjälp vid inloggning via självbetjäning.

När Azure MFA används autentiseras användare i systemet för att verifiera deras identitet när de försöker att komma åt sina konton och sina resurser. Autentisering kan genomföras via SMS eller telefon.   Ju starkare autentisering, desto troligare är det att den som begär åtkomst verkligen är den användare som äger identiteten. Användaren kan välja ett nytt lösenord för att ersätta det gamla när autentiseringen är klar.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Krav för att konfigurera upplåsning och lösenordsåterställning för självbetjäningskonton med MFA

Det här avsnittet förutsätter att du har hämtat och slutfört distributionen av Microsoft Identity Manager 2016 [komponenter för MIM-synkronisering, MIM-tjänsten och MIM-portalen](microsoft-identity-manager-deploy.md), inklusive följande komponenter och tjänster:

-   En Windows Server 2008 R2 eller senare som har konfigurerats som en Active Directory-server med AD-domäntjänster och domänkontrollant som har en angiven domän (en företagsdomän)

-   En grupprincip har definierats för kontoutelåsning

-   Synkroniseringstjänsten (Synk) för MIM 2016 har installerats och körs på en server som är domänansluten till AD-domänen

-   MIM 2016-tjänstens &amp; portaler, inklusive SSPR-portalen för registrering och SSPR-portalen för återställning, är installerade och körs på en server (kan samordnas genom Synk)

-   MIM-synkronisering har konfigurerats för AD-MIM-identitetssynkronisering, inklusive:

    -   Konfigurering av hanteringsagenten för Active Directory (ADMA) för anslutning till AD DS och möjlighet att importera identitetsdata från, och exportera dem till, Active Directory.

    -   Konfigurering av MIM Management Agent (MIM MA) för anslutning till FIM-tjänstdatabasen och möjlighet att importera identitetsdata från, och exportera dem till, FIM-databasen.

    -   Konfigurering av synkroniseringsregler i MIM-portalen för att tillåta synkronisering av användardata och förenkla synkroniseringsbaserade aktiviteter i MIM-tjänsten.

-   MIM 2016-tillägg och förlängningar, inklusive den klient som är integrerad med SSPR för Windows-inloggning, distribueras på servern eller på en separat klientdator.

Det här scenariot måste du ha MIM klientåtkomstlicenser för dina användare samt prenumeration för Azure MFA.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Förbereda MIM för integrering med multifaktorautentisering
Konfigurera MIM-synkronisering för att ge stöd för funktionerna lösenordsåterställning och kontoupplåsning. Mer information finns i [Installera FIM-tillägg och förlängningar](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [ Installera FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR-autentiseringsgater](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) och [SSPR Test Lab-guide](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

I nästa avsnitt ställer du in Azure MFA-leverantören i Microsoft Azure Active Directory. Som en del av detta genererar du en fil som innehåller de autentiseringsuppgifter MFA kräver för att kunna kontakta Azure MFA.  För att kunna fortsätta behöver du ett Azure-abonnemang.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registrera din leverantör av multifaktorautentiseringstjänsten i Azure

1.  Skapa en [MFA-provider](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider.md).

2. Öppna ett supportärende och begär direkt SDK för ASP.net 2.0 C#. SDK: N ges endast till användare av MIM med MFA eftersom direkt SDK är föråldrad. Nya kunder intar nästa version av MIM som kan integreras med MFA-servern.

3. Kopiera den skapade ZIP-filen till alla system där MIM-tjänsten är installerad.  Observera att ZIP-filen innehåller nyckeluppgifter som används vid autentisering via Azure MFA-tjänsten.

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

1.  Användare öppnar en webbläsare och navigerar till portalen för registrering av återställning av MIM-lösenordet.  (Normalt konfigureras den här portalen med Windows-autentisering).  I portalen får de uppge sitt användarnamn och lösenord igen för att bekräfta sin identitet.

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

3.  Det som händer i bakgrunden är att Azure MFA ringer upp det nummer användaren angav vid registreringen för tjänsten.

4.  Användare som svarar i telefonen blir ombedda att trycka på fyrkant, #, på telefonen. Användaren klickar sedan på **Nästa** i portalen.

    Om du även konfigurerar andra portar uppmanas användaren att ange mer information på de efterföljande sidorna.

    > [!NOTE]
    > Autentiseringen misslyckas om användare råkar trycka på **Nästa** innan de har tryckt på fyrkant, #.

5.  Efter en lyckad autentisering får användaren två alternativ, antingen att låsa upp kontot och behålla det nuvarande lösenordet eller att ange ett nytt lösenord.

6.  Användaren får sedan ange sitt nya lösenord två gånger och lösenordet återställs.

#### <a name="access-from-the-self-service-portal"></a>Åtkomst från självbetjäningsportalen

1.  Användaren kan öppna en webbläsare, navigera till **portalen för återställning av lösenord**, ange sitt lösenord och klicka på **Nästa**.

    Om MFA har konfigurerats får användaren ett telefonsamtal. Det som händer i bakgrunden är att Azure MFA ringer upp det nummer användaren angav vid registreringen för tjänsten.

    Användare som svarar i telefonen uppmanas att trycka på fyrkant, #, på telefonen. Användaren klickar sedan på **Nästa** i portalen.

2.  Om du även konfigurerar andra portar uppmanas användaren att ange mer information på de efterföljande sidorna.

    > [!NOTE]
    > Autentiseringen misslyckas om användare råkar trycka på **Nästa** innan de har tryckt på fyrkant, #.

3.  Användaren måste välja mellan att återställa sitt lösenord eller låsa upp kontot. Om han eller hon väljer att låsa upp sitt konto, blir kontot upplåst.

    ![Bild av MIM inloggningsassistent för kontoupplåsning](media/MIM-SSPR-accountUnlock.JPG)

4.  Efter en lyckad autentisering har användaren två alternativ: antingen att behålla det nuvarande lösenordet eller ange ett nytt.

5.  ! [MIM ac
6.  Räkna lyckad kontoupplåsning image](media/MIM-SSPR-account-unlock.JPG)

6.  Om användare väljer att återställa sitt lösenord måste de skriva in sitt nya lösenord två gånger och klicka på **Nästa** för att ändra lösenordet.
