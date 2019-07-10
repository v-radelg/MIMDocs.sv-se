---
title: Språk för Microsoft Identity Manager 2016 SP1 som stöds | Microsoft Docs
description: En lista över språk som stöds av Microsoft Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/23/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 2d843dcd5285d02a67955e763769a16f1a535abb
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690737"
---
# <a name="supported-languages"></a>Språk som stöds

Den här artikeln beskriver de språk som stöds och mappningen av uppdateringar från Microsoft Identity Manager 2016 SP1-versionen 4.5.x eller större.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>MIM-tjänsten och portalen och tillägg och språkpaket för tillägg 

Microsoft MIM-tjänsten och portalen Language Pack stöder följande språk 33 språk.  

> [!NOTE]
> I [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) en registernyckel som har lagts till kallas ”OverrideDefaultUILocale” till MIM-tillägg och tillägg språkpaket försöker mappa alla liknande språk till det som stöds. Exempel: om visningsspråket för Windows är ES-CL (spanska-Chile), eller alla ES -\*, görs ett försök att mappa detta till ES-ES (Spanska-Spanien).

> [!IMPORTANT]
> Texten i SSPR-tillägg och -portalen kommer lokaliseras, men frågorna kommer inte utan ytterligare arbete. Du behöver att skapa AuthN-arbetsflöden (och tillhörande uppsättningar och MPR att rikta dem) på mål-frågor på varje språk till målplatsen.

|       Språk        | FIM(4.3.x.x)/MIM(4.4.xx) | MIM(4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Bulgariska       |          BG-BG           |      bg      |
| Förenklad kinesiska  |          zh-CN           |   zh-hans    |
|   Kinesiska (Taiwan)    |          zh-TW           |   zh-hant    |
|       Kroatiska        |          HR-HR           |      hr      |
|         Tjeckiska         |          CS-CZ           |      cs      |
|        Danska         |          da-DK           |      da      |
|         Nederländska         |          NL-NL           |      nl      |
|       Estniska        |          et-EE           |      et      |
|        Franska         |          fr-FR           |      fr      |
|        Finska        |          fi-FI           |      fi      |
|        Tyska         |          de-DE           |      de      |
|         Grekiska         |          el GR           |      el      |
|         Hindi         |          Hej Indien           |      hi      |
|       Ungerska       |          hu-HU           |      hu      |
|        Italienska        |          IT-IT           |      it      |
|       Japanska        |          ja-JP           |      ja      |
|        Koreanska         |          ko-KR           |      ko      |
|      Litauiska       |          lt-LT           |      lt      |
|        Lettiska        |          lv-LV           |      lv      |
|       Norska       |          NB-NO           |    NB-NO     |
|        Polska         |          pl-PL           |      pl      |
| Portugisiska (Portugal) |          PT-PT           |      pt      |
|  Portugisiska (Brasilien)  |          pt-BR           |    pt-BR     |
|        Ryska        |          ru-RU           |      ru      |
|       Rumänska        |          RO-RO           |      ro      |
|        Spanska        |          es-ES           |      ES      |
|        Slovakiska         |          sk-SK           |      sk      |
|        Svenska        |          SV-SE           |      sv      |
|       Slovenska       |          sl-SI           |      sl      |
|   Serbiska - Serbien    |  sr-latn-CS(Depricated)  |  SR-Latn-RS  |
|         Thai          |          TH-TH           |      TH      |
|        Turkiska        |          TR-TR           |      tr      |
|       Ukrainska       |          uk-UA           |      Storbritannien      |

## <a name="certificate-management"></a>Certifikathantering 
Certifikathantering Microsoft stöder följande 9 språk. 

|Språk|FIM(4.3.x.x)/MIM(4.4.xx)|New MIM(4.5.x.x)
|-----|-----|-----|-----|
|Förenklad kinesiska|zh-CN|zh-hans|
|Kinesiska (Taiwan)|zh-TW|zh-hant|
|Nederländska|NL-NL|nl|
|Franska|fr-FR|fr|
|Tyska|de-DE|de|
|Italienska|IT-IT|it|
|Japanska|ja-JP|ja|
|Portugisiska (Portugal)|PT-PT|PT-PT|
|Spanska|es-ES|ES|

## <a name="certificate-management-modern-application"></a>Certifikat Management moderna program  
Microsoft Certificate Management modernt program har stöd för följande 33 språk. 

|Språk | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Nederländska|NL-NL|nl|
|Förenklad kinesiska|zh-CN|zh-hans|
|Kinesiska (Taiwan)|zh-TW|zh-hant|
|Tjeckiska|CS-CZ|cs|
|Danska|da-DK|da|
|Franska|fr-FR|fr|
|Finska|fi-FI|fi|
|Tyska|de-DE|de|
|Grekiska|el GR|el|
|Ungerska|hu-HU|hu|
|Italienska|IT-IT|it|
|Japanska|ja-JP|ja|
|Koreanska|ko-KR|ko|
|Norska|NB-NO|NB-NO|
|Polska|pl-PL|pl|
|Portugisiska (Portugal)|PT-PT|pt|
|Portugisiska (Brasilien)|pt-BR|pt-BR|
|Ryska|ru-RU|ru|
|Rumänska|RO-RO|ro|
|Spanska|es-ES|ES|
|Svenska|SV-SE|sv|
|Turkiska|TR-TR|tr|

## <a name="next-steps"></a>Nästa steg

- [Första distributionen](microsoft-identity-manager-deploy.md)
- [Versionshistorik](reference/version-history.md)
