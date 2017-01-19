---
title: Microsoft Identity Manager 2016 Service Pack 1 | Microsoft Docs
description: "Förstå hur MIM 2016 fungerar och gör identitetshanteringen säkrare och enklare både lokalt och i molnet."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f0947f186b5206d06a67140706ada33a5bc0e016
ms.openlocfilehash: 4f293a349916ae1a55d8551ef949758cab851b74


---
# <a name="whats-new-for-microsoft-identity-manager-2016-service-pack-1"></a>Nyheter i Microsoft Identity Manager 2016 Service Pack 1 #

Som en del av den regelbundna publiceringscykeln med korrigeringar och uppdateringar för Microsoft Identity Manager kan vi nu tillkännage [Microsoft Identity Manager 2016 (MIM) Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). Det här dokumentet beskriver uppdateringarna, förbättringarna, funktionerna och ändringarna som ingår i den här versionen.

Kontakta Microsofts kundsupport om du får problem under en produktionsdistribution av MIM SP1.

Vi vill också veta vad du tycker! Om du har feedback, kommentarer eller frågor till produktteamet kan du maila dem till [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## <a name="updates-in-this-service-pack"></a>Uppdateringar i detta Service Pack #

### <a name="mim"></a>MIM

- **Kompatibilitet för flera webbläsare på MIM-portalen för självbetjäning av slutanvändare:** I detta Service Pack har vi lagt till stöd för de flesta större webbläsare. Nu kan användarna komma åt och interagera med MIM-portalen för grupp- och profilhantering via självbetjäning från Edge, Chrome och Safari.

- **Stöd för Exchange Online i MIM-tjänsten:** MIM-tjänsten har länge haft stöd för att skicka och ta emot e-postmeddelanden för godkännanden och meddelanden. Före SP1 hade MIM endast stöd för Exchange Server eller SMTP. I och med Service Pack 1 kan MIM-tjänsten skicka och ta emot förfrågningar och e-postmeddelanden med hjälp av ett Office365 Exchange Online-konto.

- **Verifiering av bildfilformat vid överföringar:** Nu kan MIM verifiera filformatet för bilder när de laddas upp till portalen.

### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **PAM ”PRIV” stöder (skyddade) skogar på Windows Server 2016-funktionsnivån:** MIM PAM-tjänsten kan konfigureras i en miljö med domänkontrollanter som körs på funktionsnivån för Active Directory Domain Services-skogar för Windows Server 2016. När den konfigurerats tidsbegränsas en användares Kerberos-biljett till den återstående tiden för användarens rollaktivering.

    >[!Note]
    Om du vill behålla skogen på Windows Server 2012 R2-funktionsnivå i CORP-domänen rekommenderar vi att du installerar [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) och [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) på CORP-domänkontrollanten.

- **Privilegierad kontoupphöjning till grupper som är exklusiva för (den skyddade) ”PRIV”-skogen:** Nu kan administratörer informera MIM-tjänsten om grupper och användare som är exklusiva för ”PRIV”-skogen. Detta gör att de här grupperna och användarna kan läggas till i PAM-roller.  De kan sedan aktiveras för en roll och tilldelas medlemskap i grupper i ”PRIV”-skogen.

- **PAM-distributionsskript:** Med PAM-distributionsskript kan administratörer effektivisera installationen av PAM-miljön.

- **PAM-cmdlets för konfiguration av silor för autentiseringsprinciper:** Service Pack 1 tillhandahåller nya cmdlets som skärper säkerheten i din skyddsskog. Med dessa cmdlets skapas automatiskt en silo för autentiseringsprinciper, som är bunden till en mall för autentiseringsprinciper.

    >[!Note]
    Dessa cmdlets körs automatiskt som en del av distributionsskripten.


## <a name="platform-support"></a>Plattformsstöd
Uppdaterad information om plattformsstöd finns i dokumentet [Plattformar som stöds för MIM 2016](/microsoft-identity-manager/plan-design/microsoft-identity-manager-2016-supported-platforms).  Nya plattformar som stöds i detta Service Pack är SQL Server 2016 och SharePoint 2016

## <a name="issues-fixed-in-this-release-from-mim-2016-general-availability"></a>Problem som åtgärdas i den här versionen från den allmänt tillgängliga versionen av MIM 2016

### <a name="pam"></a>PAM
- New-PAMGroup skapade inte MIM-objekt för lokala domängrupper i PRIV-skogen
- New-PAMDomainConfiguration misslyckades med felmeddelandet ”netdom”
- PAM-övervakningstjänsten loggade varningar för grupper i PRIV-skogen

## <a name="how-to-upgrade-to-service-pack-1"></a>Uppgradera till Service Pack 1

Kunder som uppgraderar till Microsoft Identity Manager 2016 Service Pack 1 bör följa stegen nedan för alla tjänster som är relevanta för deras distribution.

>[!Note]
>Kunder som kör Forefront Identity Manager 2010 R2 SP1 eller tidigare måste först uppgradera sin miljö till Microsoft Identity Manager 2016-versionen från augusti 2015 och sedan följa stegen nedan.

Innan du börjar

Du måste uppgradera MIM-synkroniseringsmotorn innan du uppgraderar MIM-tjänsten och MIM-portalen.
Du måste säkerhetskopiera MIM-tjänstdatabasen och MIM-synkroniseringsdatabasen.

  1. Avinstallera Microsoft Identity Manager-komponenten som du uppgraderar
  2. När avinstallationen är klar öppnar du välkomstsidan som finns i ”FIMSplash.htm” på installationsmediet
  3. Välj den MIM-komponent som du vill uppgradera
  4. Fortsätt med installationen genom att följa anvisningarna
    * Installation av MIM-tjänsten och MIM-portalen: Om du väljer Exchange Online som e-postkonto anger du e-postadressen och autentiseringsuppgifterna för Exchange Online-kontot på nästa skärm.



<!--HONumber=Jan17_HO2-->


