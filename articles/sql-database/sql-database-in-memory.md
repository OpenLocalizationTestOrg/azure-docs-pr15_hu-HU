<properties
    pageTitle="SQL a memóriában, használatba |} Microsoft Azure"
    description="SQL a memóriában technológiák nagyban tranzakció alapú teljesítményének és analytics-munkaterhelésekből javítása. Megtudhatja, hogy miként e technológiák előnyeit."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Első lépések a memóriában (előzetes verzió) SQL-adatbázisban

A memóriában szolgáltatások nagyban növelheti a tranzakció alapú teljesítményének és a megfelelő esetekben analytics-munkaterhelésekből.

Ez a témakör a két bemutatók, a memóriában OLTP egyet, és egy a memóriában található elemző emeli ki. Minden bemutató megtalálható a lépéseket, és futtassa a videoklip kimutatásadatokat kódot tartalmazó. Lehetőségek közül választhat:

- Változatok megtekintéséhez közötti különbségek teljesítmény eredményét, tesztelje a kód segítségével vagy
- Olvassa el a kód megértéséhez az alkalmazási példát, és megtudhatja, hogy miként hozhat létre, és a memóriában objektumok csatlakozást.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Gyors indítása 1: A memóriában OLTP technológiák gyorsabb az SQL-T teljesítmény](http://msdn.microsoft.com/library/mt694156.aspx) -cikk az első lépésekhez van.

#### <a name="in-memory-oltp"></a>A memóriában OLTP

A memóriában [OLTP](#install_oltp_manuallink) (online tranzakció feldolgozása) szolgáltatásai:

- A memória optimalizálása táblákat.
- A natív össze a tárolt eljárásokat.


A memória optimalizálása tábla rendelkezik egy ábrázolása magát a memóriában aktív, a normál megjelenítését a merevlemezen mellett. A táblázat üzleti tranzakciókat gyorsabbá mivel ezek közvetlenül kapcsolatba csak az aktív memória lévő megjelenítő.

A memóriában OLTP, a érhet el felfelé 30 alkalommal tranzakció átviteli, attól függően, hogy részletei határozzák meg, a terhelést a szükséges.


A natív lefordított tárolt eljárások csak kevesebb gépi utasítást futásidőben, mint a hagyományos értelmezni, tárolt eljárások. Natív fordítási eredménye azt van látható időtartamok, amelyek az 1/100 parancsértelmezős időtartamáról.


#### <a name="in-memory-analytics"></a>A memóriában található elemző 

A memóriában [található elemző](#install_analytics_manuallink) funkció a következő:

Indexek Columnstore javítja analytics és a lekérdezések jelentéskészítés. 


#### <a name="real-time-analytics"></a>Valós idejű Analytics

[Valós idejű](http://msdn.microsoft.com/library/dn817827.aspx) elemzéséhez egyesítése a memóriában OLTP, valamint Analytics:

- Valós idejű üzleti betekintést működési adatok alapján.


#### <a name="availability"></a>Elérhetőség


Kiadás, általános elérhetőség:

- [Indexek Columnstore](http://msdn.microsoft.com/library/dn817827.aspx) , amelyek a *merevlemezen*.


Előzetes verziója esetén:

- A memóriában OLTP
- Valós idejű műveleti Analytics


Szempontok, amíg a memóriában funkciók előzetes leírt [későbbi szakaszát](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Előzetes funkciókról, hogy csak a [*prémium*](sql-database-service-tiers.md) adatbázisok nem rugalmas készletek használható és bármely Basic vagy normál adatbázisoknál nem érhető el.  Rugalmas készletek prémium-adatbázisok támogatása hamarosan. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>VÁLASZOK PARANCSRA. Telepítse a memóriában OLTP minta

A [V12] AdventureWorksLT mintaadatbázis néhány kattintással az [Azure portal](https://portal.azure.com/)által hozhat létre. Ez a szakasz lépései bemutatják, majd hogyan a AdventureWorksLT adatbázis is kiegészítése:

- A memóriában táblákat.
- A natív módon lefordított tárolt eljárás.


#### <a name="installation-steps"></a>Telepítési lépései

1. Az [Azure portál](https://portal.azure.com/)V12 kiszolgálón prémium adatbázis létrehozása. A **forrás** állítsuk be a [V12] AdventureWorksLT adatbázisban.
 - Részletes útmutatásért lásd: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).

2. Csatlakozás SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx)az adatbázist.

3. A [Transact-SQL nyelvben a memóriában OLTP parancsfájl](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) másolja a vágólapra.
 - A T-SQL nyelvben parancsfájl a AdventureWorksLT adatbázisban 1 lépésben létrehozott hoz létre a szükséges a memóriában objektumok.

4. A T-SQL nyelvben parancsprogram beillesztése SSMS, és ezután hajtsa végre a parancsfájlt.
 - Fontos van a `MEMORY_OPTIMIZED = ON` CREATE TABLE záradék, a következőképpen:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Hiba 40536


Ha a hiba jelenik 40536 futtatja a T-SQL nyelvben parancsfájlt, futtassa a következő az SQL-T ellenőrizze, hogy az adatbázis támogatja-e a memóriában:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Eredménye **0** azt jelzi, hogy nem támogatott a memóriában és 1, az azt jelenti, támogatja. A probléma diagnosztizálása:

- Gondoskodjon arról, hogy az adatbázis után a memóriában OLTP funkciók vált aktív előzetes verzióhoz készült.
- Gondoskodjon arról, hogy az adatbázis jön létre a támogatási szolgáltatás réteg.


#### <a name="about-the-created-memory-optimized-items"></a>A létrehozott memória optimalizálása elemeiről

**Táblázatok**: A minta a következő memória optimalizálása táblákat tartalmaz:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Keresztül az **Objektum Explorer** SSMS által a memória optimalizálása táblák megvizsgálhatja:

- Kattintson a jobb gombbal a **táblázatok** > **szűrő** > **Szűrőbeállításokat** > **Memória optimalizált** eredménye 1.


Vagy például kérdezheti le a katalógus nézetek:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**A natív lefordított tárolt eljárás**: SalesLT.usp_InsertSalesOrder_inmem katalógus nézet lekérdezéssel ellenőrizni kell:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>A minta OLTP terhelést futtatása

A következő két *tárolt eljárások* csak különbségét, hogy az első eljárás a memória optimalizálása verziókat táblát használ, miközben a második eljárás lemezen tábláit használja:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


Ebben a szakaszban megjelenik a praktikus **ostress.exe** segédprogram használata stressful szinten két tárolt eljárás végrehajtásához. Összehasonlíthatja a mennyi ideig tart a két terhelési futása befejezéséhez.


Ostress.exe futtatásakor azt javasoljuk, hogy a sikeres paraméterértékeket mindkét verziójához:

- Egyidejű kapcsolatok, nagy számú futtatása segítségével - n100.
- Minden kapcsolat hurok több száz időpontok úgy van használatával - r500.


Azonban érdemes lehet például - n10 sok kisebb értékű indítása és - r 50 annak biztosítására, minden működik.


### <a name="script-for-ostressexe"></a>Ostress.exe parancsfájl


Ebben a szakaszban a saját ostress.exe parancssori beágyazott az SQL-T parancsfájl jeleníti meg. A parancsfájl használja a korábban telepített az SQL-T parancsfájl által létrehozott elemeket.


A következő parancsfájl öt sor elemekkel értékesítési mintát rendelés illeszt be az alábbi memória optimalizálása *tábla*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Győződjön meg az előző T-SQL-ostress.exe _ondisk verzióját, egyszerűen a *_inmem* karakterlánc mindkét előfordulását szeretné cserélje *_ondisk*. Ezek a cseréli hatással tárolt eljárások és a táblázatok nevét.


### <a name="install-rml-utilities-and-ostress"></a>RML segédprogramok és ostress telepítése


Ideális esetben szeretné futtatni ostress.exe az Azure virtuális a. Az [Azure virtuális gép](https://azure.microsoft.com/documentation/services/virtual-machines/) az AdventureWorksLT adatbázis helye azonos Azure földrajzi régióban hozna létre. De futtathatja ostress.exe inkább a laptopján.


A virtuális, illetve bármilyen üzemelteti, válasszon, és telepítse az ismétlés Markup Language (RML) segédprogramok, amelyek ostress.exe lehetnek.

- Tanulmányozza a ostress.exe [a memóriában OLTP mintavállalati](http://msdn.microsoft.com/library/mt465764.aspx)adatbázisban.
 - Illetve nem látom [a memóriában OLTP adatbázist](http://msdn.microsoft.com/library/mt465764.aspx).
 - Vagy tekintse meg [az ostress.exe telepítésére](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Először futtassa a _inmem terhelési terhelést


Egy *RML Cmd parancssor* ablak segítségével futtassa a ostress.exe parancsot. A parancssori kapcsolók a ostress közvetlen:

- 100 kapcsolatok egyidejű futtatásának (-n100).
- Minden kapcsolat, futtassa a T-SQL nyelvben 50 alkalommal (-r 50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


A fenti ostress.exe parancssor futtatása:


1. Az adatbázis-adatok tartalom visszaállítása SSMS bármely előző fut által beszúrt összes adatot törli a következő parancs futtatásával:
```
EXECUTE Demo.usp_DemoReset;
```

2. A fenti ostress.exe parancssor szövegét a vágólapra másolása

3. Cserélje le a `<placeholders>` a paraméterek -S - U -P - d a megfelelő valós értékekkel.

4. Futtassa a szerkesztett parancssori RML Cmd ablakban.


#### <a name="result-is-a-duration"></a>Eredménye a kamatérz


Ostress.exe befejeztével kimenet a végleges sorként futtatása időtartamának ír a RML Cmd ablakot. Ha például egy rövidebb vizsgálat tartott körülbelül 1,5 perc:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Alaphelyzetbe állítása, _ondisk szerkesztése, majd futtassa újra a


Után a _inmem futtatható az eredményt, a Futtatás _ondisk elvégezheti az alábbi lépéseket:


1. Az adatbázis visszaállítása SSMS az előző futtatás által beszúrt összes adatot törli a következő parancs futtatásával:
```
EXECUTE Demo.usp_DemoReset;
```

2. Az összes *_inmem* lecserélése *_ondisk*ostress.exe parancssori szerkesztése.

3. Futtassa újra a második alkalommal ostress.exe, és az időtartam eredmény rögzíthet.

4. Ismét alaphelyzetbe állítása az adatbázist, mi lehet tesztadatokat nagy mennyiségű felelős törlését.


#### <a name="expected-comparison-results"></a>Várható összehasonlítás eredményei

A memóriában teszteket a **9-es időpontok** a teljesítmény javítása a simplistic terhelés mutatott az ugyanabban a Azure régióban az adatbázis-Azure virtuális futó ostress.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B Telepítse a memóriában található elemző minta


Ebben a részben összehasonlítja az IO és statisztika eredményeket index létrehozása columnstore hagyományos b fa index létrehozása és használata esetén.


Valós idejű elemzéséhez a egy OLTP terhelést célszerű gyakran egy fürtözött columnstore index használatát. További részletek: [Columnstore indexek leírt](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Felkészülés a columnstore analytics tesztelése


1. Az Azure portal segítségével a minta friss AdventureWorksLT adatbázis létrehozása.
 - Használja a névnek pontosan.
 - Válassza a minden prémium szolgáltatási réteg.

2. Másolja a vágólapra a [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) .
 - A T-SQL nyelvben parancsfájl a AdventureWorksLT adatbázisban 1 lépésben létrehozott hoz létre a szükséges a memóriában objektumok.
 - A parancsfájl hoz létre, a dimenzió táblában, és a két ténytáblák. A ténytáblák 3.5-ös millió sorok van feltöltve.
 - A parancsprogram 15 percet igénybe vehet.

3. A T-SQL nyelvben parancsprogram beillesztése SSMS, és ezután hajtsa végre a parancsfájlt.
 - Fontos szerepel-e a **COLUMNSTORE** kulcsszó, kattintson a **CREATE INDEX** utasítás szerint:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Kompatibilitási szintet 130 AdventureWorksLT beállítása:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Szint 130 közvetlenül nem kapcsolódó funkciók a memóriában. De szint 130 általában biztosít gyorsabb lekérdezési teljesítmény, mint 120-ra.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Fontos táblák és columnstore indexek


- dbo. FactResellerSalesXL_CCI egy csoportosított **columnstore** indexbe, amely a tömörítési *az szintjén* speciális tartalmazó táblázat.

- dbo. FactResellerSalesXL_PageCompressed egy egyenértékű normál csoportosított index, amely csak a *lap* szintjén tömörített tartalmazó táblát.


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Fontos lekérdezések az columnstore index összehasonlítása


[Itt](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) többféle T az SQL lekérdezés futtatása teljesítménybeli fejlesztések megjelenítéséhez. A 2 a T-SQL nyelvben parancsfájl van egy pár közvetlen érdeklődésére számot tartó lekérdezések. A két lekérdezést egy sorban különböznek egymástól:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Index létrehozása csoportosított columnstore be van kapcsolva a FactResellerSalesXL**_CCI** táblázatot.

A következő az SQL-T parancsfájl kivonat statisztika IO és az idő a lekérdezés minden táblázat nyomtatása


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Kép a memóriában OLTP megfontolandó szempontok


[Aktív előzetes verzió a 2015 október 28](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/)vált, a memóriában OLTP funkciók Azure SQL-adatbázisban.


Az aktuális villámnézeti és a memóriában OLTP csak támogatott:

- Egy *prémium* szolgáltatási réteg az adatbázisokat.

- A memóriában OLTP funkciók után létrehozott adatbázisok vált, az aktív.
 - Új adatbázis nem támogatja a memóriában OLTP, ha a memóriában OLTP funkciók aktív vált, mielőtt létrehozott adatbázisból vissza.


Ha kétségei vannak, mindig futtathatja az alábbi az SQL-T jelölje ki annak megállapítása, hogy az adatbázis támogatja-e a memóriában OLTP. Eredménye **1** azt jelenti, hogy az adatbázis támogatja a memóriában OLTP:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Ha a lekérdezés eredménye **1**, a memóriában OLTP támogatott ebben az adatbázisban, és bármilyen adatbázis másolás és a létrehozott adatbázis visszaállítása az adatbázis alapján.


#### <a name="objects-allowed-only-at-premium"></a>Csak a prémium engedélyezett objektumok


Ha egy adatbázis tartalmaz a következő típusú a memóriában OLTP objektumok vagy típusok közül, Visszalépés a szolgáltatási réteg Premium vagy az egyszerű, vagy a szabványos az adatbázis-ról nem támogatott. Vissza léptetheti az adatbázist, először drop ezeket az objektumokat:

- Táblázatok memória optimalizálása
- A memória optimalizálása táblázat típusai
- A natív lefordított modulok


#### <a name="other-relationships"></a>További kapcsolatok


- Az adatbázisokkal az rugalmas készletek használata a memóriában OLTP funkciók nem támogatott előzetes verzióban.
 - A memóriában OLTP objektumok volt egy rugalmas kvótáját vagy tartalmazó adatbázis áthelyezése, kövesse az alábbi lépéseket:
  - 1. Bármely memória optimalizálása táblázatok, a tábla típusú és a natív módon lefordított az SQL-T modulok húzza az adatbázisban
  - 2. A szolgáltatás réteg szabványos az adatbázis módosítása
  - 3. Az adatbázis áthelyezése a rugalmas készletben

- A memóriában OLTP használatával az SQL adatraktár nem támogatott.
 - A memóriában található elemző columnstore index jellemzője az SQL adatraktár támogatott.

- A lekérdezés tároló belső natív módon lefordított modulok lekérdezések nem rögzítése lehetőséget.

- Néhány Transact-SQL-funkciók nem támogatottak a memóriában OLTP együtt. Ez a Microsoft SQL Server és a Azure SQL-adatbázis vonatkozik. A részletekért lásd:
 - [A memóriában OLTP Transact-SQL nyelvben támogatása](http://msdn.microsoft.com/library/dn133180.aspx)
 - [A Transact-SQL nyelvben tartalmazza a memóriában OLTP által nem támogatott](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Következő lépések


- Próbálja meg [használata a memóriában OLTP egy meglévő Azure SQL-alkalmazásban.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>További források

#### <a name="deeper-information"></a>Részletesebb információk

- [További tudnivalók a memóriában OLTP, amelyben mind a Microsoft SQL Server Azure SQL-adatbázis vonatkozik:](http://msdn.microsoft.com/library/dn133186.aspx)

- [További tudnivalók: msdn valós idejű műveleti Analytics](http://msdn.microsoft.com/library/dn817827.aspx)

- A [közös terhelést mintázatok és az áttelepítés előtt megfontolandó kérdések](http://msdn.microsoft.com/library/dn673538.aspx), amely leírja, ahol a memóriában OLTP gyakran rendelkezik teljesítménybeli nyereség terhelést mintázatok követelményeinek.

#### <a name="application-design"></a>Alkalmazás tervezése

- [A memóriában OLTP (a memóriában optimalizálás)](http://msdn.microsoft.com/library/dn133186.aspx)

- [A meglévő Azure SQL-alkalmazások használata a memóriában OLTP.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Eszközök

- [Az SQL Server Data Tools előzetes (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), hogy havi legújabb verziója.

- [Az SQL Server az ismétlés Markup Language (RML) segédprogramok leírása](http://support.microsoft.com/en-us/kb/944837)

- A memóriában OLTP [monitor a memóriában tároló](sql-database-in-memory-oltp-monitoring.md) .

