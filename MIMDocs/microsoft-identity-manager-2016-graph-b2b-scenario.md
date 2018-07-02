---
title: Microsoft Identity Manager hanteringsagenten för Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Graph (förhandsversion) är externa användare Livscykelhantering för AD-kontot. I det här scenariot en organisation har bjudits in gäster i sina Azure AD-katalog och vill ge dessa gäster tillgång till lokala Windows-integrerad autentisering och Kerberos-baserade program
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479170"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Azure AD-business-to-business (B2B) samarbete med Microsoft Identity Manager(MIM) 2016 SP1 med Azure-programproxy (förhandsversion)
============================================================================================================================

Det första scenariot i preview för är externa användare Livscykelhantering för AD-kontot.   I det här scenariot en organisation har bjudits in gäster i sina Azure AD-katalog och vill ge dessa gäster tillgång till lokala Windows-integrerad autentisering och Kerberos-baserade program den [Azure AD-program](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy eller andra mekanismer för gateway. Azure AD application proxy måste varje användare ha sina egna AD DS-konto för identifiering och delegering

## <a name="scenario-specific-supported-guidance"></a>Scenariot specifika stöds anvisningar

I det här scenariot en organisation har bjudits in gäster i sina Azure AD-katalog och vill ge dessa gäster åtkomst till lokala Windows. Integrerad autentisering eller Kerberos-baserade program den [Azure AD-program](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) proxy eller andra mekanismer för gateway. Azure AD application proxy måste varje användare ha sina egna AD DS-konto för identifiering och delegering

Några antaganden som gjorts i konfigurationen av B2B med MIM och Azure Application Proxy

-   Du har installerat den [diagram hanteringsagenten](microsoft-identity-manager-2016-connector-graph.md).

-   Du har en lokal AD och Azure AD Connect set upp för att synkronisera användare och grupper till Azure AD.

    -   Använda Office-grupper som kontrollerar programmets [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Om du redan har konfigurerat Application Proxy kopplingar och connector grupper inte så kan du besöka [här](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) att installera och konfigurera

-   Publicerade ett eller flera program som förlitar sig på Windows-integrerad autentisering eller enskilda AD-konton via Azure AD App-Proxy

-   Du har bjudits in eller du bjuda in en eller flera gäster som skapats i Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager är installerat och grundläggande konfiguration av tjänsten och portalen och Active Directory Management Agent.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>Distribution av B2B-slutpunkt till slutpunkt

Scenario

Contoso Pharmaceuticals fungerar Trey Research Inc. som en del av deras R & D-avdelningen. Trey Research medarbetare behöver åtkomst till research reporting program som tillhandahålls av Contoso Pharmaceuticals.

-   Contoso Pharmaceuticals finns i en oberoende klient till har konfigurerat en anpassad domän.

-   Bjudits in en extern användare: till Contoso Pharmaceuticals klient.
    Den här användaren har accepterat inbjudan och kan komma åt resurser som delas.

-   Publicera ett program via appen Proxy och i det här scenariot som exempel för att använda MIM-tjänst och Portal för gästanvändare för att delta i MIM-processen, exempel supportavdelningsscenarion.

## <a name="create-the-graph-management-agent"></a>Skapa hanteringsagenten diagram

Obs: Innan du skapar kopplingen kontrollera, du har granskat den [diagram hanteringsagenten](microsoft-identity-manager-2016-connector-graph.md).

Välj i Synchronization Service Manager-UI **kopplingar** och **skapa**.
Välj **diagram (Microsoft)** och ge det ett beskrivande namn

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Anslutning

På sidan anslutningar måste du ange Graph API-Version produktion redo **V 1.0**, icke-produktionsmiljö är **Beta**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Behörigheter

På sidan globala parametrar kan du konfigurera DN delta ändringsloggen och ytterligare LDAP-funktioner. Sidan fylls med den information som tillhandahålls av LDAP-servern.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Globala parametrar

På sidan globala parametrar kan du konfigurera DN delta ändringsloggen och ytterligare LDAP-funktioner. Sidan fylls med den information som tillhandahålls av LDAP-servern.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Konfigurera etablering hierarki

Den här sidan används för att mappa DN komponenten, till exempel OU för den objekttyp som ska etableras, till exempel organizationalUnit. lämna detta standard klickar du på nästa

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurera partitioner och hierarkier

Markera alla namnområden med objekt som du tänker importera och exportera på sidan partitioner och hierarkier.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Välj objekttyper

Markera alla namnområden med objekt som du tänker importera och exportera på sidan partitioner och hierarkier.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Välj attribut

På skärmen väljer attribut behövs Välj attribut för att hantera B2B-användare. Attributet ”id” måste anges

-   ID

-   visningsnamn

-   e-post

-   förnamn

-   Efternamn

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Konfigurera ankare

Konfigurera fästpunkten är ett obligatoriskt steg med Konfigurera-ankaret. som standard Använd ID-attributet för mappningen.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Konfigurera anslutningsfilter

På Konfigurera kan anslutningsfilter sidan du filtrera bort objekt baserat på attributfiltret. I det här scenariot för B2B ska målet bara ta in användare med userType som är lika med gäst- och inte medlem.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Konfigurera anslutnings- och Projektionsregler

Konfigurera anslutning och projektion regler hanteras av synkroniseringsregel, behövs inte behöver identifiera en anslutning och projektion på av kopplingen själv. Lämna standard och klicka på ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Konfigurera attributflöde

Som den anslutning och projektion behövs inte för att definiera attributflöde, eftersom den är referensen av synkroniseringsregel som ska vara skapa senare. Lämna standard och klicka på ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Konfigurera avetablering

Konfigurera avetablering kan du ta bort objektet om metaversumobjekt tas bort. I det här testet kan vi göra dem disconnectors eftersom målet är att låta dem i Azure också vi inte exporterar något till azure eftersom det är Import.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Konfigurera tillägg

Konfigurera tillägg på den här management agent är ett alternativ men krävs inte som vi använder en regel för synkronisering. Om vi valt till en avancerad regel i attributflöde tidigare, skulle detta vara ett alternativ för att definiera.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Skapa regler för MIM-synkronisering

Vi börjar mappningen av B2B gästkontot och attributflödet i stegen nedan. Vissa antaganden görs här att det finns redan i Active Directory Management Agent konfigurerad och FIM MA som konfigurerats för att kommunicera med MIM-tjänsten och portalen.

Innan du skapar regeln för synkronisering måste vi skapa ett attribut som kallas userPrincipalName som är knutna till objektet person med hjälp av MV-Designer.

Välj Metaverse Designer i synkronisering-klienten

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Välj objekttyp Person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Klicka sedan på Lägg till attribut under åtgärder

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Slutligen slutföra följande uppgifter

Attributnamn: **userPrincipalName**

Attributtypen: **sträng (indexeras)**

Indexerade = **SANT**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Nästa steg kräver minimal konfiguration av FIM-tjänstens hanteringsagent och i Active Directory Domain Services Management Agent.

Mer information hittar du här konfiguration <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> -hur gör jag för att etablera användare till AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Synkroniseringsregeln: Importera gästanvändaren till MV till synkronisering Service metaversum från Azure Active Directory<br>

Navigera till MIM-tjänsten och portalen och välj Sycronization regler och klicka på ny.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Välj ”Skapa resurs i FIM” ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Flödet regler:

| **Inledande flöde** | **Använd existens test** | **Flöda (FIM värdet ⇒ Målattributet)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName] (javascript:void(0);)                        |
|                       |                           | [Vänster (id 20) ⇒accountName] (javascript:void(0);)                        |
|                       |                           | [id⇒uid] (javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType] (javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [surname⇒sn] (javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
|                       |                           | [id⇒cn] (javascript:void(0);)                                          |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone] (javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Synkroniseringsregeln: Skapa gäst användarkonto i Active Directory 

Den här synkroniseringsregeln skapar användaren i active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Flödet regler:

| **Inledande flöde** | **Använd existens test** | **Flöda (FIM värdet ⇒ Målattributet)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName] (javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [sn⇒sn] (javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
| **Y**                 |                           | [”CN =” + uid + ”, OU = B2BGuest, DC = scontoso, DC = com” ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd] (javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl] (javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Synkroniseringsregeln: Importera B2B gästen användaren objekt SID för inloggning till MIM 

Den här synkroniseringsregeln skapar användaren i active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Till sist ska vi bjuda in användaren och kör sedan hanteringen i följande ordning:

-   Fullständig Import och synkronisering på hanteringsagenten MIMMA

-   Fullständig Import och synkronisering på ADMA_SCONTOSO_B2B hanteringsagenten

-   Fullständig Import och synkronisering på hanteringsagenten B2B diagram

-   Export och Deltaimport synkronisering på ADMA_SCONTOSO_B2B hanteringsagenten

-   Export och Deltaimport synkronisering på hanteringsagenten MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Slutligen: Application Proxy med B2B Gäst och logga in i MIM

Nu när vi har skapat synkroniseringsreglerna i MIM. I appen proxykonfigurationen definierar använda molnet principen för KCD på app-proxy.
Dessutom sedan lägga till användaren manuellt till Hantera användare och grupper. Alternativen inte för användaren tills skapa har uppstått i MIM att lägga till gäst i en office-grupp som en gång etablerats kräver lite mer konfiguration som inte beskrivs i det här dokumentet.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Inledande flöde** | **Använd existens test** | **Flöda (FIM värdet ⇒ Målattributet)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName] (javascript:void(0);)                     |
|                       |                           | [”CONTOSO” ⇒domain] (javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid] (javascript:void(0);)                                      |

När alla konfigurera

Slutligen har B2B användarinloggning och se hur programmet

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Nästa steg
----------

[Hur etablerar jag användare i AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Funktionsreferens för FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Hur du ger säker fjärråtkomst till lokala program](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Ladda ned Microsoft Identity Manager management agent för Microsoft Graph (förhandsgranskning)](http://go.microsoft.com/fwlink/?LinkId=717495)
