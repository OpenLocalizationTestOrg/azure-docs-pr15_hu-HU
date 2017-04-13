<properties
   pageTitle="Az SQL adatraktár tranzakciók optimalizálása |} Microsoft Azure"
   description="Ajánlott gyakorlati tanácsokkal hatékony tranzakció frissítések írási az Azure SQL-adatraktár"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Az SQL adatraktár tranzakciók optimalizálása

Ez a cikk ismerteti, hogyan hosszú visszagörgetése kockázat minimalizálása közben a tranzakció alapú kód optimalizálásához.

## <a name="transactions-and-logging"></a>Tranzakciók és naplózás

Tranzakcióinak relációs adatbázisból motor fontos összetevője. SQL adatraktár használja a tranzakciók adatok módosítása közben. Ezeket a tranzakciókat lehet explicit vagy implicit. Egyetlen `INSERT`, `UPDATE` és `DELETE` kimutatások ábrázolnak összes implicit tranzakciók. Vannak explicit tranzakciók kifejezetten használatával által megírt `BEGIN TRAN`, `COMMIT TRAN` vagy `ROLLBACK TRAN` és a jellemző felhasználási módjuk látható, amikor több módosítást kimutatások kell egyetlen atomi egységben közös tartozik. 

Azure SQL-adatraktár véglegesíti a változtatásokat az adatbázis-tranzakción naplók használatával. Minden egyes terjesztési saját tranzakciónaplója tartalmaz. Transaction napló írások olyan automatikus. Nem szükséges beállításokat nem. Bár ezt a folyamatot garantálja az írás azt a rendszer egy általános bevezetésére. Ez a hatás által tranzakción keresztül hatékony kódírás méretűre. Tranzakción keresztül hatékony kód szélesebb körben két kategóriába esik.

- Kihasználhatja a minimális naplózás szerkezeteket, ha lehetséges
- Folyamat az adatoknak elkerülése érdekében az egyes időigényes tranzakciók kötegekben hatóköre
- Fogadja el a Váltás az adott partíciót nagy módosításai mintájának partíciót

## <a name="minimal-vs-full-logging"></a>Minimális és teljes naplózás összehasonlítása

Nem kedvelik-e teljesen naplózott műveletek, a tranzakciónaplója használatával nyomon követheti a minden sor módosítása, minimálisan naplózott műveletek és figyelemmel kísérése mértékben hozzárendelések csak a metaadatok módosításokat. Emiatt a minimális naplózás jár, csak a szükséges a hiba vagy közvetlen felkérés tranzakció visszavonás információk naplózása (`ROLLBACK TRAN`). Sokkal kevésbé információkat követi nyomon az tranzakciónaplója, mint egy minimálisan naplózott művelet jobban, mint egy hasonló méretű teljesen naplózott művelet hajt végre. Ezenkívül kevesebb írások lépjen a tranzakciónaplója, mert sok kisebb összegű naplóadatok jön létre és így további I/O hatékony.

A tranzakciók biztonsági korlátozások csak a teljes mértékben naplózott műveletek vonatkoznak.

>[AZURE.NOTE] Minimálisan naplózott műveletek közvetlen tranzakciók vehet részt. Felosztás szerkezetben minden változások nyomon követése, mint lehetőség minimálisan naplózott műveletek visszaállítása. Ha meg szeretné érteni, hogy a változtatások "minimálisan" be van-e jelentkezve fontos még nem nem naplózott.

## <a name="minimally-logged-operations"></a>Minimálisan naplózott műveletek

Az alábbi műveletek képesek minimálisan naplózott:

- TÁBLÁZAT kijelölése szerint ([CTAS][]) létrehozása
- BESZÚRÁS. VÁLASSZA A
- INDEX LÉTREHOZÁSA
- ALTER INDEXET ÚJRAÉPÍTŐ
- DROP INDEX
- TÁBLÁZAT CSONKOLÁSA
- TÁBLÁZAT LEVÁLASZTÁSA
- ALTER TÁBLÁZAT KAPCSOLÓ PARTÍCIÓT

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Mozgás belső adatműveleteket (például: `BROADCAST` és `SHUFFLE`) nem érinti a tranzakció biztonsági korlátot.

