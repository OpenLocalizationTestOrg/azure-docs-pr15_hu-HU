<properties
    pageTitle="Az SQL-adatbázis-kiszolgálói szintű tűzfal szabály beállítása |} Microsoft Azure"
    description="További információ a tűzfalat, melyhez hozzáféréssel Azure SQL server IP-címek konfigurálása."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Egy az Azure-portálon Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabály beállítása


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-firewall-configure.md)
- [Azure portál](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [A PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API-VAL](sql-database-configure-firewall-settings-rest.md)

Azure SQL server tűzfalszabályokat engedélyezze a kiszolgálók és adatbázisok kapcsolatokat használja. Kiszolgáló és adatbázis-szintjének tűzfal a diaminta vagy az adatbázis felhasználói beállításainak meghatározhatja a Azure SQL server logikai Server szelektív engedélyezi a hozzáférést az adatbázishoz. Ez a témakör azt ismerteti, hogy a kiszolgálói szintű tűzfalszabályokat.

> [AZURE.IMPORTANT] Ha engedélyezni szeretné a Azure SQL-kiszolgálóhoz való csatlakozáshoz az Azure alkalmazások, Azure kapcsolatok engedélyeznie kell. Hogyan tűzfalszabályokat munka, című cikkben talál részletes [beállításáról az Azure SQL server-tűzfal \- áttekintése](sql-database-firewall-configure.md). Ha belül az Azure felhő oszlopazonosító kapcsolatok készít, előfordulhat, néhány további TCP-portokat megnyitásához. További tudnivalókért lásd: a **V12 az SQL-adatbázis: belüli és kívüli** [portokon kívül ADO.NET 4.5 és SQL-adatbázis V12 1433](sql-database-develop-direct-route-ports-adonet-v12.md) szakasza

**Javaslat:** Használata kiszolgálói szintű tűzfalszabályokat rendszergazdák számára, és amikor sok adatbázisokra, amelyek az azonos access követelményeik vannak, és nem szeretne időt adatbázisonként egyenként konfigurálása. Microsoft adatbázis szintű tűzfalszabályokat lehetőség, hogy az adatbázis több hordozható és a biztonság fokozása használatát javasolja.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Az Azure portálon keresztül meglévő kiszolgálói szintű tűzfal szabályok kezelése

Ismételje meg a kiszolgálói szintű tűzfalszabályokat kezeléséhez.

- Az aktuális számítógép hozzáadásához kattintson a Hozzáadás ügyfél IP.
- További IP-címek hozzáadásához írja be a szabály nevét, a Start IP-cím és a záró IP-cím.
- Meglévő szabály módosításához kattintson a szabály a mezőkre, és módosítsa.
- Ha törölni szeretne egy meglévő szabályt, az egérmutatót a szabály az X jelenik meg, a sor végére. Kattintson az X, ha el szeretné távolítani a szabályt.

Kattintson a **Mentés** gombra a módosítások mentéséhez.

## <a name="next-steps"></a>Következő lépések

A cikk használatát a Transact-SQL server és adatbázis-szintjének tűzfalszabályokat létrehozása, akkor olvassa el az [Azure SQL-adatbázis konfigurálása kiszolgáló és adatbázis-szintjének tűzfalszabályokat használatával az SQL-T](sql-database-configure-firewall-settings-tsql.md)egy témakört. 

A kiszolgálói szintű tűzfalszabályokat létrehozásáról szóló cikkekre mutató más módszerekkel témakörben olvashat: 

- [Azure SQL-adatbázis kiszolgálói szintű tűzfalszabályokat konfigurálni PowerShell használatával](sql-database-configure-firewall-settings-powershell.md)
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

 
