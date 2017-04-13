<properties
   pageTitle="Adatok betöltése Azure SQL-Databaase (bcp) CSV-fájlból |} Microsoft Azure"
   description="Adatok importálása az Azure SQL-adatbázis használja egy kis adatok mérete bcp."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Adatok betöltése Azure SQL-adatraktár (egyszerű, strukturálatlan fájl) CSV

A bcp parancssori segédprogram segítségével adatokat importálhat egy CSV-fájl Azure SQL-adatbázis.

## <a name="before-you-begin"></a>Első lépések

### <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban lépéseinek, van szükség:

- Egy logikai Azure SQL-adatbázis-kiszolgáló és az adatbázis
- A telepített bcp parancssori segédprogram
- A telepített sqlcmd parancssori segédprogram

A bcp és sqlcmd segédprogramok letöltheti a [Microsoft letöltőközpontból][].

### <a name="data-in-ascii-or-utf-16-format"></a>ASCII- vagy UTF-16 adatokkal

Ha a saját adatain próbálja ebben az oktatóanyagban, az adatok kell az ASCII- vagy UTF-16 kódolás használata óta bcp UTF-8 nem támogatja. 

## <a name="1-create-a-destination-table"></a>1. a céltábla létrehozása

Táblázat meghatározásával SQL-adatbázisban a célként használt táblát. A táblázat oszlopainak meg kell egyeznie az adatfájl minden egyes sor adatait.

Hozzon létre egy táblázatot, nyisson meg egy parancssort, és futtassa a következő parancsot a sqlcmd.exe használatával:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Következő lépések

SQL Server-adatbázis áttelepítése, olvassa el az [SQL Server-adatbázis áttelepítése](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[A Microsoft letöltőközpontból.]: https://www.microsoft.com/download/details.aspx?id=36433
