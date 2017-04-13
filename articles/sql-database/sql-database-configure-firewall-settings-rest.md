<properties
    pageTitle="Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat a REST API |} Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>A REST API Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályok konfigurálása


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-firewall-configure.md)
- [Azure portál](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [A PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-VAL](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-adatbázis tűzfalszabályokat engedélyezze a kiszolgálók és adatbázisok kapcsolatokat használja. Kiszolgáló és adatbázis-szintjének tűzfal a diaminta vagy az adatbázis felhasználói beállításainak megadására, ha a az Azure SQL-adatbázis azon kiszolgálóját, szelektív engedélyezze a hozzáférést az adatbázishoz.

> [AZURE.IMPORTANT] Az adatbázis-kiszolgálóhoz való csatlakozáshoz az Azure alkalmazások engedélyezni kell Azure kapcsolatok. Tűzfalszabályokat és az Azure engedélyezése kapcsolatok kapcsolatos további tudnivalókért lásd: [Azure SQL-adatbázis tűzfalon keresztül](sql-database-firewall-configure.md). Ha belül az Azure felhő oszlopazonosító kapcsolatok készít, előfordulhat, néhány további TCP-portokat megnyitásához. További tudnivalókért lásd: a **V12 az SQL-adatbázis: belüli és kívüli** [portokon kívül ADO.NET 4.5 és SQL-adatbázis V12 1433](sql-database-develop-direct-route-ports-adonet-v12.md) szakasza


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Kiszolgálói szintű tűzfalszabályokat REST API-től kezelése
1. REST API-től tűzfalszabályokat kezelése hitelesíteni kell. Információ [fejlesztői](../resource-manager-api-authentication.md)útmutatójában engedély a Azure erőforrás-kezelő API-val.
2. Kiszolgálói szintű szabályok létrehozható, frissített vagy törölt REST API segítségével

    Hozzon létre, és frissítheti a kiszolgálói szintű tűzfal szabály, hajtsa végre a helyezése módszerrel a következő:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Szervezet kérése

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Egy meglévő kiszolgálói szintű tűzfal szabály eltávolításához hajtsa végre a törlés módszerrel a következő:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>A REST API tűzfal szabályok kezelése

* [Létrehozása vagy módosítása a szabály](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Szabály törlése](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Szabály beszerzése](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [A lista összes tűzfalszabályokat](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Következő lépések

A cikk használatát a Transact-SQL server és adatbázis-szintjének tűzfalszabályokat létrehozása, akkor olvassa el az [Azure SQL-adatbázis konfigurálása kiszolgáló és adatbázis-szintjének tűzfalszabályokat használatával az SQL-T](sql-database-configure-firewall-settings-tsql.md)egy témakört. 

A kiszolgálói szintű tűzfalszabályokat létrehozásáról szóló cikkekre mutató más módszerekkel témakörben olvashat: 

- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni az Azure portál használatával](sql-database-configure-firewall-settings.md)
- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni PowerShell használatával](sql-database-configure-firewall-settings-powershell.md)

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

 
