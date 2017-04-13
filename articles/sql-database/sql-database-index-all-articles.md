<properties
    pageTitle="SQL-adatbázis szolgáltatás összes témakörök |} Microsoft Azure"
    description="Táblázat összes témakörök az Azure szolgáltatás http://azure.microsoft.com/documentation/articles/, a cím és leírás megtalálható SQL-adatbázis neve."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Az összes témakörök Azure SQL-adatbázis-szolgáltatás

Ez a témakör felsorolja minden témakör, amely közvetlenül az Azure **SQL-adatbázis** szolgáltatás vonatkozik. Kulcsszavak a weblap az aktuális érdeklődésre számot tartó témakörök keresése a **Ctrl + F**, használatával is kereshet.




## <a name="new"></a>Új

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 1 | [Daxko/CSI Azure használják, hogy felgyorsítsa az fejlesztési ciklus és ügyfélszolgálat és a teljesítmény javítása érdekében](sql-database-implementation-daxko.md) | Megismerheti, hogyan Daxko/CSI és használ az SQL-adatbázis hogy felgyorsítsa az fejlesztési ciklus ügyfélszolgálat és a teljesítmény javítása érdekében |
| 2 | [Azure ad-GEP globális vannak, és a nagyobb hatékonyságot](sql-database-implementation-gep.md) | Megismerheti, hogyan GEP használja a SQL-adatbázis eléréséhez több globális ügyfelek, és a nagyobb hatékonyságot elérése |
| 3 | [Az Azure-SnelStart gyorsan bővített vállalati verzió szolgáltatásai és 1000 új Azure SQL-adatbázisait havonta mértéke](sql-database-implementation-snelstart.md) | Megismerheti, hogyan SnelStart használja a vállalati verzió szolgáltatásai és 1000 új Azure SQL-adatbázisait havonta mértéke gyorsan kibontott SQL-adatbázis |
| 4 | [Umbraco Azure SQL-adatbázis segítségével gyorsan a felhőben bérlők ezer rendelkezés és arányának szolgáltatások](sql-database-implementation-umbraco.md) | Hogyan Umbraco használja gyorsan hozhatók létre SQL-adatbázis és a felhőben bérlők ezer skála szolgáltatások ismertetése |
| 5 | [A PowerShell Azure SQL-adatbázis kezelése](sql-database-manage-powershell.md) | Azure SQL-adatbázis kezelése a PowerShell. |
| 6 | [Azure Active Directory MFA SQL-adatbázis és SQL adatraktár SSMS támogatása](sql-database-ssms-mfa-authentication.md) | Több figyelembe venni hitelesítési használata SSMS SQL-adatbázis és SQL adatraktár. |
| 7 | [Adatbázis-tranzakción egységek (DTUs) és rugalmas adatbázis tranzakció egységek (eDTUs)](sql-database-what-is-a-dtu.md) | Mi az Azure SQL-adatbázis ismertetése tranzakció mértékegysége. |


## <a name="updated-articles-sql-database"></a>Frissített cikkek, SQL-adatbázis

Ez a szakasz tárgyak, amelyek a frissített nemrégiben, ha a frissítés nagy, vagy jelentős lett-e. Az egyes frissített cikk durva kódtöredékének, a hozzáadott markdown szöveg jelenik meg. A cikkek lettek frissítve a dátumtartomány **2016-08-22-es** való **2016 – 10-05**belül.

