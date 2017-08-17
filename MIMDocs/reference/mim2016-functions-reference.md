---
title: "Referens för Microsoft Identity Manager 2016 fungerar | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Funktioner referens för Microsoft Identity Manager 2016


I Microsoft Identity Manager (MIM) 2016, funktionerna kan du ändra attributvärden innan flödar dem till ett mål i en function-aktivitet eller deklarativ etablering. Syftet med det här dokumentet är att ge dig en översikt över alla tillgängliga funktioner och en beskrivning av hur de kan använda.

Konfigurera mappningar av attributflöde är en grundläggande uppgift när du konfigurerar Synkroniseringsregler. Den enklaste formen av ett flöde attributmappning är en direkt mappning. Som anges med namnet som en direkt mappning tar värdet för ett källattribut och avser konfigurerade Målattributet. Finns det fall där du antingen behöver befintliga attributvärden som ska ändras eller nya attributvärden ska beräknas innan systemet använder dem till ett mål.

Funktioner är en inbyggd metod som används för att definiera typ av ändring som du behöver Synkroniseringsmotorn ska användas när ett attributvärde för ett mål.

I MIM, kan du gruppera befintliga funktioner i följande kategorier:

-   **Datahanteringsfunktioner**. Funktioner för en mängd olika manipulering åtgärder på strängar.

-   **Funktioner för hämtning av data**. Funktioner för att extrahera data från attributvärden.

-   **Data Generation funktioner**. Funktioner för att skapa värden.

-   **Funktioner i Logic**. Funktioner för att utföra åtgärder baserat på villkor.

Följande avsnitt innehåller mer information om funktioner i varje kategori.

## <a name="data-manipulation-functions"></a>Funktioner för datahantering

Datahanteringsfunktioner används för att utföra en mängd manipulering åtgärder på strängar.

| Sammanfoga        |   |
|--------------------|-------------------------|
| Beskrivning        | Funktionen sammanfoga används för att sammanfoga två eller flera strängar.                                                                                                       |
| Funktionssignaturen | sträng1 + sträng2...                                                                                                                                                     |
| Indata             | Två eller flera strängar                                                                                                                                                        |
| Åtgärder         | Alla Indatasträngen parametrar sammanfogas med varandra.                                                                                                              |
| Utdata             | En sträng        |


| Versaler         |         |
|-------------------|---------|
| Beskrivning        | Funktionen versaler konverterar alla tecken i en textsträng till versaler.         |
| Funktionssignaturen | Strängen UpperCase(string)                                                                                                                                                   |
| Indata             | En sträng                                                                                                                                                                 |
| Åtgärder         | Alla gemener för Indataparametern konverteras till versaler. Exempel: UpperCase("test") resulterar i ”TEST”.                                     |
| Utdata             | En sträng                                                              |


| Gemener          |                                 |
|--------------------|---------------------------------|
| Beskrivning        | Funktionen gemener konverterar alla tecken i en textsträng till gemener.                                                                                                  |
| Funktionssignaturen | Strängen LowerCase(string)                                                                                                                                                   |
| Indata             | En sträng                                                                                                                                                                 |
| Åtgärder         | Alla tecken med versaler för Indataparametern omvandlas till gemener. Exempel: LowerCase("TeSt") resulterar i ”test”.                                     |
| Utdata             | En sträng               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Beskrivning        | ProperCase fungera coverts det första tecknet i varje blankstegsavgränsad ord i en sträng med versal och alla andra tecken har konverterats till gemener.           |
| Funktionssignaturen | Strängen ProperCase(string)                                                                                                                                                  |
| Indata             | En sträng                                                                                                                                                                 |
| Åtgärder         | Det första tecknet i varje blankstegsavgränsad ord Indataparametern konverteras till versaler, och alla tecken med versaler omvandlas till gemener. Om ett ord i Indataparametern börjar med ett icke-alfabetiska tecken, är det första tecknet i ordet inte att konvertera till versaler. <br/> Exempel: <br/> -ProperCase("TEsT") resulterar i ”Test”. <br/> -ProperCase("britta simon") resulterar i ”Britta Simon”. <br/>-ProperCase("TEsT") resulterar i ”Test”. <br/> -ProperCase("\$TEsT") resulterar i ”\$Test”.|
| Utdata             | En sträng      |


