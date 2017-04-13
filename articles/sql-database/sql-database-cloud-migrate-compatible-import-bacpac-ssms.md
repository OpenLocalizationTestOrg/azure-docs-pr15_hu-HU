<properties
   pageTitle="SQL Server-adatbázis áttelepítése az Azure SQL-adatbázis |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázis adatbázis terjesztése, az adatbázis áttelepítése, adatbázis importálása exportálás adatbázis áttelepítése varázsló"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>BACPAC importálása SSMS használata SQL-adatbázishoz

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure portál](sql-database-import.md)
- [A PowerShell](sql-database-import-powershell.md)

Ebből a cikkből megtudhatja SQL-adatbázis [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlból való importálása és exportálása adatok réteg alkalmazás varázsló segítségével az SQL Server Management Studio.

> [AZURE.NOTE] A következő lépések feltételezik, hogy van már kiépítve a logikai Azure SQL-példány, és a kapcsolat adatait előkészítenie.

1. Ellenőrizze, hogy az SQL Server Management Studio legújabb verzióját. Új verziója Management Studio havi maradjon szinkronizálja az Azure-portálra frissítésekben frissülnek.

     > [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).

2. Az Azure SQL-adatbázis kiszolgálója csatlakozni, kattintson a jobb gombbal a **Databases** mappát, és válassza az **Importálás adatok szintű alkalmazás**

    ![Importálja az adatokat szintű alkalmazás menüpont](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Az adatbázis létrehozása az Azure SQL-adatbázissal, BACPAC fájl importálása a merevlemezre, vagy jelölje ki az Azure tárterület-fiók és, ahova feltöltötte a BACPAC fájlt tároló.

    ![Az importálási beállítások](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Egy BACPAC az Azure blob-tárolóhoz importálásakor használja a szabványos tároló. Egy BACPAC importálása prémium tárhelyről nem támogatott.

4.  **Új adatbázis neve** az adatbázist a Azure SQL-adatbázis szükséges, és állítsa a **Edition a Microsoft Azure SQL-adatbázis** (szolgáltatási réteg), a **Maximum az adatbázis mérete**és a **Szolgáltatás cél** (teljesítményszint).

    ![Adatbázis-beállításai](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Kattintson a **Tovább** gombra, és kattintson a **Befejezés gombra** az BACPAC fájl importálása az Azure SQL-adatbázis kiszolgálója az új adatbázis.

6. Objektum Intézővel csatlakozott az Azure SQL-adatbázis kiszolgálója az áttelepített adatbázishoz.

6.  Az Azure portál használatával, megtekintheti az adatbázis és annak tulajdonságait.

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
