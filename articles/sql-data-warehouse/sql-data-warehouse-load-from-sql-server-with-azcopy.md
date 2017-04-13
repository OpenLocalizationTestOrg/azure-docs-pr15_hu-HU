<properties
   pageTitle="Adatok betöltése Azure SQL-adatraktár (PolyBase) az SQL Server |} Microsoft Azure"
   description="Adatok exportálása az SQL Server egyszerű fájlok AZCopy Azure blob-tárolóhoz adatokat importálhatja, és szeretné az adatokat ingest az Azure SQL-adatraktár PolyBase bcp használja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Adatok betöltése Azure SQL-adatraktár (AZCopy) az SQL Server

Adatok betöltése az SQL Server Azure blob-tárolóhoz bcp és AZCopy parancssori segédprogramok használata Ezután segítségével PolyBase vagy Azure Data Factory az adatok betöltése Azure SQL-adatraktár. 


## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban lépéseinek, van szükség:

- Adatraktár SQL-adatbázis
- A telepített bcp parancssori segédprogram
- A telepített SQLCMD parancssori segédprogram

>[AZURE.NOTE] A bcp és sqlcmd segédprogramok letöltheti a [Microsoft letöltőközpontból][].

## <a name="import-data-into-sql-data-warehouse"></a>Adatok importálása az SQL adatraktár

Ebben az oktatóanyagban tábla létrehozása az Azure SQL-adatraktár, és az adatok importálása az a táblázat.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Lépés: 1: Azure SQL-adatraktár táblázat létrehozása

A parancssorból sqlcmd segítségével táblázatot szeretne létrehozni a példányon az alábbi lekérdezés futtatása:

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

>[AZURE.NOTE] Lásd a [Táblázat áttekintése][] vagy a [CREATE TABLE szintaxis][] hozzon létre egy tábla SQL adatraktár a WITH záradékban elérhető további információt.

### <a name="step-2-create-a-source-data-file"></a>Lépés: 2: Forrás adatfájl létrehozása

Nyissa meg a Jegyzettömb, és a következő sornyi adatot másol egy új szövegfájlt, és mentse a fájlt a helyi C:\Temp\DimDate2.txt temp könyvtár.

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

> [AZURE.NOTE] Fontos, hogy ne feledje, hogy bcp.exe nem támogatja a UTF-8 fájl kódolást. Használjon ASCII vagy UTF-16 kódolású fájlokat bcp.exe használata esetén.

### <a name="step-3-connect-and-import-the-data"></a>Lépés 3: Csatlakozás és az adatok importálása
Bcp használja, csatlakoztassa és importálni az adatokat, az alábbi paranccsal az értékek lecserélése:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Ellenőrizheti, hogy az adatok betöltése sqlcmd használja az alábbi lekérdezés futtatásával:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Meg kell a következő eredményeket adják:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Lépés: 4: Az újonnan betöltött adatok statisztikai létrehozása

Azure SQL-adatraktár támogatási automatikus létrehozása és az automatikus frissítés statisztika még nem tartalmaz. Jelentős módosításokat fordul elő, az adatok vagy annak érdekében, hogy a legjobb teljesítmény elérése érdekében a lekérdezések juthat, fontos, hogy statisztika hozható létre az összes tábla összes oszlop az első betöltése után. Statisztikai részletes leírását témakört [Statisztika][] témakörök fejlesztése csoportjában található. Az alábbi képen egy gyors példa bemutatja, hogyan hozhat létre betöltött ebben a példában a táblázatos statisztikák

Hajtsa végre a következő létrehozása statisztika kimutatások sqlcmd parancssorból:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Adatok exportálása SQL adatraktár
Ebben az oktatóanyagban létrehoz egy adatfájlt SQL adatraktár táblázatból. Azt a fenti létrehozott adatok exportálása egy új adatfájlt DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Lépés: 1: Az adatok exportálása

A bcp segédprogrammal csatlakozás és exportálása a következő, az értékek lecserélése parancsot:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Ellenőrizheti, hogy az adatok exportálása megfelelően történt az új fájl megnyitásával. A fájlban lévő adatokat egyeznie kell a szöveget, az alábbi:

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

>[AZURE.NOTE] Elosztott rendszerek jellegét, hogy az adatok rendelés nem lehet azonos adatraktár SQL-adatbázisok között. Egy másik, hogy a függvénnyel **queryout** bcp, írja be a lekérdezés venni a teljes táblázat exportálása helyett.

## <a name="next-steps"></a>Következő lépések
Betöltése áttekintést talál [az SQL adatraktár adatok betöltése][].
Fejlesztési vonatkozó további tippek [SQL adatraktár fejlesztési áttekintése][]című témakörben találhat.

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
