---
title: Arbeta med självbetjäning för återställning av lösen ord | Microsoft Docs
description: Se vad som är nytt med självbetjäning för återställning av lösenord i MIM 2016, inklusive hur SSPR fungerar med multifaktorautentisering.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 41aba931111d6ef46e60dfed173362e59c411dfe
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044284"
---
# <a name="self-service-password-reset-deployment-options"></a>Distributions alternativ för lösen ords återställning via självbetjäning

För nya kunder som är [licensierade för Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing)rekommenderar vi att du använder [lösen ords återställning via självbetjäning i Azure AD](/azure/active-directory/authentication/concept-sspr-howitworks) för att ge slutanvändarens upplevelse.  Återställning av lösen ord för självbetjäning i Azure AD ger både en webbaserad och Windows-integrerad upplevelse för en användare att återställa sina egna lösen ord och har stöd för många av samma funktioner som MIM, inklusive alternativa e-postmeddelanden och frågor&portar.  När du distribuerar lösen ords återställning via självbetjäning i Azure AD, Azure AD Connect har stöd för att [skriva tillbaka de nya lösen orden till AD DS](/azure/active-directory/authentication/concept-sspr-writeback)och MIM- [tjänsten för meddelanden om lösen ords ändring](deploying-mim-password-change-notification-service-on-domain-controller.md) kan användas för att vidarebefordra lösen orden till andra system, till exempel en annan leverantörs katalog server.  Distribution av MIM för [lösen ords hantering](infrastructure/mim2016-password-management.md) kräver inte MIM-tjänsten eller MIM självbetjäning för återställning av lösen ord eller registrerings portaler som ska distribueras.  I stället kan du följa dessa steg:

