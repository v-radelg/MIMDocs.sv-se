---
title: Språk som stöds av Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: En lista med språk som stöds av Microsoft Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/23/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 5704e978734bea13f1a362aeb203810f3864205a
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701476"
---
# <a name="supported-languages"></a>Språk som stöds

Den här artikeln beskriver de språk som stöds och mappning av uppdateringar från Microsoft Identity Manager 2016 SP1 version 4.5. x eller senare.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>Språk paket för MIM-tjänsten och portalen och tilläggen 

Microsoft MIM-tjänsten och Portal språk paketet har stöd för följande språk 33-språk.  

> [!NOTE]
> I [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) kommer en register nyckel som har lagts till med namnet "OverrideDefaultUILocale" till MIM-tillägg och-språk paket att försöka mappa alla liknande språk till det som stöds. Om t. ex. visnings språket för Windows är es-cl (spanska Chile) eller något es\*, kommer det att försöka mappa detta till es-es (spanska Spanien).

> [!IMPORTANT]
> Texten i SSPR-tillägget och-portalen kommer att lokaliseras, men frågorna kommer inte att fungera utan ytterligare arbete. Du måste skapa authn-arbetsflöden (och tillhör ande uppsättningar och MPR) för att rikta in dig på frågor på varje språk till mål platsen.

|       Språk        | FIM(4.3.x.x)/MIM(4.4.xx) | MIM(4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Bulgariska       |          BG-BG           |      Bulgarien      |
| Förenklad kinesiska  |          zh-CN           |   zh-hans    |
|   Kinesiska (Taiwan)    |          zh-TW           |   zh-hant    |
|       Kroatiska        |          HR-HR           |      timmars      |
|         Tjeckiska         |          CS-CZ           |      cs      |
|        Danska         |          da-DK           |      da      |
|         Nederländska         |          NL-NL           |      nl      |
|       Estniska        |          et-EE           |      ge      |
|        Franska         |          fr-FR           |      fr      |
|        Finska        |          fi-FI           |      fi      |
|        Tyska         |          de-DE           |      de      |
|         Grekiska         |          el GR           |      El      |
|         Hindi         |          Hej Indien           |      Hej      |
|       Ungerska       |          hu-HU           |      HU      |
|        Italienska        |          IT-IT           |      it      |
|       Japanska        |          ja-JP           |      ja      |
|        Koreanska         |          ko-KR           |      Ko      |
|      Litauiska       |          lt-LT           |      lt      |
|        Lettiska        |          lv-LV           |      LV      |
|       Norska       |          NB-NO           |    NB-NO     |
|        Polska         |          pl-PL           |      pl      |
| Portugisiska (Portugal) |          PT-PT           |      pt      |
|  Portugisiska (Brasilien)  |          pt-BR           |    pt-BR     |
|        Ryska        |          ru-RU           |      ru      |
|       Rumänska        |          RO-RO           |      ro      |
|        Spanska        |          es-ES           |      ES      |
|        Slovakiska         |          sk-SK           |      sk      |
|        Svenska        |          SV-SE           |      sv      |
|       Slovenska       |          sl-SI           |      SL      |
|   Serbiska-Serbien    |  sr-latn-CS(Depricated)  |  SR-latn-RS  |
|         Thai          |          TH-TH           |      i:te      |
|        Turkiska        |          TR-TR           |      TR      |
|       Ukrainska       |          uk-UA           |      Storbritannien      |

## <a name="certificate-management"></a>Certifikathantering 
Microsoft Certificate Management stöder följande 9 språk. 

|Språk|FIM(4.3.x.x)/MIM(4.4.xx)|Ny MIM (4.5. x. x)
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

## <a name="certificate-management-modern-application"></a>Modernt program för certifikat hantering  
Microsoft Certificate Management moderna program har stöd för följande 33-språk. 

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
|Grekiska|el GR|El|
|Ungerska|hu-HU|HU|
|Italienska|IT-IT|it|
|Japanska|ja-JP|ja|
|Koreanska|ko-KR|Ko|
|Norska|NB-NO|NB-NO|
|Polska|pl-PL|pl|
|Portugisiska (Portugal)|PT-PT|pt|
|Portugisiska (Brasilien)|pt-BR|pt-BR|
|Ryska|ru-RU|ru|
|Rumänska|RO-RO|ro|
|Spanska|es-ES|ES|
|Svenska|SV-SE|sv|
|Turkiska|TR-TR|TR|

## <a name="next-steps"></a>Nästa steg

- [Första distributionen](microsoft-identity-manager-deploy.md)
- [Versions historik](reference/version-history.md)