| LTrim              |      |
|--------------------|------|
| Beskrivning        | Funktionen LTrim tar bort inledande blanksteg från en sträng.                                                                                                             |
| Funktionssignaturen | Strängen LTrim(string)                                                                                                                                                       |
| Indata             | En sträng                                                                                                                                                                 |
| Åtgärder         | De inledande blanksteg i Indataparametern tas bort. <br/><br/>Exempel: LTrim (”Test”) ger ”Test”.                                              |
| Utdata             | En sträng      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beskrivning        | RTrim funktionen tar bort avslutande blanksteg från en sträng.                                                                 |
| Funktionssignaturen | Strängen RTrim(string)                                                                                                            |
| Indata             | En sträng                                                                                                                      |
| Åtgärder         | De avslutande blanksteg i Indataparametern tas bort. Exempel: RTrim (”Test”) ger ”Test”.  |
| Utdata             | En sträng                                                                                                                      |


| Rensa               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beskrivning        | Funktionen Rensa tar bort inledande och avslutande blanksteg från en sträng.                                                      |
| Funktionssignaturen | Strängen Trim(string)                                                                                                             |
| Indata             | En sträng                                                                                                                      |
| Åtgärder         | De inledande och avslutande blanksteg i strängen tas bort. Exempel: Trim (”Test”) ger ”Test”. |
| Utdata             | En sträng                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Beskrivning        | De RightPad funktionen höger-Pad en sträng till en angiven längd genom att använda angivna utfyllnadstecknet.                          |
| Funktionssignaturen | Sträng RightPad (sträng, längd, padCharacter)                                                                                   |
| Åtgärder         | Om längden på strängen är mindre än längden, sedan läggs padCharacter upprepade gånger till i slutet av strängen förrän den har en längd som är lika med längden. <br/> Exempel: <br/> -RightPad("User", 10, "0") leder ”User000000”. <br/> -RightPad(RandomNum(1,10), 5, ”0”) kan resultera i att ”9000”.   |
| Utdata                                                                                                                                                          | Om strängen har en längd som är större än eller lika med längden, returneras en sträng som är identiska till sträng. Om längden på strängen är mindre än längden, returnerade en ny sträng med längden som innehåller strängen fylls ut med en padCharacter. Om strängen är null returneras en tom sträng. |   |   |
>[!NOTE]
**padCharacter** kan vara ett blanksteg, men den kan inte vara ett null-värde. Om längden på **sträng** är lika med eller större än **längd**, **sträng** returneras oförändrat.


| LeftPad      |     |
|----|-------|
| Beskrivning  | De LeftPad funktionen vänster-Pad en sträng till en angiven längd genom att använda angivna utfyllnadstecknet.    |
| Funktionssignaturen      | Sträng LeftPad (sträng, längd, padCharacter)     |
| Indata |  - **Sträng.** Strängen utfyllnad. <br/> - **längden.** Ett heltal som representerar den önskade längden på strängen. <br/> - **padCharacter.** En sträng som består av ett enskilt tecken som ska användas som ett pad tecken. |
| Åtgärder  | Om längden på strängen är mindre än längden, sedan läggs padCharacter upprepade gånger till i början av strängen förrän den har en längd som är lika med längden. <br/> Exempel: <br/> -LeftPad("User", 10, "0") leder ”000000User”. <br/> LeftPad(RandomNum(1,10), 5, ”0”) kan resultera i att ”0009”. |  
|Utdata | Om strängen har en längd som är större än eller lika med längden, returneras en sträng som är identiska till sträng. <br/> Om längden på strängen är mindre än längden, returnerade en ny sträng med längden som innehåller strängen fylls ut med en padCharacter. <br/>  Om **sträng** är null, returneras en tom sträng.                                                   |

