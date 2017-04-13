<properties
   pageTitle="Az Azure SQL-adatraktár (PowerShell) visszaállítása |} Microsoft Azure"
   description="A PowerShell feladatok az Azure SQL-adatraktár visszaállításához."
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

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Az Azure SQL-adatraktár (PowerShell) visszaállítása

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Portál][]
- [A PowerShell][]
- [TÖBBI][]

Ez a cikk megtanulhatja, hogyan állíthat vissza az Azure SQL adatraktár PowerShell használatával.

## <a name="before-you-begin"></a>Első lépések

**Ellenőrizze a DTU kapacitásának.** Minden egyes SQL adatraktár egy SQL server (pl. myserver.database.windows.net), amely egy alapértelmezett DTU kvótája üzemelteti.  Egy SQL adatraktár a visszaállítás előtt ellenőrizze, hogy a az SQL server van elegendő az adatbázist visszaállítják hátralévő DTU kvótája. Megtudhatja, hogyan számítja ki a szükséges DTU, vagy kérjen további DTU, olvassa el a [DTU kvóta módosításának kérése][]című témakört.

### <a name="install-powershell"></a>PowerShell telepítése

Azure PowerShell használata SQL adatraktár, hogy szüksége lesz telepítse az Azure PowerShell 1,0 vagy újabb verziót használ.  Érdemes verziójától futtatásával **Get-modul - ListAvailable-neve AzureRM**.  A legújabb [Microsoft webes Platform telepítő][]a telepíthető.  A legújabb verziójának telepítésével kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell][].

## <a name="restore-an-active-or-paused-database"></a>Az aktív vagy felfüggesztett adatbázis visszaállítása

A [Visszaállítás-AzureRmSqlDatabase][] PowerShell-parancsmag használatával visszaállítása pillanatkép az adatbázis.

1. Nyissa meg a Windows PowerShell.
2. Azure csatlakozni, és a fiókkal társított összes előfizetések listája.
3. Jelölje ki azt az előfizetést, az adatbázis visszaállítandó tartalmazza.
4. Az adatbázis visszaállítási pontok sorolja fel.
5. Válassza ki a kívánt visszaállítási pontot a RestorePointCreationDate használatával.
6. Az adatbázis visszaállítása a kívánt visszaállítása pontra.
7. Győződjön meg arról, hogy a visszaállított adatbázist online állapotban.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] A visszaállítás befejeződése után az alábbi [állítsa be az adatbázis helyreállítás][]helyreállított adatbázis is beállíthatja.


## <a name="restore-a-deleted-database"></a>A törölt adatbázis visszaállítása

A törölt adatbázis visszaállítása, használja a [Visszaállítás-AzureRmSqlDatabase][] parancsmag.

1. Nyissa meg a Windows PowerShell.
2. Azure csatlakozni, és a fiókkal társított összes előfizetések listája.
3. Jelölje ki azt az előfizetést, visszaállítani a törölt adatbázist tartalmazó.
4. Ismerkedés a törölt az adatbázis.
5. A törölt adatbázis visszaállítása.
6. Győződjön meg arról, hogy a visszaállított adatbázist online állapotban.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] A visszaállítás befejeződése után az alábbi [állítsa be az adatbázis helyreállítás][]helyreállított adatbázis is beállíthatja.


## <a name="restore-from-an-azure-geographical-region"></a>Állítsa vissza az Azure földrajzi területhez tartozik

Adatbázis helyreállítása, használja a [Visszaállítás-AzureRmSqlDatabase][] parancsmag.

1. Nyissa meg a Windows PowerShell.
2. Azure csatlakozni, és a fiókkal társított összes előfizetések listája.
3. Jelölje ki azt az előfizetést, az adatbázis visszaállítandó tartalmazza.
4. Ismerkedés a helyreállítani kívánt adatbázist.
5. Az adatbázis helyreállítási kiosztása.
6. A geo visszaállított adatbázist állapotának ellenőrzése

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] A visszaállítás befejeződése után adja meg az adatbázist, olvassa el [a helyreállítás adatbázis konfigurálása][]. 


A visszaállított adatbázist TDE engedélyező akkor lesz a forrásadatbázis TDE engedélyezve van.


## <a name="next-steps"></a>Következő lépések
Az üzleti folytonosságot funkciók Azure SQL-adatbázis kiadásainak kapcsolatos további tudnivalókért olvassa el az [Azure SQL-adatbázis üzleti folytonosságot áttekintése][].

<!--Image references-->

<!--Article references-->
[Azure SQL-adatbázis üzleti folytonosságot – áttekintés]: sql-database-business-continuity.md
[Kvóta DTU módosításának kérése]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Az adatbázis konfigurálása helyreállítás]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Telepítse és állítsa be a Azure PowerShell hogyan]: powershell-install-configure.md
[– Áttekintés]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[A PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[TÖBBI]: ./sql-data-warehouse-restore-database-rest-api.md
[Az adatbázis konfigurálása helyreállítás]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Visszaállítás-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[A Microsoft webes Platform telepítő]: https://aka.ms/webpi-azps
