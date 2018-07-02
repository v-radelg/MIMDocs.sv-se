---
title: Språk för Microsoft Identity Manager 2016 SP1 som stöds | Microsoft Docs
description: En lista över språk som stöds av Microsoft Identity Manager 2016 SP1.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 05/23/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 834343243dfeefa8d1874414fa369751288fd64d
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288878"
---
# <a name="supported-languages"></a>Språk som stöds

Den här artikeln beskrivs de språk som stöds och mappningen av uppdateringar från Microsoft Identity Manager 2016 SP1-versionen 4.5.x eller större.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>MIM-tjänsten och portalen och tillägg och tillägg Language Pack 

Microsoft MIM-tjänsten och portalen språkpaket stöd för följande språk 33 språk.  

> [!NOTE]
> I [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) en registernyckel kallas ”OverrideDefaultUILocale” lades till MIM-tillägg och tillägg språkpaket försöker mappa alla liknande språk till det som stöds. Om exempelvis Windows visningsspråket är ES-CL (spanska underordnad), eller alla ES-**, görs ett försök att mappa till ES-ES (Spanska Spanien).

> [!IMPORTANT]
> Frågorna kommer inte utan ytterligare men kommer att placeras i SSPR-tillägg och -portalen. Du behöver skapa AuthN arbetsflöden (och tillhörande uppsättningar och MPR att rikta dem) på mål-frågor på varje språk till målplatsen.

|       Språk        | FIM(4.3.x.x)/MIM(4.4.xx) | MIM(4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Bulgariska       |          BG-BG           |      BG      |
| Kinesiska (förenklad)  |          zh-CN           |   zh-hans    |
|   Kinesiska (Taiwan)    |          zh-TW           |   zh-hant    |
|       Kroatiska        |          HR-HR           |      hr      |
|         Tjeckiska         |          CS-CZ           |      CS      |
|        Danska         |          da-DK           |      da      |
|         Nederländska         |          NL-NL           |      NL      |
|       Estniska        |          et-EE           |      et      |
|        Franska         |          fr-FR           |      fr      |
|        Finska        |          fi-FI           |      Fi      |
|        Tyska         |          de-DE           |      Tyskland      |
|         Grekiska         |          el-GR           |      el      |
|         Hindi         |          Hej IN           |      Hej      |
|       Ungerska       |          hu-HU           |      HU      |
|        Italienska        |          IT-IT           |      den      |
|       Japanska        |          ja-JP           |      Japan      |
|        Koreanska         |          ko-KR           |      Ko      |
|      Litauiska       |          lt-LT           |      lt      |
|        Lettiska        |          lv-LV           |      LV      |
|       Norska       |          NB-NO           |    NB-NO     |
|        Polska         |          pl-PL           |      PL      |
| portugisiska (Portugal) |          PT-PT           |      pt      |
|  Portugisiska (Brasilien)  |          pt-BR           |    pt-BR     |
|        Ryska        |          ru-RU           |      RU      |
|       Rumänska        |          RO-RO           |      ro      |
|        Spanska        |          es-ES           |      ES      |
|        Slovakiska         |          sk-SK           |      Sk      |
|        Svenska        |          SV-SE           |      SA      |
|       Slovenska       |          sl-SI           |      Sl      |
|   Serbiska - Serbien    |  sr-latn-CS(Depricated)  |  SR-Latn-RS  |
|         Thai          |          TH-TH           |      TH      |
|        Turkiska        |          TR-TR           |      TR      |
|       Ukrainska       |          uk-UA           |      Storbritannien      |

## <a name="certificate-management"></a>Certifikathantering 
Certifikathantering Microsoft stöder följande 9 språk. 

|Språk|FIM(4.3.x.x)/MIM(4.4.xx)|Ny MIM(4.5.x.x)
|-----|-----|-----|-----|
|Kinesiska (förenklad)|zh-CN|zh-hans|
|Kinesiska (Taiwan)|zh-TW|zh-hant|
|Nederländska|NL-NL|NL|
|Franska|fr-FR|fr|
|Tyska|de-DE|Tyskland|
|Italienska|IT-IT|den|
|Japanska|ja-JP|Japan|
|portugisiska (Portugal)|PT-PT|PT-PT|
|Spanska|es-ES|ES|

## <a name="certificate-management-modern-application"></a>Certifikat Management moderna program  
Microsoft Certificate Management moderna program stöder följande 33 språk. 

|Språk | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Nederländska|NL-NL|NL|
|Kinesiska (förenklad)|zh-CN|zh-hans|
|Kinesiska (Taiwan)|zh-TW|zh-hant|
|Tjeckiska|CS-CZ|CS|
|Danska|da-DK|da|
|Franska|fr-FR|fr|
|Finska|fi-FI|Fi|
|Tyska|de-DE|Tyskland|
|Grekiska|el-GR|el|
|Ungerska|hu-HU|HU|
|Italienska|IT-IT|den|
|Japanska|ja-JP|Japan|
|Koreanska|ko-KR|Ko|
|Norska|NB-NO|NB-NO|
|Polska|pl-PL|PL|
|portugisiska (Portugal)|PT-PT|pt|
|Portugisiska (Brasilien)|pt-BR|pt-BR|
|Ryska|ru-RU|RU|
|Rumänska|RO-RO|ro|
|Spanska|es-ES|ES|
|Svenska|SV-SE|SA|
|Turkiska|TR-TR|TR|

## <a name="next-steps"></a>Nästa steg

- [Första distributionen](microsoft-identity-manager-deploy.md)
- [Versionshistorik](/reference/version-history.md)