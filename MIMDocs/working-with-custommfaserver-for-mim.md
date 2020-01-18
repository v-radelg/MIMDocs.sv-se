---
title: Använd en annan Multi-Factor Authentication-provider via ett API för att aktivera PAM eller i SSPR-scenario | Microsoft Docs
description: Konfigurera anpassad MFA-API som ett andra säkerhets lager när användarna aktiverar roller i Privileged Access Management och använder lösen ords återställning via självbetjäning.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 9ce531fb3f6f9c831ecdb716f006f947611871e6
ms.sourcegitcommit: 1ca298d61f6020623f1936f86346b47ec5105d44
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76256605"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Använd en anpassad Multi-Factor Authentication-provider via ett API vid aktivering av PAM-rollen eller i SSPR

Kunder med Azure AD Premium eller Azure MFA kan integrera Azure MFA i två MIM-scenarier – Privileged Access Management (PAM) roll aktivering och självbetjäning för återställning av lösen ord (SSPR).

MIM-kunder har två ytterligare alternativ:

 - Använd en anpassad leverantör för eng ång slö sen ord som endast gäller i MIM SSPR-scenariot och dokumenterat i guiden för att [Konfigurera lösen ords återställning via självbetjäning med eng ång slö sen ord SMS-grind](https://docs.microsoft.com/previous-versions/mim/hh824692(v=ws.10))
 - Använd en anpassad telefonitjänstprovider för Multi-Factor Authentication. Detta gäller både MIM-SSPR och PAM-scenarier som beskrivs i den här artikeln

Den här artikeln beskriver hur du använder MIM med en anpassad Multi-Factor Authentication-provider, via ett API och en integrerings-SDK som utvecklats av kunden.  

## <a name="prerequisites"></a>Krav

För att kunna använda ett anpassat Multi-Factor Authentication-provider-API med MIM behöver du:

- telefonnummer till alla kandidatanvändare
- MIM-snabbkorrigering 4.5.202.0 eller senare – se [versions historiken](reference/version-history.md) för meddelanden
- MIM-tjänsten har kon figurer ATS för SSPR eller PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Metod användning med anpassad Multi-Factor Authentication-kod

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Steg 1: se till att MIM-tjänsten är i version 4.5.202.0 eller senare

Hämta och installera MIM Hotfix [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) eller en senare version.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Steg 2: skapa en DLL som implementerar IPhoneServiceProvider-gränssnittet

DLL-filen måste innehålla en klass som implementerar tre metoder:

- `InitiateCall`: MIM-tjänsten kommer att anropa den här metoden. Tjänsten skickar telefonnumret och begärande-ID som parametrar.  Metoden måste returnera ett `PhoneCallStatus` värde för `Pending`, `Success` eller `Failed`.
- `GetCallStatus`: om ett tidigare anrop till `initiateCall` returnerade `Pending`anropar MIM-tjänsten den här metoden. Den här metoden returnerar också `PhoneCallStatus` värde för `Pending`, `Success` eller `Failed`.
- `GetFailureMessage`: om ett tidigare anrop till `InitiateCall` eller `GetCallStatus` returnerat `Failed`, kommer MIM-tjänsten att anropa den här metoden. Den här metoden returnerar ett diagnostiskt meddelande.

Implementeringarna av dessa metoder måste vara tråd säkra, och implementeringen av `GetCallStatus` och `GetFailureMessage` får inte anta att de kommer att anropas av samma tråd som ett tidigare anrop till `InitiateCall`.

Lagra DLL-filen i `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\`-katalogen.

Exempel kod som kan kompileras med Visual Studio 2010 eller senare.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Steg 3: säkerhetskopiera MfaSettings. xml som finns i "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Steg 4: redigera filen MfaSettings. XML

Uppdatera eller ta bort följande rader:

- Ta bort/rensa alla konfigurations poster rader 

- Uppdatera eller Lägg till följande rader i följande till MfaSettings. xml med din anpassade telefon leverantör <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Steg 5: starta om MIM-tjänsten

När tjänsten har startats om använder du SSPR och/eller PAM för att validera funktioner med den anpassade identitets leverantören.

> [!NOTE] 
> Om du vill återställa inställningen ersätter du MfaSettings. xml med säkerhets kopian i steg 3


## <a name="next-steps"></a>Nästa steg

- [Komma igång med Azure Multi-Factor Authentication Server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Vad är Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Versions historik för MIM](./reference/version-history.md)
