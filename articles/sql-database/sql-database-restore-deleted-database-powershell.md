<properties
    pageTitle="A törölt Azure SQL-adatbázis (PowerShell) visszaállítása |} Microsoft Azure"
    description="Vissza a törölt Azure SQL-adatbázis (PowerShell)."
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


# <a name="restore-a-deleted-azure-sql-database-by-using-powershell"></a>A törölt Azure SQL-adatbázis visszaállítása a PowerShell használatával

> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-recovery-using-backups.md)
- [A törölt DB visszaállítása: portál](sql-database-restore-deleted-database-portal.md)
- [**A törölt DB visszaállítása: PowerShell**](sql-database-restore-deleted-database-powershell.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="get-a-list-of-deleted-databases"></a>A törölt adatbázisok listájának

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"

$DeletedDatabases = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName
```

## <a name="restore-your-deleted-database-into-a-standalone-database"></a>A törölt adatbázis visszaállítása egy különálló adatbázisba

A törölt adatbázis biztonsági másolatának visszaállítása a [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) parancsmag használatával kívánt első. Indítsa el a visszaállítás a törölt adatbázis biztonsági másolatból a [Visszaállítás-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) parancsmag használatával.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"
```


## <a name="restore-your-deleted-database-into-an-elastic-database-pool"></a>A törölt adatbázis visszaállítása az adatbázis rugalmas készletbe

A törölt adatbázis biztonsági másolatának visszaállítása a [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) parancsmag használatával kívánt első. Indítsa el a visszaállítás a törölt adatbázis biztonsági másolatból a [Visszaállítás-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) parancsmag használatával.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01"
```


## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
