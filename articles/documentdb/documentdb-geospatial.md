<properties 
    pageTitle="Azure DocumentDB az eszközzel térinformatikai adatokat vizsgálhat használata |} Microsoft Azure" 
    description="Megtudhatja, hogyan hozhat létre, index és a lekérdezés Azure DocumentDB térbeli objektumok." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Azure DocumentDB az eszközzel térinformatikai adatokat vizsgálhat használata

Ez a témakör az [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)térinformatikai funkciói bemutatása. Ez elolvasása, után fogja tudni az alábbi kérdésekre választ:

- Hogyan térbeli adatok tárolását Azure DocumentDB?
- Hogyan is lekérdezhetők az LINQ és az SQL Azure-DocumentDB az eszközzel térinformatikai adatokat?
- Hogyan engedélyezése vagy letiltása a térbeli indexelés DocumentDB?

Olvassa el a [Github project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) mintakódok.

## <a name="introduction-to-spatial-data"></a>Térbeli adatok – bevezetés

Térbeli adatok pozícióját és objektumok helyen alakjának ismerteti. A legtöbb alkalmazásban ezek megegyeznek a föld, azaz eszközzel térinformatikai adatokat vizsgálhat objektumokat. Térbeli adatok ábrázolására, egy személy, az érdeklődésre számot tartó hely vagy a város vagy egy tó szegélyét helyét használható. Közös használati eset gyakran közelség lekérdezéseket, például "keresése az összes kávézóban közelében a aktuális tartózkodási" foglalnak magukban. 

### <a name="geojson"></a>GeoJSON
DocumentDB támogatja az indexelés és a földrajzi helyétől [GeoJSON specifikációja](http://geojson.org/geojson-spec.html)használatával ábrázolt adatok lekérdezésére. Adatstruktúrák GeoJSON objektumai mindig érvényes JSON, úgy, hogy tárolhat, lekérdezett DocumentDB segítségével speciális eszközök és tárak nélkül. A DocumentDB SDK a segítő osztályok és módszerek, amelyek megkönnyítik a térbeli adatokkal végzett munkához szükséges. 

### <a name="points-linestrings-and-polygons"></a>Pontok, linestrings és sokszög
**Pont** azt jelzi, hogy egy helyen egyetlen helyre. Eszközzel térinformatikai adatokat vizsgálhat valamit jelöl, amelyek lehetnek a postai címe egy élelmiszerekre áruházból, a Kirakati bemutató, egy autót vagy a város pontos helyét.  Pont GeoJSON (és DocumentDB) használata a koordináta pár vagy a hosszúság és a szélesség jeleníti meg. Íme egy példa JSON pont.

**DocumentDB pontok**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] A GeoJSON specifikációja hosszúság adja meg az első és a szélesség második. Például más megfeleltetés alkalmazásokban hosszúság és a szélesség többszöröseire és értelmez fok jelöli. Hosszúság értékek vannak mérni a szélességi és-180 és 180.0 fok, és a szélesség között értékek vannak az Egyenlítő mért és közötti-90.0 és 90.0 fok. 
>
> DocumentDB értelmezi koordinátáit, ahogy a WGS-84 hivatkozási rendszer per. Kérjük, tanulmányozza az alábbi információkat koordinálása hivatkozás rendszerek olvashat bővebben.

Ez lehet beágyazni DocumentDB dokumentumban hely adatokat tartalmazó a felhasználói profil ebben a példában látható módon:

**Profil használata a DocumentDB tárolási helye**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Nemcsak a pontok GeoJSON is támogatja a LineStrings és sokszög. A terület és a vonal szegmensek ezeket bejelentkezzen két vagy több pont sorozata **LineStrings** jelenítik meg. Az eszközzel térinformatikai adatokat vizsgálhat a linestrings gyakran használt autópályákat vagy folyókat képviselik. **Sokszög** rajzolása az űrlapok egy zárt LineString csatlakoztatott pontok szegélyére. Sokszög gyakran használják például tavakat természetes alakzatokba vagy politikai állam, például a város és állapotok ábrázolásához. Íme egy példa a DocumentDB sokszög. 

