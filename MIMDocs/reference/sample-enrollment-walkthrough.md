---
title: "Exempel på registrering genomgången | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>Exempel registrering genomgång
Det här avsnittet visas de steg som krävs för att utföra en virtuella smarta kort självbetjäningsregistrering. Den visar en automatiskt godkänd begäran med PIN-kod anges av användaren.
1.  Klienten autentiserar användaren och sedan begära en lista över profilmallar som den autentiserade användaren kan registrera för:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  Klienten visar den lista som visas för användaren. Användaren väljer en Profilmall för vSC (virtuellt smartkort) med namnet ”virtuella smartkort VPN” och UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  Klienten hämtar princip för certifikatregistrering för den valda profilmallen med hjälp av UUID som returneras i föregående steg:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  Klienten analyserar fältet ”DataCollection” i principen returnerade påpekas att en enda datainsamling objektet rätt ”begär orsak” visas. Klienten anger också att flaggan ”collectComments” anges till *FALSKT*, så att den inte uppmana användaren att ange någon.

5.  När användaren har angett orsaken till att kräva certifikat, skapar klienten en begäran:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  Servern har skapar begäran och returnerar URI för begäran till klienten: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  Klienten hämtar request-objektet genom att anropa returnerade URI: N:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  Verifierar klienten att egenskapen ”tillstånd” i begäran anges till ”godkänd”. Begäran om körning kan börja.

9.  Klienten undersöker begäran om att se om det finns ett smartkort som är redan kopplad till begäran genom att analysera innehållet i parametern ”newsmartcarduuid”.

10. Eftersom den bara innehåller ett tomt GUID måste klienten använda ett befintligt kort inte redan används av MIM CM, eller skapa en vid profilmallen som konfigureras för virtuella smartkort.

11. Eftersom dessa har angivits till klienten via den första frågan enrollable profilmallar (steg 1), måste klienten nu skapa en virtuell smartkortenhet.

12. Klienten hämtar smartkort principen från profilmallen (med UUID för mallen som valdes i steg 3):

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. Principen för smartkort innehåller den förinställda administratörsnyckeln för kortet i egenskapen ”DefaultAdminKeyHex”. Klienten måste ange den ursprungliga administratörsnyckeln för smartkortet till den här nyckeln när du skapar smartkortet.  

14. Vid skapar smartkort enheten kan måste klienten tilldela den på begäran:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. Servern svarar till klienten med en URI för objektet nyligen skapade smartkort: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. Klienten hämtar smartkort-objektet:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Genom att kontrollera värdet för ”diversifyadminkey”-flaggan i principen för smartkort som erhölls i steg 12, känner klienten behöver distribuera admin-nyckel.

18. Klienten hämtar den föreslagna administratörsnyckeln:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. Klienten måste autentisera till kortet som administratör för att kunna ange admin-nyckel. Om du vill göra detta måste klienten begär en autentiseringsfråga från smartkortet och skickar den till servern:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    Servern skickar anrop svar i brödtexten för HTTP-svar. Leta upp den hexadecimala kontrollsträngen, konvertera till base64 och skickar den som en parameter i URL: en. URL-Adressen som returneras av en annan svar som måste konverteras till hexadecimalt format.
<br/>
20. Servern skickar anrop svar i brödtexten för HTTP-svar och klienten används för att distribuera admin-nyckel.

21. Klienten anger att användaren måste ange sina önskade PIN-koder genom att kontrollera fältet ”UserPinOption” i principen för smartkort.

22. När användaren har angett önskade PIN-koden i en dialogruta, utför klienten en anrop / svar-autentisering som beskrivs i steg 19, med den enda skillnaden är att flaggan diversifierat nu ska anges till ”true”:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. Svaret från servern använder klienten för att ange den önskade användare PIN-kod.

24. Klienten är nu redo att skapa certifikatbegäranden. Frågar profil certifikat generation mallparametrarna för att avgöra hur nycklar/begäranden ska skapas.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. Servern svarar med ett JSON-serialiserad CertificateRequestGenerationOptions objekt med information om det går att exportera nyckeln, certifikatets eget namn, hash-algoritmen, nyckelalgoritm, nyckelstorleken osv. Klienten använder dessa parametrar för att generera en certifikatbegäran.

26. Den enda certifikatbegäran skickades till servern. Klienten kan dessutom ange en PFX-lösenordet som ska användas för att dekryptera alla PFX-blobbar i de fall där certifikatmallen anger arkivering certifikat på Certifikatutfärdaren, d.v.s. CA genererar nyckelparet och skickar det till klienten. Klienten kan även välja att lägga till vissa kommentarer.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Servern svarar med ett enda, serialiseras till JSON Microsoft.Clm.Shared.Certificate objekt efter några sekunder. Genom att kontrollera flaggan ”isPkcs7” klienten lär sig att svaret inte är en PFX-blob. Klienten hämtar blob av base64 avkoda strängen ”pkcs7” och installerar den.

28. Klienten markerar begäran som slutförd.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
