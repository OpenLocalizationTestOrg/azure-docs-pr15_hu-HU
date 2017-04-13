<properties
   pageTitle="SQL Server-adatbázis kompatibilitási problémák SQL-adatbázishoz való áttelepítés előtt |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, a kompatibilitás érdekében az SQL Azure-áttelepítés varázsló"
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

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Az SQL Azure áttelepítési varázsló segítségével javítása az SQL Server adatbázis kapcsolatos kompatibilitási problémák Azure SQL-adatbázishoz való áttelepítés előtt

> [AZURE.SELECTOR]
- [Az SQL Azure-áttelepítés](sql-database-cloud-migrate-fix-compatibility-issues.md) varázslóval
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) használata
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) használata

Ebben a cikkben megismerheti, észleli és megoldhatja az SQL Server adatbázis kapcsolatos kompatibilitási problémák a varázslóval SQL Azure áttelepítési Azure SQL-adatbázishoz való áttelepítés előtt.

## <a name="using-sql-azure-migration-wizard"></a>Az SQL Azure-áttelepítés varázslóval

Az [SQL Azure-áttelepítés varázsló](http://sqlazuremw.codeplex.com/) CodePlex eszköz használatával készítése az SQL-T parancsfájl-kompatibilis adatforrás-adatbázisból. Ez a parancsfájl majd átalakul, hogy az SQL-adatbázis kompatibilis legyen a varázslóval. Azure SQL-adatbázis hajtsa végre a parancsfájlt majd csatlakozik. Ez az eszköz is elemzi a nyomkövetési fájlok kapcsolatos kompatibilitási problémák határozza meg. A parancsprogram csak séma használata hozhatók létre, vagy BCP formátumban adatokat tartalmazhat. Dokumentációt, beleértve a részletes útmutató érhető el funkcióhoz a Codeplex webhelyen az [SQL Azure-áttelepítés varázsló](http://sqlazuremw.codeplex.com/).  

 ![SAMW áttelepítési diagram](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] A varázsló által talált, nem az összes nem kompatibilis séma javítható a beépített átalakítások. Javított nem kompatibilis parancsfájl hibák, a Megjegyzések be a létrehozott parancsfájl beékelt jelentett. Ha sok hibát észlel, az Visual Studio vagy SQL Server Management Studio lépéseinek és megoldhatja az egyes hibák, amelyek nem javítható az SQL Server áttelepítési varázsló használata.

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)
- [Kompatibilis SQL Server-adatbázis áttelepítése az SQL-adatbázis](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
