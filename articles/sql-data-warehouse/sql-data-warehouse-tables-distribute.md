<properties
   pageTitle="Táblákat az SQL adatraktár terjesztése |} Microsoft Azure"
   description="Első lépések az Azure SQL-adatraktár táblák terjesztése."
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
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Az SQL adatraktár táblák terjesztése

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Adattípusok][]
- [Terjesztése][]
- [Index][]
- [Partition][]
- [Statisztika][]
- [Ideiglenes][]

SQL-adatraktár egy nagymértékben párhuzamos feldolgozási (MPP) adatbázis rendszer meghatározva.  Adatok osztani és videofunkcióinak feldolgozása több csomópontok keresztül, SQL adatraktár is kínálhatnak, nagyon nagy méretezhetőség - bármely egyetlen rendszer túl távol.  Annak eldöntésében, hogy hogyan terjesztheti az adatok a SQL adatraktár belül az egyik legfontosabb tényezők optimális teljesítmény eléréséhez.   Az optimális teljesítmény billentyűvel van minimalizálása adatok mozgását, és az adatok mozgását minimalizálása billentyűvel jelölje ki a megfelelő terjesztési stratégia.

## <a name="understanding-data-movement"></a>Adatok mozgását ismertetése

Egy MPP-rendszerben az adatok minden táblából több mögöttes adatbázisok közötti próbál osztani.  A legtöbb optimalizált lekérdezések egy MPP rendszeren egyszerűen továbbíthatók keresztül végrehajtani a az egyes elosztott adatbázisokat a másik adatbázis közötti beavatkozás nélküli.  Ha például vegyük értékesítési adatok, amely tartalmazza a két tábla, értékesítési és ügyfelek adatbázis.  Ha az értékesítési táblában csatlakozzon, ahol az ügyfelek tábla kell, hogy a lekérdezés, és meg nullával való osztás az értékesítési és az ügyfél táblák felfelé vevő számát, ügyfelek illusztráló egy másik adatbázist, értékesítési és az ügyfél Bekapcsolódás lekérdezések megoldhatók belül minden nem ismeri a többi az adatbázisok adatbázis.  Viszont ha az értékesítési adatok elosztva rendelés számának és a felhasználói adatok vevő számát, majd bármilyen adott adatbázist nem lesz adatok az ügyfelek, és így az értékesítési adatok csatlakoztatása a felhasználói adatok volna kimutatásadatokat ügyfelek beolvassa az adatokat a többi adatbázisok.  A második példában adatok mozgását kellene a felhasználói adatok áthelyezése az értékesítési adatok fordul elő, hogy a két tábla is csatlakozhat.  

Adatok mozgását nem mindig hibás valamilyen dolog, és előfordul, hogy oldja meg a lekérdezés szükség.  De elkerülhetők legyenek a felesleges ezt a lépést, ha természetes a lekérdezés gyorsabban futnak.  Adatok mozgás leggyakrabban merül fel, táblák adatbázis vagy összesítések végrehajtásának sorrendjét.  Gyakran kell tennie is, így előfordulhat, hogy nem készíthet illesztés, például egy esethez optimalizálása van szüksége segít a többi eset, például egy összesítést megoldása adatok mozgását.  A trükk az tudja meg, amely olyan, hogy kevesebb munkát.  A legtöbb esetben egy gyakran illesztett oszlopot a nagy ténytáblák terjesztéséhez a leghatékonyabb mód csökkentése érdekében a legtöbb adatok mozgását.  Csatlakozás oszlopok adatok terjesztéséhez csökkentheti az adatok mozgását, mint egy összesítést részt oszlopok adatok terjesztése sokkal több közös módszert.

## <a name="select-distribution-method"></a>Telepítési mód kiválasztása

A háttérben az SQL adatraktár az adatok osztja 60 adatbázisok.  Minden egyes adatbázis egy **terjesztési**nevezik.  Adatok az egyes táblázatokba betöltésekor SQL adatraktár tartalmaz, az adatok osztása át ezeket 60 terjesztését kíváncsi.  

A terjesztési módszert a táblázat szinten van definiálva, és jelenleg két lehetőség közül választhat:

1. **Ciklikus** amely adatok terjesztése egyenletesen, de véletlenszerűen.
2. **Kivonat elosztott** amely elosztja az adatokat a kivonatolás értékeit egyetlen oszlop alapján

Alapértelmezés szerint nem megadhatja az eloszlás módot, ha a táblázat jut el a terjesztési **ciklikus** módszerrel.  Azonban, egyre bonyolultabb a végrehajtása során, érdemes kipróbálnia **kivonat elosztott** táblák adatok mozgását, amely viszont optimalizálja a lekérdezési teljesítmény minimalizálásához.

