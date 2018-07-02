---
title: Distribuera PAM steg 4 – Installera MIM | Microsoft Docs
description: Installera och konfigurera MIM-tjänsten och -portalen på Privileged Access Management-servern och arbetsstationer.
keywords: ''
author: barclayn
ms.author: barclayn
manager: barclayn
ms.date: 09/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 81fe10b8fbf8ada08983c4bf3c58f85215cf1d66
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290041"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>Steg 4 – installera MIM-komponenter på PAM-servern och arbetsstationen

> [!div class="step-by-step"]
> [« Steg 3](step-3-prepare-pam-server.md)
> [Steg 5 »](step-5-establish-trust-between-priv-corp-forests.md)

På PAMSRV loggar du in som PRIV\Administratör för att kunna installera MIM-tjänsten och portalen och exempelwebbappen för portalen.

  > [!NOTE]
  > Du måste vara domänadministratör. Om du inte kör följande kommandon som domänadministratör slutförs inte förtroendekontrollerna i nästa steg.

Om du har laddat ned MIM packar du upp MIM-installationsarkivet till en ny mapp.

## <a name="run-the-service-and-portal-install-program"></a>Kör installationsprogrammet för tjänsten och portalen.

Följ riktlinjerna i installationsprogrammet och slutför installationen.

