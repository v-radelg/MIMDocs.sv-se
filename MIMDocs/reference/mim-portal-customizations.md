---
title: Microsoft Identity Manager 2016 Portal anpassningar | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016 Portal anpassning


>[!Warning]
Se till att rensa webbläsarens cacheminne när alla CSS-anpassningar görs.

I Microsoft Identity Manager 2016 (MIM), kan du anpassa valda element i lösenord portaler, inklusive banderollslogotyp, strängresurser och sammanhängande formatmallar.

Några saker som krävs för att kunna göra detta, beroende på vilken nivå av anpassning. Följande är en lista över artiklar som ingår i Anpassa registrering av lösenord i MIM 2016 och återställa portaler.

-   Anpassningar mappen – detta är den mapp som MIM 2016 kontrollerar innan du använder standardinställningarna. Varje portal som ska anpassas kräver en anpassningar mapp. Anpassningar bör endast göras i den här mappen eftersom installationsprogrammet inte skriva över den på uppgraderingar, ändra läge installerar eller reparera läge installerar.

-   STRINGS.resources - detta är en XML-fil som kan du ändra strängar som visas på portalen. Den här filen måste finnas i mappen anpassningar.

-   Style.CSS – detta är den övergripande formatmall som används av portalerna för anpassning. Den här formatmallen måste skapas och ändras för att ändra logotypen eller helt kan ersättas med dina egna anpassningar.

Stegvisa anvisningar om hur du anpassar lösenordsåterställning för registrering och lösenord finns portaler Test Lab-Guide: demonstrera MIM 2016 för registrering av lösenord och återställa anpassning av portalen.

>[! WARGNING] för MIM att identifiera eventuella anpassade ändringar, måste du starta om IIS genom att köra iisreset.


## <a name="creating-the-customizations-folder"></a>Skapa mappen anpassningar

Vid start söker MIM efter Strings.resources-filen i mappen anpassningar innan du använder standardinställningarna. Du måste skapa en mapp för anpassningar i katalogen för portalen som du vill anpassa (d.v.s. portalen för Lösenordsregistrering eller portalen för återställning av lösenord). Om du vill anpassa båda portaler måste du skapa en mapp för anpassningar under var och en av följande:

-   C:\\programfiler\\Microsoft Forefront Identity Manager\\2010\\portalen för registrering av lösenord

-   C:\\programfiler\\Microsoft Forefront Identity Manager\\2010\\portalen för återställning av lösenord

### <a name="to-create-the-customization-folder"></a>Skapa mappen anpassning

1.  Navigera till C:\\programfiler\\Microsoft Forefront Identity Manager\\2010\\Lösenordsregistreringsportal-mappen.

2.  Skapa en mapp med namnet anpassningar.

3.  Gå tillbaka en nivå till mappen portalen för återställning av lösenord och skapa en mapp med namnet anpassningar.

## <a name="customizing-strings"></a>Anpassa strängar

Många av strängar i portalens användargränssnitt kan anpassas genom att skapa en Strings.resources-fil och lägger till den här filen i mappen anpassningar. Du behöver skapa en Strings.resources-fil för varje portal som du vill anpassa.

### <a name="to-customize-strings"></a>Anpassa strängar

1.  Använd anteckningar, kopiera följande kod under till den och spara den till mappen anpassningar som Strings.resources

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  Under den `<!-- Customizations begin here -->` avsnittet Ändra namnet på data så att den matchar strängar som du vill anpassa och ange ett värde för denna sträng mellan den <value> </value> taggar. I listan nedan för strängar som kan anpassas och standardvärdena.

>[!NOTE]
Den **Strings.resources** filen är språkneutrala. För att skapa språk specifika anpassade strängar, måste du har det språkpaket installerat och spara filen i formatet strängar. `<language>-<culture>.resources`, till exempel Strings.en us.resources.

Följande är en lista med portalen strängar som kan anpassas.

<a name="portal-strings"></a>Portalen strängar
--------------

