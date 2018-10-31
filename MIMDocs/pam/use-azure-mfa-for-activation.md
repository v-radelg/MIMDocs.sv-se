---
title: Använd Azure MFA för att aktivera PAM | Microsoft Docs
description: Ställ in Azure MFA som ett andra säkerhetslager när dina användare aktiverar roller i Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 9cb1e37f966db5c663694aaccd71f2b4c799dd4b
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379947"
---
# <a name="using-azure-mfa-for-activation"></a>Aktivera med hjälp av Azure MFA
> [!IMPORTANT]
> På grund av meddelande om utfasning av Azure Multi-Factor Authentication Software Development Kit. Azure MFA SDK kommer att stödjas för befintliga kunder fram till datumet för tillbakadragandet för den 14 November 2018. Nya kunder och befintliga kunder kommer inte att kunna ladda ned SDK längre via den klassiska Azure-portalen. Om du vill hämta du behöver att nå ut till Azure-kundsupporten för att få din genererade autentiseringsuppgifter MFA-paket. <br> Utvecklingsteamet Microsoft arbetar med ändringar av MFA genom att integrera med MFA Server SDK.  Detta kommer att inkluderas i en kommande snabbkorrigeringen finns [versionshistorik](/reference/version-history.md) efter meddelanden. 


När du konfigurerar en PAM-roll kan du välja hur du ger behörighet till användare som ber att få aktivera rollen. De alternativ som implementeras med PAM-auktoriseringsuppgiften är:

- Godkännande av rollägare
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Om ingen kontroll är aktiverad aktiveras kandidatanvändare automatiskt för sina roller.

Microsoft Azure Multi-Factor Authentication (MFA) är en autentiseringstjänst med vilken användare måste bekräfta sina inloggningsförsök via mobilapp, telefonsamtal eller SMS. Den kan användas med Microsoft Azure Active Directory och som en tjänst för molnbaserade och lokala företagsprogram. Azure MFA ger en extra autentiseringsmetod för PAM-scenariot. Azure MFA kan användas för auktorisering, oavsett hur en användare autentiseras till Windows PRIV-domänen.

## <a name="prerequisites"></a>Krav

För att kunna använda Azure MFA med MIM behöver du:

- internetåtkomst från varje MIM-tjänst som tillhandahåller PAM för att kunna kontakta tjänsten Azure MFA
- en Azure-prenumeration
- Azure Active Directory Premium-licenser för kandidatanvändare eller ett annat sätt att licensiera Azure MFA
- telefonnummer till alla kandidatanvändare

## <a name="creating-an-azure-mfa-provider"></a>Skapa en Azure MFA-leverantör

I det här avsnittet ska ställa du in Azure MFA-leverantören i Microsoft Azure Active Directory.  Om du använder redan Azure MFA, antingen fristående eller konfigurerat med Azure Active Directory Premium, kan du gå vidare till nästa avsnitt.

