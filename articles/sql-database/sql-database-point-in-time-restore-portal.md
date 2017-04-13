<properties
    pageTitle="Egy előző pont időben (Azure portal) visszaállíthatja az Azure SQL-adatbázishoz |} Microsoft Azure"
    description="Visszaállíthatja az Azure SQL-adatbázishoz egy előző pont időben."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Egy előző pont időben az Azure portálján visszaállíthatja az Azure SQL-adatbázishoz


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-recovery-using-backups.md)
- [Pont és az idő visszaállítása: PowerShell](sql-database-point-in-time-restore-powershell.md)

Ez a cikk bemutatja, hogyan szeretné visszaállítani az adatbázis egy korábbi állapotba időben [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md) az Azure portálon.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>SQL-adatbázis visszaállítása egy korábbi állapotra időben

Jelölje ki az adatbázis visszaállítása az Azure-portálon:

1.  Nyissa meg az [Azure-portálon](https://portal.azure.com).
2.  A képernyő bal oldalán válassza a **További szolgáltatások** > **SQL-adatbázisait**.
3.  Kattintson a visszaállítani kívánt adatbázist.
4.  Az adatbázis-lap tetején válassza a **Visszaállítás**:

    ![Az Azure SQL-adatbázis visszaállítása](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  **Visszaállítása** lapon jelölje be a dátum és idő (az Egyezményes világidő) visszaállítása az adatbázis, és kattintson **az OK**gombra:

    ![Az Azure SQL-adatbázis visszaállítása](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>A visszaállítás figyelése

1. Után kattintson az **OK gombra** az előző lépésben, kattintson az értesítési ikonra a lap jobb felső, és kattintson a **Visszaállítás SQL-adatbázis** értesítésre további információt.

    ![Az Azure SQL-adatbázis visszaállítása](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Az adatbázis visszaállítása SQL lap megnyílik, és a visszaállítás állapotáról szolgáló információkat. További részletekért sortétel gombra kattint:

    ![Az Azure SQL-adatbázis visszaállítása](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
