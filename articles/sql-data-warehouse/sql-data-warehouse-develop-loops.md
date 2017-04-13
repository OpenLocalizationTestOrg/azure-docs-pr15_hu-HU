<properties
   pageTitle="Az SQL adatraktár hurkok |} Microsoft Azure"
   description="Tippek a Transact-SQL nyelvben hurkok és a megoldások fejlesztésére az Azure SQL-adatraktár tagjára kurzorok."
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

# <a name="loops-in-sql-data-warehouse"></a>Az SQL adatraktár hurkok
SQL adatraktár [közben][] le többször a blokkok utasítás végrehajtása támogatja. Ez továbbra is a mindaddig, amíg a megadott feltételek teljesülése esetén, vagy addig, amíg a kód kifejezetten megszakítja a leállításig használata a `BREAK` kulcsszó. Hurkok különösen hasznosak cserélje ki az SQL-kódot definiált kurzorok. SQL-kódot írt szinte minden kurzorok vannak, az előretekerés Szerencsére, olvassa el a csak a különböző. Így [közben] hurkok egy kiváló megoldás, ha már útvesztőjében, hogy felülírni.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Hurkok vizsgál, és az SQL adatraktár kurzorok cseréje
Azonban előtt először vezetője van kérdezze meg saját maga a következő kérdés: "Sikerült a kurzor újból írni alapuló műveletek használata?". Sok esetben a válasz lesz az Igen gombra, és a legjobb megközelítés legtöbbször. A set-alapú művelet gyakran jelentősen gyorsabban közelítéses, sorról sorra megközelítés hajt végre.

Előretekerés csak olvasható kurzorok egyszerűen egy körkörös szerkezet lecserélhető. Az alábbi van egy egyszerű példa. Ez a példa kód minden az adatbázis táblájához statisztikájának frissíti. Azt le a táblák fölé léptetés által is tudják a minden parancs végrehajtása sorrendben.

Első lépésként az egyes kimutatások azonosítja egyedi sor számot tartalmazó ideiglenes tábla létrehozása:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Második inicializálni végrehajtásához le változó figyelembevételével:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Most ismétlése a kimutatásokban, hajtsa végre azokat egy fölé egyszerre:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Végül húzza az első lépésben létrehozott ideiglenes tábla

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[KÖZBEN]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
