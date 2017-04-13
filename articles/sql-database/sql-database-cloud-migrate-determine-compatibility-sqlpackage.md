<properties
   pageTitle="Határozza meg az SQL-adatbázis kompatibilitási SqlPackage.exe használatával |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, az SQL-adatbázishoz való kompatibilitás érdekében SqlPackage"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>SQL-adatbázis kompatibilitási SqlPackage.exe használatával meghatározása

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor frissítése](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Ebben a cikkben megismerheti, annak megállapításához, hogy egy SQL Server-adatbázis kompatibilis a [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogram használata SQL-adatbázis áttelepítése.

## <a name="using-sqlpackageexe"></a>SqlPackage.exe használata

1. Nyisson meg egy parancssort, és módosítsa a legújabb verziójában sqlpackage.exe tartalmazó könyvtár. Ez a segédprogram szállított az [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és az [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)legújabb verziója, illetve a legújabb [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) letöltheti közvetlenül a Microsoft letöltőközpontból.
2. Hajtsa végre az alábbi argumentumokat környezetben SqlPackage az alábbi parancsot:

    ' sqlpackage.exe /Action:Export /ssn: < kiszolgálónév > /sdn: < adatbázisnév > /tf: < target_file > /p:TableData < schema_name.table_name >>< = output_file > 2 > & 1'

  	| Argumentum  | Leírás  |
  	|---|---|
  	| < kiszolgálónév >  | adatforrás-kiszolgáló neve  |
  	| < adatbázisnév >  | forrás adatbázisnév  |
  	| < target_file >  | a fájl nevét és helyét BACPAC fájl  |
  	| < schema_name.table_name >  | a táblák, amelyhez adatokat a célfájl kimenete  |
  	| < output_file >  | a fájl nevét és helyét a kimeneti fájl hibát, ha vannak ilyenek tartalmazó  |

    A /p:TableName argumentum oka az, hogy csak szeretnénk Azure SQL-adatbázis V12 Exportálás az adatbázis-kompatibilis tesztelje az adatok exportálása az összes táblázat helyett. Sajnos sqlpackage.exe az Exportálás argumentum nem támogatja a nulla táblázatok kibontása. Meg kell adnia a legalább egy táblázat, például egy kis táblát. < Output_file > tartalmazza a jelentés bármely hibáit. A "> 2 > & 1" karakterlánc pipák a normál kimenő és a megadott kimeneti fájl a parancs végrehajtása eredő standard hiba.

    ![A feladatok menü egy adatok szintű alkalmazás exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Nyissa meg a kimeneti fájl, és tekintse át a kompatibilitási hibákat, ha van ilyen. 

    ![A feladatok menü egy adatok szintű alkalmazás exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziójában](https://msdn.microsoft.com/library/mt204009.aspx)
[SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)
- [Adatbázis áttelepítése kapcsolatos kompatibilitási problémák megoldása](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Kompatibilis SQL Server-adatbázis áttelepítése az SQL-adatbázis](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
