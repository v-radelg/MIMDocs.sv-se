---
title: Använd Azure Multi-Factor Authentication-server för att aktivera PAM-eller SSPR-scenarier | Microsoft Docs
description: Konfigurera Azure Multi-Factor Authentication-server som ett andra säkerhets lager när användarna aktiverar roller i Privileged Access Management och lösen ords återställning via självbetjäning.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 39ebec3002f488077cfda28a5780b0c78c19f363
ms.sourcegitcommit: 28a20aaa1f08b428cc1ae0eae43ae47de4d9d22a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684099"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Använd Azure Multi-Factor Authentication-server för att aktivera PAM eller SSPR
I följande dokument beskrivs hur du konfigurerar Azure MFA-servern som ett andra säkerhets lager när användarna aktiverar roller i Privileged Access Management eller lösen ords återställning via självbetjäning.

> [!IMPORTANT]
> På grund av utfasningen av Azure Multi-Factor Authentication Software Development Kit, kommer Azure MFA SDK att stödjas för befintliga kunder fram till avtiden den 14 november 2018. Nya kunder och aktuella kunder kommer inte att kunna hämta SDK längre via den klassiska Azure-portalen. För att du ska kunna ladda ned måste du kontakta Azures kund support för att ta emot ditt genererade autentiseringsuppgifter för MFA-tjänsten.

Artikeln nedan beskriver konfigurations uppdateringen och stegen för att aktivera flytt från Azure MFA SDK till Azure Multi-Factor Authentication-server.

## <a name="prerequisites"></a>Krav

För att kunna använda Azure Multi-Factor Authentication-server med MIM behöver du:

- Internet åtkomst från varje MIM-tjänst eller MFA-server som tillhandahåller PAM-och SSPR för att kontakta Azure MFA-tjänsten
- en Azure-prenumeration
- Installationen använder redan Azure MFA SDK
- Azure Active Directory Premium-licenser för kandidatanvändare eller ett annat sätt att licensiera Azure MFA
- telefonnummer till alla kandidatanvändare
- MIM Hotfix 4,5. eller mer information finns i [versions historiken](./reference/version-history.md) för meddelanden

## <a name="azure-multi-factor-authentication-server-configuration"></a>Konfiguration av Azure-Multi-Factor Authentication-server 
> [!NOTE] 
> I konfigurationen behöver du ett giltigt SSL-certifikat som är installerat för SDK: n. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Steg 1: Ladda ned Azure Multi-Factor Authentication-server från Azure Portal 
Logga in på [Azure Portal](https://portal.azure.com/) och ladda ned Azure MFA-servern.
![work-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Steg 2: generera autentiseringsuppgifter för aktivering
Använd länken **generera autentiseringsuppgifter för att initiera användnings** länken för att generera autentiseringsuppgifter för aktivering. När du har genererat Spara för senare användning.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Steg 3: installera Azure-Multi-Factor Authentication-server
När du har hämtat servern [installerar](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) du den.  Dina autentiseringsuppgifter för aktivering krävs. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Steg 4: skapa IIS-webbappen som ska vara värd för SDK: n
1. Öppna IIS-hanteraren ![work-with-mfaserver-for-mim_iis. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Skapa nytt webbplats anrop "MIM MFASDK", länka det till en tom katalog ![work-with-mfaserver-mim_sdkweb. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Öppna Multi-Factor Authentication-konsolen och klicka på Web Service SDK ![work-with-mfaserver-for-mim_sdkinstall. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. När guiden är klar klickar du på Konfigurera, väljer "MIM MFASDK" och app pool

> [!NOTE] 
> En administratörs grupp måste skapas för att guiden ska kunna skapas. Mer information finns på Azure-> > MFA Azure Multi-Factor Authentication-server-dokumentation.

5. Härnäst måste vi importera MIM-tjänstkontot öppna Multi-Factor Authentication-konsolen Välj "användare" a. Klicka på "Importera från Active Directory" b. Gå till tjänst kontot aka "contoso\mimservice" c. Klicka på Importera och Stäng ![work-with-mfaserver-for-mim_importmimserviceaccount. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Redigera MIM-tjänstkontot för att aktivera i Multi-Factor Authentication-konsolen ![work-with-mfaserver-for-mim_enableserviceaccount. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Uppdatera IIS-autentiseringen på webbplatsen "MIM-MFASDK". Först ska vi inaktivera "Anonym autentisering" och sedan aktivera Windows-autentisering "![work-with-mfaserver-for-mim_iisconfig. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Sista steget: Lägg till MIM-tjänstekontot i "PhoneFactor admins" ![work-with-mfaserver-to-mim_addservicetomfaadmin. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Konfigurera MIM-tjänsten för Azure Multi-Factor Authentication-server 

### <a name="step-1-patch-server-to-452020"></a>Steg 1: korrigerings server att 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Steg 2: säkerhetskopiera och öppna MfaSettings. xml som finns i C:\Program Files\Microsoft Forefront Identity Manager\2010\Service

### <a name="step-3-update-the-following-lines"></a>Steg 3: uppdatera följande rader
1. Ta bort/rensa följande rader i konfigurations poster <br>
< LICENSE_KEY > </LICENSE_KEY ><br>
< GROUP_KEY > </GROUP_KEY ><br>
< CERT_PASSWORD > </CERT_PASSWORD ><br>
<CertFilePath></CertFilePath><br>

2. Uppdatera eller Lägg till följande rader i följande till MfaSettings. XML <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Starta om MIM-tjänsten och testa funktionen med Azure Multi-Factor Authentication-server.

> [!NOTE] 
> Om du vill återställa inställningen ersätter du MfaSettings. xml med säkerhets kopian i steg 2


## <a name="see-also"></a>Se även

-    [Komma igång med Azure Multi-Factor Authentication Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Vad är Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Använd anpassad Multi-Factor Authentication-API för att aktivera PAM eller SSPR](Working-with-custommfaserver-for-mim.md)
- [Versions historik för MIM](./reference/version-history.md)
