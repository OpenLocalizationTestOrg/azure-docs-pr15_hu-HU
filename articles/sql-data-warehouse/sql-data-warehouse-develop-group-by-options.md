<properties
   pageTitle="Csoportosítási beállítások a SQL adatraktár |} Microsoft Azure"
   description="Ötletek a csoport beállításai az Azure SQL-adatraktár megoldások fejlesztésével."
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

# <a name="group-by-options-in-sql-data-warehouse"></a>Csoportosítási szempont SQL adatraktár beállításai

A [GROUP BY][] záradék használatával egy összesítő sorok összesített adatok szolgál. Azt is, amely azt a funkciókat, amelyek ledolgozott köré, mivel ezek közvetlenül által nem támogatott Azure SQL-adatraktár kell kiterjesztése lehetőségekből választhat.

Ezek a beállítások
- CSOPORTOSÍTÁSI szempont összesítő
- CSOPORTOSÍTÁS BEÁLLÍTÁSA
- CSOPORTOSÍTÁSI szempont kocka

## <a name="rollup-and-grouping-sets-options"></a>Összegző és csoportosítási beállítások megadása
A legegyszerűbb lehetőséget `UNION ALL` helyette az összegző végrehajtásához helyett a közvetlen szintaxis használna. Eredménye megegyezik a pontosan

Az alábbi képen a kimutatás csoport a `ROLLUP` beállítást:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

ROLLUP használatával azt kérték a következő összesítések:
- Országban és régióban
- Ország
- Végösszeg

Felülírja ezt meg kell használni `UNION ALL`; az összesítések megadása kötelező explicit módon való visszatéréshez ugyanazt az eredményt:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Csoportosítás eredménye összes szükség végezze el a elfogadják azonos egyszerű, de csak az a csoportosítási szintek azt szeretné, hogy az UNION ALL szakaszok létrehozása

## <a name="cube-options"></a>Kocka beállításai
Érdemes lehet létrehozni, a csoport által a KOCKA az UNION ALL megközelítés használatával. A probléma oka, hogy a kód gyorsan előfordulhat, hogy lehető és kezelhetővé vált. Csökkentésében ezt a megközelítést további speciális használhatja.

A fenti példa használata.

Első lépésként a "kocka", amely definiálja az összesítést, hogy szeretne létrehozni a szintek megadásához. Fontos ajánljuk figyelmébe a CROSS JOIN származtatott két táblát. A minden szintű nekünk hoz létre. A kódot a többi van valójában a formázást.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

A CTAS eredményeinek alatti is láthatja:

![][1]

A második lépés, megadhatja az ideiglenes eredményeinek tárolása cél táblához:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

A harmadik lépésként fölé a kockában elvégzéséhez az összesítés oszlopok ismétlése. A lekérdezés #Cube ideiglenes tábla minden sorához egyszer futtatja és az eredmények tárolja az #Results temp táblázatban

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Végül azt úgy térhet vissza az eredmények a #Results ideiglenes táblából egyszerűen olvasása

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

A kód feldarabolása szakaszokra, és egy körkörös szerkezet létrehozása a kódot a kezelhető, és maintainable lesz.


## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CSOPORTOSÍTÁSI SZEMPONT]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
