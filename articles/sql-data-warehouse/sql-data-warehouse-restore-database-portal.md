<properties
   pageTitle="Az Azure SQL-adatraktár (Portal) visszaállítása |} Microsoft Azure"
   description="Azure portál feladatok az Azure SQL-adatraktár visszaállításához."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Az Azure SQL-adatraktár (Portal) visszaállítása

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Portál][]
- [A PowerShell][]
- [TÖBBI][]

Ebben a cikkben megtanulhatja, hogy miként állíthatja vissza az Azure SQL adatraktár az Azure-portálon.

## <a name="before-you-begin"></a>Első lépések

**Ellenőrizze a DTU kapacitásának.** Minden egyes SQL adatraktár egy SQL server (pl. myserver.database.windows.net), amely egy alapértelmezett DTU kvótája üzemelteti.  Egy SQL adatraktár a visszaállítás előtt ellenőrizze, hogy a az SQL server van elegendő az adatbázist visszaállítják hátralévő DTU kvótája. Megtudhatja, hogyan számítja ki a szükséges DTU, vagy kérjen további DTU, olvassa el a [DTU kvóta módosításának kérése][]című témakört.


## <a name="restore-an-active-or-paused-database"></a>Az aktív vagy felfüggesztett adatbázis visszaállítása

Adatbázis visszaállítása:

1. Jelentkezzen be az [Azure portál][]
2. A képernyő bal oldalán válassza a **Tallózás gombra** , és válassza a **SQL-kiszolgálók**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Nyissa meg a kiszolgáló, és jelölje ki azt
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Keresse meg a használni kívánt visszaállítani, és jelölje ki azt az SQL adatraktár
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. A adatraktár a lap tetején kattintson a **Visszaállítás** gombra.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Adjon meg egy új **adatbázis neve**
7. Jelölje ki a legújabb **Visszaállítási pont**
    1. Ellenőrizze, hogy úgy dönt, hogy a legújabb visszaállítási pontra.  Mivel visszaállítási pontok UTC látható, néha a alapértelmezett beállítás látható, nem a legújabb visszaállítási pont.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Kattintson az **OK gombra**
9. Az adatbázis visszaállítása folyamat elindul, és **értesítések** használatával figyelhető

>[AZURE.NOTE] A visszaállítás befejeződése után az alábbi [állítsa be az adatbázis helyreállítás][]helyreállított adatbázis is beállíthatja.


## <a name="restore-a-deleted-database"></a>A törölt adatbázis visszaállítása

A törölt adatbázis visszaállítása:

1. Jelentkezzen be az [Azure portál][]
2. A képernyő bal oldalán válassza a **Tallózás gombra** , és válassza a **SQL-kiszolgálók**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Nyissa meg a kiszolgáló, és jelölje ki azt
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Görgessen le a Műveletek csoportban a kiszolgáló lap
5. Kattintson a **Törölt adatbázisok** csempére
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Jelölje ki a visszaállítani kívánt törölt adatbázist
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Adjon meg egy új **adatbázis neve**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Kattintson az **OK gombra**
9. Az adatbázis visszaállítása folyamat elindul, és **értesítések** használatával figyelhető

>[AZURE.NOTE] A visszaállítás befejeződése után adja meg az adatbázist, olvassa el [a helyreállítás adatbázis konfigurálása][]. 

## <a name="next-steps"></a>Következő lépések
Az üzleti folytonosságot funkciók Azure SQL-adatbázis kiadásainak kapcsolatos további tudnivalókért olvassa el az [Azure SQL-adatbázis üzleti folytonosságot áttekintése][].

<!--Image references-->

<!--Article references-->
[Azure SQL-adatbázis üzleti folytonosságot – áttekintés]: ./sql-database-business-continuity.md
[– Áttekintés]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[A PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[TÖBBI]: ./sql-data-warehouse-restore-database-rest-api.md
[Az adatbázis konfigurálása helyreállítás]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Kvóta DTU módosításának kérése]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portál]: https://portal.azure.com/
