<properties
   pageTitle="Az SQL Azure adatbázis megoldás gyors indítása |} Microsoft Azure"
   description="További tudnivalók: Azure SQL-adatbázis megoldások"
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
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Ismerje meg az SQL Azure adatbázis megoldás gyors indítása

Ez a cikk az Azure SQL adatbázis megoldás gyors indítása áttekintése tartalmazza. Ezeket a rövid elindítja a GitHub SQL Server minták adattárban található, és bemutatják a SQL-adatbázis használatát a való életben esetek alapján ideális megoldás. Egyszerű lépésenkénti oktatóprogramok, amelyek bemutatják a egy bizonyos SQL-adatbázis-szolgáltatás használatát [feltárása Azure SQL-adatbázis oktatóanyagok](sql-database-explore-tutorials.md)című témakör tartalmaz.

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Próbálja ki a WingTipTickets bemutató és a gyakorlati labor

Az [Azure SQL-adatbázis WingTipTickets](https://github.com/microsoft/wingtiptickets) bemutató és a gyakorlati labor mutatja be, az Azure SQL-adatbázishoz való értékesítés lehetősége összhangban jegyek használt minta keresési Azure-alapú alkalmazás.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Elemek összegyűjtése és monitor erőforrás-használati adatok több készletek keresztül

[Megoldás rövid útmutató: PowerShell használatával rugalmas készlet telemetriai](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) összegyűjtése és SQL-adatbázis erőforrás-kihasználtság figyelése át az előfizetés több készletek nyújt megoldást. Az adatbázisok sok előfizetést is van, esetén minden rugalmas készlet külön-külön Lync lehető.

Ez a probléma megoldásához SQL-adatbázis PowerShell-parancsmagok és az SQL-T lekérdezéseket erőforrás-használati adatok összegyűjtése több készletek és az adatbázisát is összevonhatja. Ezzel az elemzéssel figyelemmel kísérheti és elemezheti az Erőforrás kihasználtsága hatékonyabban.

Ez a rövid útmutató biztosít egy PowerShell-parancsfájlokat és az SQL-T lekérdezések dokumentáció együtt a Mit jelent a megoldást, és azt megvalósításáról.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>A szoftver használatával rugalmas adatbázis készítésének alaplépései

 [Megoldás rövid útmutató: egyéni irányítópulttá rugalmas készlet szoftver](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) megoldást nyújt a használja az SQL-adatbázis egy költséghatékony és méretezhető adatbázis kódmentes nyújt a szoftver alkalmazás rugalmas adatbázis-szolgáltatás – mint-a-megoldás (szoftver) példa.

Ez a megoldás azt ismerteti, webalkalmazást végrehajtásának keresztül. A web app lehetővé teszi a ábrázolása a terhelést a betöltés nyilvántartás-készítő alkalmazás, amely egy egyéni irányítópult alkalmazással az Azure portálon használja a rugalmas adatbázissá létrehozott.

Ezt a rövid útmutató betöltés nyilvántartás-készítő alkalmazás és a Mit jelent az alkalmazás és a használatához dokumentációjában együtt felügyeleti webalkalmazás biztosít.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Az Azure SQL-adatbázis létrehozása kód első fejlesztés és a szervezet keretrendszer használatával

A video- és az [Új adatbázis első kód](https://msdn.microsoft.com/data/jj193542.aspx) minta bevezető az új adatbázis függvénynél kód első fejlesztési. Ebben az esetben egy adatbázist, amely nem létezik, de a kód első által létrejön célként. Azt is megteheti az alkalmazási példát egy üres adatbázist, amelyhez első kód hozzáadása új táblát hoz létre.

Kód először teszi lehetővé a modell definiálása C# vagy Visual Basic .NET osztályok szerint. Választható további beállítási osztályok és tulajdonságok attribútumok vagy egy fluent API segítségével végezhet.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Rugalmas Adatbáziseszközök integráció entitás keretrendszer alkalmazásba

A [szervezet keretrendszer rugalmas adatbázis ügyfél tárban](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) példa bemutatja a a entitás keretrendszer az alkalmazás integrálhatja a [rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md)a szükséges módosításokat. A fókusz van szerkesztési [shard management megfeleltetése](sql-database-elastic-scale-shard-map-management.md) és entitás keretrendszer kód első megoldása [adatok függő útválasztás](sql-database-elastic-scale-data-dependent-routing.md) .

Ez a példa egész futó példáinkban [EF az új adatbázis minta első kód](http://msdn.microsoft.com/data/jj193542.aspx) szolgál. A minta kód, a dokumentum olvashatja el a Visual Studio mintakódok minták rugalmas adatbázis eszközök csoportja része.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Rugalmas Adatbáziseszközök integrálása sor szintű biztonság

[Rugalmas Adatbáziseszközök és sor szintű biztonsági multitenant alkalmazások](sql-database-elastic-tools-multi-tenant-row-level-security.md) jeleníti meg a módosításokat van szüksége, hogy a szervezet keretrendszer alkalmazás [rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md) integrálása [sor szintű biztonsági](https://msdn.microsoft.com/library/dn765131). Ez a példa szemlélteti, hogyan e technológiák közös létrehozásához használandó egy erősen méretezhető adatok réteg multitenant shards támogató kérelmet.

Ehhez ADO.NET SqlClient vagy entitás keretrendszer használatával. Ez a példa a [szervezet keretrendszer rugalmas adatbázis ügyfél tárban](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) bővíti a multitenant shard adatbázisok támogatása hozzáadásával.
Azt hoz létre egy egyszerű New blogok és a bejegyzéseket, négy bérlők és két multitenant shard adatbázis készítéséhez.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Online felmérés létrehozása a Dejójáték Kft felmérések alkalmazással

Ez a [Dejójáték Kft felmérések minta alkalmazás](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) multitenant webalkalmazás, felmérések, neve, amely lehetővé teszi a felhasználóknak online felmérések létrehozásához. A minta oldja meg arról, hogy miként kezelheti a egy multitenant alkalmazásban, beleértve a regisztrációs, hitelesítés, engedélyezési és alkalmazás szerepkörök felhasználói identitások néhány főbb vonatkozik.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>A Contoso klinikán bemutató alkalmazással SQL-adatbázis legújabb biztonsági szolgáltatásainak bemutatása

A [Contoso klinikán bemutató alkalmazást](https://github.com/Microsoft/azure-sql-security-sample) a legújabb biztonsági szolgáltatások SQL-adatbázis bővíthető.

## <a name="next-steps"></a>Következő lépések

[Ismerkedés az Azure SQL-adatbázis oktatóanyagok](sql-database-explore-tutorials.md)
