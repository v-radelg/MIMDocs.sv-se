---
title: Planera en skyddsmiljö | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f8fd71d2244760d3a6561c6f55bf676e6f42561a
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380076"
---
# <a name="planning-a-bastion-environment"></a>Planera en skyddsmiljö

Genom att lägga till en skyddsmiljö med en dedikerad administrativ skog för Active Directory kan organisationer enkelt hantera administratörskonton, arbetsstationer och grupper i en miljö med starkare säkerhetskontroller än den befintliga produktionsmiljön.

Den här arkitekturen möjliggör ett antal kontroller som inte är möjliga eller enkla att konfigurera i en arkitektur med en enda skog. Det omfattar att tilldela konton som vanliga icke-privilegierade användare i den administrativa skogen, vilka är mycket privilegierade i produktionsmiljön, vilket möjliggör större teknisk tillämpning av styrningen. Den här arkitekturen möjliggör också användning av funktionen för selektiv autentisering av ett förtroende, vilket gör att det går att begränsa inloggning (och exponering av autentiseringsuppgifter) till enbart auktoriserade värdar. När du behöver en högre säkerhetsnivå för produktionsskogen men vill slippa kostnaden och komplexiteten för en fullständig ombyggnation, kan du få en miljö som höjer säkerhetsnivån i produktionsmiljön med hjälp av en administrativ skog.

Annan teknik kan användas vid sidan av den dedikerade administrativa skogen. Sådan teknik kan bland annat begränsa platserna där administratörers autentiseringsuppgifter exponeras, begränsa användarnas rollprivilegier i skogen och säkerställa att administrativa uppgifter inte utförs på värdar som används för standardanvändares aktiviteter (till exempel e-postanvändning och webbsurfande).

## <a name="best-practice-considerations"></a>Rekommenderade överväganden

En dedikerad administrativ skog är en Active Directory-standardskog med en domän som används för hantering av Active Directory. En fördel med att använda administrativa skogar och domäner är att de kan ha fler säkerhetsåtgärder än produktionsskogar på grund av sina begränsade användningsområden. Eftersom den här dedikerade skogen är avskild från och inte har förtroende för organisationens befintliga skogar påverkas den dessutom inte av ett säkerhetsintrång i en annan skog.

Vid utformningen av en administrativ skog ingår följande överväganden:

### <a name="limited-scope"></a>Begränsat omfång

Fördelen med en administrativ skog är den höga säkerhetsnivån och minskade attackytan. Skogen kan innehålla ytterligare hanteringsfunktioner och program, men varje ökning i omfattningen ökar risken för angrepp på skogen och dess resurser. Målet är att begränsa skogens funktioner för att hålla risken för angrepp på lägsta nivå.

Enligt [nivåmodellen](tier-model-for-partitioning-administrative-privileges.md) för partitionering av administratörsbehörighet bör konton i en dedikerad administrativ skog vara på en enda nivå, vanligtvis nivå 0 eller 1. Om en skog ligger på nivå 1 ska du överväga att begränsa den till ett bestämt programområde (till exempel ekonomiappar) eller en bestämd användargrupp (till exempel IT-underleverantörer).

### <a name="restricted-trust"></a>Begränsat förtroende

*CORP*-skogen för produktion bör ha förtroende för den administrativa *PRIV*-skogen, men inte tvärtom. Det kan vara ett domänförtroende eller ett skogsförtroende. Den administrativa skogens domän behöver inte ha förtroende för hanterade domäner och skogar för att hantera Active Directory, men för ytterligare program kan det krävas en dubbelriktad förtroenderelation, säkerhetsvalidering och testning.

Selektiv autentisering bör användas för att säkerställa att kontona i den administrativa skogen endast använder rätt produktionsvärdar. För att hantera domänkontrollanter och delegera behörigheter i Active Directory kräver detta att behörigheten "Tillåts att logga in" på domänkontrollanter tilldelas angivna administratörskonton på nivå 0 i den administrativa skogen. I [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc816580.aspx) (Konfigurera inställningar för selektiv autentisering) finns mer information.

## <a name="maintain-logical-separation"></a>Upprätthålla logisk uppdelning

