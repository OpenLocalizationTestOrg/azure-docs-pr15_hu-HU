<properties 
    pageTitle="Rugalmas adatbázis ügyfél-tár használata Dapper |} Microsoft Azure" 
    description="Rugalmas adatbázis ügyfél-tár használata Dapper." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Dapper rugalmas adatbázis ügyfél-tár használata 

A dokumentum van a fejlesztők számára, a Dapper alkalmazások létrehozásához használja arra, de is szeretne kibővül [Rugalmas adatbázis szerszámok](sql-database-elastic-scale-introduction.md) sharding skála meg az adatok réteg az alkalmazások létrehozásához.  A dokumentum Dapper-alapú alkalmazások rugalmas Adatbáziseszközök integrálása szükséges módosításait mutatja be. A fókusz van a szerkesztési rugalmas adatbázis shard kezelését és adatok függő útválasztás Dapper. 

**Példakódot**: [Azure SQL-adatbázis - Dapper integrációs rugalmas Adatbáziseszközök](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
**Dapper** és **DapperExtensions** rugalmas adatbázis-ügyfél tárral integrálása az Azure SQL-adatbázis használata egyszerű. Az alkalmazások függő módosítása a létrehozás, majd az új [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektumok útválasztási adatok segítségével a [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hívás az [ügyfél-tár](http://msdn.microsoft.com/library/azure/dn765902.aspx)használata. Ez az alkalmazás csak a módosítások korlátozza, ahol új kapcsolatok létrehozása és megnyitása. 

## <a name="dapper-overview"></a>Dapper – áttekintés
**Dapper** egy objektum relációs hozzárendelést. Azt rendeli hozzá a .NET-objektumok a levelezőprogramból relációs adatbázisból (vagy fordítva). A minta kód első része azt szemlélteti, hogyan integrálhatja a rugalmas adatbázis ügyfél tár Dapper alapú alkalmazásokkal. A második rész a példakódot szemlélteti, hogyan integrálása Dapper és a DapperExtensions használata esetén.  

A hozzárendelést funkciókat Dapper bemutat bővítmény a küldő T-SQL-utasítások végrehajtási vagy az adatbázis lekérdezése leegyszerűsíti az adatbázis-kapcsolatokat. Ha például Dapper egyszerűen feleltesse meg a .NET-objektumok és SQL-utasításait a **végrehajtás** hívások paramétereinek között, illetve felhasználása az SQL-lekérdezés eredményének használata a **lekérdezési** hívásait Dapper .NET-objektumot. 

Amikor DapperExtensions, már nincs szüksége az SQL-utasítások megadását. Bővítmények módszerek például **GetList** meg vagy **szúrja be** az adatbázis-kapcsolaton keresztül hozzon létre Bepillantás a színfalak mögé SQL-utasításait.
 
Egy másik Dapper, és DapperExtensions előnye, hogy az alkalmazás vezérlők létrehozása az adatbázis-kapcsolatot. Ezzel az elemzéssel az ügyfél rugalmas adatbázis-tár, amely kereskedők adatbázis-adatbázisokhoz shardlets hozzárendelése alapján kapcsolatok használata.

A Dapper szerelvények című témakörben kaphat [nettó Dapper pont](http://www.nuget.org/packages/Dapper/). A Dapper bővítmények olvassa el a [DapperExtensions](http://www.nuget.org/packages/DapperExtensions)című témakört.

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Az ügyfél rugalmas adatbázis library gyors áttekintése

Rugalmas adatbázis ügyfél tárral határozza meg az alkalmazás adatok nevű *shardlets* partíciók, adatbázisokhoz feleltesse meg őket, és *sharding kulcsok*azonosítja őket. Tetszőleges számú adatbázist kell lennie, vagy a shardlets elosztása az adatbázisok is. Az adatbázisok fő értékek sharding hozzárendelése a tár API-k által biztosított shard térkép tárolja. Ez a képesség neve **shard megfeleltetése kezelése**. A shard térkép is figyelmezteti arra az adatbázis-kapcsolatokat sharding kulcs szállítására kérelmekhez közvetítő. Ez a képesség nevezik **függő útválasztási adatok**.

![Shard térképekről és az adatok függő továbbítása][1]

A shard térkép kezelő felhasználók megakadályozza shardlet adatokká akkor fordulhat elő, amikor egyidejű shardlet adatkezelési műveletek vannak történik az adatbázist, nem következetes nézetek. Ehhez a shard térképek broker az alkalmazás, a tár épülő az adatbázis-kapcsolatokat. Shard adatkezelési műveletek hatással lehetnek a shardlet, amikor ez lehetővé teszi a térkép shard funkciók automatikusan törlése egy adatbázis-kapcsolatot. 

A hagyományos módon hozhatnak létre kapcsolatot a Dapper helyett [OpenConnectionForKey módszer](http://msdn.microsoft.com/library/azure/dn824099.aspx)szükség. Biztosan megtörténjen az, hogy mi történjen, az összes adatérvényesítési és kapcsolatok kezelése megfelelően amikor adatokat shards közé helyezi.

### <a name="requirements-for-dapper-integration"></a>Dapper integrációs vonatkozó követelmények

Amikor a rugalmas adatbázis ügyfél tár és a Dapper API-khoz is, szeretnénk őrizni a következő tulajdonságokat:

* **Scaleout**: hozzáadása vagy törlése az adatok réteg alkalmazásának sharded kapacitás igényeivel szükség szerint az alkalmazás adatbázisok szeretnénk. 

-    **Egységesebb**: az alkalmazás átméretezi sharding használni, mivel függő útválasztási adatok végrehajtásához szükséges. Az adatok függő útválasztási funkciók a tár használni ehhez szeretnénk. Különösen szeretnénk őrizni az érvényesítési, és egységesebb biztosítja, hogy a jövőben elkerülhesse sérülést vagy helytelen lekérdezés eredménye a shard térkép kezelő keresztül vannak brokered kapcsolatok által biztosított. Ezzel biztosíthatja, hogy a kapcsolatokat az egy adott shardlet elutasítva vagy ha (például) a shardlet jelenleg helyez át egy másik shard felosztása és egyesítése API-k használata leállt.

-    **Objektum leképezése**: szeretnénk, amely megőrzi az osztályok alkalmazásban és az alapul szolgáló adatbázis struktúrák között szeretne Dapper által biztosított megfeleltetések kényelme mellett használják. 

A következőkben nyújt útmutatást ezeknek a követelményeknek, az alkalmazások **Dapper** és **DapperExtensions**alapján.

## <a name="technical-guidance"></a>Technikai útmutató
### <a name="data-dependent-routing-with-dapper"></a>Függő a Dapper útválasztási adatok 

A Dapper az alkalmazás felelős általában a hozhat létre, és megnyitja a az alapul szolgáló adatbázis-kapcsolatot. A T típusú képlettel az alkalmazást, Dapper eredményt ad, lekérdezés, t Dapper típusú .NET gyűjtemények hajtja végre a hozzárendelés az SQL-T eredmény soraiból származó t típusú az objektumok Hasonlóképpen Dapper SQL értékek vagy adatok kezelő nyelv kimutatások paramétereinek .NET objektumok rendeli hozzá. Dapper bővítmény módszerek a ADO .NET SQL ügyfélhez tárakból a normál [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektumon keresztül ezt a funkciót kínál. Az SQL-kapcsolat esetén a skála rugalmas API-k által visszaadott DDR is normál [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektumok. Ebben a csoportban adhatja us közvetlenül használni kívánt Dapper bővítmények az ügyfél tár DDR API, által visszaadott fölé egyben egyszerű SQL ügyfélhez kapcsolat.

E megfigyelések ellenőrizze az Dapper a rugalmas adatbázis ügyfél Library brokered kapcsolatok használata egyszerű.

A kód példa (a mellékelt minta) azt szemlélteti, hogy hol a sharding billentyűt a kapcsolatot a megfelelő shard broker abba a dokumentumtárba, az alkalmazás nyújtotta megközelítés.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

A hívást a [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API felváltja az alapértelmezett létrehozását és a kapcsolat SQL ügyfélhez megnyitásakor. A [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hívás hajtja végre a szükséges adatok függő útválasztás argumentumokat: 

-    A shard térkép függő útválasztási kapcsolódási adatok eléréséhez
-    A sharding billentyűt a shardlet azonosítása
-    A hitelesítő adatok (felhasználónév és jelszó) a shard csatlakoztatása

A térkép shard objektum kapcsolatot hoz létre, amely tartalmazza az adott sharding billentyű a shardlet a shard. Az adatbázis rugalmas ügyfélprogram API-khoz is címke a kapcsolatot a konzisztencia garanciákkal végrehajtásához. [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hívás normál SQL ügyfélkapcsolati objektumot ad vissza, mivel a Dapper a **végrehajtás** bővítmény módszer a további hívásokat követi a szabványos Dapper gyakorlat.

Lekérdezések nagyon ugyanúgy működnek, – meg kell nyitnia a kapcsolat [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API ügyfélprogramból. A normál Dapper bővítmény módszerek majd az SQL-lekérdezés eredményei feleltetni a .NET-objektumok átalakítása használja:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Megjegyzendő, hogy a DDR kapcsolatot **használ** letiltani a egy shard, ahol tenantId1 legyen a letiltás belül az összes adatbázis-műveletek hatókörök. A lekérdezés csak az aktuális shard-on tárolt blogok, de nem a bármely más shards-on tárolt lehetőségekből adja vissza. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Függő Dapper és DapperExtensions útválasztási adatok

Egy további bővítmények, amelyek nyújtanak további kényelmesebbé és hardverabsztrakciós az adatbázisból, ha az adatbázis-alkalmazások fejlesztésével ökológiai Dapper megtalálható. DapperExtensions képen. 

DapperExtensions használata az alkalmazás nem változtatja meg az adatbázis-kapcsolatokat hogyan létrehozása és kezelése. Feladata még az alkalmazás kapcsolatok ablak megnyitásához, és a bővítmény módszerekkel várható normál SQL-ügyfél kapcsolati objektumokat. Azt számíthat az [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) feletti ismertetettek szerint. Ahogy a következő mintakódok megjelenítéséhez csak módosítása az, hogy azt már nem kell írni a T-SQL-utasításait:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

És Íme, a kód minta a lekérdezés: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Ideiglenes (tranziens) hibák kezelése

A Microsoft Patterns és eljárások csapatának [Tranziens hibafa kezelése-alkalmazás letiltása](http://msdn.microsoft.com/library/hh680934.aspx) érdekében fordul elő, ha a felhőben futó közös tranziens hibafa feltételek csökkentésében alkalmazásfejlesztő közzé. További tudnivalókért lásd: [Perseverance, az összes Triumphs titkos: tranziens hiba kezelésének alkalmazás Tiltás használatával](http://msdn.microsoft.com/library/dn440719.aspx).

A kód minta tranziens hibák elleni védelem az ideiglenes (tranziens) hiba tár támaszkodik. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** a kódban definíciója egy **SqlDatabaseTransientErrorDetectionStrategy** a 10 és 5 másodperc várakozás újrapróbálkozások között eltelt idő kísérletek számát. Ha a tranzakciók használ, ellenőrizze, hogy a újrapróbálkozási hatókör vissza egy ideiglenes (tranziens) hiba esetén a tranzakció elejére.

## <a name="limitations"></a>Korlátozások

Az ehhez a dokumentumhoz a ismertetett eljárások korlátozások, néhány foglalja magában:

* A minta kódot a dokumentum nem szemléltetik séma kezelése shards keresztül.
* Adott kérést, feltételezzük, hogy az adatbázis-feldolgozás belül található egy egyetlen shard a sharding billentyűt a kérelem által biztosított által meghatározott. Azonban a feltételezésen nem mindig rendelkeznek, például ha nem egy sharding kulcs elérhetővé tenni. Ez az a cím, a rugalmas adatbázis ügyfél tárat a [MultiShardQuery osztály](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx)tartalmaz. Az osztály hajtja végre a kapcsolat hardverabsztrakciós felett több shards lekérdezésére. A dokumentum túlra van MultiShardQuery Dapper együttes alkalmazásával.

## <a name="conclusion"></a>Elfogadásáról

Alkalmazások Dapper és DapperExtensions használatával egyszerűen előnyeivel rugalmas Adatbáziseszközök Azure SQL-adatbázis. Keresztül című cikk lépéseit a dokumentumban az alkalmazások használata az eszközt lehetőséget függő módosítása a létrehozás, majd az új [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektumok útválasztási adatok is használhatja a rugalmas adatbázis ügyfél tár a [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) hívást. Ezzel a korlátozza a szükséges a e helyek, ahol új kapcsolatokat létrehozott és megnyitott alkalmazás módosításokat. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 