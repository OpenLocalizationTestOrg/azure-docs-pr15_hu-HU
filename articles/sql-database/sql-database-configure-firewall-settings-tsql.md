<properties
    pageTitle="Azure SQL-adatbázis kiszolgáló és adatbázis-szintjének tűzfalszabályokat az SQL-T használ |} Microsoft Azure"
    description="További információ a tűzfalat, amely az access Azure SQL-adatbázisok IP-címek konfigurálása."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Azure SQL-adatbázis kiszolgáló és adatbázis-szintjének tűzfalszabályokat az SQL-T használ konfigurálása


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-firewall-configure.md)
- [Azure portál](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [A PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-VAL](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-adatbázis tűzfalszabályokat engedélyezze a kiszolgálók és adatbázisok kapcsolatokat használja. Kiszolgáló és adatbázis-szintjének tűzfal a diaminta vagy az adatbázis felhasználói beállításainak megadására, ha a az Azure SQL-adatbázis azon kiszolgálóját, szelektív engedélyezze a hozzáférést az adatbázishoz.

> [AZURE.IMPORTANT] Az adatbázis-kiszolgálóhoz való csatlakozáshoz az Azure alkalmazások engedélyezni kell Azure kapcsolatok. Tűzfalszabályokat és az Azure engedélyezése kapcsolatok kapcsolatos további tudnivalókért lásd: [Azure SQL-adatbázis tűzfalon keresztül](sql-database-firewall-configure.md). Ha belül az Azure felhő oszlopazonosító kapcsolatok készít, előfordulhat, néhány további TCP-portokat megnyitásához. További tudnivalókért lásd: a **V12 az SQL-adatbázis: belüli és kívüli** [portokon kívül ADO.NET 4.5 és SQL-adatbázis V12 1433](sql-database-develop-direct-route-ports-adonet-v12.md) szakasza


## <a name="server-level-firewall-rules"></a>Kiszolgálói szintű tűzfalszabályokat

Csak a kiszolgálói szintű fő bejelentkezési vagy az Azure Active Directory rendszergazda is kiszolgálói szintű tűzfal szabály létrehozása a Transact-SQL nyelvben.

1. Indítsa el a lekérdezés ablak, és Csatlakozás SQL Server Management Studio segítségével a virtuális fő adatbázist.
2. Kiszolgálói szintű tűzfalszabályokat kijelölt, létrehozott, frissítése vagy törlődik a lekérdezés ablak belül.
3. Szabályok létrehozása vagy módosítása kiszolgálói szintű tűzfal, hajtsa végre a `sp_set_firewall_rule` tárolt eljárás. A következő példa lehetővé teszi, hogy a kiszolgálón Contoso az IP-címeket tartalmazó.<br/>Kezdje azzal, hogy milyen szabályok már láthatók.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Ezután a tűzfal szabály hozzáadása lehetőségre.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    A kiszolgálói szintű tűzfal szabály törléséhez hajtsa végre a sp_delete_firewall_rule tárolt eljárás. A következő példa ContosoFirewallRule nevű szabályt törli.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 E tárolt eljárások kapcsolatos további tudnivalókért olvassa el a [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) és [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)című témakört.

## <a name="database-level-firewall-rules"></a>Adatbázis-szintű tűzfalszabályokat

Csak egy adatbázis-felhasználó az adatbázisban (például az adatbázis tulajdonosa) **hozzáféréssel** rendelkező adatbázis szintű tűzfal szabály hozhat létre.

1. Miután létrehozta a IP-cím kiszolgálói szintű tűzfalat, indítsa el a lekérdezés ablak vagy SQL Server Management Studio a klasszikus portálon keresztül.
2. Csatlakozás az adatbázis szintű tűzfal szabályt szeretne létrehozni kívánt adatbázist.

    Hozzon létre egy új vagy meglévő adatbázis szintű tűzfal szabály frissítése, hajtsa végre a `sp_set_database_firewall_rule` tárolt eljárás. Az alábbi példa létrehoz nevű ContosoFirewallRule tűzfal új szabályt.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Ha törölni szeretne egy meglévő adatbázis szintű tűzfal szabályt, hajtsa végre a `sp_delete_database_firewall_rule` tárolt eljárás. A következő példa ContosoFirewallRule nevű szabályt törli.
`
   VÉGREHAJTÁSI sp_delete_database_firewall_rule @name = N'ContosoFirewallRule "

E tárolt eljárások kapcsolatos további tudnivalókért olvassa el a [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) és [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx)című témakört.

## <a name="next-steps"></a>Következő lépések

A kiszolgálói szintű tűzfalszabályokat létrehozásáról szóló cikkekre mutató más módszerekkel témakörben olvashat: 

- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni az Azure-portálon](sql-database-configure-firewall-settings.md)
- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni PowerShell használatával](sql-database-configure-firewall-settings-powershell.md)
- [A REST API Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályok konfigurálása](sql-database-configure-firewall-settings-rest.md)

Az adatbázis létrehozására a oktatóanyagban [létrehozása az Azure portálon percben SQL-adatbázishoz](sql-database-get-started.md)című témakör tartalmaz.
A Megnyitás vagy harmadik fél által fejlesztett alkalmazások csatlakoztatása az Azure SQL-adatbázishoz című témakörben kaphat segítséget [ügyfél rövid mintakódok SQL-adatbázishoz](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Adatbázisok navigáljon annak megértéséhez, olvassa el a [adatbázis az access és a bejelentkezési biztonságának kezelési](https://msdn.microsoft.com/library/azure/ee336235.aspx)című témakört.


## <a name="additional-resources"></a>További források

- [Az adatbázis biztonságossá tétele](sql-database-security.md)
- [Biztonság otthon az SQL Server adatbázismotort és Azure SQL-adatbázis](https://msdn.microsoft.com/library/bb510589)
