<properties
   pageTitle="Az SQL adatraktár táblázatok – áttekintés |} Microsoft Azure"
   description="Első lépések az Azure SQL adattáblák raktári."
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
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Az SQL adatraktár táblázatok – áttekintés

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Adattípusok][]
- [Terjesztése][]
- [Index][]
- [Partition][]
- [Statisztika][]
- [Ideiglenes][]

Első lépések SQL adatraktár táblázatok létrehozása egyszerű.  Az egyszerű [Táblázat létrehozása][] alábbi követi a közös szintaxist, valószínűleg már ismerős az adatbázis használata.  Egy táblázat létrehozásához, egyszerűen szeretne nevezze el a táblázatot, nevezze el az oszlopok és az egyes oszlopok adattípusok meghatározása.  Ha már létrehozott táblák más adatbázis, ezt kell kinéznie nagyon ismerősnek Önnek.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

A fenti példa létrehoz egy táblázatot, ügyfelek nevű Utónév és Vezetéknév két oszloppal.  Minden egyes oszlopát VARCHAR(25), amely korlátozza a 25 karakteres adatok adattípusú van megadva.  Egy táblázat, valamint egyéb, a következő alapvető attribútumok megegyeznek főleg más adatbázisokat.  Adattípusok az egyes oszlopok határozza meg, és az adatok integritását.  Indexek felvehetők I/O csökkentésével teljesítmény javítása érdekében.  Szétválasztás lehet hozzáadni a teljesítmény javítása esetén, módosítania kell adatokat.

[Átnevezés] [ RENAME] SQL adatraktár táblázat így néz ki:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Az elosztott táblák

Elosztott rendszerek, például SQL adatraktár bevezetett új alapvető attribútum az **eloszlás oszlop**.  Az eloszlás oszlop, nagyon mi azt hangzik.  Határozza meg, hogy miként terjesztése, és osztása, a háttérben az adatok az oszlop.  Az eloszlás oszlop nélkülire táblázatot hoz létre, ha a táblázat automatikusan történik **ciklikus**.  Ciklikus táblák elegendő bizonyos esetekben lehetnek, terjesztési oszlopok meghatározása nagyban csökkentheti adatok mozgását lekérdezésekből, így a teljesítmény optimalizálása.  Lásd: a [táblázat összes][elosztás] jelöljön ki egy terjesztési oszlopot használatáról további információt.

## <a name="indexing-and-partitioning-tables"></a>Az indexelés és a szétválasztás táblák

Válnak további speciális használata SQL adatraktár, és a teljesítmény optimalizálása szeretné, ha többet szeretne tudni a Táblázateszközök Tervezés érdemes.  További tudnivalókért lásd: a cikkek [Táblázat adattípusok][Adattípusok], a [táblázat terjesztése][elosztás], a [táblázat indexelés][Index] és a [táblázat szétválasztás][partíciót]a.

## <a name="table-statistics"></a>Táblázat statisztika

A statisztikákat egy rendkívül fontos, hogy a legjobb teljesítmény elérése érdekében az SQL adatraktár ki az első.  SQL adatraktár nem még automatikusan hoz létre, és meg, mint az derült kap számíthat, a következő cikkünket [statisztikai][] olvasási Azure SQL-adatbázis frissítése statisztika valószínűleg a legfontosabb cikkek egyikét, annak érdekében, hogy a lekérdezések el a legjobb teljesítmény elérése érdekében olvassa el.

## <a name="temporary-tables"></a>Ideiglenes táblák

Ideiglenes táblák táblázatok, amely csak a bejelentkezési hosszának léteznek, és nem láthatja mások által is.  Ideiglenes táblák jó módszer tiltásához a következőt ideiglenes eredmények és is csökkentheti a Lemezkarbantartó van szükség lehet.  Ideiglenes táblák is csatlakozást helyi tároló, mivel az egyes műveletek gyorsabb teljesítmény is kínálnak.  Cikkek az [Ideiglenes tábla][ideiglenes] ideiglenes táblák olvashat bővebben.

## <a name="external-tables"></a>Külső tábla

