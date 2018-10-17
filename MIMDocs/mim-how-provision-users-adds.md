---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: Gå igenom processen för att skapa användare i ADDS med Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 88473df88271937b07450df409353c0b3ca08684
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358796"
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Hur etablerar jag användare i AD DS

Gäller för: Microsoft Identity Manager 2016 SP1 (MIM)

Ett grundläggande krav för ett identitetshanteringssystem är möjligheten att etablera resurser till ett externt system.

Den här guiden leder dig igenom de viktigaste byggstenarna som ingår i processen för att etablera användare från Microsoft® Identity Manager (MIM) 2016 till Active Directory® Domain Services (AD DS), förklarar hur du kan verifiera att scenariot fungerar som förväntat, tillhandahåller förslag för att hantera Active Directory-användare med hjälp av MIM 2016 och beskriver ytterligare informationsresurser.

## <a name="before-you-begin"></a>Innan du börjar


I det här avsnittet hittar du information om det här dokumentets omfattning. Vägledningar är normalt riktade mot användare som redan har grundläggande kunskaper om processen för att synkronisera objekt med MIM enligt beskrivningen i de relaterade [guiderna för att komma igång](http://go.microsoft.com/FWLink/p/?LinkId=190486).

### <a name="audience"></a>Målgrupp


Den här guiden är avsedd för IT-experter som redan har grundläggande kunskaper om hur MIM-synkroniseringsprocessen fungerar och är intresserade av att få praktiska erfarenheter och mer konceptuell information om specifika scenarier.

### <a name="prerequisite-knowledge"></a>Förhandskunskaper


Det här dokumentet förutsätter att du har tillgång till en instans av MIM som körs och att du har erfarenhet av att konfigurera enkla synkroniseringsscenarier enligt beskrivning i följande dokument:

-   [Introduktion till ingående synkronisering](http://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [Introduktion till utgående synkronisering](http://go.microsoft.com/FWLink/p/?LinkId=189653)

Innehållet i det här dokumentet är avsett att fungera som ett tillägg till dessa grundläggande dokument.

### <a name="scope"></a>Omfång


Det scenario som beskrivs i det här dokumentet har förenklats för att bemöta kraven i en grundläggande laboratoriemiljö. Fokus ligger på att lära dig att de förstå begrepp och tekniker som beskrivs.

Det här dokumentet hjälper dig att utveckla en lösning som omfattar hantering av grupper i AD DS med hjälp av MIM.

### <a name="time-requirements"></a>Tidskrav


Processerna i det här dokumentet tar 90 till 120 minuter att slutföra.

Denna tidsberäkning förutsätter att testmiljön redan är konfigurerad och omfattar inte den tid som krävs för att konfigurera testmiljön.

### <a name="getting-support"></a>Få support


Om du har frågor om innehållet i detta dokument eller om du har allmänna kommentarer som du vill diskutera kan du lägga upp ett meddelande i [forumet för Forefront Identity Manager 2010](http://go.microsoft.com/FWLink/p/?LinkId=189654).

## <a name="scenario-description"></a>Scenario-beskrivning


Fabrikam, ett fiktivt företag, planerar att använda MIM för att hantera användarkonton i företagets AD DS med hjälp av MIM. Som en del av den här processen måste Fabrikam etablera användare i AD DS. För att starta den första testningen har Fabrikam installerat en grundläggande laboratoriemiljö som består av MIM och AD DS.
I den här laboratoriemiljön testar Fabrikam ett scenario som består av en användare som skapades manuellt i MIM-portalen. Målet med det här scenariot är att etablera användaren som en aktiverad användare med ett fördefinierat lösenord i AD DS.

## <a name="scenario-design"></a>Utformning av scenario


Om du vill använda den här guiden behöver du tre strukturella komponenter:

-   Active Directory-domänkontrollant

-   En dator som kör FIM-synkroniseringstjänsten
-   En dator som kör FIM-portalen

Följande bild visar den miljö som krävs.

![Obligatorisk miljö](media/how-provision-users-adds/image001.png)


Du kan köra alla komponenter på en dator.

> [!NOTE]
> Mer information om hur du konfigurerar MIM finns i [FIM-installationsguiden](http://go.microsoft.com/FWLink/p/?LinkId=165845).

## <a name="scenario-components-list"></a>Lista över scenariokomponenter


Följande tabell visar de komponenter som är en del av scenariot i denna guide.

| ![Organisationsenhet](media/how-provision-users-adds/image005.jpg)   | Organisationsenhet                | MIM-objekt – organisationsenhet (OU) som används som ett mål för de etablerade användarna.                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Användarkonton](media/how-provision-users-adds/image006.jpg)   | Användarkonton                      | &#183; **ADMA** – Active Directory-användarkonto med behörighet att ansluta till AD DS.<br/> &#183; **FIMMA** – Active Directory-användarkonto med behörighet att ansluta till MIM.
                                                                 |
| ![Hanteringsagenter och körningsprofiler](media/how-provision-users-adds/image007.jpg)  | Hanteringsagenter och körningsprofiler | &#183; **Fabrikam ADMA** – hanteringsagenten som utbyter data med AD DS. <br/> &#183; Fabrikam FIMMA – hanteringsagenten som utbyter data med MIM.                                                                                 |
| ![Synkroniseringsregler](media/how-provision-users-adds/image008.jpg)  | Synkroniseringsregler              | Utgående synkroniseringsregel för Fabrikam-grupp – utgående synkroniseringsregel som etablerar användare i AD DS.                                     |
| ![Uppsättningar](media/how-provision-users-adds/image009.jpg)   | Uppsättningar                               | Alla leverantörer – uppsättning med dynamiskt medlemskap för alla objekt med EmployeeType-attributvärdet Leverantör.                                |
| ![Arbetsflöden](media/how-provision-users-adds/image010.jpg)  | Arbetsflöden                          | AD-etableringsarbetsflöde – arbetsflöde för att sätta MIM-användaren i omfånget för den utgående synkroniseringsregeln för AD.                                |
| ![Hantering av principregler](media/how-provision-users-adds/image011.jpg)   | Hantering av principregler            | Hanteringsprincipregel för AD-etablering – hanteringsprincipregel (MPR) som utlöses när en resurs blir medlem i uppsättningen Alla leverantörer. |
| ![MIM-användare](media/how-provision-users-adds/image012.jpg) | MIM-användare                          | Britta Simon – MIM-användare som du etablerar i AD DS.                                                                                             |



## <a name="scenario-steps"></a>Scenarioanvisningar


Scenariot som beskrivs i den här handledningen består av de byggstenar som visas i följande bild.

![Scenarioanvisningar](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Konfigurera de externa systemen


I det här avsnittet finns anvisningar för de resurser som du behöver skapa som ligger utanför MIM-miljön.

### <a name="step-1-create-the-ou"></a>Steg 1: Skapa organisationsenheten


Du behöver organisationsenheten som en container för den etablerade exempelanvändaren. Mer information om hur du skapar organisationsenheter finns i [Skapa en ny organisationsenhet](http://go.microsoft.com/FWLink/p/?LinkId=189655).

Skapa en organisationsenhet som heter MIMObjects i AD DS.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Steg 2: Skapa Active Directory-användarkonton

För scenariot i den här handledningen behöver du två Active Directory-användarkonton:

- **ADMA** – används av Active Directory-hanteringsagenten.

- **FIMMA** – används av FIM-tjänstens hanteringsagent.

I båda fallen räcker det att skapa vanliga användarkonton. Mer information om de specifika kraven för båda kontona finns senare i det här dokumentet. Mer information om hur du skapar organisationsenheter finns i [Skapa ett nytt användarkonto](http://go.microsoft.com/FWLink/p/?LinkId=189656).


## <a name="configuring-the-fim-synchronization-service"></a>Konfigurera FIM-synkroniseringstjänsten


För konfigurationsstegen i det här avsnittet måste du starta hanteraren för FIM-synkroniseringstjänsten.

### <a name="creating-the-management-agents"></a>Skapa hanteringsagenterna

För scenariot i den här handledningen måste du skapa två hanteringsagenter:

-   **Fabrikam ADMA** – hanteringsagent för AD DS.

-   **Fabrikam FIMMA** – hanteringsagent för FIM-tjänstens hanteringsagent.

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Steg 3: Skapa Fabrikam ADMA-hanteringsagenten

När du konfigurerar en hanteringsagent för AD DS måste du ange ett konto som används av hanteringsagenten i datautbytet med AD DS. Du bör använda ett vanligt användarkonto. För att importera data från AD DS måste dock kontot har behörighet att avsöka ändringar från DirSync-kontrollen. Om du vill att hanteringsagenten ska exportera data till AD DS måste du ge kontot tillräcklig behörighet i målets organisationsenheter. Mer information om det här ämnet finns i [Konfigurera ADMA-kontot](http://go.microsoft.com/FWLink/p/?LinkId=189657).

Om du vill skapa en användare i AD DS måste du flöda ut objektets DN. Utöver detta är det bra att flöda förnamn, efternamn och visningsnamn för att se till att objekten är synliga.

I AD DS är det fortfarande vanligt att användarna använder samma sAMAccountName-attribut för att logga in på katalogtjänsten. Om du inte anger ett värde för det här attributet genererar katalogtjänsten ett slumpmässigt värde för det. Dessa slumpmässiga värden är dock inte användarvänliga, och därför är en användarvänlig version av det här attributet normalt en del av en export till AD DS. För att en användare ska kunna logga in på AD DS måste du även inkludera ett lösenord som skapas med hjälp av unicodePwd-attributet i exportlogiken.

> [!Note]
> Kontrollera att värdet som du anger som unicodePwd uppfyller lösenordsprinciperna för målets AD DS.

När du anger ett lösenord för AD DS-konton måste du också skapa ett konto som ett aktiverat konto. Det gör du genom att konfigurera userAccountControl-attributet. Mer information om userAccountControl-attributet finns i [Använda FIM för att aktivera eller inaktivera konton i Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189658).

I följande tabell visas de viktigaste scenariospecifika inställningarna som du behöver konfigurera.

| Sida för hanteringsagentens designer                          | Konfiguration                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Skapa hanteringsagent                                 | 1. **Hanteringsagent för:** AD DS  <br/> 2.  **Namn:** Fabrikam ADMA |
| Anslut till Active Directory-skogen                      | 1. **Välj katalogpartitioner:** "DC=Fabrikam, DC=com"   <br/>   2. Klicka på **Container** för att öppna dialogrutan **Välj container** och se till att **MIMObjects** är den enda organisationsenhet som är markerad.        |
| Välj objekttyper                                     | Utöver de objekttyperna som redan har valts väljer du **användare.** |
| Välj attribut                                       | 1. Klicka på **Visa alla.** <br/>   2. Välj följande attribut: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

Mer information finns i följande avsnitt i hjälpen:
- Skapa en hanteringsagent
- Ansluta till en Active Directory-skog
- Använda hanteringsagenten för Active Directory
- Konfigurera katalogpartitioner

> [!Note]
> Kontrollera att du har en attributflödesregel för import konfigurerad för ExpectedRulesList-attributet.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Steg 4: Skapa Fabrikam FIMMA-hanteringsagenten

När du konfigurerar en hanteringsagent för FIM-tjänsten måste du ange ett konto som används av hanteringsagenten i datautbytet med FIM-tjänsten.

Du bör använda ett vanligt användarkonto. Kontot måste vara samma konto som du angav under installationen av MIM. Information om ett skript som du kan använda för att fastställa namnet på FIMMA-kontot som du angav under konfigurationen och för att testa om detta konto fortfarande är giltigt finns i Använda Windows PowerShell för att utföra ett [snabbtest för FIM MA-kontokonfiguration](http://go.microsoft.com/FWLink/p/?LinkId=189659).

I följande tabell visas de viktigaste scenariospecifika inställningarna som du behöver konfigurera. Skapa hanteringsagenten baserat på informationen i tabellen nedan.  

| Sida för hanteringsagentens designer | Konfiguration |
|------------|------------------------------------|
| Skapa hanteringsagent | 1. **Hanteringsagent för:** FIM-tjänstens hanteringsagent <br/> 2. **Namn:** Fabrikam FIMMA |
| Anslut till databas     | Använd följande inställningar: <br/> &#183; **Server:** localhost <br/> &#183; **Databas:** FIMService <br/> &#183;**FIM-tjänstens basadress:** http://localhost:5725 <br/> <br/> Ange information om kontot som du skapade för den här hanteringsagenten |
| Välj objekttyper                                     | Utöver de objekttyper som redan har valts väljer du **Person.**   |
| Konfigurera objekttypsmappningar                          | Utöver de objekttypsmappningar som redan finns lägger du till en mappning för personen **Typ av datakällobjekt** till **Metaversum**-objekttyppersonen. |
| Konfigurera attributflöde                                | Utöver de mappningar av attributflöde som redan finns lägger du till följande mappningar av attributflöde: <br/><br/> ![Attributflöde](media/how-provision-users-adds/image018.jpg) |




Mer information finns i följande avsnitt i hjälpen:
-   Skapa en hanteringsagent

-   Ansluta till en Active Directory-databas

-   Använda hanteringsagenten för Active Directory

-   Konfigurera katalogpartitioner

> [!NOTE]
>  Kontrollera att du har en attributflödesregel för import konfigurerad för ExpectedRulesList-attributet.

### <a name="step-5-create-the-run-profiles"></a>Steg 5: Skapa körningsprofilerna

I följande tabell visas de körningsprofiler som du måste skapa för scenariot i den här handledningen.

| Hanteringsagenten  | Körningsprofil     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1. Fullständig import  <br/> 2. Fullständig synkronisering <br/> 3. Deltaimport <br/> 4. Deltasynkronisering <br/> 5. Exportera                                                                    |
| Fabrikam FIMMA   | 1. Fullständig import <br> 2. Fullständig synkronisering <br/> 3. Deltaimport <br/> 4. Deltasynkronisering <br/> 5. Exportera|                                                                                                                                                                                   

Skapa körningsprofiler för varje hanteringsagent enligt föregående tabell.


> [!Note]
> Mer information finns i Skapa en körningsprofil för hanteringsagenten i MIM-hjälpen.                                                                                                                  
> 
> 
> [!Important]
>  Kontrollera att etablering är aktiverad i miljön. Du kan göra detta genom att köra skriptet, använda Windows PowerShell för att aktivera etablering (http://go.microsoft.com/FWLink/p/?LinkId=189660).


## <a name="configuring-the-fim-service"></a>Konfigurera FIM-tjänsten


För scenariot i den här handledningen måste du konfigurera en etableringsprincip på det sätt som visas i följande bild.

![Etablering av princip](media/how-provision-users-adds/image019.png)

Målet med den här etableringsprincipen är att föra in grupper i omfattningen för den utgående synkroniseringsregeln för AD-användare. Genom att föra in resursen i omfattningen för synkroniseringsregeln aktiverar du synkroniseringsmotorn så att den kan etablera resursen till AD DS i enlighet med din konfiguration.

Om du vill konfigurera FIM-tjänsten, navigera Windows Internet Explorer® till http://localhost/identitymanagement. På sidan för MIM-portalen skapar du etableringsprincipen genom att gå till de relaterade sidorna från avsnittet Administration. Du kan kontrollera konfigurationen genom att köra skriptet i [Using Windows PowerShell to document your provisioning policy configuration](http://go.microsoft.com/FWLink/p/?LinkId=189661) (Använda Windows PowerShell för att dokumentera konfiguration av etableringsprincipen).

### <a name="step-6-create-the-synchronization-rule"></a>Steg 6: Skapa synkroniseringsregeln

Följande tabeller visar konfiguration av synkroniseringsregeln för etablering som krävs för Fabrikam. Skapa synkroniseringsregeln enligt data i följande tabeller.

| Konfiguration av synkroniseringsregel                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Namn                                                                                                       | Utgående synkroniseringsregel för Active Directory-användare                         |                                                          
| Beskrivning                                                                                               |                                                                             |                                                           
| Prioritet                                                                                                | 2                                                                           |                                                           
| Dataflödesriktning   | Utgående             |       
| Beroende       |         |                                         


| Omfång |                                                                             |                                                           
|--------|-------|
| Resurstyp för metaversum | person |                                                         
| Externt system                   |Fabrikam ADMA                                                               |                                                       
| Resurstyp för externt system                                                                              | användare      



| Relation ||
|------------|---------|
| Skapa resurs i externt system                                                                         | Sant                                                                        |                                                           
| Aktivera avetablering                                                                                      | Falskt                                                                       |                                                           

| Relationsvillkor                                                                                      | |
|------------|----------|
| ILM-attribut     | Attribut för datakälla                                                       |
| Attribut för datakälla         | sAMAccountName    |

| Inledande utgående attributflöden        | |                                                             |
|-------------------|---------------------- |---------------|
| Tillåt null-värden                 | Mål                                                                 | Källa                                                    |
| falskt                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| falskt                       | userAccountControl                                                          | **Konstant:** 512                                         |
| falskt                                                                     | unicodePwd                    | Konstant: P\@\$\$W0rd                                    |

| Beständiga utgående attributflöden  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Tillåt null-värden                                                                                                | Mål                                                                 | Källa                                                    |
| falskt                                                                                                      | sAMAccountName                                                              | kontonamn                                               |
| falskt                                                                                                      | visningsnamn                                                                 | visningsnamn                                               |
| falskt                                                                                                      | förnamn                                                                   | förnamn                                                 |
| falskt                                                                                                      | sn                                                                          | efternamn                                                  |



> [!NOTE]
>  Viktigt. Kontrollera att du har valt Endast initialflöde för attributflödet som har DN som mål.                                                                          

### <a name="step-7-create-the-workflow"></a>Steg 7: Skapa arbetsflödet

Målet med AD-etableringsarbetsflödet är att lägga till synkroniseringsregeln för Fabrikam-etablering i en resurs. Följande tabeller visar konfigurationen.  Skapa ett arbetsflöde enligt data i tabellerna nedan.

| Arbetsflödeskonfiguration               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Namn                                 | Etableringsarbetsflöde för Active Directory-användare                     |
| Beskrivning                          |                                                                 |
| Arbetsflödestyp                        | Action                                                          |
| Kör vid principuppdatering                 | Falskt                                                           |

| Synkroniseringsregel                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Namn                                 | Utgående synkroniseringsregel för Active Directory-användare             |
| Action                               | Lägg till                                                             |




### <a name="step-8-create-the-mpr"></a>Steg 8: Skapa hanteringsprincipregel

Den hanteringsprincipregel (MPR) som krävs har typen Uppsättningsövergång och utlöses när en resurs blir medlem i uppsättningen Alla leverantörer. Följande tabeller visar konfigurationen.  Skapa MPR enligt data i tabellerna nedan.

| MPR-konfiguration                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Namn                                 | Hanteringsprincipregel för etablering av AD-användare                 |
| Beskrivning                          |                                                             |
| Typ                                 | Uppsättningsövergång                                              |
| Ger behörigheter                   | Falskt                                                       |
| Inaktiverad                             | Falskt                                                       |

| Definition av övergång                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Övergångstyp                      | Övergång in                                               |
| Övergångsuppsättning                       | Alla leverantörer                                             |

| Principarbetsflöden                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Typ                                 | Action                                                      |
| Visningsnamn                         | Etableringsarbetsflöde för Active Directory-användare                 |




## <a name="initializing-your-environment"></a>Initiera miljön


Målen för initieringsfasen är följande:

-   Föra in synkroniseringsregeln i metaversum.

-   Föra in Active Directory-strukturen i Active Directory-anslutarplatsen.

### <a name="step-9-run-the-run-profiles"></a>Steg 9: Kör körningsprofilerna

Följande tabell visar de körningsprofiler som ingår i initieringsfasen.  Kör körningsprofilerna enligt tabellen nedan.

| Kör                                                                                                           | Hanteringsagenten                                      | Körningsprofil          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | Fullständig import          |
| 2                                                                                                             |                                                       | Fullständig synkronisering |
| 3                                                                                                             |                                                       | Exportera               |
| 4                                                                                                             |                                                       | Deltaimport         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | Fullständig import          |
| 6                                                                                                             |                                                       | Fullständig synkronisering |



> [!NOTE]
> Du bör kontrollera att den utgående synkroniseringsregeln har projicerats till metaversum.

## <a name="testing-the-configuration"></a>Testa konfigurationen


Målet med det här avsnittet är att testa den faktiska konfigurationen. För att testa konfigurationen ska du:

1.  Skapa en exempelanvändare i FIM-portalen.

2.  Kontrollera etableringskraven för exempelanvändaren.

3.  Etablera exempelanvändaren i AD DS.

4.  Kontrollera att användaren finns i AD DS.

### <a name="step-10-create-a-sample-user-in-mim"></a>Steg 10: Skapa en exempelanvändare i MIM


I följande tabell visas egenskaperna för exempelanvändaren. Skapa en exempelanvändare enligt data i tabellen nedan.

| Attribut                              | Värde                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Förnamn                             | Britta                                                         |
| Efternamn                              | Simon                                                          |
| Visningsnamn                           | Britta Simon                                                   |
| Kontonamn                           | BSimon                                                         |
| Domain                                 | Fabrikam                                                       |
| Typ av anställd                          | Leverantör                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>Kontrollera etableringskraven för exempelanvändaren


Två krav måste uppfyllas för att etablera exempelanvändaren i AD DS:

1.  Användaren måste vara medlem i uppsättningen Alla leverantörer.

2.  Uppsättningsanvändaren måste finnas i omfattningen för den utgående synkroniseringsregeln.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Steg 11: Kontrollera att användaren är medlem i Alla leverantörer

Kontrollera om användaren är medlem i uppsättningen Alla leverantörer genom att öppna uppsättningen och sedan klicka på Visa medlemmar.

![Kontrollera att användaren är medlem i alla leverantörer](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Steg 12: Kontrollera att användaren finns i omfattningen för den utgående synkroniseringsregeln

Kontrollera om användaren finns i omfattningen för synkroniseringsregeln genom att öppna användarens egenskapssida och granska attributet Expected Rules List på fliken Etablering. Attributet Lista över förväntade regler ska innehålla AD-användaren

Utgående synkroniseringsregel. Följande skärmbild visar ett exempel på attributet Lista över förväntade regler.

![Status för synkroniseringsregel](media/how-provision-users-adds/image023.jpg)

Vid denna tidpunkt i processen är synkroniseringsregelns status Väntar. Det innebär att synkroniseringsregeln inte har verkställts på användaren än.



### <a name="step-13-synchronize-the-sample-group"></a>Steg 13: Synkronisera exempelgruppen


Innan du startar den första synkroniseringscykeln för ett testobjekt ska du spåra förväntat tillstånd för objektet efter varje körningsprofil som du kör i en testplan. Intill allmänt tillstånd för objektet (skapad, uppdaterad eller raderad) bör testplanen även innehålla de attributvärden du förväntar dig.
Använd testplanen för att verifiera förväntningarna på testplanen. Om ett steg inte returnerar förväntade resultat ska du inte fortsätta med nästa steg förrän du har löst skillnaden mellan det förväntade resultatet och det faktiska resultatet.

Du kan använda synkroniseringsstatistik som en första indikator för att verifiera förväntningarna. Om du till exempel förväntar dig att nya objekt ska mellanlagras i en anslutarplats, men importstatistiken inte returnerar några ”Lägger till” är det uppenbarligen något i miljön som inte fungerar som förväntat.

![Synkroniseringsstatistik](media/how-provision-users-adds/image024.jpg)

Även om synkroniseringsstatistiken kan ge en första indikation på om scenariot fungerar som förväntat kan du använda Search Connector Space och Metaverse-sökfunktionen i hanteraren för synkroniseringstjänsten för att verifiera de förväntade attributvärdena.

Synkronisera användaren till AD DS:

1.  Importera användaren till FIM MA-anslutningsplatsen.

2.  Projicera användaren till metaversum.

3.  Etablera användaren till Active Directory-anslutningsplatsen.

4.  Exportera statusinformation till FIM.

5.  Exportera användaren till AD DS.

6.  Bekräfta att den nya användaren har skapats.

Kör följande körningsprofiler för att utföra dessa uppgifter.

| Hanteringsagenten | Körningsprofil  |
|------------------|--------------|
| Fabrikam FIMMA   | 1. Deltaimport <br/> 2. Deltasynkronisering <br/> 3. Exportera <br/> 4. Deltaimport |
| Fabrikam FIMMA   | 1. Exportera <br/> 2. Deltaimport       |


Efter importen från FIM-tjänstdatabasen, Britta Simon och objektet ExpectedRuleEntry mellanlagras som länkar Britta till den utgående Synkroniseringsregeln för AD-användare i Fabrikam FIMMA-anslutningsplatsen. När du granskar Brittas egenskaper i anslutningsplatsen bredvid de attributvärden som du har konfigurerat i FIM-portalen hitta du också en giltig referens till objektet Expected Rule Entry. Följande skärm visar ett exempel på detta.

![Objektegenskaper för anslutarplats](media/how-provision-users-adds/image025.jpg)

Målet med körning av deltasynkronisering på Fabrikam FIMMA är att utföra flera åtgärder:

-   Projektion – det nya användarobjektet och det relaterade objektet för Expected Rule Entry projiceras in i metaversum.

-   Etablering – objektet Britta Simon som nyligen har projiceras etableras i anslutarplatsen för Fabrikam ADMA.

-   Exportera attributflöden – export av attributflöden sker på båda hanteringsagenterna. På Fabrikam ADMA fylls objektet Britta Simon som nyligen har etablerats på med nya attributvärden. På Fabrikam FIMMA uppdateras det befintliga objektet Britta Simon och det relaterade objektet ExpectedRuleEntry med attributvärden som är resultatet av projiceringen.

![Synkroniseringsstatistik](media/how-provision-users-adds/image026.jpg)

En etableringsaktivitet har redan skett i anslutarplatsen för Fabrikam ADMA, vilket syns i synkroniseringsstatistiken. När du granskar metaversumobjektegenskaperna för Britta Simon kan du se att denna aktivitet är resultatet av attributet ExpectedRulesList som har fyllts i med en giltig referens.

![egenskaper för metaversumobjekt](media/how-provision-users-adds/image027.jpg)

Under följande export på Fabrikam FIMMA uppdateras synkroniseringsregelstatus för Britta Simon från Väntar till Tillämpad, vilket visar att den utgående synkroniseringsregeln nu är aktiv på objektet i metaversum.

![Verkställd synkroniseringsregel](media/how-provision-users-adds/image028.jpg)

Eftersom ett nytt objekt har etablerats i ADMA-anslutarplatsen bör du ha en export med Lägg till väntande på denna hanteringsagent. Med hjälp av ett skript för detta syfte kan du se en rapporterad export med Lägg till väntande för Fabrikam ADMA. Om du vill använda skriptet kan du gå till [Using Windows PowerShell to Display the Export State of a Management Agent](http://go.microsoft.com/FWLink/p/?LinkId=189664) (Använda Windows PowerShell för att visa exporttillstånd för en hanteringsagent).

![Väntande exporter för hanteringsagent](media/how-provision-users-adds/image029.jpg)

I FIM kräver varje exportkörning en följande deltaimport att slutföra exportåtgärden. Deltaimporten som du kör efter en föregående exportkörning kallas en bekräftande import. Bekräftande rapporter krävs för att aktivera FIM-synkroniseringstjänsten så att den kan uppfylla lämpliga uppdateringskrav under efterföljande synkroniseringskörningar.


Kör körningsprofilerna enligt anvisningarna i det här avsnittet.

> [!IMPORTANT]
> Varje körning av körningsprofil måste slutföras utan fel.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Steg 14: Verifiera den etablerade användaren i AD DS

Kontrollera att exempelanvändaren har etablerats i AD DS genom att öppna organisationsenheten FIMObjects. Britta Simon bör finnas i organisationsenheten FIMObjects.

![verifiera att användaren finns i organisationsobjektet FIMObjects](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>Sammanfattning
=======

Syftet med det här dokumentet är att presentera de viktigaste byggstenarna för att synkronisera en användare i MIM med AD DS. Under det första testet bör du börja med det lägsta antalet attribut som krävs för att slutföra en uppgift och lägga till fler attribut i scenariot när de allmänna stegen fungerar som förväntat. Genom att se till att komplexiteten är så låg som möjligt förenklas felsökningsprocessen.

När du testar konfigurationen är det mycket troligt att du tar bort och skapar om nya testobjekt. För objekt med ett

ifyllt ExpectedRulesList-attribut kan det här leda till frånkopplade ERE-objekt.
En beskrivning av hur du kan ta bort dessa objekt från testmiljön finns i [En metod för att ta bort överblivna ExpectedRuleEntry-objekt från miljön](http://go.microsoft.com/FWLink/p/?LinkId=189667).

I ett normalt synkroniseringsscenario som omfattar AD DS som synkroniseringsmål är MIM inte auktoritativt för alla attribut för ett objekt. När du exempelvis hanterar användarobjekt i AD DS med hjälp av FIM måste domänen och objectSID-attributen som minst bidras till av AD DS-hanteringsagenten.
Kontonamn, domän och objectSID-attribut krävs om du vill göra det möjligt för en användare att logga in på FIM-portalen. En ytterligare ingående synkroniseringsregel krävs för AD DS-anslutarplatsen för att fylla i dessa attribut från AD DS. När du hanterar objekt med flera källor för attributvärden måste du se till att du konfigurerar attributflödets prioritet korrekt. Om attributflödets prioritet inte är korrekt konfigurerad blockerar synkroniseringsmotorn attribut från att fyllas i. Mer information om prioritet för attributflödet finns i artikeln [Om prioritet för attributflöde](http://go.microsoft.com/FWLink/p/?LinkId=189675).

<a name="see-also"></a>Se även
=========

<a name="other-resources"></a>Andra resurser
---------------

[Använda FIM för att aktivera eller inaktivera konton i Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189670)

[Om referensattribut](http://go.microsoft.com/FWLink/p/?LinkId=189671)

[Hur kan jag hantera mitt FIM MA-konto](http://go.microsoft.com/FWLink/p/?LinkId=189672)

[Identifiera icke-auktoritativa konton – Del 1: Visualisera](http://go.microsoft.com/FWLink/p/?LinkId=189673)

[Fattigmansversionen av en mekanism för anslutningsidentifiering](http://go.microsoft.com/FWLink/p/?LinkId=189674)

[Konfigurera ADMA-kontot](http://go.microsoft.com/FWLink/p/?LinkId=189657)

[En metod för att ta bort överblivna ExpectedRuleEntry-objekt från miljön](http://go.microsoft.com/FWLink/p/?LinkId=189667)

[Om prioritet för attributflöde](http://go.microsoft.com/FWLink/p/?LinkId=189675)

[Om exporter](http://go.microsoft.com/FWLink/p/?LinkId=189676)
