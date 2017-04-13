<properties
   pageTitle="SQL Server-adatbázis kompatibilitási problémák SQL-adatbázishoz való áttelepítés előtt |} Microsoft Azure"
   description="Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, kompatibilis, az SQL Azure-áttelepítés varázsló, SSDT"
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

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Az SQL Server-adatbázis áttelepítése az Azure SQL-adatbázis használata SQL Server Data Tools for Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor frissítése](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Ebben a cikkben megismerheti, észleli és megoldhatja az SQL Server adatbázis kapcsolatos kompatibilitási problémák használata a az SQL Server Data Tools for Visual Studio Azure SQL-adatbázishoz való áttelepítés előtt.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Használatával az SQL Server Data Tools for Visual Studio

Használja az SQL Server Data Tools for Visual Studio ("SSDT") az adatbázissémát importálhatja az elemzéshez Visual Studio adatbázis projekt. Szeretne elemezni, adja meg a projekt a cél platform SQL-adatbázis V12, és ezután létrehozhatja a projektet. Ha az összeállítás sikeres, az adatbázis kompatibilis. Ha nem sikerül összeállítása, megoldhatja a a hibák SSDT (vagy az egyéb eszközök ebben a témakörben tárgyalt egyikére). Miután a projekt sikeres hoz létre, a vissza másolatként a forrásadatbázis közzéteheti. Másolja az adatokat a forrásadatbázis kompatibilis V12 a Azure SQL-adatbázishoz SSDT a az adatok összehasonlítása szolgáltatás majd használni. A frissített adatbázis majd telepítheti át. Használja ezt a beállítást, töltse le a [legújabb verziójában SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![VSSSDT áttelepítési diagram](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Ha csak séma áttelepítési szükség, a séma tehetők közzé közvetlenül a Visual Studio közvetlenül az Azure SQL-adatbázis. Használata ezt a módszert, ha az adatbázis sémája van szüksége, mint az áttelepítési varázsló egyedül is kezelhető további módosításokat.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Annak ellenőrzése, használja az SQL Server Data Tools for Visual Studio kapcsolatos kompatibilitási problémák
   
1.  Nyissa meg az **SQL Server-objektum Explorer** Visual Studio. **Adja hozzá az SQL Server** használatával az SQL Server-példányt, az áttelepítendő adatbázist tartalmazó csatlakozni. Objektum Intézőben keresse meg az adatbázist, kattintson a jobb gombbal az adatbázist, és jelölje be az **Új projekt létrehozása...**     
    
    ![Új projekt](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Állítsa be az importálási beállítások importálása **csak az alkalmazás – munkafüzetszintű objektumok**. Törölje a jelet a beállítások, az alábbi importálandó: hivatkozott bejelentkezések, engedélyek és adatbázis-beállításokat.    

    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Kattintson a **Start** importálni az adatbázist, és a projektet, amely tartalmazza az SQL-T parancsfájl minden az adatbázis-objektum létrehozása gombra. A parancsfájlok beágyazott a mappák a projektben.    

    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  A Visual Studio megoldás Intéző kattintson a jobb gombbal az adatbázis projekt, és válassza a Tulajdonságok parancsot. A **Projekt-beállítások** lapon állítsa be a cél Platform, a Microsoft Azure SQL-adatbázis V12.    
    
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Kattintson a jobb gombbal a projektet, és válassza a **Szerkesztés** össze a projekt.    
    
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  A **Hiba lista** minden nem kompatibilis jeleníti meg. Ebben az esetben a felhasználónév NT Authority/Hálózati szolgáltatás nem kompatibilis. Mivel ez éppen nem kompatibilis, is megjegyzést ki vagy távolíthatja el (és a bejelentkezési adatok és a szerepkör eltávolítása az adatbázis-megoldás vonzataiért cím).     
    
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Rögzítési használata SQL Server Data Tools for Visual Studio kapcsolatos kompatibilitási problémák

1.  Kattintson duplán az első parancsprogramot, nyissa meg a parancsfájlt egy lekérdezés ablak és a Megjegyzés meg a parancsfájlt, és ezután hajtsa végre a parancsfájlt.     
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Ismételje meg ezt a folyamatot minden tartalmazó kompatibilitási problémák, amíg nem hiba maradnak parancsfájlok.    
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Ha az adatbázis hibátlan, kattintson a jobb gombbal a projekt, és válassza a **Közzététel**. A forrásadatbázis másolatának összeállítása és közzétett (ajánlott kezdetben legalább egy példányát, használandó).     
 - Mielőtt közzétenne, attól függően, hogy a forrás SQL Server verziónál-verziót (SQL Server 2014-es), szükség lehet a projekt cél platform ahhoz, hogy telepítési alaphelyzetbe állítása.     
 - Ha egy régebbi SQL Server-adatbázishoz, nem kiegészíteni bármely funkciók a project által nem támogatott a forrásban SQL Server-ig áttelepítése az adatbázis az SQL Server újabb verzióját.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  Az SQL Server objektum Explorerben kattintson a jobb gombbal a forrásadatbázis, és kattintson az **Adatok összehasonlítása**. A projekt, hogy az eredeti adatbázis összehasonlítása segít megértéséhez, hogy milyen módosításokat esett a varázslóval. Jelölje ki az Azure SQL-V12 a verziójával, amelyet az adatbázist, és kattintson a **Befejezés gombra**.    
    
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Tekintse át a talált különbségek, és kattintson a **Frissítés cél** áttelepítendő adatok forrásadatbázis az Azure SQL-V12 adatbázisba.     
    
    ![helyettesítő szöveg](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  A telepítési mód kiválasztása Lásd: [kompatibilis SQL Server-adatbázis áttelepítése az SQL-adatbázis.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Következő lépések

- [SSDT legújabb verziója](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio legújabb verziója](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>További források

- [SQL-adatbázis V12](sql-database-v12-whats-new.md)
- [A Transact-SQL nyelvben részben vagy nem támogatott funkciók](sql-database-transact-sql-information.md)
- [Az SQL Server áttelepítési Segéd használata SQL Server-adatbázis áttelepítése](http://blogs.msdn.com/b/ssma/)
