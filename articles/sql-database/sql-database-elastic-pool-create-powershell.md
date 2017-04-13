<properties
    pageTitle="Egy új rugalmas adatbázis-készlet létrehozása a PowerShell |} Microsoft Azure"
    description="Megtudhatja, hogy miként PowerShell-lel való skála kivételének Azure SQL-adatbázis erőforrások: hozzon létre több adatbázis kezelése méretezhető rugalmas adatbázis erőforráskészlethez tartozik."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>A PowerShell rugalmas adatbázis-készlet létrehozása

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-pool-create-portal.md)
- [A PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Megtudhatja, hogyan hozhat létre egy [Rugalmas adatbázis készlet](sql-database-elastic-pool.md) PowerShell-parancsmagok használata. 

Gyakori hibakódok című [SQL-hibakódok az SQL-adatbázis-ügyfélalkalmazásokban: adatbázis-kapcsolati hiba és egyéb problémák](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Rugalmas készletek Észak központi US és a nyugati India, ahol jelenleg előzetes verzióban kivételével az összes Azure régiók szerepelnek, általában elérhető (kiadás).  Az alábbi régiókban rugalmas készletek Georgia nyújtanak minél korábban beállítást. Ezenkívül rugalmas készletek jelenleg nem támogatja [a memóriában OLTP és a memóriában található elemző](sql-database-in-memory.md)használatával adatbázisokat.


1,0 vagy újabb Azure PowerShell futnia kell. Információ megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Új készlet létrehozása

A [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) parancsmag létrehoz egy új készlet. Egy készlet, min és max Dtus eDTU értékének (basic, normál és premium) szolgáltatás réteg érték szerint korlátozott. Lásd: [eDTU és tárolási korlátozza a rugalmas készletek és rugalmas esetén](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Hozzon létre egy új rugalmas adatbázist a készletben

[Új-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) parancsmag, és a **ElasticPoolName** paraméter értéke a cél készletre. Meglévő adatbázis egy készletbe című témakörben [egy rugalmas készletbe adatbázis áthelyezése](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Készlet létrehozása és a több új adatbázisok feltöltéséről 

A sok erőforráskészlethez tartozik az adatbázisok létrehozása után a portálon vagy egyszerre csak egy adatbázis létrehozása PowerShell-parancsmagok használata időt vehet igénybe. Az új készlet létrehozása automatizálása, olvassa el a [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Példa: a PowerShell használatá készlet létrehozása 

Ez a parancsfájl a Azure erőforráscsoport és új kiszolgáló hoz létre. Amikor a rendszer kéri, adja meg a-on használt rendszergazdai felhasználónevével és a jelszót az új kiszolgáló (nem a Azure hitelesítő adatok).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Következő lépések

- [A készlet kezelése](sql-database-elastic-pool-manage-powershell.md)
- [Rugalmas feladatok létrehozása](sql-database-elastic-jobs-overview.md) Rugalmas feladatok lehetővé teszik az SQL-T parancsprogramokat futtassanak adatbázisokat a készletben tetszőleges számú.
- [Azure SQL-adatbázis áttetszőséggel](sql-database-elastic-scale-introduction.md): rugalmas Adatbáziseszközök méretezési szeretne használni.