1. När du väljer komponentfunktioner tar du med MIM-tjänsten (med Privileged Access Management, men inte MIM-rapportering) och MIM-portalen  

   ![Anpassad installation – skärmbild](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. När du konfigurerar vanliga tjänster och anslutningen till MIM-databasen, anger du **Skapa en ny databas**.

   > [!NOTE]
   > Om du installerar MIM-tjänsten flera gånger för hög tillgänglighet anger du **Använd en befintlig databas** för alla efterföljande installationer.

3. När du konfigurerar en anslutning till en e-postserver, anger du värdnamnet för en Exchange- eller SMTP-server för CORP-miljön som e-postserver (använd corpdc.contoso.local om du inte har en e-postserver) och avmarkerar kryssrutorna **Använd SSL** och **E-postservern är Exchange Server 2007 eller Exchange Server 2010**.

4. Välja att generera ett nytt självsignerat certifikat.

5. Ange följande autentiseringsuppgifter för kontot:
   - Namn på tjänstkonto: *MIMService*  
   - Lösenord för tjänstkonto: <em>Pass@word1</em> (eller lösenordet du skapade i steg 2)  
   - Domän för tjänstkonto: *PRIV*  
   - E-postkonto för tjänsten: <em>MIMService@priv.contoso.local</em>  

6. Godkänn standardinställningarna för synkroniseringsserverns värdnamn och ange *PRIV\MIMMA* som konto för MIM-hanteringsagenten. En varning visas om att synkroniseringstjänsten för MIM inte finns. Det är OK, eftersom synkroniseringstjänsten för MIM inte används i det här scenariot.

7. Ange *PAMSRV* som MIM-tjänstens serveradress.

8. Ange *http://pamsrv.priv.contoso.local:82* som SharePoint-webbplatssamlingen.

9. Lämna Registrering av portal-URL tomt.

10. Markera kryssrutan för att öppna portarna 5725 och 5726 i brandväggen, och kryssrutan för att bevilja alla autentiserade användare åtkomst till MIM-portalens webbplats.

11. Lämna PAM REST API-värdnamnet tomt och ange *8086* som portnummer.

    ![Bindningsinformation för PAM REST-API – skärmbild](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Konfigurera REST-API-kontot för MIM PAM att använda samma konto som SharePoint (eftersom MIM-portalen är samordnad på den här servern):
    - Namn på programpoolskonto: *SharePoint*  
    - Lösenord för programpoolskonto: <em>Pass@word1</em> (eller lösenordet du skapade i steg 2)  
    - Domän för programpoolskonto: *PRIV*  

    ![Autentiseringsuppgifter för programpoolskonto – skärmbild](./media/PAM_GS_Configure_Component_Service.png)

    En varning kan visas om att tjänstkontots nuvarande konfiguration inte är säker. Det är OK.

13. Konfigurera MIM PAM-komponenttjänsten:
    - Namn på tjänstkontot – *MIMComponent*
    - Lösenord för tjänstkonto: <em>Pass@word1</em> (eller lösenordet du skapade i steg 2)  
    - Domän för tjänstkonto: *PRIV*

    ![Autentiseringsuppgifter för PAM-komponenttjänstkonto – skärmbild](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Konfigurera PAM-övervakningstjänsten:
    - Namn på tjänstkonto: *MIMMonitor*  
    - Lösenord för tjänstkonto: <em>Pass@word1</em> (eller lösenordet du skapade i steg 2)  
    - Domän för tjänstkonto: *PRIV*  

    ![Autentiseringsuppgifter för PAM-övervakningstjänstkonto – skärmbild](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. På portalsidan Ange informationen för MIM-lösenord lämnar du kryssrutorna tomma och fortsätter. Klicka på **Nästa** och fortsätt med installationen.

När installationen är klar startas servern om. Kontrollera att MIM-portalen är aktiv och tillåt att användarna att visa sina egna objektresurser i MIM.

## <a name="set-up-mim-portal-management-policy-rules"></a>Konfigurera MIM-portalens hanteringsprincipregler

1. När PAMSRV startat om loggar du in som PRIV\Administratör.

2. Starta Internet Explorer och Anslut till MIM-portalen på http://pamsrv.priv.contoso.local:82/identitymanagement. Det kan ske en kort fördröjning den första gången sidan finns.

3. Om det behövs loggar du in som PRIV\Administratör för Internet Explorer.

4. I Internet Explorer går du till **Internetalternativ**, sedan till fliken **Säkerhet** och lägger till webbplatsen i **Zonen Lokalt nätverk** om den inte redan finns där. Stäng dialogrutan Internetalternativ.

5. Visa MIM-portalen i Internet Explorer och klicka på **Hanteringsprincipregler (MPR)**.

6. Sök efter hanteringsprincipregeln **Användarhantering: användare får läsa sina egna attribut**.

7. Välj den här hanteringsprincipregeln och avmarkera **Principen är inaktiverad**. Klicka på **OK** och **Skicka**.

## <a name="verify-the-firewall-connections"></a>Kontrollera brandväggens anslutningar

Brandväggen ska tillåta inkommande anslutningar till TCP-portarna 5725, 5726, 8086 och 8090.

1.  Starta **Windows-brandväggen med avancerad säkerhet** (i Administrationsverktyg).  
2.  Klicka på **Regler för inkommande trafik**.  
3.  Kontrollera att de här två reglerna visas:  
    - Forefront Identity Manager-tjänsten (STS)
    - Forefront Identity Manager-tjänsten (Webbtjänst)  
4.  Klicka på **Ny regel** > **Port** > **TCP**, och ange de specifika lokala portarna *8086* och *8090*. Klicka dig igenom guiden och acceptera standardinställningarna, ge regeln ett namn och klicka på **Slutför**.  
5.  Slutför guiden och stäng programmet Windows-brandvägg.

6.  Starta **kontrollpanelen**.  
7.  Under Nätverk och Internet väljer du **Visa nätverksstatus och -åtgärder**.  
8.  Verifiera att det finns ett aktivt nätverk listat som priv.contoso.local och ett domännätverk.  
9. Stäng **kontrollpanelen**.

## <a name="set-up-the-sample-web-application"></a>Ställ in exempelwebbappen

I det här avsnittet ska du installera och konfigurera exempelwebbappen för REST-API:t för MIM PAM.

1. Ladda ner [identitetshanteringsexemplen](https://github.com/Azure/identity-management-samples) som en zip-fil från exempelwebbappens arkiv.

2. Packa upp innehållet i mappen **identity-management-samples-master\Privileged-Access-Management-Portal\src** i en ny mapp**C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3. Skapa den nya webbplatsen i IIS med ett webbplatsnamn för exempelportalen för MIM Privileged Access Management, fysisk sökväg C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management-portal och port 8090.  Det kan du göra med följande PowerShell-kommando:

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. Konfigurera exempelwebbappen att kunna vidarebefordra användare till REST-API:t för MIM PAM. Med en textredigerare, till exempel Anteckningar, redigerar du filen **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**. I avsnittet **<system.webServer>** lägger du till följande rader:

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. Konfigurera exempelwebbappen. Med en textredigerare, till exempel Anteckningar, redigerar du filen **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**. Ange värdet för **pamRespApiUrl** till *http://pamsrv.priv.contoso.local:8086/api/pamresources/*.

6. Starta om IIS med följande kommando för att ändringarna ska börja gälla.

   ```cmd
   iisreset
   ```

7. (Valfritt) Kontrollera att användaren kan autentiseras för REST-API:t. Öppna en webbläsare som administratör på PAMSRV.  Gå till webbplatsens URL http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/, autentisera om det behövs och kontrollera att en nedladdning sker.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>Installera MIM PAM-cmdletarna för begärande

Installera MIM PAM-cmdletarna för begärande på arbetsstationen du konfigurerade i steg 1.

1.  Logga in på CORPWKSTN som administratör.

2.  Ladda ned **-tilläggen** till CORPWKSTN-datorn, om de inte redan finns där.

3.  Packa upp mappen **Tillägg** från arkivet till en ny mapp.

4.  Kör installationsprogrammet **setup.exe**.

5.  På den anpassade installationen anger du den **PAM-klient** som ska installeras, men inte **MIM-tillägg för Outlook** och **MIM-lösenord och autentiseringstillägg**.

6.  I PAM-Server-adressen anger du *pamsrv.priv.contoso.local* som värdnamnet för PRIV MIM-servern.

När installationen är klar startar du om CORPWKSTN för att slutföra registreringen av den nya PowerShell-modulen.

I nästa steg ska du upprätta förtroende mellan PRIV- och CORP-skogar.

> [!div class="step-by-step"]
> [« Steg 3](step-3-prepare-pam-server.md)
> [Steg 5 »](step-5-establish-trust-between-priv-corp-forests.md)
