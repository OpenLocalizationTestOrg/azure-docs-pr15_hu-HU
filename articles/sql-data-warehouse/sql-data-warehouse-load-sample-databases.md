<properties
   pageTitle="Mintaadatok betöltése SQL adatraktár |} Microsoft Azure"
   description="Mintaadatok betöltése SQL adatraktár"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Mintaadatok betöltése SQL adatraktár

Egyszerű betöltése és az Adventure Works mintaadatbázis lekérdezési lépések végrehajtásával. A parancsfájlok sqlcmd először használata SQL, amely hoz létre a táblák és nézetek futtatásához. Táblázatok létrehozása után a parancsfájlok fogja használni bcp adat betöltése.  Ha még nincs sqlcmd és bcp van telepítve, kövesse ezeket a hivatkozásokat [bcp telepítése][] és [sqlcmd telepítéséhez][].

##<a name="load-sample-data"></a>Példaadatok betöltése

1. Az [Adventure Works minta parancsfájlok SQL adatraktár][] zip-fájl letöltése.

2. A fájlok kinyerése letöltött zip könyvtár a helyi számítógépre.

3. A kibontott fájl aw_create.bat szerkesztése, és adja meg a fájl tetején található, a következő változók.  Ügyeljen arra, hogy nincs közötti térköz hagyja a "=" és a paramétert.  Az alábbi példák hogyan nézhet ki a szerkesztést.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. A Windows cmd parancssorából futtassa a szerkesztett aw_create.bat.  Győződjön meg arról, hogy Ön a címtárban aw_create.bat szerkesztett verziójának mentési helyének.
Ez a parancsfájl fog...
    * Bármely Adventure Works táblák vagy nézetek, amely már létezik az adatbázis leválasztása
    * Az Adventure Works táblák és nézetek létrehozása
    * Egyes Adventure Works táblázatot bcp betöltése
    * A sorok száma Adventure Works mindegyikhez ellenőrzése
    * Az Adventure Works táblázatoknak minden oszlopra Statisztika gyűjtése


##<a name="query-sample-data"></a>A lekérdezés a mintaadatok

Miután mintaadatokkal be az SQL adatraktár által betöltött, néhány lekérdezések gyorsan futtathatja.  A [Visual Studio lekérdezés][] dokumentum ismertetett módon csatlakozni az újonnan létrehozott Adventure Works adatbázis Azure SQL-DW a Visual Studio és SSDT, a lekérdezés futtatásához.

Példa: egyszerű select utasítás az alkalmazottak az adatok eléréséhez:

```sql
SELECT * FROM DimEmployee;
```

Nézze meg az összes értékesítés végösszegét minden nap szerkezeteket, például a GROUP BY használatával összetettebb lekérdezés példája:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Példa: egy adott dátumig rendelések kiszűrésére WHERE záradékot tartalmazó SELECT:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL-adatraktár szinte minden az SQL-T szerkezeteket, amely támogatja az SQL Server támogatja.  Bármely különbségek a [kód áttelepítése][] dokumentáció ismertetett.

## <a name="next-steps"></a>Következő lépések
Most, hogy a lyncen próbálja ki az egyes a mintaadatokat tartalmazó lekérdezések lehetőséget, ellenőrizze, hogy miként [fejlesztése][], [betölteni][]vagy [áttelepítése][] SQL adatraktár.

<!--Image references-->

<!--Article references-->
[áttelepítése]: sql-data-warehouse-overview-migrate.md
[kidolgozása]: sql-data-warehouse-overview-develop.md
[betöltése]: sql-data-warehouse-overview-load.md
[a Visual Studio lekérdezés]: sql-data-warehouse-query-visual-studio.md
[kód áttelepítése]: sql-data-warehouse-migrate-code.md
[bcp telepítése]: sql-data-warehouse-load-with-bcp.md
[sqlcmd telepítése]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works minta parancsfájlokat az SQL adatraktár]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