### <a name="round-robin-tables"></a>Ciklikus táblák

Ciklikus módszerrel adatok elosztása nagyon hogyan úgy tűnik, hogy.  Adatok betöltése, mint minden egyes sorára egyszerűen a következő eloszlás küldi.  Ez a módszer elosztása az adatokat fog véletlenszerűen mindig az adatok nagyon egyenletes elosztása összes a terjesztését.  Ez azt jelenti, hogy nem nincs rendezés munkavégzés a ciklikus során, amely az adatokat helyezi el.  Ciklikus eloszlás emiatt a véletlen kivonat nevezik.  Ciklikus elosztott táblával nincs szükség az adatok megértéséhez.  Emiatt ciklikus gyakran táblázatokkal jó betöltése célokat.

Alapértelmezés szerint nincs terjesztési módszert választja, ha a ciklikus terjesztési módszerrel történik.  Azonban közben ciklikus táblák könnyen használható, mert az adatok véletlenszerűen elosztva a rendszer, az azt jelenti, hogy a rendszer nem garantálja az milyen terjesztési minden sor be van kapcsolva.  Emiatt a rendszer előfordul, hogy egy adatok mozgását műveletet az adatok hatékonyabb rendszerezéséhez, mielőtt megoldhatja a lekérdezés meghívása kell.  Ebben a lépésben további lassíthatják a lekérdezések.

Fontolja meg inkább ciklikus terjesztési a táblázat az alábbi esetekben:

- Ha egyszerű kiindulási pontként első lépések
- Ha nincs egyértelmű csatlakozó kulcs
- Ha nem a jó jelölt oszlopban a táblázat összes kivonat
- Ha a táblázat nem osztja meg egy közös illesztés kulcs más táblázatokkal
- Ha más illesztések a lekérdezés-nél kisebb jelentősen az illesztés
- A táblázat ideiglenes átmeneti tárolásra szolgáló táblázat esetén

Ezek a példák a létrehoz egy ciklikus táblát:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Ciklikus pedig az alapértelmezett táblázat típus alatt a DDL a közvetlen javasoljuk egy, amelyek a táblázat elrendezése szándékának törlése másoknak.

### <a name="hash-distributed-tables"></a>Az elosztott táblák kivonat

A táblázatok terjesztheti **kivonat elosztott** algoritmus segítségével is teljesítmény javítása sok felhasználási területei adatok mozgását csökkentésével lekérdezés időben.  Elosztott kivonat táblák táblákat, amelyek ujjlenyomat algoritmus segítségével egyetlen oszlop, amely, jelölje be a megosztott adatbázis közötti vannak csoportosítva.  Az eloszlás oszlop Mitől függ, hogyan végig a megosztott adatbázisokat az adatokat próbál osztani.  A kivonat függvény az eloszlás oszlop használja a sorok hozzárendelése terjesztését.  A ujjlenyomat algoritmus és az eredményül kapott terjesztési mérvadó.  Ennek az az azonos adattípusúnak értéket mindig fel az azonos eloszlás.    

Ez a példa létrehoz egy táblát, elosztott azonosító:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Terjesztési oszlop kijelölése

Ha úgy dönt, hogy **kivonat terjesztése** táblázat szüksége lesz jelöljön ki egy terjesztési egyetlen oszlopot.  Terjesztési oszlop kiválasztásakor tényező három fő is célszerű figyelembe venni.  

Jelölje be a program egyetlen oszlop:

1. Nem frissülnek
2. Adatok egyenletesen, terjesztése, amellyel elkerülheti a ferdeség adatok
3. Adatok mozgását kisméretűvé alakítása

### <a name="select-distribution-column-which-will-not-be-updated"></a>Jelölje ki a terjesztési oszlop, amely nem frissülnek

Terjesztési oszlopok nem frissíthető, ezért, jelölje be a statikus értékeket tartalmazó oszlop.  Ha egy oszlop van szükség lehet frissíteni, még általában nem egy jó terjesztési jelölt.  Ha egy olyan eset, ahol frissíteni kell a terjesztési oszlop, ez a sor első törlése, majd kattintson az új sor beszúrása elvégezhető.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Jelölje ki a terjesztési oszlop, amely a egyenletes elosztása adatok