- Om du först behöver skicka lösen ord till andra kataloger än Azure AD och AD DS distribuerar du MIM Sync med kopplingar till Active Directory Domain Services och eventuella ytterligare mål system, konfigurerar MIM för [lösen ords hantering](infrastructure/mim2016-password-management.md) och distribuerar [tjänsten för meddelanden om lösen ords ändring](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Om du sedan behöver skicka lösen ord till andra kataloger än Azure AD konfigurerar du Azure AD Connect för att [skriva tillbaka de nya lösen orden till AD DS](/azure/active-directory/authentication/concept-sspr-writeback).
- Du kan också, [för att registrera användare](/azure/active-directory/authentication/howto-sspr-authenticationdata).
- Slutligen kan du [distribuera lösen ords återställning via självbetjäning i Azure AD till dina slutanvändare](/azure/active-directory/authentication/howto-sspr-deployment).

För befintliga kunder som tidigare har distribuerat Forefront Identity Manager (FIM) för lösen ords återställning via självbetjäning och som är licensierade för Azure Active Directory Premium rekommenderar vi att du planerar över gången till återställning av lösen ord för Azure AD-självbetjäning.  Du kan överföra slutanvändare till lösen ords återställning via självbetjäning i Azure AD utan att behöva registrera dem på nytt, genom att [Synkronisera eller ställa in via PowerShell en användares alternativa e-postadress eller mobiltelefon nummer](/azure/active-directory/authentication/howto-sspr-authenticationdata). När användare har registrerats för lösen ords återställning via självbetjäning i Azure AD kan du inaktivera portalen för att återställa lösen ord.

För kunder som inte har distribuerat återställning av lösen ord för Azure AD via självbetjäning för sina användare, innehåller MIM även portaler för lösen ords återställning via självbetjäning.  Jämfört med FIM innehåller MIM 2016 följande ändringar:

- Den MIM självbetjänings portalen för lösen ords återställning och Windows inloggnings skärm låter användarna låsa upp sina konton utan att ändra sina lösen ord.
- En ny autentiseringsprovider, telefon porten, har lagts till i MIM. Detta gör det möjligt att autentisera användare via telefonsamtal via Microsoft Azure Multi-Factor Authentication-tjänsten (MFA).

MIM 2016-versionen bygger upp till version 4.5.26.0 som förlitar sig på kunden för att ladda ned Azure Multi-Factor Authentication Software Development Kit (Azure MFA SDK).  SDK: n är inaktuell och Azure MFA SDK kommer att stödjas för befintliga kunder fram till indragnings datumet den 14 november 2018. Fram till det datumet måste kunderna kontakta Azure-kundsupporten för att ta emot ditt genererade paket med autentiseringsuppgifter för MFA-tjänsten eftersom de inte kan ladda ned Azure MFA SDK. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>Nytt! Uppdatera den aktuella Azure MFA-konfigurationen till Azure Multi-Factor Authentication-server

Den här [artikeln](working-with-mfaserver-for-mim.md) beskriver hur du uppdaterar portalen för återställning av lösen ord för självbetjäning och pam-konfiguration med hjälp av Azure Multi-Factor Authentication-Server för Multi-Factor Authentication.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Distribuera MIM självbetjänings portalen för lösen ords återställning med Azure MFA för Multi-Factor Authentication

I följande avsnitt beskrivs hur du distribuerar MIM självbetjänings portalen för återställning av lösen ord med hjälp av Azure MFA för Multi-Factor Authentication.  De här stegen är bara nödvändiga för kunder som inte använder Azure AD självbetjänings återställning av lösen ord för sina användare.

Microsoft Azure Multi-Factor Authentication är en autentiseringstjänst där användare måste bekräfta sina inloggningsförsök via en mobilapp, ett telefonsamtal eller SMS. Den är tillgänglig för användning med Microsoft Azure Active Directory och som en tjänst för molnprogram och lokala företagsprogram.

Azure MFA ger en extra autentiseringsmetod som kan förstärka befintliga autentiseringsprocesser, t.ex. den som utförs av MIM för hjälp vid inloggning via självbetjäning.

När Azure MFA används autentiseras användare i systemet för att verifiera deras identitet när de försöker att komma åt sina konton och sina resurser. Autentisering kan genomföras via SMS eller telefon.   Ju starkare autentisering, desto troligare är det att den som begär åtkomst verkligen är den användare som äger identiteten. Användaren kan välja ett nytt lösenord för att ersätta det gamla när autentiseringen är klar.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Krav för att konfigurera upplåsning och lösenordsåterställning för självbetjäningskonton med MFA

Det här avsnittet förutsätter att du har laddat ned och slutfört distributionen av Microsoft Identity Manager 2016 [MIM-synkronisering, MIM-tjänsten och MIM-portalens komponenter](microsoft-identity-manager-deploy.md), inklusive följande komponenter och tjänster:

-   En Windows Server 2008 R2 eller senare som har konfigurerats som en Active Directory-server med AD-domäntjänster och domänkontrollant som har en angiven domän (en företagsdomän)

-   En grupprincip har definierats för kontoutelåsning

-   Synkroniseringstjänsten (Synk) för MIM 2016 har installerats och körs på en server som är domänansluten till AD-domänen

-   MIM 2016-tjänstens &amp; portaler, inklusive SSPR-portalen för registrering och SSPR-portalen för återställning, är installerade och körs på en server (kan samordnas genom Synk)

-   MIM-synkronisering har konfigurerats för AD-MIM-identitetssynkronisering, inklusive:

    -   Konfigurering av hanteringsagenten för Active Directory (ADMA) för anslutning till AD DS och möjlighet att importera identitetsdata från, och exportera dem till, Active Directory.

    -   Konfigurering av MIM Management Agent (MIM MA) för anslutning till FIM-tjänstdatabasen och möjlighet att importera identitetsdata från, och exportera dem till, FIM-databasen.

    -   Konfigurering av synkroniseringsregler i MIM-portalen för att tillåta synkronisering av användardata och förenkla synkroniseringsbaserade aktiviteter i MIM-tjänsten.

-   MIM 2016-tillägg och förlängningar, inklusive den klient som är integrerad med SSPR för Windows-inloggning, distribueras på servern eller på en separat klientdator.

I det här scenariot måste du ha MIM-CALs för dina användare och prenumeration för Azure MFA.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Förbereda MIM för integrering med multifaktorautentisering
Konfigurera MIM-synkronisering för att ge stöd för funktionerna lösenordsåterställning och kontoupplåsning. Mer information finns i [Installera FIM-tillägg och förlängningar](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [ Installera FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR-autentiseringsgater](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) och [SSPR Test Lab-guide](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

I nästa avsnitt ställer du in Azure MFA-leverantören i Microsoft Azure Active Directory. Som en del av detta genererar du en fil som innehåller de autentiseringsuppgifter MFA kräver för att kunna kontakta Azure MFA.  För att kunna fortsätta behöver du ett Azure-abonnemang.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registrera din leverantör av multifaktorautentiseringstjänsten i Azure

1.  Skapa en [MFA-Provider](/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Öppna ett support ärende och begär Direct SDK för ASP.net 2,0 C#. SDK kommer endast att tillhandahållas aktuella användare av MIM med MFA eftersom direkt-SDK: n är föråldrad. Nya kunder bör anta nästa version av MIM som ska integreras med MFA Server.

3. Kopiera den skapade ZIP-filen till alla system där MIM-tjänsten är installerad.  Observera att ZIP-filen innehåller nyckeluppgifter som används vid autentisering via Azure MFA-tjänsten.

### <a name="update-the-configuration-file"></a>Uppdatera konfigurationsfilen

1. Logga in på datorn där MIM-tjänsten finns installerad som den användare som installerade MIM.

2. Skapa en ny katalogmapp direkt under den katalog där MIM-tjänsten installerades, t.ex. **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Använd Utforskaren i Windows för att navigera till mappen **\pf\certs** i zip-filen som hämtades i föregående avsnitt och kopiera filen **cert_key. p12** till den nya katalogen.

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

Obs! Om du använder Azure MFA Server eller en annan provider som genererar eng ång slö sen ord, se till att fältet längd som har kon figurer ATS ovan har samma längd som det som genereras av MFA-providern.  Längden måste vara 6 för Azure MFA Server.  Azure MFA Server genererar också sin egen meddelande text så att SMS-textmeddelandet ignoreras.

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

5.  ! [MIM AC
6.  räkna upplåst lyckad avbildning] (Media/MIM-SSPR-account-unlock. JPG)

6.  Om användare väljer att återställa sitt lösenord måste de skriva in sitt nya lösenord två gånger och klicka på **Nästa** för att ändra lösenordet.