Külső táblákban, más néven a Polybase táblában, olyan táblákat, amelyek SQL adatraktár külső adatokhoz lekérdezhető SQL adatraktár, de a pont.  Például táblát is létrehozhat egy külső mely pontok Azure Blob-tárolóhoz fájlokat.  Hogyan hozhat létre, és a lekérdezés egy külső táblában, lásd: [adatok betöltése a Polybase][].  

## <a name="unsupported-table-features"></a>Nem támogatott táblázatszolgáltatások

SQL-adatraktár más adatbázisokat által kínált ugyanazon táblában szolgáltatásainak nagy része tartalmaz, amíg vannak bizonyos szolgáltatások, amelyek még nem támogatott.  Az alábbi képen a táblázat, amely még nem támogatott funkciók közül egyesek listáját.

| Nem támogatott szolgáltatások |
| --- |
|[Azonosító tulajdonság][] (lásd: a [Helyettesítő kulcs megoldás hozzárendelése][])|
|Elsődleges kulcs, idegen kulcsok, egyedi és a [Táblázat kényszerek][] jelölőnégyzet|
|[Egyedi index][]|
|[A számított oszlopok][]|
|[A ritka oszlopok][]|
|[Felhasználó által definiált típusai][]|
|[Sorszám][]|
|[Eseményindítók][]|
|[Az indexelt nézetek][]|
|[Szinonimák][]|

## <a name="table-size-queries"></a>Táblázat méretének lekérdezések

Egy egyszerű helyet és egy másik tábla minden egyes a 60 terjesztését elfogyasztott sorok módja [DBCC PDW_SHOWSPACEUSED][]használja.

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Azonban DBCC parancsaival is lehet igazán korlátozása.  Dinamikus kezelése nézetek (DMVs) sokkal részletek megtekintéséhez, valamint a sokkal jobban kézben tarthatók a lekérdezés eredményének ad teszi lehetővé.  Ez a nézet úgy, hogy mi ez, és további cikkek a példák számos hivatkozni létrehozásával kell kezdenie.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Táblázat terület összefoglalása

A lekérdezés tábla a sorok és a térköz adja eredményül.  Lásd: a táblákat a legnagyobb táblák illetve hogy ezek ciklikus vagy elosztott kivonat remek lekérdezés.  Elosztott kivonat táblák azt is látható a terjesztési oszlopban.  A legtöbb esetben a legnagyobb táblák csoportosított columnstore index létrehozása eloszlású kivonat kell lennie.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Telepítési típus szerint táblázat terület

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Táblázat terület, index típus szerint

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Telepítési hely összefoglalása

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Következő lépések

További információért lásd a cikkek [Táblázat adattípusok][Adattípusok], [táblázat terjesztése][elosztás], [táblázat indexelés][Index], [táblázat szétválasztás][partíciót], [Táblázat statisztika fenntartása][Statisztika] és [Ideiglenes táblák][ideiglenes].  Ajánlott eljárások bővebben: [SQL adatok raktári gyakorlati tanácsokat][].

<!--Image references-->

<!--Article references-->
[– Áttekintés]: ./sql-data-warehouse-tables-overview.md
[Adattípusok]: ./sql-data-warehouse-tables-data-types.md
[Terjesztése]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md
[Ideiglenes]: ./sql-data-warehouse-tables-temporary.md
[Ajánlott eljárások a SQL-adatok raktári]: ./sql-data-warehouse-best-practices.md
[Adatok betöltése a Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[TÁBLÁZAT LÉTREHOZÁSA]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Azonosító tulajdonság]: https://msdn.microsoft.com/library/ms186775.aspx
[Helyettesítő kulcsok megoldás hozzárendelése]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Táblázat korlátozásokkal]: https://msdn.microsoft.com/library/ms188066.aspx
[A számított oszlopok]: https://msdn.microsoft.com/library/ms186241.aspx
[A ritka oszlopok]: https://msdn.microsoft.com/library/cc280604.aspx
[Felhasználó által definiált típusai]: https://msdn.microsoft.com/library/ms131694.aspx
[Sorszám]: https://msdn.microsoft.com/library/ff878091.aspx
[Eseményindítók]: https://msdn.microsoft.com/library/ms189799.aspx
[Az indexelt nézetek]: https://msdn.microsoft.com/library/ms191432.aspx
[Szinonimák]: https://msdn.microsoft.com/library/ms177544.aspx
[Egyedi index]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
