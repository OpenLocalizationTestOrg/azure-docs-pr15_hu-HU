<properties
    pageTitle="Az Azure SQL-adatbázis visszaállítása az automatikus biztonsági mentés (Azure portal) |} Microsoft Azure"
    description="Az Azure SQL-adatbázis visszaállítása az automatikus biztonsági mentés (Azure portal)."
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


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Az automatikus biztonsági mentés az Azure portálon visszaállítani Azure SQL-adatbázishoz


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-recovery-using-backups.md#geo-restore)
- [GEO-visszaállítása: PowerShell](sql-database-geo-restore-powershell.md)

Ez a cikk bemutatja, hogyan szeretné visszaállítani az adatbázis- [automatikus biztonsági mentés](sql-database-automated-backups.md) be egy új webkiszolgálóra a [Geo-visszaállítása](sql-database-recovery-using-backups/.md#geo-restore) az Azure portálon.

## <a name="select-a-database-to-restore"></a>Jelölje ki az adatbázis visszaállítása

Az Azure-portálon adatbázis visszaállításához tegye a következőket:

1.  Nyissa meg az [Azure-portálon](https://portal.azure.com).
2.  A képernyő bal oldalán válassza az **+ Új** > **adatbázisok** > **SQL-adatbázis**:

    ![Az Azure SQL-adatbázis visszaállítása](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Jelölje ki a **biztonsági másolat** forrásaként, és válassza ki a visszaállítani kívánt biztonsági másolat. Adja meg az adatbázis nevét, a kiszolgáló szeretné állítani az adatbázist, és kattintson a **Létrehozás**gombra:
  
    ![Az Azure SQL-adatbázis visszaállítása](./media/sql-database-geo-restore-portal/geo-restore.png)

Az értesítési ikonra a képernyő jobb felső sarkában a lapon kattintson a visszaállítás állapotának figyelése 


## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
