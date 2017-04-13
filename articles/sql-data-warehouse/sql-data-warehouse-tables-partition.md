<properties
   pageTitle="Táblákat az SQL adatraktár szétválasztás |} Microsoft Azure"
   description="Első lépések az Azure SQL-adatraktár szétválasztás táblázat."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Az SQL adatraktár táblák szétválasztás

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Adattípusok][]
- [Terjesztése][]
- [Index][]
- [Partition][]
- [Statisztika][]
- [Ideiglenes][]

Támogatott SQL adatraktár táblázat diagramtípusokat; a szétválasztás többek között csoportosított columnstore, a csoportosított index és a halommemória.  Az összes terjesztési típusairól, beleértve a kivonat és a normális eloszlású ciklikus szétválasztás is támogatott.  Szétválasztás lehetővé teszi, hogy az adatok osztása a szétválasztás kisebb csoportok adatok és a legtöbb esetben történik meg egy dátumot tartalmazó oszlopból.

## <a name="benefits-of-partitioning"></a>Szétválasztás előnyei

Szétválasztás élvezheti adatok karbantartása és lekérdezési teljesítmény.  Csak az egyik vagy mindkét előnyöket e függ, hogyan adatok betöltése, és hogy mindkét célra használható ugyanabban az oszlopban, mivel a szétválasztás csak végezhető el egy oszlop.

### <a name="benefits-to-loads"></a>Terhelések előnyei

A hatékonyság és partíciót törlés használatával adatok betöltése, váltás és egyesítése teljesítményének javítása az SQL adatraktár szétválasztás elsődleges előnyeit  A legtöbb esetben adatok particionált meg egy dátumot tartalmazó oszlopból példány szorosan kapcsolódik a sorozat, amelyek az adatok betöltése az adatbázishoz.  A legnagyobb előnyei partíciót megőrzéséhez az adatok közül, a elkerülő tranzakció naplózási.  Egyszerűen beszúrása, módosítása vagy törlése az adatok lehet a legegyszerűbb megoldás, egy kis gondolkodási és munkamennyiség, miközben a betöltés közben szétválasztás használata lényegében javíthatja a teljesítményt.

Partition váltás használható gyorsan távolítsa el vagy cserélje le a táblázat egyik szakaszát.  Ha például egy értékesítési ténytáblában, csak az adatokat az elmúlt 36 hónap tartalmazhat.  Minden egyes hónap végén a legrégebbi hónap értékesítési adatok törlődnek az a táblázat.  Ezeket az adatokat egy delete utasítás segítségével törölheti az adatokat a legrégebbi hónap sikerült törölni.  Azonban nagy mennyiségű adat--soronként egy delete utasítás törlése is nagyon lassan, valamint a kockázat nagy tranzakciók, amely túl hosszú ideig is eltelhet visszavonás, ha valami mentésük létrehozása.  A további optimális megközelítés is egyszerűen húzza a legrégebbi partíciót adatok.  Ha az egyes sorok törlése óráig is eltarthat, törlés, egy teljes partíciót másodperc is eltelhet.

### <a name="benefits-to-queries"></a>Lekérdezések legfontosabb előny

Szétválasztás is használható a lekérdezési teljesítmény javítása érdekében.  Lekérdezés particionált oszlop szűrő érinti, ha ez korlátozhatja a beolvasott képet, csak az esetleg az adatokat, a teljes táblázat vizsgálata elkerülése sok kisebb részhalmazát feljogosító partíciók.  Csoportosított columnstore indexek bevezetésével predikátumalakzat eltávolítási teljesítmény előnyökkel jár kisebb előnyös, de bizonyos esetekben lehet lekérdezések előny.  Például ha az értékesítési ténytáblában van partícionálni 36 hónap használ az értékesítési dátum mezőben, majd a szűrő lekérdezések az értékesítés napon átugorhatja keresés a partíciók, amelyek nem felelnek meg a szűrő.

## <a name="partition-sizing-guidance"></a>Partition méretező útmutató

Szétválasztás bizonyos esetekben a teljesítmény javítása érdekében is használható, miközben a partíciók **túl sok** tábla létrehozása is sérti a teljesítmény bizonyos körülmények között.  A problémákat különösen teljesül csoportosított columnstore táblákhoz.  Szétválasztás hasznosak lehetnek, fontos megtudhatja, hogy mikor érdemes használni a szétválasztás és hozhat létre partíciók száma.  Nincs mód lehetőség használatával, hány partíciót túl sok kemény gyors szabály, amelytől függ az adatok és hány partíciók tölt való egyidejű.  De egy általános szabály a legjobb megoldás, mint érdemes 10s partíciók, hogy 100s nem 1000s hozzáadásával.

