<properties
    pageTitle="Azure SQL-adatbázis visszaállítása biztonsági másolatból geo felesleges (PowerShell) |} Microsoft Azure"
    description="Azure SQL-adatbázis visszaállítása be egy új webkiszolgálóra geo felesleges biztonsági másolatból"
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

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Azure SQL-adatbázis visszaállítása biztonsági geo felesleges másolatból PowerShell használatával


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-recovery-using-backups.md)
- [GEO-visszaállítása: Azure portál](sql-database-geo-restore-portal.md)

Ez a cikk bemutatja, hogyan az adatbázis visszaállítása az új kiszolgáló geo-visszaállítási használatával. Ez a Powershellen keresztül elvégezhető.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>GEO-visszaállítása az adatbázis egy különálló adatbázisba

1. Első geo felesleges adatbázis biztonsági másolatát a kívánt visszaállítása a [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) parancsmag.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Indítsa el a visszaállítás geo felesleges biztonsági másolatból a [visszaállítása-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) parancsmag.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Az adatbázis-rugalmas adatbázis készletbe GEO-visszaállítása

1. Első geo felesleges adatbázis biztonsági másolatát a kívánt visszaállítása a [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) parancsmag.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Indítsa el a visszaállítás geo felesleges biztonsági másolatból a [visszaállítása-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) parancsmag. Adja meg a készlet nevét, az adatbázis be a visszaállítani kívánt.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md)című témakörben találhat üzleti folytonosságot.
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért olvassa el a [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)című témakört.
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md).
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md).  
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolatot](sql-database-copy.md).
