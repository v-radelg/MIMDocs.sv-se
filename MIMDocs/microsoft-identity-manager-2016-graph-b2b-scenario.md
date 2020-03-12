---
title: Konfigurera Microsoft Identity Manager Connector för Microsoft Graph för B2B | Microsoft Docs
author: billmath
description: Microsoft Graph koppling är en extern användares livs cykel hantering för AD-konto. I det här scenariot har en organisation bjudit in gäster till sin Azure AD-katalog och vill ge gästarna till gång till lokala Windows-integrerade autentiseringar eller Kerberos-baserade program
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2f91a5c24df5475130755574c77b536f57e64d24
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044250"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Azure AD Business-to-Business (B2B)-samarbete med Microsoft Identity Manager (MIM) 2016 SP1 med Azure Application Proxy
============================================================================================================================

Det inledande scenariot är livs cykel hantering av AD-användare.   I det här scenariot har en organisation bjudit in gäster till sin Azure AD-katalog och vill ge dessa gäster åtkomst till lokala Windows-integrerade autentiserings-eller Kerberos-baserade program, via [Azure AD-programproxyn](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) eller andra gateway-mekanismer. Azure AD-programproxyn kräver att varje användare har sitt eget AD DS-konto, i syfte att identifiera och delegering.

## <a name="scenario-specific-guidance"></a>Scenario – speciell vägledning

Några antaganden som görs i konfigurationen av B2B med MIM och Azure AD-programproxy:

