<properties
   pageTitle="Adatok betöltése SQL adatraktár bcp segítségével |} Microsoft Azure"
   description="Megtudhatja, milyen bcp, és az esetek adattárolási használatához."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="mausher;barbkess;sonyama"/>


# <a name="load-data-with-bcp"></a>Adatok betöltése a bcp

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Adatok gyári](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)


**[BCP][]** egy parancssori tömeges betöltés segédprogram, amely lehetővé teszi, hogy másolja a vágólapra az adatokat az SQL Server, adatfájlokból és az SQL-adatraktár között. Adatok exportálása az SQL Server-táblák adatok a fájlokba vagy sorok nagyszámú importálja a táblákat az SQL-adatraktár bcp használata Ha használja a queryout lehetőség, kivéve bcp szükséges nem ismeri a Transact-SQL nyelvben.

BCP módja a gyorsan és egyszerűen kisebb adatkészletek lépjen be- és kijelentkezés a adatraktár SQL-adatbázis. A pontos adatmennyiség betöltés/kibontása keresztül bcp ajánlott függ, hogy a hálózat-e az Azure adatközpont kapcsolatot.  Általánosságban elmondható dimenzió táblák töltődik be, és a bcp könnyen kibontása, azonban bcp nem ajánlott betöltése, vagy nagy mennyiségű adattal kibontása.  Polybase betöltése és kiolvasó nagy mennyiségű adattal, ugyanúgy, mint feljebb helyezése SQL adatraktár nagymértékben párhuzamos feldolgozási architektúráját hatékonyabban a javasolt eszköz.

Bcp van lehetősége:

- Adatok betöltése SQL adatraktár a egy egyszerű parancssori segédprogram használatával.
- Egy egyszerű parancssori segédprogrammal adatok kinyerése SQL adatraktár.

Ebből az oktatóanyagból megtudhatja, hogyan szeretné:

- Adatokat importál egy táblázat parancs a bcp használata
- Adatok exportálása egy táblázat uisng a bcp parancsának végrehajtása

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

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
[BCP]: https://msdn.microsoft.com/library/ms162802.aspx
[TÁBLÁZAT létrehozása szintaxisa]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[A Microsoft letöltőközpontból.]: https://www.microsoft.com/download/details.aspx?id=36433
