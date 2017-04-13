<properties
   pageTitle="Adatok betöltése Azure SQL-adatraktár (bcp) az SQL Server |} Microsoft Azure"
   description="A kis adatok méret használja bcp adatok exportálása az SQL Server strukturálatlan fájlokat, és importálja az adatokat közvetlenül Azure SQL-adatraktár."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Adatok betöltése Azure SQL-adatraktár (egyszerű, strukturálatlan fájl) az SQL Server

> [AZURE.SELECTOR]
- [AZ SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

A kis adatkészletek majd töltse be közvetlenül az Azure SQL-adatraktár és az adatok exportálása az SQL Server a bcp parancssori segédprogramot is használhatják.

Ebben az oktatóanyagban használandó bcp szeretne:

- A táblázat exportálása az SQL Server a bcp parancs használatával (vagy hozzon létre egy egyszerű példa fájlt)
- A tábla importálása strukturálatlan fájlhoz SQL adatraktár.
- Hozzon létre statisztika betöltött adatokat.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Első lépések

### <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban lépéseinek, van szükség:

- Adatraktár SQL-adatbázis
- A telepített bcp parancssori segédprogram
- A telepített sqlcmd parancssori segédprogram

A bcp és sqlcmd segédprogramok letöltheti a [Microsoft letöltőközpontból][].

### <a name="data-in-ascii-or-utf-16-format"></a>ASCII- vagy UTF-16 adatokkal

Ha a saját adatain próbálja ebben az oktatóanyagban, az adatok kell az ASCII- vagy UTF-16 kódolás használata óta bcp UTF-8 nem támogatja. 

PolyBase UTF-8 támogatja, de még nem támogatja a UTF-16. Fontos tudni, hogy tetszés szerint kombinálhatják a bcp PolyBase meg fog átalakítására UTF-8 után azt az SQL Server exportálja. 


## <a name="1-create-a-destination-table"></a>1. a céltábla létrehozása

Táblázat, amely a célként használt táblát a betöltés az SQL-adatraktár definiálása. A táblázat oszlopainak meg kell egyeznie az adatfájl minden egyes sor adatait.

Hozzon létre egy táblázatot, nyisson meg egy parancssort, és futtassa a következő parancsot a sqlcmd.exe használatával:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. a forrás-adatfájl létrehozása

Nyissa meg a Jegyzettömb, és a következő sornyi adatot másol egy új szövegfájlt, és mentse a fájlt a helyi C:\Temp\DimDate2.txt temp könyvtár. Ezeket az adatokat a ASCII-formátumban van.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Nem kötelező) A saját adatok exportálása az SQL Server-adatbázishoz, nyisson meg egy parancssort, és futtassa a következő parancsot. A saját adatokkal cserélje le a táblanév, kiszolgálónév, adatbázisnév, felhasználónév és jelszó.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. a adat betöltése
Töltse be az adatokat, nyisson meg egy parancssort, és futtassa a következő parancsot, és az értékek cseréje a kiszolgáló nevét, a adatbázis nevét, a felhasználónév és a jelszavát a sajátjaira.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Ellenőrizze, hogy az adatok betöltése megfelelően történt a parancs segítségével

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Az eredmények így néz ki:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

## <a name="4-create-statistics"></a>4. a statisztika létrehozása

SQL-adatraktár támogatási automatikus létrehozása vagy az automatikus frissítés statisztika még nem tartalmaz. Úgy juthat az ajánlott a lekérdezési teljesítmény, fontos, statisztikai adatokat hozhat létre az összes tábla összes oszlop az első betöltése után vagy jelentős módosításokat fordulhat elő, az adatok. Statisztikai részletes leírást lásd: [statisztikai adatokat][]. 

Futtassa a következő parancsot, statisztikai adatokat hozhat létre az újonnan betöltött táblában.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. SQL-adatraktár adatokat exportálni
A megvalósítás exportálhatja az adatokat vissza kívül SQL adatraktár betöltött.  A parancs az exportálandó nem pontosan ugyanaz, mint az SQL Server exportálása.

Jó helyen jár a különbség a találatok között van. Mivel az adatok tárolása SQL adatraktár, belül megosztott helyeken adatok exportálásakor az egyes számítási csomópontok azt adatot ír a kimeneti fájl. Az adatok, a kimeneti fájl sorrendjének valószínűleg ugyanaz, mint a bemeneti fájlban lévő adatok sorrendjét.

### <a name="export-a-table-and-compare-exported-results"></a>A táblázat exportálása, és hasonlítsa össze az exportált eredménye

Lásd: az exportált adatokat, nyisson meg egy parancssort, és a saját paramétereket parancsot. Kiszolgálónév a logikai Azure SQL-kiszolgáló neve.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Ellenőrizheti, hogy az adatok exportálása megfelelően történt az új fájl megnyitásával. A fájlban lévő adatokat egyeznie kell a szöveget, az alábbi, de eltérő sorrendű valószínűleg is rendezhetők:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-the-results-of-a-query"></a>A lekérdezés eredményének exportálása

A bcp **queryout** függvény segítségével helyett a teljes táblázat exportálása lekérdezés eredményének exportálása. 

## <a name="next-steps"></a>Következő lépések
Betöltése áttekintést talál [az SQL adatraktár adatok betöltése][].
Fejlesztési vonatkozó további tippek [SQL adatraktár fejlesztési áttekintése][]című témakörben találhat.
Lásd: [Táblázat áttekintése][] vagy [táblázat létrehozása szintaxisát][] SQL adatraktár a táblázatokról további információt.

<!--Image references-->

<!--Article references-->

[Adatok betöltése SQL adatraktár]: ./sql-data-warehouse-overview-load.md
[SQL-adatraktár fejlesztése – áttekintés]: ./sql-data-warehouse-overview-develop.md
[Táblázat áttekintése]: ./sql-data-warehouse-tables-overview.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[TÁBLÁZAT létrehozása szintaxisa]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[A Microsoft letöltőközpontból.]: https://www.microsoft.com/download/details.aspx?id=36433