1.  Öppna en webbläsare och anslut till den [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör för ett Azure-abonnemang.

2.  Klicka på **Ny** längst ned till vänster.

3.  Klicka på **App Services &gt; Active Directory &gt; Leverantör av multifaktorautent. &gt; Snabbregistrering**.

4.  I fältet **Namn** skriver du **PAM**, och i fältet Användningsmodell väljer du Per aktiverad användare. Om du redan har en Azure AD-katalog väljer du den katalogen. Klicka på **Skapa**.

## <a name="downloading-the-azure-mfa-service-credentials"></a>Ladda ned autentiseringsuppgifter för Azure MFA-tjänsten

Nu genererar du en fil som innehåller de autentiseringsuppgifter som krävs för att PAM ska kunna kontakta Azure MFA.

1. Öppna en webbläsare och anslut till den [klassiska Azure-portalen](https://manage.windowsazure.com) som administratör för ett Azure-abonnemang.

2.  Klicka på **Active Directory** på menyn Azure Portal och klicka sedan på fliken **Leverantör av multifaktorautent**.

3.  Klicka på Azure MFA-leverantören du ska använda för PAM och klicka på **Hantera**.

4.  I vänsterpanelen i det nya fönstret, under **Konfigurera** klickar du på **Inställningar**.

5.  I fönstret **Azure Multi-Factor Authentication** klickar du på **SDK** under **Hämtade filer**.

6.  Klicka på länken **Hämta** i ZIP-kolumnen för filen med språket **SDK för ASP.net 2.0 C\#**.

![Ladda ned SDK:t för Multi-Factor Authentication – skärmbild](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Kopiera den skapade ZIP-filen till alla system där MIM-tjänsten är installerad. 

>[!NOTE]
> ZIP-filen innehåller nyckeluppgifter som används vid autentisering via Azure MFA-tjänsten.

## <a name="configuring-the-mim-service-for-azure-mfa"></a>Konfigurera MIM-tjänsten för Azure MFA

1.  Logga in på datorn där MIM-tjänsten är installerad som administratör eller användaren som installerade MIM.

2.  Skapa en ny katalogmapp i katalogen där MIM-tjänsten är installeras, exempelvis ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Windows Explorer, gå till den ```pf\certs``` mappen i ZIP-filen som hämtades i föregående avsnitt. Kopiera filen ```cert\_key.p12``` till den nya katalogen.

4.  Windows Explorer, gå till den ```pf``` mappen ZIP och öppna filen ```pf\_auth.cs``` i en textredigerare, till exempel Wordpad.

5. Lokalisera dessa tre parametrar: ```LICENSE\_KEY```, ```GROUP\_KEY```, ```CERT\_PASSWORD```.

![Kopiera värden från filen pf\_auth.cs – skärmbild](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Använd Anteckningar och öppna **MfaSettings.xml** som finns i ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Kopiera värdena från parametrarna LICENSE\_KEY, GROUP\_KEY och CERT\_PASSWORD i filen pf\_auth.cs till deras respektive xml-element i filen MfaSettings.xml.

8. I XML-elementet **<CertFilePath>** anger du den fullständiga sökvägen till filen cert\_key.p12 du extraherade tidigare.

9. Ange ett valfritt användarnamn i elementet **<username>**.

10. I elementet **<DefaultCountryCode>** ange du landskoden till dina användare, till exempel 1 för USA och Kanada. Det här värdet används när användare är registrerade med telefonnummer utan landskod. Om en användares telefonnummer har en annan internationell landskod än den som har konfigurerats för organisationen, måste den landskoden inkluderas i det telefonnumret som registreras.

11. Spara och skriv över filen **MfaSettings.xml** i MIM-tjänstmappen ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> I slutet av processen kontrollerar du att filen **MfaSettings.xml**, eventuella kopior av den och ZIP-filen inte kan läsas offentligt.

## <a name="configure-pam-users-for-azure-mfa"></a>Konfigurera PAM-användare för Azure MFA

En användarens telefonnummer måste lagras i MIM om användaren ska kunna aktivera en roll som kräver Azure MFA. Du kan ange det här attributet på två sätt.

Med kommandot `New-PAMUser` kopieras först ett telefonnummerattribut från användarens katalogpost i CORP-domänen till MIM-tjänstens databas. Den här åtgärden behöver bara utföras en gång.

Sedan uppdateras telefonnummerattributet i MIM-tjänstens databas med kommandot `Set-PAMUser`. Följande ersätter till exempel en befintlig PAM-användarens telefonnummer i MIM-tjänsten. Användarens katalogpost påverkas inte.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Konfigurera PAM-roller för Azure MFA

När alla telefonnummer till kandidatanvändare för en PAM-roll finns lagrade i MIM-tjänstens databas kan rollen konfigureras att kräva MFA Azure. Det här gör du med kommandot `New-PAMRole` eller `Set-PAMRole`. Exempel:

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Azure MFA kan inaktiveras för en roll genom att parametern "-MFAEnabled 0" anges i kommandot `Set-PAMRole`.

## <a name="troubleshooting"></a>Felsökning

Följande händelser kan du hitta i händelseloggen för Privileged Access Management:

| ID  | Allvarlighetsgrad | Genererat av | Beskrivning |
|-----|----------|--------------|-------------|
| 101 | Fel       | MIM-tjänst            | Användaren slutförde inte Azure MFA (t.ex. svarade inte i telefon) |
| 103 | Information | MIM-tjänst            | Användaren slutförde Azure MFA vid aktivering                       |
| 825 | Varning     | PAM-övervakningstjänst | Telefonnumret har ändrats                                |

Du kan också visa eller ladda ned en rapport från Azure MFA om du vill veta mer om misslyckade telefonsamtal (händelse nummer 101).

1.  Öppna en webbläsare och anslut till den [klassiska Azure-portalen](https://manage.windowsazure.com) som global administratör för Azure AD.

2.  Välj **Active Directory** på menyn Azure Portal och klicka sedan på fliken **Leverantör av multifaktorautent**.

3.  Välj Azure MFA-leverantören du använder för PAM och klicka på **Hantera**.

4.  I det nya fönstret klickar du på **Användning** och sedan på **Användarinformation**.

5.  Ange tidsintervall och markera kryssrutan bredvid **Namn** i den extra rapportkolumnen. Klicka på **Exportera till CSV**.

6.  När rapporten har skapats kan du visa den i portalen. Om MFA-rapporten är omfattande kan du ladda ned den som en CSV-fil. **SDK**-värdena i kolumnen **AUTENTISERINGSTYP** visar rader som är relevanta som PAM-aktiveringsbegäranden: de är händelser från MIM och annan lokal programvara. Fältet **ANVÄNDARNAMN** är GUID för användarobjektet i MIM-tjänstens databas. Om ett samtal misslyckades är värdet i kolumnen **AUTHD** **Nej** och värdet i kolumnen **SAMTALSRESULTAT** innehåller information om felorsaken.

## <a name="next-steps"></a>Nästa steg

- [Vad är Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Skapa ditt kostnadsfria Azure-konto i dag](https://azure.microsoft.com/free/)
