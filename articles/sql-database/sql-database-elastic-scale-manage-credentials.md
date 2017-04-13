<properties 
    pageTitle="Az adatbázis rugalmas ügyfél beállításai párbeszédpanelen a hitelesítő adatok kezelése |} Microsoft Azure" 
    description="Hogyan kell beállítani a megfelelő szintű hitelesítő adatait, rendszergazdát, hogy csak olvasható, az rugalmas adatbázis-alkalmazások" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Az adatbázis rugalmas ügyfél tára eléréséhez használt hitelesítő adatok

A [Rugalmas adatbázis ügyfél tár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) hitelesítő adatok három különböző típusú használja a [shard térkép manager](sql-database-elastic-scale-shard-map-management.md)érhető el. Attól függően, hogy szükség van a hitelesítő adatok használata az access lehetséges a legalacsonyabb.

* **Adatkezelési hitelesítő adatok**: létrehozásának és kezelésére szolgáló shard térkép kezelőjének. (Lásd: a [Szószedet](sql-database-elastic-scale-glossary.md).) 
* **Hozzáférési hitelesítő adatok**: shards információt szeretne kapni egy meglévő shard térkép manager eléréséhez.
* **Kapcsolat hitelesítő adatok**: shards csatlakozni. 

Lásd még: [adatbázisok kezelése és bejelentkezések Azure SQL-adatbázisban](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Tudnivalók a hitelesítő adatok

Hozzon létre egy [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objektumot shard térképek módosítására alkalmazásokhoz kezelése adatokat használja. (Például lásd: [egy rugalmas Adatbáziseszközök használatával shard hozzáadásával](sql-database-elastic-scale-add-a-shard.md) , és a [függő útválasztási adatok](sql-database-elastic-scale-data-dependent-routing.md)) A skála rugalmas ügyfél tárban felhasználójának az SQL-felhasználók és az SQL-bejelentkezések hoz létre, és ellenőrzi, minden egyes engedéllyel rendelkezik az olvasási/írási a globális shard adatbázist és az összes shard adatbázistípus. Ha meg szeretné őrizni a globális shard térképen és a helyi shard térképeket a shard térkép módosítások végrehajtásának sorrendjét a hitelesítő adatokat használják. A shard térkép kezelő objektum létrehozásához használja például hitelesítő adatok kezelése ( [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)használata: 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

A változó **smmAdminConnectionString** a kapcsolati karakterlánc, amely tartalmazza a hitelesítő adatok kezelése. A felhasználói azonosítója és jelszava itt shard térkép adatbázis és az egyes shards olvasási/írási hozzáféréssel. Az adatkezelési kapcsolati karakterlánc is tartalmaz, a kiszolgáló neve és az adatbázis neve azonosítja a globális shard adatbázist. Erre a célra a következő tipikus kapcsolati karakterláncot:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Ne használjon értékek formájában "username@server"—instead egyszerűen írja be a "felhasználónév" értéket.  Ennek oka az, hitelesítő adatokat kell dolgozniuk a shard térkép manager-adatbázis és az egyes shards, esetleg különböző kiszolgálókon szemben.

## <a name="access-credentials"></a>Hozzáférési hitelesítő adatok
  
Egy shard térkép kezelő létrehozása olyan alkalmazás, amely nem felügyelete shard térképeket, használja a hitelesítő adatait a globális shard térképen a csak olvasható jogosultsággal kell rendelkeznie. Ezeket a hitelesítő adatok területén globális shard leképezés beolvasott adatok [útválasztási adatok függő](sql-database-elastic-scale-data-dependent-routing.md) és a shard leképezés gyorsítótárát az ügyfélgépen kitöltéséhez használt. A hitelesítő adatok szolgáltatáson keresztül azonos hívás típusúak **GetSqlShardMapManager** az alább látható módon: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Megjegyzés: a használható különböző hitelesítő adatokat a hozzáférés **nem szerepkörű** felhasználók nevében megfelelően a **smmReadOnlyConnectionString** felhasználása: ezek a hitelesítő adatok nem biztosítania kell írási engedélye a globális shard térképen. 

## <a name="connection-credentials"></a>Kapcsolat hitelesítő adatok 

További hitelesítő adatokra van szükség, ha a [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) módszerrel egy társított sharding kulcs shard eléréséhez. A hitelesítő adatokat kell megadnia a helyi shard térkép táblákat a shard tartózkodó csak olvasható elérésére vonatkozó engedélyek. Ez kapcsolat érvényesítése az adatok függő útválasztás a shard végrehajtásához szükséges. A kódtöredék lehetővé teszi, hogy adatokhoz való hozzáférés az adatok függő útválasztási környezetben: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

Ebben a példában a **smmUserConnectionString** tárolja a kapcsolati karakterláncot, a felhasználói hitelesítő adatokat. Azure SQL-adatbázis Íme egy tipikus kapcsolati karakterláncot, a felhasználói hitelesítő adatokat: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

A rendszergazdai hitelesítő adatait, és nem értékek formájában "username@server". Ehelyett egyszerűen használja "felhasználónév".  Tartsa szem előtt, hogy a kapcsolati karakterlánc nem tartalmaz a kiszolgáló nevét és az adatbázis nevét. Ennek oka az, a **OpenConnectionForKey** hívás automatikusan irányítsa át a kapcsolatot a megfelelő shard kulcs alapján. Emiatt az adatbázis nevét, és a kiszolgáló neve nem találhatók. 

## <a name="see-also"></a>Lásd még:
[Adatbázisok és bejelentkezések az Azure SQL-adatbázis kezelése](sql-database-manage-logins.md)

[Az SQL-adatbázis biztonságossá tétele](sql-database-security.md)

[Rugalmas adatbázis feladatok használatába](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 