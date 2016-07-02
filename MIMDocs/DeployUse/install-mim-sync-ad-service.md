---
title: "Installera MIM 2016&#58; synkronisera Active Directory and MIM-tjänsten | Microsoft Identity Manager"
description: "Använd hanteringsagenter och MIM-synkroniseringstjänsten för att synkronisera Active Directory och MIM-databaser."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 8fd779bcc4d41b6e67d0fa31aa0f37c4ea2b410a
ms.openlocfilehash: f4e94980c6a03b08221fd46e19c421cce226086d


---

# Installera MIM 2016: synkronisera Active Directory och MIM-tjänsten

>[!div class="step-by-step"]
[« MIM-tjänst och -portal](install-mim-service-portal.md)

> [!NOTE]
> I den här genomgången används exempelnamn och -värden från företaget Contoso. Ersätt dem med dina egna namn och värden. Exempel:
> - Namn på domänkontrollant – **mimservername**
> - Domännamn – **contoso**
> - Lösenord – **Pass@word1**

MIM-synkroniseringstjänsten (Sync) har som standard inga anslutningar konfigurerade.  Normalt är första steget att använda MIM Sync för att lägg till befintliga Active Directory-konton i MIM-tjänstdatabasen. För att göra detta använder du MIM Sync-tjänstprogrammet.

## Skapa MIM-hanteringsagenten
MIM-hanteringsagenten (MA) används för anslutning av MIM Sync till MIM-tjänsten. Använd guiden Skapa hanteringsagent för att skapa den här anslutningen.

När du ska konfigurera en MIM-hanteringsagent måste du ange ett användarkonto. I det här dokumentet används **MIMMA** som namn på det här kontot.

> [!NOTE]
> Kontot du använder för MIM-hanteringsagenten måste vara samma konto som du angav vid installationen av MIM-tjänsten.

###Skapa MIMMA

1.  genom att öppna Hanteraren för synkroniseringstjänsten.

2.  Så här öppnar du guiden Skapa hanteringsagenten: på menyn **Åtgärder** klickar du på **Skapa**.

3.  På sidan **Skapa hanteringsagent** anger du följande inställningar och klickar sedan på **Nästa**.

    -   Hanteringsagent för: MIM-tjänstens hanteringsagent

    -   Namn: MIMMA

4.  På sidan **Anslut till databas** anger du följande inställningar och klickar sedan på **Nästa**

    -   Server: localhost

    -   Databas: MIMService

    -   MIM-tjänstbasens adress: http://localhost:5725

    -   Autentiseringsläge: Windows-integrerad autentisering

    -   Användarnamn: MIMMA

    -   Lösenord: Pass@word

    -   Domän: contoso

5.  På sidan **Valda objekttyper** verifierar du att de objekttyper som listas nedan är markerade och klickar sedan på **Nästa**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   Grupp

6.  På sidan **Markerade attribut** verifierar du att alla listade attribut är markerade och klickar sedan på **Nästa**.

7.  På sidan **Konfigurera anslutningsfilter** klickar du på **Nästa**.

8.  På sidan **Konfigurera mappningar av objekttyper** lägger du till följande mappning och klickar sedan på **Nästa**

    - Välj **Person** i listan **Typ av datakällobjekt** .
    - Klicka på **Lägg till mappning** för att öppna dialogrutan Mappning.
    - Välj **person** i listan **Typ av metaversumobjekt**.
    - Klicka på **OK** för att stänga dialogrutan Mappning.

9.  På sidan **Konfigurera attributflöde** tillämpar du följande mappningar av attributflödet och klickar sedan på **Nästa**

    | **Attribut för datakälla** | **Flödesriktning** | **Attribut för metaversum** |
    |-|-|-|
    | Kontonamn | Exportera | kontonamn |
    | DisplayName | Exportera | visningsnamn |
    | Domain | Exportera | domän |
    | EmployeeID | Exportera | anställnings-ID |
    | Anställningstyp | Exportera | anställningstyp |
    | E-post | Exportera | e-post |
    | Förnamn | Exportera | förnamn |
    | Efternamn | Exportera | efternamn |
    | Objekt-SID | Exportera | objekt-SID |