| Namn                                                     | Standardvärde                                                                                                                                                                                                                                                                                                                                         | Kommentar                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Om         |       |
| ButtonCancel                                             | Avbryt                                                                                 |     |
| ButtonFinish                                             | Slutför    |    |
| ButtonNext                                               | Nästa    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | Sessionen är inte längre aktiv. Du kan stänga fönstret eller kan du starta om genom att klicka på länken nedan.         |      |
| CancelFinishedTitle                                      | Sessionen avslutades                                                              |      |
| ErrorDescription_3000                                    | Ett fel har uppstått. Försök igen och kontakta hjälp supportavdelningen eller systemadministratören om problemet kvarstår. (Fel 3000)       |          |
| ErrorDescription_3001                                    | Se till att du anger ditt användarnamn på rätt sätt. Om du fortfarande inte kan återställa lösenordet kontaktar du din      |           |
|                                                          | supportavdelningen för hjälp. (Fel 3001)   |                   |
| ErrorDescription_3002                                    | Sessionen har avslutats. Gå tillbaka till startsidan för att starta igen. (Fel 3002)                                     |                |
| ErrorDescription_3003                                    | Det aktuella användarkontot kan inte identifieras av Forefront Identity Manager.Please kontakta hjälp supportavdelningen eller systemadministratören. (Fel 3003)           |               |
| ErrorDescription_3004                                    | Du har inte behörighet att registrera för återställning av lösenord. Kontakta hjälp supportavdelningen eller systemadministratören. (Fel 3004)     |              |
| ErrorDescription_3005                                    | En eller flera svar som du angav matchar inte de svar som du angav under Registration.In lösenord för att återställa ditt lösenord, svaren som du anger stämma överens med svaren som du angav när du har registrerat. Du kan starta igen från startsidan eller kontakta hjälp supportavdelningen eller systemadministratören. (Fel 3005) |           |
| ErrorDescription_3006                                    | Lösenordet som du angav är felaktigt. Du måste ange rätt lösenord för att kunna registrera för återställning av lösenord. (Fel 3006)            |              |
| ErrorDescription_3007                                    | Du får tillfälligt inte återställa lösenordet. Försök igen senare eller kontakta hjälp supportavdelningen eller systemadministratören om hjälp. (Fel 3007)     |         |
| ErrorDescription_3008                                    | Ett fel har uppstått. Försök igen och kontakta hjälp supportavdelningen eller systemadministratören om problemet kvarstår. (Fel 3008)        |          |
| ErrorDescription_3009                                    | Din inmatning innehåller text i ett format som inte är tillåtet. Försök igen med en annan inmatning eller kontakta hjälp supportavdelningen eller systemadministratören. (Fel 3009)     |             |
| ErrorDescription_3010_Registration                       | Skript är inte aktiverat i webbläsaren. Aktivera skript och gå tillbaka till startsidan för registrering av lösenord, eller kontakta supportavdelningen för hjälp.      |               |
| ErrorDescription_3010_Reset                              | Skript är inte aktiverat i webbläsaren. Aktivera skript och gå tillbaka till startsidan för återställning av lösenord, eller kontakta supportavdelningen för hjälp.     |            |
| ErrorDescription_3011                                    | Den här webbplatsen använder cookies. Konfigurera webbläsaren för att acceptera cookies och försök igen eller kontakta supportavdelningen för hjälp.          |                                           |
| ErrorDescription_3012                                    | De data som du angett matchade inte säkerhetskoden som skickats till dig. Du kan försöka att återställa lösenordet igen eller kontakta supportavdelningen för hjälp.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Det går inte att skicka en säkerhetskod. Kontakta supportavdelningen för hjälp.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Ange ditt användarnamn i korrekt format.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Ange ett användarnamn för att fortsätta.                                              |                         |
| ErrorMessagePasswordRequired                             | Ange ett lösenord.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Se till att båda lösenorden matchar.                                |           |
| ErrorPageDefaultHeading                                  | Programfel                                            |               |
| ErrorPageServerTime                                      | Servertid: {0:T}                     | {0} är den tid då undantagsfel uppstod. I t-orsakar skickades i tiden formateras som ”lång tid”. Den kommer att hamna visar timme, minut och sekund och eventuellt tid beteckning (beroende på den aktuella kulturen). |
| ErrorPageTitle                                           | Forefront Identity Management - felaktigt lösenord                     |   |
| ErrorTitle_3000                                          | Fel                                  |  |
| ErrorTitle_3001                                          | Åtkomst nekad                          |  |
| ErrorTitle_3002                                          | Sessionen avslutades                          |  |
| ErrorTitle_3003                                          | Okänd användare                      |  |
| ErrorTitle_3004                                          | En obehörig användare                      |  |
| ErrorTitle_3005                                          | Svaren stämmer inte överens                    |  |
| ErrorTitle_3006                                          | Felaktigt lösenord                     |  |  
| ErrorTitle_3007                                          | Åtkomst nekad tillfälligt              |  |
| ErrorTitle_3008                                          | Fel i kommunikationen                    |  |
| ErrorTitle_3009                                          | Otillåtna indata                       |  |
| ErrorTitle_3010                                          | Konfigurationsfel för webbläsare            |  |
| ErrorTitle_3011                                          | Konfigurationsfel för webbläsare            |  |
| ErrorTitle_3012                                          | Verifieringen misslyckades                    |  |
| ErrorTitle_3013                                          | Det gick inte att skicka säkerhetskoden   |  |
| FinalizeRegistrationHeading1                             | Om du någon gång behöver återställa ditt lösenord:        |   |
| FinalizeRegistrationSubHeading1                          | Gå till portalen för lösenordsåterställning   |   |
| FinalizeRegistrationSubHeading2                          | Verifiera din identitet   |   |
| FinalizeRegistrationSubHeading3                          | Välj ditt nya lösenord    |     |
| FinishingDescription                                     | Välj ditt nya lösenord        |    |
| FinishingTitle                                           | Återställning av lösenord:      |     |
| GotoPortalPrefix                                         | Gå till     |        |
| GotoPortalSuffix                                         | Startsida     |      |
| LabelTroubleshootingLinkText                             | Visa information           |    |
| LoadingText                                              | Läser in...     |  |
| NoScriptTagErrorMessage                                  | Skript är inte aktiverat i webbläsaren. Aktivera skript och gå tillbaka till startsidan eller kontakta supportavdelningen för hjälp.      |   |
| PasswordResetOperationGeneralErrorMessage                | Fel vid försök att återställa lösenordet.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | Lösenordet uppfyller inte organisationens lösenordsprinciper.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Fel vid återställning av lösenord, användaren kan inte ändra lösenordet.    |   |
| PrivacyStatement                                         | sekretesspolicy                                                      |    |
| RegistrationDescription                                  | Registrering av lösenord för självbetjäning     |    |
| RegistrationMission                                      | Om du glömmer lösenordet, kan du återställa den själv utan ringa supportavdelningen.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - registrering av lösenord                                          |   |
| RegistrationSteps                                        | Börja registreringen genom att klicka 'Nästa'.   |      |
| RegistrationSuccessDescription                           | Du är nu registrerad        |   |
| RegistrationSuccessTitle                                 | Slutfört:   |    |
| RegistrationWelcomeTitle                                 | Registrering av lösenord:    | |
| ResetDescription                                         | Lösenordsåterställning via självbetjäning  |    |
| ResetEnterNamePrompt                                     | Ange användarnamnet nedan     |     |
| ResetEnterPassword                                       | Ange ett nytt lösenord:                                                  |   |
| ResetExample1                                            | Contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Exempel:  |    |
| ResetPageTitle                                           | Forefront Identity Management - återställning av lösenord       |     |
| ResetReenterPassword                                     | Ange lösenordet igen:    | |
| ResetSuccessDescription                                  | Ditt lösenord har återställts    |    |
| ResetSuccessTitle                                        | Lyckades:                                |    |
| ResetUseNewPassword                                      | Du kan nu använda ditt nya lösenord för att logga in.     |      |
| ResetUsernameTextFormat                                  | (Om du återställer lösenordet för {0})       | {0} är användarens inloggning             |
| ResetWelcomeTitle                                        | Återställning av lösenord:     |      |
| TroubleshootingEmailSubject                              | Information om bearbetningsfel för FIM-begäran     |       |
| TroubleshootingLabelAttributes                           | Attribut:    |    |
| TroubleshootingLabelCloseButton                          | Stäng       |    |
| TroubleshootingLabelCopyToClipboard                      | Kopiera till Urklipp     |     |
| TroubleshootingLabelCorrelationId                        | Korrelations-Id:      |          |
| TroubleshootingLabelDetails                              | Detaljer:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | Informationen har kopierats till Urklipp.           |    |
| TroubleshootingLabelRequestId                            | Begärande-Id:                  |    |
| TroubleshootingLabelSendEmail                            | Skicka Information via e-post                            |    |
| TroubleshootingLabelSource                               | Orsak:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Visa information om begäran        |              |                                                    
| TroubleshootingLinkText                                  | Information om felsökning          |                  | |



