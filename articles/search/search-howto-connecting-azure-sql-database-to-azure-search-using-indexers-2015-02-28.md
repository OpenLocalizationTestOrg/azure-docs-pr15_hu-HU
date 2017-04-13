<properties 
    pageTitle="Azure SQL-adatbázis csatlakoztatása az indexelő használatával Azure keresési |} Microsoft Azure |} Indexelő" 
    description="Útmutató: adatok lekérése az Azure SQL-adatbázis használata az indexelő Azure keresési indexhez." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure SQL-adatbázis csatlakoztatása az indexelő használatával Azure-keresés

Azure keresési szolgáltatása, amely megkönnyíti a kiváló keresési élményt biztosít a felhőben tárolt keresési szolgáltatásának. Mielőtt is kereshet, kell az adatok Azure keresési index feltöltéséhez. Ha az adatok él Azure SQL-adatbázisban, az új **Azure keresési indexelő Azure SQL-adatbázis** (vagy **Azure SQL-indexelő** rövid) Azure keresés automatizálhatja az indexelési folyamatot. Ez azt jelenti, hogy kevesebb-kódot az írja be, és kevesebb infrastruktúrát érdeklő van.

Jelenleg indexelő csak dolgozhat Azure SQL-adatbázissal, SQL Server Azure VMs és [Azure DocumentDB](../documentdb/documentdb-search-indexer.md). Ez a cikk azt fogja koncentráljon indexelő, amely az Azure SQL-adatbázis használata. Ha azt szeretné, hogy további adatforrások támogatása, adjon meg visszajelzését a [Azure keresési Visszajelzési fórum](https://feedback.azure.com/forums/263029-azure-search/).

Ez a cikk az indexelő használatával idejéről fogják ellátni, de azt fogja is végezhet részletezést funkciók és viselkedések, hogy csak az SQL-adatbázisok (például integrált a változások követésének) találhatók.

## <a name="indexers-and-data-sources"></a>Adatok és az indexelő adatforrások

Állíthatja be, és állítsa be az Azure SQL-indexkiszolgáló, hívja fel az [Azure keresési REST API -t](http://go.microsoft.com/fwlink/p/?LinkID=528173) az **Indexelő** és az **adatforrás**létrehozása és kezelése. 

Az adatok importálása varázsló vagy [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)az [Indexelő osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) az [Azure portál](https://portal.azure.com) létrehozása és ütemezése indexkiszolgáló is használhatja.

Egy **adatforráshoz** megadja indexelést állít be, az adatok és a házirendek az Azure-keresés (új, módosítás vagy törölt sorok) az adatok változásai hatékony azonosítása engedélyezése eléréséhez szükséges hitelesítő adatokat. Egy független erőforrás definíciója, így több indexelő használható.

Egy **indexelési** , amelyek az adatforrásokhoz kapcsolódnak cél keresési indexeket az erőforrás. Indexkiszolgáló alapul, az alábbi módon:
 
- Végezze el az adatok kitöltéséhez az index egyszeri másolatát.
- Tárgymutató frissítése az ütemezés az adatforrásban végrehajtott módosítások.
- Futtassa az igény szerinti szükség szerint a tárgymutató frissíthető. 

## <a name="when-to-use-azure-sql-indexer"></a>Mikor érdemes használni az SQL Azure-indexelő

Attól függően, hogy az adatok vonatkozó számos tényező az Azure SQL-indexelő előfordulhat, hogy vagy nem feltétlenül szükséges. Ha az adatok elfér az alábbi követelményeket, az SQL Azure-indexelő használható: 

- Az összes egyetlen táblák és nézetek származnak az adatok
    - Ha több táblát van tárolja az adatokat, hozzon létre egy nézetet, és nézet használata az indexelő. Vigyázzon, hogy egy nézet használata esetén nem használhatja az SQL Server integrált módosítása észlelési. Ez a szakasz további részleteket lásd. 
- Az adatforrás használt adattípusok az indexelő által támogatott. A legtöbb, de nem az összes az SQL támogatott adattípusok. Részletekért olvassa el [az Azure keresési adattípusok hozzárendelése](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Nem szükséges közelében az index egy sor megváltozásakor valós idejű frissítéseit. 
    - Az indexelő is új indexet a tábla legfeljebb 5 percenként. Ha az adatok gyakran változnak, és a módosítások kell másodpercben vagy percben egyetlen belül az index megjelenjenek, közvetlenül a [Azure keresési Index API](https://msdn.microsoft.com/library/azure/dn798930.aspx) használata ajánlott. 
- Ha nagy mennyiségű adathalmaz és tervezi, hogy az indexelő időközönként, a séma lehetővé teszi, hogy hatékony azonosítása us Futtatás megváltozott (és töröl, ha van ilyen) sort. További részletekért olvassa el "a rögzítés megváltozott és törölt" alatti sorokban. 
- A sor az indexelt mezőkben mérete nem haladja meg a kérelmet, amelyet 16 MB indexelés Azure keresés maximális méretét. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Hozzon létre, és az SQL Azure-indexkiszolgáló

Első lépésként az adatforrás létrehozása: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


A kapcsolati karakterlánc kinyerése az [Azure klasszikus portál](https://portal.azure.com); használja a `ADO.NET connection string` lehetőséget.

Ezután hozzon létre a cél Azure keresési index, ha nincs telepítve egyik már. Ehhez a [portálon felhasználói felület](https://portal.azure.com) vagy a [Tárgymutató-API létrehozása](https://msdn.microsoft.com/library/azure/dn798941.aspx).  Győződjön meg arról, hogy a célhely index sémájának a forrástáblához sémájának kompatibilis – lásd: az [SQL Azure és a keresési adattípusok közötti hozzárendelés](#TypeMapping) további információt.

Végezetül hozhat létre az indexelő adjon meg nevet, és az adatok forrás- és célwebhelyek index hivatkozó:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Az így létrehozott indexkiszolgáló ütemezett nem tartozik. Automatikusan fusson egyszer, amint létrehozása. Futtathatja bármikor újra **futtassa az indexelő** kérelmének használata:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Testre szabhatja a számos tulajdonságát indexelő viselkedését, például a köteg méretét és a hány dokumentumokat is kihagyja, mielőtt az indexelő végrehajtás sikertelen lesz. További információra kíváncsi olvassa el a [Indexelő API létrehozása](https://msdn.microsoft.com/library/azure/dn946899.aspx)című témakört.
 
Előfordulhat, hogy engedélyeznie kell az adatbázishoz való csatlakozáshoz Azure szolgáltatások. [Csatlakozás az Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) megtennie útmutatást talál.

Figyelheti az indexelési állapot és a végrehajtás előzmények (indexelt elemeinek száma, hibák, stb.), használja **az indexelő állapotának** kérelmet: 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

A válasz a következőhöz hasonlóan kell kinéznie: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Végrehajtási előzmények a nemrég befejeződött, vannak rendezve az, fordított időrendben (úgy, hogy a legújabb végrehajtás véget előbb, a válasz) végrehajtások legfeljebb 50 tartalmazza. További információ a válasz található [Indexelési állapot beolvasása](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Futtassa az indexelő időközönként

Az indexelő futtatásához rendszeres időközönként is rendezheti. Ehhez hozzáadása a az **Ütemezés** tulajdonság létrehozásakor vagy frissítésekor az indexelő. Az alábbi példában látható helyezése kérelmének az indexelő frissítése:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Az **intervallum** paraméter szükség. Két egymást követő indexelő végrehajtások kezdésének között eltelt idő az intervallum hivatkozik. Megengedett legkisebb intervallum 5 perccel; egy nappal leghosszabb. Akkor XSD "dayTimeDuration" értelmezi (korlátozott részhalmazát az [ISO 8601 időtartam](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) érték) lesz formázva. Ezt a minta: `P(nD)(T(nH)(nM))`. Példa: `PT15M` a 15 percenként `PT2H` minden 2 órán keresztül.

A választható **Kezdő időpont** azt jelzi, ha az ütemezett végrehajtások kell megkezdeni; Ha nem adja meg, az aktuális Egyezményes világidő lesz. Ez esetben a múltbeli –, amelyben esetben az első végrehajtása lesz ütemezve, mintha az indexelő futása folyamatosan óta a kezdő időpont lehet.  

Egy adott indexelő csak egy végrehajtását egyszerre futtatható. Indexkiszolgáló már fut, amikor a következő dokumentumot értendő indítása, ha a végrehajtás kimarad, és önéletrajzok, a következő időközönként, feltéve, hogy a többi indexelő feladat nem működik.

Nézzünk meg, hogy ez legyen több konkrét példát. Tegyük fel, hogy az alábbi óránkénti ütemezés beállítva: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Az alábbi történik: 

1. Első indexelő végrehajtása elindítja a vagy körül 2015 március 1 12:00 de. UTC.
1. Tegyük fel, a végrehajtás megnyitja 20 percet (vagy bármikor kisebb, mint 1 óra).
1. Második végrehajtása elindítja a vagy körül 2015 március 1 1:00 de. 
1. Most már feltételezve, hogy a végrehajtás megnyitja-e több, mint egy óra (ehhez van szükség a nagyon nagy számú erre valójában fordulhat elő, a dokumentumok, de egy hasznos ábra) – például 70 perc –, hogy körülbelül 2:10 óra befejezése
1. Célszerű most 2:00 de., indításához harmadik végrehajtásához szükséges időt. Jó helyen jár mert az 1 óra második végrehajtása továbbra is fut, a harmadik végrehajtás kimarad. A harmadik végrehajtás 3 délelőtt kezdődik.

Hozzáadása, módosítása vagy törlése a meglévő indexkiszolgáló ütemtervét **HELYEZI az indexelő** kérelmének használatával. 

## <a name="capturing-new-changed-and-deleted-rows"></a>Új, rögzítésével megváltozott és sorok törlése

Ha ütemezett használ, és a táblázat a sorok nem trivial számot tartalmaz, egy módosítása észlelési házirendet, hogy az indexelő hatékony meghallgathatja csak az új vagy megváltozott sorokat anélkül, hogy a teljes táblázat az új indexet kell használni.

### <a name="sql-integrated-change-tracking-policy"></a>SQL integrált nyomon követése a házirend módosítása

Ha az SQL-adatbázis támogatja a [változások követése](https://msdn.microsoft.com/library/bb933875.aspx), **SQL integrált módosítása követés házirend**használata ajánlott. Ezzel a házirenddel lehetővé teszi, hogy a lehető leghatékonyabb a változások követésének, és lehetővé teszi Azure keresési azonosítása törölt sorok nélkül explicit "finom törlése" oszlop elhelyezése a táblázatban.

Integrált a változások követésének kezdve az alábbi SQL Server-adatbázis verziók támogatott műveletek:
 
- SQL Server 2008 R2 vagy újabb verzió, ha az SQL Server Azure VMs használ. 
- Azure SQL adatbázis V12, Azure SQL-adatbázis használata.

Amikor használata SQL integrált a változások követésének házirendet, nem ad meg egy külön adatok törlési észlelési házirend - e házirendje rendelkezik beépített támogatása azonosító törölt sorok.

Ezzel a házirenddel csak akkor használható táblázatokkal; a nézetek nem használhatók. Szükséges ahhoz, hogy a változások követésének a táblázatot, mielőtt használatba vehetné a házirend használ. [Engedélyezése és letiltása a változások követésének](https://msdn.microsoft.com/library/bb964713.aspx) talál utasításokat. 

Ezzel a házirenddel, hozzon létre vagy jelennek meg az adatforrás frissítése:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

### <a name="high-water-mark-change-detection-policy"></a>Nagy víz megjelölés módosítása észlelési házirend

Az SQL integrált változások követése házirend ajánlott, amíg nem használhatja, ha az adatok egy nézetben van, vagy az Azure SQL-adatbázis egy régebbi verziójának használata. Ebben az esetben fontolja meg inkább a Max víz megjelölés házirend. Ezzel a házirenddel is használható, ha a táblázat, amely megfelel az alábbi feltételek oszlopot tartalmaz:

- Az összes szúr be az oszlop értékének meghatározása. 
- Egy elem összes frissítését is módosíthatja az oszlop értékét. 
- Az e oszlop értéke az egyes módosítások megnöveli.
- Lekérdezi használó egy `WHERE` hasonló záradék `WHERE [High Water Mark Column] > [Current High Water Mark Value]` hatékony futtatható.

Például az indexelt **rowversion** oszlop egy ideális jelölt a nagy víz megjelölés oszlop. Ezzel a házirenddel, hozzon létre vagy jelennek meg az adatforrás frissítése: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Lágy törlése oszlop törlése észlelési házirend

Amikor sorokat a forrástáblához törlődnek, valószínűleg kívánt azok a sorok törlése, valamint a keresési indexhez. Az integrált SQL-változáskövetési házirend használata esetén ez van készített kezeli meg. Azonban a nagy víz megjelölés változáskövetési házirend nem segíthessen Önnek a törölt sorokat. Mi a teendő? 

A sorok fizikailag törlődnek az értékeket, ha kívül számolva is – a nincs mód előállítani a rekordok, amelyek már nem szerepelnek a jelenléti adatok alapján.  Azonban szeretne megjelölni, amelyekkel logikailag törölt sorok anélkül, hogy eltávolítaná azokat az értékeket oszlop hozzáadásával, és a sorok megjelölése törölt jelölő érték használata az adott oszlopban a "finom + delete" módszerrel is használhatja.

A lágy törlési technika használata esetén adja meg a finom házirend törlése az alábbi képlettel történik létrehozásakor, illetve az adatforrás frissítése: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Figyelje meg, hogy a **softDeleteMarkerValue** karakterlánc – kell lennie a tényleges érték karakterlánc ábrázolása használja. Ha például használja a Ha az egész oszlopokra, hol jelöli törölt sorok értéke 1, `"1"`; Ha telepítve van az adott törölt sorok jelöli az IGAZ logikai értéket BIT oszlop, `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>SQL-adattípusok és Azure keresési adattípusok közötti megfeleltetés

|SQL-adattípus | Engedélyezett cél index mezőtípusok |Jegyzetek 
|------|-----|----|
|bit|Edm.Boolean, Edm.String| |
|int és smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| bigint | Edm.Int64, Edm.String | |
| valós, lebegtetés |Edm.Double, Edm.String | |
| megjelenítésekor, a rendszer pénzt decimális szám | Edm.String| Azure keresés nem támogatja a decimális típusú átalakítása Edm.Double, mert ez elveszít pontosság |
| karakter, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Egy karakterlánc oszlop átalakítása Collection(Edm.String) be van szükség egy minta API verzió 2015-02-28 – előzetes verzió használatával. [Ez a cikk](search-api-indexers-2015-02-28-preview.md#CreateIndexer) további információ| 
|smalldatetime, a dátum és idő, a datetime2, a dátum, a datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|földrajzi hely | Edm.GeographyPoint | Támogatott típusú SRID 4326 (Ez az alapértelmezett), csak földrajzi példányok | | 
|ROWVERSION| A #HIÁNYZIK |Sor verziójú oszlopok nem tárolhatók a keresési index, de is használható a változások követése | |
| idő, időszak, bináris, varbinary, kép, xml, mértani, CLR típusai | A #HIÁNYZIK |Nem támogatott |


## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Q:** Használhatom-e indexelő Azure SQL Azure-ban IaaS VMs futó SQL-adatbázisait?

V: Igen. Jó helyen jár engedélyeznie kell a keresési szolgáltatás adatbázishoz való csatlakozáshoz a hajtsa végre az alábbi két dolog. Tanulmányozza [az SQL Server-Azure virtuális Azure keresési indexkiszolgáló kapcsolat beállítása](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) a cikk további információt.

1. Előfordulhat, hogy az adatbázis konfigurálása a egy megbízható tanúsítvánnyal, hogy a keresési szolgáltatás megnyithatja az adatbázis SSL-kapcsolatot.
2. Állítsa be a tűzfalat, hogy engedélyezze a keresési szolgáltatás IP-címének a hozzáférést.

**Q:** Használhatom-e az SQL Azure-indexelő futó helyszíni SQL-adatbázisait? 

V: nem javasoljuk, és nem támogatja, a ezzel lenne szüksége nyissa meg az adatbázisok az internetes forgalmat. 

**Q:** Használhatom-e az SQL Azure-indexelő eltérő SQL Server Azure IaaS a forgalmi adatbázisokhoz? 

A: Ebben az esetben nem támogatott diagramtípusról, mert az SQL Server-től különböző bármely adatbázisokkal az indexelő még nem teszteltük.  

**Q:** Létrehozhatok-e több indexelő időközönként fut? 

V: Igen. Jó helyen jár csak egy indexelő futhat egy csomóponton egy időben. Ha több indexelő párhuzamosan fut, fontolja meg a keresési szolgáltatás egynél több keresési egységre méretezés. 

**Q:** Indexkiszolgáló futó befolyásolja a lekérdezés terhelést? 

V: Igen. Indexelő a keresési szolgáltatás csomópontját egyik fut, és a csomópont erőforrások megosztott az indexelés és a lekérdezést adatforgalom és más API kérelmek között. Ha intenzív az indexelés és a lekérdezés munkaterhelésekből és 503 hibák vagy növekvő válaszidő nagy mértékű találkozik, fontolja meg a méretezés a keresési szolgáltatás be.