Du säkerställer att skyddsmiljön inte påverkas av befintliga eller framtida säkerhetsincidenter i organisationens Active Directory genom att följa de här riktlinjerna när du förbereder system för skyddsmiljön:

- Windows-servrar får inte vara domänanslutna eller använda distribuerad programvara eller distribuerade inställningar från den befintliga miljön.

- Skyddsmiljön måste innehålla egna Active Directory Domain Services som tillhandahåller skyddsmiljön med Kerberos och LDAP, DNS, tid och tidstjänster.

- MIM ska inte använda en SQL-databasgrupp i den befintliga miljön. Sedan ska SQL Server distribueras på dedicerade servrar i skyddsmiljön.

- För skyddsmiljön krävs Microsoft Identity Manager 2016, särskilt MIM-tjänsten och PAM-komponenterna måste distribueras.

- Säkerhetskopior av programvara och media för skyddsmiljön måste hållas separat från de i system i befintliga skogar så att en administratör i den befintliga skogen inte kan ändra en säkerhetskopia för skyddsmiljön.

- Användare som hanterar servrarna i skyddsmiljön måste logga in från arbetsstationer som inte är tillgängliga för administratörer i den befintliga miljön så att autentiseringsuppgifterna för skyddsmiljön inte sprids.

## <a name="ensure-availability-of-administration-services"></a>Säkerställa tillgängligheten för administrativa tjänster

I och med att administrationen av program överförs till skyddsmiljön måste du överväga hur du ska ge tillräcklig tillgänglighet för att uppfylla kraven för programmen. Tekniken omfattar:

- Distribuera Active Directory Domain Services på flera datorer i skyddsmiljön. Det krävs minst två för att säkerställa löpande autentisering också när en server startas om tillfälligt för schemalagt underhåll. Det kan krävas fler datorer för högre belastningar och för att hantera geografiskt spridda resurser och administratörer.

- Förbered nödkonton i den befintliga skogen och den dedikerade administrationsskogen för nödfall.

- Distribuera SQL Server och MIM-tjänsten på flera datorer i skyddsmiljön.

- Skapa en säkerhetskopia av AD och SQL för varje ändring av användare och rolldefinitioner i den dedikerade administrationsskogen.

## <a name="configure-appropriate-active-directory-permissions"></a>Konfigurera lämpliga Active Directory-behörigheter

Den administrativa skogen ska konfigureras för det lägsta privilegium som krävs för Active Directory-administration.

- Konton i den administrativa skogen som används för att administrera produktionsmiljön bör inte beviljas administratörsbehörighet för den administrativa skogen, domäner i den eller arbetsstationer i den.

- Administratörsbehörigheter över själva den administrativa skogen bör vara hårt styrda av en offlineprocess för att minska externa och interna angripares möjlighet att radera granskningsloggar. Detta hjälper också att se till att personal med konton för produktionsadministration inte kan minska begränsningarna för sina konton och öka risken för organisationen.

- Den administrativa skogen bör följa Microsoft Security Compliance Managers (SCM) konfigurationer för domänen, inklusive ordentliga konfigurationer för autentiseringsprotokoll.

När du skapar skyddsmiljön, innan du installerar Microsoft Identity Manager, identifierar och skapar du de konton som ska användas för administration i miljön. Det innefattar:

- **Nödkonton** ska bara kunna logga in på domänkontrollanter i skyddsmiljön.

- **Nödfallsadministratörer** etablerar andra konton och utför oplanerat underhåll. De här kontona tilldelas ingen behörighet till befintliga skogar eller system utanför skyddsmiljön. Autentiseringsuppgifterna, t.ex. smartkort, bör skyddas fysiskt och användning av konton bör loggas.

- **Tjänstkonton** som krävs av Microsoft Identity Manager, SQL Server och annan programvara.

## <a name="harden-the-hosts"></a>Stärk säkerheten för värdarna

Alla värdar, inklusive domänkontrollanter, servrar och arbetsstationer som är anslutna till den administrativa skogen ska ha de senaste operativsystemen och Service Pack installerade och uppdaterade.