-   Du har redan distribuerat en lokal AD och Microsoft Identity Manager installerat och grundläggande konfiguration av MIM-tjänsten, MIM-portalen, Active Directory hanterings agent (AD MA) och FIM Management Agent (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Du har redan följt anvisningarna i artikeln om hur du hämtar och installerar [graf Connector](microsoft-identity-manager-2016-connector-graph.md).

-   Du har Azure AD Connect konfigurerat för synkronisering av användare och grupper till Azure AD.

-   Du har redan konfigurerat Application Proxy-kopplingar och anslutnings grupper, om inte kan du gå [hit](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) för att installera och konfigurera

-   Du har redan publicerat ett eller flera program som förlitar sig på Windows-integrerad autentisering eller enskilda AD-konton via Azure AD App proxy

-   Du har bjudit in eller du bjuder in en eller flera gäster, som har resulterat i att en eller flera användare skapas i Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Exempel scenario för B2B-slut punkt till slut punkt

Den här guiden bygger på följande scenario:

Contoso Pharmaceuticals fungerar med Trey Research Inc. som en del av R & D-avdelningen. Trey Research-anställda behöver få åtkomst till programmet för forsknings rapportering som tillhandahålls av contoso Pharmaceuticals.

-   Contoso Pharmaceuticals finns i en egen klient organisation, så att du har konfigurerat en anpassad domän.

-   Någon har bjudit in en extern användare till contoso Pharmaceuticals-klienten.
    Den här användaren har accepterat inbjudan och kan komma åt resurser som delas.

-   Contoso Pharmaceuticals har publicerat ett program via App proxy. I det här scenariot är exempel programmet MIM-portalen. Detta gör det möjligt för en gäst användare att delta i MIM-processer, till exempel i support scenarier eller för att begära åtkomst till grupper i MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Konfigurera AD och Azure AD Connect för att utesluta användare som har lagts till från Azure AD

Som standard förutsätter Azure AD Connect att icke-administratörs användare i Active Directory måste synkroniseras till Azure AD.  Om Azure AD Connect hittar en befintlig användare i Azure AD som matchar användaren från den lokala AD-platsen kommer Azure AD Connect att matcha de två kontona och förutsätter att detta är en tidigare synkronisering av användaren och att den lokala AD auktoritativ används.  Detta standard beteende är dock inte lämpligt för B2B-flödet, där användar kontot kommer från Azure AD. 

De användare som förts in i AD DS av MIM från Azure AD måste därför lagras på ett sätt som Azure AD inte kommer att försöka synkronisera de här användarna tillbaka till Azure AD.
Ett sätt att göra detta är att skapa en ny organisationsenhet i AD DS och konfigurera Azure AD Connect att exkludera den organisationsenheten.  

Mer information finns i [Azure AD Connect Sync: Konfigurera filtrering](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Skapa Azure AD-programmet 


Obs! innan du skapar i MIM synkroniserar du hanterings agenten för graf Connector, kontrollerar att du har granskat guiden för att distribuera [graf Connector](microsoft-identity-manager-2016-connector-graph.md)och skapat ett program med klient-ID och hemlighet.
Se till att programmet har behörighet för minst en av följande behörigheter: `User.Read.All``User.ReadWrite.All``Directory.Read.All` eller `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Skapa den nya hanterings agenten


I Synchronization Service Manager användar gränssnitt väljer du **kopplingar** och **skapa**.
Välj **Graph (Microsoft)**  och ge den ett beskrivande namn.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Anslutningar

På sidan anslutningar måste du ange Graph API version. Produktions klara PAI är **V 1,0**och icke-produktion är **beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Globala parametrar

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Konfigurera etablerings hierarki

Den här sidan används för att mappa DN-komponenten, till exempel OU, till den objekt typ som ska tillhandahållas, till exempel organizationalUnit. Detta behövs inte för det här scenariot, så lämna det som standard och klicka på Nästa.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurera partitioner och hierarkier

På sidan partitioner och hierarkier väljer du alla namn områden med objekt som du vill importera och exportera.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Välj objekt typer

På sidan objekt typer väljer du de objekt typer som du tänker importera. Du måste välja minst användare.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Välj attribut

På skärmen Välj attribut väljer du attribut från Azure AD som behövs för att hantera B2B-användare i AD. Attributet "ID" krävs.  Attributen `userPrincipalName` och `userType` kommer att användas senare i den här konfigurationen.  Andra attribut är valfria, inklusive

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Konfigurera ankare

På sidan Konfigurera fäst punkt krävs ett obligatoriskt steg när du konfigurerar attributet Anchor. som standard använder du attributet ID för användar mappning.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Konfigurera anslutnings filter

På sidan Konfigurera anslutnings filter kan du använda MIM för att filtrera bort objekt baserat på attribut filter. I det här scenariot för B2B är målet att bara hämta användare med värdet för det `userType`-attribut som är lika med `Guest`, och inte användare med userType som är lika med `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Konfigurera regler för anslutning och projektion

Den här guiden förutsätter att du skapar en Synkroniseringsregel.  När du konfigurerar kopplings-och projektions regler hanteras av en Synkroniseringsregel, behöver du inte identifiera en koppling och projektion på själva kopplingen. Lämna standard och klicka på OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Konfigurera Attribute Flow

Den här guiden förutsätter att du skapar en Synkroniseringsregel.  Projektion behövs inte för att definiera Attribute-flödet i MIM Sync, eftersom det hanteras av den Synkroniseringsregel som skapas senare. Lämna standard och klicka på OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Konfigurera avetablering

Med inställningen konfigurera avetablering kan du konfigurera MIM Sync så att objektet tas bort om metaversum-objektet tas bort. I det här scenariot gör vi dem frånkopplade eftersom målet är att lämna dem i Azure AD. I det här scenariot exporterar vi inte något till Azure AD och kopplingen är konfigurerad för import.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Konfigurera tillägg

Konfigurera tillägg för den här hanterings agenten är ett alternativ, men krävs inte eftersom vi använder en Synkroniseringsregel. Om vi valde att använda en avancerad regel i attributet Flow tidigare, kan det finnas ett alternativ för att definiera regel tillägget.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Utöka metaversum-schemat


Innan du skapar synkroniseringsregeln måste vi skapa ett attribut med namnet userPrincipalName knutet till person objekt med hjälp av MV-designern.

I synkroniserings klienten väljer du metaversum designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Välj sedan person objekt typ

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klicka på Lägg till attribut i nästa åtgärd

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Slutför sedan följande information

Attributnamn: **userPrincipalName**

Attributtyp: **sträng (indexbar)**

Indexerad = **Sant**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Skapa regler för synkronisering av MIM-tjänster

i stegen nedan påbörjar vi mappningen av B2B-gäst konto och Attribute-flödet. Vissa antaganden görs här: att du redan har Active Directory MA konfigurerat och FIM MA konfigurerat för att hämta användare till MIM-tjänsten och-portalen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Nästa steg kräver att du lägger till minimal konfiguration till FIM MA och AD MA.

Mer information hittar du här för konfigurations <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> – hur etablerar jag användare i AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Synkroniseringsregel: importera gäst användare till MV till metaversum för synkroniseringstjänst från Azure Active Directory<br>

Navigera till MIM-portalen, Välj regler för synkronisering och klicka på ny.  Skapa en regel för inkommande synkronisering för B2B-flödet via graf Connector.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

I steget Relations villkor, se till att välja "skapa resurs i FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Konfigurera följande flödes regler för inkommande attribut.  Var noga med att fylla i `accountName``userPrincipalName` och `uid` attribut som kommer att användas senare i det här scenariot:

| **Endast initialt flöde** | **Använd som Existenss test** | **Flow (käll värde ⇒ FIM-attribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Vänster (id 20) ⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Synkroniseringsregel: skapa ett gäst användar konto som ska Active Directory 

Den här synkroniseringsregeln skapar användaren i Active Directory.  Se till att flödet för `dn` måste placera användaren i organisationsenheten som uteslöts från Azure AD Connect.  Uppdatera även flödet för `unicodePwd` för att uppfylla lösen ords principen för AD – användaren behöver inte känna till lösen ordet.  Observera värdet för `262656` för `userAccountControl` kodar flaggorna `SMARTCARD_REQUIRED` och `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Flödes regler:

| **Endast initialt flöde** | **Använd som Existenss test** | **Flow (FIM-värde ⇒ målattribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Ja**                 |                           | [”CN =” + uid + ”, OU = B2BGuest, DC = contoso, DC = com” ⇒dn](javascript:void(0);) |
| **Ja**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Ja**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Valfri Synkroniseringsregel: importera B2B-gäst användar objekt SID för att tillåta inloggning till MIM 

Den här regeln för inkommande synkronisering hämtar användarens SID-attribut från Active Directory tillbaka till MIM, så att användaren kan komma åt MIM-portalen.  MIM-portalen kräver att användaren har attributen `samAccountName`, `domain` och `objectSid` som är ifyllda i MIM-tjänstens databas.

Konfigurera det externa käll systemet som `ADMA`, eftersom attributet `objectSid` anges automatiskt av AD när MIM skapar användaren.
 
Observera att om du konfigurerar användare att skapas i MIM-tjänsten bör du se till att de inte omfattas av några uppsättningar som är avsedda för de anställdas SSPR hanterings princip regler.  Du kan behöva ändra uppsättnings definitionerna för att utesluta användare som har skapats av B2B-flödet. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Endast initialt flöde** | **Använd som Existenss test** | **Flow (käll värde ⇒ FIM-attribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | [”CONTOSO” ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Kör reglerna för synkronisering

Sedan bjuder vi in användaren och kör sedan reglerna för hanterings agenten i följande ordning:

-   Fullständig import och synkronisering av `MIMMA` hanterings agenten.  Detta säkerställer att MIM-synkronisering har de senaste reglerna för synkronisering.

-   Fullständig import och synkronisering av `ADMA` hanterings agenten.  Detta säkerställer att MIM och Active Directory är konsekventa.  I det här läget kommer det ännu inte finnas några väntande exporter för gäster.

-   Fullständig import och synkronisering på hanterings agenten för B2B-grafen.  Detta ger gäst användarna till metaversum.  Ett eller flera konton väntar nu på att exporteras för `ADMA`.  Om det inte finns någon väntande export kontrollerar du att gäst användare har importer ATS till anslutnings utrymmet och att reglerna har kon figurer ATS för att få AD-konton.

-   Exportera, delta import och synkronisering i `ADMA` hanterings agenten.  Om exporten misslyckades kontrollerar du regel konfigurationen och kontrollerar om det fanns några saknade schema krav. 

-   Exportera, delta import och synkronisering i `MIMMA` hanterings agenten.  När detta är klart bör det inte längre finnas några väntande exporter.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Valfritt: programproxy för B2B-gäster som loggar in på MIM-portalen

Nu när vi har skapat reglerna för synkronisering i MIM. I konfiguration av App-proxy definierar du Använd Cloud-huvudobjektet för att tillåta KCD på App proxy.
Dessutom har användaren lagt till användaren manuellt i hantera användare och grupper. Alternativen för att inte Visa användaren förrän skapandet har skett i MIM för att lägga till gästen i en Office-grupp när etableringen har slutförts kräver lite mer konfiguration som inte beskrivs i det här dokumentet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


När alla har kon figurer ATS, har du B2B-användar inloggning och kan se programmet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Nästa steg
----------

[Hur etablerar jag användare i AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Funktionsreferens för FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Så här ger du säker fjärråtkomst till lokala program](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Hämta Microsoft Identity Manager Connector för Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