<[!NOTE]
**padCharacter** kan vara ett blanksteg, men den kan inte vara ett null-värde. Om längden på **sträng** är lika med eller större än **längd**, **sträng** returneras oförändrat.

| BitOr    |  |
|----- |------|
| Beskrivning  | BITOR-funktion anger lite angivna för en flagga till 1.     |
| Funktionssignaturen  | Int BitOr(mask, flag)       |  
| Indata     | 1. **mask.** Ett hexadecimalt värde som anger bitars ska anges flaggan. <br/> 2. **flaggan.** Ett hexadecimalt värde som ska ha en viss bitars ändras.    |   
| Åtgärder         | Den här funktionen konverterar båda parametrarna till binär representation och jämför dem: <br/> -Anger lite till 1 om en eller båda motsvarande bitar i mask och flaggan är mellan 1 och 0 om båda motsvarande bits är 0. <br/> -Den returnerar 1, utom där motsvarande bitarna av båda parametrarna är 0. <br/> -De resulterande bitmönster är i ”Ange” (1 eller true) bitar för någon av de två operanderna. Flera flaggan bits kan anges om flera bits har värdet 1 i mask.  |
| Utdata             | En ny version av **flaggan**, med angivna bitarna i **mask** värdet 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Beskrivning        | Funktionen BitAnd anger lite angivna för en flagga som 0.                           |
| Funktionssignaturen | Int BitOr(mask, flag)                                                              |
| Indata             | 1. **mask.** Ett hexadecimalt värde som anger bitars att ändra på flaggan. <br/> 2. **flaggan.** Ett hexadecimalt värde som ska ha en viss bitars ändras   |
| Åtgärder         | Den här funktionen konverterar båda parametrarna till binär representation och jämför dem: <br/> -Anger lite till 0 om en eller båda av motsvarande bitar i **mask** och **flaggan** är 0 och 1 om båda motsvarande bitar är 1. <br/> -Returneras 0, utom där motsvarande bitarna av båda parametrarna är 1. Flera flaggan bits kan anges till 0 om flera bits har värdet 0 i **mask.** |
| Utdata             | En ny version av **flaggan** med angivna bitarna i **mask** anges som 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Beskrivning       | Funktionen DateTimeFormat används för att formatera ett datetime-värde i form av sträng till ett bestämt format.     |
| Funktionssignaturen   | Strängen DateTimeFormat (dateTime, format)      |
| Indata   | 1. dateTime. En sträng som representerar DateTime ska formateras.  <br/> 2. **format.** En sträng som representerar det format som ska konverteras till.  |   
| Åtgärder           | Den formatsträng som anges i formatet tillämpas på datum/tid i dateTime-sträng. <br/> Strängen som anges i formatet måste vara ett giltigt DateTime-format. Om det inte finns returneras ett fel som anger att formatet inte är ett giltigt DateTime-format. <br/> Exempel: DateTime (”2007-12-25” ”, åååå-MM-dd”) resulterar i ”2007-12-25”.|   
| Utdata     | En sträng som uppstår till följd av att tillämpa **format** till **dateTime.**   |

