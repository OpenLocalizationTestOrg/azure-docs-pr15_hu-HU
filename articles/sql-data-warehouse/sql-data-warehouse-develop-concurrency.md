<properties
   pageTitle="Feldolgozási és a terhelést-kezelés az SQL adatraktár |} Microsoft Azure"
   description="Feldolgozási és a terhelést-kezelés az Azure SQL-adatraktár megoldások fejlesztésére ismertetése."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Az SQL adatraktár feldolgozási és a terhelést kezelése

A Microsoft Azure SQL-adatraktár segítséget nyújt a méretarány kiszámíthatóan teljesítményt nyújtsa szabályozhatja a feldolgozási szintek és erőforrás-hozzárendelések, például a memória és Processzor rangsor. Ez a cikk bemutatja a párhuzamos és terhelést-kezelés mailbe arról, hogy hogyan mindkettőt alkalmaznak, és hogyan szabályozhatja, hogy azok a adatraktár az alapfogalmakkal. Több felhasználó környezetekben támogatást SQL adatraktár terhelést kezelése szolgál. Több elem bérlői munkaterhelésekből van nem készült.

## <a name="concurrency-limits"></a>Feldolgozási korlátai

SQL-adatraktár lehetővé teszi, hogy legfeljebb 1024 egyidejű kapcsolatok. 1024 a minden kapcsolat egyidejűleg küldhet lekérdezések. Azonban átviteli optimalizálása, SQL adatraktár előfordulhat, hogy várólista bizonyos lekérdezések annak érdekében, hogy minden lekérdezés megkapja a minimális memória támogatás. Lekérdezés-végrehajtási időben Queuing fordul elő. Várólista lekérdezések feldolgozási korlátai elérésekor SQL adatraktár növelésével teljes átviteli azáltal, hogy aktív lekérdezések férhet hozzá a kritikus szükséges memória erőforrásokat.  

Két fogalmak feldolgozási korlátozások vonatkoznak: *egyidejű lekérdezések* és *feldolgozási helyek*. Hajtsa végre a lekérdezést akkor végre kell hajtania a lekérdezés feldolgozási korlát és a párhuzamos tárolóhely felosztás is belül.