- De program som krävs för administration ska finnas på alla arbetsstationer så att kontona som använder dem inte måste finnas i den lokala administratörsgruppen för att installera dem. Underhållet av domänkontrollanter kan vanligtvis utföras med RDP och verktyg för fjärrserveradministration.

- Värdarna för de administrativa skogarna ska uppdateras automatiskt med säkerhetsuppdateringar. Även om det kan skapa risk för att underhåll av domänkontrollanter avbryts minskar det säkerhetsriskerna betydligt för okorrigerade säkerhetsproblem.

### <a name="identify-administrative-hosts"></a>Identifiera administrativa värdar

Risken för ett system eller en arbetsstation mäts efter den uppgift med högst risk som utförs, till exempel internet- och e-postanvändning eller användning av andra program som behandlar okänt eller osäkert innehåll.

Administrativa värdar omfattar följande datorer:

- En dator på vilken autentiseringsuppgifter för administratören anges eller skrivs in.

- Administrativa fjärrservrar på vilka administrativa sessioner och verktyg körs.

- Alla värdar på vilka administrativa uppgifter utförs, inklusive de som använder en vanlig användardator som kör en RDP-klient till att fjärradministrera servrar och program.

- Servrar som är värdar för program som måste administreras, och kan kommas åt via RDP med begränsat-adminläge eller Windows PowerShell-fjärrkommunikation.

### <a name="deploy-dedicated-administrative-workstations"></a>Distribuera dedikerade administrativa arbetsstationer

Även om det kan vara osmidigt kan det krävas separata arbetsstationer med förstärkt säkerhet som är speciellt avsedda för användare med autentiseringsuppgifter för administration med hög inverkan. Det är viktigt att ange en värd med en säkerhetsnivå som är lika med eller större än behörighetsnivån som tilldelats autentiseringsuppgifterna. Överväg att använda följande åtgärder för ytterligare skydd:

- **Kontrollera att alla media i bygget är rena** för att minska risken för att skadlig kod installeras i en huvudavbildning eller införs i en installationsfil som laddas ned och lagras.

- Startkonfigurationerna bör utgå från **baslinjer för säkerhet**. Med Microsoft Security Compliance Manager (SCM) kan du konfigurera baslinjer på administrativa värdar.

- **Säker Start** skyddar mot angripare och skadlig kod som försöker läsa in osignerad kod i startprocessen.

- **Begränsning av programvara** så att endast auktoriserade administrativa program körs på de administrativa värdarna. Kunder kan använda AppLocker för den här uppgiften med en godkänd lista över auktoriserade program och på så sätt förhindra att skadlig programvara och obehöriga program körs.

- **Fullständig volymkryptering** skyddar mot fysisk förlust av datorer, som bärbara administrationsdatorer för fjärranvändning.

- **USB-begränsningar** skyddar mot fysiska angrepp.

- **Nätverksisolering** skyddar mot nätverksattacker och oavsiktliga administrativa åtgärder. Värdens brandväggar ska blockera alla inkommande anslutningar utom de som uttryckligen krävs och blockera all onödig utgående internetåtkomst.

- **Program mot skadlig kod** skyddar mot kända hot och skadlig kod.

- **Skydd mot trojaner** skyddar mot okända hot och trojaner, bland annat Enhanced Mitigation Experience Toolkit (EMET).

- **Analys av attackyta** förhindrar att nya attackvektorer införs i Windows när ny programvara installeras. Verktyg som Attack Surface Analyzer (ASA) hjälper dig att utvärdera konfigurationsinställningar på en värd och identifiera attackvektorer som införs med programvara eller ändringar av konfigurationen.

- **Administrativ behörighet** bör inte ges till användare på deras lokala datorer.

