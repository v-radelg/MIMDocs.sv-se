---
title: Arbeta med Identity Manager Hybrid Reporting | Microsoft Identity Manager
description: "Lär dig hur du sammanför lokala data och molndata i hybridrapporter i Azure och hur du hanterar och visar dessa rapporter."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: e2d0bd6120628d4fd2a14718fc205cde976c7785


---

# Arbeta med Identity Manager Hybrid Reporting

## Tillgängliga hybridrapporter
De första tre Microsoft Identity Manager (MIM)-rapporterna i Azure AD är **Återställningsaktivitet för lösenord**, **Registrering för lösenordsåterställning** och **Aktiviteter för självbetjäningsgrupper**.

-   Återställningsaktiviteten för lösenord visar alla tillfällen någon användare har genomfört en lösenordåterställningsaktivitet via SSPR och anger de portaler eller **metoder** som användes vid autentiseringen.

    ![Hybridrapportering i Azure – bild för återställningsaktivitet för lösenord](media/MIM-Hybrid-passwordreset.jpg)

-   Registrering för lösenordsåterställning visar alla tillfällen någon användare har registrerat sig för SSPR och vilka **metoder** som använts för autentisering, exempelvis ett mobiltelefonnummer eller frågor och svar.
    Observera att vid registrering för lösenordsåterställning görs ingen skillnad mellan SMS-gate och MFA-gate, båda betraktas som **mobiltelefon**.

-   Aktiviteter för självbetjäningsgrupper visar alla försök av någon att lägga till eller ta bort sig själv från en grupp och försök att skapa en grupp.

> [!NOTE]
> Rapporterna visar i nuläget data för upp till en månad bakåt i tiden.
>
> Om du vill avinstallera hybridrapporter ska du avinstallera agenten MIMreportingAgent.msi.

## Krav

1.  Installera Microsoft Identity Manager 2016 inklusive MIM-tjänsten.

2.  Säkerställ att du har en Azure AD Premium-klient med en behörig administratör i din katalog.

3.  Se till att du har en utgående internetanslutning från Microsoft Identity Manager-servern till Azure.

## Installera Microsoft Identity Manager Reporting i Azure AD
Efter att rapportagenten har installerats exporteras data från aktiviteten i Microsoft Identity Manager från MIM till Windows händelselogg. MIM-rapportagenten bearbetar händelserna och överför dem till Azure. I Azure parsas, dekrypteras och filtreras händelser för de begärda rapporterna.

1.  Installera Microsoft Identity Manager 2016.

2.  Hämta rapportagenter för Microsoft Identity Manager:

    1.  Logga in på hanteringsportalen för Azure AD och klicka på Active Directory-ikonen.

    2.  Dubbelklicka på katalogen för vilken du är global administratör och har ett Azure AD Premium-abonnemang.

    3.  Klicka på **Konfiguration** och hämta rapportagenten.

3.  Installera rapportagenten för Microsoft Identity Manager:

    1.  Skapa en katalog på datorn.

    2.  Extrahera filerna `MIMHybridReportingAgent.msi` och `tenant.cert` till katalogen.

    3.  Kör installationsprogrammet för agenten.

    4.  Se till att MIM-rapportagenttjänsten körs

    5.  Starta om MIM-tjänsten.

4.  Validera att Microsoft Identity Manager Reporting arbetar i Azure.

    Du kan generera rapportdata genom att använda portalen för Lösenordsåterställning via självbetjäning för Microsoft Identity Manager för att återställa en användares lösenord. Kontrollera att lösenordsåterställningen lyckades och kontrollera sedan att dessa data visas i hanteringsportalen för Azure AD.

## Visa hybridrapporter i Azure klassiska portal

1.  Logga in på [Azure klassiska portal](https://manage.windowsazure.com/) med ditt globala administratörskonto för klienten.

2.  Klicka på **Active Directory**-ikonen.

3.  Välj klientkatalogen i listan över tillgängliga kataloger för ditt abonnemang.

4.  Klicka på **Rapporter** och sedan på **Återställningsaktivitet för lösenord**.

5.  Var noga med att välja **Identity Manager** i källans nedrullningsbara meny.

> [!WARNING]
> Det kan ta lite tid innan Microsoft Identity Manager visas i Azure AD.

## Avbryta generering av hybridrapporter
Om du vill avbryta uppladdningen av rapportdata från Microsoft Identity Manager till Azure Active Directory ska du avinstallera hybridrapporteringsagenten. Använd Windows-verktyget **Lägg till eller ta bort program** för att avinstallera Microsoft Identity Manager Hybrid Reporting.

## Windows-händelser som används för hybridrapportering
Händelser som genererats av Microsoft Identity Manager loggas i Windows händelselogg och visas i Loggboken under Program- och tjänstloggar &gt; **Begärandelogg för Identity Manager**. Varje MIM-begäran exporteras som en händelse i Windows händelselogg i JSON-strukturen. Denna kan exporteras till din SIEM.

|Händelsetyp|ID|Händelseinformation|
|--------------|------|-----------------|
|Information|4121|MIM-händelsedata som inkluderar alla begärandedata.|
|Information|4137|Förlängning av MIM-händelse 4121, om det finns för många data för en enskild händelse. Rubriken för den här händelsen är i följande format: `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Jun16_HO4-->


