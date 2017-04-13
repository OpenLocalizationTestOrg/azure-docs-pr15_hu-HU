<properties
   pageTitle="Tábla SQL adatraktár (CTAS) kijelölésekor létrehozása |} Microsoft Azure"
   description="Tippek kódolásának a tábla létrehozása az Azure SQL-adatraktár (CTAS) select utasítás mint megoldások fejlesztésére."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Táblázat létrehozása SQL adatraktár, jelölje be (CTAS)
Jelölje ki, táblázat létrehozása vagy `CTAS` érhető el a legfontosabb az SQL-T funkciók közül. Egy új táblát hoz létre a SELECT utasítás kimenetét alapján teljesen parallelized műveletet. `CTAS`Hozzon létre egy táblázatot egy példányát a legegyszerűbb és a leggyorsabb módja van. Feltöltött változata szükségesnek `SELECT..INTO` mezőkódokkal. A dokumentum tartalmaz példák és a gyakorlati tanácsok a `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Táblázat másolása CTAS használatával

Esetleg a leggyakoribb egyikét használja a `CTAS` hoz létre egy táblázatot egy példányát, hogy a DDL módosíthatja. Ha például eredetileg létrehozott, a táblázat `ROUND_ROBIN` , és most már kívánt módosítsa az oszlop, elosztott táblázat `CTAS` , hogyan szeretné módosítani a terjesztési oszlopban. `CTAS`is használható szétválasztás, indexelés vagy az oszlop típusának módosítása.

Tegyük fel, az alábbi táblázat az alapértelmezett terjesztési típusú használatával létrehozott `ROUND_ROBIN` elosztott, mivel a megadott nincs terjesztési oszlop a `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Most már szeretne létrehozni egy új példányát az alábbi táblázat a csoportosított Columnstore indexű, hogy akkor kihasználhatja a csoportosított Columnstore táblák teljesítményét. Is szeretné osztani az alábbi táblázat a ProductKey, mivel meg vannak előrejelző illesztések ezt az oszlopot, és szeretné kerülni az adatok mozgás közben a ProductKey illesztések. Végül is szeretne hozzáadni a szétválasztás OrderDateKey meg, hogy a régi adatok történő húzással a régi partíciót gyorsan törölheti. Az alábbiakban a CTAS nyilatkozat, amely a régi tábla volna másol egy új táblát.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Végül a táblákat az új táblában felcserélése és a régi tábla majd ejtse átnevezheti.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL-adatraktár támogatási automatikus létrehozása és az automatikus frissítés statisztika még nem tartalmaz.  Jelentős módosításokat fordul elő, az adatok vagy annak érdekében, hogy a legjobb teljesítmény elérése érdekében a lekérdezések juthat, fontos, hogy statisztika hozható létre az összes tábla összes oszlop az első betöltése után.  Statisztikai részletes leírását témakört [Statisztika][] témakörök fejlesztése csoportjában található.

## <a name="using-ctas-to-work-around-unsupported-features"></a>CTAS használata a nem támogatott szolgáltatások

`CTAS`is használható az alább felsorolt nem támogatott szolgáltatások számos kerülhető. Ez gyakran szemléltetéséhez, nyereség/win helyzet lesz, nem csak a kód lesz kompatibilis, de azt fogja gyakran végrehajtása gyorsabb SQL adatraktár. Ez a teljes mértékben parallelized tervének eredményeként. Olyan esetek, amelyek körül a CTAS is dolgozott:

- JELÖLJE KI. AZ
- A frissítések ANSI ILLESZTÉSEK
- ANSI illesztések törlésekről
- Kimutatás EGYESÍTÉSE

> [AZURE.NOTE] Próbálja meg gondolja, hogy "CTAS első". Ha úgy gondolja, hogy úgy oldhatja meg a problémát használatával `CTAS` , akkor általában irányból – a legjobb módja, ha több adatot ír eredményt adja.
>

## <a name="selectinto"></a>JELÖLJE KI. AZ
Akkor azt tapasztalhatja `SELECT..INTO` helyek a megoldás a számos jelenik meg.

Az alábbi képen egy `SELECT..INTO` utasítás:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

A fenti konvertálása `CTAS` igazán egyenes továbbítás:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS jelenleg csak egy terjesztési oszlopban kell megadni.  Ha Ön nem szándékosan próbál az eloszlás oszlop módosítása a `CTAS` a leggyorsabb végez, ha kijelöl egy terjesztési oszlop, amely megegyezik a táblát, a stratégia elkerülhető adatok mozgását.  Ha szeretne létrehozni egy kis táblára, ahol teljesítmény nem most, majd megadhatja `ROUND_ROBIN` zsugorítása döntse el, terjesztési oszlopon.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI illesztés feltölteni az update utasítás

Előfordulhat, hogy van egy összetett frissítést, hogy a kettőnél több táblát együttes használatával szintaxis csatlakozás ANSI frissítése vagy a törlés végrehajtásához csatlakozik.

Tegyük fel, az alábbi táblázat frissítése van:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Az eredeti lekérdezés előfordulhat, hogy rendelkezik volt a alábbihoz hasonló:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Mivel SQL adatraktár nem támogatja az ANSI illesztő a `FROM` záradéka egy `UPDATE` utasítás, nem másolhat fölé kód kissé módosítás nélkül.

