---
title: Konfigurera Microsoft Identity Manager-anslutningen för Microsoft Graph för B2B | Microsoft Docs
author: billmath
description: Microsoft Graph-anslutningen är extern användare Livscykelhantering för AD-konto. I det här scenariot kan en organisation har bjudit in gäster i sin Azure AD-katalog och vill ge dessa gäster åtkomst till lokal Windows-Integrated autentisering eller Kerberos-baserade program
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358779"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Azure AD-business-to-business (B2B) samarbete med Microsoft Identity Manager(MIM) 2016 SP1 med Azure Application Proxy
============================================================================================================================

Det första scenariot är extern användare Livscykelhantering för AD-konto.   I det här scenariot kan en organisation har bjudit in gäster i sin Azure AD-katalog och vill ge dessa gäster åtkomst till lokal Windows-Integrated autentisering eller Kerberos-baserade program den [Azure AD-programproxyn](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) och andra mekanismer som gateway. Azure AD-programproxy måste varje användare ha sina egna AD DS-kontot för identifiering och delegering.

## <a name="scenario-specific-guidance"></a>Scenariospecifika anvisningar

Några antaganden som gjordes i konfigurationen av B2B med MIM och Azure AD Application Proxy:

-   Du redan har distribuerat en lokal AD, och Microsoft Identity Manager är installerade och grundläggande konfiguration av MIM-tjänsten, MIM-portalen, Active Directory-hanteringsagenten (AD MA) och FIM-hanteringsagenten (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Du redan har följt instruktionerna i artikeln om hur du hämtar och installerar den [Graph connector](microsoft-identity-manager-2016-connector-graph.md).

-   Du har Azure AD Connect som konfigurerats för att synkronisera användare och grupper till Azure AD.

-   Du har Azure AD Connect som konfigurerats för att synkronisera Office-grupper för att styra program [tillbaka till den lokala AD DS](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Om du redan har konfigurerat Application Proxy-kopplingar och anslutningsapp-grupper inte så kan du besöka [här](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) att installera och konfigurera

-   Du har redan publicerat ett eller flera program som förlitar sig på Windows-integrerad autentisering eller enskilda AD-konton via Azure AD App Proxy

-   Du har bjudit in eller du bjuder in en eller flera gäster som resulterat i en eller flera användare håller på att skapas i Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Exempelscenario för B2B från slutpunkt till slutpunkt distribution

Den här guiden bygger på följande scenario:

Contoso Pharmaceuticals fungerar med Trey Research Inc. som en del av deras R & D avdelning. Trey Research anställda behöver åtkomst till research reporting program som tillhandahålls av Contoso Pharmaceuticals.

-   Contoso Pharmaceuticals finns i deras egen klienter du har konfigurerat en anpassad domän.

-   Någon har bjudit in en extern användare till Contoso Pharmaceuticals-klient.
    Den här användaren har accepterat inbjudan och kan komma åt resurser som delas.

-   Contoso Pharmaceuticals har publicerat ett program via App Proxy. I det här scenariot är exempelprogrammet som MIM-portalen. Detta gör att gästanvändare att delta i MIM-processer, till exempel i help desk-scenarier eller att begära åtkomst till grupper i MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Konfigurera AD och Azure AD Connect att undanta användare har lagts till från Azure AD

Som standard förutsätter Azure AD Connect att icke-administratörer i Active Directory måste synkroniseras till Azure AD.  Om Azure AD Connect hittar en befintlig användare i Azure AD som matchar användaren från en lokal AD, Azure AD Connect ska matcha de två kontona och förutsätter att detta är en tidigare synkronisering av användaren och gör lokalt AD auktoritativa.  Men är det här standardbeteendet inte lämpligt för B2B-flödet där användarkontot har sitt ursprung i Azure AD. 

Därför måste användare som registrerat sig för AD DS av MIM från Azure AD som ska lagras på ett sätt som Azure AD inte kommer att försöka synkronisera dessa användare tillbaka till Azure AD.
Ett sätt att göra detta är att skapa en ny organisationsenhet i AD DS och konfigurera Azure AD Connect för att undanta den organisationsenheten.  

Mer information finns i [Azure AD Connect-synkronisering: Konfigurera filtrering](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Skapa Azure AD-program 


Obs: Innan du skapar i MIM-synkronisering hanteringsagenten för graph-anslutningen, se till att du har granskat vägledning för distribution av den [Graph Connector](microsoft-identity-manager-2016-connector-graph.md), och skapat ett program med en klient-ID och hemlighet.
Se till att programmet behörighet för minst en av följande behörigheter: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` eller `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Skapa ny hanteringsagenten


Välj i Synchronization Service Manager-UI **Anslutningsappar** och **skapa**.
Välj **Graph (Microsoft)** och ge den ett beskrivande namn.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Anslutning

På sidan anslutningar måste du ange Graph API-versionen. Produktion redo PAI är **V 1.0**, produktion är **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Globala parametrar

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Konfigurera etablering hierarki

Den här sidan används för att mappa DN komponenten, till exempel Organisationsenhet för den objekttyp som ska etableras, till exempel organizationalUnit. Detta behövs inte i det här scenariot så lämna det här standardvärdet och klicka på Nästa.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurera partitioner och hierarkier

Markera alla namnrum med objekt som du planerar att importera och exportera på sidan partitioner och hierarkier.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Välj objekttyper

På sidan för typer av objekt väljer du de objekttyper som du tänker importera. Du måste välja minst ”användare”.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Välj attribut

Välj attribut på skärmen Välj attribut från Azure AD som behövs för att hantera B2B-användare i AD. Attributet ”ID” måste anges.  Attributen `userPrincipalName` och `userType` kommer att användas senare i den här konfigurationen.  Andra attribut är valfria, inklusive

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Konfigurera ankare

På skärmen Konfigurera ankare är konfigurera attributet förtroendeankare ett obligatoriskt steg. som standard använder du ID-attributet för användarmappning av.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Konfigurera anslutningsfilter

På sidan Konfigurera anslutningsfilter kan MIM du filtrera bort objekt baserat på attributfiltret. I det här scenariot för B2B målet är att hämta endast användare med värdet för den `userType` attribut som är lika med `Guest`, och inte användare med userType som är lika med `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Konfigurera anslutnings- och Projektionsregler

Den här handboken förutsätts att du skapar en synkroniseringsregel för.  När du konfigurera regler för anslutning och projektion hanteras av synkroniseringsregel, behövs det inte behöver identifiera en anslutning och projektion på kopplingen själv. Lämna standard och klicka på ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Konfigurera attributflöde

Den här handboken förutsätts att du skapar en synkroniseringsregel för.  Projektion behövs inte för att definiera attributflödet i MIM-synkronisering, som hanteras av synkroniseringsregel som skapas senare. Lämna standard och klicka på ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Konfigurera avetablering

Inställningen för att konfigurera avetablering kan du konfigurera MIM-synkronisering för att ta bort objektet, om metaversumobjekt tas bort. I det här scenariot kan göra vi dem disconnectors eftersom målet är att låta dem vara i Azure AD. I detta scenario vi exporterar inte något till Azure AD och anslutningen har konfigurerats för att importera endast.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Konfigurera tillägg

Konfigurera tillägg på den här management agent är ett alternativ men krävs inte eftersom vi använder en synkroniseringsregel. Om vi valt att använda en avancerad regel i attributflödet tidigare, skulle kommer det finnas möjlighet att ange tillägg för regler.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Utöka metaversumschemat


Innan du skapar regeln för synkronisering måste vi skapa ett attribut som kallas userPrincipalName knutna till personobjektet med hjälp av MV-Designer.

Välj Metaversumdesigner i synkronisering-klienten

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Välj typen av objekt som Person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klicka sedan på Lägg till attribut under åtgärder

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Slutför följande information

Attributnamn: **userPrincipalName**

Attributtyp: **sträng (vara)**

Indexerade = **SANT**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Skapa Synkroniseringsregler i MIM-tjänsten

i stegen nedan börjar vi mappningen av B2B gästkontot och attributflödet. Vissa antaganden görs här: att du redan har Active Directory MA som konfigurerats och FIM MA som konfigurerats för att lägga till användare till MIM-tjänsten och portalen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Nästa steg kräver att lägga till minimal konfiguration att FIM MA- och AD MA.

Mer information finns här för konfigurationen <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> – hur gör jag för att etablera användare i AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Synkroniseringsregeln: Importera gästanvändare till MV till synkronisering Service metaversum från Azure Active Directory<br>

Gå till MIM-portalen, väljer Synkroniseringsregler och klickar på ny.  Skapa en regel för inkommande synkronisering för B2B-flöde via graph-anslutningsprogrammet.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Relation kriterier steget måste du markera ”skapa resurs i FIM”.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Konfigurera följande regler för inkommande attributflöde flöde.  Se till att fylla i den `accountName`, `userPrincipalName` och `uid` attribut som de ska användas senare i det här scenariot:

| **Endast Initialflöde** | **Används som Befintlighetstest** | **Flöda (källa värdet ⇒ FIM-attribut)**                          |
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

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Synkroniseringsregeln: Skapa gäst användarkonto i Active Directory 

Den här synkroniseringsregeln skapar användaren i Active Directory.  Var noga med att flödet för `dn` måste placera användaren i organisationsenheten som har undantagits från Azure AD Connect.  Uppdatera också flödet för `unicodePwd` för att uppfylla din AD-lösenordsprincip - användaren behöver inte känna till lösenordet.  Anteckna värdet för `262656` för `userAccountControl` kodar flaggorna `SMARTCARD_REQUIRED` och `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Postflödesregler:

| **Endast Initialflöde** | **Används som Befintlighetstest** | **Flöda (FIM värdet ⇒ målattribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | [”CN =” + uid + ”, OU = B2BGuest, DC = contoso, DC = com” ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Valfritt Synkroniseringsregeln: Importera B2B gäst användar-objekt SID för inloggning till MIM 

Den här regeln för inkommande synkronisering tar användarens SID-attribut från Active Directory tillbaka till MIM, så att användaren har åtkomst till MIM-portalen.  MIM-portalen kräver att användaren har attributen `samAccountName`, `domain` och `objectSid` ifylld i MIM-tjänstens databas.

Konfigurera externa källsystemet som den `ADMA`, som den `objectSid` attributet anges automatiskt av AD när du skapar i MIM.
 
Observera att om du konfigurerar användare ska kunna skapas i MIM-tjänsten, se inte till att de i omfånget för alla uppsättningar avsedd för hanteringsprincipregler (MPR) medarbetare SSPR.  Du kan behöva ändra dina definitioner för att undanta användare som har skapats med hjälp av B2B-flödet. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Endast Initialflöde** | **Används som Befintlighetstest** | **Flöda (källa värdet ⇒ FIM-attribut)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | [”CONTOSO” ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Kör synkroniseringsreglerna

Nu ska vi bjuda in användaren och kör sedan hanteringen agenten Synkroniseringsregler i följande ordning:

-   Fullständig Import och synkronisering på den `MIMMA` hanteringsagent.  Detta säkerställer att MIM-synkronisering har de senaste Synkroniseringsregler som har konfigurerats.

-   Fullständig Import och synkronisering på den `ADMA` hanteringsagent.  Detta säkerställer att MIM- och Active Directory är konsekventa.  Då det ännu inte några väntande exporter för gäster.

-   Fullständig Import och synkronisering på hanteringsagenten för B2B-diagram.  Detta öppnar i gästanvändare till metaversum.  Ett eller flera konton kommer nu att väntande exporten av `ADMA`.  Om det finns inga väntande exporter, kontrollerar du att gästanvändare har importerats till anslutningsplatsen och att reglerna har konfigurerats att få ett AD-konton.

-   Export och Deltaimport synkronisering på platsen på den `ADMA` hanteringsagent.  Om exporten misslyckades, kontrollera regelkonfigurationen och avgöra om det fanns någon saknas schema-krav. 

-   Export och Deltaimport synkronisering på platsen på den `MIMMA` hanteringsagent.  När du är klar, bör det inte längre finnas några väntande exporter.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Valfritt: Application Proxy för B2B gästerna logga in på MIM-portalen

Nu när vi har skapat synkroniseringsreglerna i MIM. I App Proxy-konfigurationen definierar använda cloud-huvudnamn för KCD på approxy.
Dessutom sedan lägga till användaren manuellt till Hantera användare och grupper. Alternativ inte ska visas för användaren förrän skapandet har inträffat i MIM för att lägga till gästen för att en office-grupp som en gång etablerats kräver lite mer konfiguration som inte omfattas i det här dokumentet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


När alla har konfigurerats, har B2B användarinloggning och se programmet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Nästa steg
----------

[Hur etablerar jag användare i AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Funktionsreferens för FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Att tillhandahålla säker fjärråtkomst till lokala program](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Ladda ned Microsoft Identity Manager-anslutningen för Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
