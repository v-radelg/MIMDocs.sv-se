---
title: Kontrollen visas konfigurationen XML-Resursreferensen | Microsoft Docs
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Kontrollen visas konfigurationen XML-Resursreferensen

Resursen är kontrollen visas konfigurationen (RCDC) användardefinierade resurser som du kan använda för att styra hur andra resurser i datalagret Microsoft Identity Manager 2016 SP1 (MIM) visas i användargränssnittet (UI) för slutanvändaren. Alla RCDC-resurser innehåller en XML-konfigurationsfil som du kan ändra om du vill lägga till, ändra eller ta bort UI text och UI-kontroller. Medan MIM 2016 SP1 innehåller flera standard RCDC-resurser, kan du också skapa anpassade RCDC-resurser för anpassade resurser. Mer information om hur du använder RCDC UI i FIM-portalen finns [introduktion till konfigurera och anpassa FIM-portalen](http://go.microsoft.com/fwlink/?LinkID=165848) i FIM-dokumentationen.


## <a name="known-issues"></a>Kända problem

Standardvärdet i många Resource Control Display Configuration-kontroller stöds inte

I den här versionen stöds inte inställningen standardvärden i kontroller i en resurskontroll förutom alternativknapp. Du kan undvika det här problemet för en listruta genom att ange ett standardvärde som inte är associerad med ett värde och tvinga användaren att ändra valet. Du kan lösa problemet med andra kontroller, måste du använder ett godkännande arbetsflöde för att tillhandahålla ett standardvärde under överföringen av begäran.

## <a name="basic-structure"></a>Grundläggande struktur

XML-data för en resurs i RCDC består av det XML-elementet **ObjectControlConfiguration.**

>[!NOTE]
Fullständig XSD-schemat finns i bilaga A: standard XSD-Schema senare i det här dokumentet.

Följande är XSD-schemat för ObjectControlConfiguration-elementet:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



Den **ObjectControlConfiguration** elementet innehåller följande:

1.  **ObjectDataSource**: det här elementet anger TypeName i en datakällsklass som använder RC (Resource Control). En beskrivning och schemadefinitionen finns i det följande avsnittet för Data i det här dokumentet. En **ObjectControlConfiguration** -element kan innehålla upp till 32 noder i den **ObjectDataSource** element.

2.  **XmlDataSource**: Detta är en enkel datakälla som oftast används för att ange en sammanfattningssida design. En beskrivning och schemadefinitionen finns i det följande avsnittet för Data i det här dokumentet. En **ObjectControlConfiguration**: elementet kan innehålla upp till 32 noder i den **XmlDataSource** element.

3.  **Panelen**: administratören kan anpassa layouten till sidan RCDC genom att ändra elementen i panelen-element. Mer information finns i avsnittet panelen senare i det här dokumentet. En **ObjectControlConfiguration** -elementet måste ha endast ett element i Kontrollpanelen.

4.  **Händelser**: Administratörer kan inte tillhandahålla anpassade koden bakom, den här funktionen är begränsad. Detta är den händelse som kan generera en panel eller en kontroll, baserat på en tillståndsändring. Mer information finns i avsnittet händelser senare i det här dokumentet. En **ObjectControlConfiguration** element kan innehålla eventuellt en **händelse** element. I allmänhet användningen av anpassade **händelser** stöds inte om inte specifikt har utvecklats i senare förbättringar.

## <a name="data-sources"></a>Datakällor

Microsoft Identity Manager använder datakällor som ett sätt att binda data till UI-komponenter. På så sätt kan du underlätta uppdelning av data från presentation lagret. Det finns två typer av datakällor i RCDC resurskonfigurationsdata: **ObjectDataSource** och **XmlDataSource**.

-   **ObjectDataSources** ange en Microsoft .NET-klass som skickar data till RC. Det finns en fast uppsättning tillgängliga typer av ObjectDataSources förutsatt att administratören kan välja att använda vid redigering av RCDCs.

-   **XMLDataSources** ger ett enkelt sätt att struktur XML-baserade data och de kan användas av administratörer för att ange anpassade data. XML-data måste anges direkt i RCDC, såvida du inte använda den inbyggda fördefinierade XML-strukturen. Den inbyggda XML-strukturen används för att generera sammanfattas i RC.

Du kan binda dessa datakällor attribut av UI-kontroller som anges i RCDC att generera Användargränssnittet i RCDC.

### <a name="objectdatasource"></a>ObjectDataSource

Microsoft Identity manager innehåller vanliga källa datatyperna i följande tabell som är tillgängliga för alla typer av resurser (förutom där annat anges).

| TypeName                        | Beskrivning     | Stöder dubbelriktad bindning | Stöd för bindning syntax        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Detta representerar FIM 2010-resurs som skapas, redigeras eller visades. Sökvägen i bindningen strängen är attributets namn. Observera att resurstypen anges av attributet TargetObjectType i RCDC i stället för i RCDC. Attributet ConfigurationData. | Ja                     | [AttributeName] Värdet för objektattributet som angetts av dess namn.    |
| PrimaryResourceDeltaDataSource  | Den här datakällan skapar delta XML som jämför det ursprungliga tillståndet och FIM 2010 resursens aktuella tillstånd. Genererade delta XML förbrukas av kontrollen för RC sammanfattning för att återge Användargränssnittet för begäran som användaren skickar.                                    | Nej                      | DeltaXml: </br> Detta används med sammanfattning kontrollen för att visa delta.                                                 |
| PrimaryResourceRightsDataSource | Den här datakällan innehåller infogade rättigheter för varje resurs för FIM 2010-attribut. Detta gör att RC för att bestämma innan skicka vilka behörigheter användaren har för attributet och sedan återge Användargränssnittet för attributet på lämpligt sätt.                     | Nej                      | [AttributeName]                                                                                         |
| SchemaDataSource                | Den här datakällan kan användas för att komma åt schema-relaterad information, till exempel namn, beskrivning, huruvida attributet är obligatoriskt, samt information om resurser.                                                                                             | Nej                      | [AttributeName]. **Krävs** booleskt värde som anger om attributet måste ha ett värde ska vara giltigt. <br/> [AttributeName]. **DisplayNameString** värde som anger bindningens visningsnamn <br/>[AttributeName]. **DescriptionString** värde som anger bindningens beskrivning <br/>[AttributeName]. StringRegexString-värde som anger strängen Regex av en bindning. <br/>[AttributeName]. **DisplayName** <br/> [AttributeName]. **Beskrivning** <br/> [AttributeName]. [AttributeName]. **IntergerValueMinimum** <br/>[AttributeName]. **IntergerValueMaximum** <br/>[AttributeName]. **LocalizedAllowedValues**|
| DomainDataSource                | Den här datakällan innehåller en uppräkning av domäner, baserat på konfigurationen resurser i domänen. Observera att den här datakällan kan användas i RCDCs som gäller gruppresurser och resurser för användare.                                                                           | Ja                     | Domain           |

Följande är ett exempel RCDC fragment som binder tre datakällor till UocTextBox kontrollen att redigera attributet beskrivning för en grupp:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Med hjälp av en **XMLDataSource**, du kan ange anpassade data i RCDC kan använda för en viss resurs. I det här fallet måste du ange XML-data i RCDC. Alternativt kan användas den här datakällan för att referera till en inbyggda XML-datastruktur för att återge Användargränssnittet för sammanfattning sidor. Du kan styra vilken typ av **XMLDataSource** ska användas när du definierar i RCDC.


| TypeName                 | Beskrivning   | | |
|--------------------------|------------|
| **XMLDataSource**            | Datakällan representerar XML-data. Det kan vara i XSL- eller inbäddade XSL-format: <br/>**XSL-format:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll < min: XmlDataSource min: Name = ” <br/>summaryTransformXsl"my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl ”>< / min: XmlDataSource ><br/> **Inbäddade XSL-format:** <br/> < min: XmlDataSource min: Name = ”RequestStatusTransformXsl” > <br/> < xsl: stylesheet version = ”1.0” xmlns: XSL = http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl = ”urn: schemas-microsoft-com:xslt” ><br/>< / xsl: stylesheet >< / min: XmlDataSource >                       |Nej | ```Xpath[;namespaces]``` <br/> Var: Xpath som en giltig XML-xpath Välj noteringen krävs oftast ”/” (rot) <br/>Namnområden är en valfri lista med prefix = URI strängar, avgränsade med semikolon, om det behövs för xpath arbeta mot namespaced XML. |
| **ReferenceDeltaDataSource** | Datakällan representerar går flervärdesattribut referensattribut. Den används endast på RCDC för gruppen och ange. <br/> Även om datakällan inte är begränsad till grupper eller uppsättningar kräver kodändringar i RCDC värden att skicka sådana går. Grupp- och Set för närvarande endast värdarna som identifierar den här datakällan.  | Ja                      | [AttributeName]. Lägg till där [AttributeName] representerar ett referensattribut och de data som returnerades är delta-tillägg. <br/> Exempel: [ReferenceAttribute]. Lägg till <br/>Exempel: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName]. Ta bort där [AttributeName] representerar ett referensattribut och de data som returnerades är delta borttagningar. <br/> DeltaXml |
|**RequestDetailsDataSource**| Datakällan representerar attributet RequestParameter för begäran-objekt. Parametern anger det maximala antalet attributvärden som ska visas per flervärdesattribut den används endast i RCDC för begäran. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| Nej | DeltaXml |
|**RequestStatusDataSource**| Datakällan representerar den **RequestStatusDetails** attribut för begäran-objekt. <br/>Den används endast i RCDC för begäran.  | Nej | DeltaXml |