10.  Välj **Person** som typ av datakällobjekt.

    -   Välj **Person** som typ av metaversumobjekt.

    -   Välj **Direkt** som mappningstyp.

    -   Utför följande steg för varje rad i den föregående tabellen:

        -   Välj den **flödesriktning** som visas för den raden i tabellen.

        -   Välj det **datakällsattribut** som visas för den raden i tabellen.

        -   Välj det **Metaversum-attribut** som visas för den raden i tabellen.

        -   För att använda flödesmappningen, klicka på **Ny**.

    -   Välj **Grupp** som typ av datakälla och metaversum-objekttyp.

    -   Välj **Direkt** som mappningstyp.

    -   Utför följande steg för varje rad i följande tabell:

        -   Välj den **flödesriktning** som visas för den raden i tabellen.

        -   Välj det **datakällsattribut** som visas för den raden i tabellen.

        -   Välj det **Metaversum-attribut** som visas för den raden i tabellen.

        -   För att använda flödesmappningen, klicka på **Ny**.

    | **Attribut för datakälla** | **Flödesriktning** | **Attribut för metaversum** |
    |-|-|-|
    | Kontonamn | Exportera | kontonamn |
    | DisplayName | Exportera | visningsnamn |
    | Domain | Exportera | domän |
    | E-post | Exportera | e-post |
    | Smeknamn för e-post | Exportera | smeknamn för e-post |
    | Medlem | Exportera | medlem |
    | Objekt-SID | Exportera | objekt-SID |
    | Omfång | Exportera | omfång |
    | Typ | Exportera | typ |
    | Lägg till arbetsflöde för medlemskap | Exportera | lägg till arbetsflöde för medlemskap |
    | Medlemskap låst | Exportera | medlemskap låst |
    | DisplayName | Importera | visningsnamn |
    | Omfång | Importera | omfång |
    | Typ | Importera | typ |
    | Medlem | Importera | medlem |
    | Kontonamn | Importera | kontonamn |
    | Visad ägare | Importera | visad ägare |
    | Smeknamn för e-post | Importera | smeknamn för e-post |


11.  På sidan **Konfigurera avetablering** klickar du på **Nästa**

12.  Skapa hanteringsagenten på sidan **Konfigurera tillägg** genom att klicka på **Slutför**.

## Skapa AD-hanteringsagenten
Active Directory-hanteringsagenten är en anslutning för AD-domäntjänster. Använd guiden Skapa hanteringsagent för att skapa den här anslutningen.

1. Så här öppnar du guiden Skapa hanteringsagenten: på menyn **Åtgärder** klickar du på **Skapa**.

2. På sidan **Skapa hanteringsagent** anger du följande inställningar och klickar sedan på **Nästa**

    - Hanteringsagent för: Active Directory Domain Services
    - Namn: ADMA

3. På sidan **Anslut till Active Directory-skog** anger du följande inställningar och klickar sedan på **Nästa**:

    - Namn på skog: contoso.local
    - Användarnamn: administratör
    - Lösenord: &lt;kontots lösenord&gt;
    - Domän: contoso

4. På sidan **Konfigurera katalogpartitioner** anger du följande inställningar och klickar sedan på **Nästa**:

    - I listan **Välj katalogpartitioner** väljer du **DC=CONTOSO, DC=local**.

    - Öppna dialogrutan Välj behållare genom att klicka på **Behållare**.

    - Om du vill ändra behållare så att den endast har MIM-hanteringsobjekt i en viss behållare klickar du på noden **DC = CONTOSO, DC = local**. Klicka sedan på noden för den behållare du vill ha.

    - Stäng dialogrutan Välj behållare genom att klicka på **OK**.

5. På sidan **Konfigurera etableringshierarki** klickar du på **Nästa**.

6. På sidan **Välj objekttyper ** anger du följande inställningar och klickar sedan på **Nästa**

    - I listan **Objekttyper** väljer du **användare** och **grupp**.

7. På sidan **Välj attribut** anger du följande inställningar och klickar sedan på **Nästa**

    - Välj **Visa alla**.

8. I listan **Attribut** väljer du följande attribut:

    -   företag
    -   visningsnamn
    -   anställnings-ID
    -   anställningstyp
    -   förnamn
    -   grupptyp
    -   manager
    -   hanteradAv
    -   medlem
    -   objekt-SID
    -   sAMAccountName
    -   sAMAccountType
    -   sn
    -   unicodePwd
    -   userAccountControl

9. På sidan **Konfigurera anslutningsfilter** klickar du på **Nästa**.

10. På sidan **Konfigurera anslutnings- och projektionsregler** klickar du på **Nästa**.

11. På sidan **Konfigurera attributflöde** klickar du på **Nästa**.

12. På sidan **Konfigurera avetablering** klickar du på **Nästa**.

13. På sidan **Konfigurera tillägg** klickar du på **Slutför**.


## Skapa körningsprofiler

Skapa körningsprofiler för ADMA- och MIMMA-kopplingarna.