## <a name="minimal-logging-with-bulk-load"></a>A tömeges betöltés minimális naplózás

`CTAS`és `INSERT...SELECT` vannak mindkét tömeges betöltés műveletek. Azonban mindkét befolyásolják a cél tábla definícióját, és a betöltés forgatókönyv függnek. Az alábbi táblázat, amelyből megtudhatja, hogy ha a tömeges művelet a rendszer teljes mértékben vagy minimálisan naplózza a következő:  

| Elsődleges Index               | Forgatókönyv betöltése                                            | Naplózás módja |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Halommemória                        | Bármely                                                      | **Minimális**  |
| Csoportosított Index             | Üres céltáblában                                       | **Minimális**  |
| Csoportosított Index             | Betöltött sorok nem áll átfedésben a cél meglévő lapokkal | **Minimális**  |
| Csoportosított Index             | A meglévő lapokkal cél betöltött sorok átfedés        | Teljes         |
| Csoportosított Columnstore Index | A Köteg mérete > = 102,400 per igazított partíciót terjesztési | **Minimális**  |
| Csoportosított Columnstore Index | Méret < 102,400 igazított partíciót terjesztési egy köteg  | Teljes         |

Érdemes megjegyezni, bármely írások frissítése a másodlagos vagy nem csoportosított indexek mindig kell a teljes mértékben naplózott műveletek.

> [AZURE.IMPORTANT] SQL-adatraktár 60 terjesztését tartalmaz. Ezért, feltéve, hogy minden sor egyenletesen elosztott és egyetlen partíciót jelenjenek meg, a köteg kell 6,144,000 sort tartalmaz vagy nagyobb a csoportosított Columnstore Index írásakor minimálisan jelentkezzen be. Ha a táblázat particionálva van, és be szeretné illeszteni a sorokat partíciót határai időtartamát, majd szüksége lesz egy partíciót oszlopazonosító feltételezve, hogy a még az adatok terjesztési 6,144,000 sorok. Minden egyes terjesztési partíciót független haladhatja meg a Beszúrás minimálisan kell bejelentkeznie az eloszlás 102 400 sor küszöbértékét.

Adatok betöltése a csoportosított indexű nem üres táblába teljesen naplózott és minimálisan naplózott sorok többféle gyakran tartalmaznak. A csoportosított indexe kiegyensúlyozott fa (b-fa) oldalát. Ha a lap már olyan sorokat tartalmaz, a másik tranzakciók írt, majd ezek írások teljesen naplózandó. Jó helyen jár Ha az oldal üres majd az írás azon a lapon a rendszer minimálisan naplózza.

## <a name="optimizing-deletes"></a>Optimalizálás törlése

`DELETE`egy olyan teljesen naplózott művelet.  Egy nagy mennyiségű adatot, táblázat vagy partíciót törölni szeretne, ha gyakran célszerű további `SELECT` szeretné megtartani, az adatok, amelyek minimálisan naplózott műveletként futtatását is lehetővé teszi.  Ehhez [CTAS][]hozzon létre egy új táblázatot.  Létrehozása után használja a cserélheti jobbra az újonnan létrehozott táblázatot a régi táblázat [átnevezése][] parancsára.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimalizálás a frissítések

`UPDATE`egy olyan teljesen naplózott művelet.  Ha módosítania kell a tábla sorainak sok, vagy egy partíciót gyakran lehet sokkal hatékonyabban [CTAS][] például egy minimálisan naplózott művelet használni ehhez.

A teljes táblázat az alábbi példában frissítés alakított egy `CTAS` , hogy a minimális naplózás lehetőség.