**A DocumentDB sokszög**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] A GeoJSON specifikációja igényel, hogy érvényes sokszög, a megadott utolsó koordinálása pár kell ugyanaz, mint az első zárt alakzat létrehozása céljából.
>
>Sokszög rajzolása belül pontok meg kell adni az óramutató járásával megegyező sorrendet. Az óramutató járásával megegyező sorrendet megadott sokszög rajzolása a területéhez inverze jelöli.

Nemcsak a pont, LineString és sokszög GeoJSON is itt adhatja meg a jelölésének hogyan több földrajzi helyek csoportba, valamint hogy miként társítható tetszőleges tulajdonságok feloldás földrajzi helye a **szolgáltatás**. Mivel az objektumok érvényes JSON, is az összes tárolt és DocumentDB feldolgozása. Azonban DocumentDB csak akkor támogatja pontok automatikus indexelés.

### <a name="coordinate-reference-systems"></a>Hivatkozás rendszerek koordinálása

Mivel az alakzatot, a földet szabálytalan, eszközzel térinformatikai adatokat vizsgálhat koordinátáit rendszerekben sok koordinálása hivatkozás (CRS), a saját határoló keret és a mértékegységek jeleníti meg. Ha például a "nemzeti rács a Britannia" az egy hivatkozás rendszer nagyon pontos az Egyesült Királyság, de nem kívülre. 

