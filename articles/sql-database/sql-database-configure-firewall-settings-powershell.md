<properties
    pageTitle="Azure SQL-adatbázis-kiszolgálói szintű tűzfalszabályokat állítsa be a PowerShell használatával |} Microsoft Azure"
    description="További információ a tűzfalat, amely az access Azure SQL-adatbázisok IP-címek konfigurálása."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Azure SQL-adatbázis-kiszolgálói szintű tűzfalszabályokat állítsa be a PowerShell használatával


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-firewall-configure.md)
- [Azure portál](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [A PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-VAL](sql-database-configure-firewall-settings-rest.md)


Azure SQL-adatbázis tűzfalszabályokat engedélyezze a kiszolgálók és adatbázisok kapcsolatokat használja. A fő vagy felhasználói adatbázist kiszolgáló és adatbázis-szintjének tűzfal beállításainak is meghatározhatja az SQL-adatbázis kiszolgálója szelektív engedélyezi a hozzáférést az adatbázishoz.

> [AZURE.IMPORTANT] Az adatbázis-kiszolgálóhoz való csatlakozáshoz az Azure alkalmazások engedélyezni kell Azure kapcsolatok. Tűzfalszabályokat és az Azure engedélyezése kapcsolatok kapcsolatos további tudnivalókért lásd: [Azure SQL-adatbázis tűzfalon keresztül](sql-database-firewall-configure.md). Ha belül az Azure felhő oszlopazonosító kapcsolatok készít, előfordulhat, néhány további TCP-portokat megnyitásához. További tudnivalókért lásd: a "V12 SQL-adatbázis: belüli és kívüli" [portokon kívül ADO.NET 4.5 és SQL-adatbázis V12 1433](sql-database-develop-direct-route-ports-adonet-v12.md)szakaszát.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Kiszolgáló tűzfalszabályokat létrehozása

Szabályok létrehozható, kiszolgálói szintű tűzfal frissülnek, és törli a Azure PowerShell használatával.

Új kiszolgálói szintű tűzfal szabály létrehozásához hajtsa végre az [új-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) parancsmag. A következő példa lehetővé teszi, hogy a kiszolgálón Contoso az IP-címeket tartalmazó.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Ha módosítani szeretné egy meglévő kiszolgálói szintű tűzfal szabályt, hajtsa végre a [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) parancsmag. Az alábbi példa a tartomány elfogadható IP-címek a szabály ContosoFirewallRule nevű változik.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Egy meglévő kiszolgálói szintű tűzfal szabály törléséhez hajtsa végre a [eltávolítása-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) parancsmag. A következő példa ContosoFirewallRule nevű szabályt törli.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Tűzfal szabályok kezelése a PowerShell használatával

A PowerShell használatával tűzfal szabályok kezelése. További információ a következő témakörökben olvashatók:

* [Új AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Eltávolítása-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Következő lépések

Arról, hogy miként használhatja a Transact-SQL server- és adatbázis szintjének tűzfalszabályokat létrehozása című témakörből [Azure SQL-adatbázis konfigurálása kiszolgáló és adatbázis-szintjének tűzfalszabályokat az SQL-T használja](sql-database-configure-firewall-settings-tsql.md).

Más módszerekkel kiszolgálói szintű tűzfalszabályokat létrehozásával kapcsolatos további tudnivalókért lásd:

- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni az Azure portál használatával](sql-database-configure-firewall-settings.md)
- [A REST API Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályok konfigurálása](sql-database-configure-firewall-settings-rest.md)

Az adatbázis létrehozására a oktatóanyagban [létrehozása az Azure portálon percben SQL-adatbázishoz](sql-database-get-started.md)című témakör tartalmaz.
A Megnyitás vagy harmadik fél által fejlesztett alkalmazások csatlakoztatása az Azure SQL-adatbázishoz című témakörben kaphat segítséget [ügyfél rövid mintakódok SQL-adatbázishoz](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Adatbázisok navigáljon annak megértéséhez, olvassa el a [adatbázis az access és a bejelentkezési biztonságának kezelési](https://msdn.microsoft.com/library/azure/ee336235.aspx)című témakört.


## <a name="additional-resources"></a>További források

- [Az adatbázis biztonságossá tétele](sql-database-security.md)
- [Biztonság otthon az SQL Server adatbázismotort és Azure SQL-adatbázis](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