### Skapa körningsprofiler för ADMA-anslutningen

Den här tabellen visar de fem körningsprofiler du ska skapa för ADMA-anslutningen:

| Namn | Typ |
| ---- | ---- |
| Profil1 | Fullständig import (endast mellanlagring) |
| Profil2 | Fullständig synkronisering |
| Profil3 | Deltaimport (endast mellanlagring) |
| Profil4 | Deltasynkronisering |
| Profil5 | Exportera |

För att skapa körningsprofiler för ADMA-anslutningen:

1. Öppna Hanteraren för synkroniseringstjänsten. På menyn **Verktyg** klickar du sedan på **Hanteringsagenter**.

2. I listan **Hanteringsagenter** väljer du **ADMA**.

3. Så här öppnar du dialogrutan Konfigurera körningsprofiler: på menyn **Åtgärder** klickar du på **Konfigurera körningsprofiler**.

4. Utför följande steg för varje körningsprofil i tabellen:

    - Öppna guiden Konfigurera körningsprofil genom att klicka på **Ny profil**.

    - I rutan **Namn** anger du namnet på profilen i tabellen och klickar på **Nästa**.

    - I listan **Typ** väljer du stegtyp i tabellen och klickar sedan på **Nästa**.

    - Klicka på **Slutför** för att skapa körningsprofilen.

5. Stäng dialogrutan Konfigurera körningsprofiler genom att klicka på **OK**.

### Skapa körningsprofiler för MIMMA-anslutningen

I den här tabellen visas de fem matchande körningsprofilerna för MIMMA-anslutningen:

| Namn | Typ |
| -------- | -------- |
| Profil1 | Fullständig import (endast mellanlagring) |
| Profil2 | Fullständig synkronisering |
| Profil3 | Deltaimport (endast mellanlagring) |
| Profil4 | Deltasynkronisering |
| Profil5 | Exportera |

Skapa körningsprofiler för MIMMA-anslutningen genom att:

1. Öppna Hanteraren för synkroniseringstjänsten. På menyn **Verktyg** klickar du sedan på **Hanteringsagenter**.

2. I listan **Hanteringsagenter** väljer du **MIMMA**.

3. Så här öppnar du dialogrutan Konfigurera körningsprofiler: på menyn **Åtgärder** klickar du på **Konfigurera körningsprofiler**.

4. Utför följande steg för varje körningsprofil i tabellen:

    - Öppna guiden Konfigurera körningsprofil genom att klicka på **Ny profil**.

    - I rutan **Namn** anger du namnet på profilen i tabellen och klickar på **Nästa**.

    - I listan **Typ** väljer du stegtyp i tabellen och klickar sedan på **Nästa**.

    - Klicka på **Slutför** för att skapa körningsprofilen.

5. Stäng dialogrutan Konfigurera körningsprofiler genom att klicka på **OK**.

## Konfigurera MIM-tjänsten

Med hjälp av MIM-portalen skapar du regeln för inkommande synkronisering av AD-användare för MIM-tjänsten.

Så här skapar du regeln för inkommande synkronisering av AD-användare:

1. I navigeringsfältet på hemsidan i MIM-portalen klickar du på **Administration**.

2. Öppna sidan Synkroniseringsregler genom att klicka på **Synkroniseringsregler**.

3. Öppna guiden Skapa regel för synkronisering i verktygsfältet genom att klicka på **Ny**.

4. På fliken **Allmänt** anger du följande uppgifter och klickar sedan på **Nästa**.

    -   Visningsnamn: Regel för inkommande synkronisering av AD-användare
    -   Dataflödesriktning: inkommande

5. På fliken **Omfattning** anger du följande uppgifter och klickar sedan på **Nästa**.

    -   Resurstyp för metaversum: person
    -   Externt system: ADMA
    -   Resurstyp för externt system: person

6. På fliken **Relation** anger du följande uppgifter och klickar sedan på **Nästa**:

    -   För att konfigurera Relationskriterierna väljer du **Objekt-SID** i listan MetaversumObjekt:person(Attribut) och i listan AnslutetSystemObjekt:person (attribut).

    -   Välj **Skapa resurs i MIM**.

