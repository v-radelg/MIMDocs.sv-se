---
title: Allmän LDAP-anslutning | Microsoft Docs
description: Den här artikeln beskriver hur du konfigurerar Microsofts allmänna LDAP-anslutning.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.reviewer: davidste
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 06/26/2018
ms.author: billmath
ms.openlocfilehash: bb6460ebf4106aa8c9295be0db3ce9da426b0778
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044402"
---
# <a name="generic-ldap-connector-technical-reference"></a>Teknisk referens för allmän LDAP-anslutning
Den här artikeln beskriver den allmänna LDAP-anslutningen. Artikeln gäller följande produkter:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Måste använda Hotfix 4.1.3671.0 eller senare [KB3092178](https://support.microsoft.com/kb/3092178).

För MIM2016 och FIM2010R2 är anslutningen tillgänglig som en nedladdning från [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

När du refererar till IETF RFC: er använder det här dokumentet formatet (RFC [RFC Number]/[section i RFC-dokument]), till exempel (RFC 4512/4.3).
Du hittar mer information på [https://tools.ietf.org/](https://tools.ietf.org/). I den vänstra panelen anger du ett RFC-nummer i dialog rutan **dokument hämtning** och testar det för att kontrol lera att det är giltigt.

## <a name="overview-of-the-generic-ldap-connector"></a>Översikt över den allmänna LDAP-anslutningen
Med den allmänna LDAP-anslutningen kan du integrera synkroniseringstjänsten med en LDAP v3-Server.

Vissa åtgärder och schema element, till exempel de som krävs för att utföra delta import, anges inte i IETF RFC: er. För dessa åtgärder stöds endast LDAP-kataloger som uttryckligen anges.

För att ansluta till katalogerna, testa vi med rot-/administratörs kontot.  Om du vill använda ett annat konto för att använda mer detaljerade behörigheter kan du behöva granska med LDAP-katalogen.

I ett perspektiv med hög nivå stöds följande funktioner av den aktuella versionen av Connector:

| Funktion | Support |
| --- | --- |
| Ansluten data Källa |Anslutningen stöds med alla LDAP v3-servrar (RFC 4510-kompatibel). Den har testats med följande: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory global katalog (AD GC)</li><li>389-katalog server</li><li>Apache-katalog server</li><li>IBM Tivoli DS</li><li>Isode-katalog</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Öppna DJ</li><li>Öppna DS</li><li>Öppna LDAP (openldap.org)</li><li>Oracle (tidigare) katalog Server Enterprise Edition</li><li>RadiantOne Virtual Directory Server (VDS)</li><li>Sun, en katalog server</li><li>Microsoft Active Directory Domain Services (AD DS)</li><ul><li>I de flesta fall måste du använda den inbyggda Active Directory anslutningen i stället för att vissa funktioner kanske inte fungerar</li></ul>**Viktiga kända kataloger eller funktioner stöds inte:**<li>Microsoft Active Directory Domain Services (AD DS)<ul><li>Tjänsten för meddelanden om lösen ords ändring (PCNS)</li><li>Exchange-etablering</li><li>Ta bort Active Sync-enheter</li><li>Stöd för nTDescurityDescriptor</li></ul></li><li>Oracle Internet-katalog (OID)</li> |
| Scenarier |<li>Hantering av objekt livs cykel</li><li>Grupp hantering</li><li>Lösen ords hantering</li> |
| Åtgärder |Följande åtgärder stöds i alla LDAP-kataloger: <li>Fullständig import</li><li>Exportera</li>Följande åtgärder stöds bara för angivna kataloger:<li>Deltaimport</li><li>Ange lösen ord, ändra lösen ord</li> |
| Schema |<li>Schemat har upptäckts från LDAP-schemat (RFC3673 och RFC4512/4.2)</li><li>Stöder strukturella klasser, AUX-klasser och extensibleObject objekt klass (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Delta import och stöd för lösen ords hantering
Kataloger som stöds för delta import och lösen ords hantering:

* Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord
* Microsoft Active Directory global katalog (AD GC)
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord
* 389-katalog server
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* Apache-katalog server
  * Har inte stöd för delta import eftersom den här katalogen inte har någon beständig ändrings logg
  * Har stöd för ange lösen ord
* IBM Tivoli DS
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* Isode-katalog
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* Novell eDirectory och NetIQ eDirectory
  * Stöder åtgärder för att lägga till, uppdatera och byta namn för delta import
  * Har inte stöd för borttagnings åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* Öppna DJ
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* Öppna DS
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* Öppna LDAP (openldap.org)
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord
  * Har inte stöd för ändrings lösen ord
* Oracle (tidigare) katalog Server Enterprise Edition
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* RadiantOne Virtual Directory Server (VDS)
  * Måste använda version 7.1.1 eller senare
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord
* Sun, en katalog server
  * Stöder alla åtgärder för delta import
  * Har stöd för ange lösen ord och ändra lösen ord

### <a name="prerequisites"></a>Förutsättningar
Innan du använder anslutningen måste du kontrol lera att du har följande på servern för synkronisering:

* Microsoft .NET 4.5.2-ramverk eller senare

### <a name="detecting-the-ldap-server"></a>Identifiera LDAP-servern
Anslutningen bygger på olika metoder för att identifiera och identifiera LDAP-servern. Anslutningen använder rot-DSE, leverantörs namn/version och kontrollerar schemat för att hitta unika objekt och attribut som är kända för att finnas i vissa LDAP-servrar. Dessa data, om de hittas, används för att i förväg fylla i konfigurations alternativen i anslutningen.

### <a name="connected-data-source-permissions"></a>Behörigheter för anslutna data källor
Om du vill utföra import-och export åtgärder för objekten i den anslutna katalogen måste anslutnings kontot ha tillräcklig behörighet. Anslutningen behöver Skriv behörighet för att kunna exportera och läsa behörigheter för att kunna importera. Behörighets konfigurationen utförs i själva mål katalogens hanterings upplevelser.

### <a name="ports-and-protocols"></a>Portar och protokoll
Anslutningen använder det port nummer som anges i konfigurationen, vilket som standard är 389 för LDAP och 636 för LDAPs.

För LDAPs måste du använda SSL 3,0 eller TLS. SSL 2,0 stöds inte och kan inte aktive ras.

### <a name="required-controls-and-features"></a>Nödvändiga kontroller och funktioner
Följande LDAP-kontroller/funktioner måste vara tillgängliga på LDAP-servern för att anslutningen ska fungera korrekt:  
`1.3.6.1.4.1.4203.1.5.3` TRUE/FALSE-filter

True/false-filtret rapporteras ofta inte som stöds av LDAP-kataloger och kan visas på den **globala sidan** under **obligatoriska funktioner som inte hittas**. Den används för att skapa **eller** filtrera i LDAP-frågor, till exempel när du importerar flera objekt typer. Om du kan importera fler än en objekt typ stöder LDAP-servern den här funktionen.

Om du använder en katalog där en unik identifierare är fäst punkten måste följande också vara tillgängligt (mer information finns i avsnittet [Konfigurera ankare](#configure-anchors) ):  
`1.3.6.1.4.1.4203.1.5.1` alla operativa attribut

Om katalogen har fler objekt än vad som får plats i ett anrop till katalogen, rekommenderar vi att du använder växling. För att växlingen ska fungera måste du ha ett av följande alternativ:

**Alternativ 1:**  
`1.2.840.113556.1.4.319` pagedResultsControl

**Alternativ 2:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

Om båda alternativen är aktiverade i anslutnings konfigurationen används pagedResultsControl.

`1.2.840.113556.1.4.417` ShowDeletedControl

ShowDeletedControl används endast med import metoden USNChanged delta för att kunna Visa borttagna objekt.

Anslutningen försöker identifiera de alternativ som finns på servern. Om det inte går att identifiera alternativen finns det en varning på den globala sidan i kopplings egenskaperna. Alla LDAP-servrar visar inte alla kontroller/funktioner som de stöder och även om den här varningen finns kan anslutningen fungera utan problem.

### <a name="delta-import"></a>Deltaimport
Delta import är endast tillgängligt när en support katalog har identifierats. Följande metoder används för närvarande:

* LDAP-Accesslog. Se [http://www.openldap.org/doc/admin24/overlays.html#Access loggning](http://www.openldap.org/doc/admin24/overlays.html#Access%20Logging)
* LDAP-ändringsloggen. Se [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Tidsstämpel. För Novell/NetIQ-eDirectory använder anslutnings tjänsten senaste datum/tid för att skapa och uppdatera objekt. Novell/NetIQ-eDirectory tillhandahåller inte ett likvärdigt sätt för att hämta borttagna objekt. Det här alternativet kan också användas om inga andra delta-import metoder är aktiva på LDAP-servern. Det här alternativet kan inte importera borttagna objekt.
* USNChanged. Se: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Stöds inte
Följande LDAP-funktioner stöds inte:

* LDAP-referenser mellan servrar (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Skapa en ny anslutning
Om du vill skapa en allmän LDAP-anslutning väljer du **hanterings agent** i **synkroniseringstjänst** och **skapar**. Välj den **allmänna LDAP-anslutningen (Microsoft)** .

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Anslutningar
På sidan anslutningar måste du ange information om värden, porten och bindningen. Beroende på vilken bindning som har valts kan ytterligare information anges i följande avsnitt.

![Anslutningar](./media/microsoft-identity-manager-2016-connector-genericldap/connectivity.png)

* Inställningen tids gräns för anslutning används bara för den första anslutningen till servern när schemat identifieras.
* Om bindningen är anonym används varken username/Password eller Certificate.
* För andra bindningar anger du information antingen i användar namn/lösen ord eller väljer ett certifikat.
* Om du använder Kerberos för att autentisera kan du även ange användarens domän/domän.

Text rutan **attribut-alias** används för attribut som definieras i schemat med RFC4522-syntax. De här attributen kan inte identifieras under schema identifieringen och anslutnings programmet behöver hjälp för att identifiera attributen. Till exempel måste följande anges i rutan alias för att korrekt identifiera attributet userCertificate som ett binärt attribut:

`userCertificate;binary`

Följande är ett exempel på hur den här konfigurationen kan se ut:

![Anslutningar](./media/microsoft-identity-manager-2016-connector-genericldap/connectivityattributes.png)

Markera kryss rutan **Inkludera operativa attribut i schemat** om du även vill inkludera attribut som skapats av servern. Detta inkluderar attribut som när objektet skapades och senaste uppdaterings tid.

Välj **Inkludera utöknings Bart attribut i schemat** om utöknings bara objekt (RFC4512/4.3) används och aktivering av det här alternativet tillåter att alla attribut används för alla objekt. Om du väljer det här alternativet blir schemat mycket stort, om inte den anslutna katalogen använder den här funktionen rekommenderar vi att du låter alternativet vara omarkerat.

### <a name="global-parameters"></a>Globala parametrar
På sidan globala parametrar konfigurerar du DN till förändrings loggen och ytterligare LDAP-funktioner. Sidan fylls i automatiskt med den information som finns på LDAP-servern.

![Anslutningar](./media/microsoft-identity-manager-2016-connector-genericldap/globalparameters.png)

I det övre avsnittet visas information som tillhandahålls av själva servern, till exempel namnet på servern. Anslutningen verifierar också att de obligatoriska kontrollerna finns i rot-DSE. Om dessa kontroller inte visas visas en varning. Vissa LDAP-kataloger visar inte alla funktioner i rot-DSE och det är möjligt att kopplingen fungerar utan problem även om det finns en varning.

Kryss rutorna **kontroller som stöds** styr beteendet för vissa åtgärder:

* När Tree Delete är markerat tas en hierarki bort med ett LDAP-anrop. När borttagning av trädet är omarkerad gör kopplingen en rekursiv borttagning om det behövs.
* När växlade resultat har marker ATS gör kopplingen en växlad import med den storlek som anges i körnings stegen.
* VLVControl och SortControl är ett alternativ till pagedResultsControl för att läsa data från LDAP-katalogen.
* Om alla tre alternativen (pagedResultsControl, VLVControl och SortControl) är avmarkerade importerar anslutningen alla objekt i en åtgärd, vilket kan Miss förväntas om det är en stor katalog.
* ShowDeletedControl används endast när delta import metoden är USNChanged.

Ändrings loggens unika namn är namngivnings kontexten som används av förändrings loggen, till exempel **CN = ändringsloggen**. Det här värdet måste anges för att kunna utföra delta import.

Följande är en lista över standard-DNs för ändrings logg:

| Directory- | Delta ändrings logg |
| --- | --- |
| Microsoft AD LDS och AD GC |Identifieras automatiskt. USNChanged. |
| Apache-katalog server |Inte tillgängligt. |
| Katalog 389 |Ändrings logg. Standardvärde som ska användas: **CN = ändringsloggen** |
| IBM Tivoli DS |Ändrings logg. Standardvärde som ska användas: **CN = ändringsloggen** |
| Isode-katalog |Ändrings logg. Standardvärde som ska användas: **CN = ändringsloggen** |
| Novell/NetIQ eDirectory |Inte tillgängligt. Tidsstämpel. Anslutnings programmet använder senast uppdaterad datum/tid för att hämta nya och uppdaterade poster. |
| Öppen DJ/DS |Ändrings logg.  Standardvärde som ska användas: **CN = ändringsloggen** |
| Öppna LDAP |Åtkomst logg. Standardvärde som ska användas: **CN = accesslog** |
| Oracle-DSEE |Ändrings logg. Standardvärde som ska användas: **CN = ändringsloggen** |
| RadiantOne VDS |Virtuell katalog. Är beroende av den katalog som är ansluten till VDS. |
| Sun, en katalog server |Ändrings logg. Standardvärde som ska användas: **CN = ändringsloggen** |

Attributet Password är namnet på attributet som anslutningen ska använda för att ange lösen ordet i åtgärder för lösen ords ändring och lösen ords uppsättning.
Det här värdet är som standard inställt på **userPassword** , men kan ändras när det behövs för ett visst LDAP-system.

I listan ytterligare partitioner är det möjligt att lägga till ytterligare namn områden som inte identifieras automatiskt. Den här inställningen kan till exempel användas om flera servrar utgör ett logiskt kluster som ska importeras samtidigt. Precis som Active Directory kan ha flera domäner i en skog, men alla domäner delar ett schema, kan samma sätt simuleras genom att ange ytterligare namn rymder i den här rutan. Varje namnrymd kan importera från olika servrar och konfigureras ytterligare på sidan Konfigurera partitioner och hierarkier. Använd CTRL + RETUR för att hämta en ny rad.

### <a name="configure-provisioning-hierarchy"></a>Konfigurera etablerings hierarki
Den här sidan används för att mappa DN-komponenten, till exempel OU, till den objekt typ som ska tillhandahållas, till exempel organizationalUnit.

![Etablerings hierarki](./media/microsoft-identity-manager-2016-connector-genericldap/provisioninghierarchy.png)

Genom att konfigurera en etablerings-hierarki kan du konfigurera anslutningen till att automatiskt skapa en struktur när det behövs. Om det till exempel finns ett namn område DC = contoso, DC = com och ett nytt objekt, CN = Johan, OU = Seattle, c = US, DC = contoso, DC = com har skapats, kan kopplingen skapa ett objekt av typen land för oss och en organizationalUnit för Seattle om de inte redan finns i katalogen.

### <a name="configure-partitions-and-hierarchies"></a>Konfigurera partitioner och hierarkier
På sidan partitioner och hierarkier väljer du alla namn områden med objekt som du vill importera och exportera.

![Partitioner](./media/microsoft-identity-manager-2016-connector-genericldap/partitions.png)

För varje namnrymd är det också möjligt att konfigurera anslutnings inställningar som åsidosätter de värden som anges på anslutnings skärmen. Om dessa värden lämnas till standardvärdet som är tomt används informationen från anslutnings skärmen.

Det är också möjligt att välja vilka behållare och organisationsenheter som anslutningen ska importera från och exportera till.

När du utför en sökning sker detta på alla behållare i partitionen. I de fall där det finns ett stort antal behållare leder detta till försämrade prestanda.

> [!NOTE]
> Från och med mars 2017-uppdateringen av den allmänna LDAP-anslutningsprogrammet kan endast de valda behållarna begränsas. Det kan du göra genom att markera kryss rutan Sök endast i valda behållare som visas på bilden nedan.

![Sök endast efter valda behållare](./media/microsoft-identity-manager-2016-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Konfigurera ankare
Den här sidan har alltid ett förkonfigurerat värde och kan inte ändras. Om Server leverantören har identifierats kan ankaret fyllas i med ett attribut som inte kan ändras, till exempel GUID för ett objekt. Om den inte har identifierats eller är känd att inte ha ett attribut som inte kan ändras använder anslutnings tjänsten DN (unikt namn) som ankare.

![ankare](./media/microsoft-identity-manager-2016-connector-genericldap/anchors.png)


Följande är en lista över LDAP-servrar och ankaret som används:

| Directory- | Anchor-attribut |
| --- | --- |
| Microsoft AD LDS och AD GC |objectGUID |
| 389-katalog server |dn |
| Apache-katalog |dn |
| IBM Tivoli DS |dn |
| Isode-katalog |dn |
| Novell/NetIQ eDirectory |GUID |
| Öppen DJ/DS |dn |
| Öppna LDAP |dn |
| Oracle-ODSEE |dn |
| RadiantOne VDS |dn |
| Sun, en katalog server |dn |

## <a name="other-notes"></a>Andra anteckningar
Det här avsnittet innehåller information om aspekter som är speciella för den här anslutningen eller av andra orsaker är viktiga att känna till.

### <a name="delta-import"></a>Deltaimport
Delta vattenstämpeln i Open LDAP är UTC-datum/tid. Av den anledningen måste klockorna mellan FIM-synkroniseringstjänsten och öppna LDAP synkroniseras. Om inte, kan vissa poster i förändrings loggs loggen utelämnas.

Delta importen identifierar inte några objekt borttagningar för Novell eDirectory. Därför är det nödvändigt att köra en fullständig import regelbundet för att hitta alla borttagna objekt.

För kataloger med ändrings loggen som baseras på datum/tid rekommenderar vi starkt att köra en fullständig import vid regelbundna tidpunkter. Den här processen gör att Synkroniseringsmotorn kan hitta och skilja mellan LDAP-servern och vad som finns i anslutnings utrymmet.

## <a name="troubleshooting"></a>Felsökning
* Information om hur du aktiverar loggning för fel sökning av anslutnings programmet finns i [så här aktiverar du ETW-spårning för anslutningar](http://go.microsoft.com/fwlink/?LinkId=335731).
