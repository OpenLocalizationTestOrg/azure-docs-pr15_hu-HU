<properties 
    pageTitle="Rövid útmutató: gépi tanulási javaslatok API |} Microsoft Azure" 
    description="Azure gépi tanulási javaslatok – rövid útmutató" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Rövid útmutató az gépi tanulási javaslatok API

>[AZURE.NOTE]Ez a verzió nem kell a javaslatok kognitív API-szolgáltatás használatbavétele. A javaslatok kognitív szolgáltatást fogja cseréje ezt a szolgáltatást, és az új szolgáltatások vannak készüljön. Új funkciók támogatása, jobban API Explorer, a tisztább API felületre, egységesebb előfizetési és számlázási élmény stb kötegelés például rendelkezik.
> További tudnivalók [az új kognitív szolgáltatás áttelepítése](http://aka.ms/recomigrate)


A dokumentum ismerteti, hogyan való beépített a szolgáltatás vagy a Microsoft Azure gépi tanulási javaslatok használni kívánt alkalmazást. További részleteket a javaslatok API a [gyűjteményben](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2)található.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Általános áttekintés

Azure gépi tanulási javaslatok használatához kell kövesse az alábbi lépéseket:

* Hozzon létre egy a modellben - a modell a használati adatok, katalógusadatok és az ajánlási modell tároló.
* Katalógus adatok importálása - katalógust az elemek metaadatok információkat tartalmaz. 
* Látogatottsági adatok importálása - használati adatainak fel kell tölteni a 2 módszerek valamelyikével (vagy mindkettő):
    * Által használatát adatokat tartalmazó fájl feltöltésekor.
    * Adatok WIA események küldésével.
    Használatát fájl általában annak érdekében, hogy az eredeti ajánlási modell (betöltő) létrehozása és használata mindaddig, amíg a rendszer az elég adatait az adatformátum WIA használatával gyűjti össze tudja feltöltése.
* Javaslat modell összeállítása – Ez az ajánlási rendszer szükséges összes használati adatainak és létrehoz egy ajánlási modell aszinkron műveletet. Ez a művelet vehet igénybe néhány percig, amíg vagy több óra az adatok és a Szerkesztés konfigurációs paraméterek méretétől függően. Az összeállítás el, ha jelenik meg egy összeállítás azonosítót. Használni, amikor az összeállítás folyamat javaslatok használhatnak megkezdése előtt véget ért.
* Javaslatok - Get javaslatok egy adott elem vagy az elemek listáját mobilalkalmazásokban

Az összes, a fenti lépéseket az Azure gépi tanulási javaslatok API-k történik.  Letöltheti a minta alkalmazás, amely az egyes a lépéseket a [, valamint a gyűjtemény.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Korlátozások

* A maximális száma előfizetésenként modellek 10.
* A katalógus élvező elemek maximális száma 100 000.
* Maximális száma, melyek folyamatosan használatát pontok ~ 5,000,000. A legrégebbi is törli, ha újakat fog tölthetők fel vagy jelentett.
* A bejegyzés (pl. katalógus adatok importálása párbeszédpanelen használati adatok importálása) küldött adatok maximális mérete 200 MB-ot.
* A szám, amely nem aktív ajánlási modell építés másodpercenként tranzakciók ~ 2TPS. Aktív ajánlási modell építés is tartsa lenyomva az ujját való 20TPS.

##<a name="integration"></a>Integráció

###<a name="authentication"></a>Hitelesítés
Micosoft Azure piactéren elérhető támogatja a Basic vagy OAuth hitelesítési módszert. A fiók billentyűk könnyen megtalálhatja a piactér, [a Fiókbeállítások](https://datamarket.azure.com/account/keys)csoportban lévő kulcsok való navigálással. 
####<a name="basic-authentication"></a>Alapszintű hitelesítés
Az engedély élőfej hozzáadása:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Base64 átalakítása (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Base64 átalakítása (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>A szolgáltatás URI
A szolgáltatás legfelső szintű az Azure gépi tanulási javaslatok API-k URI [itt.](https://api.datamarket.azure.com/amla/recommendations/v2/)

A teljes szolgáltatás URI van megadva, az OData-specifikáció elemek használatával.

###<a name="api-version"></a>API-verzió
Minden API-hívás a végén lesz egy lekérdezési paraméter apiVersion, amely a "1.0" értékre kell állítani.

###<a name="ids-are-case-sensitive"></a>Azonosítók nagybetűk
Azonosítók, bármelyik az API-k által visszaadott nagybetűk, és a használandó ilyenként paraméterként további API-hívások. Például modell azonosítókhoz és a katalógus azonosítók nagybetűk között.

###<a name="create-a-model"></a>Adatmodell létrehozása
"A modell létrehozása" kérelem létrehozása:

| HTTP-metódus | URI |
|:--------|:--------|
|BEJEGYZÉS     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
|   modelName   |   Csak betűk (A – Z, a – z) számokat (0 – 9) kötőjeleket (--), és az aláhúzásjel _ engedélyezettek.<br>A maximális hossza: 20 |
|   apiVersion      | 1.0 |
|||
| Szervezet kérése | NINCS LEHETŐSÉG |


**Válasz**:

HTTP-állapotkód: 200

- `feed/entry/content/properties/id`-Tartalmaz a modell azonosítóját.
**Megjegyzés**: modell azonosító a kis-és nagybetűk.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Adatok importálása katalógus

Ha több katalógus fájl feltöltése a több hívások ugyanazt a modellt, azt az új alkalmazáskatalógus elemek szúrja be. Az eredeti értékeket tartalmazó elemek marad.

| HTTP-metódus | URI |
|:--------|:--------|
|BEJEGYZÉS     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
|   modelId |   Egyedi azonosító (kis-és nagybetűk) a modell  |
| fájlnév | A katalógus szöveges azonosítója.<br>Csak betűk (A – Z, a – z) számokat (0 – 9) kötőjeleket (--), és az aláhúzásjel _ engedélyezettek.<br>A maximális hossza: 50 |
|   apiVersion      | 1.0 |
|||
| Szervezet kérése | Katalógus adatokat. Formátum:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>név</th><th>Kötelező</th><th>Típus</th><th>Leírás</th></tr><tr><td>Listaelem-azonosító</td><td>igen</td><td>Alfanumerikus, maximális hossz 50</td><td>Egy elem egyedi azonosító</td></tr><tr><td>Elem neve</td><td>igen</td><td>Alfanumerikus, maximális hossz 255 karakter</td><td>Elem neve</td></tr><tr><td>Elem kategória</td><td>igen</td><td>Alfanumerikus, maximális hossz 255 karakter</td><td>Kategóriájára ezt az elemet (például főző könyvek, ábráknak...)</td></tr><tr><td>Leírás</td><td>nem</td><td>Alfanumerikus, maximális hossz 4000</td><td>Ez a cikk leírása</td></tr></table><br>Maximális fájlmérete 200 MB-ot.<br><br>Példa:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan, a címjegyzék<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, a PIN-kód elfelejtése szobában: lefoglalhat egy fantasztikus (Byzantium címjegyzék),<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework, a címjegyzék<br>552a1940-21e4-4399-82bb-594b46d7ed54, korlátozás a vadállatok, a címjegyzék</pre> |


**Válasz**:

HTTP-állapotkód: 200

- `Feed\entry\content\properties\LineCount`-Elfogadott sorok számát.
- `Feed\entry\content\properties\ErrorCount`-Hiba miatt nem beszúrt sorok számát.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Látogatottsági adatok importálása

####<a name="uploading-a-file"></a>Egy fájl feltöltése
Ebben a részben használati adatainak feltöltése-fájl használatával. Ez az API használatát adatokat tartalmazó többször is felhívhatja. Minden használati adatainak a program menti az összes hívásokhoz.

| HTTP-metódus | URI |
|:--------|:--------|
|BEJEGYZÉS     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
|   modelId |   Egyedi azonosító (kis-és nagybetűk) a modell |
| fájlnév | A katalógus szöveges azonosítója.<br>Csak betűk (A – Z, a – z) számokat (0 – 9) kötőjeleket (--), és az aláhúzásjel _ engedélyezettek.<br>A maximális hossza: 50 |
|   apiVersion      | 1.0 |
|||
| Szervezet kérése | Látogatottsági adatok. Formátum:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>név</th><th>Kötelező</th><th>Típus</th><th>Leírás</th></tr><tr><td>Felhasználói azonosító</td><td>igen</td><td>Alfanumerikus</td><td>Egy felhasználó egyedi azonosítója</td></tr><tr><td>Listaelem-azonosító</td><td>igen</td><td>Alfanumerikus, maximális hossz 50</td><td>Egy elem egyedi azonosító</td></tr><tr><td>Idő</td><td>nem</td><td>Formátumú dátum: YYYY/hh/ddTHH (pl. 2013/06/20T10:00:00)</td><td>Az adatok idő</td></tr><tr><td>Esemény</td><td>Nem, ha a megadott, akkor is kell elhelyezni a dátum</td><td>Az alábbiak egyikét:<br>• Kattintás<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Megvásárlása</td><td></td></tr></table><br>Maximális fájlmérete 200 MB-ot.<br><br>Példa:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Válasz**:

HTTP-állapotkód: 200

- `Feed\entry\content\properties\LineCount`-Elfogadott sorok számát.
- `Feed\entry\content\properties\ErrorCount`-Hiba miatt nem beszúrt sorok számát.
- `Feed\entry\content\properties\FileId`-Fájl azonosítója.


Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Adatbeszerzési használata
Ez a szakasz megtudhatja, hogy miként küldhet eseményeket valós idejű Azure gépi tanulási javaslatok, általában a webhelyről.

| HTTP-metódus | URI |
|:--------|:--------|
|BEJEGYZÉS     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Szervezet kérése| Az egyes események esemény adatbevitel el szeretné küldeni. Küldje el az adott felhasználó vagy a böngésző munkamenetben az ugyanazt az Azonosítót a munkamenet-azonosító mező. (Lásd: az esemény törzsében az alábbi példák.)|


- Esemény "Kattintson" Példa:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esemény "RecommendationClick" Példa:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esemény "AddShopCart" Példa:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esemény "RemoveShopCart" Példa:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esemény "Vásárlás" Példa:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Küldés 2 események, kattintson"és"AddShopCart"Példa:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Válasz**: HTTP állapotkód: 200

###<a name="build-a-recommendation-model"></a>Javaslat modell összeállítása

| HTTP-metódus | URI |
|:--------|:--------|
|BEJEGYZÉS     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
| modelId | Egyedi azonosító (kis-és nagybetűk) a modell  |
| userDescription | A katalógus szöveges azonosítója. Figyelje meg, hogy ha szóközöket, kell kódolást 20 %-os helyette. A fenti példában látható.<br>A maximális hossza: 50 |
| apiVersion | 1.0 |
|||
| Szervezet kérése | NINCS LEHETŐSÉG |

**Válasz**:

HTTP-állapotkód: 200

Az aszinkron API-val. A Szerkesztés azonosító válaszként fog kapni. Tudja meg, amikor összeállítása véget ért, érdemes hívja fel az "Első épít állapotát, a modell" API, és keresse meg a választ az összeállítás azonosító. Fontos tudni, hogy egy összeállítás is perctől az adatok méretétől függően órára.

Javaslatok összeállítása eddig nem lehet felhasználni befejeződik.

Érvényes build állapot:

- Hozzon létre – modell hozták létre.
- Aszinkron – modell build indított volt, és azt az aszinkron.
- Építőelem – modell elkészült.
- SUCCESS – a összeállítás sikeresen befejeződött.
- Hibaüzenet – Build véget hibát.
- Visszavont – Build megszakították.
- Lemondása – összeállítás megszakítása folyamatban van.


Tartsa szem előtt, hogy az összeállítás azonosító csoportban a következő helyen található:`Feed\entry\content\properties\Id`

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>A modell build állapotának beszerzése

| HTTP-metódus | URI |
|:--------|:--------|
|GET     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
|   modelId         |   Egyedi azonosító (kis-és nagybetűk) a modell    |
|   onlyLastBuild   |   Azt jelzi, hogy a modell build előzmények vagy csak a legutóbbi szerkesztés állapotának ad vissza. |
|   apiVersion      |   1.0                                 |


**Válasz**:

HTTP-állapotkód: 200

A válasz egy tételt összeállítás tartalmazza. Minden tétel rendelkezik az alábbi adatokat:

- `feed/entry/content/properties/UserName`– Annak a felhasználónak a nevét.
- `feed/entry/content/properties/ModelName`– A modell nevét.
- `feed/entry/content/properties/ModelId`– Minta egyedi azonosítója.
- `feed/entry/content/properties/IsDeployed`– E összeállítása (más néven rendszerbe aktív build).
- `feed/entry/content/properties/BuildId`– Egyedi azonosító összeállítása.
- `feed/entry/content/properties/BuildType`-Összeállítása típusát.
- `feed/entry/content/properties/Status`– Összeállítása állapotát. A következők egyike lehet: hiba, épület, aszinkron, Cancelling, lemondva, sikeres
- `feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak az adott államok érvényes).
- `feed/entry/content/properties/Progress`– Összeállítása állapotát (%).
- `feed/entry/content/properties/StartTime`– Összeállítása a kezdési időpontot.
- `feed/entry/content/properties/EndTime`– Összeállítása a befejezési időpontot.
- `feed/entry/content/properties/ExecutionTime`– Időtartam összeállítása.
- `feed/entry/content/properties/ProgressStep`– Az aktuális szakasz, amely a folyamatban lévő építés olvashat.

Érvényes build állapot:
- Létrehozott – Build kérelem tétel jött létre.
- Aszinkron – Build kérelem indított volt, és azt az aszinkron.
- Építőelem – összeállítás folyamatban van.
- SUCCESS – a összeállítás sikeresen befejeződött.
- Hibaüzenet – Build véget hibát.
- Visszavont – Build megszakították.
- Lemondása – összeállítás megszakítása folyamatban van.

Szerkesztés típusa érvényes értékeit:
- Rangsor - rangját összeállítása. (Rangsor felépítése a részleteket, olvassa el a "Gépi tanulási javaslat API dokumentációját" dokumentum.)
- Javaslat - ajánlási build.
- FBT – gyakran vásárolt egy közös Szerkesztés.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Javaslatok beszerzése

| HTTP-metódus | URI |
|:--------|:--------|
|GET     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
| modelId | Egyedi azonosító (kis-és nagybetűk) a modell |
| hogy az elemazonosítók | Javasoljuk az elemek vesszővel tagolt listáját.<br>A maximális hossza: 1024 |
| numberOfResults | Szükséges találatok száma |
| includeMetatadata | Mindig hamis későbbi használatra |
| apiVersion | 1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott elemet tartalmazza. Minden tétel rendelkezik az alábbi adatokat:

- `Feed\entry\content\properties\Id`-Excel elem azonosítója.
- `Feed\entry\content\properties\Name`-Az elem neve.
- `Feed\entry\content\properties\Rating`-Az ajánlási; minősítése nagyobb szám nagyobb megbízhatóság azt jelenti.
- `Feed\entry\content\properties\Reasoning`-Ajánlási mintafelismerési (pl. ajánlási magyarázatok).

Az OData-XML

Az alábbi példa válasz 10 ajánlott elemeket tartalmazza:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Frissítési modell
Frissítheti a modell leírása vagy az aktív build azonosítójával.
*Aktív build azonosító* – minden build minden modell build azonosítóval rendelkezik. Az aktív build azonosító minden új modell első sikeres összeállítása. Van egy aktív build azonosító, és el további épít ugyanazt a modellt, explicit módon beállítja az alapértelmezett build ID Ha azt szeretné, hogy kell. Javaslatok, ha nem adja meg a használni kívánt összeállítás Azonosítót használja fel, ha az alapértelmezett automatikusan használható.

Ez az eljárás lehetővé teszi, -, ha már ajánlási modell a gyártási – új modellek összeállítása és előtt előléptetése őket a termelési tesztelni őket.

| HTTP-metódus | URI |
|:--------|:--------|
|HELYEZÉSE     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Paraméterek neve  |   Az érvényes értékek                        |
|:--------          |:--------                              |
| azonosító | Egyedi azonosító (kis-és nagybetűk) a modell |
| apiVersion | 1.0 |
|||
| Szervezet kérése | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Ne feledje, hogy az XML-címkéket, leírás és ActiveBuildId nem kötelező. Ha nem szeretné, hogy leírás vagy ActiveBuildId kíván megadni, a teljes címke eltávolítása. |

**Válasz**:

HTTP-állapotkód: 200

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Jogi
A dokumentum megadva "mint-van". Adatok és a dokumentum URL-CÍMEK és más internetes webhely, beleértve a kifejezett nézetek értesítés nélkül változhatnak. Néhány példa szereplő használható ábra csak és kitalált. Nincs valósággal kapcsolat íródott vagy eseményekkel. A dokumentum nem nyújt jogot bármely szellemi tulajdonnal kapcsolatos bármilyen Microsoft-termék. Előfordulhat, hogy másolja a vágólapra, és használja ezt a dokumentumot saját, belső tájékoztatási célra. © 2014-es Microsoft. Microsoft Corporation. 
 