| &nbsp; | A cikk | Módosított szöveget, kódtöredékének | Mikor frissülnek |
| --: | :-- | :-- | :-- |
| 8 | [A PowerShell Azure SQL-adatbázis kezelése](sql-database-command-line-tools.md) | Az SQL-adatbázis és a New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx) parancsmagot a kapcsolódó Azure erőforrások erőforráscsoport létrehozása. *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" új-AzureRmResourceGroup-$resourceGroupName név-hely $resourceGroupLocation* További tudnivalókért lásd: Azure PowerShell használatá Azure erőforrás-kezelővel (... / powershell-azure-erőforrás-manager.md). Mintaparancsfájl a PowerShell-parancsprogramot (sql-adatbázis-get-lépések-powershell.md create-a-sql-database-powershell-script) SQL-adatbázis létrehozása című témakör tartalmaz. **Hozzon létre egy SQL-adatbázis-kiszolgálót** Hozzon létre egy SQL-adatbázis kiszolgálója a New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx) parancsmag. *Kiszolgáló1* cserélje le a a kiszolgáló nevét. Kiszolgáló nevének egyedinek kell lennie a összes Azure SQL-adatbázis-kiszolgálón keresztül. Ha már vett a a kiszolgáló nevét, hibaüzenetet kap. Ez a parancs eltarthat néhány percig, amíg befejezéséhez. A resou | 2016-09-14 |
| 9 | [Másolja az Azure SQL-adatbázis PowerShell használatával](sql-database-copy-powershell.md) |   SQL adatbázis forrása (másolása a meglévő adatbázis) – $sourceDbName = "D1" $sourceDbServerName = "KISZOLGALO1" $sourceDbResourceGroupName = "rg1" SQL-adatbázis másolása (az új adatbázis hozható létre) – $copyDbName = "db1_copy" $copyDbServerName = "kiszolgáló2" $copyDbResourceGroupName = "rg2" másolása ugyanazon a kiszolgálón – új-AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName adatbázis - kiszolgálónév $sourceDbServerName - adatbázisnév $sourceDbName - CopyDatabaseName $copyDbName adatbázis másolása egy másik kiszolgálóra – új-AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - kiszolgálónév $sourceDbServerName - adatbázisnév $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName adatbázis másol egy rugalmas adatbázis készlet – $poolName = "pool1" új-AzureRmSqlDatabaseCopy - ResourceGroupName $ sourceDbResourceGroupName - kiszolgálónév $sourceDbServerName - adatbázisnév $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Egy rugalmas adatbázis-készlet létrehozása a és C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine("a(z) ("kiszolgáló tűzfal...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine("a(z) (" kiszolgáló tűzfal: "+ fwr. FirewallRule.Id);  Console.WriteLine("a(z) ("rugalmas készlet...");  ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine("a(z) (" rugalmas készlet: "+ epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine("a(z) (" adatbázis: "+ dbr. Database.Id);  Console.WriteLine("a(z) ("nyomja meg bármelyik gombot továbbra is...");  Console.ReadKey();  } statikus ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016-09-14 |
| 11 | [Az adatbázisok egy rugalmas adatbázis készlet alkalmas azonosító PowerShell-parancsprogramot](sql-database-elastic-pool-database-assessment-powershell.md) | Rugalmas az adatbázis-készlet **jelöltek, azt kizárja az fő- és bármely adatbázisok már erőforráskészlethez tartozik. Az alábbi kizárt listához további adatbázisok szükség szerint vehet fel** $ListOfDBs = Get-AzureRmSqlDatabase - kiszolgálónév $servername. Split('.') 0 - ResourceGroupName $ResourceGroupName / hol-objektum {$_. Adatbázisnév - notin ("fő") - és $_. CurrentServiceLevelObjectiveName - notin ("ElasticPool")- és $_. CurrentServiceObjectiveName - notin ("ElasticPool")} $outputConnectionString = "adatforrás $outputServerName; = integrált adatvédelem = false; kezdeti katalógus = $outputdatabaseName; Felhasználói azonosító = $outputDBUsername; Jelszó = $outputDBpassword "$destinationTableName ="resource_stats_output" **Mértékek gyűjtemény kimeneti adatbázis létrehozása** $sql =" Ha NOT EXISTS (VÁLASSZA a * sys.objects a WHERE object_id = OBJECT_ID(N'$($destinationTableName)'), és írja be a (N'U ")) KEZDŐ tábla létrehozása $($destinationTableName) (adatbázisnév varchar(128) zat varchar(20), end_time datetime, avg_cpu lebegtetés, avg_ | 2016-09-29 |
| 12 | [SQL-adatbázis oktatóprogram: perc egy SQL-adatbázis létrehozása az Azure portál használatával](sql-database-get-started.md) |   ! Új sql-adatbázis 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Kattintson az **SQL-adatbázissal** az SQL-adatbázis lap megnyitásához. Ez a lap tartalmának az előfizetések és a meglévő objektumok (például a meglévő-kiszolgálók) számától függően változik.  ! Új sql-adatbázis 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. **Az adatbázis neve** mezőbe nevezze el az első adatbázis – például a "saját adatbázis". Egy zöld pipa jelzi, hogy meg van adva egy érvényes nevet.  ! Új sql-adatbázis 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Ha több előfizetéssel rendelkezik, jelöljön ki egy előfizetést. 6. **Erőforráscsoport**csoportban kattintson a **Létrehozás új** gombra, és nevezze el az első erőforráscsoport – például a "saját-erőforrás-csoport". Egy zöld pipa jelzi, hogy meg van adva egy érvényes nevet.  ! Új sql-adatbázis 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. A **Jelölje be a forrás* | 2016-09-08 |
| 13-mal | [Próbálja meg az SQL-adatbázis: Használata C# SQL-adatbázis létrehozása az SQL-adatbázis tárral a .NET rendszerhez](sql-database-get-started-csharp.md) | **Egy egyszerű erőforrások eléréséhez szolgáltatás létrehozása** Az alábbi PowerShell parancsprogramot hoz létre, az Active Directory (AD) alkalmazás és a szolgáltatás egyszerű, amely a C alkalmazás hitelesítő szükség. A parancsprogram szükséges az előző C minta értékeket exportálja. Részletes tudnivalókért lásd: Azure PowerShell használatá hozzon létre egy egyszerű erőforrások eléréséhez (... / erőforrás-csoport-hitelesítést végezni-szolgáltatás – principal.md).  Jelentkezzen be az Azure.  Adja hozzá AzureRmAccount esetén több előfizetéssel, vegye ki a megjegyzésjeleket, és adja meg az előfizetéshez, amellyel dolgozni szeretne.  $subscriptionId "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" = Set-AzureRmContext - SubscriptionId $subscriptionId szükséges ezeket az értékeket az új AAD alkalmazást.  $appName az alkalmazás a megjelenítendő név, a címtárában egyedinek kell lennie.  $uri nem kell valós uri.  $secret hoz létre jelszót.  $appName = "{alkalmazás neve}" $uri = "http://{app-name}" $secret = "{alkalmazás-jelszó}" létrehozása egy AAD alkalmazás $azureAdApplication új-AzureRmADApp = | 2016-09-23 |
| 14 | [Az Azure portálon Azure SQL-adatbázisok kezelése](sql-database-manage-portal.md) | Megtekintéséhez, vagy az adatbázis-beállítások frissítése, kattintson a kívánt beállítást a SQL-adatbázis lap:! SQL-adatbázis beállítások (. / media/sql-database-manage-portal/settings.png) **hogyan egy SQL-adatbázisok teljesen minősített kiszolgáló nevének megállapítása?** Az adatbázis-kiszolgáló nevének az **SQL-adatbázis** lap az **Áttekintés** hivatkozásra, és jegyezze fel a kiszolgáló neve:! SQL-adatbázis beállítások (. / media/sql-database-manage-portal/server-name.png) **hogyan tűzfalszabályokat való hozzáférés korlátozása az SQL server és a adatbázis kezelése?** Megtekintése, létrehozása és frissítése tűzfalszabályokat, **állítsa a kiszolgáló tűzfal** gombjára kattintva az **SQL-adatbázis** lap. A részletekért lásd: a Configure egy Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályt, az Azure portálon (sql-adatbázis-konfigurálása-tűzfal-settings.md). ! a tűzfal szabályok (. / media/sql-database-manage-portal/sql-database-firewall.png) **Hogyan módosíthatom a SQL adatbázis szolgáltatás réteg vagy a teljesítmény szintjét?** A szolgáltatás réteg vagy a teljesítmény szintet SQL-adatbázis frissítéséhez kattintson ** árak réteg (vasárnap | 2016-09-20 |





## <a name="get-started"></a>Első lépések

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 15 | [Hozza létre a több elem bérlői alkalmazások elkülönítési és a hatékonyság az Azure SQL-adatbázissal](sql-database-build-multi-tenant-apps.md) | Megtudhatja, hogyan hozza létre a SQL-adatbázis több bérlői alkalmazások |
| 16 | [Az SQL Server Management Studio SQL-adatbázis csatlakoztatása, majd hajtsa végre a minta T az SQL lekérdezés](sql-database-connect-query-ssms.md) | További információ a Azure SQL-adatbázis csatlakoztatása az SQL Server Management Studio (SSMS) használatával. Futtassa a Transact-SQL nyelvben (T-SQL) használ lekérdezéssel minta parancsra. |
| 17 | [SQL-adatbázis csatlakoztatása a .NET (C#) használatával](sql-database-develop-dotnet-simple.md) | Használata ezt a rövid a minta kódot a és C# modern alkalmazás összeállítása és hatékony relációs adatbázis a felhőben Azure SQL-adatbázis biztonsági. |
| 18 | [Rugalmas adatbázis-készlet létrehozása az Azure portálján](sql-database-elastic-pool-create-portal.md) | Hogyan lehet az SQL-adatbázis beállítások könnyebben felügyelete és erőforrás-adatbázisok számos különböző megosztó méretezhető rugalmas adatbázis erőforráskészlethez tartozik hozzáadni. |
| 19 | [Ismerje meg az SQL Azure-adatbázis oktatóanyagok](sql-database-explore-tutorials.md) | SQL-adatbázis-szolgáltatások és funkciók ismertetése |
| 20 | [SQL-adatbázis oktatóprogram: perc egy SQL-adatbázis létrehozása az Azure portál használatával](sql-database-get-started.md) | Megtudhatja, hogy miként állíthat be egy logikai SQL-adatbázis azon kiszolgálóját, a kiszolgáló tűzfal szabályt, az SQL-adatbázis és a mintaadatok. Ezenkívül megtudhatja, hogy miként csatlakozhat az ügyféleszközök elől, állítsa be a felhasználók és adatbázis tűzfal szabály beállítása. |
| 21 | [Azure SQL-adatbázis védelemmel látja el, és védi](sql-database-helps-secures-and-protects.md) | Megtudhatja, hogyan SQL-adatbázis segít biztonságos és védelme |
| 22-es hibakód | [Azure SQL-adatbázis folyamatosan tanul &amp; alkalmazkodik](sql-database-learn-and-adapt.md) | Ismerje meg, hogyan SQL-adatbázis folyamatosan tanul és alkalmazkodik |
| 23 | [Válassza az SQL Server lehetőség felhő: (PaaS) Azure SQL-adatbázis vagy SQL Server Azure VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Ismerje meg, hogy melyik felhő SQL Server lehetőséget az alkalmazás illeszkedik: (PaaS) Azure SQL-adatbázis vagy a felhőben a Azure virtuális gépeken futó SQL Server. |
| 24 | [Azure SQL-adatbázis értékekhez menet közben](sql-database-scale-on-the-fly.md) | Megtudhatja, hogyan menet méretezze át SQL-adatbázis |
| 25 | [Ismerje meg az SQL Azure adatbázis megoldás gyors indítása](sql-database-solution-quick-starts.md) | További tudnivalók: Azure SQL-adatbázis megoldások |
| 26 | [A környezetben működik Azure SQL-adatbázis](sql-database-works-in-your-environment.md) | Ismerje meg, hogyan SQL-adatbázis segít, titkosítja és védi |



## <a name="build-an-app"></a>Alkalmazás összeállítása

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 27 | [Hibaelhárítás diagnosztizálása és megakadályozhatja, hogy az SQL-kapcsolat hibák és tranziens hibák SQL-adatbázis](sql-database-connectivity-issues.md) | Megtudhatja, hogy miként kapcsolatos hibák elhárítása diagnosztizálása és megakadályozhatja, hogy egy SQL-kapcsolati hiba vagy a ideiglenes (tranziens) hiba Azure SQL-adatbázisban.  |
| 28 | [Csatlakozás a Visual Studio SQL-adatbázishoz](sql-database-connect-query.md) | Írja be a program lekérdezéséhez és SQL-adatbázis csatlakoztatása a C#. IP-címek, kapcsolati karakterláncot, biztonságos bejelentkezés és ingyenes Visual Studio információt. |
| 29 | [ADO.NET 4.5 és SQL-adatbázis V12 1433 túl portok](sql-database-develop-direct-route-ports-adonet-v12.md) | Azure SQL-adatbázis V12 a ADO.NET-ügyfél kapcsolatot előfordul, hogy a proxy átlépése, és közvetlenül az adatbázis használhatják. Portokon kívül 1433 fontos válnak. |
| 30 | [SQL-hibakódok az SQL-adatbázis-ügyfélalkalmazásokban: adatbázis-kapcsolati hiba és egyéb problémák](sql-database-develop-error-messages.md) | Tudjon meg többet az SQL-adatbázis ügyfélalkalmazásokban, például adatbázis kapcsolat kapcsolatos gyakori hibákra, az adatbázis-példány problémák és a általános hibák SQL-hibakódok.  |
| 31. | [SQL-adatbázis csatlakoztatása a Windows PHP használatával](sql-database-develop-php-simple.md) | Hogyan jeleníti meg a ügyfélről Windows Azure SQL-adatbázishoz csatlakozik, és az ügyfél szükséges szoftvert összetevővel mutató hivatkozások minta PHP programot. |
| 32 | [Próbálja meg az SQL-adatbázis: Használata C# SQL-adatbázis létrehozása az SQL-adatbázis tárral a .NET rendszerhez](sql-database-get-started-csharp.md) | Próbálja ki az SQL-adatbázis SQL a és C# alkalmazások fejlesztésével, és hozzon létre egy SQL Azure-adatbázis C# az SQL-adatbázis tár használata a .NET rendszerhez. |
| 33 | [Azure SQL-adatbázisban JSON szolgáltatások – első lépések](sql-database-json-features.md) | Azure SQL-adatbázis elemzési, lekérdezések és adatok formázása a JavaScript objektum jelölés (JSON) jelöléssel lehetővé teszi. |
| 34 | [Azure SQL-adatbázishoz kapcsolódási problémák elhárítása](sql-database-troubleshoot-common-connection-issues.md) | Azonosítása és megoldása a kapcsolat kapcsolatos gyakori hibákra Azure SQL-adatbázis lépéseket. |
| 35-tel | [Hiba "Adatbázis-kiszolgálón jelenleg nem áll rendelkezésre" sql-adatbázishoz való csatlakozáskor](sql-database-troubleshoot-connection.md) | Az adatbázis elhárítása a kiszolgáló nem áll jelenleg elérhető hibák céljával SQL-adatbázishoz csatlakozik. |



## <a name="develop"></a>Kidolgozása

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 36 | [A szükséges értékeket beszerzése hitelesítő kódot az SQL-adatbázis eléréséhez az alkalmazások](sql-database-client-id-keys.md) | Hozzon létre egy egyszerű kódból SQL-adatbázis eléréséhez. |
| 37 | [SQL-adatbázis csatlakoztatása a Windows JDBC Java használatával](sql-database-develop-java-simple.md) | Azure SQL-adatbázishoz való csatlakozáshoz használhatja Java kód minta jeleníti meg. A minta használ JDBC, és a Windows ügyfélszámítógépen fusson. |
| 38 | [Csatlakozás SQL-adatbázis Node.js használatával](sql-database-develop-nodejs-simple.md) | Azure SQL-adatbázishoz való csatlakozáshoz használhatja Node.js kód minta jeleníti meg. |
| 39 | [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md) | Tudnivalók a rendelkezésre álló kapcsolódási tárak és a gyakorlati tanácsok az SQL-adatbázis csatlakoztatása az alkalmazások. |
| 40 | [Csatlakozás SQL-adatbázis Python használatával](sql-database-develop-python-simple.md) | Azure SQL-adatbázishoz való csatlakozáshoz használhatja Python kód minta jeleníti meg. |
| 41 | [SQL-adatbázis csatlakoztatása a fonetikus használatával](sql-database-develop-ruby-simple.md) | Azure SQL-adatbázis csatlakoztatása futtatva fonetikus kód minta ad. |
| 42 | [Azure SQL-adatbázisban időbeli táblázatok – első lépések](sql-database-temporal-tables.md) | Megtudhatja, hogy miként veheti használatba az Azure SQL-adatbázis időbeli tábláinak használatával. |



## <a name="performance-and-scale"></a>A teljesítmény és a méret

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 43 | [SQL-adatbázis Advisor](sql-database-advisor.md) | Az Azure SQL-adatbázis tanácsadó a meglévő is gyorsításához aktuális lekérdezés SQL-adatbázisok javaslatokat tesz. |
| 44 | [SQL-adatbázis Advisor az Azure portál használatával](sql-database-advisor-portal.md) | Használhatja az Azure SQL-adatbázis Advisor az Azure-portálon tekintheti át és javaslatok a meglévő is gyorsításához aktuális lekérdezés SQL-adatbázisok végrehajtása. |
| 45 | [Azure SQL-adatbázis javasolt – áttekintés](sql-database-benchmark-overview.md) | Ez a témakör használni a Azure SQL-adatbázis teljesítményének Azure SQL-adatbázis javasolt. |
| 46 | [Ha egy rugalmas adatbázis készlet használni?](sql-database-elastic-pool-guidance.md) | Egy rugalmas adatbázis készlet rendelkezésre álló erőforrásokat rugalmas adatbázisok csoportja által megosztott gyűjteménye. A dokumentum tartalmaz útmutatást segítséget mérje fel, hogy egy rugalmas adatbázis készlet változatával adatbázisok csoportja megfelelőségét. |
| 47 | [Rugalmas Adatbáziseszközök – első lépések](sql-database-elastic-scale-get-started.md) | Egyszerű magyarázat rugalmas adatbázis eszközök funkció Azure SQL-adatbázis, például egyszerű példa alkalmazás futtatásához. |
| 48 | [SQL-adatbázis – gyakori kérdések](sql-database-faq.md) | Válaszok a gyakori kérdések ügyfelekre, és kérdezze felhő adatbázisok és Azure SQL-adatbázissal, a Microsoft relációs adatbázis-kezelő rendszer (RDBMS) és adatbázis szolgáltatásként a felhőben. |
| 49 | [Első lépések a memóriában (előzetes verzió) SQL-adatbázisban](sql-database-in-memory.md) | SQL a memóriában technológiák nagyban tranzakció alapú teljesítményének és analytics-munkaterhelésekből javítása. Megtudhatja, hogy miként e technológiák előnyeit. |
| 50 | [SQL-adatbázisban az alkalmazás teljesítményének javítása használata a memóriában OLTP (előzetes verzió)](sql-database-in-memory-oltp-migration.md) | Fogadja el a memóriában OLTP egy meglévő SQL-adatbázis tranzakció alapú teljesítmény javítása érdekében. |
| 51 | [Monitor a memóriában OLTP tárhely](sql-database-in-memory-oltp-monitoring.md) | Becslés és monitor XTP a memóriában tárterületet használ, kapacitás; 41823 kapacitás hiba megoldása |
| 52 | [A lekérdezés tároló Azure SQL-adatbázisban működő](sql-database-operate-query-store.md) | Megtudhatja, hogy miként működnek a lekérdezésben tároló Azure SQL-adatbázisban |
| 53 | [SQL-adatbázis teljesítmény betekintést](sql-database-performance.md) | Az SQL Azure-adatbázis nyújt segítséget azokat a területeket, amellyel aktuális lekérdezési teljesítmény teljesítmény eszközöket. |
| 54 | [Azure SQL-adatbázishoz, és a teljesítmény egyetlen adatbázisok](sql-database-performance-guidance.md) | Ez a cikk segíthetnek annak eldöntésében, hogy melyik szolgáltatási réteg választhatja ki az alkalmazás. Azt is módjai az alkalmazás hozhatja ki a legtöbbet a Azure SQL-adatbázis finomhangolása javasolja. |
| 55 | [Az SQL Azure adatbázis lekérdezési teljesítmény betekintést](sql-database-query-performance.md) | A legtöbb Processzor használata más lekérdezések lekérdezés teljesítményét figyelve azonosítja Azure SQL-adatbázis. |
| 56 | [Szolgáltatás réteg és a teljesítmény szintjének módosítása (árak réteg) SQL-adatbázishoz](sql-database-scale-up.md) | Módosítsa a szolgáltatási réteg, és Azure SQL-adatbázis teljesítményszint szemlélteti, hogyan lehet az SQL-adatbázis méretezni, felfelé vagy lefelé. A árak réteg Azure SQL-adatbázis módosítása. |
| 57 | [Azure SQL-adatbázisban adatbázis teljesítmény figyelése](sql-database-single-database-monitor.md) | Tudjon meg többet az adatbázis Azure eszközök és dinamikus kezelése nézetek figyelemmel kísérésére a beállításokat. |
| 58 | [SQL-adatbázis teljesítmény eszközbeállítási tippek](sql-database-troubleshoot-performance.md) | Tippek a teljesítmény elérése érdekében keresztül értékelése és javítása az Azure SQL-adatbázis beállítása. |
| 59 | [Kötegelés használata SQL-adatbázis alkalmazás teljesítmény javítása érdekében](sql-database-use-batching-to-improve-performance.md) | A témakör igazolja, hogy eljárásnak adatbázis-műveletek nagyban imroves sebességének és méretezhetőség: az SQL Azure-adatbázis alkalmazás. Habár e eljárásnak technikák bármely SQL Server-adatbázis működik, a cikk elsősorban a Azure. |



## <a name="tools-for-scaling-out"></a>Ellenőrző eszközök el méretezés

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 60 | [Multitenant szoftver-alkalmazások és Azure SQL-adatbázis mintázatok tervezése](sql-database-design-patterns-multi-tenancy-saas-applications.md) | Ez a cikk azt ismerteti, hogy a követelmények és az általános adatok architektúra mintázatok multitenant adatbázis felhőalapú környezetben futtatott alkalmazások figyelembe kell vennie és ezek a minták társított különböző kompromisszumok. Azt is elmagyarázza, hogy Azure SQL-adatbázis rugalmas eszközök és rugalmas készletek ezeknek a követelményeknek, nincs – nem biztonságos módon a cím. |
| 61 | [Méretezési meglévő adatbázis áttelepítése](sql-database-elastic-convert-to-use-elastic-tools.md) | Rugalmas Adatbáziseszközök: hozzon létre egy shard térkép manager használandó sharded adatbázisok konvertálása |
| 62 | [Építőelem méretezhető felhő adatbázisok](sql-database-elastic-database-client-library.md) | Méretezhető a .NET alkalmazások rugalmas adatbázis-ügyfél tárral összeállítása |
| 63 | [Teljesítmény számláló shard térkép Manager](sql-database-elastic-database-perf-counters.md) | ShardMapManager osztály és az adatok függő útválasztási teljesítmény számláló |
| 64 | [Egy rugalmas Adatbáziseszközök használatával shard hozzáadása](sql-database-elastic-scale-add-a-shard.md) | Hogyan lehet új shards hozzáadása egy shard API-skála rugalmas hoz segítségével beállítása. |
| 65 | [Felosztása és egyesítése szolgáltatás telepítése](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Felosztása és egyesítése a rugalmas Adatbáziseszközök |
| 66 | [Függő útválasztási adatok](sql-database-elastic-scale-data-dependent-routing.md) | Hogyan .NET-alkalmazásokban használandó ShardMapManager osztály adatok függő továbbításában szerepet Azure SQL-adatbázis rugalmas adatbázisok szolgáltatása |
| 67 | [Rugalmas Adatbáziseszközök – gyakori kérdések](sql-database-elastic-scale-faq.md) | Gyakori kérdések az SQL Azure-adatbázis rugalmas skála. |
| 68 | [Rugalmas adatbázis eszközök szószedet](sql-database-elastic-scale-glossary.md) | Rugalmas Adatbáziseszközök használt kifejezések magyarázata |
| 69 | [Méretezés kifelé az Azure SQL-adatbázis](sql-database-elastic-scale-introduction.md) | A Service (szoftver) fejlesztők szoftver egyszerűen készíthet rugalmas, méretezhető adatbázisok a felhőben, ezek az eszközök használatával |
| a 70 | [Az adatbázis rugalmas ügyfél tára eléréséhez használt hitelesítő adatok](sql-database-elastic-scale-manage-credentials.md) | Hogyan kell beállítani a megfelelő szintű hitelesítő adatait, rendszergazdát, hogy csak olvasható, az rugalmas adatbázis-alkalmazások |
| 71 | [Több elem shard lekérdezése](sql-database-elastic-scale-multishard-querying.md) | Lekérdezések futtatása a rugalmas adatbázis ügyfél-tár használata shards keresztül. |
| 72 | [Adatok áthelyezése méretezett kivételének felhő adatbázisok között](sql-database-elastic-scale-overview-split-and-merge.md) | Megtudhatja, hogyan kezelheti shards, és helyezze át az adatokat rugalmas adatbázis API-k használata önálló szolgáltatott szolgáltatáson keresztül. |
| 73 | [A térkép shard manager adatbázis méretezése](sql-database-elastic-scale-shard-map-management.md) | A ShardMapManager, rugalmas adatbázis ügyfél-tár használata |
| 74 | [Felosztása és egyesítése biztonsági beállításai](sql-database-elastic-scale-split-merge-security-configuration.md) | Állítsa be x409 titkosítás tanúsítványai |
| 75 | [A legújabb rugalmas adatbázis ügyfél könyvtár használata-alkalmazás frissítése](sql-database-elastic-scale-upgrade-client-library.md) | Frissítse az alkalmazások és -tárak Nuget használatával |
| 76 | [Rugalmas adatbázis entitás keretrendszer ügyfél tárban](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Adatbázisok coding rugalmas adatbázis ügyfél és a szervezet keretrendszer használata |
| 77 | [Dapper rugalmas adatbázis ügyfél-tár használata](sql-database-elastic-scale-working-with-dapper.md) | Rugalmas adatbázis ügyfél-tár használata Dapper. |
| 78 | [Több elem bérlői alkalmazások rugalmas Adatbáziseszközök és sor szintű biztonság](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Megtudhatja, hogy miként rugalmas Adatbáziseszközök sor szintű biztonsági együtt használja az alkalmazások és a nagyon méretezhető adatok réteg épülnek Azure SQL-adatbázissal, amely támogatja a több elem bérlői shards. |



## <a name="elastic-jobs"></a>Rugalmas feladatok

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 79 | [Létrehozhatja és kezelheti a méretezett ki Azure SQL-adatbázisait rugalmas feladatok (előzetes verzió)](sql-database-elastic-jobs-create-and-manage.md) | Haladjon végig és létrehozásának és kezelésének egy rugalmas adatbázis feladatot. |
| 80 | [Rugalmas adatbázis feladatok használatába](sql-database-elastic-jobs-getting-started.md) | Rugalmas adatbázis feladatok használata |
| 81 | [Méretezett kivételének felhő adatbázisok kezelése](sql-database-elastic-jobs-overview.md) | Azt mutatja be, a rugalmas adatbázis feladat szolgáltatás |
| 82 | [Létrehozása és kezelése egy SQL-adatbázis rugalmas adatbázis feladatok (előzetes verzió) PowerShell használatával](sql-database-elastic-jobs-powershell.md) | A PowerShell használatával készletek Azure SQL-adatbázis kezelése |
| 83 | [Telepítése rugalmas adatbázis feladatok – áttekintés](sql-database-elastic-jobs-service-installation.md) | Haladjon végig a rugalmas feladat szolgáltatás telepítése és. |
| 84 | [Rugalmas adatbázis-feladatok összetevők eltávolítása](sql-database-elastic-jobs-uninstall.md) | Az adatbázis rugalmas feladatok eszköz eltávolítása |



## <a name="elastic-pools"></a>Rugalmas készletek

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 85 | [Mi az az Azure rugalmas készlet?](sql-database-elastic-pool.md) | Több száz vagy a készlet használatával adatbázisokat ezer kezelése. Egy sor olyan teljesítményegységek egy árát terjeszthetők pedig fölé. Helyezze át az adatbázisok vagy kicsinyítés a program. |
| 86 | [Egy rugalmas adatbázis-készlet létrehozása a és C#](sql-database-elastic-pool-create-csharp.md) | C# adatbázis fejlesztési technikákat segítségével méretezhető rugalmas adatbázis-készlet létrehozása Azure SQL-adatbázisban, így azok megoszthatók erőforrások sok adatbázis keresztül. |
| 87 | [A PowerShell rugalmas adatbázis-készlet létrehozása](sql-database-elastic-pool-create-powershell.md) | Megtudhatja, hogy miként PowerShell-lel való skála kivételének Azure SQL-adatbázis erőforrások: hozzon létre több adatbázis kezelése méretezhető rugalmas adatbázis erőforráskészlethez tartozik. |
| 88 | [Az adatbázisok egy rugalmas adatbázis készlet alkalmas azonosító PowerShell-parancsprogramot](sql-database-elastic-pool-database-assessment-powershell.md) | Egy rugalmas adatbázis készlet rendelkezésre álló erőforrásokat rugalmas adatbázisok csoportja által megosztott gyűjteménye. A dokumentum egy Powershell-parancsprogramot mérje fel, hogy egy rugalmas adatbázis készlet változatával adatbázisok csoportja megfelelőségét súgója tartalmaz. |
| 89 | [Figyelése és a C# egy rugalmas adatbázis készlet kezelése](sql-database-elastic-pool-manage-csharp.md) | Az SQL Azure-adatbázis rugalmas adatbázis készlet kezelése a C# adatbázis fejlesztési technikákat használatával. |
| 90 | [Figyelése és kezelése egy rugalmas adatbázis készlet az Azure portálján](sql-database-elastic-pool-manage-portal.md) | Ismerje meg az Azure-portálra, és az SQL-adatbázis beépített üzletiintelligencia használata kezelheti, figyelésére és a jobb-adatbázis teljesítményének optimalizálása, és kezelheti a költség méretezhető rugalmas adatbázis erőforráskészlethez tartozik. |
| 91 | [Figyelése és kezelése a PowerShell-rugalmas adatbázis készlet](sql-database-elastic-pool-manage-powershell.md) | Megtudhatja, hogy miként egy rugalmas adatbázis készlet kezelése a PowerShell használatával. |
| 92 | [Figyelheti és kezelheti a Transact-SQL-rugalmas adatbázis készlet](sql-database-elastic-pool-manage-tsql.md) | Az SQL-T használja-Azure SQL-adatbázis létrehozása az rugalmas készletben. Vagy helyezze át a datbase készletek és kijelentkezés az SQL-T használja. |
| 93 | [Rugalmas adatbázis készlet számlázás és árinformációkat](sql-database-elastic-pool-price.md) | Rugalmas adatbázis készletek-specifikus árak adatait. |



## <a name="elastic-query"></a>Rugalmas lekérdezés

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 94 | [Jelentés keresztül méretezett kivételének felhő adatbázisok (előzetes verzió)](sql-database-elastic-query-getting-started.md) | az adatbázis-lekérdezések adatbázis tartományok használatáról |
| 95 | [Első lépések az idegen adatbázis-lekérdezések (függőleges szétválasztás) (előzetes verzió)](sql-database-elastic-query-getting-started-vertical.md) | függőlegesen particionált adatbázisok rugalmas adatbázis-lekérdezés használata |
| 96 | [Jelentéskészítés keresztül méretezett kivételének felhő adatbázisok (előzetes verzió)](sql-database-elastic-query-horizontal-partitioning.md) | vízszintes partíciót fölé rugalmas lekérdezések beállítása |
| 97 | [Azure SQL-adatbázis rugalmas adatbázis a lekérdezések áttekintése (előzetes verzió)](sql-database-elastic-query-overview.md) | A lekérdezés rugalmas szolgáltatás áttekintése |
| 98 | [Lekérdezése a felhőben adatbázisok különböző sémával (előzetes verzió)](sql-database-elastic-query-vertical-partitioning.md) | függőleges partíciót felett több adatbázis-lekérdezések beállítása |



## <a name="elastic-transaction"></a>Rugalmas tranzakció

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 99 | [Az elosztott tranzakciók felhő adatbázisok között](sql-database-elastic-transactions-overview.md) | Rugalmas adatbázis-tranzakciók, az Azure SQL-adatbázis áttekintése |



## <a name="backup-and-recovery"></a>Biztonsági mentése és helyreállítása

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 100 | [SQL-adatbázis biztonsági mentése](sql-database-automated-backups.md) | Tudjon meg többet beépített SQL-adatbázissal, amely lehetővé teszi a visszaállítás adatbázis biztonsági mentése ismét Azure SQL-adatbázis előző pontjához idő, vagy adatbázis másolása földrajzi régióban (legfeljebb 35 nap) új adatbázis. |
| 101 | [Az SQL Azure-adatbázis üzemkészségének áttekintése](sql-database-business-continuity.md) | Megtudhatja, hogyan támogatja az Azure SQL-adatbázis a felhő üzemkészségének és az adatbázis-helyreállítás és segíti futó felhőalapú kulcsfontosságú alkalmazásokat. |
| 102 | [Hogyan lehet egyetlen visszaállítani egy SQL Azure-adatbázis biztonsági mentése](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Megtudhatja, hogy miként Azure SQL-adatbázis biztonsági mentése egyetlen visszaállítani. |
| 103 | [Az aktív Geo replikációs használata SQL-adatbázis felhő vészhelyreállítás-alkalmazások tervezése](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Megtudhatja, hogy miként tervezése a felhőben katasztrófa helyreállítási megoldások üzleti folytonosságot tervezési geo replikációs alkalmazás adatok biztonsági mentése Azure SQL-adatbázis használata. |
| 104 | [Az Azure SQL-adatbázis vagy feladatátvevő visszaállítása a másodlagos](sql-database-disaster-recovery.md) | Megtudhatja, hogy miként adatbázis helyreállítása regionális adatközponthoz üzemszünetek vagy sikertelen az Azure SQL adatbázis aktív Geo replikációs és Geo-helyreállítási lehetőségeket. |
| 105 | [Katasztrófa helyreállítási részletező végrehajtása](sql-database-disaster-recovery-drills.md) | További útmutatást és a gyakorlati tanácsok a Azure SQL-adatbázissal, amely segítséget nyújt a katasztrófa helyreállítási gyakorlatokat végrehajtásához megőrzése a misszió kritikus hibák és kimaradások rugalmassá üzleti alkalmazások. |
| 106 | [SQL-adatbázis rugalmas készlet használó alkalmazásai katasztrófa helyreállítási stratégiák](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Megtudhatja, hogy miként vészhelyreállítás a felhőben megoldás tervezése a megfelelő feladatátvevő minta kiválasztásával. |
| 107 | [A RecoveryManager osztály használatával shard térkép kapcsolatos problémák megoldása](sql-database-elastic-database-recovery-manager.md) | Használja a RecoveryManager osztály shard térképekkel problémáinak megoldása |
| 108 | [Egy Azure SQL-adatbázis létrehozása BACPAC-fájl importálása](sql-database-import.md) | Azure SQL-adatbázis létrehozása meglévő BACPAC fájl importálásával. |
| 109 | [Azure SQL-adatbázis visszaállítása az Azure-portálra az időt egy előző pont](sql-database-point-in-time-restore-portal.md) | Visszaállíthatja az Azure SQL-adatbázis egy előző pont időben. |
| 110 | [Azure SQL-adatbázis visszaállítása az időt PowerShell egy előző pont](sql-database-point-in-time-restore-powershell.md) | Azure SQL-adatbázis visszaállítása az előző pontjához időben |
| 112 | [Az Azure SQL-adatbázisokkal automatikus adatbázis biztonsági mentése helyreállítása](sql-database-recovery-using-backups.md) | Tudjon meg többet a pont és az idő visszaállítása, amely lehetővé teszi, hogy visszaállíthatja az Azure SQL-adatbázis egy előző pont időben (legfeljebb 35 nap). |
| 113 | [Az Azure-portálon törölt Azure SQL-adatbázis visszaállítása](sql-database-restore-deleted-database-portal.md) | Vissza a törölt Azure SQL-adatbázis (Azure Portal). |
| 114 | [A törölt Azure SQL-adatbázis visszaállítása a PowerShell használatával](sql-database-restore-deleted-database-powershell.md) | Vissza a törölt Azure SQL-adatbázis (PowerShell). |
| 115 | [Adatbázis visszaállítása az előző pontjához időben, törölt adatbázis visszaállítása vagy egy adatok központ üzemszünetek helyreállítása](sql-database-troubleshoot-backup-and-restore.md) | Megtudhatja, hogy miként felhő adatbázis helyreállítása hibák és a biztonsági mentés és a kópiák használata SQL Azure-adatbázis kimaradások. |



## <a name="migrate"></a>Áttelepítése

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 116 | [SQL Server-adatbázis exportálása SqlPackage BACPAC fájlt](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, exportálása az adatbázist, majd az Exportálás BACPAC fájl sqlpackage |
| 117 | [SQL Server-adatbázis exportálása használata SQL Server Management Studio BACPAC-fájlba](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, exportálása adatbázist, majd az Exportálás BACPAC fájlt, az adatok réteg alkalmazás exportálás varázsló |
| 118 | [SQL-adatbázishoz importálása SqlPackage BACPAC fájlt](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, adatbázis importálása, sqlpackage BACPAC fájl importálása |
| 119 | [BACPAC importálása SSMS használata SQL-adatbázishoz](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL-adatbázis adatbázis terjesztése, az adatbázis áttelepítése, adatbázis importálása exportálás adatbázis áttelepítése varázsló |
| 120-ra | [SQL Server-adatbázis áttelepítése az SQL-adatbázis terjesztése adatbázist, hogy a Microsoft Azure-adatbázis varázsló használatával](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése a Microsoft Azure-adatbázis varázsló |
| 121 | [SQL Server-adatbázis áttelepítése az Azure SQL-adatbázis tranzakció alapú replikációs használatával](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, az adatbázis, importálása tranzakció alapú replikációs |
| 122 | [SQL-adatbázis kompatibilitási SqlPackage.exe használatával meghatározása](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, az SQL-adatbázishoz való kompatibilitás érdekében SqlPackage |
| 123 | [SQL Server Management Studio segítségével határozza meg az SQL-adatbázis kompatibilitási Azure SQL-adatbázishoz való áttelepítés előtt](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, az SQL-adatbázishoz való kompatibilitás érdekében exportálási réteg alkalmazás varázsló |
| 124 | [Az SQL Azure áttelepítési varázsló segítségével javítása az SQL Server adatbázis kapcsolatos kompatibilitási problémák Azure SQL-adatbázishoz való áttelepítés előtt](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, a kompatibilitás érdekében az SQL Azure-áttelepítés varázsló |
| 125 | [Az SQL Server-adatbázis áttelepítése az Azure SQL-adatbázis használata SQL Server Data Tools for Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, kompatibilis, az SQL Azure-áttelepítés varázsló, SSDT |
| 126 | [Az SQL Server adatbázis kompatibilitási problémák megoldása az SQL Server Management Studio használata SQL-adatbázishoz való áttelepítés előtt](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL-adatbázissal, az adatbázis áttelepítése, a kompatibilitás érdekében az SQL Azure-áttelepítés varázsló |
| 127 | [Az Azure SQL-adatbázis BACPAC fájlba archiválhatja a PowerShell használatával](sql-database-export-powershell.md) | Az Azure SQL-adatbázis BACPAC fájlba archiválhatja a PowerShell használatával |
| 128 | [Egy Azure SQL-adatbázis létrehozása a PowerShell használatával BACPAC-fájl importálása](sql-database-import-powershell.md) | Egy Azure SQL-adatbázis létrehozása a PowerShell használatával BACPAC-fájl importálása |
| 129 | [Adatok betöltése Azure SQL-adatraktár (egyszerű, strukturálatlan fájl) CSV](sql-database-load-from-csv-with-bcp.md) | Adatok importálása az Azure SQL-adatbázis használja egy kis adatok mérete bcp. |
| 130 | [Kiszolgálók között, előfizetés között, és Azure-és kijelentkezés adatbázisok áthelyezése](sql-database-troubleshoot-moving-data.md) | A rövid lépéseket követve másolása, áthelyezése és adatok és az adatbázisokkal az Azure SQL-adatbázis áttelepítése. |



## <a name="move-data-in-and-out"></a>Adatok áthelyezése és kicsinyítés

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 131 | [Az SQL Server-adatbázis áttelepítési a felhőben SQL-adatbázishoz](sql-database-cloud-migrate.md) | Megtudhatja, hogyan kell tudni a helyszíni SQL Server adatbázis áttelepítése az Azure SQL-adatbázis a felhőben. Kompatibilitás az adatbázis áttelepítése előtt ellenőrizze a adatbázis áttelepítési eszközök segítségével. |
| 132 | [Másolja az Azure SQL-adatbázis](sql-database-copy.md) | Azure SQL-adatbázishoz másolatának létrehozása |
| 133 | [Az Azure-portálon BACPAC fájlba Azure SQL-adatbázisból archiválása](sql-database-export.md) | Az Azure-portálon BACPAC fájlba Azure SQL-adatbázisból archiválása |



## <a name="security"></a>Biztonsági

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 134 | [Csatlakozás SQL-adatbázis vagy adatraktár SQL Azure Active Directory-hitelesítés használatával](sql-database-aad-authentication.md) | Megtudhatja, hogy miként SQL-adatbázis csatlakoztatása az Azure Active Directory-hitelesítés használatával. |
| 135 | [Mindig titkosított: SQL-adatbázisban kényes adatok védelme és tárolni a titkosítási kulcs a Windows-tárolóban található](sql-database-always-encrypted.md) | Az SQL-adatbázis kényes adatok védelme biztonsági perc alatt. |
| 136 | [Mindig titkosított: SQL-adatbázisban kényes adatok védelme és tárolni a titkosítási kulcs Azure kulcs tárolóból elemre](sql-database-always-encrypted-azure-key-vault.md) | Az SQL-adatbázis kényes adatok védelme biztonsági perc alatt. |
| 137 | [SQL-adatbázis - támogatja a régebbi ügyfelek és végponton módosítja a naplózás](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Tudjon meg többet SQL-adatbázis régebbi ügyfelek számára nyújtott támogatás és IP-végpontot módosításokat a naplózás. |
| 138 | [Azure SQL-adatbázis-kiszolgálói szintű tűzfalszabályokat állítsa be a PowerShell használatával](sql-database-configure-firewall-settings-powershell.md) | További információ a tűzfalat, amely az access Azure SQL-adatbázisok IP-címek konfigurálása. |
| 139 | [A REST API Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályok konfigurálása](sql-database-configure-firewall-settings-rest.md) | További információ a tűzfalat, amely az access Azure SQL-adatbázisok IP-címek konfigurálása. |
| 140 | [Azure SQL-adatbázis kiszolgáló és adatbázis-szintjének tűzfalszabályokat az SQL-T használ konfigurálása](sql-database-configure-firewall-settings-tsql.md) | További információ a tűzfalat, amely az access Azure SQL-adatbázisok IP-címek konfigurálása. |
| 141 | [Első lépések az SQL adatbázis dinamikus adatok maszkolás (Azure Portal)](sql-database-dynamic-data-masking-get-started.md) | Ismerkedés az SQL adatbázis dinamikus adatok maszkolás az Azure-portálon |
| 142 | [Első lépések az SQL adatbázis dinamikus adatok maszkolás (Azure klasszikus Portal)](sql-database-dynamic-data-masking-get-started-portal.md) | Ismerkedés az SQL adatbázis dinamikus adatok maszkolás az Azure klasszikus portálon |
| 143 | [Azure SQL-adatbázis tűzfalszabályokat konfigurálni \- – áttekintés](sql-database-firewall-configure.md) | Megtudhatja, hogy miként állítható be egy SQL-adatbázis tűzfal az való hozzáférés kezelése a kiszolgálón és adatbázis-szintjének tűzfalszabályokat. |
| 144 | [SQL-adatbázis oktatóprogram: SQL-adatbázis eléréséhez, és egy adatbázis kezelése a felhasználói fiókok létrehozása](sql-database-get-started-security.md) | Megtudhatja, hogy miként hozhat létre felhasználói fiókot, elérésére és kezelése egy adatbázisban. |
| 145 | [SQL-adatbázis hitelesítés és engedélyezési: hozzáférés engedélyezése](sql-database-manage-logins.md) | További tudnivalók: az SQL-adatbázis biztonsági kezelését, kifejezetten hogyan kell kezelni adatbázis az access és a bejelentkezési biztonsági kiszolgálói szintű fő fiókon keresztül. |
| 146 | [Azure SQL-adatbázis biztonsági irányelvei és korlátai](sql-database-security-guidelines.md) | Tudjon meg többet a Microsoft Azure SQL-adatbázis irányelvei és biztonsági kapcsolatos korlátai. |
| 147 | [SQL-adatbázis veszélyt észlelési – első lépések](sql-database-threat-detection-get-started.md) | Ismerkedés az SQL-adatbázis veszélyt észlelése az Azure-portálon |
| 148 | [Hogyan végezhetők el a leggyakoribb felügyeleti feladatokat, például az Azure SQL-adatbázis rendszergazdai jelszó alaphelyzetbe állítása](sql-database-troubleshoot-permissions.md) | Megtudhatja, hogy miként elvégezhető leggyakoribb felügyeleti SQL-adatbázisban. Ha például rendszergazdai jelszó alaphelyzetbe állítása, megadásának és eltávolítás access. |



## <a name="manage-your-database"></a>Az adatbázis kezelése

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 149 | [Első lépések az SQL-adatbázis naplózás](sql-database-auditing-get-started.md) | Első lépések az SQL-adatbázis naplózás |
| 150 | [Egy az Azure-portálon Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabály beállítása](sql-database-configure-firewall-settings.md) | További információ a tűzfalat, melyhez hozzáféréssel Azure SQL server IP-címek konfigurálása. |
| 151 | [Másolja az Azure SQL-adatbázissal az Azure portál használatával](sql-database-copy-portal.md) | Azure SQL-adatbázishoz másolatának létrehozása |
| 152 | [Azure SQL-adatbázisait Azure automatizálási kezelése](sql-database-manage-automation.md) | További tudnivalók: hogyan az Azure automatizálási szolgáltatásai használhatók a méretezés Azure SQL-adatbázisok kezelése. |
| 153 | [Áttekintés: eszközeit SQL-adatbázis](sql-database-manage-overview.md) | Miben más az eszközök és Azure SQL-adatbázis kezelésére szolgáló beállítások |
| 154 | [Figyelés Azure SQL-adatbázis kezelése dinamikus nézetek használata](sql-database-monitoring-with-dmvs.md) | Megtudhatja, hogy miként észleli és közös teljesítménybeli problémáinak diagnosztizálása figyelése a Microsoft Azure SQL-adatbázis kezelése dinamikus nézetek használatával. |
| 155 | [Az SQL-adatbázis biztonságossá tétele](sql-database-security.md) | Azure SQL-adatbázis és az SQL Server biztonságáról szól, beleértve a felhőben közötti különbségeket és az SQL Server helyszínen amikor hitelesítés, engedélyezési, kapcsolat biztonsági, titkosítást és megfelelőség. |



## <a name="extended-events"></a>Bővített események

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 156 | [Fájl cél eseménykód SQL-adatbázisban bővített eseményekre vonatkozóan:](sql-database-xevent-code-event-file.md) | Mintaként szolgáló kétfázisú kódot, amely bemutatja a nézeteihez célhelyet Azure SQL-adatbázis a bővített esemény PowerShell és a Transact-SQL nyelvben biztosít. Azure tároló ebben az esetben szükséges része. |
| 157 | [Csengetés, bővített események SQL-adatbázisban pufferelési cél kódját](sql-database-xevent-code-ring-buffer.md) | A gyűrű pufferelési céljának Azure SQL-adatbázisban használatával gyors és egyszerű végzett Transact-SQL nyelvben kód minta biztosít. |
| 158 | [Bővített események SQL-adatbázisban](sql-database-xevent-db-diff-from-svr.md) | Bővített események (XEvents) Azure SQL-adatbázisban, és hogyan esemény munkamenetek némileg eltérő Microsoft SQL Server-alapú esemény munkamenetek ismerteti. |



## <a name="geo-replication"></a>GEO replikációs

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 159 | [Azure SQL-adatbázis tervezett vagy nem tervezett feladatátvevő kezdeményezése az Azure-portálra](sql-database-geo-replication-failover-portal.md) | Tervezett vagy nem tervezett feladatátvevő kezdeményezése az Azure portálon Azure SQL-adatbázis |
| 160 | [Tervezett vagy nem tervezett feladatátvevő kezdeményezzen PowerShell Azure SQL-adatbázis](sql-database-geo-replication-failover-powershell.md) | Tervezett vagy nem tervezett feladatátvevő kezdeményezzen Azure SQL-adatbázis PowerShell használatával |
| 161 | [Tervezett vagy nem tervezett feladatátvevő kezdeményezzen a Transact-SQL Azure SQL-adatbázis](sql-database-geo-replication-failover-transact-sql.md) | Tervezett vagy nem tervezett feladatátvevő kezdeményezzen Azure SQL-adatbázis használata a Transact-SQL nyelvben |
| 162 | [Áttekintés: Az SQL adatbázis aktív Geo replikációs](sql-database-geo-replication-overview.md) | Aktív Geo replikációs lehetővé teszi, hogy az adatbázis bármelyik az Azure adatközpontokkal 4 kópiák beállítása. |
| 163 | [Azure SQL-adatbázissal az Azure portálján Geo replikációs beállítása](sql-database-geo-replication-portal.md) | Az Azure portálon Azure SQL-adatbázis Geo-replikáció konfigurálása |
| 164 | [A PowerShell Azure SQL-adatbázis Geo-replikáció konfigurálása](sql-database-geo-replication-powershell.md) | Állítsa be az aktív Geo replikációs Azure SQL-adatbázis PowerShell használatával |
| 165 | [Hogyan kell kezelni az Azure SQL-adatbázis biztonsági vészhelyreállítás után](sql-database-geo-replication-security-config.md) | Ebből a témakörből megtudhatja, biztonsági megfontolások biztonságkezelési egy adatbázis visszaállítása vagy feladatátvevő után. |
| 166 | [A replikáció Geo beállítása a Transact-SQL Azure SQL-adatbázis](sql-database-geo-replication-transact-sql.md) | Azure SQL-adatbázis használata a Transact-SQL nyelvben Geo-replikáció konfigurálása |
| 167 | [GEO-visszaállítása biztonsági másolatból geo felesleges az Azure-portálon az Azure SQL-adatbázis](sql-database-geo-restore-portal.md) | Visszaállítás GEO az Azure SQL-adatbázis biztonsági másolatból geo felesleges (Azure Portal). |
| 168 | [Azure SQL-adatbázis visszaállítása biztonsági geo felesleges másolatból PowerShell használatával](sql-database-geo-restore-powershell.md) | Azure SQL-adatbázis visszaállítása be egy új webkiszolgálóra geo felesleges biztonsági másolatból |



## <a name="upgrade"></a>Frissítés

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 169 | [A lekérdezési teljesítmény növelése kompatibilitásra szint 130 Azure SQL-adatbázisban](sql-database-compatibility-level-query-performance-130.md) | A lépéseket, illetve annak megállapítása, hogy melyik kompatibilitási szintet a legjobb az adatbázis Azure SQL-adatbázis vagy a Microsoft SQL Server eszközöket |
| 170 | [Közbeni használata SQL adatbázis aktív Geo replikációs felhő alkalmazások kezelése](sql-database-manage-application-rolling-upgrade.md) | Megtudhatja, hogy miként online frissítése a felhőbeli alkalmazás támogatja az Azure SQL-adatbázis geo replikációs használatával. |
| 171 | [Az Azure portálon Azure SQL-adatbázis V12 frissítése](sql-database-upgrade-server-portal.md) | Megtudhatja, hogyan frissítése a Microsoft Azure SQL-adatbázis V12 például üzleti és a webes adatbázis frissítése, az adatbázis áttelepítése közvetlenül egy rugalmas adatbázis készlet az Azure portálon V11 kiszolgáló frissítéséről. |
| 172 | [Azure SQL-adatbázis V12 frissítés PowerShell használatával](sql-database-upgrade-server-powershell.md) | Megtudhatja, hogyan frissítése a Microsoft Azure SQL-adatbázis V12 például üzleti és a webes adatbázis frissítése, az adatbázis áttelepítése közvetlenül egy PowerShell használatával rugalmas adatbázis készlet V11 kiszolgáló frissítéséről. |
| 173 | [Megtervezése és a Felkészülés a SQL-adatbázis V12 frissítése](sql-database-v12-plan-prepare-upgrade.md) | Előkészületet és korlátozások verzióra V12 az Azure SQL-adatbázis frissítése részt ismerteti. |
| 174 | [SQL-adatbázis V12 újdonságai](sql-database-v12-whats-new.md) | Ismerteti, hogy miért előnyei a Azure SQL-adatbázis használják a felhőben business rendszerek-verzióra való frissítésről V12 most szerint. |
| 175 | [Webes és üzleti Edition sunset gyakori kérdések](sql-database-web-business-sunset-faq.md) | Megtudhatja, hogy az Azure SQL-webes és az üzleti adatbázisok visszavonásának, és ismereteket szerezhet a és az új szolgáltatás a meghatározási funkciók. |



## <a name="tools"></a>Eszközök

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 176 | [A PowerShell Azure SQL-adatbázis kezelése](sql-database-command-line-tools.md) | Azure SQL-adatbázis kezelése a PowerShell. |
| 177 | [SQL-adatbázis oktatóprogram: az Excel csatlakozni Azure SQL-adatbázishoz, és a jelentés létrehozása](sql-database-connect-excel.md) | Útmutató a Microsoft Excel csatlakozni a felhőben Azure SQL-adatbázishoz. Adatok importálása az Excel-jelentése és az adatok feltárása. |
| 178 | [SQL-adatbázis létrehozása, és hajtsa végre a PowerShell-parancsmagok közös adatbázis beállítási feladatok](sql-database-get-started-powershell.md) | Olvassa el a most PowerShell SQL-adatbázis létrehozása. Adatbázis-beállítási gyakori feladatok kezelhető PowerShell-parancsmagok között. |
| 179 | [Első lépések az SQL Azure-adatok szinkronizálása (előzetes verzió)](sql-database-get-started-sql-data-sync.md) | Ebben az oktatóanyagban segít, hogy az első lépések az SQL Azure adatszinkronizálás (előzetes verzió). |
| 180 | [SQL Server Management Studio segítségével Azure SQL-adatbázis kezelése](sql-database-manage-azure-ssms.md) | Megtudhatja, hogyan lehet az SQL Server Management Studio segítségével SQL-adatbázis-kiszolgáló és az adatbázisok kezelése. |
| 181 | [Az Azure portálon Azure SQL-adatbázisok kezelése](sql-database-manage-portal.md) | Megtudhatja, hogy miként az Azure-portálon használja a felhőben, az Azure-portálon relációs adatbázisok kezelését. |
| 182 | [Szolgáltatás réteg és a teljesítmény szintjének módosítása (árak réteg) PowerShell SQL-adatbázishoz](sql-database-scale-up-powershell.md) | Módosítsa a szolgáltatási réteg, és Azure SQL-adatbázis teljesítményszint bemutatja, hogyan méretezheti az SQL-adatbázis felfelé vagy lefelé a PowerShell. A PowerShell-Azure SQL-adatbázis árak réteg módosítása. |
| 183 | [SQL Server Management Studio használata Azure RemoteApp SQL-adatbázishoz való csatlakozáshoz](sql-database-ssms-remoteapp.md) | Ebből az oktatóanyagból megtudhatja, hogy miként használható az SQL Server Management Studio Azure RemoteApp a biztonság és a teljesítmény SQL-adatbázishoz való csatlakozáskor használata |



## <a name="reference"></a>Hivatkozás

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 184 | [A kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md) | Megjeleníti az egyes ügyfélprogramok segítségével kapcsolódhat Azure SQL-adatbázis vagy a Microsoft SQL Server-illesztőprogram legkisebb verziószáma. Hivatkozás a Közösség, hanem a Microsoft által kiadott-illesztőprogramok verzió adatait megadva. |
| 185 | [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md) | Ezen az oldalon ismerteti néhány gyakori erőforrás Azure SQL-adatbázis. |
| 186 | [Azure SQL-adatbázis-Transact-SQL különbségek](sql-database-transact-sql-information.md) | Kisebb, mint támogatott Azure SQL-adatbázisban Transact-SQL-kimutatásokban |



## <a name="miscellaneous"></a>Vegyes

| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 187 | [Másolja az Azure SQL-adatbázis PowerShell használatával](sql-database-copy-powershell.md) | Készítsen másolatot a PowerShell használatá Azure SQL-adatbázishoz |
| 188 | [Másolja az használata a Transact-SQL Azure SQL-adatbázishoz](sql-database-copy-transact-sql.md) | Másolat egy használata a Transact-SQL Azure SQL-adatbázishoz |
| 189 | [Azure SQL-adatbázis általános korlátai és útmutató](sql-database-general-limitations.md) | Ez a lap azt ismerteti, hogy bizonyos korlátozások, amelyek az általános Azure SQL-adatbázissal, valamint az együttműködési és támogatási részeinek. |
| 190 | [SQL-adatbázis árak réteg javaslatok](sql-database-service-tier-advisor.md) | Árak módosításakor az Azure-portálon réteg javaslatok árak rétegek vannak, feltéve hogy ajánlott a réteg, amely a leginkább alkalmas rendszert futtató egy meglévő Azure SQL-adatbázis a terhelést. Árak rétegek SQL-adatbázis szolgáltatás réteg és a teljesítmény szintjét ismertetik. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Extrák

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Azure szolgáltatásokból kapcsolódó cikkek

- [Készítsen biztonsági másolatot az Azure alkalmazás Services alkalmazás Azure-ban](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Dokumentáció eszközök

- [Azure SQL-adatbázis bejelentve a frissítések](http://azure.microsoft.com/updates/?service=sql-database)

- [Keresés a dokumentáció diagramtípusokat Microsoft Azure-szűrők](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Tanulási javaslat ábrák

- [Tanulási javaslat grafikus az összes Microsoft Azure-szolgáltatások](http://azure.microsoft.com/documentation/learning-paths/)

- [Tanulási javaslat: További tudnivalók az Azure SQL-adatbázis](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Tanulási javaslat: SQL-adatbázis rugalmas skála](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