7. På sidan **Inkommande attributflöde** anger du följande uppgifter och klickar sedan på **Nästa**:

    | Flödesregel | Källa | Mål |
    |-|-|-|
    |Regel 1|samAccountName|f|
    |Regel 2|visningsnamn|visningsnamn|
    |Regel 3|Anställningstyp|Anställningstyp|
    |Regel 4|förnamn|förnamn|
    |Regel 5|sn|efternamn|
    |Regel 6|Manager|manager|
    |Regel 7|objekt-SID|Objekt-SID|
    |Regel 8|”Contoso”|domän|

    Utför följande steg för varje rad i tabellen:

    - Öppna dialogrutan Flödesdefinition genom att klicka på **Nytt attributflöde**.

    - På fliken **Källa** väljer du det attribut som visas för den raden i tabellen.

    - På fliken **Mål** väljer du det attribut som visas för den raden i tabellen.

    - Tillämpa konfigurationen av attributflödet genom att klicka på **OK**.

8. På fliken **Sammanfattning** klickar du på **Skicka**.

## Initiera testmiljön
Det finns fyra steg du måste utföra innan du kan testa MIM-konfigurationen med AD-data:

### Aktivera etablering

1. genom att öppna Hanteraren för synkroniseringstjänsten.

2. Öppna dialogrutan Alternativ på menyn **Verktyg** genom att klicka på **Alternativ**

3. Välj **Aktivera etablering av synkroniseringsregeln**.

4. Stäng dialogrutan Alternativ genom att klicka på **OK**.

### Initiera MIMMA

Kör en fullständig synkroniseringscykel på den här anslutningen. Den fullständiga cykeln består av följande körningsprofiler:

- Fullständig import
- Fullständig synkronisering
- Exportera
- Deltaimport

Följ dessa steg för att köra var och en av de fyra körningsprofilerna.

1. Öppna Hanteraren för synkroniseringstjänsten. På menyn **Verktyg** klickar du sedan på **Hanteringsagenter**.

2. I listan **Hanteringsagenter** väljer du **MIMMA**.

3. Öppna dialogrutan Kör hanteringsagenten på menyn **Åtgärder** genom att klicka på **Kör**.

4. Utför följande steg för varje körningsprofil som anges ovan:

    - Öppna dialogrutan Kör hanteringsagenten på menyn **Åtgärder** genom att klicka på **Kör**.

    - I listan **körningsprofiler** väljer du den körningsprofil som du vill köra.

    - Starta körningsprofilen genom att klicka på **OK**.

#### Konfigurera prioritet för attributflöde

Under initieringen av MIM-anslutningen har de konfigurerade synkroniseringsreglerna förts in i metaversumet.

Justera prioriteten för attributflödet för de attribut som den här anslutningen har medfört för att säkerställa att attribut som redan finns i AD kan flöda till metaversumet och senare även till MIM-tjänstdatabasen.

### Initiera ADMA

För att initiera Active Directory-anslutningen måste du köra en fullständig import och en fullständig synkronisering av den. Den fullständiga importen för de befintliga objekten från AD till anslutningsplatsen. Vid en fullständig synkronisering uppdateras synkroniseringsreglerna så att de stämmer överens med MIM-anslutningens regler.

1. Öppna Hanteraren för synkroniseringstjänsten. På menyn **Verktyg** klickar du sedan på **Hanteringsagenter**.

2. I listan **Hanteringsagenter** väljer du **ADMA**.

3. Öppna dialogrutan Kör hanteringsagenten på menyn **Åtgärder** genom att klicka på **Kör**.

4. Utför följande steg för varje körningsprofil som anges ovan:

    - Öppna dialogrutan Kör hanteringsagenten på menyn **Åtgärder** genom att klicka på **Kör**.

    - I listan **körningsprofiler** väljer du den körningsprofil som du vill köra.

    - Starta körningsprofilen genom att klicka på **OK**.

### Lägg till i MIM-tjänstdatabasen

För att fylla MIM-tjänstdatabasen med objekten måste du köra en synkroniseringscykel på MIMMA-anslutningen. Cykeln består av:

- Exportera
- Fullständig import
- Fullständig synkronisering

Följ dessa steg för att köra var och en av de tre körningsprofilerna.

1. Öppna Hanteraren för synkroniseringstjänsten och klicka på **Hanteringsagenter** på menyn **Verktyg**.

2. Välj **MIMMA** i listan **Hanteringsagenter**.

3. Klicka på **Kör** på menyn **Åtgärder** för att öppna dialogrutan Kör hanteringsagenten.

4. Utför följande steg för varje körningsprofil som anges ovan:

    - Klicka på **Kör** på menyn **Åtgärder** för att öppna dialogrutan Kör hanteringsagenten.
    - Välj den körningsprofil du vill köra i listan **Körningsprofiler**.
    - Klicka på **OK** för att starta körningsprofilen.

>[!div class="step-by-step"]
[« MIM-tjänst och -portal](install-mim-service-portal.md)



<!--HONumber=Jun16_HO4-->