Mivel elosztott rendszer csak olyan gyorsan, mint a legkisebb terjesztési hajt végre, célszerű a munkát osztja fel egyenletesen végig a felosztás megtörtént a rendszerben kiegyensúlyozott adatvégrehajtás eléréséhez.  A munka oszlik elosztott rendszeren módon ahol minden egyes terjesztési adatait él alapul.  Emiatt a nagyon fontos, jelölje ki a megfelelő terjesztési oszlop összes az adatokat, így minden egyes terjesztési egyenlő munkát, és annak részét a munka elvégzéséhez az azonos időt vesz igénybe.  Munka jól oszlik végig a rendszer, amikor a program az adatokat át a felosztás meghatározni.  Adatok párosítva egyenletesen van, hogy hívja fel, a **ferdeség adatok**.  

Egyenletesen osztása adatokat, és a ferdeség adatok elkerüléséhez, vegye figyelembe a következőket, ha kijelöli a terjesztési oszlopot:

1. Jelölje ki a nagyszámú egyedi értékeket tartalmazó oszlop.
2. Kerülje a terjesztése néhány egyedi értékeket tartalmazó oszlop adatait. 
3. Kerülje a terjesztése oszlopok NULL értékek nagy gyakorisággal adatait.
4. Kerülje a terjesztése dátumoszlopok adatokat.

Mivel az egyes értékek – 1 60 terjesztését kivonatolt, akár terjesztési eléréséhez kívánt jelöljön ki egy oszlopot, amely erősen egyedi, és több mint 60 egyedi értékeket tartalmaz.  Ábrázolására, fontolja meg egy olyan esetben, ahol oszlop csak akkor van 40 egyedi értékeket.  Ebben az oszlopban a terjesztési kulcsként választotta, ha a táblázat adatait szeretné land 40 terjesztését a legfeljebb 20 terjesztését elhagyása nincsenek adatok, és nincs elvégzendő feldolgozás.  A többi 40 terjesztését kellene viszont a több munkához a teendő, ha az adatok egyenletesen történt terjednek több mint 60 terjesztését.  Ebben az esetben az adatok ferdeség képen.

MPP-rendszerben minden egyes lekérdezéslépés megvárja, amíg az összes terjesztését a megosztás a munka elvégzéséhez.  Ha egy terjesztési további munkavégzés van, mint a többi, majd a más terjesztését az erőforrás vannak lényegében eltérő csak várakozás a elfoglalt eloszlás.  Munka nem egyenletesen kiterjed összes terjesztését át, amikor hívása a **ferdeség feldolgozása**.  Feldolgozási ferdeség okoz lassabban fut, ha a terhelést a is egyenletesen elosztva végig a felosztás mint lekérdezéseket.  Ferdeség adatok vezet ferdeség feldolgozása.

Kerülje a terjesztése erősen nullable oszlopra, a null értékek fogja az összes léphet az azonos eloszlás. Egy dátumot tartalmazó oszlopból a terjesztése is okozhatják feldolgozás ferdeség, mivel az egy adott dátumhoz tartozó összes adat fog léphet az azonos eloszlás. Ha több felhasználó végrehajtása lekérdezések minden szűrés a ugyanazt a dátumot, majd a 60 terjesztését csak 1 lesz módon, az összes munkáját egy adott dátum óta csak a is egy terjesztési. Ebben az esetben a lekérdezések valószínűséggel 60 időpontok, ha az adatok volt egyenletesen elosztva összes a felosztás lassabb fog futni. 

Ha jó jelölt oszlopot tartalmaz, akkor érdemes ciklikus a terjesztési szolgál.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Jelölje ki a terjesztési oszlop, amely az adatok mozgását fog kisméretűvé alakítása

Adatok mozgását minimalizálása jelöljön ki a megfelelő terjesztési oszlop az egyik azok a legfontosabb stratégiák optimalizálása a SQL adatraktár teljesítményét.  Adatok mozgás leggyakrabban merül fel, táblák adatbázis vagy összesítések végrehajtásának sorrendjét.  A használt oszlopok `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` és `HAVING` záradékok az összes jelölőnégyzet bejelölésével **jó** szolgálatot tesznek az eloszlás kivonat. 

A többi készlet oszlopai a `WHERE` záradék do **nem** jó kivonat oszlop szolgálatot tesznek az korlátozzák mely terjesztését részt a lekérdezéshez, mert ferdeség feldolgozás okoz.  Egy olyan oszlopra, amely lehet, hogy a osztania tempting, de gyakran okozhatják a ferdeség feldolgozás jó példája egy dátumot tartalmazó oszlopból.

Általánosságban elmondható Ha két nagy ténytáblák gyakran érintett illesztés, érheti el a legtöbb teljesítmény azáltal, hogy az illesztés oszlopok egyik mindkét tábla.  Ha egy olyan táblát, soha nem csatlakozik egy másik nagy ténytábla, keresse meg az oszlopokat, amelyeket gyakran szerepel a `GROUP BY` záradék.

