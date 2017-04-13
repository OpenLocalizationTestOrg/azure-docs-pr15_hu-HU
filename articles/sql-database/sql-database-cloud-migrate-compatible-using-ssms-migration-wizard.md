<properties
   pageTitle="SQL Server-adatbázis áttelepítése az adatbázis terjesztése Microsoft Azure-adatbázis varázsló használata SQL-adatbázis |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése a Microsoft Azure-adatbázis varázsló"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>SQL Server-adatbázis áttelepítése az SQL-adatbázis terjesztése adatbázist, hogy a Microsoft Azure-adatbázis varázsló használatával


> [AZURE.SELECTOR]
- [SSMS áttelepítése varázsló](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC fájl exportálása](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importálás BACPAC fájlból](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tranzakció alapú replikációs](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

A Microsoft Azure-adatbázis varázsló az SQL Server Management Studio üzembe adatbázis közvetlenül az Azure SQL-adatbázis kiszolgálója áttelepíti a [kompatibilis SQL Server-adatbázishoz](sql-database-cloud-migrate.md) .

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>A központi telepítés adatbázis Microsoft Azure-adatbázis varázsló használata

> [AZURE.NOTE] A következő lépések feltételezik, hogy van egy [SQL-adatbázis kiszolgálója kiépítve](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Ellenőrizze, hogy az SQL Server Management Studio legújabb verzióját. Új verziója Management Studio havi maradjon szinkronizálja az Azure-portálra frissítésekben frissülnek.

    > [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).

2. Nyissa meg a Management Studio eszközben, és csatlakozott az SQL Server-adatbázishoz az objektum Explorer áttelepítendő.
3. Kattintson a jobb gombbal az objektum-kezelő ablakban az adatbázis, mutasson a **tevékenységek**, és kattintson a **Microsoft Azure SQL-adatbázis... adatbázis terjesztése**

    ![Feladatok menüből Azure telepítése](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  A telepítési varázslóban kattintson a **Tovább**gombra, és kattintson a **Csatlakozás** kattintva állítsa be az SQL-adatbázis kiszolgálója a kapcsolatot.

    ![Feladatok menüből Azure telepítése](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. A csatlakozás kiszolgáló párbeszédpanelt írja be a kapcsolat adatait az SQL-adatbázis-kiszolgálóhoz való csatlakozáshoz.

    ![Feladatok menüből Azure telepítése](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Adja meg a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlt, amely során az áttelepítési folyamatot létrehozása varázsló az alábbi lépéseket:

 - **Új adatbázis neve** 
 - A **Edition a Microsoft Azure SQL-adatbázis** ([szolgáltatási réteg](sql-database-service-tiers.md))
 - A **maximális az adatbázis mérete**
 - A **Szolgáltatás cél** (teljesítményszint)
 - **Ideiglenes fájl neve**  

    ![Beállítások exportálása](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  A varázsló. Attól függően, hogy méretétől és összetettségétől az adatbázisról telepítési időt vehet igénybe néhány perc sok óra. Ha ezzel a varázslóval kapcsolatos kompatibilitási problémák észlel, hibák jelennek meg a képernyőre, és az áttelepítés nem továbbra is. Adatbázis kapcsolatos kompatibilitási problémák megoldásával kapcsolatos útmutatást lépjen [adatbázis kapcsolatos kompatibilitási problémák](sql-database-cloud-migrate-fix-compatibility-issues.md)megoldásához.

7.  Objektum Intézővel csatlakozott az Azure SQL-adatbázis kiszolgálója az áttelepített adatbázishoz.
8.  Az Azure portál használatával, megtekintheti az adatbázis és annak tulajdonságait.

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
