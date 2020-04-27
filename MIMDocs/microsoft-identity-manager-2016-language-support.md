---
title: Språk som stöds av Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: En lista med språk som stöds av Microsoft Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 2caf9f06067c229d585019f912a7ff4e00fad3e6
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044131"
---
# <a name="supported-languages"></a>Språk som stöds

Den här artikeln beskriver de språk som stöds och mappning av uppdateringar från Microsoft Identity Manager 2016 SP1 version 4.5. x eller senare.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>Språk paket för MIM-tjänsten och portalen och tilläggen 

Microsoft MIM-tjänsten och Portal språk paketet har stöd för följande språk 33-språk.  

> [!NOTE]
> I [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) kommer en register nyckel som har lagts till med namnet "OverrideDefaultUILocale" till MIM-tillägg och-språk paket att försöka mappa alla liknande språk till det som stöds. Om t. ex. visnings språket för Windows är ES-CL (spanska Chile) eller något ES\*, kommer det att försöka mappa detta till ES-es (spanska Spanien).

> [!IMPORTANT]
> Texten i SSPR-tillägget och-portalen kommer att lokaliseras, men frågorna kommer inte att fungera utan ytterligare arbete. Du måste skapa authn-arbetsflöden (och tillhör ande uppsättningar och MPR) för att rikta in dig på frågor på varje språk till mål platsen.

|       Språk        | FIM (4.3. x)/MIM (4.4. xx) | MIM (4.5. x. x) |
|-----------------------|--------------------------|--------------|
|       Bulgariska       |          bg-BG           |      Bulgarien      |
| Kinesiska (förenklad)  |          zh-CN           |   zh-hans    |
|   Kinesiska (Taiwan)    |          zh-TW           |   zh-Hant    |
|       Kroatiska        |          hr-HR           |      tim      |
|         Tjeckiska         |          cs-CZ           |      östasiatisk      |
|        Danska         |          da-DK           |      da      |
|         Nederländska         |          nl-NL           |      nl      |
|       Estniska        |          et-EE           |      ge      |
|        Franska         |          fr-FR           |      fr      |
|        Finska        |          fi-FI           |      fi      |
|        Tyska         |          de-DE           |      de      |
|         Grekiska         |          el-GR           |      El      |
|         Hindi         |          hi-IN           |      Hej      |
|       Ungerska       |          hu-HU           |      HU      |
|        Italienska        |          it-IT           |      it      |
|       Japanska        |          ja-JP           |      ja      |
|        Koreansk         |          ko-KR           |      Ko      |
|      Litauiska       |          lt-LT           |      långsiktiga      |
|        Lettiska        |          lv-LV           |      LV      |
|       Norska       |          nb-NO           |    nb-NO     |
|        Polska         |          pl-PL           |      pl      |
| Portugisiska (Portugal) |          pt-PT           |      pt      |
|  Portugisiska (Brasilien)  |          pt-BR           |    pt-BR     |
|        Ryska        |          ru-RU           |      ru      |
|       Rumänska        |          ro-RO           |      ro      |
|        Spanska        |          es-ES           |      ES      |
|        Slovakiska         |          sk-SK           |      sk      |
|        Svenska        |          sv-SE           |      sv      |
|       Slovenska       |          sl-SI           |      SL      |
|   Serbiska-Serbien    |  SR-latn-CS (inaktuell)  |  sr-Latn-RS  |
|         Thailändska          |          th-TH           |      i:te      |
|        Turkiska        |          tr-TR           |      TR      |
|       Ukrainska       |          uk-UA           |      Storbritannien      |

## <a name="certificate-management"></a>Certifikathantering 
Microsoft Certificate Management stöder följande 9 språk. 

|Språk|FIM (4.3. x)/MIM (4.4. xx)|Ny MIM (4.5. x. x)
|-----|-----|-----|-----|
|Kinesiska (förenklad)|zh-CN|zh-hans|
|Kinesiska (Taiwan)|zh-TW|zh-Hant|
|Nederländska|nl-NL|nl|
|Franska|fr-FR|fr|
|Tyska|de-DE|de|
|Italienska|it-IT|it|
|Japanska|ja-JP|ja|
|Portugisiska (Portugal)|pt-PT|pt-PT|
|Spanska|es-ES|ES|

## <a name="certificate-management-modern-application"></a>Modernt program för certifikat hantering  
Microsoft Certificate Management moderna program har stöd för följande 33-språk. 

|Språk | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Nederländska|nl-NL|nl|
|Kinesiska (förenklad)|zh-CN|zh-hans|
|Kinesiska (Taiwan)|zh-TW|zh-Hant|
|Tjeckiska|cs-CZ|östasiatisk|
|Danska|da-DK|da|
|Franska|fr-FR|fr|
|Finska|fi-FI|fi|
|Tyska|de-DE|de|
|Grekiska|el-GR|El|
|Ungerska|hu-HU|HU|
|Italienska|it-IT|it|
|Japanska|ja-JP|ja|
|Koreansk|ko-KR|Ko|
|Norska|nb-NO|nb-NO|
|Polska|pl-PL|pl|
|Portugisiska (Portugal)|pt-PT|pt|
|Portugisiska (Brasilien)|pt-BR|pt-BR|
|Ryska|ru-RU|ru|
|Rumänska|ro-RO|ro|
|Spanska|es-ES|ES|
|Svenska|sv-SE|sv|
|Turkiska|tr-TR|TR|

## <a name="next-steps"></a>Nästa steg

- [Första distributionen](microsoft-identity-manager-deploy.md)
- [Versions historik](reference/version-history.md)