- Egyidejű lekérdezések a lekérdezések végrehajtása egy időben futnak. SQL-adatraktár legfeljebb 32 egyidejű lekérdezések DWU nagyobb méretű használatát támogatja.
- Feldolgozási helyek van rendelve DWU alapján. Minden egyes 100 DWU 4 feldolgozási helyek biztosít. Egy DW100 lefoglalja 4 feldolgozási helyek és a DW1000 40 osztja ki. Minden lekérdezés fogyaszt egy vagy több feldolgozási helyek, az [erőforrás osztály](#resource-classes) a lekérdezés függ. Lekérdezések futtatása a smallrc erőforrás osztály egy feldolgozási tárolóhely mobilalkalmazásokban Lekérdezések futtatása egy újabb erőforrás osztály további feldolgozási helyek mobilalkalmazásokban

Az alábbi táblázat ismerteti a korlátozások egyidejű lekérdezések és a párhuzamos helyek a különböző DWU méretben.

### <a name="concurrency-limits"></a>Feldolgozási korlátai

|  DWU   | Max egyidejű lekérdezések  | Felosztott feldolgozási helyek |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120-ra            |
| DW6000 |           32            |              240            |

Ha ezek küszöbértékek egyike teljesül, az új lekérdezésekre aszinkron és alapon első a kiküldési hajtja végre.  A lekérdezések befejeződött, és a korlátok alá esik lekérdezések és helyek számát, várólistás lekérdezések kiadott. 

> [AZURE.NOTE]  *Jelölje be* a kizárólag a dinamikus kezelése (DMVs) és katalógus nézetek végrehajtása lekérdezések nem vonatkoznak a feldolgozási korlátai közül. Figyelheti a rendszer, függetlenül attól, rajta végrehajtása lekérdezések száma.

## <a name="resource-classes"></a>Erőforrás-osztályok

Erőforrás osztályok memóriafoglalást szabályozhatja a lekérdezéshez adott Processzor ciklus segítséget. Négy erőforrás osztályok rendelhet a felhasználó *adatbázis szerepkörök*formájában. A négy erőforrás osztályok **smallrc**, **mediumrc**, **largerc**és **xlargerc**. A felhasználók smallrc a memória kisebb összegű kapnak, és magasabb feldolgozási kihasználhatja. Viszont xlargerc rendelt felhasználók kapnak nagy mennyiségű memóriát, és ezért kevesebb lekérdezéseket futtatják a futhat.

Alapértelmezés szerint minden felhasználó tagja a kis erőforrás osztály, smallrc. Az eljárás `sp_addrolemember` segítségével növelje az erőforrás osztály és `sp_droprolemember` csökkentheti az erőforrás osztály szolgál. Ez a parancs például loaduser largerc az erőforrás osztálya szeretné növelni:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Javasolt véglegesen felhasználók hozzárendelése az erőforrás osztályaik módosítása helyett egy erőforrás osztály. Például csoportosított columnstore táblák terhelések létrehozása jobb minőségű indexek, ha több memóriát kiosztva. Gondoskodhat arról, hogy terhelések magasabb memória, hozzon létre egy felhasználót kifejezetten az adatok betöltése, és a felhasználó végleges hozzárendelése egy újabb erőforrás osztály.

Létezik néhány lekérdezésre, amely nem egy nagyobb memóriafoglalást tartoznak. A rendszer figyelmen kívül hagyja a az erőforrás osztály-tárhelyüket, és a mindig a lekérdezések futtatása a kis erőforrás osztály helyette. Ha ezeket a lekérdezéseket mindig futtatja a kisméretű erőforrás osztály, amikor feldolgozási helyek nyomása csoportban, és nem szükséges, mint további helyek igényelnek futtathatnak. További információt talál [az erőforrás osztály kivételek](#query-exceptions-to-concurrency-limits) .

Néhány további részletek az erőforrás osztály:

- Módosíthatja egy felhasználó az erőforrás osztály *szerepkör megváltoztathatja* engedély szükséges.  
- Bár a felhasználó felvétele a magasabb erőforrás osztályok közül egy vagy több, a felhasználók a legmagasabb erőforrás osztály, amelyhez hozzá vannak rendelve attribútumait tart. Ez azt jelenti, hogy a felhasználó mediumrc és a largerc van rendelve, az újabb erőforrás osztály (largerc) esetén az erőforrás osztály, hogy a program kell fogadja.  
- Az erőforrás osztály a rendszer felügyeleti felhasználó nem módosíthatók.

A részletes példa című témakörben [módosítása felhasználói erőforrás osztály példa](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Memóriafoglalást

Vannak előnyei és hátrányai növelése egy felhasználói erőforrás osztály. Növelése egy erőforrás osztály a felhasználó lesz a lekérdezések hozzáférési engedély több memóriát, jelentheti a lekérdezések gyorsabb végrehajtása.  Magasabb erőforrás osztályok is egyidejű futtathatók lekérdezések száma csökkentheti. Ez az egyetlen lekérdezés memória nagy mennyiségű lefoglalása vagy más lekérdezések is szükség van a memória-hozzárendelések egyidejű futtatásának engedélyezése között a csökkentés. Ha egy felhasználónak van egy lekérdezés memória magas hozzárendelések, más felhasználók nem hozzáférést kap, hogy a lekérdezés futtatásához azonos memóriát.

Az alábbi táblázat az egyes terjesztési rendelt DWU és erőforrás-osztály memória rendeli.

### <a name="memory-allocations-per-distribution-mb"></a>A memória-hozzárendelések egy terjesztési (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1,600   |
| DW500  |   100   |    400   |    800  |  1,600   |
| DW600  |   100   |    400   |    800  |  1,600   |
| DW1000 |   100   |    800   |  1,600  |  3,200   |
| DW1200 |   100   |    800   |  1,600  |  3,200   |
| DW1500 |   100   |    800   |  1,600  |  3,200   |
| DW2000 |   100   |  1,600   |  3,200  |  6,400   |
| DW3000 |   100   |  1,600   |  3,200  |  6,400   |
| DW6000 |   100   |  3,200   |  6,400  |  12,800  |

Az előző táblából láthatja, hogy egy erőforrás xlargerc osztály DW2000 futó lekérdezést szeretné, hogy 6400 MB a belül 60 megosztott adatbázis elérését.  Az SQL-adatraktár vannak 60 terjesztését. Emiatt a lekérdezés egy adott erőforrás osztály összes memóriafoglalást kiszámításához, a fenti értékeket kell megszorozni 60.

### <a name="memory-allocations-system-wide-gb"></a>A memória hozzárendelések rendszerbeli (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375.    |
| DW3000 |    6    |    94    |   188   |   375.    |
| DW6000 |    6    |   188    |   375.   |   750    |

A rendszer szintű memória hozzárendelések táblázatból láthatja, hogy egy erőforrás xlargerc osztály DW2000 futó lekérdezést oszlik 375 GB memóriát összesen (6400 MB * 60 terjesztését / 1024 GB konvertálása) a SQL adatraktár egészét fölé.

## <a name="concurrency-slot-consumption"></a>Feldolgozási tárolóhely felhasználás

SQL-adatraktár magasabb erőforrás osztályok futó lekérdezések több memóriát biztosít. A memória rögzített erőforrás.  Emiatt a lekérdezés projektenként több memóriát, a kevesebb egyidejű lekérdezések hajthat végre. Az alábbi táblázat az összes verzió-ellenőrzési helyek DWU áll rendelkezésre, és az elfogyasztott minden erőforrás osztály helyek számát megjelenítő egyetlen nézetben az előző fogalmak ismételten kifejezi.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Felosztás és feldolgozási helyek felhasználása

|  DWU   | Egyidejű lekérdezések maximális  | Felosztott feldolgozási helyek | A helyek smallrc által használt |  A helyek mediumrc által használt |  A helyek largerc által használt |  A helyek xlargerc által használt |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120-ra            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Ebből a táblából megtekintése, hogy SQL adatraktár futó DW1000 lefoglalja 32 egyidejű lekérdezések legfeljebb és 40 feldolgozási helyek összesen. Smallrc az összes felhasználó szolgáltatás, 32 egyidejű lekérdezések szeretné tenni, mivel minden lekérdezés 1 feldolgozási tárolóhely szeretne felhasználni. Ha egy DW1000 összes felhasználója mediumrc voltak fut, mindkét lekérdezés volna kell projektenként 800 MB eloszlás egy teljes memóriafoglalást 47 GB lekérdezése és feldolgozási korlátozódik, 5 felhasználó részére (40 feldolgozási helyek / 8 résidők mediumrc felhasználónként).

## <a name="query-importance"></a>Lekérdezés fontosság

SQL-adatraktár erőforrás osztályok terhelést csoportok használatával hajtja végre. Vannak olyan összesen nyolc terhelést csoportok, amely a különböző méretű DWU végig az erőforrás osztályok viselkedésének szabályozásához. SQL-adatraktár bármely DWU, használja a nyolc terhelést csoportok csak négy. Ez van ilyesmire lehetőség, mivel minden terhelést csoport van hozzárendelve egy erőforrás-osztályok négy: smallrc, mediumrc, largerc, vagy xlargerc. A terhelést-csoportok ismertetése fontosságát, hogy néhány terhelést csoportokhoz vannak beállítva magasabb *sürgős*. Sürgős használatos Processzor ütemezési. Lekérdezések futtatása fontosnak háromszor további Processzor ciklus, mint a közepes sürgős fog kapni. Feldolgozási tárolóhely hozzárendelések ezért is Processzor prioritás határozza meg. Lekérdezés 16 vagy több helyek fogyasztása, ha futtatja fontosként.

A következő táblázat mutatja az egyes terhelést csoportok sürgős megfeleltetések.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Terhelést csoport hozzárendelések feldolgozási helyek és fontosság

| Csoportok terhelést | Feldolgozási tárolóhely hozzárendelése | MB / ki. | Sürgős hozzárendelése |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Közepes       |
| SloDWGroupC01   |            2             |         200       |       Közepes       |
| SloDWGroupC02   |            4             |         400       |       Közepes       |
| SloDWGroupC03   |            8             |         800       |       Közepes       |
| SloDWGroupC04   |           16             |       1,600       |       Magas         |
| SloDWGroupC05   |           32             |       3,200       |       Magas         |
| SloDWGroupC06   |           64             |       6,400       |       Magas         |
| SloDWGroupC07   |          128             |      12,800       |       Magas         |

A **Felosztás és feldolgozási helyek fogyasztása** diagramról megtekintheti, hogy egy DW500 használ 1, 4, 8 vagy 16 feldolgozási helyek smallrc, mediumrc, largerc és xlargerc, illetve. Megtekintheti ezeket az értékeket a megelőző diagramon a sürgős minden erőforrás osztály kereséséhez.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Az erőforrás osztályok sürgős DW500 hozzárendelése

| Erőforrás osztály | Terhelést csoport | Feldolgozási helyek használt | MB / ki. | Sürgős |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Közepes   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Közepes   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Közepes   |
| xlargerc       | SloDWGroupC04  |          16            |        1,600      |   Magas     |


A következő DMV lekérdezés is használhatja, tekintse meg az erőforrás-terhelés memóriahasználat részletességgel elsősorban az erőforrás szemszögéből közötti különbségek, illetve elemzése a terhelést csoportok aktív és történelmi használatát, ha a hiba elhárításához.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Feldolgozási korlátai figyelembe veszi-lekérdezéseket

A legtöbb lekérdezések erőforrás osztályok irányadók. Ezek a lekérdezések mindkét egyidejű lekérdezés és a párhuzamos tárolóhely küszöbértékek kell illeszkedjen. A felhasználó nem választhatja ki, ha nem szeretné egy lekérdezés a feldolgozási tárolóhely modell.

Megújítja, hogy a következő utasítások figyelembe veszi az erőforrás osztályok:

- BESZÚRÁS – KIVÁLASZTÁSA
- FRISSÍTÉS
- TÖRLÉSE
- KIJELÖLÉSE (Ha a lekérdezés felhasználói táblák)
- ALTER INDEXET ÚJRAÉPÍTŐ
- ALTER INDEX ÁTSZERVEZ
- ALTER TÁBLÁZAT ÚJRAÉPÍTŐ
- INDEX LÉTREHOZÁSA
- CSOPORTOSÍTOTT COLUMNSTORE INDEX LÉTREHOZÁSA
- TÁBLÁZAT KIJELÖLÉSE SZERINT (CTAS) LÉTREHOZÁSA
- Adatok betöltése
- Adatok mozgását műveletének az adatok mozgását szolgáltatás (Dokumentumkezelő)

## <a name="query-exceptions-to-concurrency-limits"></a>Lekérdezés alóli kivételek feldolgozási korlátai

Bizonyos lekérdezések veszi figyelembe az erőforrás osztály, amelyek a felhasználók hozzá van rendelve. Ezek a kivételek feldolgozási határértékek történik, ha egy adott parancs szükséges memória-erőforrások sokszor alacsony, mivel a parancsot a metaadat-művelet. Ezek a kivételek célja elkerülése érdekében a nagyobb memória hozzárendelések lekérdezések soha nem kell őket. Ezekben az esetekben az alapértelmezett kis erőforrás osztály (smallrc) mindig használják a tényleges erőforrás osztály a felhasználóhoz rendelt függetlenül. Ha például `CREATE LOGIN` mindig smallrc fog futni. Ez a művelet teljesítéséhez szükséges erőforrások is nagyon kicsi, így nem értelme szeretné hozzáadni a lekérdezést a feldolgozási tárolóhely modell.  Ezek a lekérdezések is nem korlátozza a 32 felhasználói feldolgozási korlát, ezek a lekérdezések tetszőleges számú futtatását is lehetővé teszi a 1024 munkamenetek, munkamenet korlátja.

A következő utasítások veszi figyelembe az erőforrás osztályok:

- LÉTREHOZÁS és a táblázat LEVÁLASZTÁSA
- ALTER TABLE... VÁLTÁS, felosztása vagy PARTÍCIÓT EGYESÍTÉSE
- ALTER INDEX LETILTÁSA
- DROP INDEX
- LÉTREHOZÁS, frissítése vagy LEVÁLASZTÁSA statisztika
- TÁBLÁZAT CSONKOLÁSA
- ALTER ENGEDÉLY
- BEJELENTKEZÉS LÉTREHOZÁSA
- LÉTREHOZÁS, módosítás vagy DROP USER
- LÉTREHOZÁS, ALTER vagy LEVÁLASZTÁSA eljárás
- LÉTREHOZÁS vagy közvetlen NÉZET
- ÉRTÉKEK BEILLESZTÉSE
- Jelölje be a rendszer nézetek és DMVs
- MAGYARÁZÓ
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Egy felhasználó erőforrás osztály példa módosítása

1. **Létrehozása bejelentkezési:** Nyissa meg a **fő** adatbázist az SQL server kiszolgálót az adatraktár SQL-adatbázis a kapcsolatot, és a következő parancsok végrehajtása.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Érdemes felhasználó létrehozása az Azure SQL-adatraktár-felhasználóknak a fő adatbázist. A felhasználó létrehozásának minta lehetővé teszi a felhasználóknak bejelentkezési eszközeivel például SSMS nélkülire az adatbázis nevét.  Lehetővé teszi az objektum Intézővel adatbázisokra meg az SQL server őket.  Miként hozhat létre, és a felhasználók kezelése, lásd: [az SQL adatraktár adatbázis biztonságos][].

2. **SQL adatraktár létrehozása felhasználói:** Nyisson meg egy kapcsolatot a **Adatraktár SQL** -adatbázishoz, és hajtsa végre a következő parancsot.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Engedélyek megadása:** Adja meg a következő példa `CONTROL` **Adatraktár SQL** -adatbázisban. `CONTROL`az adatbázist szint: az SQL Server-alapú db_owner egyenértékű.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Erőforrás osztály növelése:** Kövesse az alábbi lekérdezés felhasználó magasabb terhelést management szerepkörbe.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Erőforrás osztály csökkentése:** Az alábbi lekérdezéssel eltávolít egy felhasználót terhelést management szerepkörből.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Még nem lehetséges a felhasználó eltávolítása smallrc.

## <a name="queued-query-detection-and-other-dmvs"></a>Aszinkron lekérdezés észlelési és más DMVs

Használhatja a `sys.dm_pdw_exec_requests` DMV feldolgozási várólista várják lekérdezések azonosításához. Várakozás a egy feldolgozási tárolóhely lekérdezések **felfüggesztett**állapotban lesz.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Terhelést felügyeleti szerepkörök megtekintheti a `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Az alábbi lekérdezés jeleníti meg, hogy milyen minden felhasználóhoz rendelt szerepkör.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

A következő típusú Várakozás SQL adatraktár foglalja magában:

- **LocalQueriesConcurrencyResourceType**: kívüli feldolgozási tárolóhely keretében ülnie-lekérdezéseket. DMV lekérdezések és a rendszer működik, mint például `SELECT @@VERSION` helyi lekérdezések ábrázolnak.
- **UserConcurrencyResourceType**: feldolgozási tárolóhely keretén belül ülnie-lekérdezéseket. Végfelhasználói táblák lekérdezéseket példák, amelyek az erőforrástípus volna jelenítik meg.
- **DmsConcurrencyResourceType**: mozgását adatműveleteket eredő várakozik.
- **BackupConcurrencyResourceType**: A várakozás azt jelzi, hogy egy adatbázis másolat készül. Ez az erőforrás típusa maximális értéke 1. Ha egyszerre több biztonsági másolatok felkért, a többi várólista lesz.

A `sys.dm_pdw_waits` DMV milyen kérelmének vár erőforrásokat használva.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

A `sys.dm_pdw_resource_waits` DMV csak a megadott lekérdezés elfogyasztott erőforrás vár jeleníti meg. Erőforrás várakozási idő csak méri, az idő Várakozás erőforrásokra adni, nem pedig az időt, amíg a ütemezése alakzatot a Processzor a lekérdezés alapjául szolgáló SQL-kiszolgálók jel várakozási időt.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

A `sys.dm_pdw_wait_stats` DMV vár történelmi trend elemzéséhez használható.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Következő lépések

Adatbázis-felhasználók és a biztonsági kezelésével kapcsolatos további tudnivalókért lásd: [az SQL adatraktár adatbázis biztonságos][]. További információt a nagyobb, hogy az erőforrás osztályok is csoportosított columnstore index minőségének című témakörben [Rebuilding indexek szakasz minőségének].

<!--Image references-->

<!--Article references-->
[Az SQL adatraktár az adatbázis védelmét]: ./sql-data-warehouse-overview-manage-security.md
[A szakasz minőségének indexek újbóli létrehozása]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Az SQL adatraktár az adatbázis védelmét]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