Ebben az esetben azt utólag hozzáadni engedmény összege a sales a táblázatban:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Hozza létre a nagy táblázatok használatuk előnyeivel SQL adatraktár terhelést dokumentumkezelési funkciókat. További részletekért olvassa el a terhelést kezelése csoportban [feldolgozási][] című témakörben.

## <a name="optimizing-with-partition-switching"></a>A partíciók váltás optimalizálása

Ha elektronikus belül egy [táblázat partíciót][]nagyméretű módosításokat, váltás a minta partíciót értelemben sok teszi. Ha az adatok módosítását jelentős, és több partíciót átnyúlik, majd a partíciók fölé egyszerűen léptetés éri el ugyanazt az eredményt.

Partition kapcsoló elvégzendő lépések a következők:
1. Hozzon létre egy üres partíciót meg
2. A frissítés ténykedés egy CTAS
3. Váltás a meglévő adatokat, a kimenő tábla
4. Váltás az új adatok
5. Az adatok karbantartása

Jó helyen jár Váltás a partíciók azonosításához azt először kell egy segítő eljárást, mint például az alábbi itt összeállítása. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Ez az eljárás maximalizálja újra felhasználhat kódot, és továbbra is a partíciót tömörebb például váltás.

Az alábbi kód lépéseinek öt imént említett lekérdezéstípusokról gyakorlatának Váltás teljes partíciót eléréséhez.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>A kisebb kötegekben naplózás kisméretűvé alakítása

Nagy adatok módosítás műveletekhez lehet meghagyni a művelet osztása szövegadattömb egy vagy több szűkítheti a címzettek körét a munka módosítása.

Egy működő példát alatt áll rendelkezésre. Jelölje ki a technika trivial számmá van állítva a tétel méretét. A valóságban lényegesen nagyobb lenne a tétel méretét. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Szünet, és útmutatást méretezése

Azure SQL-adatraktár lehetővé teszi, mutasson az egérrel, folytathatja és méretezni a adatraktár igény. Amikor felfüggesztheti, és a fontos megértéséhez, hogy minden olyan repülés tranzakciók megszakítja azonnal; SQL adatraktár méretezése bármely nyitott tranzakciók állítható vissza okozza. Ha a terhelést volna ki egy ideig futtatott és hiányos adatok módosítását, a szünet vagy a méretezés művelet előtt, majd a munka kell vonható. Ez hatással lehetnek a ideig tart, felfüggesztheti és az Azure SQL-adatraktár adatbázis méretezni. 

> [AZURE.IMPORTANT] Mindkét `UPDATE` és `DELETE` teljesen naplózott művelet, így ezek visszavonás/mégis műveletek jelentősen hosszabb egyenértékű minimálisan bejelentkezett műveletek. 

A legjobb esetet, hogy az adatok nézetbeli módosítás teljes szüneteltetése vagy SQL adatraktár méretezés előtt tranzakciók. Azonban ez nem mindig lehet gyakorlati. Egy hosszú visszaállítása kockázatcsökkentés, fontolja meg az alábbi lehetőségek közül:

- Írja be újra, hosszú futó műveletek [CTAS][] használatával
- A művelet szétválasztani szövegadattömb; a sorok csak egy részhalmazát működő

## <a name="next-steps"></a>Következő lépések

Lásd: az [SQL adatraktár tranzakciók][] , ha többet szeretne tudni a elkülönítési szintek és tranzakció alapú korlátai.  Ajánlott eljárások a többi áttekintése lásd: az [SQL adatok raktári gyakorlati tanácsok][].

<!--Image references-->

<!--Article references-->
[Az SQL adatraktár tranzakciók]: ./sql-data-warehouse-develop-transactions.md
[táblázat partíciót]: ./sql-data-warehouse-tables-partition.md
[Feldolgozási]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Ajánlott eljárások a SQL-adatok raktári]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[ÁTNEVEZÉSE]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

