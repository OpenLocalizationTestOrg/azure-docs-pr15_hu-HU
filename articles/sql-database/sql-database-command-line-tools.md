<properties
    pageTitle="A PowerShell Azure SQL-adatbázis kezelése |} Microsoft Azure"
    description="Azure SQL-adatbázis kezelése a PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>A PowerShell Azure SQL-adatbázis kezelése


> [AZURE.SELECTOR]
- [Azure portál](sql-database-manage-portal.md)
- [A Transact-SQL nyelvben (SSMS)](sql-database-manage-azure-ssms.md)
- [A PowerShell](sql-database-manage-powershell.md)

Ez a témakör bemutatja a PowerShell-parancsmagok, amely számos Azure SQL-adatbázissal kapcsolatos feladat végzik. Teljes listáját olvassa el az [Azure SQL-adatbázis parancsmagok](https://msdn.microsoft.com/library/mt574084.aspx)című témakört.


## <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása

Az SQL-adatbázis és a [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837.aspx) parancsmagot a kapcsolódó Azure erőforrások erőforráscsoport létrehozása.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

További tudnivalókért lásd: [Azure PowerShell használatá Azure erőforrás-kezelővel](../powershell-azure-resource-manager.md).
Mintaparancsfájl a [PowerShell-parancsprogramot SQL-adatbázis létrehozása](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)című témakör tartalmaz.

## <a name="create-a-sql-database-server"></a>Hozzon létre egy SQL-adatbázis kiszolgálója

Hozzon létre egy SQL-adatbázis kiszolgálója a [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx) parancsmag. *Kiszolgáló1* cserélje le a a kiszolgáló nevét. Kiszolgáló nevének egyedinek kell lennie a összes Azure SQL-adatbázis-kiszolgálón keresztül. Ha a kiszolgálónév már származik, hibaüzenetet kap. Ez a parancs eltarthat néhány percig, amíg befejezéséhez. Az erőforráscsoport már léteznie kell az előfizetést.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

További tudnivalókért olvassa el a [Mi az SQL-adatbázis](sql-database-technical-overview.md)című témakört. Mintaparancsfájl a [PowerShell-parancsprogramot SQL-adatbázis létrehozása](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)című témakör tartalmaz.


## <a name="create-a-sql-database-server-firewall-rule"></a>SQL-adatbázis-kiszolgáló tűzfal szabály létrehozása

A [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx) parancsmagot a kiszolgáló elérésére tűzfal szabály létrehozása. A következő parancsot, a kezdő és záró IP-címek helyettesítve érvényes értékeket az ügyfél számára. Az erőforráscsoport és a kiszolgáló már léteznie kell az előfizetést.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Más Azure szolgáltatások hozzáférést a kiszolgálóra, a tűzfal szabály létrehozása, és mind a `-StartIpAddress` és `-EndIpAddress` **0.0.0.0**. A speciális tűzfal szabály lehetővé teszi, hogy az összes Azure-alapú forgalmat a kiszolgáló elérésére.

További tudnivalókért lásd: [Azure SQL-adatbázis tűzfalon keresztül](https://msdn.microsoft.com/library/azure/ee621782.aspx). Mintaparancsfájl a [PowerShell-parancsprogramot SQL-adatbázis létrehozása](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)című témakör tartalmaz.


## <a name="create-a-sql-database-blank"></a>Hozzon létre egy SQL-adatbázis (üres)

A [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) parancsmag adatbázis létrehozása. Az erőforráscsoport és a kiszolgáló már léteznie kell az előfizetést. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

További tudnivalókért olvassa el a [Mi az SQL-adatbázis](sql-database-technical-overview.md)című témakört. Mintaparancsfájl a [PowerShell-parancsprogramot SQL-adatbázis létrehozása](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script)című témakör tartalmaz.


## <a name="change-the-performance-level-of-a-sql-database"></a>SQL-adatbázis teljesítmény szintjének módosítása

Méretezze át a [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) parancsmag adatbázis felfelé vagy lefelé. Az erőforráscsoport, a kiszolgáló és az adatbázis már léteznie kell az előfizetést. Állítsa a `-RequestedServiceObjectiveName` egyszerű réteg számára (például a következő kódtöredékének) Szimpla sorköz. Állítsa be a *S0*, *S1*, *P1*, *P6*, például az előző példában az egyéb rétegek stb.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

További tudnivalókért lásd: [SQL-adatbázis beállítások és a teljesítmény: megértéséhez, hogy mi érhető el az egyes szolgáltatási réteg](sql-database-service-tiers.md). Mintaparancsfájl olvassa el a [minta PowerShell-parancsprogramot az SQL-adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database)című témakört.

## <a name="copy-a-sql-database-to-the-same-server"></a>SQL-adatbázis másolása ugyanazon a kiszolgálón

SQL-adatbázis másolása ugyanazon a kiszolgálón a [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644.aspx) parancsmag. Állítsa a `-CopyServerName` és `-CopyResourceGroupName` csoportként a forrás adatbázis-kiszolgáló és az erőforrás azonos értékre.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

További tudnivalókért olvassa el a [Azure SQL-adatbázis másolása](sql-database-copy.md)című témakört. Mintaparancsfájl [másolja az SQL-adatbázis PowerShell-parancsprogramot](sql-database-copy-powershell.md#example-powershell-script)című témakör tartalmaz.


## <a name="delete-a-sql-database"></a>SQL-adatbázis törlése

Az [Eltávolítás-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx) parancsmag SQL-adatbázis törlése Az erőforráscsoport, a kiszolgáló és az adatbázis már léteznie kell az előfizetést.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Egy SQL-adatbázis kiszolgálója törlése

Törölje az [Eltávolítás-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx) parancsmag kiszolgálóval.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Létrehozása és kezelése a PowerShell használatá rugalmas adatbázis-készletek

Létrehozásával kapcsolatban további tájékoztatást rugalmas adatbázis készletek PowerShell használatával olvassa el a [PowerShell új rugalmas adatbázis-készlet létrehozása](sql-database-elastic-pool-create-powershell.md)című témakört.

További információ a PowerShell használatá rugalmas adatbázis-készletek kezelésével kapcsolatban [Monitor és kezelése a PowerShell-rugalmas adatbázis készlet](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Kapcsolódó információ

- [Azure SQL-adatbázis parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx)
- [Azure parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/dn708514.aspx)
