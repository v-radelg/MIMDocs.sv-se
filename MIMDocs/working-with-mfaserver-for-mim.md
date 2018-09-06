---
title: Använda SDK för Azure Multi-Factor Authentication Server för att aktivera PAM eller SSPR scenarier | Microsoft Docs
description: Ställ in Azure Multi-Factor Authentication Server SDK som ett andra säkerhetslager när dina användare aktiverar roller i Privileged Access Management och återställning för återställning av lösenord.
keywords: ''
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 7191e445688cc9e3c5c02b9c6852c869a28a937a
ms.sourcegitcommit: ad0690bd57e3d056397108bf1c8417965d69a32c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43772686"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Använda Azure Multi-Factor Authentication-servern för att aktivera PAM eller SSPR
Följande dokument beskriver hur du ställer in Azure MFA-servern som ett andra säkerhetslager när dina användare aktiverar roller i hantering av behörighet eller självbetjäningsportalen för lösenordsåterställning.

> [!IMPORTANT]
> På grund av meddelande om utfasning av Azure Multi-Factor Authentication Software Development Kit. Azure MFA SDK kommer att stödjas för befintliga kunder fram till datumet för tillbakadragandet för den 14 November 2018. Nya kunder och befintliga kunder kommer inte att kunna ladda ned SDK längre via den klassiska Azure-portalen. Om du vill hämta du behöver att nå ut till Azure-kundsupporten för att få din genererade autentiseringsuppgifter MFA-paket. <br> Utvecklingsteamet Microsoft arbetar med ändringar av MFA genom att integrera med Azure Multi-Factor Authentication Server SDK.

Artikeln nedan beskriver uppdateringen och steg för att aktivera för en enkel växel från Azure MFA SDK till Azure Multi-Factor Authentication Server SDK när den släpps som detta ska ingå i en kommande snabbkorrigeringen finns [versionshistorik ](/reference/version-history.md) efter meddelanden. 

## <a name="prerequisites"></a>Krav

För att kunna använda Azure Multi-Factor Authentication-servern med MIM behöver du:

- Internetåtkomst från varje MIM-tjänsten eller MFA-Server som tillhandahåller PAM och SSPR, för att kontakta tjänsten Azure MFA
- en Azure-prenumeration
- Installera använder redan Azure MFA SDK
- Azure Active Directory Premium-licenser för kandidatanvändare eller ett annat sätt att licensiera Azure MFA
- telefonnummer till alla kandidatanvändare
- MIM snabbkorrigering 4.5. eller större ser [versionshistorik](/reference/version-history.md) för meddelanden

## <a name="azure-multi-factor-authentication-server-configuration"></a>Azure Multi-Factor Authentication-serverkonfiguration 
> [!NOTE] 
> I konfigurationen behöver du ett giltigt SSL-certifikat som installerats för SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Steg 1: Ladda ned Azure Multi-Factor Authentication-servern från azure portal 
Logga in på den [Azure-portalen](https://portal.azure.com/) och ladda ned Azure MFA-servern.
![aktiv-med-mfaserver-för-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Steg 2: Skapa autentiseringsuppgifter för aktivering
Använd den **skapa autentiseringsuppgifter för aktivering för att initiera användning** länk för att skapa autentiseringsuppgifter för aktivering. När åtkomstprinciepn har skapats Spara för senare användning.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Steg 3: Installera Azure Multi-Factor Authentication Server
När du har laddat ned servern, [installera](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) den.  Autentiseringsuppgifter för aktivering måste utföras. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Steg 4: Skapa din IIS-webbprogram som är värd för SDK
1. Öppna IIS-hanteraren ![fungerande-med-mfaserver-för-mim_iis. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Skapa nytt anrop för webbplatsen ”MIM MFASDK”, länka den till en tom katalog ![fungerande-med-mfaserver-för-mim_sdkweb. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Öppna Multi-Factor Authentication-konsolen och klicka på webbtjänst-SDK ![fungerande-med-mfaserver-för-mim_sdkinstall. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. En gång guider är klicka igenom konfig, Välj ”MIM MFASDK” och programpoolen

> [!NOTE] 
> Guiden kräver en administratörsgrupp som ska skapas. Mer information finns på Azure >> MFA Azure Multi-Factor Authentication Server-dokumentationen.

5. Därefter måste vi importera MIM-tjänstens konto öppna Multi-Factor Authentication-konsolen väljer ”användare” en. Klicka på ”Importera från Active Directory” b. Gå till tjänstkontot kallas även ”contoso\mimservice” c. Klicka på ”Importera” och ”Stäng” ![fungerande-med-mfaserver-för-mim_importmimserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Redigera konto för MIM-tjänsten att aktivera i Multi-Factor Authentication-konsolen ![fungerande-med-mfaserver-för-mim_enableserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Uppdatera IIS-autentisering på webbplatsen ”MIM MFASDK”. Först inaktiverar vi ”anonym autentisering” och aktivera Windows-autentisering ” ![fungerande-med-mfaserver-för-mim_iisconfig. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Sista steget: Lägga till MIM-tjänstkontot i ”PhoneFactor Admins” ![fungerande-med-mfaserver-för-mim_addservicetomfaadmin. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Konfigurera MIM-tjänsten för Azure Multi-Factor Authentication Server 

### <a name="step-1-patch-server-to-452020"></a>Steg 1: Patch-servern till 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Steg 2: Säkerhetskopiering och öppna MfaSettings.xml finns i ”C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”

### <a name="step-3-update-the-following-lines"></a>Steg 3: Uppdatera följande rader
1. Ta bort/rensa följande konfiguration poster rader <br>
&LT; LICENSNYCKEL &GT;&LT; / LICENSNYCKEL &GT;<br>
&LT; GROUP_KEY &GT;&LT; / GROUP_KEY &GT;<br>
&LT; CERT_PASSWORD &GT;&LT; / CERT_PASSWORD &GT;<br>
<CertFilePath></CertFilePath><br>

2. Uppdatera eller lägga till följande rader till följande för att MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Starta om MIM-tjänsten och testa funktionen med Azure Multi-Factor Authentication-servern.

> [!NOTE] 
> Om du vill återställa inställningen Ersätt MfaSettings.xml med säkerhetskopian i steg 2


## <a name="next-steps"></a>Nästa steg

-    [Komma igång med Azure Multi-Factor Authentication Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Vad är Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Använd anpassad Multi-Factor Authentication API för att aktivera PAM eller SSPR](Working-with-custommfaserver-for-mim.md)
- [MIM-versionshistorik](./reference/version-history.md)