Följande är en lista med Autentiseringsgaten strängar som kan anpassas.

<a name="authentication-gate-strings"></a>Autentisering Gate-strängar
---------------------------

| Namn                                          | Standardvärde                                                                                                                                                                                                                | Commnent |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | E-postadress:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | Fältet e-postadress får inte vara tom.     |          |
| OTPEmailRegistrationFooterReadOnly            | Följ processen som fastslagits av din organisation eller kontakta supportavdelningen om du vill uppdatera din e-postadress.                   |          |
| OTPEmailRegistrationFooterReadWrite           | E-postadressen lagras av din organisation i Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | Verifiering av e-postadress   |          |
| OTPEmailRegistrationHeaderReadOnly            | Om du någon gång behöver återställa ditt lösenord skickas en säkerhetskod för verifiering till din e-post. Om e-postadressen nedan inte är korrekt, måste du uppdatera det för att kunna använda lösenordsåterställning via självbetjäning. |          |
| OTPEmailRegistrationHeaderReadWrite           | Ange din e-postadress nedan. Om du någon gång behöver återställa ditt lösenord skickas en Verifieringskod till din e-post.                 |          |
| OTPEmailResetGateTitle                        | Verifiera din identitet: e-post         |          |
| OTPEmailResetHeader                           | Ange din säkerhetskod nedan. En säkerhetskod har skickats till den e-postadress som registrerats för den här organisationen.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | Det angivna värdet matchar inte det förväntade formatet.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | Fältet för säkerhetskod kan inte vara tomt.        |          |
| OTPResetVerificationLabel                     | Säkerhetskod:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Följ processen som fastslagits av din organisation eller kontakta supportavdelningen om du vill uppdatera ditt mobiltelefonnummer.   ||
| OTPSmsRegistrationFooterReadWrite                     | Mobiltelefonnumret lagras av din organisation i Forefront Identity Manager.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Verifiering via mobiltelefon                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | En säkerhetskod för verifiering kommer skickas till din mobiltelefon, om du någon gång behöver återställa ditt lösenord. Om mobiltelefonnumret nedan inte är korrekt behöver du uppdatera det för att kunna använda lösenordsåterställning via självbetjäning. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Ange ditt mobiltelefonnummer nedan. Om du någon gång behöver återställa ditt lösenord skickas en Verifieringskod till din mobiltelefon.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Fältet för mobiltelefonnummer kan inte vara tomt.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobiltelefon:                    |   |
| OTPSmsResetGateTitle                                  | Verifiera din identitet: mobiltelefon         |   |
| OTPSmsResetHeader                                     | Ange din säkerhetskod nedan. En säkerhetskod har skickats till mobiltelefonen som har registrerats för den här organisationen.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Ange det nuvarande lösenordet nedan och klicka på 'Nästa'.        |   |
| PasswordGateErrorMessagePasswordRequired              | Ange ditt nuvarande lösenord.                  |   |
| PasswordGateGateTitle                                 | Ditt nuvarande lösenord               |   |
| PasswordGatePasswordLabelText                         | Lösenord:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(inloggad som:`<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | Du måste svara på minst {0} frågor.        |   |
| QAGateIncorrectAnswer                                 | Dina svar är inte korrekt.       |   |
| QAGatePrivacyNotice                                   | Svaren som du anger lagras av din organisation i Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Du måste svara på minst {0} frågor att registrera.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | En eller flera svar stämmer inte med principen.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Svaret är inte kompatibel med principen.      |   |
| QAGateRegistrationTitle                               | Registrera dina svar         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | Du måste svara på {0} följande {1} frågor.   |   |
| QAGateResetTitle                                      | Verifiera din identitet: Skicka dina svar                                  |   |



## <a name="customizing-the-logo-banner"></a>Anpassa logotypen banderoll

Standardbanderoll på sidorna portalen kan anpassas för din organisation.
Anpassa logotypen banderoll:

1.  Skapa din egen sidhuvud och spara dem som PNG-filer. Filerna måste uppfylla följande rekommendationer:

 - Storlek: 490 X 50 bildpunkter.

 - Bit djup: 32

2.  Kopiera filerna till den \\anpassningar mapp i varje portal som du vill anpassa.

3.  Skapa en Style.css-filen i varje mapp. Den har peka på mappen anpassningar och den nya logotypen... Du kan ändra logotypnamn om tillämpligt (dvs. /Customizations/contosologo.png). Koden bör se ut ungefär så här:

   **Exempel 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Om du använder Internet Explorer 6.0 måste ange en annan icke-transparent logotyp och lägga till följande kod i Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Exempel 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Om du använder Internet Explorer 6.0 måste ange en annan icke-transparent logotyp och lägga till följande kod i Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Anpassa avbildning för Smartphone

Du kan anpassa avbildningen för Smartphone-enheter med hjälp av följande. Att anpassa SmartPhone-avbildningen:

1.  Skapa avbildningen och spara dem som PNG-filer. Filerna måste uppfylla följande rekommendationer:

    - Storlek: 190 X 50 bildpunkter.
    - Bit djup: 32

2.  Kopiera filerna till den \\anpassningar mapp i varje portal som du vill anpassa.

2.  Skapa en Style.css-filen i varje mapp. Den har peka på mappen anpassningar och den nya logotypen... Du kan ändra logotypnamn om tillämpligt (dvs. /Customizations/contosologo.png). Koden bör se ut ungefär så här:

  **Exempel 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Anpassa formatmallar

Du kan ändra layout och format för portalerna lösenord med hjälp av en anpassad CSS (CSS). Använda en anpassad CSS:

1.  Skapa anpassade CSS-filer och spara dem som Style.css.

2.  Kopiera filerna till den \\anpassningar mapp i varje portal som du vill anpassa.

Följande är ett grundläggande exempel på en Style.css-filen:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Du måste starta om IIS genom att köra iisreset för MIM att identifiera eventuella anpassade ändringar.                                                                                                                                                                                       

Följande är ett mer avancerat exempel på Style.css-filen. Den här filen innehåller smartphone och ipad specifik information för att visa portalerna på dessa enheter.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Vanliga problem med anpassning

Följande tabell är en lista över kända vanliga problem som kan uppstå med att uppgradera FIM-tjänsten och portalen. Den här tabellen innehåller också lösningar på dessa problem

|Problem |Lösning                                                                    |
|------|------------------------------|
| Jag har gjort en sträng anpassning men det visas inte i Användargränssnittet         | Sträng anpassningar i strings.resources kräver alltid iisreset         |
| När du ändrar strings.resources, visas inte någon av strängen ändringarna längre     | STRINGS.resources format är förmodligen felaktigt och därför ignoreras av portalen. Kontrollera händelseloggen under Windows-loggar – program- och tjänstloggar – Forefront Identity Manager                        |
| Första gången jag har lagt till Style.css, jag inte se format ändringarna i portalen            | Första gången du införa en Style.css-filen som du behöver göra ett iisreset     |
| Nya format har lagts till/har ändrats i Style.css men ändringarna visas inte i webbläsaren      | Rensa webbläsarens cache och uppdatera sidan <br/> Kontrollera CSS-syntax    |
| Innehållet i mappen CSS i path_to_sspr_portal direkt har ändrat\\css\\\*CSS eller banderollslogotyp i path_to_sspr_portal\\bilder\\fimlogo.png och förlorade ändringarna vid uppgradering | Du bör aldrig ändra dessa filer till att börja med. Använd bara mappen anpassning som ett sätt för att tillhandahålla en banderollslogotyp och CSS formatera anpassningar i Style.css. Anpassningar mappen avsiktligt inte skrivs över av större uppgraderingar <br/><br/>Försök inte att använda verktyg som ILSpy och Reflector för att ändra strängar i portalen sammansättningar. Använd strings.resources för att åsidosätta standard strängar. Sammansättningarna ersätts vid uppgradering  |
| Banderollslogotyp visas inte i portalerna / visas fortfarande FIM-logotyp     | Den bild/sökvägen i Style.css är inte giltigt eller rensades inte webbläsarens cache          |
| Banderollslogotyp verkar ugly i IE6       | Du måste ange en icke-transparent bild för IE6 och en särskild medföljande formatmall i style.css        |