-   Definiera en anpassad XML-datakälla:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Definiera datakällan på följande sätt om du vill använda den inbyggda sammanfattning kontrollen XSL:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Om du skapar en RCDC för en anpassad resurstyp kan du använda den här metoden för att återge automatiskt sammanfattningen för den anpassa resursen.

Följande är ett exempel på hur du skapar en sammanfattning flik i RCDC, med PrimaryResourceDeltaDataSource med XMLDataSource med hjälp av inbyggda XSL:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Alternativt kan kan användaren ersätta elementet XmlDataSource angetts tidigare med följande format för att definiera en anpassad layout för en sammanfattningssida. Som en referens, standard FIM 2010 sammanfattning XSL ingår i bilaga B: standard sammanfattning XSL senare i det här dokumentet.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Schemat för datakällor
Följande är XSD-schemat för två typer av datakällor:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>händelser
En händelse definierar status för en kontroll. Utökning av den här funktionen är begränsad eftersom du inte kan skriva en anpassad funktion (hanterare) definierar beteendet när en händelse utlöses. Samma händelse element kan användas i elementet panelen. Mer information finns i avsnittet panelen senare i det här dokumentet. Följande är XSD-schemat för elementet händelse:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


En händelse är ett tomt element och den har två attribut.

**Attribut:**

1.  **Namnet**: Detta är det unika namnet för en händelse. Det går endast att händelsen i den **ObjectControlConfiguration** är händelsen. Den här händelsen utlöses när sidan läses in först.

2.  **Hanteraren**: Detta är det unika namnet för en hanterare. När händelsen utlöses kallas vanligtvis en metod för programmet för att hantera ändring av status för kontrollen. stöds inte i följande fall: Om du tar bort en befintlig hanterare från en befintlig kontroll, skapa en ny hanterare och kopplas till en befintlig eller ny kontroll.

Exempel:

Följande är ett exempel på ett element för händelser.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panelen**


Panelen elementet är core-element i en RCDC layout. Följande är XSD-schemat för elementet panelen:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Det här elementet innehåller ett återkommande element gruppering. Mer information finns i avsnittet gruppering i det här dokumentet.

Panelen-elementet innehåller fyra attribut:

1.  **Namnet**: namnet på panelen. Detta är ett obligatoriskt, typen string-attribut.

2.  **DisplayAsWizard**: det här attributet för närvarande är föråldrad. Motsvarande VerbContext attribut för RCDC styr om resursen layouten är i guiden eller på fliken läge. Om anges till 0 (skapa-läge), är det också i guideläge. Annars är det i fliken läge. Mer information finns i introduktion till konfigurera och anpassa FIM-portalen i dokumentationen.

3.  **Bildtexten**: det här attributet för närvarande är föråldrad. Användaren kan ange beskrivningar för en sida genom att lägga till en grupp som innehåller endast huvudinformation. Mer information finns i avsnittet gruppering i det här dokumentet.

4.  **AutoValidate**: Detta är ett valfritt booleskt attribut. Utlöses när den är inställd på true, verifiering mot varje kontroll på den aktuella fliken. Som standard om attributet saknas, är den inställd på true. Det kan användas tillsammans med egenskapen reguljärt uttryck. Mer information finns i ”reguljärt uttryck” i ett senare avsnitt av det här dokumentet.

## <a name="grouping"></a>Gruppering

Gruppering-elementet definierar utseendet för en Panel. Det fungerar som en behållare som grupperar ihop enskilda kontroller i olika avsnitt och flikar. Följande är XSD-schemat för gruppering-element:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Det finns tre typer av **grupperingar**:

1.  **Huvudet gruppering**: A sidhuvud gruppering är valfritt. Det kan bara finnas en rubrik gruppering i en **panelen**. En rubrik gruppering visas ovanpå en panel som beskrivning.
    Endast en UocCaptionControl får användas i den här grupperingen. Ett exempel på en rubrik gruppering finns i avsnittet exempel.

2.  **Innehåll grupperingar**: minst en innehållsgrupperna krävs. Det kan finnas flera grupperingar som innehåll i en Panel. En gruppering av innehåll visas som innehållet i en RCDC-sida. Varje innehåll gruppering visas som en flik på samma panel och kan innehålla från 1 till 256 kontroller. Se avsnittet exemplet nedan ett exempel på en **innehållsgrupperna**.

3.  **Översikt över grupperingar**: A sammanfattning gruppering är valfritt. Det kan bara finnas en sammanfattning av gruppering på en Panel. En sammanfattning av gruppering visas som sista fliken för en Panel. Endast en **UocHtmlSummary** kontrollen kan användas i en sammanfattning av gruppering för att visa ändringarna som användaren har gjort innan du skickar en begäran. Se avsnittet exemplet nedan ett exempel på en sammanfattning av gruppering.

Varje typ av gruppering innehåller följande element:

1.  **Hjälpa**: det här elementet innehåller hjälptext som visas på en flik. Du kan också använda den för att lägga till en länk till en hjälpfil för fliken.

2.  **Kontroller**: information om det här elementet finns i avsnittet i det här dokumentet. Varje gruppering måste ha 1 och 256 kontroller portintervallet, beroende på vilken typ av grupperingen.

3.  **Händelser**: information om det här elementet finns i avsnittet händelser i det här dokumentet. Varje gruppering kan också ha en händelse. De händelser som stöds i elementet gruppering är följande:

    - **BeforeLeave**: den här händelsen utlöses när användaren är redo att lämna en flik i en innehåll gruppering.
    - **AfterEnter**: den här händelsen utlöses när användaren är redo att ange en flik i en innehåll gruppering.

Attribut:

1.  **Namnet**: Detta är det obligatoriska namnet på grupperingen. Den **namn** måste vara unika inom det **panelen**.

2.  **Bildtexten**: den **beskrivning** visas som rubrik beskrivning i ett sidhuvud gruppering. Det verkar som fliken titel på ett innehåll eller sammanfattning gruppering.

3.  **Beskrivning**: attributet valfri sträng **beskrivning** fungerar endast när den används i en innehåll gruppering. Använd det här elementet för att ge användaren vissa detaljer om information inom samma flik.

  >[!NOTE]
  Om det här attributet används i en sammanfattning av gruppering ska XML-filen anses vara ogiltig. Om det här attributet används i en rubrik gruppering anses XML vara giltiga men ignoreras.

4.  **Aktiverad**: ett valfritt booleskt attribut aktiverad har angetts till true om den saknas. Om aktiverad är inställd på false, ser användaren en inaktiverad flik. Det här attributet fungerar bara i en innehåll gruppering.

  >[!NOTE]
  Om det här attributet används i en sammanfattning av gruppering ska XML-filen anses vara ogiltig. Om det här attributet används i en rubrik gruppering anses XML vara giltiga men ignoreras.

5.  **Synliga**: du kan dölja en RCDC fliken eller dess rubrik genom att ange det här attributet till false. Standardvärdet är valfria, boolesk typ är attributet till true. Det här attributet fungerar bara på en innehåll gruppering.

  >[!NOTE]
  När det finns endast en innehållsgrupperna i en Panel, fungerar inte den här funktionen. När det finns mer än en gruppering av innehåll på en Panel, fungerar den enligt beskrivningen ovan.

6.  **IsHeader**: det här attributet är ett valfritt, booleskt attribut som definierar om grupperingen är en gruppering i huvudet. Om det här attributet inte anges är den inställd på false.

7.  **IsSummary**: Detta är ett valfritt, booleskt attribut som definierar om gruppen är en översikt över gruppering. Om det här attributet inte anges är den inställd på false.

![BEL konfigurations-XML](media/rcd-configuration-xml-reference/image005.jpg)

Följande exempelkod för XML-genererar tidigare sidhuvud grupperingen. Gruppering för huvudet är området med texten exempel sidhuvud gruppering.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![BEL konfigurations-XML](media\rcd-configuration-xml-reference/image007.jpg)

Följande exempelkod för XML-genererar tidigare innehållsgrupperna. Innehållsgrupperna är fliken längst till vänster med texten **exempel innehåll gruppering**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![BEL konfigurations-XML](media/rcd-configuration-xml-reference/image010.jpg)

Följande exempelkod för XML-genererar tidigare sammanfattning grupperingen. Sammanfattning-grupperingen är fliken längst till höger med texten **sammanfattning**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Hjälp

Hjälp-element kan som ett alternativ ingå i en grupp eller ett element i kontrollen. Om den används i en grupp, måste det vara det första elementet som används. Det ger textrepresentation hjälper till att slutanvändarna hjälper dem att ge korrekt information. Följande är XSD-schemat för hjälp-element:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Följande är ett exempel på Hjälp-elementet:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Kontroll

En gruppering-elementet innehåller ett eller flera element i kontrollen. Kontroller är uppgifterna i en RCDC. Du kan anpassa elementet gruppering genom att definiera de olika elementen i kontrollen som den innehåller. Följande är XSD-schemat för kontroll-element:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

En kontroll innehåller följande element:

1.  **Hjälpa**: det här elementet ignoreras. Den fungerar bara i gruppering.

2.  **Anpassade egenskaper**: det här elementet stöds inte.

3.  **Alternativ**: det här elementet används bara i kombination med den **UocDropDownList** eller **UocRadioButtonList** kontroller. Det fungerar inte med andra kontroller. Se avsnittet alternativ i det här dokumentet för strukturen på det här elementet. Se enskilda kontrollen för att se hur den används i kontexten för en kontroll.

4.  **Knapparna**: det här elementet används bara i kombination med den **UocListView** kontroll. Det fungerar inte för andra kontroller. Mer information finns i avsnittet UocListView i det här dokumentet.

5.  Egenskaper: Det här elementet används i alla kontroller för att ange ytterligare beteenden för en kontroll. Information om det här elementet finns i avsnittet Egenskaper i det här dokumentet.

6.  **Händelser**: strukturen för det här elementet finns i avsnittet händelser tidigare i det här dokumentet. Se enskilda kontrolldefinition att bestämma vilken händelse som ska användas i kontrollen.

En kontroll innehåller följande attribut:

1.  **Namnet**: Detta är namnet på kontrollen. Namnet på en kontroll måste vara unika inom varje panel. Detta är ett obligatoriskt, typen string-attribut.

2.  **TypeName**: det här attributet anger vilken typ av kontroll. Detta är ett obligatoriskt, typen string-attribut. Se avsnittet enskilda kontroller i det här dokumentet för varje kontrollnamn.

3.  **Bildtexten**: du kan använda det här attributet för att inkludera en beskrivning för kontrollen.
    Beskrivningen är vanligtvis visningsnamnet för de data som kontrollen visar eller mata in. Du kan uttryckligen ange ett värde för etiketten eller bindning med namnet schemainformation attribut visas. Beskrivningen visas till vänster i en normal storlek kontroll. Om en kontroll utsträckning hela skärmen beskrivningen visas på kontrollen. Det här är ett valfritt, attribut av typen string. Information om hur du binder en datakälla med ett attribut eller ett värde för egenskapen finns i avsnittet Egenskaper.

   I följande exempel visas hur en beskrivning kan användas explicit:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     I följande exempel visar hur du kan använda en rubrik med en datakälla. Om du använder mallen för en datakälla som du såg tidigare i det här dokumentet är datakällan schema. Vi rekommenderar att du binda attributets visningsnamn med attributet Beskrivning.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Aktiverad: Detta är ett valfritt, Boolean-type-attribut. Användaren kan inaktivera kontrollen genom att ange det här attributvärdet false. Standardvärdet är inställd på true.

5.  Synliga: Detta är ett valfritt, boolesk typ-attribut. Du kan använda det här attributet för att dölja hela kontrollen. Standardvärdet är inställd på true.

6.  Beskrivning: Med det här valfria, typen string-attributet innehåller en beskrivning som hjälper slutanvändaren att förstå vad de ska skriva i kontrollen eller vad kontrollen gör. Du kan uttryckligen ange ett värde för beskrivningen eller binda med schemainformation för attributet Beskrivning. <br/>Beskrivningen visas på till vänster i en normal storlek kontroll under rubriken. Om en kontroll utsträckning hela skärmen, visas beskrivningen överst kontrollen under rubriken. Information om hur du binder en datakälla med ett attribut eller ett värde för egenskapen finns i avsnittet Egenskaper i det här dokumentet.

7.  I följande exempel visas hur en beskrivning kan användas explicit:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  Det här exemplet visas hur en beskrivning kan användas med en datakälla. Om du använder mallen för en datakälla som du såg tidigare i det här dokumentet är datakällan **schemat**. Vi rekommenderar att du binda attributets **beskrivning** med attributet Beskrivning.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: Det här attributet anger om kontrollen sträcker sig över hela skärmen. Detta är ett valfritt, Boolean-type-attribut. Standardvärdet anges till false.

    >[!NOTE]
    Attributen rubrik och beskrivning inaktiveras när det här attributet har angetts till true. Du måste använda UocLabel-kontroll för att ge en beskrivning för en utökad kontroll.
9. **Tipset**: Detta är ett valfritt, attribut av typen string. Texten i attributet tipset hjälper slutanvändaren bestämma vad som är en giltig inmatning för kontrollen. Tipset visas under kontrollen.

10.  **AutoPostback**: Detta är ett valfritt, Boolean-type-attribut. Standardvärdet är false. Om värdet false, uppdatera sidan inte kan uppdatera kontrollen. För information om AutoPostback, leta efter Microsoft ASP.NET UI-egenskapen för kontrollen med samma namn.

11. **RightsLevel**: Detta är ett valfritt, attribut av typen string. Du kan binda det här attributet endast med infogad behörighet till en datakälla. Kontrollen är dynamiskt aktiverad eller inaktiverad, baserat på användarens rättigheter. Information om hur du binda datakällor med ett attribut eller ett värde för egenskapen finns i avsnittet Egenskaper i det här dokumentet.

    Det här exemplet visas hur en **RightsLevel** attributet kan användas med en datakälla. Om du använder mallen för en datakälla som du såg tidigare i det här dokumentet är datakällan **rättigheter**. Använd attributnamnet som sökväg.

### <a name="properties"></a>Enheter

Du kan använda en egenskap för att ytterligare anpassa beteendet för varje kontroll. En egenskap är ett tomt element. Följande är XSD-schemat för egenskapen-element:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Varje egenskap har två attribut:

1.  **Namnet**: den här typen string-attributet är det unika namnet på egenskapen.
    Olika kontroller har olika egenskaper. Det finns vissa vanliga egenskaper som kan användas av alla kontroller. Mer information om vilka namn som är tillgängliga för en viss kontroll finns i avsnittet gemensamma egenskaper och enskilda kontroller.

2.  **Värdet**: Detta är värdet för egenskapen. Datatypen för värdet underordnade på vilken egenskap som den är tilldelad till. Se avsnittet följande format för tillåtna värde för specifika egenskaper.

Vissa egenskaper kan bindas med information från en datakälla. Om du vill göra det, som du behöver använda följande strängformat. Se enskilda egenskaper under enskilda kontroller för att ta reda på hur du binda dem till en datakälla.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**EXEMPEL:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Gemensamma egenskaper
-----------------

Alla RCDC-kontroller som anges i det här dokumentet kan ha följande gemensamma egenskaper. Du kan använda dessa egenskaper tillsammans med andra egenskaper som är specifika för olika kontroller.

1.  Obligatoriskt: Den här egenskapen anger att fältet är ett obligatoriskt fält eller ett valfritt fält. Ett obligatoriskt fält måste fyllas i med ett värde. Ett tomt värde stöds inte när det gäller sträng indata. Ett valfritt fält kan lämnas tomt. Om det här fältet är ett obligatoriskt fält utan värde som fylls i, visas ett felmeddelande på inkommande kontrollen. Du kan uttryckligen ange om ett fält är obligatoriska eller valfria. Du kan också binda fältet med schemainformation för en given bindning mellan ett attribut och resurstypen. Som standard om den här egenskapen saknas, betyder det att kontrollen är en valfri inmatad kontroll.

   Följande är ett exempel som använder ett explicit värde för den här egenskapen:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Detta är ett exempel som använder en dynamisk datakälla för den här egenskapen. Om du använder mallen för en datakälla som visas i föregående avsnitt i det här dokumentet är datakällan schema. Använd \<attributnamn\>. Krävs som sökväg.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**: genom att den här egenskapen till true slutanvändaren inträffar kontrollen i skrivskyddat läge. Detta är ett valfritt, Boolean-type-attribut.
    Standardvärdet anges till false. Men skrivs ibland beteendet för den här egenskapen över av typ av rättigheter som en person har på data-bind med kontrollen. Om en användare inte har behörighet att uppdatera ett fält och fältet är bundet med infogad behörighet, ser användaren data i en skrivskyddad även den här egenskapen är inställd på false.

