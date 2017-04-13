
<properties
   pageTitle="SQL Server-adatbázis exportálása SQL Server Management Studio BACPAC fájlt |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, exportálása az adatbázist, majd az Exportálás BACPAC fájlt, az adatok réteg alkalmazás exportálás varázsló"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>SQL Server-adatbázis exportálása használata SQL Server Management Studio BACPAC-fájlba

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
Ez a cikk bemutatja, hogyan SQL Server-adatbázis exportálása adatok réteg alkalmazás exportálás varázsló segítségével az SQL Server Management Studio [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) fájlba. 

1. Ellenőrizze, hogy az SQL Server Management Studio legújabb verzióját. Új verziója Management Studio havi maradjon szinkronizálja az Azure-portálra frissítésekben frissülnek.

     > [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).

2. Nyissa meg a Management Studio eszközben, és csatlakozás a forrásadatbázis az objektum Explorerben.

    ![A feladatok menü egy adatok szintű alkalmazás exportálása](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Kattintson a jobb gombbal a forrásadatbázis az objektum Explorer, mutasson a **feladatok**és kattintson a **… Adatok szintű alkalmazás exportálása**

    ![A feladatok menü egy adatok szintű alkalmazás exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Állítsa be az adatok exportálása az BACPAC fájl mentéséhez vagy egy helyi merevlemez-helyet, vagy szeretne egy Azure blob-az exportálás varázsló. Az exportált BACPAC mindig tartalmazza a teljes adatbázis sémája és alapértelmezés szerint minden táblázatok adatait. Ha el szeretne rejteni adatok néhány vagy összes a tábla, használja a Speciális fülre. Előfordulhat, hogy, például válassza csak azokat az adatokat hivatkozás táblák és nem az összes táblázat exportálása.

***Fontos*** Egy BACPAC Azure blob-tárolóhoz exportálásakor használja a szabványos tároló. Egy BACPAC importálása prémium tárhelyről nem támogatott.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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
