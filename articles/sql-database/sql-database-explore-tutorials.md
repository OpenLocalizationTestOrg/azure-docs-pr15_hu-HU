<properties
   pageTitle="Ismerje meg az SQL Azure-adatbázis oktatóanyagok |} Microsoft Azure"
   description="SQL-adatbázis-szolgáltatások és funkciók ismertetése"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Ismerje meg az SQL Azure-adatbázis oktatóanyagok

Az alábbi hivatkozásokon felsorolt szolgáltatás területenként és egy egyszerű a lépések-lépés oktatóprogram mindegyik területén. Megoldás – munkafüzetszintű rövid indul el, amelyek bemutatják a felhasználás SQL-adatbázis valós esetek alapján ideális megoldás [Azure SQL adatbázis megoldás gyors indítása](sql-database-solution-quick-starts.md)című témakör tartalmaz.

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio segítségével

Az alábbi oktatóanyag megtanulhatja, felügyelete és Azure SQL-adatbázis lekérdezés SQL Server Management Studio használatáról.


> [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


| Oktatóprogram  | Leírás  |
|---|---|---|
| [Kiszolgálói szintű fő bejelentkezési használatával Azure SQL-adatbázis csatlakoztatása](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| Ebből az oktatóanyagból megismerheti, hogyan csatlakozhat az Azure SQL-adatbázisokkal kiszolgálói szintű fő bejelentkezési.|
| [Olyan felhasználóként Azure SQL-adatbázis csatlakoztatása](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | Ebből az oktatóanyagból megtanulhatja, hogyan egy adatbázis szintű felhasználói fiók használatával Azure SQL-adatbázishoz való csatlakozáshoz.|
||||

## <a name="elastic-pools"></a>Rugalmas készletek

Az alábbi oktatóanyagok megtanulhatja [rugalmas készletek](sql-database-elastic-pool.md) használata a teljesítmény mappákból körben eltérő és járhat használatát mintázatok létrehozott több adatbázisok kezelése.

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Egy rugalmas készlet létrehozása](sql-database-elastic-pool-create-portal.md) | Ebből az oktatóanyagból megismerheti, hogyan hozhat létre Azure SQL-adatbázisokhoz méretezhető erőforráskészlethez tartozik. |
| [Rugalmas adatbázissá figyelése](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | Ebből az oktatóanyagból megismerheti, hogyan Lync-esetleges problémákat tapasztal az egyes rugalmas adatbázisban. |
| [Értesítés készlet erőforrás hozzáadása](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | Ebből az oktatóanyagból megismerheti, hogyan szabályok hozzáadása információforrások, amelyek az e-mail küldése a személyek vagy URL-cím végpontokhoz riasztási karakterláncok, amikor az erőforrás találatok beállított kihasználtsági küszöbértéket. |
| [Adatbázis áthelyezése egy rugalmas készlet](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | Ebből az oktatóanyagból megismerheti, hogyan adatbázis áthelyezése egy rugalmas készletbe. |
| [Adatbázis kiveszi az rugalmas készlet](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | Ebből az oktatóanyagból megismerheti, hogyan adatbázis áthelyezése egy rugalmas készlet ki. |
| [Egy készlet teljesítmény beállításainak módosítása](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | Ebből az oktatóanyagból megismerheti, hogyan módosíthatja a teljesítmény és a tárhely korlátozások erőforráskészlethez tartozik. |
||||

## <a name="elastic-database-jobs"></a>Rugalmas adatbázis feladatok

Az alábbi oktatóanyagok megtanulhatja [Rugalmas adatbázis feladatok](sql-database-elastic-jobs-overview.md)használatáról.

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Rugalmas Adatbáziseszközök – első lépések](sql-database-elastic-scale-get-started.md) | Ebből az oktatóanyagból megismerheti, hogyan egy egyszerű sharded alkalmazással rugalmas Adatbáziseszközök lehetőségeit használni. |
| [Első lépések az Azure SQL-adatbázis rugalmas feladatok](sql-database-elastic-jobs-getting-started.md)  | Ebből az oktatóanyagból megismerheti, hogyan szeretné létrehozása és kezelése feladatok csoport kapcsolódó az adatbázisok kezelése.  | 
||||

## <a name="elastic-queries"></a>Rugalmas lekérdezések

Az alábbi oktatóanyagok megtanulhatja [rugalmas lekérdezések](sql-database-elastic-query-overview.md)futtatása. 

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Lekérdezi egy vízszintesen (sharded) adatbázisos keresztül)](sql-database-elastic-query-getting-started.md) | Ebből az oktatóanyagból megismerheti, hogyan készíthet jelentéseket a vízszintes (sharded) adatbázisos [Rugalmas lekérdezés](sql-database-elastic-query-overview.md) használata az összes adatbázisokból |
| [Lekérdezés különböző függőlegesen adatbázisos)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | Ebből az oktatóanyagból megismerheti, hogyan készíthet jelentéseket az összes adatbázisokból egy függőlegesen adatbázis [Rugalmas lekérdezés](sql-database-elastic-query-overview.md) használata |
| [Méretezési egy meglévő adatbázis áttelepítése](sql-database-elastic-convert-to-use-elastic-tools.md)| Ebből az oktatóanyagból megismerheti (shard) Azure SQL-adatbázis vízszintesen méretezni. |
||||

## <a name="performance-optimization"></a>Az optimalizálása

Az alábbi oktatóanyagok megtanulhatja az [egyetlen adatbázisok teljesítménye](sql-database-performance-guidance.md)optimalizálása. Több adatbázis optimalizálásáról [rugalmas készletek](#elastic-pools)talál.

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Az adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | Ebből az oktatóanyagból megismerheti, hogyan méretezheti, vagy le egy Azure SQL-adatbázis használ szolgáltatási rétegek teljesítményét méretezni. |
| [SQL adatbázis Advisor lekérdezési teljesítmény betekintést](sql-database-performance.md#performance-overview) | Ebből az oktatóanyagból megismerheti, hogyan megnyitása és használata SQL adatbázis Advisor lekérdezési teljesítmény betekintést.|
| [SQL-adatbázis Advisor teljesítmény javaslatok](sql-database-advisor.md#viewing-recommendations) | Ebből az oktatóanyagból megismerheti, hogyan megtekintése és SQL-adatbázis Advisor teljesítményét javaslatok alkalmazni. |
| [Tekintse át a lekérdezések használata más felső Processzor](sql-database-query-performance.md#review-top-cpu-consuming-queries)| Ebből az oktatóanyagból megismerheti, hogyan SQL adatbázis Advisor lekérdezési teljesítmény betekintést használni, tekintse át a lekérdezések használata más felső Processzor.|
| [Egyéni lekérdezési részleteinek megtekintése](sql-database-query-performance.md#viewing-individual-query-details)| Ebből az oktatóanyagból megismerheti, hogyan használata SQL adatbázis Advisor lekérdezési teljesítmény betekintést egyes lekérdezési teljesítmény részletes adatainak megjelenítéséhez.|
||||

## <a name="sql-database-migration-and-archive"></a>SQL-adatbázis áttelepítése és archív mappák 

Az alábbi oktatóanyagok megtanulhatja [egy meglévő adatbázist az SQL Server Azure SQL-adatbázis](sql-database-cloud-migrate.md)frissítéséről.

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Annak ellenőrzése, használja az SQL Server Data Tools for Visual Studio kapcsolatos kompatibilitási problémák](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | Ebből az oktatóanyagból megismerheti, hogyan SQL Server Data Tools for Visual Studio segítségével Azure SQL-adatbázis kompatibilitási határozza meg. |
| [Rögzítési használata SQL Server Data Tools for Visual Studio kapcsolatos kompatibilitási problémák](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | Ebből az oktatóanyagból megismerheti, hogyan Azure SQL-adatbázissal kapcsolatos kompatibilitási problémák megoldása az SQL Server Data Tools for Visual Studio segítségével. |
| [Határozza meg az SQL-adatbázis kompatibilitási SqlPackage.exe használatával](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | Ebből az oktatóanyagból megismerheti, hogyan segédprogrammal SQLPackage.exe parancssori Azure SQL-adatbázis kompatibilitási határozza meg.|
| [SQL-adatbázis kompatibilitási SSMS használatával meghatározása](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |Ebből az oktatóanyagból megismerheti, hogyan használata SQL Server Management Studio Azure SQL-adatbázis kompatibilitási határozza meg.|
| [SQL Server-adatbázis áttelepítése az SQL-adatbázis terjesztése adatbázist, hogy a Microsoft Azure-adatbázis varázsló használatával](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | Ebből az oktatóanyagból megtanulhatja, hogyan kompatibilis SQL Server-adatbázis áttelepítése az adatbázis terjesztése Microsoft Azure-adatbázis varázsló használata SQL Server Management Studio Azure SQL-adatbázis.
| [SQL Server-adatbázis exportálása SSMS BACPAC fájlt](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Ebből az oktatóanyagból megtanulhatja kompatibilis SQL Server-adatbázis exportálása adatok réteg alkalmazás exportálás varázsló segítségével az SQL Server Management Studio BACPAC fájlba.|
| [SQL Server-adatbázis exportálása SqlPackage BACPAC fájlt](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Ebből az oktatóanyagból megtanulhatja kompatibilis SQL Server-adatbázis exportálása a SQLPackage.exe parancssori segédprogrammal BACPAC fájlba.|
| [Azure SQL-adatbázisokkal SSMS BACPAC fájl importálása](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Ebből az oktatóanyagból megtanulhatja az adatbázis importálásának Azure SQL-adatbázis adatok réteg alkalmazás exportálás varázsló segítségével az SQL Server Management Studio BACPAC fájlból. |
| [Azure SQL-adatbázisokkal SqlPackage BACPAC fájl importálása](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | Ebből az oktatóanyagból megtanulhatja az adatbázis importálásának Azure SQL-adatbázis SQLPackage parancssori segédprogrammal BACPAC fájlból. |
| [BACPAC fájl importálása az Azure portálon Azure SQL-adatbázis](sql-database-import.md) | Ebből az oktatóanyagból megtanulhatja az adatbázis importálásának Azure SQL-adatbázissal, amely az Azure blob-az Azure-portálon található BACPAC fájlból.|
| [BACPAC fájl importálása az Azure SQL-adatbázis PowerShell használatával](sql-database-import-powershell.md) | Ebből az oktatóanyagból megtanulhatja az adatbázis importálásának Azure SQL-adatbázis PowerShell használatával BACPAC fájlból.|
| [Az Azure portálon Azure SQL-adatbázisból archiválása](sql-database-export.md#export-your-database) | Ebből az oktatóanyagból megismerheti, hogyan archiválását az Azure portálon BACPAC fájlba Azure SQL-adatbázisból. |
| [Egy PowerShell használatával Azure SQL-adatbázis archiválása](sql-database-export-powershell.md) | Ebből az oktatóanyagból megismerheti, hogyan archiválását BACPAC-fájlba történő PowerShell-Azure SQL-adatbázishoz. |
| [Másolja az Azure portálon Azure SQL-adatbázisból](sql-database-copy.md#copy-your-sql-database) | Ebből az oktatóanyagból megismerheti, hogyan másolása az Azure portálon Azure SQL-adatbázisból. |
| [Másolja az Azure SQL-adatbázis PowerShell használatával](sql-database-copy-powershell#copy-your-sql-database) | Ebből az oktatóanyagból megismerheti, hogyan másolni egy PowerShell használatával Azure SQL-adatbázishoz. |
| [Másolja az használata a Transact-SQL Azure SQL-adatbázishoz](sql-database-copy-transact-sql.md#copy-your-sql-database) | Ebből az oktatóanyagból megismerheti, hogyan másolni egy Azure SQL-adatbázisokkal Transact-SQL nyelvben. |
||||

##<a name="develop"></a>Kidolgozása

Az alábbi oktatóanyagok megtanulhatja [SQL-adatbázis fejlesztés](sql-database-develop-overview.md) és a [kapcsolat tárak](sql-database-libraries.md)használatával kapcsolatban.

| Oktatóprogram  | Leírás  |
|---|---|---|
| [SQL-adatbázis csatlakoztatása a .NET (C#) használatával](sql-database-develop-dotnet-simple.md) | Ebből az oktatóanyagból megismerheti, hogyan egy használatával C# Azure SQL-adatbázishoz való csatlakozáshoz. |
| [Csatlakozás SQL-adatbázis Java használatával](sql-database-develop-java-simple.md) | Ebből az oktatóanyagból megismerheti, hogyan csatlakozhat az Azure SQL-adatbázisokkal Java. |
| [Csatlakozás SQL-adatbázis Node.js használatával](sql-database-develop-nodejs-simple.md) | Ebből az oktatóanyagból megismerheti, hogyan csatlakozhat az Azure SQL-adatbázisokkal Node.js. |
| [Csatlakozás SQL-adatbázis PHP használatával](sql-database-develop-php-simple.md) | Ebből az oktatóanyagból megismerheti, hogyan csatlakozhat az Azure SQL-adatbázisokkal PHP. |
| [Csatlakozás SQL-adatbázis Python használatával](sql-database-develop-python-simple.md) | Ebből az oktatóanyagból megismerheti, hogyan csatlakozhat az Azure SQL-adatbázisokkal Python. |
| [SQL-adatbázis csatlakoztatása a fonetikus használatával](sql-database-develop-ruby-simple.md) | Ebből az oktatóanyagból megismerheti, hogyan csatlakozhat az Azure SQL-adatbázisokkal fonetikus. |
||||
 
## <a name="database-access"></a>Adatbázis elérése

Az alábbi oktatóanyagok fog megtudhatja, miként [hozhat létre, és a bejelentkezési adatok és a felhasználók kezelése](sql-database-manage-logins.md).

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Az Azure portálon Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabály létrehozása](sql-database-configure-firewall-settings.md)  | Ebben az oktatóanyagban megismerheti, hogy miként konfigurálható az Azure portálon SQL-adatbázis-kiszolgálói szintű tűzfalat.  |
| [Hozzon létre egy adatbázis szintű tűzfalszabályokat Transact-SQL nyelvben](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | Ebben az oktatóanyagban fog megtudhatja, hogyan hozhat létre egy adatbázis szintű tűzfalszabályokat Transact-SQL nyelvben.|
| [Használja a Transact-SQL nyelvben kiszolgálói szintű tűzfal szabályok kezelése](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | Ebből az oktatóanyagból megtanulhatja kezelése az Azure SQL-adatbázis kiszolgálói szintű tűzfal használata a Transact-SQL nyelvben.|
| [A PowerShell használatával kiszolgálói szintű tűzfal szabályok kezelése](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | Ebben az oktatóanyagban megtanulhatja, hogy miként kezelheti egy PowerShell használatával Azure SQL-adatbázis-kiszolgálói szintű tűzfalat.|
| [A REST API kiszolgálói szintű tűzfal szabályok kezelése](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | Ebből az oktatóanyagból megtanulhatja az Azure SQL-adatbázis-kiszolgálói szintű a ALAPHELYZETBE API tűzfal kezelése.|
| [Kiszolgálói szintű fő bejelentkezési használatával Azure SQL-adatbázis csatlakoztatása](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| Ebből az oktatóanyagból megismerheti, hogyan csatlakozhat az Azure SQL-adatbázisokkal kiszolgálói szintű fő bejelentkezési.|
| [Bejelentkezési adatbázis hozzáférés engedélyezése] (sql-database-manage-logins.md#granting-database-access-to-a-login() | Ebből az oktatóanyagból megismerheti, hogyan való hozzáférést adatbázis kiszolgálói szintű bejelentkezési.|
| [Hozzon létre új adatbázis-felhasználói SSMS használatával](sql-database-get-started-security.md#create-new-database-user-using-ssms) | Ebből az oktatóanyagból megismerheti, hogyan hozhat létre egy új adatbázis-felhasználói egy meglévő adatbázis SSMS használatával.|
| [Új adatbázis-felhasználói db_owner engedélyek](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | Ebből az oktatóanyagból megismerheti, hogyan engedélyeket szeretne adni egy meglévő adatbázishoz felhasználói db_owner.|
| [Olyan felhasználóként Azure SQL-adatbázis csatlakoztatása](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | Ebből az oktatóanyagból megismerheti, hogyan egy adatbázis szintű felhasználói fiók használatával Azure SQL-adatbázishoz való csatlakozáshoz.|
||||


## <a name="data-security"></a>Adatvédelem

Az alábbi oktatóanyagok megtanulhatja [Azure SQL-adatbázis adatainak](sql-database-security.md)védelme. 

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Az adatbázis az Azure portálon veszélyt észlelésének engedélyezése](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | Ebből az oktatóanyagból megismerheti, hogyan állíthatja be az Azure-portálon az adatbázis veszélyt észlelési.|
| [Bizalmas adatokat uisng mindig titkosított biztonságos](sql-database-always-encrypted-azure-key-vault.md) | Ebben az oktatóanyagban a mindig titkosított varázsló használandó biztonságossá tétele Azure SQL-adatbázisban bizalmas adatokat.|
| [Áttetsző adatok titkosítással senstive adatvédelem](https://msdn.microsoft.com/library/dn948096.aspx)| Ebből az oktatóanyagból megismerheti, hogyan senstive adatvédelem átlátszó adatok-titkosítás használatára.|
| [Titkosítsa az adatok egy oszlopban](https://msdn.microsoft.com/library/ms179331.aspx)| Ebből az oktatóanyagból megismerheti, hogyan titkosítása egy oszlopnyi adatot, használja a Transact-SQL nyelvben.|
| [Dinamikus adatok maszkolás beállítása](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | Ebből az oktatóanyagból megismerheti, hogyan állíthatja be az Azure SQL-adatbázis dinamikus adatok maszkolás. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Üzleti folytonosságot és a lekérdezés skála meg

Az alábbi oktatóanyagok megtanulhatja való használatáról az [Geo-visszaállítási és az aktív Geo-replikációs](sql-database-business-continuity.md) reccover a hibák, az üzleti folytonosságot és a lekérdezés skála meg.

| Oktatóprogram  | Leírás  |
|---|---|---|
| [Azure SQL-adatbázis visszaállítása az Azure-portálra az időt egy előző pont](sql-database-point-in-time-restore-portal.md)| Ebből az oktatóanyagból megismerheti, hogyan egy korábbi pontra az adatbázis visszaállítása az Azure portálon időben.|
| [Azure SQL-adatbázis visszaállítása az időt PowerShell egy előző pont](sql-database-point-in-time-restore-powershell.md) | Ebből az oktatóanyagból megismerheti, hogyan visszaállítása az adatbázis egy korábbi állapotba időben PowerShell használatával|
| [Az Azure-portálon törölt Azure SQL-adatbázis visszaállítása](sql-database-restore-deleted-database-portal.md) | Ebből az oktatóanyagból megismerheti, hogyan visszaállítani a törölt adatbázist az Azure portálon.|
| [A PowerShell használatá törölt Azure SQL-adatbázis visszaállítása](sql-database-restore-deleted-database-powershell.md) | Ebben az oktatóanyagban megismerheti, hogyan állíthat vissza törölt adatbázis PowerShell használatával.|
| [Az Azure portálon Azure SQL-adatbázis Geo-replikáció konfigurálása](sql-database-geo-replication-portal.md)| Ebből az oktatóanyagból megismerheti, hogyan konfigurálása az Active Geo replikációs az Azure portálon.|
| [A replikáció Geo konfigurálása az Azure SQL-adatbázis PowerShell használatával](sql-database-geo-replication-powershell.md)| Ebből az oktatóanyagból megismerheti, hogyan konfigurálása az Active Geo-replikáció PowerShell használatával.|
| [Azure SQL-adatbázis használata a Transact-SQL nyelvben Geo-replikáció konfigurálása](sql-database-geo-replication-transact-sql.md)| Ebből az oktatóanyagból megismerheti, hogyan konfigurálása az Active Geo-replikáció használata a Transact-SQL nyelvben.|
| [Tervezett vagy nem tervezett feladatátvevő kezdeményezése az Azure portálon Azure SQL-adatbázis](sql-database-geo-replication-failover-portal.md) | Ebből az oktatóanyagból megismerheti, hogyan az Azure portálon másodlagos geo replikált kópia áttérni.|
| [Tervezett vagy nem tervezett feladatátvevő kezdeményezzen Azure SQL-adatbázis PowerShell használatával](sql-database-geo-replication-failover-powershell.md) | Ebből az oktatóanyagból megismerheti, hogyan kell a PowerShell használatá másodlagos geo replikált kópia áttérni.|
| [Tervezett vagy nem tervezett feladatátvevő kezdeményezzen Azure SQL-adatbázis használata a Transact-SQL nyelvben](sql-database-geo-replication-failover-transact-sql.md) | Ebből az oktatóanyagból megismerheti, hogyan használja a Transact-SQL nyelvben másodlagos geo replikált kópia áttérni.|
||||

## <a name="data-sync"></a>Adatok szinkronizálása

Ebből az oktatóanyagból megtanulhatja, kapcsolatos [Adatok szinkronizálása](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Oktatóprogram  | Leírás  |
|---|---|---| 
| [Első lépések az SQL Azure-adatok szinkronizálása (előzetes verzió)](sql-database-get-started-sql-data-sync.md)  | Ebből az oktatóanyagból megismerheti a kapcsolatos alapismeretekről: Azure SQL-adatok szinkronizálása az Azure klasszikus portálon. |
||||

## <a name="next-steps"></a>Következő lépések

[Ismerje meg az SQL Azure adatbázis megoldás gyors indítása](sql-database-solution-quick-starts.md)
