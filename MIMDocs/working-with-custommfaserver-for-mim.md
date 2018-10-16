---
title: Använda en alternativ Multi-Factor Authentication-provider via ett API för att aktivera PAM eller i SSPR scenario | Microsoft Docs
description: Ställ in anpassat API för MFA som ett andra säkerhetslager när dina användare aktiverar roller i Privileged Access Management och använda lösenord självåterställning.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.openlocfilehash: c878fa85ae2c93e77a34f497c4f770e30517462f
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333419"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Använda en anpassad Multi-Factor Authentication-provider via ett API under aktiveringen för PAM-rollen eller i SSPR

Azure AD Premium eller Azure MFA-kunder kan du integrera Azure MFA i två MIM-scenarier – Privileged Access Management (PAM) rollaktivering och Self-Service lösenord återställer (SSPR).

MIM-kunder har ytterligare två alternativ:

 - Använda en anpassad en gång lösenord delivery provider som gäller endast i MIM SSPR-scenariot och är dokumenterade i handboken för att [konfigurera självbetjäningsportalen för lösenordsåterställning med SMS-Gate för Engångslösenord](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10))
 - Använd en anpassad telefoni flerfunktionsautentiseringsleverantör. Detta gäller i både MIM SSPR och PAM-scenarier, som beskrivs i den här artikeln

Den här artikeln beskrivs hur du använder MIM med en anpassad Multi factor authentication-provider, via ett API och en integration SDK som har utvecklats av kunden.  

## <a name="prerequisites"></a>Krav

Om du vill använda en anpassad Multi-Factor Authentication-provider API med MIM behöver du:

- telefonnummer till alla kandidatanvändare
- MIM-snabbkorrigeringen [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) eller senare – i [versionshistorik](/reference/version-history.md) för meddelanden
- MIM-tjänsten som konfigurerats för SSPR eller PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Metoden med hjälp av anpassade multifaktorautentisering kod

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Steg 1: Kontrollera att MIM-tjänsten är version 4.5.202.0 eller senare

Ladda ned och installera MIM-snabbkorrigeringen [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) eller en senare version.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Steg 2: Skapa en DLL-fil som implementerar gränssnittet IPhoneServiceProvider

DLL-filen måste innehålla en klass som implementerar tre metoder:

- `InitiateCall`: MIM-tjänsten ska anropa den här metoden. Tjänsten skickar phone tal och begäran-ID som parametrar.  Metoden måste returnera ett `PhoneCallStatus` värdet för `Pending`, `Success` eller `Failed`.
- `GetCallStatus`: Om ett tidigare anrop till `initiateCall` returnerade `Pending`, MIM-tjänsten anropar den här metoden. Den här metoden returnerar också `PhoneCallStatus` värdet för `Pending`, `Success` eller `Failed`.
- `GetFailureMessage`: Om ett tidigare anrop av `InitiateCall` eller `GetCallStatus` returnerade `Failed`, MIM-tjänsten anropar den här metoden. Den här metoden returnerar en diagnostikmeddelande.

Implementeringar av dessa metoder måste vara trådsäkra, och dessutom implementeringen av den `GetCallStatus` och `GetFailureMessage` måste förutsätter inte att de kommer att anropas av samma tråd som ett tidigare anrop till `InitiateCall`.

Store DLL-filen i den `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` directory.

Exempelkod, som kan vara kompilerade med hjälp av Visual Studio 2010 eller senare.

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
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Steg 3: Säkerhetskopiera MfaSettings.xml finns i ”C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Steg 4: Redigera filen mfasettings.XML

Uppdatera eller ta bort följande rader:

- Ta bort/rensa alla configuration poster rader 

- Uppdatera eller lägga till följande rader till följande till MfaSettings.xml med din telefon, eget-provider <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Steg 5: Starta om MIM-tjänsten

När tjänsten har startats om, använda SSPR och/eller PAM för att validera funktioner med den anpassade identitetsprovidern.

> [!NOTE] 
> Om du vill återställa inställningen Ersätt MfaSettings.xml med säkerhetskopian i steg 3


## <a name="next-steps"></a>Nästa steg

- [Komma igång med Azure Multi-Factor Authentication Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Vad är Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [MIM-versionshistorik](./reference/version-history.md)
