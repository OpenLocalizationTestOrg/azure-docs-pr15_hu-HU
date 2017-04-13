<properties
   pageTitle="SQL-adatbázishoz importálása SqlPackage BACPAC fájlt"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, adatbázis importálása, sqlpackage BACPAC fájl importálása"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>SQL-adatbázishoz importálása SqlPackage BACPAC fájlt

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portál](sql-database-import.md)
- [A PowerShell](sql-database-import-powershell.md)

Ez a cikk bemutatja, hogyan [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogrammal [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájl importálása SQL-adatbázishoz. Ez a segédprogram szállított az [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) és az [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)legújabb verziója, illetve a legújabb [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) letöltheti közvetlenül a Microsoft letöltőközpontból.


> [AZURE.NOTE] A következő lépések feltételezik, hogy meg van már kiépítve SQL-adatbázis-kiszolgálóval, a kapcsolat adatait előkészítenie és ellenőrizte, hogy a forrásadatbázis kompatibilis.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Azure SQL-adatbázisokkal SqlPackage BACPAC fájlból való importálása

Kövesse az alábbi lépéseket a [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) parancssori segédprogrammal kompatibilis SQL Server-adatbázis (vagy Azure SQL-adatbázis) importálhat BACPAC fájlból.

> [AZURE.NOTE] A következő lépések feltételezik, hogy van már kiépítve egy SQL Azure-adatbázis-kiszolgáló és a kapcsolat adatait előkészítenie.

1. Nyisson meg egy parancssort, és módosítsa a sqlpackage.exe parancssori segédprogram tartalmazó könyvtár – Ez a segédprogram a Visual Studio és az SQL Server rendszerhez.
2. Hajtsa végre az alábbi argumentumokat környezetben sqlpackage.exe az alábbi parancsot:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argumentum  | Leírás  |
  	|---|---|
  	| < kiszolgálónév >  | tároló kiszolgáló neve  |
  	| < adatbázisnév >  | Cél adatbázisnév  |
  	| < felhasználónév >  | a felhasználó nevét a cél kiszolgálóban |
  	| < jelszó >  | a felhasználók jelszava  |
  	| < source_file >  | a fájl nevét és helyét a BACPAC importálandó fájlban  |

    ![A feladatok menü egy adatok szintű alkalmazás exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
