<properties
   pageTitle="SQL Server-adatbázis exportálása SqlPackage BACPAC fájlt |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, exportálása az adatbázist, majd az Exportálás BACPAC fájl sqlpackage"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>SQL Server-adatbázis exportálása SqlPackage BACPAC fájlt

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

Ez a cikk bemutatja, hogyan SQL Server-adatbázis exportálása a [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogrammal [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlba. Ez a segédprogram szállított az [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és az [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)legújabb verziója, illetve a legújabb [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) letöltheti közvetlenül a Microsoft letöltőközpontból.

1. Nyisson meg egy parancssort, és módosítsa a sqlpackage.exe parancssori segédprogram tartalmazó könyvtár – Ez a segédprogram a Visual Studio és az SQL Server rendszerhez. Használja a keresőt a számítógépen keresse meg az elérési utat a környezetben.
2. Hajtsa végre az alábbi argumentumokat környezetben sqlpackage.exe az alábbi parancsot:

    "sqlpackage.exe /Action:Export /ssn: < kiszolgálónév > /sdn: < adatbázisnév > /tf: < target_file >

  	| Argumentum  | Leírás  |
  	|---|---|
  	| < kiszolgálónév >  | adatforrás-kiszolgáló neve  |
  	| < adatbázisnév >  | forrás adatbázisnév  |
  	| < target_file >  | a fájl nevét és helyét BACPAC fájl  |

    ![A feladatok menü egy adatok szintű alkalmazás exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)
- [Egy BACPAC importálása az Azure SQL-adatbázis SSMS használatával](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Egy BACPAC importálása az Azure SQL-adatbázis SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Egy BACPAC importálása az Azure SQL-adatbázis Azure portál](sql-database-import.md)
- [Egy BACPAC importálása az Azure SQL-adatbázis PowerShell](sql-database-import-powershell.md)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
