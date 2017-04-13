<properties
   pageTitle="SQL-adatbázisokkal tranzakció alapú replikációs áttelepítése |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, az adatbázis, importálása tranzakció alapú replikációs"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>SQL Server-adatbázis áttelepítése az Azure SQL-adatbázis tranzakció alapú replikációs használatával

> [AZURE.SELECTOR]
- [SSMS áttelepítése varázsló](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC fájl exportálása](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importálás BACPAC fájlból](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tranzakció alapú replikációs](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Ebben a cikkben megismerheti az SQL Server tranzakció alapú replikációs szolgáltatásával minimális legrövidebb leállás Azure SQL-adatbázis kompatibilis SQL Server-adatbázis áttelepítése.

## <a name="understanding-the-transactional-replication-architecture"></a>A tranzakció alapú replikációs architektúra ismertetése

Amikor elvesztését nem engedheti meg magának az SQL Server-adatbázishoz eltávolítása gyártási, miközben az áttelepítés mi, SQL Server tranzakció alapú replikációs az áttelepítési megoldásként is használhatja. A megoldás használatára, az Azure SQL-adatbázissal, az áttelepíteni kívánt helyszíni SQL Server-példányt előfizető konfigurálása. A helyszíni tranzakció alapú replikációs distributor szinkronizálja az adatokat a szinkronizálandó (publisher), amíg az új tranzakciók továbbra is fennáll a helyszíni adatbázisból. 

Tranzakció alapú replikációs áttelepítése a helyszíni adatbázis csak egy részhalmazát is használhatja. Lehet, hogy a kiadványt, bizonyos Azure SQL-adatbázishoz, csak egy részét a replikált adatbázisból származó táblázatot. Replikált mindegyikhez korlátozhatja a sorokat egy részét, illetve az oszlopok csak egy részhalmazát szeretné az adatokat.

Tranzakció alapú ismétlésekkel az adatok és a séma minden módosítás jelenik meg az Azure SQL-adatbázisban. Miután befejeződött a szinkronizálás, és készen áll a áttelepítéséhez, módosítsa a kapcsolati karakterláncot, az alkalmazás az Azure SQL-adatbázishoz mutassanak. Miután tranzakció alapú replikációs lezuhan balra a helyszíni adatbázis és az összes alkalmazás Azure DB mutasson a módosításokat, tranzakció alapú replikációs távolíthatja el. Az Azure SQL-adatbázis ettől kezdve a munkakörnyezetben.

 ![SeedCloudTR diagram](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Tranzakció alapú replikációs vonatkozó követelmények

Tranzakció alapú replikációs egy olyan technológia, a beépített és az SQL Server-integrált, SQL Server 6.5 óta. Olyan, hogy a legtöbb DBAs tudják élmény rendelkeznek, amelyekkel elért és igazolt technológia. Az [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)most lehetősége az Azure SQL-adatbázis konfigurálása a helyszíni kiadványhoz [tranzakció alapú replikációs előfizetői](https://msdn.microsoft.com/library/mt589530.aspx) szerint. A felület, amely akkor első beállítását a Management Studio megegyezik, mintha egy helyszíni kiszolgálón tranzakció alapú replikációs előfizető beállítása. A publisher és a distributor legalább egy, a következő SQL Server-verzió esetén támogatott funkciók támogatása ebben az esetben:

 - Az SQL Server 2016-ban és a fenti 
 - Az SQL Server 2014-es SP1 CU3 és a fenti
 - Az SQL Server 2014-es RTM CU10 és a fenti
 - Az SQL Server 2012 SP2 CU8 és a fenti
 - Az SQL Server 2012 SP3 és a fenti


> [AZURE.IMPORTANT] Az SQL Server Management Studio maradjon legújabb verziójának használata szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázishoz. Az SQL Server Management Studio korábbi verzióiban a SQL-adatbázis előfizető nem tudja beállítani. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Következő lépések

- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)
- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [Az SQL Server 2016-ban](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>További források

- [Tranzakció alapú replikációs](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