3.  **Reguljärt uttryck**: den här egenskapen anger begränsningar som införs på värdet i kontrollen. Format för det här egenskapsvärdet är de format som stöds i den .NET StringRegex som standard. Mer information finns i [reguljära uttryck för .NET Framework](http://go.microsoft.com/fwlink/?LinkId=165361). Om kontrollen används för att ange ett värde, värdet kontrolleras mot den begränsning som anges i den här egenskapen när användaren atempts att lämna den aktuella sidan.
    Ett felmeddelande visas på kontrollen som är ogiltiga indata. Användaren kan uttryckligen ange ett reguljärt uttryck. Användaren kan också binda den med schemainformation för ett angivet attribut. Som standard om den här egenskapen saknas, innebär det att kontrollen inte kontrollerar om eventuella begränsningar för indatasträng.
    Följande är ett exempel som använder ett explicit värde för den här egenskapen:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Detta är ett exempel som använder en dynamisk datakälla för den här egenskapen. Om du använder mallen för en datakälla som visas tidigare i det här dokumentet är datakällan schema. Använd den <attribute name>. StringRegex som sökväg.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Synliga: Detta är ett valfritt, boolesk typ-attribut. Du kan använda det här attributet för att dölja hela kontrollen. Standardvärdet är inställd på true.

### <a name="options"></a>Options

Alternativ-elementet innehåller ett eller flera alternativet underordnade noder. Elementet alternativ används bara med UocRadioButtonList och UocDropDownList kontroller. Mer information om hur du använder finns dem, i avsnittet enstaka kontroller. Följande är XSD-schemat för elementet alternativ:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Attribut:

1.  Värde: Detta är ett obligatoriskt attribut av typen string. Attributet value måste vara unika inom samma kontroll. Endast A till Z, kan du använda skiftlägeskänsliga tecken.

2.  Beskrivning: Det krävs för attributet är visningsnamnet för varje alternativ.

3.  Tips: Detta är ett valfritt attribut. Använd det här attributet för att ge mer information och tips för slutanvändaren.

### <a name="environment-variables"></a>Miljövariabler

Miljövariabler i följande tabell kan användas i RCDC konfiguration.

| variabeln | Beskrivning |
|--------|--------|
| `<LoginID>`       | Visar ID för den användare som för närvarande är inloggad.           |
| `<LoginDomain>`   | Visar domänen för den användare som för närvarande är inloggad.       |
| `<Today>  `       | Visar aktuellt datum och tid                                |
| `<FromToday_nnn>` | Visar aktuellt datum plus nnn och tid. nnn är ett heltal.  |
| `<ObjectID> `     | RCDC primära resurs-ID.                                     |
| `<Attribute_xxx>` | Returnerar ett specificerat attribut xxx av RCDC primära resursen. |

### <a name="debugging-xml-configuration-files"></a>Konfigurationsfiler för XML-felsökning


När du utvecklar eller ändra XML-konfigurationsfiler för en RCDC kan du minska risken för fel vid verifiering av XML-filen mot XSD-filer med hjälp av en textredigerare, till exempel Microsoft Visual Studio®. Mer information finns i [en introduktion till XML-verktyg i Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512).

### <a name="customizing-a-help-file"></a>Anpassa en hjälpfil

Om du skapar nya resurser och attribut, kanske du vill uppdatera befintliga hjälpfilerna i FIM-portalen med innehåll för dina anpassade resurser. Hjälpfilerna i FIM-portalen är i HTML-format och de kan redigeras manuellt.

>[!IMPORTANT]
Mer information om hur du skapar anpassade attribut finns i introduktion till anpassade resursen och attributhantering i FIM 2010-dokumentationen.

>[!IMPORTANT]
Det här avsnittet innehåller inte information om grunderna för att formatera eller redigera HTML. Om du vill ändra hjälpfiler är du redan bekant med redigering HTML


**Platsen för hjälpfilerna**: alla filer som hjälp för Microsoft Identity Management 2016 SP1-portalen finns i följande mapp på servern för MIM-tjänsten:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Hur du hittar rätt hjälpfilen**: alla hjälpfiler för FIM-portalen namnges med en globalt unik identifierare (GUID). Att hitta rätt fil för din anpassade resurs:

1.  Öppna hjälpfilen på portalen som du vill anpassa i FIM-portalen.

2.  Högerklicka på hjälpfilen och klickar sedan på **egenskaper**.

3.  Markera och kopiera den `<GUID\>.htm` filen i fältet URL-adress.

4.  Navigera till mappen där hjälpfilerna lagras, och Sök efter filen.

**Lägga till innehåll för ett nytt attribut**: att lägga till beskrivande innehåll för ett nytt attribut i ett befintligt gruppering element (fliken):

1.  Identifiera och hitta lämpliga hjälpfilen.

2.  Använd en HTML-redigerare, öppna filen.

3.  Leta upp där du vill lägga till innehåll. Detta är vanligtvis en ytterligare punkt, till exempel:

`<p xmlns="">A new paragraph with customized information.</p>`

Det kan också vara ett objekt som infogas i en befintlig lista, till exempel:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Lägga till innehåll för ett nytt element gruppering**: majoriteten av FIM-Portal-sidorna som har flera gruppering element (eller flikar) och tillhörande hjälp filer har bokmärken avsnitten som relaterar till varje element i grupp. Bokmärken i HTML anges i avsnitt. Detta är till exempel HTML för fliken arbete Info från hjälpfilen för sidan Skapa användare i FIM-portalen:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Det refereras av elementet gruppering **WorkInfo** i XML-konfigurationsfilen Data-filen för den **konfiguration för skapande av användaren** RCDC. Observera att den `\<GUID\>.htm` filnamn och bokmärket har angetts i den min: länk parameter:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Enkelt exempel**

Följande bild visar ett exempel på olika enkel Textruta kontroller i olika lägen.

Exempel:

![](media/rcd-configuration-xml-reference/image016.gif)

Följande kodsegmentet skapar den första textruta kontrollen som använder explicit text för alla egenskaper och attribut:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


Följande kodsegmentet skapar den andra textruta kontrollen som använder dynamisk bindning tekniken för att länka kontrollen med en annan typ av datakälla:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


Följande kodsegmentet skapar den tredje expanderade etiketten och textrutan kontrollen:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

Följande kodsegmentet skapas fjärde inaktiverad textrutan kontrollen.
Även om den här kontrollen inte visar en visas skillnaden mellan inaktiverat tillstånd och det aktiverade läget, kan användaren inte längre ange data i textrutan.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Enskilda kontroller

### <a name="uocbutton"></a>UocButton

**Namn på**: UocButton

**Beskrivning**: Detta är en enkel kontroll som du kan använda för att aktivera vissa åtgärder. Men eftersom du inte kan ange egna hanterare är användning av den här kontrollen begränsad.

**Egenskaper för**:

1.  **Alla gemensamma egenskaper**: information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  **Text**: den här egenskapen anger den text som visas på knappen. Det här är ett valfritt, attribut av typen string. Texten som tar ett explicit värde.

Händelse:

   • **OnButtonClicked**: händelsen genereras när användaren klickar på knappen.

Exempel:

![](media/rcd-configuration-xml-reference/image017.png)


Följande XML-segmentet ger enkel knappen:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Namn på**: UocCaptionControl

**Beskrivning**: den här kontrollen används för att visa beskrivningen av en RCDC-sida. Den här kontrollen är utformad för att endast användas som en enskild kontroll i en rubrik gruppering.
Den används i andra sammanhang kan orsaka problem vid rendering eller portal fel.

**Läget**: Läs endast (OneWay)

**Egenskaper:**

1.  **Alla gemensamma egenskaper:** mer information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  **MaxHeight:** den här egenskapen anger maximal höjd på ikonen i beskrivningsavsnittet. Den här egenskapen är valfri. Den här egenskapen tar ett heltalsvärde i bildpunkter. Standardvärdet är 32 bildpunkter.

Exempel:

![](media/rcd-configuration-xml-reference/image020.jpg)

Följande kodsegmentet genererar den **huvud beskrivning**:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


Den här kodsegmentet genererar den **Explicit innehåll beskrivning:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

Följande kodsegmentet genererar den **visningsnamn** dynamiska beskrivning:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Namn på**: UocCheckBox

Beskrivning: Det här är en enkel kryssruta. Vi rekommenderar att användaren binda den här kontrollen med typen Boolean-data. Den här kontrollen kan användas som en skrivskyddad kontrollen eller en uppdateringsbar, baserat på de data som den Binder till.

>[!NOTE]
I den här versionen när med hjälp av kryssrutan Kontrollera i redigeringsläge för att visa ett booleskt attribut, om attributet saknar ett värde som tidigare tilldelats Resource Control lägger till ett värde av **FALSKT** till attributet när **OK** klickas i redigeringsläge. Runt är att alltid skapa ett booleskt attribut som förutsätter att icke-är samma som **FALSKT**, eller Använd andra kontroller, till exempel en alternativknapp för booleska attribut.

**Egenskaper för**:

1.  **Alla gemensamma egenskaper**: Mer information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  **DefaultValue**: Detta är en valfri, Boolean-type-egenskap. Standardvärdet anges till false. Det här fältet anger standardinställningen för en kryssruta.
    Det kan anges explicit.

3.  **Alternativet är markerat**: Detta är en valfri, Boolean-type-egenskap. Standardvärdet anges till false. Det här värdet skriver över egenskapen standardvärde när det finns en sådan tillsammans med standardvärde. Det här fältet anger beteendet för en kryssruta. Precis som standardvärde, kan detta anges explicit eller bundna till data från servern.

4.  **Text**: Detta är ett valfritt, attribut av typen string. Texten som visas till höger om kryssrutan. Du kan använda den här egenskapen anger text som innehåller mer information till slutanvändaren.

**Händelser**:

   • CheckedChanged: den här händelsen släpps när dess status ändras i den här kryssrutan.

I följande exempel skapas en anpassad bindning mellan den anpassade resursen och attributet **IsConfigurationType**. XML-filen används i RCDC för en anpassad resurstyp.

Exempel:

![](media/rcd-configuration-xml-reference/image022.png)

I följande kod segment ger en **dynamiska kryssrutan**, som visas som dynamiska kryssrutan i föregående bild. Den här typen av bindning är vanligtvis mer flexibel och användbar än en explicit kryssruta. Attributet måste tillhöra aktuellt resurstypen.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Beskrivning**: Detta är en flerradig textruta-kontroll som har stöd för särskilda strängformatering. Varje värde mellan flera värden posterna är avgränsade från varandra med semikolon. eller en radbrytning i textrutan. Vi rekommenderar att den här kontrollen vara bundna till data för flera värden, korta sträng och heltal-typer. Den här kontrollen stöder både skrivskyddad och uppdateras läge.

**Egenskaper för**:

1.  **Alla gemensamma egenskaper**: Mer information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  **DataType**: Detta är ett obligatoriskt, typen string-attribut. Du kan ange detta som en **sträng, heltal**, eller **DateTime** skriver explicit. Du kan också binda attribut med attributet schemat **DataType** egenskapen. Ett flervärdesattribut referenstyp ska hanteras av **UOCListView** eller **UOCIdentityPicker**. Booleska flervärdesattribut är inte en datatyp som stöds.

3.  **Rader**: Detta är ett valfritt, typen integer-attribut. Du kan definiera höjden på rutan i antal tecken. Standardvärdet anges till 1.

4.  **Kolumner**: Detta är ett valfritt, typen integer-attribut. Du kan ange hur många bred rutan finns i antal tecken. Standardvärdet anges till
    20.

5.  **Värdet**: Detta är ett valfritt, attribut av typen string. Du kan binda det här attributet endast med datakällan.

Händelser:

   • **ValueListChanged**: den här händelsen utlöses när det aktuella värdet i kontrollen ändras.

I följande exempel visas ett flervärdesattribut strängattribut med namnet **AMultiValueString** skapas och bunden till anpassade resurstypen. Det här exemplet fungerar endast när den här bindningen har skapats.

**Exempel:**

![](media/rcd-configuration-xml-reference/image024.jpg)

Följande kodsegmentet genererar bilden ovan:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Namn på**: UocDateTimeControl

**Beskrivning**: Detta liknar en textruta kontroll, men **beskrivning** accepterar bara ett visst format. I skrivskyddat läge visas den som en etikett. Formatet på den inmatade strängen som stöds finns i **DateTimeFormat** egenskap i det här avsnittet.

**Egenskaper för**:

1.  **Alla gemensamma egenskaper**: Mer information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  **DateTimeFormat**: Detta är ett valfritt, attribut av typen string. Format som stöds är DateTime eller DateOnly. Standardvärdet anges till DateTime-format.
    a. DateTime-format: det här attributet är formaterad som mm/dd/åååå hh: mm: ss AM.

      <[!NOTE]
      Båda **DateTime** och **DateOnly** format som stöds, oavsett användaren som anger skillnaden.
3.  **Värdet**: Detta är ett valfritt, attribut av typen string. Du binda det här attributet med en datakälla för resursen. Värdet för det här attributet måste överensstämma med rätt datetime-format.

Händelser:

   • **DateTimeChanged**: när ändringarna för datetime-värde, händelsen inträffar.

Exempel:

![](media/rcd-configuration-xml-reference/image027.jpg)

Följande kodsegmentet ger först **DateTime** kontroll.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


Följande kodsegmentet ger andra **DateTime** kontroll. Om du har använt exempelkoden i avsnittet datakällor i **ExpirationTime** attributet är bundet till alla typer av resurser. Därför kan du använda den med följande kod:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Namn på**: UocDropDownList

Beskrivning: Det här är en enkel nedrullningsbar-kontroll. Den här kontrollen används vanligtvis när du vill välja alternativ från en definierad uppsättning alternativ. Datatyperna för sträng, integer, datetime och booleskt värde är bra kandidater för den här kontrollen.

**Egenskaper för**:

1.  **Alla gemensamma egenskaper**: Mer information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  **ValuePath**: egenskapen att hämta attributet Value från ItemSource. När ItemSource anges som eget anges värdet sökvägen till värde. Det har bindningar med fältet värde från det alternativelement definieras senare i det här dokumentet.

3.  **CaptionPath**: egenskapen att hämta attributet Value från ItemSource. När ItemSource anges som eget anges värdet sökvägen till rubriken. Det har bindningar med fältet rubrik från alternativet-element som har definierats senare i det här dokumentet.

4.  **HintPath**: egenskapen att hämta attributet Value från ItemSource. När ItemSource anges som eget anges värdet sökvägen till tipset. Det har bindningar med fältet tipset från alternativet-element som har definierats senare i det här dokumentet.

5.  **ItemSource**: en samling ListControlItems som definierar val i listan. Användaren kan uttryckligen ange anpassat och Använd alternativet-elementet för att ange strängvärdet.

6.  **SelectedValue**: det värde som är markerad. Det här är en obligatorisk, egenskap av typen string. Den här egenskapen är bunden med strängdata från datakällan.

Händelser:

  • SelectedIndexChanged: händelsen inträffar när markeringen i den nedrullningsbara listrutan ändras.

Alternativ:

Struktur för ett alternativelement finns i avsnittet ”alternativet” i det här dokumentet.

1.  **Värdet**: det värdet för ett enstaka alternativelement kan anges till en sträng som är giltiga indata för den datakälla som kontrollen binds till.

2.  **Bildtexten**: beskrivning kan vara ett värde.

3.  **Tipset**: tips kan vara ett värde.

Exempel:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Om du vill att provet fungerar, måste du binda ett befintligt, attribut av typen string **omfång** med den anpassa resurstyp som RCDC gäller för.


Den här kodsegmentet genererar den nedrullningsbara listan:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Namn: UocFileDownload

Beskrivning: Den här kontrollen innehåller en hyperlänk. När du klickar på hyperlänken visas en sida för Windows-spara filen. Användaren kan spara filen på sin lokala enhet.
Öppna alternativet stöds även om filformatet kan återges i Internet Explorer. Rekommenderade datatyperna som ska använda den här kontrollen med är formaterad sträng (XML) och binära typer.

>[!NOTE]
I den här versionen av Microsoft Identity Manager 2016 SP1 Stäng användaren Internet Explorer-fönstret som han eller hon har öppnat filen och sedan uppdatera sidan. Användaren kan sedan starta nedladdningen om du vill spara eller öppna samma fil igen i det ursprungliga fönstret när du har uppdaterat Internet Explorer-fönstret.


Egenskaper:

1.  **Alla gemensamma egenskaper**: Mer information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  **Text**: Detta är ett valfritt, typen string-attribut som definierar text för hyperlänken. Användaren kan ange en explicit sträng för den här egenskapen.

3.  **Värdet**: Detta är ett obligatoriskt attribut. Bindningen för attributet anger på den server vars innehåll ska hämtas.

4.  **PromptedFileName**: Detta är ett valfritt, attribut av typen string. Detta är det filnamn som föreslås för användaren när de spara den hämta filen.

5.  **ContentType**: Detta är ett obligatoriskt, typen string-attribut. Det här är den filtyp som data sparas i. Text eller binär är de två alternativen för strängen. Om den är text, anses det returnera värdet som en lång sträng.
    För binära, annars betraktas returvärdet som byte []. Om texten är markerad användaren kan som ett alternativ, lägga till ett suffix för att ange vilken typ av format texten finns i. Till exempel är text/xml giltig.


>[!NOTE]
Om det värde som är bundet till den här kontrollen är tom används saknar kontrollen hyperlänken ska användas för att utlösa download åtgärd. Det beror på att det finns inget att hämta.


**Händelser**:

Det finns inga händelser för den här kontrollen.

**Exempel**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Innan du laddar upp exempelfilen skapa användaren en bindning mellan en egen resurstyp och det befintliga attributet ConfigurationData.


Följande kodsegmentet genererar download filkontroll i föregående bild:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Namn på**: UocFileUpload

**Beskrivning**: den här kontrollen innehåller en textruta som visar platsen för den lokala filen ska överföras, knappen Bläddra fil och en knappen Skicka. När användaren klickar på bläddringsknappen, visas en öppen fil för Windows-fönstret. Slutanvändaren kan välja en fil på sin lokala enhet att överföra. När filen har valts visas filens plats i textrutan. När du klickar på knappen Skicka överföra filen till klientens lokala datakällan. Filinnehållet ännu inte har skickats till servern. Rekommenderade datatyperna som ska använda den här kontrollen med är följande: formaterad sträng (XML) eller binära typer.

>[!NOTE]
Det finns inga tecken överföringen pågår eller status. När filen har överförts till den lokala datakällan, är rutan avmarkerad.


Egenskaper:

1.  Alla gemensamma egenskaper: Information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  Värde: Detta är ett obligatoriskt attribut. Det anger attributet schemabindning på servern där data överförs.

3.  ContentType: Detta är ett valfritt, attribut av typen string. Det här är den datatyp som är spara filen på servern. Detta kan ställas in till Text eller Binary. När egenskapen saknas, är standardvärdet Binary.

4.  MaxFileSize: Detta är ett valfritt, attribut av typen string. MaxFileSize definierar hur stor överförda filen kan vara. Som standard är den maximala storleken 1 megabyte (MB) om egenskapen saknas.

5.  PromptedForNoValue: Detta är ett valfritt, attribut av typen string. Den definierar den text som visas för användaren när en fil inte överförs.

Händelser:

   • FileUploaded: den här händelsen har genererats när filen har överförts.

Exempel:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
För att göra följande exempelkod fungerar kan du skapa ett nytt binary-type-attribut med namnet ABinaryAttribute och sedan skapa en ny bindning mellan en egen resurstyp och det här attributet.


Följande kodsegmentet genererar kontrollen i föregående bild:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Namn: UocFilterBuilder

Beskrivning: Detta är en komplex kontroll som används att rendera en MIM 2016 XPath-uttryck. Vissa XPath-uttryck stöds inte. Information om hur du använder filterverktyget finns i hjälpen för filterverktyget.

Egenskaper:

1.  Alla gemensamma egenskaper: Mer information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  PermittedObjectTypes: Detta definierar en lista över resurstyper som ska visas i select-instruktionen i en filterverktyget. Information om hur du använder filterverktyget finns filterverktyget hjälp. Strängen är i formatet ResourceTypeA ResourceTypeB, där varje resurstypen avgränsas med kommatecken (,).

3.  Värde: Detta är det värde som filterverktyget återges.
    Endast en bindning med data av typen string som innehåller ett XPath-uttryck stöds. Filterattributet är en rekommenderad attribut för att binda den här kontrollen.

4.  PreviewButtonVisible: Detta är en valfri, Boolean-type-egenskap. När den här egenskapen är inställd på false, en Preview-knappen i användaren inte visas. Standardvärdet är inställd på true. Den här knappen kan användas i kombination med en listvy kontroll för förhandsgranskning av resultatet av ett XPath-uttryck.

5.  ExcludeGroupMembership: Detta är en boolesk egenskap. När den här egenskapen har angetts true, du kan inte skapa ett filter som använder \<referensattribut\> (till exempel ResourceID) är medlem av \<gruppobjekt\>. Med andra ord, när den här egenskapen har angetts true, du kan inte skapa ett filter som använder katalogen medlemskap i gruppen.

6.  PreviewButtonCaption: Detta är en valfri sträng. När PreviewButtonVisible har angetts till true, du kan använda den här egenskapen för att ger du en anpassad text. Texten som visas på knappen förhandsgranskning.

Händelser:

   • OnFilterChanged: den här händelsen utlöses när filter builder innehållet ändras.

Exempel:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Innan du använder den här exempelkoden måste du skapa en ny bindning mellan ett befintligt filterattribut och en egen resurstyp.


Följande är exempelkod som innehåller en UOCLabel-kontroll, en enkel filterverktyget med PermittedObjectTypes och en listvy för förhandsgranskning. Du måste peka på samma attribut för datakälla att koppla de två listvyn ListFilter egenskapen och filterverktyget Value-egenskap.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Namn: UocHtmlSummary

Beskrivning: Du kan använda den här kontrollen för att definiera en sammanfattningssida i en RCDC-sida.
Översikt över sidan visas när användaren skickar en begäran. Den här kontrollen kan endast användas i en sammanfattning av gruppering och det måste vara den enda kontrollen. Vi rekommenderar starkt att du använder exempelkoden som har angetts.

>[!NOTE]
Den här kontrollen har inte testats stor utsträckning.


Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  ModificationsXml: Den här egenskapen måste formateras som {bindning källa = delta, sökväg = DeltaXml}, där delta har definierats i rubriken configuration ObjectDataSource.

3.  TransformXsl: Den här egenskapen är vanligtvis formaterad som {bindning källa = summaryTransformXsl, sökväg = /}, där summaryTransformXsl har definierats i rubriken configuration XmlDataSource.

En befintlig exempel på den här kontrollen finns i ”sammanfattning gruppering” i avsnittet gruppering tidigare i det här dokumentet.

### <a name="uochyperlink"></a>UocHyperLink

Namn: UocHyperLink

Beskrivning: Det här är en enkel hyperlänkkontroll. Du kan använda den här kontrollen för att visa information som en hyperlänk.

Egenskaper:

1.  Alla gemensamma egenskaper: Information finns i avsnittet med vanliga egenskaper i det här dokumentet.

2.  ObjectReference: Detta är en valfri, referenstyp egenskap. Om det GUID som definieras i den här egenskapen refererar till en giltig resurs, kan hyperlänken användaren åtkomst till resursen. Detta är ömsesidigt uteslutande med NavigateUrl-egenskap (nedan).

3.  Text: Det här är en valfri, egenskap av typen string. Den här egenskapen används för att definiera den text som visas som hyperlänk.

4.  NavigateUrl: Det här är en valfri, egenskap av typen string. Den här egenskapen används för att ange den fullständiga sökväg URL som hyperlänken länkar till. Detta är ömsesidigt uteslutande med egenskapen ObjectReference (ovan).

Exempel:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
Du behöver ett giltigt GUID för en resurs för att länka den till. I det här fallet genereras andra hyperlänken med ett giltigt GUID. Den första som kan vara en webbplats.


Följande kodsegmentet genererar en omdirigerar hyperlänk:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

Följande kodsegmentet genererar en hyperlänk som refererar till en resurs. Explicit referens kan ersättas av uttrycket {bindning källa = objekt, sökväg = Creator} att binda det till en datakälla. Detta kan vara giltigt endast när den resource manager finns och är av referenstyp värdet.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Namn: UocIdentityPicker

Beskrivning: Den här kontrollen består av en valfri Resolve-ruta och ett fönster. Rutan valfritt Lös består av ett valfritt textruta för att ange identitet och en Resolve-knapp för att matcha identiteten Bläddra-knappen för att begära ett popup-fönster. Fönstret Bläddra gör det möjligt för användaren att välja identiteter via en listvy kontroll. Den markerade identiteten från fönstret Bläddra visas i rutan Lös.

Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  UsageKeywords: Detta är en valfri sträng-egenskap. Du kan definiera en lista med sökomfång som ska användas i Väljaren resursen genom att tillhandahålla en lista över nyckelord för användning som stöds av SearchScopeConfiguration struktur, där varje nyckelord avgränsas med (').

3.  Filter: Det här är en valfri sträng-egenskap. Användaren anger ett XPath-uttryck omfång Väljaren resurs om du vill visa de objekt som får plats inom en definierad omfång. Den här egenskapen är ömsesidigt uteslutande med egenskapen UsageKeywords (ovan). När sökomfånget används har den här egenskapen ingen effekt.

4.  ResultObjectType: Detta är en valfri sträng-egenskap. Resurstypen används för att återge resurser i listan över popup-dialogrutan. Detta används med filtret för att väljaren identitet identifiera vilken resurstyp som returneras av filtret och därefter återge data. Den här egenskapen är ömsesidigt uteslutande med egenskapen UsageKeywords (se ovan). När sökomfånget används har inställningen ingen effekt. Den sträng som accepteras för den här egenskapen är en enskild, giltig resurstyp namn, till exempel Person.
    När filtret förväntas returnera flera resurstyper, används resurs.

5.  PreviewTitle: Detta är preview titeln används på en listvy. Information om den här egenskapen finns i avsnittet UocListView.

6.  ListViewTitle: Detta är en valfri sträng-egenskap. Du kan använda den här egenskapen för att definiera den text som visas ovanpå listvyn som en rubrik.

7.  Värde: Det här är en valfri sträng-egenskap. Vi rekommenderar att du binda detta med ett schemaattribut ansluta värdet till en datakälla.

8.  Läge: Det här är en valfri sträng-egenskap. Den här egenskapen används för att ange om identitet Väljaren kan välja ett värde eller flera identiteter kan väljas. SingleResult och MultipleResult är de tillåtna värdena. Som standard anges till SingleResult.

9.  ObjectTypes: Detta är en valfri, egenskap av typen string. Du kan definiera en lista över resurstyper som användaren kan lösa transaktioner mot i rutan identitet Väljaren lösa. Listan består av en lista över resurstypen namn, avgränsade med kommatecken (,).

10. AttributesToSearch: Detta är en valfri sträng typeproperty. Du kan definiera en lista över attribut som används för att matcha objektet i Väljaren identitet om listan är en lista över schemaattribut, avgränsade med kommatecken (,). Till exempel om AttributesToSearch anges till DisplayName, Alias, innebär det att användaren kan söka i objekt med DisplayName = \<söka värdet\> eller Alias =\<söka värdet\>. Vi rekommenderar att attributnamn som anges här är giltiga attribut i mål resurstyper i datakällan som är belägna i värde. Resurstyper målet kan hittas i fältet ObjectTypes. Alla attribut måste vara giltig om alla angivna resurstyper som anges i fältet ObjectTypes.

11. ColumnsToDisplay: Detta är en valfri, egenskap av typen string. Användaren anger en lista över schemat attributnamn, avgränsade med kommatecken (,). De attribut som definieras här utgör kolumnen i listvyn i Väljaren identitet.

12. Rader: Det här är en valfri, heltalsegenskap. Det fungerar endast när läget är inställt på MultipleResult. Använd den här egenskapen om du vill ange höjden på textrutan Lös till en given storlek i tecken.

13. MainSearchScreenText: Detta är en valfri, egenskap av typen string. Detta är den anpassade texten som visas när sökningen körs i fönstret Bläddra.

Händelser:

 • SelectedObjectChanged: den här händelsen genereras när användaren ändrar de valda resurserna.

Exempel:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Du måste skapa en ny bindning mellan Manager-attribut och eventuella anpassade resurstyp som den här XML gäller för det här exemplet ska fungera.


Följande kodsegmentet genererar en Identity-väljare i ett enda läge med hjälp av egenskaperna Filter och ResultObjectType som en del av RCDC:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Exempel:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Om du vill göra den här exempelkoden fungera, måste du binda attributet ExplicitMember (ett flervärdesattribut referensattribut) till den anpassa resurstypen. Du måste också skapa sökomfattningar med egenskapen UsageKeyword Person och gruppen.


Följande kodsegmentet skapar kontrollen i bilden ovan:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Namn: UocLabel

Beskrivning: Det här är en enkel, skrivskyddad, text etikett. Vi rekommenderar att den här kontrollen för att visa skrivskyddade data.

Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  Text: Detta är ett attribut av typen string. Du kan definiera den här egenskapen genom att tillhandahålla ett explicit värde eller genom att binda till en datakälla. En exempel-bindning som tilldelar walue för den här egenskapen är {bindningen källa = objekt, sökväg =\<ogiltigt attributnamn\>.

Ett exempel på UocLabel kontrollen finns i enkla kontrollen i avsnittet enkla exempel.

### <a name="uoclistview"></a>UocListView

Namn: UocListView

Beskrivning: Det här är en avancerad listvyn kontroll. Det består av en enkel listvy, ett valfritt enkel sökning, en valfri avancerad sökning-kontroll, ett valfritt alternativrutan för förhandsgranskning och en knapp Åtgärdsfältet. Valfria enkel sökning består av ett sökomfång och en textruta för enkel sökning. Avancerad sökning-kontrollen är en filterverktyget. I listan visas en prerendered lista över resurser. Det kan också visa sökresultat som kommer från sökkontrollerna i den här kontrollen. Fältet åtgärdsknappen definierar vad som kan göras baserat på valet i listvyn. Rutan Förhandsgranska markering visar vilka objekt som väljs från listan.

>[!IMPORTANT]
UocListView fungerar inte med enstaka referensattribut. Den kan användas med flervärdesattribut referensattribut. Enstaka referensattribut finns UocIdentityPicker i det här dokumentet.


Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  SelectedValue: Det här är en valfri, typen string-egenskap som vanligtvis är bunden till ett flervärdesattribut referensattribut som accepterar en lista med strängar som GUID-format.

3.  PageSize: Detta är ett valfritt heltalsegenskap. Användaren kan ange hur många poster som får plats i en sida i en Listvykontroll. Standardvärdet är 10 poster. Ett positivt heltal är giltig.

4.  UsageKeyword: Detta är en valfri, egenskap av typen string. Användaren kan ange en lista över nyckelord som definierar vilka sökomfånget används i listvyn sökkontrollen. Sök efter scope resurser finns i FIM 2010-server. Attributet på en SearchScopeConfiguration struktur som kallas UsageKeyword, används för att gruppera en uppsättning sökomfattningar. Listvyn förbrukar listan över nyckelord. Varje nyckelord avgränsade med kommatecken (,).
    Det här usagekeyword används på motsvarande sökomfånget som du vill visa i den här listvyn. Detta är endast aktiverad när ShowSearchControl-egenskapen har angetts till true.

5.  SearchControlAutoPostback: Detta är en valfri boolesk egenskap. Ange värdet för den här egenskapen till true för att utföra autopostback när en sökning initieras. Som standard är SearchControlAutoPostback inställd på false.

6.  EmptyResultText: Detta är en valfri, egenskap av typen string. Som standard anges till några objekt, men den kan anges till ett värde. Den här texten visas när ett sökresultat är tom.

7.  ButtonHeight: Detta är en valfri, egenskap av typen integer. Ange värdet för den här egenskapen till valfritt positivt heltal. Den här egenskapen anger höjden på knapparna i Åtgärdsfältet i bildpunkter. Standardvärdet är 32 bildpunkter.

8.  ButtonWidth: Det här är en valfri, egenskap av typen integer. Ange värdet för den här egenskapen till valfritt positivt heltal. Den här egenskapen anger bredden på knapparna i Åtgärdsfältet i bildpunkter. Standardvärdet är 32 bildpunkter.

9.  CaptionImageMaxHeight: Detta är en valfri, egenskap av typen integer. Ange värdet för den här egenskapen till ett positivt heltal. Den här egenskapen anger en alternativ rubrik ikon för maximal höjd. Standardvärdet är 32 bildpunkter.

10. CaptionImageMaxWidth: Detta är en valfri, egenskap av typen integer. Ange värdet för den här egenskapen till ett positivt heltal. Den här egenskapen anger en alternativ rubrik ikon för maximal bredd. Standardvärdet är 32 bildpunkter.

11. CaptionImageUrl: Detta är en valfri, egenskap av typen string. Den här egenskapen definierar en URL som länkar till en bild som visas som rubrik avbildningen.

12. PreviewTitle: Detta är en valfri, egenskap av typen string. Den här egenskapen används för att definiera den text som visas ovanpå förhandsgranskningsrutan.

13. EnableSelection: Det här är en valfri, Boolean-type-egenskap. Du kan använda den här egenskapen för att bestämma om en listvy är i läget. Om det finns en listvy i markeringsläget, en kolumn av kryssrutorna som visas i kolumnen längst till vänster i listvyn och en markering preview visas längst ned i listan. Standardvärdet för den här egenskapen anges till true.

14. SingleSelection: Detta är en valfri, Boolean-type-egenskap. Om markeringsläget är aktiverat för listvyn, begränsar om det här värdet anges till true användaren att välja endast ett objekt i listan. Som standard anges värdet för den här egenskapen till false. Det innebär att som standard slutanvändaren kan välja flera objekt i listan.

15. RedirectUrl anges: Det här är en valfri, egenskap av typen string. Använd den här egenskapen för att ange en sida som ska användas när användaren klickar på ett länkade objekt i listan. URL: en kan innehålla platshållare som ersätts med det faktiska värdet under körningen. Platshållarna är följande:

     ◦ {0} - objectType

     ◦ {1} - objekt-ID

     ◦ {2} - displayName

16.  ShowTitleBar: Detta är en valfri, Boolean-type-egenskap. Använd den här egenskapen för att ange om namnlisten ska visas. Standardvärdet för den här egenskapen är false.

17.  ShowActionBar: Detta är en valfri, Boolean-type-egenskap. Använd den här egenskapen för att ange om det åtgärden området ska visas. Standardvärdet för den här egenskapen är true.

18.  ShowPreview: Detta är en valfri, Boolean-type-egenskap. Använd den här egenskapen för att ange om förhandsgranskningen ska visas. Standardvärdet för den här egenskapen är true.

19.  ShowSearchControl: Detta är en valfri, Boolean-type-egenskap. Använd den här egenskapen för att ange om sökkontrollen ska visas. Standardvärdet för den här egenskapen är true.

20.  ResultObjectType: Detta är en valfri, egenskap av typen string. Använd den här egenskapen för att ange förväntade objekttypen i sökresultaten. Standardvärdet för den här egenskapen är resurs. Om sökresultatet innehåller flera resurstyper, bör det här värdet anges som resurs.

21.  ColumnsToDisplay: Detta är en valfri egenskap. Använd den här egenskapen om du vill ange vilka attribut listvyn ska visas som kolumner. Standardvärdet för den här egenskapen är DisplayName, ResourceType. Varje kolumn representeras av systemnamnet på ett attribut. Varje kolumn är avgränsade med kommatecken (,). Du behöver inte ange ett värde för den här egenskapen när listvyn används i markeringsläget. Markeringsläget kommer inställningen kolumnen från attributet SearchScopeColumn för sökomfånget som är markerade.

22.  ListFilter: Det här är en valfri, egenskap av typen string. Det här är den xpath som används för att återge listvyn och är endast aktiverad när ShowSearchControl-egenskap är inställd på false. När det här värdet anges listvyn använder det här egenskapsvärdet för frågor och listvyn är inte i läget. Filtret kan antingen vara kopplat till ett strängattribut för resursen, enligt följande:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     eller vara en sträng som innehåller vissa fördefinierade miljövariabeln enligt följande: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: Detta är en föråldrad egenskap. Värdet ska vara systemnamnet för ett refererade flervärdesattribut. Vi rekommenderar att den här egenskapen inte användas längre. Till exempel i grupphantering kommer i stället för följande:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Det bör vara:`<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: Detta är en valfri, egenskap av typen string. Använd den här egenskapen för att ange om du vill att klicka på en Listvyobjekt som utlöser ett återanslående server eller för att visa en detaljerad vy av artikeln. Två värden för alternativ som stöds: ModelessDialog och Server. Standardvärdet är ModelessDialog.

25.  SearchOnLoad: Detta är en valfri, boolesk typ-egenskap som anger om listvyn kontrollen ska fråga inläsning. Den här egenskapen gäller endast när listvyn är i läget. Standardvärdet för den här egenskapen är true. Du kan inaktivera den om du tror att användaren vanligtvis in text-sökning för att få ett meningsfullt resultat. I det här fallet visar listvyn ett meddelande som talar om hur du utför en sökning användaren först. Texten som kan anpassas med följande egenskaper:

26.  MainSearchScreenText: Det här valfria strängtyp egenskapen gäller bara när SearchOnload har angetts till true. Den här egenskapen kan användas för att anpassa text som visas i mitten listvyn när listvyn inte Sök automatiskt. Standardvärdet för den här egenskapen är hitta resurserna du vill med hjälp av sökfunktionen ovan. Du kan ange ett värde så att texten mer relevant för ditt scenario.

27.  SubSearchScreenText: Det här valfria egenskapen av typen string används för att anpassa den text som visas under MainSearchScreenText. Vanligtvis behöver du inte ange ett värde för den här egenskapen om du vill lägga till vissa ytterligare instruktioner om hur du använder listvyn.

Exempel på hur du använder listvyn tillsammans med UocFilterBuilder kontrollen som en förhandsgranskning lista finns exempel UocFilterBuilder tidigare i det här dokumentet. UocListView kan även användas utan filterverktyget.

### <a name="uocnumericbox"></a>UocNumericBox

Namn: UocNumericBox

Beskrivning: Det här är en enkel textruta som tar endast heltalsvärden. Den här kontrollen stöder både skrivskyddad och uppdateras läge.

Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  MaxValue: Det här är en valfri, egenskap av typen integer. Använd den här egenskapen om du vill definiera en klientsidan verifiering för kontrollen. Det värde som användaren anger får inte överskrida det här värdet. Du kan ange ett explicit heltal eller binda detta med heltalsdata från en datakälla med hjälp av {bindning källa = schema sökväg = IntegerMaximum}.

3.  MinValue: Det här är en valfri, egenskap av typen integer. Använd den här egenskapen om du vill definiera en klientsidan verifiering för kontrollen. Det värde som användaren anger får inte vara lägre än detta värde. Du kan ange ett explicit heltal eller binda detta med heltalsdata från en datakälla med hjälp av bindning källa = schema, sökväg = IntegerMinimum}.

4.  Standardvärde: Det här är en valfri, egenskap av typen integer. Använd den här egenskapen för att definiera ett standardvärde för kontrollen om kontrollen används för att skapa nya data. Det här värdet kan endast anges explicit till en statisk heltal.

5.  Värde: Det här är en valfri, egenskap av typen integer. När du binder det med ett heltal typen data från en datakälla, visas värdet för attributet när sidan läses in och sparas sedan till datakällan efter.

Hanterare:

   • TextChanged: den här händelsen genereras när det aktuella värdet i kontrollen ändras.

Exempel:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
Följande exempelkod genererar den första numeriska rutan. Rutan numeriska är inte kopplad till en datakälla eller schemainformation.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

Följande exempelkod genererar den andra numeriska rutan.

>[!NOTE]
För det här exemplet fungerar, behöver du först skapa en ny, typen integer attributet kallas AnIntegerAttribute och binda med anpassade resurstypen.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Namn: UocPictureBox

Beskrivning: Den här kontrollen används för att återge bild, binär typ data. Vi rekommenderar att den här kontrollen kan användas med typen binary-data. Bilden kan återges genom en angivna bild-URL, binär typ data eller källan för attributet som innehåller bildtyp data.

Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  ImageUrl: Det här är en valfri, egenskap av typen string. Ange på mål-URL.

3.  MaxHeight: Detta är en valfri sträng-egenskapen type. Den definierar maximal höjd för avbildningen som ska renderas i bildpunkter.

4.  MaxWidth: Detta är en valfri, egenskap av typen string. Den definierar den maximala bredden för bilden som ska renderas i bildpunkter.

5.  ImageData: Detta är en binary-type-egenskap. Använd den här egenskapen för att binda en datakälla med visad bild. Bundna datakällan måste vara binära.
    Du kan också använda det här fältet för en bild genom att tillhandahålla data i format för byte [].

6.  ImageResource: Detta är en valfri, binary-type-egenskap.

7.  AlternativeText: Detta är en valfri, egenskap av typen string. Den här egenskapen visas som alternativ text när bilden inte kan visas.

Exempel:

>[!NOTE]
Om du vill använda det här exemplet måste du ha en befintlig bilddata binda med kontrollen.


Följande kodsegmentet genererar en bild-kontroll som binder en datakälla med kontrollen:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

Följande kodsegmentet genererar en bild-kontroll som binder en URL-bild med kontrollen:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Namn: UocRadioButtonList

Beskrivning: Det här är en enkel, alternativknappen lista. Alternativen är ömsesidigt uteslutande i den här listan. Den här kontrollen rekommenderas när användarna har fem eller färre alternativ att välja mellan. Annars rekommenderas UOCListView.

Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  ValuePath: Värdet sökväg har angetts till värdet. Det har bindningar med fältet värde från det alternativ-element som har definierats i det här dokumentet.

3.  CaptionPath: Värdet sökväg har angetts till rubriken. Det har bindningar med fältet rubrik från alternativet-element som har definierats i det här dokumentet.

4.  HintPath: Värdet sökväg har angetts till tipset. Det har bindningar med fältet tipset från alternativet-element som har definierats i det här dokumentet.

5.  SelectedValue: Det värde som är markerad. Det här är en obligatorisk, egenskap av typen string. Denna egenskap Binder med strängdata från datakällan.

Händelser:

1.  SelectedIndexChanged

2.  CheckedChanged

Alternativ:

Det kan bara finnas två alternativelement i alternativ för den här kontrollen.

1.  Värde: Fältet värde i ett enstaka alternativelement måste anges till True eller False.

2.  Beskrivning: Det kan vara ett värde.

3.  Tips: Detta kan vara ett värde.

Exempel:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Du måste skapa en ny booleska attributet ABooleanAttribute, och binda den med din egen resurstyp för det här exemplet ska fungera.

Följande kodsegmentet skapar alternativlistan-knapp i bilden ovan:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Namn: UocSimpleRadioButton

Beskrivning: Det här är en enkel, alternativknappen kontroll. Användning av den här kontrollen liknar en enkel kryssruta. Det finns två, på alternativknappar visas bredvid texten etiketter. Du rekommenderas att binda kontrollen till boolesk typ data.

Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i avsnittet med vanliga egenskaper i det här dokumentet

2.  Sanntext: Det här är en valfri, egenskap av typen string. Detta är den text som visas när knappen markeras.

3.  Falsktext: Det här är en valfri, egenskap av typen string. Detta är den text som visas när användaren klickar på knappen inte är markerad.

4.  SelectedItem: Detta är en valfri, Boolean-type-egenskap. Det här värdet anger att alternativknappen är markerad. Detta kan binda med boolesk typ data från en datakälla. Standardvärdet anges till false.

Händelser:

   • CheckedChanged: när alternativet knappen ändras tillstånd från valt för att omarkerade eller motsatsen signalen har genererats.

Exempel:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Om du vill att provet fungerar, måste du skapa ett nytt booleskt attribut ABooleanAttribute och binda den till din egen resurstyp. RCDC-data används samma anpassade resurstypen.

Följande kodsegmentet genererar alternativknappen i föregående bild:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Namn: UocTextBox

Beskrivning: Det här är en enkel textruta som stöder indata av typen string. Vi rekommenderar att du använder den här kontrollen för att binda data av typen string.

Egenskaper:

1.  Alla gemensamma egenskaper: Information om den här egenskapen finns i egenskapsavsnittet med vanliga i det här dokumentet.

2.  MaxLength: Detta är ett valfritt, typen integer-attribut. Den här egenskapen anger den maximala längden för en sträng som indata. Standardvärdet för den här egenskapen är 128 tecken.

3.  Text: Det här är en valfri, egenskap av typen string. Detta är den text som visas i textrutan. Du kan definiera en explicit sträng som visas i textrutan under första inläsningen av kontrollen eller binda den till ett schemaattribut av typen string.

4.  Rader: Det här är en valfri, egenskap av typen integer. Den här egenskapen anger textrutans höjd i tecken. Standardvärdet är 1 tecken.

5.  Kolumner: Det här är en valfri, egenskap av typen integer. Den här egenskapen anger textrutans bredd i tecken. Standardvärdet är 20 tecken.

6.  Omslutning: Det här är en valfri, Boolean-type-egenskap. Genom att ange värdet för den här egenskapen till true, kan användaren funktionen radbyte i textrutan. Standardvärdet för den här egenskapen anges till true.

7.  UniquenessValidationXPath: Detta är en valfri, egenskap av typen string. Det tar ett giltigt uttryck för FIM-XPath-filtret och garanterar att värdet för indata av användaren är unika inom de resurser som ingår i omfattningen av filtret.
    För att säkerställa att användaren begärde namn är unikt inom alla e-post aktiverad säkerhetsgrupper i FIM-Tjänstdatabasen, skulle du till exempel använda XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. Åtgärden verifiering utförs när användaren lämnar sidan. Den här egenskapen stöds endast i RCDC för att skapa en resurs.

8.  UniquenessErrorMessage: Detta är en valfri, egenskap av typen string. Den här strängen används för att visa ett felmeddelande om UniquenessValidationXPath valideringen misslyckas, och kan vara explicit text eller en sträng resurs variabeln. Om den här egenskapen inte anges är standardfelmeddelandet för en misslyckad validering ”%-värdet finns redan. Försök med en annan ”.

Händelser:

   • TextChanged: den här händelsen har genererats när texten i textrutan ändras.

Avsnittet enkelt exempel som ett fullständigt exempel för den här kontrollen.

## <a name="appendix-a-default-xsd-schema"></a>Bilaga A: standard XSD-Schema

Följande är klar XSD-schemat för alla standard RCDCs som tillhandahålls med Microsoft Identity Manager 2016 SP1:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>Bilaga B: standard sammanfattning XSL

Följande är fullständig sammanfattning XSL som tillhandahålls med Microsoft Identity Manager 2016 SP1:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
