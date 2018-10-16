---
title: Begära certifikat i Certifikathanteraren med mallar | Microsoft Docs
description: Lär dig att använda Certifikathanteraren för att skapa och förnya programcertifikat med profilmallar.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ce86d2eed526972873015e62ef134358de56fdbd
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333130"
---
# <a name="create-software-certificates-with-certificate-manager"></a>Skapa programcertifikat med Certifikathanteraren
För att registrera och förnya programcertifikat behöver du inte vara administratör och du behöver inte heller ha ett virtuellt smartkort. Det bör påpekas att du vid en viss tidpunkt kommer att uppmanas tillåta en certifikatåtgärd, vilket är normalt.

## <a name="create-a-software-certificate-profile-template-in-mim-2016-certificate-manager"></a>Skapa en profilmall för programcertifikat i Certifikathanteraren i MIM 2016

1.  Skapa en mall för det certifikat du ska begära för det virtuella smartkortet. Öppna MMC.

2.  Klicka på **Arkiv** och sedan på **Lägg till/ta bort snapin-modul**.

3.  Klicka på **Certifikatmallar** i listan Tillgängliga snapin-moduler och klicka sedan på **Lägg till**.

4.  **Certifikatmallar** finns nu under konsolroten i MMC. Dubbelklicka på den för att visa alla tillgängliga certifikatmallar.

5.  Högerklicka på **Användarmall** och klicka sedan på **Kopiera mallen**.

6.  På fliken **Kompatibilitet** under Certifikatutfärdare väljer du Windows Server 2008 och under Certifikatmottagare väljer du Windows 8.1/Windows Server 2012 R2.

    1.  På fliken **Allmänt** i fältet Visa namn anger du **Arkiverad certifikatmall**.

    2.  b.  På fliken **Hantering av begäranden**

        1.  anger du **Syfte** som Signatur och kryptering.

        2.  Markera **Inkludera symmetriska algoritmer som tillåts av ämnet**.

        3.  Om du vill arkivera nyckeln markerar du **Arkivera ämnets privata krypteringsnyckel**.

        4.  Under Gör följande ... väljer du **Ställ frågor till användaren när registrering sker**.

    3.  På fliken **Kryptografi**

        1.  under Leverantörskategori väljer du **Nyckellagringsleverantör**

        2.  Välj **Begäranden kan använda valfri provider som finns på ämnets dator**.

    4.  På fliken **Säkerhet** lägger du till den säkerhetsgrupp till vilken du vill ge åtkomst för att **Registrera**. Om du till exempel vill ge alla användare åtkomst väljer du gruppen **Autentiserade** användare och väljer sedan **Registrera** behörigheter för dessa.

    5.  På fliken **Ämnesnamn**

        1.  avmarkerar du **Inkludera e-postnamnet i ämnesnamnet**.

        2.  Under **Inkludera följande information i alternativt ämnesnamn** avmarkerar du **E-postnamn**.

    6.  Klicka på **OK** för att slutföra ändringarna och skapa den nya mallen. Den nya mallen bör nu visas i listan över certifikatmallar.

    7.  Välj **Arkiv** och klicka sedan på **Lägg till/ta bort snapin-modulen** för att lägga till snapin-modulen för certifikatutfärdaren i MMC-konsolen. När du tillfrågas om vilken dator du vill hantera väljer du **Lokal dator**.

    8.  Expandera **Certifikatutfärdaren (lokal)** i rutan till vänster i MMC och expandera sedan din certifikatutfärdare i listan över certifikatutfärdare.

    9. Högerklicka på **Certifikatmallar**, klicka på **Ny** och klicka sedan på **Certifikatmall** att utfärda.

    10. I listan väljer du den nya mall du just skapat (**Arkiverad certifikatmall**) och klickar sedan på **OK**.

## <a name="create-the-profile-template"></a>Skapa profilmallen

1.  Logga in på CM-portalen som en användare med administratörsbehörighet.

2.  Gå till **Administration &gt; Hantera profilmallar** och markera rutan bredvid **MIM CM Exempelprofilmall för smartkort** och klicka sedan på **Kopiera en markerad profilmall**.

3.  Skriv in profilmallens namn och klicka på **OK**.

4.  På nästa skärm klickar du på **Lägg till ny certifikatmall**. Se till att markera kryssrutan bredvid namnet på Certifikatutfärdaren.

5.  Markera kryssrutan bredvid namnet på det arkiverade programcertifikatet och klicka på **Lägg till**.

6.  Ta bort användarmallen genom att markera kryssrutan bredvid den och klicka sedan på **Ta bort markerade certifikatmallar**. Klicka på **OK**.

7.  Klicka på **Ändra allmänna inställningar**.

8.  Markera kryssrutorna till vänster om **Generera krypteringsnycklar på servern** och klicka på **OK**. I det vänstra fönstret klickar du på **Återställ princip**.

9. Klicka på **Ändra allmänna inställningar**.

10. Om du vill återutfärda arkiverade certifikat markerar du kryssrutorna till vänster om **Återutfärda arkiverade certifikat** och klickar på **OK**.

11. Om du använder Certifikathanteraren för virtuella smartkort måste du inaktivera datainsamlingsobjekt eftersom det inte fungerar när datainsamling har aktiverats. Inaktivera datainsamling för varje policy genom att klicka på respektive policy i den vänstra rutan och avmarkera sedan kryssrutan bredvid **Exempeldataobjekt** och klicka sedan på **Ta bort datainsamlingsobjekt**. Klicka sedan på **OK**.
