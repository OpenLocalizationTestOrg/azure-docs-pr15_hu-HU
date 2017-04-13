<properties
   pageTitle="SQL Server Management Studio segítségével határozza meg az SQL-adatbázis kompatibilitási Azure SQL-adatbázishoz való áttelepítés előtt |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, az SQL-adatbázishoz való kompatibilitás érdekében exportálási réteg alkalmazás varázsló"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>SQL Server Management Studio segítségével határozza meg az SQL-adatbázis kompatibilitási Azure SQL-adatbázishoz való áttelepítés előtt

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor frissítése](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
Ebben a cikkben megismerheti, határozza meg, ha az SQL Server-adatbázis van kompatibilis adatok réteg alkalmazás exportálás varázsló segítségével az SQL Server Management Studio SQL-adatbázis áttelepítése.

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio segítségével

1. Ellenőrizze, hogy az SQL Server Management Studio legújabb verzióját. Új verziója Management Studio havi maradjon szinkronizálja az Azure-portálra frissítésekben frissülnek.

     > [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).

2. Nyissa meg a Management Studio eszközben, és csatlakozás a forrásadatbázis az objektum Explorerben.
3. Kattintson a jobb gombbal a forrásadatbázis az objektum Explorer, mutasson a **feladatok**és kattintson a **… Adatok szintű alkalmazás exportálása**

    ![A feladatok menü egy adatok szintű alkalmazás exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Az exportálás varázslóban kattintson a **Tovább**gombra, és kattintson a **Beállítások** lapon az Exportálás a merevlemezre helyre vagy egy Azure blob BACPAC mentéshez konfigurálása. BACPAC fájlba menti a program nem adatbázis kapcsolatos kompatibilitási problémák esetén. Kompatibilitási problémák merülnek fel, ha azokat meg a konzolon megjelennek.

    ![Beállítások exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Szeretne ugrani, adatok exportálása, kattintson a **Speciális fülre** , és törölje a jelet **Az összes kijelölése** jelölőnégyzetből. Ezen a ponton célunk csak tesztelje a kompatibilitás érdekében.

    ![Beállítások exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Kattintson a **Tovább** gombra, és kattintson a **Befejezés**gombra. Adatbázis-kompatibilitási problémák, után jelennek meg a varázsló ellenőrzi a sémában.

    ![Beállítások exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Ha nincs hiba jelenik meg, az adatbázis kompatibilis, és készen áll a áttelepítése. Ha hibákat, szükséges a hibák kijavításának céljából. A hibák megtekintéséhez kattintson az **Validating séma** **hiba** . 
    ![Beállítások exportálása](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Ha a *. BACPAC fájl sikeresen jön létre, majd az adatbázis kompatibilis SQL-adatbázishoz, és készen áll a áttelepítése.

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)
- [Adatbázis áttelepítése kapcsolatos kompatibilitási problémák megoldása](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Kompatibilis SQL Server-adatbázis áttelepítése az SQL-adatbázis](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
