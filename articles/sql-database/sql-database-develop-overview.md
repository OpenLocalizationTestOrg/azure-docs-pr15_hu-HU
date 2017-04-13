<properties
    pageTitle="SQL-adatbázis kidolgozása áttekintése |} Microsoft Azure"
    description="Tudnivalók a rendelkezésre álló kapcsolódási tárak és a gyakorlati tanácsok az SQL-adatbázis csatlakoztatása az alkalmazások."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>SQL-adatbázis fejlesztése – áttekintés
Az alábbiakban ismertetjük a alapvető szempontjait, hogy a fejlesztők tisztában kell lennie Azure SQL-adatbázis csatlakoztatása a kód beírásakor.

## <a name="language-and-platform"></a>A nyelvi és a platform
Nincsenek mintakódok különböző programozási nyelven és platformokon. A kód minták mutató hivatkozásokat talál: 

* További információ: [kapcsolat tárak SQL-adatbázis és az SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Az erőforrás-korlátozások
Azure SQL-adatbázis kezelése két különböző mechanizmusok segítségével adatbázis rendelkezésére álló erőforrásokat: erőforrások és a végrehajtás vonatkozó korlátozások.

* További információ: [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md)

## <a name="security"></a>Biztonsági
Azure SQL-adatbázis erőforrások biztosít, a hozzáférés korlátozása jelszóval védenie az adatok, valamint a tevékenységek egy SQL-adatbázisban figyelése.

* További információ: [az SQL-adatbázis biztonságossá tétele](sql-database-security.md)

## <a name="authentication"></a>Hitelesítés
* Azure SQL-adatbázis egyaránt SQL Server-hitelesítés felhasználói bejelentkezések, valamint [Azure Active Directory authentication](sql-database-aad-authentication.md) és felhasználók bejelentkezések támogatja.
* Adjon meg egy adott adatbázis helyett a *fő* adatbázist alapértelmezett szüksége.
* Váltani szeretne egy másik adatbázist nem használható a Transact-SQL nyelvben **használata myDatabaseName;** kimutatás SQL-adatbázisban.
* További információ: [SQL-adatbázis biztonsági: adatbázis az access és a bejelentkezési biztonságának kezelési](sql-database-manage-logins.md)

## <a name="resiliency"></a>Tűrőképessége
Ha egy ideiglenes (tranziens) hiba történik az SQL-adatbázishoz való csatlakozáskor, a kód meg kell ismételni a hívást.  Azt javasoljuk, hogy újra logika használata visszalépési logika, hogy nem elborít az SQL-adatbázis újbóli próbálkozás egyszerre több ügyfelekkel.

* Minták kód: mintakódok, amelyek bemutatják az újbóli próbálkozáshoz logika a választott nyelv minták lásd: [a kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md)
* További információ: [az SQL-adatbázis ügyfélprogramok hibaüzenetek](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Kapcsolatok kezelése
* Az ügyfél kapcsolati logika a bírálja felül az alapértelmezett időtúllépés 30 másodperces kell.  15 másodperc alapértelmezés szerint az interneten függő túl rövid a kapcsolatokat.
* A [kapcsolat készlet](http://msdn.microsoft.com/library/8xx3tyca.aspx)használja, ne a kapcsolatot a program nem aktívan használja azt, és újra felhasználhatja azt nem készül azonnali bezárása.

## <a name="network-considerations"></a>Hálózati kapcsolatos szempontok
* Azon a számítógépen, amelyen az ügyfélprogram győződjön meg róla, a tűzfal lehetővé teszi, hogy a kimenő TCP-kapcsolati port 1433.  További információ: [az SQL Azure-adatbázis tűzfal konfigurálása](sql-database-configure-firewall-settings.md)
* Ha az ügyfélprogram SQL-adatbázis V12 csatlakozik, az ügyfél-Azure virtuális gépen (virtuális) futása közben, meg kell nyitnia a virtuális bizonyos port tartományok. További információ: [túl ADO.NET 4.5 és SQL-adatbázis V12 1433 portok](sql-database-develop-direct-route-ports-adonet-v12.md)
* Azure SQL-adatbázis V12 ügyfélkapcsolatok előfordul, hogy a proxy átlépése, és közvetlenül az adatbázis használhatják. Portokon kívül 1433 fontos válnak. További információ: [túl ADO.NET 4.5 és SQL-adatbázis V12 1433 portok](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Adatok Sharding a skála rugalmas
Skála rugalmas leegyszerűsíti a Méretezés (és). 

* [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázissal tervezés mintázatok](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Függő útválasztási adatok](sql-database-elastic-scale-data-dependent-routing.md)
* [Azure SQL-adatbázis skála rugalmas előzetes verzió – első lépések](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Következő lépések

Ismerkedjen meg az [SQL-adatbázis lehetőségeit](https://azure.microsoft.com/services/sql-database/).
