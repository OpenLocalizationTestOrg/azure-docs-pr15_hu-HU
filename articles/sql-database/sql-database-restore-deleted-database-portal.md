<properties
    pageTitle="A törölt Azure SQL-adatbázis (Azure portal) visszaállítása |} Microsoft Azure"
    description="Vissza a törölt Azure SQL-adatbázis (Azure portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Az Azure-portálon törölt Azure SQL-adatbázis visszaállítása

> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-recovery-using-backups.md)
- [**A törölt DB visszaállítása: portál**](sql-database-restore-deleted-database-portal.md)
- [A törölt DB visszaállítása: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Jelölje ki az adatbázis visszaállítása 

Az Azure-portálon törölt adatbázis visszaállítása:

1.  Az [Azure-portálon](https://portal.azure.com)kattintson a **További szolgáltatások** > **SQL-kiszolgálók**.
3.  Jelölje be a kiszolgáló, amely tartalmazza a visszaállítani kívánt adatbázist.
4.  Görgessen le a kiszolgáló lap **Műveletek** szakaszát, és válassza a **Törölt elemek adatbázisok**: ![egy Azure SQL-adatbázis visszaállítása](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Jelölje ki a visszaállítani kívánt adatbázist.
6.  Adja meg az adatbázis nevét, és kattintson az **OK gombra**:

    ![Az Azure SQL-adatbázis visszaállítása](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
