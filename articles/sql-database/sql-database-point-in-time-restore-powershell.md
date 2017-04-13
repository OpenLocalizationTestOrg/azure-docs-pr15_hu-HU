<properties
    pageTitle="Egy előző pont Azure SQL-adatbázis visszaállítása (PowerShell) idő |} Microsoft Azure"
    description="Azure SQL-adatbázis visszaállítása az előző pontjához időben"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Azure SQL-adatbázis visszaállítása az időt PowerShell egy előző pont

> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-recovery-using-backups.md)
- [Pont és az idő visszaállítása: Azure portal](sql-database-point-in-time-restore-portal.md)

Ez a cikk bemutatja, hogyan szeretné visszaállítani az adatbázis egy korábbi állapotba időben [SQL-adatbázis automatikus biztonsági mentést](sql-database-automated-backups.md). Ehhez a PowerShell használatával.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Az adatbázis visszaállítása pontra a különálló adatbázis egyidejűleg

1. Az adatbázis visszaállítása az [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) parancsmag használatával szeretne beszerzése.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Az adatbázis visszaállítása pontra az idő használatával a [visszaállítása-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) parancsmag.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Az adatbázis visszaállítása pontra az idő egy rugalmas adatbázis készletbe

1. Az adatbázis visszaállítása az [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) parancsmag használatával szeretne beszerzése.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Az adatbázis visszaállítása pontra az idő használatával a [visszaállítása-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) parancsmag.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"


## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolása](sql-database-copy.md)
