<properties 
    pageTitle="Másolja az Azure SQL-adatbázis PowerShell használatával |} Microsoft Azure" 
    description="Készítsen másolatot a PowerShell használatá Azure SQL-adatbázishoz" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Másolja az Azure SQL-adatbázis PowerShell használatával


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-copy.md)
- [Azure portál](sql-database-copy-portal.md)
- [A PowerShell](sql-database-copy-powershell.md)
- [AZ SQL-T](sql-database-copy-transact-sql.md)

Ez a cikk bemutatja, hogyan PowerShell SQL-adatbázis másolása ugyanarra a kiszolgálóra, egy másik kiszolgálóra, vagy egy adatbázis másol egy [Rugalmas adatbázis készlet](sql-database-elastic-pool.md). Az adatbázis-példány művelet a [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) parancsmag használja. 


Ez a cikk a következőkre lesz szüksége:

- Azure SQL-adatbázis (adatbázis másolása). Ha nem rendelkezik egy SQL-adatbázissal, hozzon létre egy az ebben a cikkben leírt lépéseket követve: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).
- Azure PowerShell legújabb verzióját. Információ megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).


SQL-adatbázis számos új szolgáltatást csak akkor támogatottak, az [erőforrás-kezelő Azure telepítési modell](../azure-resource-manager/resource-group-overview.md), így például a [Azure SQL-adatbázis PowerShell-parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx) használata az erőforrás-kezelő használata esetén. A meglévő klasszikus telepítési modell [(klasszikus) Azure SQL-adatbázis-parancsmagok](https://msdn.microsoft.com/library/azure/dn546723.aspx) használata támogatott, a visszamenőleges kompatibilitás, de javasoljuk, hogy az erőforrás-kezelő parancsmagok használata.


>[AZURE.NOTE] Attól függően, hogy az adatbázis mérete a Másolás eltarthat egy kis időt befejezéséhez.


## <a name="copy-a-sql-database-to-the-same-server"></a>SQL-adatbázis másolása ugyanazon a kiszolgálón

A másolat létrehozása ugyanazon a kiszolgálón, hagyja ki az `-CopyServerName` paraméter (vagy állítsa be ugyanazt a kiszolgálót).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>SQL-adatbázis másolása egy másik kiszolgálóra

Egy másik kiszolgálón hozza létre a másolás, helyezze el a `-CopyServerName` paraméter, és állítsa be egy másik kiszolgálóra. A *Másolás* kiszolgáló már léteznie kell. Ha egy másik erőforráscsoport a, majd a fel kell venni a `-CopyResourceGroupName` paraméter.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>SQL-adatbázis másol egy rugalmas adatbázis készlet

SQL-adatbázis biztonsági másolatának létrehozása erőforráskészlethez tartozik, állítsa be a `-ElasticPoolName` paraméter egy meglévő csoportba.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Bejelentkezések feloldása

A másolási művelet befejezése után bejelentkezések feloldásához lásd: a [bejelentkezések feloldása](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Példa PowerShell-parancsprogramot

A következő parancsfájl feltételezi, hogy az összes erőforráscsoport kiszolgálók, és a készlet már léteznie kell (változó értékeket cserélje ki a meglévő erőforrásokat). A szolgáltatás léteznie kell, az adatbázis-példány kivételével.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Következő lépések

- Lásd: [Azure SQL-adatbázishoz másolása](sql-database-copy.md) Azure SQL-adatbázis másolása áttekintése.
- Lásd: [az Azure portálon Azure SQL-adatbázisból másolása](sql-database-copy-portal.md) az Azure portálon adatbázis másolása.
- Lásd: a [Másolás egy Azure SQL-adatbázis használata az SQL-T](sql-database-copy-transact-sql.md) a Transact-SQL-adatbázisokkal másolásához.
- [Azure SQL-adatbázis biztonsági után vészhelyreállítás kezelése](sql-database-geo-replication-security-config.md) című témakörben talál tudnivalókat a felhasználók és a bejelentkezés egy másik logikai-kiszolgáló adatbázis másolásakor témakörben találhat.


## <a name="additional-resources"></a>További források

- [Új AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Bejelentkezés kezelése](sql-database-manage-logins.md)
- [Az SQL Server Management Studio SQL-adatbázis csatlakoztatása, és hajtsa végre a minta T az SQL lekérdezés](sql-database-connect-query-ssms.md)
- [Az adatbázis exportálása egy BACPAC](sql-database-export.md)
- [Üzleti folytonosságot – áttekintés](sql-database-business-continuity.md)
- [SQL-adatbázis dokumentáció](https://azure.microsoft.com/documentation/services/sql-database/)
- [Az SQL Azure adatbázis PowerShell parancsmagjai – referencia](https://msdn.microsoft.com/library/mt574084.aspx)
