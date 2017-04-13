<properties
pageTitle="Az Azure keresési indexelő mező megfeleltetésének"
description="Mezőnevek, és az adatok megjelenítését a különbségeket fiókhoz Azure keresési indexelő mező megfeleltetésének beállítására"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Az Azure keresési indexelő mező megfeleltetésének

Amikor keresés Azure indexelő, időnként megtalálhatja saját maga esetekben, ahol a beviteli adatok nem igazán felel meg a sémának a cél index. Ezekben az esetekben **mező megfeleltetésének** is használhatja az adatok alakíthat ki a kívánt alakzatot. 

Bizonyos esetekben, ahol mező megfeleltetésének hasznosak:
 
- Az adatforrás tartalmaz olyan mezőt `_id`, de Azure keresés nem engedélyezi a mezőnevek aláhúzásjel kezdve. A mező megfeleltetésének mező "átnevezése" teszi lehetővé. 
- Fel kívánja tölteni a ugyanezekkel az adatokkal adatok forrása, például a tárgymutató több mezőt, mert másik gázelemzők alkalmazni a módosításokat. A mező megfeleltetésének adhatja meg a "ágaznak" adatforrás mezője.
- Base64 kódolását vagy szükséges dekódolását az adatok. Mező megfeleltetésének több **megfeleltetés függvények**, többek között a Base64 függvényei kódolás és visszafejtés támogatja.   


> [AZURE.IMPORTANT] A mező megfeleltetésének funkciók jelenleg előzetes verzióban. Akkor érhető el, csak a **Skype 2015-02-28 – előzetes**verzió használatával REST API-t. Kérjük, ne feledje, előnézete API-k tesztelése és az kiértékelés szánt, és nem használható éles környezetben.

## <a name="setting-up-field-mappings"></a>A mező megfeleltetésének beállítása

A mező megfeleltetésének is hozzáadhat, az [Indexelő létrehozása](search-api-indexers-2015-02-28-preview.md#create-indexer) API segítségével egy új indexelő létrehozásakor. Kattintson a [Frissítés indexelő](search-api-indexers-2015-02-28-preview.md#update-indexer) API segítségével az indexelő indexkiszolgáló mező megfeleltetésének kezelése 

A mező megfeleltetésének 3 részből áll: 

1. A `sourceFieldName`, amely jelenti, hogy egy mezőt az adatforrásban. Ez a tulajdonság szükség. 

2. Választható `targetFieldName`, amely jelenti, hogy a keresési index mező. Adja meg, ha ugyanazt a nevet, ahogy az adatforrás használják. 

3. Választható `mappingFunction`, amely alakíthatják át az adatok közül számos az előre definiált függvényeket. A függvények teljes listája [alatt](#mappingFunctions).

Mezők megfeleltetésének bekerülnek a `fieldMappings` az indexelő definition tömb. 

Ha például az alábbiakban hogyan rendezhetők a mezőnevek közötti különbségek: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indexkiszolgáló beállíthatja, hogy több mező megfeleltetésének. Ha például az alábbiakban hogyan akkor is "ágaznak" mező:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure keresési nagybetűk összehasonlító használja a mező és a függvény a nevek mező megfeleltetésének feloldása. Ez kényelmes (nem kell minden géphez megfelelő első), de azt jelenti, hogy az adatforrás vagy index nem lehet mezőket, amelyeket csak eset különbözőek.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Mező a hozzárendelés függvények

Ezek a funkciók jelenleg támogatott: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Hajt végre, a bemeneti karakterlánc *URL-cím vannak* Base64 kódolását. Feltételezi, hogy a bemeneti UTF-8 kódolású. 

#### <a name="sample-use-case"></a>Minta használati eset 

Csak az URL-cím vannak karaktereket (azért, mert az ügyfelek kell tenni a dokumentumot a keresési API-t, például a cím) jelenhetnek meg egy dokumentum Azure keresési billentyűt. Ha azt szeretné, hogy a keresési index kulcsmező kitöltéséhez használandó az adatok URL-címe nem biztonságos karaktereket tartalmaz, a függvény használatáról.   

#### <a name="example"></a>Példa 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Hajt végre, a bemeneti karakterlánc dekódolását Base64. A bemeneti *URL-cím vannak* Base64-címként kódolt karakterláncot tekinti. 

#### <a name="sample-use-case"></a>Minta használati eset 

Egyéni metaadatértékekkel BLOB kell lennie az ASCII-címként kódolt. Base64 kódolás segítségével egyéni blob-metaadatok tetszőleges Unicode karakterláncok képviseli. Azonban, hogy értelmes keresési, segítségével ezt a funkciót, ha a keresési index feltöltése vissza alakítani "normál" karakterláncot a kódolt adatok.  

#### <a name="example"></a>Példa 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Egy karakterlánc mezőben használja a megadott elválasztó kettéválaszthatja-e, és a token választja ki az eredményül kapott felosztása a megadott pozícióban.

Ha például a bemeneti `Jane Doe`, a `delimiter` van `" "`(szóköz), és a `position` értéke 0, az eredmény `Jane`; Ha a `position` értéke 1, az eredmény `Doe`. Ha a pozíció hivatkozik egy nem létező jogkivonat, hiba történt visszatér.

#### <a name="sample-use-case"></a>Minta használati eset 

Az adatforrás tartalmaz egy `PersonName` mezőbe, és szeretné azt tárgymutató-két külön `FirstName` és `LastName` mezőket. Ez a funkció használatával a bemeneti a szóköz használata elválasztóként felosztása.

#### <a name="parameters"></a>Paraméterek

- `delimiter`: a bemeneti karakterlánc felosztása elválasztó melyikkel karakterlánc.
- `position`: a jogkivonat kiválasztása után a bemeneti karakterlánc osztott egy egész szám nulla alapú pozícióját.    

#### <a name="example"></a>Példa

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Egy karakterlánc, egy karakterlánc tömböt kitöltéséhez használt be karakterláncok JSON tömbként formázott átalakítások egy `Collection(Edm.String)` az index mezőjében. 

Ha például a bemeneti karakterlánc `["red", "white", "blue"]`, majd a célmező típusa `Collection(Edm.String)` tölti fel a három értékekkel `red`, `white` és `blue`. A bemeneti értékek, amelyek nem értelmezhetők JSON-karakterlánc tömbök hibaüzenetet fog rendszer. 

#### <a name="sample-use-case"></a>Minta használati eset

Azure SQL-adatbázis nincs beépített adattípus, természetesen megfelelteti `Collection(Edm.String)` Azure keresés mezőket. Karakterlánc webhelycsoport mezők feltöltése, a forrásadatokban JSON karakterlánc tömbként formázása, és ezzel a funkcióval. 

#### <a name="example"></a>Példa 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Segítse a szolgáltatás Azure keresési továbbfejlesztésében

Ha kérhetek új funkciókat vagy javítása ötleteket, kérjük, keresse meg a kapcsolatfelvételi a [UserVoice webhelyen](https://feedback.azure.com/forums/263029-azure-search/).