---
title: "Förnyelse av smartkort för Microsoft Identity Manager med självbetjäning utan administratörsbehörighet | Microsoft Docs"
description: "Lär dig hur du registrerar smartkort för användare utan administratörsåtkomst till sina datorer så att de kan använda Certifikathanteraren."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: 89e095cff66984140cdcef3617dd0ccc3d3714d8


---

# <a name="enroll-smart-cards-for-non-administrators"></a>Registrera smartkort för icke-administratörer
Om en användare inte är lokal administratör på sin dator kan denne normalt inte registrera ett smartkort på sin egen dator. Genom att följa proceduren som beskrivs nedan kan dock du kringgå den här begränsningen.

## <a name="enabling-smart-card-renewal-for-non-admins-in-mim-2016-certificate-manager"></a>Aktivera förnyelse av smartkort för icke-administratörer i Certifikathanteraren i MIM 2016 

1.  **Packa upp appx-filen**

    Hämta ett signeringscertifikat. Följ stegen för att [signera Windows 8-program med ett internt PKI](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx). Stanna när du kommer till ”Signera programmet”. Ange ett namn på den exporterade pfx-filen. Exportera den även som en .cer-fil och importera den till klienten med hjälp av cer-filen i det nya signeringscertifikatet.

    Utför följande åtgärder för att packa upp appx-filen:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Ändra konfigurationsfilen**

    Byt namn på filen som heter `CustomDataExample.xml custom.data`. CM-appen (Cerfikathanterarappen) börjar leta efter det här filnamnet.

    Redigera custom.data-filen och ändra följande:

    1.  I elementet &lt;NonAdmin&gt; ändrar du värdet för attributet Value till ”True”

    2.  Spara filen och avsluta redigeraren

    3.  Ta bort filen som heter AppxSignature.p7x

    4.  Redigera filen som heter AppxManifest.xml

    5.  I elementet &lt;Identity&gt; ändrar du värdet för attributet Publisher till subjektet i ditt signeringscertifikat, t.ex. ”CN=ABCD”

        Subjektet här måste vara detsamma som subjektet i signeringscertifikatet du använder för att signera appen.

    6.  Spara filen och avsluta redigeraren.

3.  **Packa app-paketet på nytt och signera det (appx-fil)**

    Utför följande åtgärder för att packa och signera appx-filen:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Duplicera profilmallen och lägg till den ursprungliga administratörsnyckeln för att konfigurera MIM-servern:

    1.  Logga in på CM-portalen som en användare med administratörsbehörighet.

    2.  Gå till **Administration** &gt; **Hantera profilmallar** och kontrollera att kryssrutan bredvid den profilmall du skapat är markerad. Klicka sedan på Kopiera en vald profilmall.

    3.  Ange namnet på profilmallen, lägg till ”nonAdmin” och klicka på **OK**.

    4.  När de allmänna inställningarna för profilmallen visas rullar du ned till slutet. Under **Konfiguration av smartkort** klickar du på **Ändra inställningar**.

    5.  Under **Ursprungligt värde för administratörsnyckel (hex)** anger du den förinställda administratörsnyckeln: ”010203040506070801020304050607080102030405060708”

    6.  Rulla ned och klicka på **OK**.

5.  **Skapa ett konto för icke-administratörer på klientdatorn**

    Användare som inte är administratörer kan inte skapa det virtuella smartkortet på TPM, så därför måste du skapa det åt dem.

6.  **Skapa ett virtuellt smartkort med hjälp av TpmVscMgr**

    Utför följande (fortfarande som administratör) för att skapa ett tomt virtuellt smartkort på en dator. Du kan göra detta genom Intune, SCCM eller grupp-policyer.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Installera CM-appen på icke-administratörskontot**

8.  **Starta CM-appen och registrera uppgifterna för ett virtuellt smartkort**



<!--HONumber=Jan17_HO4-->