Létrehozásakor szétválasztás **csoportosított columnstore** táblázatokban, akkor vegye figyelembe, hogy hány sort az összes partíciót fog land.  Az optimális tömörítés és a teljesítmény a csoportosított columnstore táblák legalább egy terjesztési és partíciót 1 millió sorok van szükség.  Létrehozta a partíciók, mielőtt SQL adatraktár már osztja táblákra 60 elosztott adatbázisok.  Minden felvett tábla szétválasztás van mellett a létre Bepillantás a színfalak mögé terjesztését.  Ebben a példában ha a értékesítési ténytábla szereplő 36 havi partíciók, és tekintve, hogy SQL adatraktár 60 terjesztését tartalmaz, majd a értékesítési ténytábla kell tartalmaznia havi 60 millió sorok vagy 2.1 milliárd sorok minden hónapnál vannak töltve használata  Ha egy táblázat jelentősen kisebb, mint a sorok partíciót egy javasolt minimális érték sorokat tartalmaz, fontolja meg kevesebb partíciót partíciót egy sorok számát növelni kezdeményezéséhez.  A [indexelés][Index] cikk, amely magában foglalja a SQL adatraktár minőségének fürt columnstore indexek futó lekérdezések is megjelenik.

## <a name="syntax-difference-from-sql-server"></a>Az SQL Server szintaxis különbség

SQL-adatraktár egyszerűsített meghatározását partíciót némileg eltér az SQL Server vezet be.  Particionáló függvények és színsémák nem használt SQL adatraktár mint az SQL Server.  Ehelyett kell tennie mindössze particionált oszlopokat, valamint a oszlopazonosító pontok azonosításához.  Lehet, hogy a szétválasztás szintaxisa a következő SQL Server némileg eltér, miközben az alapfogalmak megegyeznek.  Az SQL Server és a SQL adatraktár támogatja a táblázat, amely lehet volt partíciót egy egyoszlopos partíciót.  Szétválasztás kapcsolatos további információért lásd: [particionálva táblák és indexek][].

Az alábbi példában particionálva SQL adatraktár [tábla létrehozása][] kimutatás partíciók a FactInternetSales tábla a OrderDateKey oszlopra:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Az SQL Server szétválasztás áttelepítése

Áttelepítése az SQL Server partíciót definíciók SQL adatraktár egyszerűen:

- Az SQL Server [partíciót rendszer][]kiküszöbölése.
- [Partition függvény][] meghatározása hozzáadása a táblázat létrehozása.

Ha áttelepítése particionált táblázat az SQL Server-példányt az alábbi SQL segítséget nyújthat interrogate, amely az összes partíciót a sorok számát.  Ne feledje, hogy az azonos particionáló Granularitás a SQL adatraktár használata esetén partíciót egy sorok számát csökkenni fog 60 tényezővel.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Terhelést kezelése

Egy végleges darab figyelembe veszi kiszámíthatja a táblázat partíciót döntés akkor [terhelést][].  SQL-adatraktár terhelést kezelését akkor elsősorban a memória és a párhuzamos.  A lekérdezés végrehajtása során minden egyes terjesztési rendelt a maximális memóriaméret SQL adatraktár irányadó erőforrás osztályok.  Ideális a partíciók fog méretű egyéb tényezők, például a memória igényeinek csoportosított columnstore indexek tudnivalóit figyelembe véve.  Nagyban csoportosított columnstore indexek kedvezménye, ha van rendelve, hogy több memóriát.  Ezért érdemes annak érdekében, hogy a egy partíciót indexet újraépítő nem függeszteni memória. A lekérdezéshez rendelkezésre álló memória növelésével naplókról az alapértelmezett szerepkört, smallrc, például largerc a szerepkörök egyikét lehet elérni.

A felosztás egy terjesztési memória információkat szabályozó dinamikus kezelése erőforrásnézetek lekérdezésével érhető el. A valóságban a memória támogatás lesz kisebb, mint az alábbi adatokat. Azonban ezt az útmutatást, amely használható, amikor a partíciók adatok műveletekhez méretező szintű nyújt.  Lehetőleg ne a partíciók túl a memória támogatás a nagyon nagy erőforrás osztály által biztosított méretezése. A partíciók kívül az alábbi ábrán a nagyobb, futtassa a memória nyomása, amely kisebb optimális tömörítést viszont vezet kockázata.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Partition Váltás

