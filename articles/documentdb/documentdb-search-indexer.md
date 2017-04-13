<properties
    pageTitle="Csatlakozás DocumentDB az indexelő használatával Azure keresés |} Microsoft Azure"
    description="Ez a cikk bemutatja, hogyan adatforrásként DocumentDB az Azure keresési indexelő használatával."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Csatlakozás DocumentDB az indexelő használatával Azure-keresés

Ha remek keresési élményt a DocumentDB adatot felett végrehajtásához, Azure keresési indexelő szolgáltatás használata DocumentDB! Ebben a cikkben bemutatjuk, hogyan Azure DocumentDB integrálása az Azure keresési anélkül, hogy az indexelő infrastruktúra megőrzéséhez bármely kódírás!

Ezeket a beállításokat, hogy az [Azure keresési fiók beállítása](../search/search-create-service-portal.md) (nem kell frissítése a szabványos keresési) rendelkezik, és hívhatja DocumentDB **adatforrás** és az **Indexelő** adott adatforrás létrehozása az [Azure keresési REST API -t](https://msdn.microsoft.com/library/azure/dn798935.aspx) .

A sorrend küldés kérelmek a REST API-k használata [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)vagy a beállításait, bármilyen eszközzel is használhatja.


##<a id="Concepts"></a>Azure keresési indexelő fogalmak

Azure keresési támogatja, létrehozásának és kezelésének adatok forrásokat (beleértve a DocumentDB) és a ellen adatforrások működő indexelő.

Egy **adatforráshoz** Megadja, hogy milyen adatokat kell indexelni, hitelesítő adatokat, és ahhoz, hogy hatékony azonosítja a módosításokat, az adatok (például módosított vagy törölt belül a követett dokumentumok) Azure keresési házirendek eléréséhez. Az adatforrás független erőforrásként van definiálva, így több indexelő használható.

Egy **indexelési** ismerteti, hogyan az adatok szövegdobozról az adatforrás a cél keresési indexbe. Meg kell terveznie egy indexelő minden cél index és az adatok forrása kombináció létrehozásával kapcsolatban. Lehet, hogy több indexelő ír be ugyanazt az indexet, miközben az indexelő csak írhat be egyetlen index létrehozása. Indexkiszolgáló használt:

- Végezze el az adatok kitöltéséhez az index egyszeri másolatát.
- Index az ütemezés az adatforrásban végrehajtott módosítások szinkronizálása. Az ütemezés az indexelő definition része.
- Szükség szerint index igény szerinti frissítéseire hivatkozhat.

##<a id="CreateDataSource"></a>Lépés: 1: Adatforrás létrehozása

Új adatforrás létrehozása az Azure keresési szolgáltatás, a következő kérelem fejlécekkel együtt a HTTP-bejegyzés kérelmének ki.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

A `api-version` szükség. Az érvényes értékek `2015-02-28` vagy újabb verziója. Keresse fel [az Azure keresési API-verziók](../search/search-api-versions.md) összes támogatott keresési API-verziók megtekintéséhez.

A szöveg a kérelem tartalmazza az adatforrás-definíciót, amelyben szerepelnie kell a következő mezőket:

- **név**: válassza ki az DocumentDB adatbázis ábrázolásához bármely nevét.

- **Írja be**: használata `documentdb`.

- **hitelesítő adatok**:

    - **connectionString**: szükséges. Adja meg a kapcsolat adatai az Azure DocumentDB-adatbázisba a következő formátumban:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **tároló**:

    - **név**: szükséges. Adja meg az indexelendő DocumentDB webhelycsoport azonosítóját.

    - **lekérdezés**: nem kötelező. Megadhatja, hogy a lekérdezés egy tetszőleges JSON dokumentum összeolvasztása az Azure keresési index is strukturálatlan sémára.

- **dataChangeDetectionPolicy**: nem kötelező. Lásd: az [adatok észlelési házirendjének módosítása](#DataChangeDetectionPolicy) az alábbi.

- **dataDeletionDetectionPolicy**: nem kötelező. Keresse meg az alábbi [adatok törlési észlelési házirend](#DataDeletionDetectionPolicy) .

Lásd az alábbi egy [Példa összehívás törzsébe](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Módosított dokumentumokat rögzítése

Az adatok módosítása észlelési házirend célja módosult adatelemeket hatékony azonosítása. Az egyetlen támogatott házirend jelenleg a `High Water Mark` házirend használata a `_ts` utoljára módosította időbélyeg tulajdonság által DocumentDB - szerepel a következőképpen:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Is kell hozzáadnia `_ts` a a kivetítés és `WHERE` záradék a lekérdezés. Példa:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Törölt dokumentum rögzítése

Amikor sorokat a forrástáblához törlődnek, azok a sorok kell töröl, valamint a keresési index. Az adatok törlését észlelési házirend célja hatékony az adatokat a törölt elemek azonosítása. Az egyetlen támogatott házirend jelenleg a `Soft Delete` házirend (Törlés funkciója jelzővel van megjelölve), amely szerepel az alábbi képlettel történik:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Szüksége lesz egy egyéni kivetítés használatakor a softDeleteColumnName tulajdonság felvenni a SELECT záradékban.

###<a id="CreateDataSourceExample"></a>Példa a szervezet kérése

Az alábbi példa létrehoz egy adatforrást az egyéni lekérdezés és a házirend javaslatok:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Válasz

HTTP 201 létrehozott választ értesítést kap, ha az adatforrás létrehozása sikeresen befejeződött.

##<a id="CreateIndex"></a>Lépés: 2: Index létrehozása

Cél Azure keresési index létrehozása, ha nincs telepítve egyik már. Ehhez az [Azure portál felhasználói felület](../search/search-create-index-portal.md) vagy a [Tárgymutató-API létrehozása](https://msdn.microsoft.com/library/azure/dn798941.aspx).

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Ellenőrizze, hogy a célhely index sémájának a séma a forrás JSON dokumentumok vagy az egyéni lekérdezés kivetítés kimenetét kompatibilis.

>[AZURE.NOTE] Az alapértelmezett dokumentum kulcs particionált gyűjtemények, DocumentDB meg `_rid` tulajdonság, amely az átnevezett kap `rid` Azure keresés. Ezenkívül DocumentDB's `_rid` értékek Azure keresési kulcsok; érvénytelen karaktereket tartalmaz. Emiatt a `_rid` értékei kódolt Base64.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>A: ábra JSON adattípusok és Azure keresési adattípusok közötti hozzárendelés

| JSON ADATTÍPUS|   KOMPATIBILIS CÉL INDEX MEZŐTÍPUSOK|
|---|---|
|Logikai|Edm.Boolean, Edm.String|
|Számok, egész számok néznek|Edm.Int32, Edm.Int64, Edm.String|
|Számok, így néznek lebegő pontok|Edm.Double, Edm.String|
|Karakterlánc|Edm.String|
|Egyszerű tömbök fájltípusok: például "a", "b"; "c" |Collection(EDM.String)|
|Karakterláncok, amelyek így néznek dátumok| Edm.DateTimeOffset, Edm.String|
|GeoJSON objektumokat, például {"típus": "Pont", "koordináták": [hosszú, lat]} | Edm.GeographyPoint |
|Más JSON-objektumok|A #HIÁNYZIK|

###<a id="CreateIndexExample"></a>Példa a szervezet kérése

Az alábbi példa létrehoz egy indexet azonosító és a leírás mezővel:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Válasz

HTTP 201 létrehozott választ értesítést kap, ha az index létrehozása sikeresen befejeződött.

##<a id="CreateIndexer"></a>3 lépés: Az indexelő létrehozása

Egy új indexelő belül az Azure keresési szolgáltatás a következő élőfejjel HTTP POST felkérés használatával is létrehozhat.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

A szöveg a kérelem tartalmazza az indexelő-definíciót, amely tartalmaznia kell a következő mezőket:

- **név**: szükséges. Az indexelő neve.

- **következővel név**: szükséges. Létező adatforrás neve.

- **targetIndexName**: szükséges. Egy meglévő indexet neve.

- **Ütemezés**: nem kötelező. Lásd: [az indexelő ütemezés](#IndexingSchedule) az alábbi.

###<a id="IndexingSchedule"></a>Indexelő futó ütemezett

Másik lehetőségként indexkiszolgáló ütemezést is megadhat. Ütemezett esetén az indexelő ütemezve rendszeres fog futni. A következő attribútumok ütemezés foglalja magában:

- **intervallum**: szükséges. Időtartam érték, amely megadja egy időköz vagy az indexelő időszakot fut. Megengedett legkisebb intervallum 5 perccel; egy nappal leghosszabb. Akkor XSD "dayTimeDuration" értelmezi (korlátozott részhalmazát az [ISO 8601 időtartam](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) érték) lesz formázva. Ezt a minta: `P(nD)(T(nH)(nM))`. Példa: `PT15M` a 15 percenként `PT2H` minden 2 órán keresztül.

- **Kezdés időpontja**: szükséges. Egy UTC datetime, amely meghatározza, hogy az indexelő kell elkezdődnie operációs rendszert futtató.

###<a id="CreateIndexerExample"></a>Példa a szervezet kérése

Az alábbi példa létrehoz egy indexelő, amely a gyűjteményből által hivatkozott adatokat másolja a `myDocDbDataSource` az adatforrás a `mySearchIndex` index időközönként a január 1, a Skype 2015 UTC kezdődő és óránként lefut.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Válasz

HTTP 201 létrehozott választ értesítést kap, ha az indexelő sikeresen hozták létre.

##<a id="RunIndexer"></a>Lépés: 4: Indexkiszolgáló futtatása

Nemcsak a operációs rendszert futtató rendszeres időközönként, indexkiszolgáló is is hivatkozhat igény kiadása a következő HTTP POST kérelem:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Válasz

HTTP 202 elfogadott választ értesítést kap, ha az indexelő sikeresen elindították.

##<a name="GetIndexerStatus"></a>Lépés 5: Get indexelési állapot

HTTP GET kérés indexkiszolgáló aktuális állapotát és a végrehajtás előzmények beolvasásához állíthatnak be:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Válasz

Ekkor megjelenik egy válasz a szervezet információkat összesített indexelési állapot állapot, utolsó indexelő meghívását, valamint a legutóbbi indexelő meghívásához előzményeit (ha van ilyen) tartalmazó együtt visszaadott HTTP 200 OK választ.

A válasz a következőhöz hasonlóan kell kinéznie:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Végrehajtási előzmények felfelé az 50 legutóbbi bejegyzett végrehajtások, amely vannak rendezve fordított időrendben (ahol a legújabb végrehajtás véget előbb, a válasz) tartalmazza.

##<a name="NextSteps"></a>Következő lépések

Gratulálok! Csak megtanulta azt Azure DocumentDB integrálása az Azure keresési az indexelő DocumentDB verzióval.

 - Megtudhatja, hogy hogyan Azure DocumentDB kapcsolatban, lásd: a [DocumentDB szolgáltatás lapon](https://azure.microsoft.com/services/documentdb/).

 - Hogyan további információ a Azure keresési című témakörben talál a [keresési szolgáltatás lapon](https://azure.microsoft.com/services/search/).