- **RestrictedAdmin-läge** för utgående RDP-sessioner, utom när de krävs för rollen. Mer information finns i [Vad är nytt i fjärrskrivbordstjänster i Windows Server](https://technet.microsoft.com/library/dn283323.aspx).

Vissa av de här åtgärderna kan verka extrema, men de senaste åren har flera rapporter visat på de avancerade verktyg skickliga angripare använder mot sina mål.

## <a name="prepare-existing-domains-to-be-managed-by-the-bastion-environment"></a>Förbereda befintliga domäner som ska hanteras i skyddsmiljön

MIM använder PowerShell-cmdletar till att upprätta förtroende mellan de befintliga AD-domänerna och den dedikerade administrativa skogen i skyddsmiljön. När skyddsmiljön har distribuerats och innan några användare eller grupper konverteras till JIT måste `New-PAMTrust`- och `New-PAMDomainConfiguration`-cmdletarna uppdatera domänförtroenderelationer och skapa de artefakter som behövs för AD och MIM.

När den befintliga Active Directory-topologin ändras kan cmdletarna `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` och `Remove-PAMDomainConfiguration` användas till att uppdatera förtroenderelationer.

## <a name="establish-trust-for-each-forest"></a>Upprätta förtroende för varje skog

`New-PAMTrust`-cmdleten måste köras en gång per befintlig skog. Den anropas på MIM-tjänstdatorn i administrationsdomänen. Parametrarna för det här kommandot är domännamnet för den översta domänen i den befintliga skogen och autentiseringsuppgifterna för en administratör i domänen.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

När du har upprättat förtroendet konfigurerar du varje domän för att aktivera hantering från skyddsmiljön enligt beskrivningen i nästa avsnitt.

## <a name="enable-management-of-each-domain"></a>Aktivera hantering av varje domän

Det finns sju krav för att aktivera hantering för en befintlig domän.

### <a name="1-a-security-group-on-the-local-domain"></a>1. En säkerhetsgrupp i den lokala domänen

Det måste finnas en grupp i den befintliga domänen, vars namn är NetBIOS-domännamnet följt av tre dollartecken, t.ex. *CONTOSO$$$*. Gruppomfånget måste vara *domänlokal* och grupptypen måste vara *säkerhet*. Det krävs för att grupper ska kunna skapas i den dedikerade administrativa skogen med samma säkerhetsidentifierare som grupper i domänen. Skapa den här gruppen med följande PowerShell-kommando, som utförs av en administratör för den befintliga domänen och körs på en arbetsstation som är ansluten till den befintliga domänen:

```PowerShell
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### <a name="2-success-and-failure-auditing"></a>2. Granskning av lyckade och misslyckade åtgärder

Grupprincipinställningarna på domänkontrollanten för granskning måste innehålla granskning av både misslyckade och lyckade åtgärder för Granska kontohantering och Granska katalogtjänståtkomst. Det kan göras av en administratör för den befintliga domänen med konsolen Grupprinciphantering och sedan köras på en arbetsstation som är ansluten till den befintliga domänen:

3. Gå till **Start** > **Administrationsverktyg** > **Grupprinciphantering**.

4. Gå till **Skog: contoso.local** > **Domäner** > **contoso.local** > **Domänkontrollanter** > **Standardprincip för domänkontrollanter**. Ett informationsmeddelande visas.

    ![Standardprincip för domänkontrollanter – skärmbild](media/pam-group-policy-management.jpg)

5. Högerklicka på **Standardprincip för domänkontrollanter** och välj **Redigera**. Ett nytt fönster visas.

6. I fönstret Redigeraren Grupprinciphantering under trädet Standardprincip för domänkontrollanter går du till **Datorkonfiguration** > **Principer** > **Windows-inställningar** > **Säkerhetsinställningar** > **Lokala principer** > **Granskningsprincip**.

    ![Redigeraren Grupprinciphantering – skärmbild](media/pam-group-policy-management-editor.jpg)

5. Högerklicka på **Granska kontohantering** i informationsfönstret och välj **Egenskaper**. Välj **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade** och klicka på **Tillämpa** och **OK**.

6. Högerklicka på **Granska katalogtjänståtkomst** i informationsfönstret och välj **Egenskaper**. Välj **Definiera följande principinställningar**, markera kryssrutorna **Lyckade** och **Misslyckade** och klicka på **Tillämpa** och **OK**.

    ![Principinställningarna Lyckade och Misslyckade – skärmbild](media/pam-group-policy-management-editor2.jpg)

7. Stäng fönstren Redigeraren Grupprinciphantering och Grupprinciphantering. Tillämpa granskningsinställningarna genom att öppna ett PowerShell-fönster och skriva:

    ```cmd
    gpupdate /force /target:computer
    ```

Meddelandet ”Uppdatering av grupprincip har slutförts”. bör visas efter några minuter.

### <a name="3-allow-connections-to-the-local-security-authority"></a>3. Tillåt anslutningar till den lokala säkerhetskontrollen

Domänkontrollanterna måste tillåta RPC över TCP/IP-anslutningar för lokal säkerhetskontroll (LSA) från skyddsmiljön. TCP/IP-stöd i LSA måste aktiveras i registret på äldre versioner av Windows Server:

```PowerShell
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### <a name="4-create-the-pam-domain-configuration"></a>4. Skapa PAM-domänens konfiguration

`New-PAMDomainConfiguration`-cmdleten måste köras på MIM-tjänstdatorn i administrationsdomänen. Parametrarna för det här kommandot är domännamnet för den befintliga domänen och autentiseringsuppgifterna för en administratör i domänen.

```PowerShell
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### <a name="5-give-read-permissions-to-accounts"></a>5. Bevilja läsbehörighet för konton

De konton i skyddsskogen som används till att skapa roller (administratörer som använder cmdletarna `New-PAMUser` och `New-PAMGroup`), samt det konto som används av MIM-övervakningstjänsten måste ha läsbehörighet i domänen.

Följande steg ger läsbehörighet för användaren *PRIV\Administratör* till domänen *Contoso* inom domänkontrollanten *CORPDC*:

1. Kontrollera att du är inloggad i CORPDC som domänadministratör för Contoso (till exempel Contoso\Administratör).

2. Starta Active Directory – användare och datorer.

3. Högerklicka på domänen **contoso.local** och välj **Delegera kontroll**.

4. På fliken Valda användare och grupper klickar du på **Lägg till**.

5. I popup-fönstret Välj användare, datorer eller grupper klickar du på **Platser** och ändrar platsen till *priv.contoso.local*. På objektnamnet skriver du *Domänadministratörer* och klickar på **Kontrollera namn**. När ett popup-fönster visas anger du användarnamnet *priv\administratör* och lösenordet.

6. Efter Domänadministratörer skriver du *; MIMMonitor*. När namnen Domänadministratörer och MIMMonitor är understrukna klickar du på **OK** och sedan på **Nästa**.

7. I listan med vanliga uppgifter väljer du **Läsa all användarinformation** och klickar på **Nästa** och **Slutför**.

18. Stäng Active Directory – användare och datorer.

### <a name="6-a-break-glass-account"></a>6. Ett nödkonto

Om målet med hanteringen av privilegierad åtkomst är att minska antalet konton med domänadministratörsbehörighet som är permanent tilldelade för domänen måste det också finnas ett *nödkonto* i domänen om det senare skulle uppstå problem i förtroenderelationen. Konton för nödåtkomst till produktionsskogen ska finnas i varje domän och ska bara kunna logga in på domänkontrollanter. För organisationer med flera platser, kan det krävas ytterligare konton för redundans.

### <a name="7-update-permissions-in-the-bastion-environment"></a>7. Uppdatera behörigheter i skyddsmiljön

Granska behörigheterna i objektet *AdminSDHolder* i systemcontainern i den domänen. Objektet *AdminSDHolder* har en unik åtkomstkontrollista (ACL) som används till att kontrollera behörigheterna för säkerhetsobjekt som är medlemmar i inbyggda privilegierade Active Directory-grupper. Observera om det har gjorts ändringar i standardbehörigheterna som påverkar användare med administratörsbehörighet i domänen, eftersom de behörigheterna inte gäller för användare vars konton finns i skyddsmiljön.

## <a name="select-users-and-groups-for-inclusion"></a>Välja användare och grupper som ska ingå

Nästa steg är att definiera PAM-rollerna och associera de användare och grupper till vilka de ska ha åtkomst. Det här är vanligtvis en delmängd av användarna och grupperna för nivån som hanteras i skyddsmiljön. Mer information finns i [Definiera roller för Privileged Access Management](defining-roles-for-pam.md).
