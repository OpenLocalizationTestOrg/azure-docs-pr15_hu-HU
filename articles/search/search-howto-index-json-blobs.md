<properties
pageTitle="A keresés Azure blob-indexelő JSON BLOB indexelés"
description="A keresés Azure blob-indexelő JSON BLOB indexelés"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>A keresés Azure blob-indexelő JSON BLOB indexelés 

Ez a cikk bemutatja, hogyan konfigurálható a keresési Azure blob indexelő strukturált tartalom kinyerése JSON tartalmazó BLOB.

## <a name="scenarios"></a>Felhasználási területei

Alapértelmezés szerint a [keresési Azure blob-indexelő](search-howto-indexing-azure-blob-storage.md) JSON BLOB elemzi a szöveg egyetlen adattömb szerint. Gyakran érdemes megőrzése a JSON dokumentumok felépítésének. Ha például adott a JSON-dokumentum 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

érdemes lehet elemezni a "szöveg", "datePublished" és "kódok" mezők Azure keresési dokumentumba.

Azt is megteheti a BLOB **JSON objektumok tömbje**tartalmaz, akkor érdemes a tömb lesz egy külön Azure keresési dokumentum minden elemet. Ha például adott, a JSON blob:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

a saját "azonosító" és "szöveg" mezőt Azure keresési index 3 külön dokumentumokkal kitöltheti. 

> [AZURE.IMPORTANT] Ez a funkció jelenleg előzetes verzióban. Akkor érhető el, csak a **Skype 2015-02-28 – előzetes**verzió használatával REST API-t. Kérjük, ne feledje, előnézete API-k tesztelése és az kiértékelés szánt, és nem használható éles környezetben. 

## <a name="setting-up-json-indexing"></a>Az indexelés JSON beállítása

A tárgymutató-JSON BLOB, állítsa be a `parsingMode` konfigurációs paramétere `json` (az egyes blob egyetlen dokumentumként index) vagy `jsonArray` (Ha a BLOB tartalmaznak a JSON tömb): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Ha szükséges, a **mező megfeleltetésének** használatával válassza ki a cél keresési index kitöltéséhez használt JSON forrásdokumentumban tulajdonságainak.  Ez az alábbi részletes ismertetése. 

> [AZURE.IMPORTANT] Használatakor `json` vagy `jsonArray` elemzése mód, Azure keresési feltételezi, hogy, hogy az adatforrásban valamennyi BLOB lesz JSON. Ha kombinálja a JSON, és nem JSON BLOB támogatja az azonos adatforrás van szüksége, tudassa velünk [UserVoice](https://feedback.azure.com/forums/263029-azure-search)webhelyünkön.

## <a name="using-field-mappings-to-build-search-documents"></a>A mező hozzárendelések használata a dokumentumok keresése a létrehozásához 

Jelenleg Azure keresés nem index tetszőleges JSON dokumentumokat közvetlenül, mert csak egyszerű adattípusok, karakterlánc tömbök és GeoJSON pontok támogat. Azonban "emelje fel a" őket a keresési dokumentum legfelső szintű mezőkbe, és válassza ki a JSON dokumentumrész **mező megfeleltetésének** is használhatja. Mező megfeleltetésének alapjai kapcsolatos további tudnivalókért lásd: [Azure keresési indexelő mező megfeleltetésének egymással, áthidalhatják a adatforrások és a keresési index közötti különbségeket](search-indexer-field-mappings.md).

Példa JSON dokumentum visszatérésének: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Vegyük a keresési index, a következő mezőket: `text` Edm.String, típusú `date` típusú Edm.DateTimeOffset, és `tags` Collection(Edm.String) típusú. A JSON feleltesse meg a kívánt alakzatba, használja a következő mező megfeleltetésének: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

A forrás mezőnevek megfeleltetések adható [JSON mutató](http://tools.ietf.org/html/rfc6901) jelölés használatával. Ha nézni szeretné a JSON-dokumentum legfelső szintű perjellel kezdődik, majd hatoljon le a kívánt tulajdonság (a beágyazási tetszőleges szintjén), előre perjel tabulátorral tagolt elérési út segítségével. 

Akkor is hivatkozhat egyes tömbelemek nulla alapú index létrehozása használatával. Válassza ki az első elemet a fenti példában a "kódok" tömb, például használja a mező megfeleltetésének jelennek meg:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Ha egy forrásnév mező a hozzárendelés mező az elérési egy tulajdonság, amely nem szerepel a JSON hivatkozik, a leképezés kimarad hiba nélkül. Ez történik, hogy egy másik séma (ami egy közös használatieset) tartalmazó dokumentumok is támogatjuk. Mivel nincs ellenőrzés, kell elkerülése érdekében a mező megfeleltetés specifikációja elírásából adódnak gondoskodik. 

Ha a JSON dokumentumokat csak tartalmaz egyszerű legfelső szintű tulajdonságai, nem lehet, mező megfeleltetésének egyáltalán. Például a JSON így néz ki, ha a legfelső szintű tulajdonságok "szöveg", "datePublished" és "kódok" fog közvetlenül a megfelelő mezőinek egyeztetése a keresési index: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Az indexelési beágyazott JSON tömbök

Mi történik, ha ki szeretne JSON objektumok tömbjét, de a tömb index van beágyazva valahol a dokumentumot? Melyik tulajdonság tartalmazza a tömb használatával is választhat a `documentRoot` konfigurációs tulajdonságot. Ha például a BLOB néz ki: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

e beállítás használatával a tömb tartalmazza az "level2" tulajdonságban indexelni: 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Példák kérése

Ez az összes elhelyez együtt, az alábbi példák a teljes tartalmak számára. 

Adatforrás: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indexelő:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Segítse a szolgáltatás Azure keresési továbbfejlesztésében

Ha kérhetek új funkciókat vagy javítása ötleteket, kérjük, keresse meg a kapcsolatfelvételi a [UserVoice webhelyen](https://feedback.azure.com/forums/263029-azure-search/).