Kombinációját is használhatja a `CTAS` és cseréje a kód implicit illesztéssé:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI illesztés helyett kimutatások törlése
Előfordul, hogy a legjobb megközelítés adatok törlése környezetbe `CTAS`. Az adatok egyszerűen törléssel jelölje ki az adatokat meg szeretné őrizni. A különösen igaz `DELETE` csatlakozó szintaxisát SQL adatraktár nem támogatja az ANSI óta csatlakozik az ansi használó kimutatásokat a `FROM` záradékában egy `DELETE` utasítás.

Példa egy konvertált DELETE utasítás alatti érhető el:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Egyesítés kimutatások cseréje
Egyesítés kimutatásokat is kell cserélni, legalább részben használatával `CTAS`. Is összevonhatja az `INSERT` és a `UPDATE` egy utasítás be. A törölt rekordokat le kell zárni a második utasításban kellene.

Példa egy `UPSERT` alatti érhető el:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS javaslat: kifejezetten az adattípus és a kimeneti nullázhatósági beállítás

Kód áttelepítéséhez Észreveheti, ilyen típusú coding mintát keresztül futtatása:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinctively gondolná kód át kell telepítenie egy CTAS, és szeretné, hogy helyes-e. Azonban van egy rejtett probléma.

A következő kódot nem ugyanazt az eredményt adnak:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Figyelje meg, hogy az oszlopban a "eredménye" előre végzi az adatok típusát és nullázhatósági beállítás értékeket a kifejezés. Finom szórásnégyzetre az értékek ez is vezethet, ha még nem ügyeljen.

Próbálkozzon a következő példában:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Az eredmény tárolt érték különböző. Állandó eredmény oszlopának értékét használja a program az egyéb kifejezéseket a hiba még jelentősebb válik.

![][1]

Ez különösen fontos az adatok áttelepítések. Annak ellenére, hogy a második lekérdezés arguably pontosabb probléma van. Az adatok másik és összehasonlítása az adatforrás rendszer és, amely az áttelepítés integritás kérdések vezet. Ez az egyik e ritka esetekben, amikor a "nem megfelelő" válasz ténylegesen a megfelelőt!

Ez a két érték közötti különbség láthatja oka az implicit típus döntő le. A tábla első példa oszlop definíciója határozza meg. A sort szúr be egy implicit típuskonverziós fordul elő. A második példában van nincs implicit típuskonverziós, a kifejezés az oszlop adattípusa határozza meg. Is figyelje meg, hogy a második példában az a oszlopban meg van adva NULLable oszlopként mivel az első példa nem. Kattintson az első, például oszlop nullázhatósági beállítás a táblázat létrehozásának lett megadott külön. A második azt csak maradt a kifejezést, és alapértelmezés szerint ez a példa a NULL definíciója eredményezne.  

A problémák megoldásához kifejezetten meg kell adnia a típuskonverziós és a nullázhatósági beállítás a `SELECT` részét a `CTAS` utasítás. A tulajdonságok nem állíthatja be a tábla létrehozása kijelzőt.

Az alábbi példa bemutatja, hogyan lehet a kód javítása:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Vegye figyelembe az alábbiakat:
- CAST vagy konvertálás sikerült használtak
- ISNULL használatos nullázhatósági beállítás nem ÖSSZETEVŐJÉBEN kényszerítése
- ISNULL, akkor az legkülső függvény
- A második a ISNULL része állandó tehát 0

> [AZURE.NOTE] A nullázhatósági beállítás megfelelően meg azt a fontos használandó `ISNULL` , és nem `COALESCE`. `COALESCE`nem mérvadó függvényt, és így a kifejezés eredménye a program mindig NULLable. `ISNULL`nem azonos. Mérvadó. Ezért ha a második rész a `ISNULL` függvény állandó vagy szövegkonstans, akkor az eredményül kapott értékkel lesz nem null értékű.

Ez a tipp nem áll a számítások integritását biztosító csak hasznos. Fontos is táblázat partíciót váltására szolgáló. Tegyük fel, az alábbi táblázat a FAKT jelenti van:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Jó helyen jár az érték mező értéke azt nem része a forrásadatok számított kifejezés.

A művelet érdemes lehet particionált adatkészlet létrehozása:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

A lekérdezés tökéletesen finom szeretné futtatni. A probléma tartalmaz, végezze el a Váltás a partíciót használatakor. A táblázat definíciók nem egyeznek. Ellenőrizze a táblázat definíciók felel meg a CTAS kell módosítani.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Látható tehát, hogy típus egységesebb és fenntartása egy CTAS nullázhatósági beállítás-tulajdonságok egy jó mérnöki javasoljuk. Segít megelőzni, a számításokat az adatintegritás fenntartása és a is biztosítható, hogy partíciót váltás lehetséges.

Kérjük, olvassa el az MSDN [CTAS][]használatáról további információt. A legfontosabb kimutatások az Azure SQL-adatraktár egyike. Győződjön meg arról, hogy alapos megértése.

## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