Létezik néhány fontosabb feltételeket, amelyek az illesztés alatt adatok mozgását elkerülésére teljesülnie kell:

1. Az illesztés érintett táblákat **egy** vesz részt az illesztés oszlopot a normális eloszlású kivonat kell lennie.
2. Az illesztési oszlopok adattípusát egyeznie kell a két tábla között.
3. Az oszlopok az egyenlő operátorral tartományhoz kell csatlakoznia.
4. Az illesztés típusát nem lehet egy `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Ferdeség hibaelhárítási adatok

Továbbításakor táblázatok adatait a kivonat terjesztési módszerrel van, hogy bizonyos terjesztését ferde kell-e aránytalanul több, mint a többi adatot lehetőséget. Fölösleges adatok ferdeség hatással lehet a lekérdezési teljesítmény, mivel a megosztott lekérdezés végleges eredménye várja meg, a leghosszabb futó eloszlás befejezéséhez. Attól függően, hogy az adatok ferdeség fokú szükség lehet foglalkozik.

### <a name="identifying-skew"></a>Ferdeség azonosítása

Adjon meg egy táblát, mint a ferde egyszerűen környezetbe `DBCC PDW_SHOWSPACEUSED`.  Ennek módja a nagyon gyorsan és egyszerűen szeretné látni, amelyeket az adatbázis 60 elosztása az egyes sorok számát.  Ne feledje, hogy a legkiegyensúlyozottabb teljesítmény, a elosztott táblázat soraira kell egyenletesen át az összes terjesztését.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Ha a lekérdezés az Azure SQL-adatraktár dinamikus kezelése nézetek (DMV) részletesebb elemzése hajthatják végre.  Szeretné kezdeni, használja a [Táblázat áttekintése][áttekintése] cikkből az SQL nézet [dbo.vTableSizes][] létrehozása  Amikor létrejött a nézetet, futtassa a táblákat lehet több, mint 10 %-os adatok ferdeség azonosítása ezt a lekérdezést.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Ferdeség adatok megoldása

Az összes ferdeség van-e elegendő, hogy egy fix.  Bizonyos esetekben a táblázat bizonyos lekérdezések teljesítményének a ferdeség adatok kárt is lényegesebbnek.  Döntse el, ha meg kell megoldása adatok egy táblázatban, ferdeség ismernie kell lehető a adaton és lekérdezésekkel kapcsolatos adatoknak a terhelést a.   A ferdeség hatásait vizsgálata egy úgy, hogy a [Lekérdezés ellenőrzése][] cikk ismertetett lépésekkel hozhatja létre Lync-befejezéséhez kattintson az egyes terjesztését készítése mennyi lekérdezések hatása a lekérdezési teljesítmény és kifejezetten ferdeség hatását.

Adatok terjesztéséhez frissítését és adatok ferdeség minimalizálása és a kis méretre adatok mozgását közötti jobb egyensúly találja. Ezek is ellentétes célok, és előfordul, hogy Ön fog szeretné megőrizni ferdeség adatok mozgását csökkentése érdekében. Például az eloszlás oszlop esetén gyakran illesztések és összesítések megosztott oszlopára, fog kell minimalizálása adatok mozgását. A minimális adatok mozgását előnyös előfordulhat, hogy lényegesebbnek ferdeség adatok hatását. 

A szokásos megoldása adatok ferdeség módja hozza létre újból a egy másik terjesztési oszlopot tartalmazó táblázat. Mivel nincs mód módosítása a terjesztési oszlopban, a meglévő táblához, a táblázat eloszlását módosítása úgy, hogy egy [CTAS] szögletes hozza létre újból.  Két példa arra, hogy miként megoldása adatok ferdeség az alábbiakban:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Példa: 1: Hozza létre újból a tábla egy új terjesztési oszloppal

Ez a példa a [[CTAS]] hozza létre újból a különböző kivonat terjesztési oszlopot tartalmazó táblázat. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Példa 2: Hozza létre újból a táblázat ciklikus eloszlás használatával

Ez a példa [CTAS], [] hozza létre a kivonat eloszlás helyett ciklikus tartalmazó táblázatot. Ez a változás még adatok terjesztési de megnövelt adatok mozgás hoznak létre. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Következő lépések

Ha többet szeretne megtudni a Táblázateszközök Tervezés, olvassa el a [elosztás][], [Index][], [partíciót][], [Adattípusok][], [Statisztika][] és [Ideiglenes táblák][ideiglenes] cikkek című témakört.

Ajánlott eljárások áttekintése lásd: az [SQL adatok raktári gyakorlati tanácsok][].


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
[Lekérdezés figyelése]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