>[!Note]                                                                                                                                                                             
Tecken som kan skapa användardefinierade format, se [användardefinierade format för datum/tid](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Beskrivning       | ConvertSidToString konverterar en bytematris som innehåller en säkerhetsidentifierare till en sträng.         |
| Funktionssignaturen      | Strängen ConvertSidToString(ObjectSID)    |
| Indata  | **ObjectSID.** En bytematris som innehåller en säkerhetsidentifierare (SID).   |
| Åtgärder    | Den angivna binära SID konverteras till en sträng.    |
| Utdata              | En strängrepresentation av SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Beskrivning         | **ConvertStringToGuid** funktionen konverterar strängrepresentation av en GUID till en binär representation av GUID.      |
| Funktion            | Byte [] ConvertStringToGuid(stringGuid)  |  
| Indata              | **GUID.** En sträng formaterad i det här mönstret: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, där värdet för GUID är representeras som en serie hexadecimala siffror i grupper med 8, 4, 4, 4 och 12 siffror och avgränsade med bindestreck. Ett exempel på ett returvärde är ”382c74c3-721d-4f34-80e557657b6cbc27”.  |
| Åtgärder          | Strängen **Guid** anges i parametern 1 konverteras till dess binär representation. <br/> Om strängen inte är en representation av en giltig **Guid**, funktionen avvisar argumentet med följande fel: <br/> **Parametern för funktionen ConvertStringToGuid måste vara en sträng som representerar ett giltigt Guid.**  |
| Utdata              | En binär representation av Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Beskrivning         | Funktionen ReplaceString ersätter alla förekomster av en textsträng med en annan sträng.  |   
| Funktion            | Sträng ReplaceString (sträng, OldValue, NewValue)    |                                                                          
| Indata              | 1. **Sträng.** En sträng som ska ersätta värden. <br/> 2. **OldValue.** Sträng att söka efter och ersätta. <br/> 3. Nytt-värde. Strängen som ska ersätta. |
| Åtgärder          | Alla förekomster av OldValue i strängen ersätts med NewValue. Funktionen måste kunna hantera följande specialtecken: <br/> - **\n.** Ny rad. <br/> - **\r.** Vagnretur. <br/> - **\t.** Fliken. <br/> Exempel: ReplaceString (”One\n\rMicrosoft\n\r\Way”, ”\n\r” ”,”) returnerar ”One Microsoft Way”. |   
| Utdata              | En sträng med alla förekomster av **OldValue** i strängen ersätts med **NewValue.**      |

## <a name="data-retrieval-functions"></a>Funktioner för hämtning av data

Funktioner för hämtning av data används för att utföra åtgärder som att hämtar önskad tecken från en sträng.

| Word       |        |
|--------------------|---------------|
| Beskrivning        | Funktionen Word returnerar ett ord som ingår i en sträng, baserat på parametrarna som beskriver avgränsare ska användas och word-nummer för att returnera.                                                                |
| Funktionssignaturen | Sträng Word (sträng, siffra, avgränsare)                                                                                                                                                                        |
| Indata             | 1. **sträng.** Strängen som ska returnera ett ord. <br/> 2. **nummer.** Ett värde som identifierar vilka word tal som ska returneras. <br/> 3. **avgränsare.** En sträng som representerar avgränsare som ska användas för att identifiera ord. |
| Åtgärder         | Varje sträng med tecken i en sträng som separeras med ett tecken i avgränsare identifieras som ett ord. Det ord som hittades på den plats som anges i parametern 3 (nummer) returneras: <br/> -Om number < 1, returnerar en tom sträng. <br/> -Om strängen är null returneras en tom sträng. <br/><br/> Exempel: <br/> 1. Word (”testa; för % funktionen;”, 3 ”,; $& %") returnerar ”används”. <br/> 2. Word (”testa; Funktionen ”, 2,” ”;) Returnerar ”” (en tom sträng). 3. Word (”testa; för % funktionen;”, 0 ”,; $& %") returnerar ”” (en tom sträng).
| Utdata             | En sträng som innehåller ordet vid position användaren uppge. Om **sträng** innehåller mindre än antalet ord, eller **sträng** innehåller inte några ord som identifieras av **avgränsare,** returneras en tom sträng. |  


| Vänster               |   |
|-------|-------|
| Beskrivning        | Funktionen vänster returnerar ett angivet antal tecken från vänster i en sträng.       |
| Funktionssignaturen | Sträng Left (sträng, numChars)     |
| Indata             | 1. **sträng.** Strängen som ska returnera tecken. 2. **numChars.** Ett nummer som identifierar antalet tecken ska returneras från början av en sträng.         |
| Åtgärder         | **numChars** tecken som ska returneras från den första positionen i strängen. <br/> Exempel: Left (”Britta Simon”, 3) returnerar ”Bri”.   |
| Utdata             | En sträng som innehåller de första numChars tecken i strängen.  <br/> -Om numChars = 0, returneras en tom sträng. <br/> -Om numChars < 0, returnerar en indatasträng. <br/> -Om strängen är null returneras en tom sträng. |




| Höger       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Beskrivning | Funktionen höger returnerar ett angivet antal tecken från höger (end) i en sträng.                                 |
| Funktionssignaturen   | Sträng höger (sträng, numChars)   |
| Indata      | 1. **Sträng.** Strängen som ska returnera tecken. <br/> 2. **numChars.** Ett nummer som identifierar antalet tecken ska returneras från slutet av strängen.  |
| Åtgärder  | **numChars.** Tecken som ska returneras från slutet av en sträng. <br/> Exempel: Höger (”Britta Simon”, 3) returnerar ”måndag”.                  |
| Utdata      | En sträng som innehåller de senaste numChars tecken i en sträng. Om numChars = 0, returneras en tom sträng. <br/> -Om **numChars** < 0, returnera en indatasträng. <br/> -Om strängen är null returneras en tom sträng. <br/> -Om strängen innehåller färre tecken än antalet angivna i numChars, returneras en sträng som är identiska till sträng. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Beskrivning | Funktionen Mid returnerar ett angivet antal tecken från en angiven position i en sträng.                              |
| Funktionssignaturen    | Strängen Mid(string, pos, numChars)                                                                                             |
| Indata      | 1. **sträng.** Strängen som ska returnera tecken.   <br/> 2. **pos.** Ett nummer som anger startpositionen i en sträng för att returnera tecken. <br/> 3. **numChars.** Ett nummer som identifierar antalet tecken ska returneras från en position i strängen.  |
| Åtgärder  | Returnera **numChars** tecken med början från positionen **pos** i strängen. <br/>Exempel: Mid (”Britta Simon”, 3, 5) returnerar ”itta”. |
| Utdata      | En sträng som innehåller **numChars** tecken från position **pos** i en sträng: <br/> -Om **numChars** = 0, returneras en tom sträng. <br/> -Om **numChars** < 0, returneras en tom sträng. <br/> -Om **pos** > längden på strängen, returnera en indatasträng. <br/> -Om **pos** ≤ 0, returnera en indatasträng. <br/> -Om **sträng** är null returneras en tom sträng. <br/> Om det finns inga **numChar** tecken kvar i **sträng** från position **pos**eftersom många tecken som kan returneras som ska returneras.

## <a name="data-generation-functions"></a>Funktioner för generering av data

Data generation funktioner används för att generera värden för specifika datatyper.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Beskrivning        | CRLF genererar en transport radmatningstecken. Du kan använda den här funktionen för att lägga till en ny rad. |
| Funktionssignaturen | Sträng CRLF                                                                              |
| Indata             | Inga parametrar                                                                            |
| Åtgärder         | En CRLF returneras.                                                                      |
|                    | Exempel: AddressLine1 + CRLF() + AddressLine2 resulterar i: <br/> -AddressLine1 <br/> -AddressLine2 |
| Utdata             | En CRLF är utdata.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Beskrivning        | Funktionen RandomNum returnerar ett slumptal inom ett visst intervall.                                       |   
| Funktionssignaturen | Int RandomNum(start, end)                                                                                         |   
| Indata             | - **Starta**. Ett nummer som identifierar den nedre gränsen för slumpmässiga värdet ska genereras.   <br/> - **end**. Ett nummer som identifierar den övre gränsen för slumpmässiga värdet ska genereras.  |
| Åtgärder         | Ett slumptal som är större än eller lika med **starta** och mindre än eller lika med **end** genereras. <br/>  Exempel: Random(0,999) kunde returnera 100.                      |
| Utdata             | Ett slumptal inom det angivna intervallet **starta** och **end**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Beskrivning        | Den *EscapeDNComponent* metoden bearbetar Indatasträngen baserat på vilken typ av hanteringsagenten som används. |
| Funktionssignaturen | Strängen EscapeDNComponent(string)                                                                                  |
| Indata     | **strängen**. En sträng som används för att bearbeta ett unikt namn. Strängen får inte innehålla kommenterad tecken. |
| Åtgärder | Metoden EscapeDNComponent från MIISUtils används för att utföra den här åtgärden. Den här metoden bearbetar Indatasträngen baserat på vilken typ av hanteringsagenten som används. <br/> Eftersom olika hanteringsagenter kräver olika huvudnamnet format, bearbetar den här metoden indatasträng baserat på vilken typ av hanteringsagenten. Typerna är LDAP för Lightweight Directory Access Protocol) huvudnamnet sådana asActive Directory® Domain Services, Sun Directory Server (tidigare iPlanet Directory Server), Microsoft Exchange Server. hierarkiska nonLDAP, till exempel Microsoft Lotus Notes; och extrinsic som databasen och XML utan LDAP unika namn. <br/> ** LDAP unikt namn: ** <br/> – Alla ogiltiga XML-tecken i värde-delen av en viss del är hexadecimal-kodad. <br/>– Alla ogiltiga tecken (inklusive ogiltiga XML-tecken) i del av en viss del genererar ett fel. <br/> -Följande tecken är undantagna: <br/> &nbsp;&nbsp;&nbsp;-Kommatecken (',') <br/> &nbsp;&nbsp;&nbsp;-Lika med-tecken ('=') <br/> &nbsp;&nbsp;&nbsp;-Plustecknet (”+”) <br/> &nbsp;&nbsp;&nbsp;-Mindre-än-tecken (”<”) <br/> &nbsp;&nbsp;&nbsp;-Större-än-tecken ('> ') <br/> &nbsp;&nbsp;&nbsp;-Nummertecken (#) <br/> &nbsp;&nbsp;&nbsp;-Semikolon (;') <br/> &nbsp;&nbsp;&nbsp;-Omvänt snedstreck ('\') <br/> &nbsp;&nbsp;&nbsp;-Citattecken (”” ”) <br/> – Om det sista tecknet i strängen är ett blanksteg, hoppas det utrymmet. <br/> – Alla överflödig inledande eller avslutande blanksteg runt en delnamn har tagits bort. <br/> -För XML-hanteringsagenten om det finns flera delar sedan delarna visas i bokstavsordning. <br/> – Om flera delar anges, är det sammansatta unika namnet sammanfogning av de enskilda strängar åtskilda av plustecken. <br/> -Ett fel genereras om den inmatade strängen inte är en giltig, LDAP-format unikt namnsträngen. <br/><br/> **Hierarkisk icke-LDAP** <br/> -Dessa hanteringsagenter stöder inte multipart-komponenter. Om flera strängar skickas till EscapeDNComponent genereras ett ArgumentException. <br/> – Om något tecken i strängen är ogiltiga XML-tecken genereras ett ArgumentException. <br/> -Alla kommatecken och omvända snedstreck i Indatasträngen är undantagna. <br/> – Om det sista tecknet i strängen är ett blanksteg, hoppas det utrymmet. <br/><br/> **Extrinsic:** <br/> 1. Om någon del är binär eller innehåller ett ogiltigt XML-tecken, lagras den del som en hexadecimal-kodad version av rådata med tecknet '#' framför strängen som prefix. Till exempel om en del var 'AxC' (där x representerar ett ogiltigt XML-tecken, till exempel '0x10'), är den del kodad som '#410010004300'. <br/> 2. Annars är alla instanser av följande tecken undantagna: <br/> &nbsp;&nbsp;&nbsp;-Omvänt snedstreck ('\') <br/> &nbsp;&nbsp;&nbsp;-Kommatecken (',') <br/> &nbsp;&nbsp;&nbsp;-Plustecknet (‘+’) <br/> &nbsp;&nbsp;&nbsp;-Nummertecken (‘#’) <br/> 3. Om det sista tecknet i en viss del sträng kan hoppas det utrymmet. <br/> 4. Om flera delar anges, är det sammansatta unika namnet sammanfogning av de enskilda strängar som åtskilda av plustecken.
| Utdata      | En sträng som innehåller ett giltigt domännamn.                                                                                                                  |   

>[!NOTE]
Verifiering av unika namn är mindre strikta än den syntax som definierats i LDAP-specifikationer. EscapeDNComponent(String[]) tillåter ett namn som innehåller en kombination av en eller flera av tecknen ”a”-”z”, ”A” - Z, '0'-'9', '-', och '.'. <br/>
Det går inte att ange en binära delen med den här metoden. Det är dock möjligt att ha en binär del **CommitNewConnector** om huvudnamnet konstrueras utifrån fästpunkt attribut och ett av attributen fästpunkt är en binär typ.


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Beskrivning | Funktionen Null används för att definiera som den här MA saknar ett attribut med egna bidrag och att Attributprioritet fortsätter med nästa st. |   
| Funktionssignaturen    | Strängen är Null    |
| Indata      | Inga parametrar                                                                                                                                             |   
| Åtgärder  | Ett null-värde returneras. <br/> Exempel: IIF(Eq(domain), ”okänt”, Null())                                                                                           |   
| Utdata      | Ett null-värde är utdata.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Logik funktioner
Logik funktioner används för att utföra en åtgärd baserat på villkor som utvärderas av systemet.

| IIF        |  |
|-------------|---|
| Beskrivning | Funktionen IIF returnerar ett av en uppsättning möjliga värden baserat på ett angivet villkor.    |
| Funktionssignaturen   | Objektet IIF (villkor, värdeomsant, värdeomfalskt)   |                                                 |
| Indata      | 1. **Villkoret**. Ett värde eller uttryck som kan utvärderas till true eller false. 2. **värde_om_sant** ett värde som returneras om villkoret utvärderas till true. <br/> 3. **värdeomfalskt** ett värde som returneras om villkoret utvärderas till false. <br/><br/> Följande är funktioner som är tillgängliga för användning som uttryck i funktionen IIF som **villkor:** <br/> **EQ.** Den här funktionen Jämför två argument sinsemellan. <br/> **NotEquals.** Den här funktionen Jämför två argument för ojämlikhet, returnerar true om de inte är lika med och FALSKT om de är lika.<br/> Exempel: NotEquals (EmployeeType, ”leverantör”)<br/> **LessThan.** Den här funktionen Jämför två talen, annars returnerar true om först är mindre än det andra och false.<br/>Exempel: LessThan (lön 100000) <br/>**GreaterThan.** Den här funktionen Jämför två talen, returnerar true om först annars är större än det andra och false.<br/> Exempel: GreaterThan (lön 100000) <br/> **LessThanOrEquals.** Den här funktionen Jämför två talen, returnerar true om först är mindre än eller lika med den andra och false i annat.<br/>Exempel: LessThanOrEquals (lön 100000) <br/> **GreaterThanOrEquals.* Den här funktionen Jämför två talen, returnerar true om först är större än eller lika med den andra och false annars. <br/>Exempel: GreaterThanOrEquals (lön 100000)<br/> IsPresent. Den här funktionen tar som indata för ett attribut i ILM-schemat och returnerar true om det inte är null och false om attributet är null.|
| Åtgärder  | Om **villkoret** utvärderas till true, returnerar **värde_om_sant.** Annars returnerar **värdeomfalskt.** <br/>Exempel: IIF (Eq (EmployeeType, ”interna”), ”t-” + Alias, Alias) returnerar alias för en användare med ”t-” läggs till i början av det om användaren är en intern. Annars returnerar användarens alias som är. |
| Utdata      | Utdata är **värdeomsant** om villkoret är SANT eller **värdeomfalskt** om villkoret är FALSKT. |      