SQL-adatraktár partíciót felosztása egyesítése és váltás támogatja. Egyes függvényekhez excuted az [ALTER TABLE][] utasítás használatával.

Válthat a két tábla közötti partíciót gondoskodnia kell arról, hogy a partíciók igazításához a megfelelő határai és, hogy a táblázat definíciók egyezik-e. Ellenőrző korlátozások meg lehessen őrizni a tábla körét nem érhetők el, a forrástáblához tartalmaznia kell a azonos partíciót határokat, a cél táblához. Ha ez nem akkor a partíciót váltás sikertelen lesz, nem a rendszer szinkronizálja a partíciót metaadatok.

### <a name="how-to-split-a-partition-that-contains-data"></a>Hogyan lehet adatokat tartalmazó partíciót felosztása

A lehető leghatékonyabb módszer fel szeretne osztani egy adatokat tartalmazó partíciót egy `CTAS` utasítás. Ha a particionált táblázat a csoportosított columnstore majd a táblázat partíciót lehet üres előtt lehet megosztani.

Az alábbi képen egy particionált columnstore mintatáblázat egy sor az összes partíciót tartalmazó:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] A statisztikai objektum létrehozásával azt győződjön meg arról, hogy a táblázat metaadatok pontosabb. Azt kihagyja statisztika létrehozása, ha SQL adatraktár alapértelmezett értékeket fogja használni. További információt a statisztika véleményezze a következőt [statisztikai adatokat][].

Kattintson a sorok száma az kérdezhet azt a `sys.partitions` katalógus megtekintése:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Próbálja ki az alábbi táblázat felosztása, ha azt hiba jelenik meg:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Üzenet 35346 szint 15, State 1, a vonal 44 felosztása záradék ALTER PARTÍCIÓT utasítás nem sikerült, mert a partíciót nem üres.  Csak az üres partíciók is lehet megosztani a columnstore index létrehozása a táblázatban megléte esetén. Célszerű letiltani a columnstore index előtt az ALTER PARTÍCIÓT utasítás kiállító, majd a columnstore index Újraépítés, MEGVÁLTOZTATHATJA PARTÍCIÓT befejeződése után.

Azonban ábrázolásakor `CTAS` tartása, hogy adatait új tábla létrehozása.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Szerint vannak rendezve, a partíciót határai kapcsoló engedélyezett (permitted). A forrástáblához azt követően is osztott üres partíciót a hagyja meg.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Még hátralévő feladatokat csak az adatokat az új partíciót határokat a igazítása `CTAS` és a Váltás a fő táblára ismét az adatok

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Az adatok mozgásának befejezése után célszerű annak érdekében, hogy pontosan a megfelelő partíciót adatait új eloszlását tükrözik a cél táblához statisztikai adatok frissítése:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Adatforrás-vezérlő szétválasztás táblázat

A forrás rendszerben elkerülése érdekében a táblázat definíció **Rozsdás** érdemes vegye figyelembe az alábbi módon:

1. A tábla létrehozása, particionált táblázatként, de nincs partíciót értékekkel

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`a telepítés részeként a tábla:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Az eljárás a kódot a verziókövetés statikus marad, és a particionáló oszlopazonosító értékeket engedélyezettek dinamikus; változó a raktári az adott idő alatt.

## <a name="next-steps"></a>Következő lépések

További információért lásd a cikkek [Táblázat áttekintése][– Áttekintés], [Táblázat adattípusok][Adattípusok], [táblázat terjesztése][elosztás], [táblázat indexelés][Index], [Táblázat statisztikák fenntartása][Statisztika] és [Ideiglenes táblák][ideiglenes].  Ajánlott eljárások bővebben: [SQL adatok raktári gyakorlati tanácsokat][].

<!--Image references-->

<!--Article references-->
[– Áttekintés]: ./sql-data-warehouse-tables-overview.md
[Adattípusok]: ./sql-data-warehouse-tables-data-types.md
[Terjesztése]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md
[Ideiglenes]: ./sql-data-warehouse-tables-temporary.md
[terhelést kezelése]: ./sql-data-warehouse-develop-concurrency.md
[Ajánlott eljárások a SQL-adatok raktári]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Particionált táblák és indexek]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TÁBLÁZAT LÉTREHOZÁSA]: https://msdn.microsoft.com/library/mt203953.aspx
[partition függvény]: https://msdn.microsoft.com/library/ms187802.aspx
[partition séma]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
