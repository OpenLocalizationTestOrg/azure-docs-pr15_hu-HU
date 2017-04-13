<properties
   pageTitle="Az SQL Server adatbázis kompatibilitási problémák megoldása az SQL Server felügyeleti Studio segítségével SQL-adatbázishoz való áttelepítés előtt |} Microsoft Azure"
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

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Az SQL Server adatbázis kompatibilitási problémák megoldása az SQL Server Management Studio használata SQL-adatbázishoz való áttelepítés előtt

> [AZURE.SELECTOR]
- [Az SQL Azure-áttelepítés](sql-database-cloud-migrate-fix-compatibility-issues.md) varázslóval
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) használata
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) használata

A haladó felhasználók háríthatja el az SQL Server adatbázis kapcsolatos kompatibilitási problémák használata SQL Server Management Studio Azure SQL-adatbázishoz való áttelepítés előtt.


> [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio segítségével

SQL Server Management Studio segítségével különböző Transact-SQL-parancsok, például **Az adatbázis módosítása**kapcsolatos kompatibilitási problémák megoldásához. Ez a módszer elsősorban a speciális felhasználókat a kényelmes dolgozik Transact-SQL nyelvben élő adatbázis. Egyéb esetben ajánlott SSDT használható. 



## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)
- [Kompatibilis SQL Server-adatbázis áttelepítése az SQL-adatbázis](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
