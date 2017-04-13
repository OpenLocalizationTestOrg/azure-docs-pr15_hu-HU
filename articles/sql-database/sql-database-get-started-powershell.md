<properties
    pageTitle="A PowerShell új SQL-adatbázis beállítása |} Microsoft Azure"
    description="Olvassa el a most PowerShell SQL-adatbázis létrehozása. Adatbázis-beállítási gyakori feladatok kezelhető PowerShell-parancsmagok között."
    keywords="Hozzon létre új sql-adatbázishoz adatbázis beállítása"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>SQL-adatbázis létrehozása, és hajtsa végre a PowerShell-parancsmagok közös adatbázis beállítási feladatok


> [AZURE.SELECTOR]
- [Azure portál](sql-database-get-started.md)
- [A PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Megtudhatja, hogy miként hozhat létre az SQL-adatbázis PowerShell-parancsmagok használatával. (Rugalmas adatbázisok létrehozására, lásd: [a PowerShell új rugalmas adatbázis-készlet létrehozása](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Adatbázis beállítása: egy erőforráscsoport, a kiszolgáló és a szabály létrehozása

Ha befejezte az access parancsmagok futtassanak kijelölt Azure előfizetését, a következő lépésben az erőforráscsoport, amely tartalmazza a hol is létrejön az adatbázis-kiszolgáló meghatározása. A következő parancs a kiválasztott használja bármely érvényes helyet szerkesztheti. Futtassa a **(Get-AzureRmLocation |} Hol-objektum {$_. Szolgáltatók - eq "Microsoft.Sql"}). Hely** érvényes helyek listájának eléréséhez.

Futtassa az erőforrás csoport létrehozásához a következő parancsot:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Kiszolgáló létrehozása

SQL-adatbázisait készült Azure SQL-adatbázis-kiszolgálók belül. Futtassa az [új-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) kiszolgálót hoz létre. A kiszolgáló nevét az összes Azure SQL-adatbázis kiszolgálója egyedinek kell lennie. Ha a kiszolgálónév már származik, hibaüzenetet kap. Is megjegyezni, hogy ez a parancs eltarthat néhány percig. A parancs, amellyel bármely érvényes hely úgy dönt, szerkesztheti, de kell használnia az erőforráscsoport az előző lépésben létrehozott használt ugyanazon a helyen.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Ha ezt a parancsot futtatja, a rendszer kéri felhasználónevét és jelszavát. Nem adja meg az Azure hitelesítő adatait. Ehelyett, adja meg a felhasználónevet és jelszót létrehozni, a kiszolgáló rendszergazdája. Ez a cikk alján a parancsfájl szemlélteti, hogyan lehet módosítani szeretné a kiszolgáló hitelesítő kódot.

A kiszolgáló részletei jelennek meg, a kiszolgáló sikeres létrehozását követően.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Állítsa be a kiszolgáló tűzfal szabályt a kiszolgálóra való hozzáférés engedélyezése

A kiszolgáló elérésére kell tűzfal szabály létrehozása. Futtassa az [új-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) parancs, a kezdő és záró IP-címek helyettesítve érvényes értékeket a számítógépen.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

A tűzfal szabály részletei jelennek meg a szabály sikeres létrehozását követően.

A kiszolgáló elérésére egyéb Azure szolgáltatás engedélyezéséhez tűzfal szabály hozzáadása és beállítása a StartIpAddress és a EndIpAddress 0.0.0.0. Ez a szabály lehetővé teszi, hogy a kiszolgáló elérésére bármely Azure előfizetésből Azure forgalmat.

További tudnivalókért lásd: [Azure SQL-adatbázis tűzfalon keresztül](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

Most már van egy erőforráscsoport, a kiszolgáló és elérheti a kiszolgálón beállított tűzfal szabály.

A következő [új-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) parancs az (üres) SQL-adatbázishoz, a szokásos szolgáltatás réteg a egy S1 teljesítményszint hoz létre:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Az adatbázis részletei jelennek meg, az adatbázis sikeres létrehozását követően.

## <a name="create-a-sql-database-powershell-script"></a>Egy PowerShell-parancsprogramot SQL-adatbázis létrehozása

Az alábbi PowerShell parancsprogramot létrehoz egy SQL-adatbázis és az összes függő erőforrás. Az összes cseréje `{variables}` jellemző az előfizetést és erőforrások (távolítsa el a **{}** , az értékek beállításakor) értékű.

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Következő lépések
Miután egy SQL-adatbázis létrehozása és alapvető adatbázis beállítási feladatok végrehajtására, készen áll a következő:

- [A PowerShell SQL-adatbázis kezelése](sql-database-manage-powershell.md)
- [Az SQL Server Management Studio SQL-adatbázis csatlakoztatása, és hajtsa végre a minta T az SQL lekérdezés](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>További források

- [Az SQL azure-adatbázis parancsmagok] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure SQL-adatbázis](https://azure.microsoft.com/documentation/services/sql-database/)