Használja a leggyakrabban használt CRS ma a világ geodéziai rendszer [WGS-84](http://earth-info.nga.mil/GandG/wgs84/). GPS eszközök és sok megfeleltetés szolgáltatások beleértve a Google térkép és a Bing Maps API-k használata a WGS-84. DocumentDB támogatja az indexelés és a lekérdezés használata csak a WGS-84 CRS földrajzi adatok. 

## <a name="creating-documents-with-spatial-data"></a>Térbeli adatokat tartalmazó dokumentumok létrehozása
GeoJSON értékeket tartalmazó dokumentumok létrehozásakor azok automatikusan indexelve vannak a térbeli indexű az indexelési házirendre a gyűjtemény megfelelően. Ha egy DocumentDB SDK például Python vagy Node.js dinamikusan beírt nyelven dolgozik, létre kell hoznia érvényes GeoJSON.

**Node.js az eszközzel térinformatikai adatokat tartalmazó dokumentum létrehozása**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

A .NET (vagy Java) dolgozik SDK, ha a Microsoft.Azure.Documents.Spatial névtér belül új pont és sokszög osztályok helyére vonatkozó adatok beágyazása a alkalmazás objektumok belül is használhatja. Ezek az osztályok könnyítik meg a szerializálási és térbeli adatok deszerializálás GeoJSON be.

**.NET az eszközzel térinformatikai adatokat tartalmazó dokumentum létrehozása**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Ha nincs telepítve a szélesség és hosszúság adatokat, de rendelkezik a fizikai címek vagy a hely neve, például város vagy ország, például a Bing Maps többi szolgáltatások geokódolási szolgáltatás használatával megjeleníthetők a tényleges koordinátákat. További tudnivalók a Bing Maps geokódolási [ide](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Térbeli típusú lekérdezése

Most, hogy azt már-ablak jelent meg, hogyan szúrhat be az eszközzel térinformatikai adatokat vizsgálhat nézzük meg vele, hogyan ezeket az adatokat az SQL- és LINQ DocumentDB lekérdezéséhez.

### <a name="spatial-sql-built-in-functions"></a>Térbeli beépített SQL-függvényekkel
DocumentDB támogatja a következő nyissa meg a földrajzi Consortium (OGC) a beépített függvények térinformatikai lekérdezésére. További információt a beépített függvények az SQL nyelv teljes készlete olvassa el a [Lekérdezés DocumentDB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Használat</strong></td>
  <td><strong>Leírás</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>A két GeoJSON pont kifejezések közé eső a távolság.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Adja eredményül, jelezve, hogy a GeoJSON pont, az első argumentumban megadott érték belül a GeoJSON sokszög a második argumentumban szereplő logikai kifejezés.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Jelzi, hogy a megadott GeoJSON pont, illetve sokszög kifejezés érvényes logikai értéket ad eredményül.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>JSON érték logikai érték tartalmazó megadott GeoJSON pont, illetve a sokszög kifejezés érvényes, és ha érvénytelen értéket adja eredményül, ezenkívül egy karakterláncértéket okát.</td>
</tr>
</table>

Térbeli függvények közelség ellen térbeli adatok lekérdezésére használható. Ha például az alábbiakban az összes családi dokumentumokat, amelyek a megadott helyen, a ST_DISTANCE beépített függvénnyel 30 km belül visszaadó lekérdezés. 

**Lekérdezés**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Eredmény**

    [{
      "id": "WakefieldFamily"
    }]

Ha az indexelési házirend térbeli indexelés, majd "távolság lekérdezések" szolgáltató hatékony az index keresztül. A térbeli indexelés, kérjük, lásd: az alábbi szakaszt. Ha még nincs telepítve a megadott elérési útja térbeli index létrehozása, akkor is elvégezhetők térbeli lekérdezések megadásával `x-ms-documentdb-query-enable-scan` kérelem fejlécben értéke "igaz". A .NET ez történik a választható **FeedOptions** argumentum [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) igaz értékre kell állítani a lekérdezések megkerülhetők. 

Ha valamit esik sokszög ellenőrzése ST_WITHIN használható. Sokszög gyakran az irányítószámokat, állapot határai és természetes alakzatokba határai jelölésére használhatók. Ismét Ha térbeli indexelés az indexelési házirendet, majd "belül" lekérdezések szolgáltató hatékony az index keresztül. 

ST_WITHIN sokszög argumentumok csak csengetés is tartalmazhat, azaz a sokszög holes bennük nem tartalmazhat. 

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Eredmény**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Hogyan egyező típusú működik, de DocumentDB lekérdezés, ha a hely értéket vagy argumentum a hibás vagy érvénytelen, akkor lesz **definiálva** és a lekérdezés eredményéből kihagyja kiértékelt dokumentum kiértékelésének eredménye a megadott hasonló. Ha a lekérdezés eredménye nem tartalmaz, futtassa a ST_ISVALIDDETAILED szeretné hibakeresése miért spatail típusa érvénytelen.     

DocumentDB is támogat inverz lekérdezések elvégzéséhez, azaz sokszög vagy sorok DocumentDB a tárgymutató, majd a lekérdezés az egy adott pontot tartalmazó funkcióival kapcsolatos. A minta általában arra használják a logisztika azonosítása, például amikor egy gördülő belépett vagy a kijelölt terület elhagyja. 

**Lekérdezés**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Eredmény**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

Annak ellenőrzése, ha a térbeli objektum érvényes ST_ISVALID és ST_ISVALIDDETAILED használható. Az alábbi lekérdezés ellenőrzi, például a "Házon kívül" tartomány szélesség értékét (-132.8) pont érvényességét. ST_ISVALID olyan logikai értéket adja eredményül, és ST_ISVALIDDETAILED ad eredményül, logikai és az OK, miért célszerű érvénytelen tartalmazó karakterlánc.

**Lekérdezés**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Eredmény**

    [{
      "$1": false
    }]

Az előbbi függvényekkel is használható sokszög érvényesítéséhez. Ha például az alábbi használjuk ST_ISVALIDDETAILED érvényesítésére nem bezárt sokszög rajzolása. 

**Lekérdezés**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Eredmény**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>A .NET SDK lekérdezése LINQ

A DocumentDB .NET SDK is a szolgáltatók helyettes módszerek `Distance()` és `Within()` belüli LINQ kifejezések használatra. A DocumentDB LINQ szolgáltató az SQL-beépített függvény egyenértékű hívások módszer hívásokat megfelelője (ST_DISTANCE és ST_WITHIN beírhat). 

Íme egy példa LINQ lekérdezés által talált összes dokumentumot a DocumentDB webhelycsoport, amelyeknek "hely" érték van 30 kilométer, a megadott sugarú pont LINQ használatával.

**A Distance LINQ lekérdezése**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Hasonlóképpen az alábbiakban lekérdezés megkeresésére, amelynek "hely" esik a megadott mezőben sokszög dokumentumokat. 

**LINQ belül a lekérdezést.**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Most, hogy azt már-ablak jelent meg vele, hogyan LINQ és SQL használata dokumentumok lekérdezni nézzük egy DocumentDB térbeli indexelés konfigurálása.

## <a name="indexing"></a>Az indexelés

Ahogy azt a [Séma Azure DocumentDB indexelés felismerése nélkül](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) nyomtatja a, azt tervezett DocumentDB's adatbázismotort kell valóban séma felismerése nélkül, és adja meg a JSON első osztályú támogatása. Az írás optimalizált adatbázismotor DocumentDB a natív módon értelmez a GeoJSON szabványban ábrázolt térbeli adat (pontok, sokszög és vonalak).

A mértani rövid geodéziai koordináták 2D sík alakzatot a tervezett akkor fokozatosan osztani egy **quadtree**cellák. Ezek a cellák **Hilbert terület Kitöltés gomb**, amely megőrzi a pontok településen belül a cella helyét alapján 1N vannak megfeleltetve. Ezenkívül akkor, amikor helyadatok indexelt, magától **csempézés**neve folyamat, tehát minden a cellákat, amelyek egy helyen metszete azonosítva és tárolni a DocumentDB tárgymutató kulcsként. Lekérdezés időben argumentumot, például a pontok és sokszög is ki kell olvasni a megfelelő azonosító cellatartomány Csempézett, majd az indexből adatok beolvasásához használt.

Ha, adja meg az indexelési házirend térbeli index tartalmazó / * (minden elérési út), majd a webhelycsoporton belüli található összes pont indexelve vannak hatékony térbeli lekérdezések (ST_WITHIN és ST_DISTANCE). Térbeli indexek nem pontosság értéke, és mindig egy alapértelmezett pontosság értéket.

>[AZURE.NOTE] DocumentDB támogatja a pontok, sokszög és LineStrings Automatikus indexelés

A következő JSON kódtöredékének jeleníti meg az indexelési házirend térbeli indexelés engedélyezve, azaz tárgymutató-dokumentumokból térbeli lekérdezése található GeoJSON bármikor. Az indexelési házirendet, az Azure-portálon módosításakor a következő JSON indexelési házirend ahhoz, hogy az indexelés a webhelycsoport térbeli is megadhat.

**A webhelycsoport indexelés házirend JSON-ban a engedélyezve van, pontok és sokszög Spatial**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Íme egy kódtöredéket .NET, amely bemutatja, hogyan hozzon létre egy webhelycsoport térbeli indexelés engedélyezett a pontok tartalmazó minden elérési út. 

**Hozzon létre egy webhelycsoport térbeli indexelés**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

És Íme, hogyan módosíthatja egy meglévő webhelycsoport térbeli indexelés felülírja a dokumentumokból tárolt pontokat előnyeit.

**Egy meglévő webhelycsoport és térbeli indexelés módosítása**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Ha hibás vagy érvénytelen GeoJSON érték a dokumentumon belül helyét, majd azt nem első indexelve térbeli lekérdezésére. Ellenőrizheti, hogy ST_ISVALID és ST_ISVALIDDETAILED hely értékeket.
>
> Webhelycsoport definíciójának partíciót kulcs tartalmaz, nem jelenteni indexelés átalakítása előrehaladását. 

## <a name="next-steps"></a>Következő lépések
Most, hogy Ön már értesült arról, hogy miként térinformatikai támogatási DocumentDB az első lépések, a következőkre van lehetősége:

- Indítsa el a kódolása a [térinformatikai .NET mintakódok a Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
- Pálcikák kapcsolatba térinformatikai a [DocumentDB lekérdezés játszótéri](http://www.documentdb.com/sql/demo#geospatial) a lekérdezés
- További tudnivalók a [DocumentDB lekérdezés](documentdb-sql-query.md)
- További tudnivalók a [DocumentDB indexelés házirendek](documentdb-indexing-policies.md)
