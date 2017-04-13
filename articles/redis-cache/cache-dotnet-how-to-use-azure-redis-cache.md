<properties 
    pageTitle="Azure vgx.dll gyorsítótár használata |} Microsoft Azure" 
    description="Megtudhatja, hogy miként az Azure alkalmazás Azure vgx.dll gyorsítótár és a teljesítmény javítása érdekében" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Azure vgx.dll gyorsítótár használata

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [NODE.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Ez az útmutató megtudhatja, hogy hogyan kezdjek hozzá az **Azure vgx.dll gyorsítótár**. Microsoft Azure vgx.dll gyorsítótárat a népszerű Megnyitás vgx.dll gyorsítótár alapul. Azt férhet hozzá egy biztonságos, célorientált vgx.dll gyorsítótár, a Microsoft kezeli. A gyorsítótár készült Azure vgx.dll gyorsítótár belül a Microsoft Azure bármely alkalmazásból érhető el.

Microsoft Azure vgx.dll gyorsítótárat a következő rétegek érhető el:

-   **Egyszerű** – egyetlen csomópontot. Több 53 GB méretű.
-   **Szabványos** – két-csomópont elsődleges/replika. Több 53 GB méretű. 99,9 % SLA.
-   **Prémium verzió** – az elsődleges két-csomópont replika legfeljebb 10 shards a. 6 GB betűméretek 530 GB (kapcsolatfelvétel további). Minden, szabványos réteg funkció és [fürt vgx.dll](cache-how-to-premium-clustering.md) [Vgx.dll adatmegőrzési](cache-how-to-premium-persistence.md)és [Azure virtuális hálózati](cache-how-to-premium-vnet.md)további beleértve támogatása. 99,9 % SLA.

Minden réteg eltérő funkciói és árak értelmez. Árak olvashat a [Gyorsítótár árak részletei][]című részben találja.

Ez az útmutató megtudhatja, hogy hogyan használja az [StackExchange.Redis][] ügyfelet használ C\# kódot. Az érintett esetek **létrehozása és konfigurálása a gyorsítótár**, **gyorsítótár-ügyfelek konfigurálása**és **hozzáadása és eltávolítása a gyorsítótárból objektumok**. Azure vgx.dll gyorsítótár használatával kapcsolatos további tudnivalókért olvassa el a [Következő lépésekkel][] részt. Tudnivalóit egy ASP.NET MVC web app gyorsítótárának vgx.dll lépésenkénti oktatóanyagot elolvashatja, [hogyan hozhat létre a gyorsítótár vgx.dll webalkalmazást](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Azure vgx.dll gyorsítótár – első lépések

Első lépések – Azure vgx.dll gyorsítótár használata egyszerű. Első lépésként kiépítése, és állítsa be a gyorsítótár. Ezután beállíthatja a gyorsítótár-ügyfelek, hogy férjen hozzá a gyorsítótár. Miután a gyorsítótár-ügyfelek van konfigurálva, megkezdheti velük a munkát.

-   [A gyorsítótár létrehozása][]
-   [A gyorsítótár-ügyfelek konfigurálása][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Hozzon létre egy gyorsítótár

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Ezt követően a gyorsítótár eléréséhez jön létre

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

A gyorsítótárazás beállításával kapcsolatos további tudnivalókért lásd: [Azure vgx.dll gyorsítótár beállítása](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>A gyorsítótár-ügyfelek konfigurálása

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Miután a ügyfél projekt gyorsítótárazás van konfigurálva, a gyorsítótárat a az alábbi szakaszokban ismertetett módszereket is használhatja.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Gyorsítótárát használata

Ez a szakasz lépései bemutatják, hogyan gyorsítótár gyakori feladatok elvégzéséhez.

-   [A gyorsítótár csatlakoztatása][]
-   [Hozzáadása és objektumok beolvasni a gyorsítótárból][]
-   [A gyorsítótárban lévő .NET-objektumok használata](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>A gyorsítótár csatlakoztatása

A gyorsítótár programozás útján dolgozni, a gyorsítótár hivatkozás van szüksége. Adja hozzá a következő bármely fájl, amelyből a StackExchange.Redis ügyfél-Azure vgx.dll gyorsítótár eléréséhez használni kívánt tetején.

    using StackExchange.Redis;

>[AZURE.NOTE] A StackExchange.Redis ügyfél .NET-keretrendszer 4-es vagy újabb szükséges.

A kapcsolat az Azure vgx.dll gyorsítótár kezeli a `ConnectionMultiplexer` osztály. Az osztály célja, hogy a megosztott, és az ügyfélalkalmazás egészében újrafelhasználható, és nem szükséges, egy műveletet alapon hozható létre. 

Az Azure vgx.dll gyorsítótár kapcsolódhat, és vissza kell egy csatlakoztatott egy példányának `ConnectionMultiplexer`, hívja fel a statikus `Connect` módszer és a gyorsítótár végpont és kulcs fázis, például a következő példa. A jelszó paraméterként Azure portálról generált billentyűvel.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Figyelmeztetés: Soha ne tár hitelesítő adatai forráskód. Ahhoz, hogy ez a Példa egyszerű, e vagyok megjelenítve őket a forráskód. [Hogyan alkalmazás karakterláncok és a kapcsolati karakterláncot munka][] talál tájékoztatást arról, hogyan hitelesítő adatainak tárolása.

Ha nem szeretné használni az SSL, vagy állítsa `ssl=false` vagy nincs megadva a `ssl` paraméter.

>[AZURE.NOTE] Az-SSL port új gyorsítótárát alapértelmezés szerint le van tiltva. Az-SSL port engedélyezésével kapcsolatos útmutatásért lásd: az [Access-portok](cache-configure.md#access-ports).

Megosztási egyik megközelítés egy `ConnectionMultiplexer` példányt az alkalmazásban, hogy egy statikus tulajdonságot, amely visszaadja a csatlakoztatott példány, a következőhöz hasonló. Egyetlen kapcsolódó inicializálni szál vannak ezáltal `ConnectionMultiplexer` példány. Ezekben a példákban `abortConnect` értéke HAMIS, ami azt jelenti, hogy a hívás sikeres lesz, ha nem az Azure vgx.dll gyorsítótárba kapcsolatot létesíteni. Egy fontos szolgáltatása `ConnectionMultiplexer` , hogy csatlakozási automatikusan visszaállítja a gyorsítótár egyszer a hálózati probléma, vagy más okai megszűnt-e.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

A speciális csatlakozási beállítások további tudnivalókért lásd: [StackExchange.Redis konfigurációs modell][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Miután a kapcsolat létrejött, hivatkozását adja eredményül a gyorsítótár vgx.dll adatbázis hívja fel a `ConnectionMultiplexer.GetDatabase` módot. Az objektum által visszaadott a `GetDatabase` módszer könnyű átadó objektum, és nem kell tárolni.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Most, hogy miként csatlakozhat Azure vgx.dll gyorsítótár-példányhoz, és a gyorsítótár adatbázis hivatkozását adja eredményül, most tekintsünk át egy a gyorsítótár használata.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Hozzáadása és objektumok beolvasni a gyorsítótárból

Elemek tárolja, és segítségével gyorsítótárból lekéri a `StringSet` és `StringGet` módszereket.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Vgx.dll tárolja a lehető legtöbb adatot vgx.dll karakterláncok, de ezek a karakterláncok számos különböző típusú adatok szerializált bináris adatokat, amelyeket is használható, ha objektumokra tárolása .NET gyorsítótár együtt is tartalmazhat.

Hívásakor `StringGet`, az objektum létezik, ha a ad vissza, ha nem, és `null` értéket adja vissza. Ebben az esetben az érték beolvasása a kívánt adatforrásból, tárolja azt a későbbi felhasználásra a gyorsítótár. A gyorsítótár-függetlenül minta nevezik.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

A gyorsítótárban lévő elem lejártát megadásához használja a `TimeSpan` paramétere: `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>A gyorsítótárban lévő .NET-objektumok használata

Azure vgx.dll gyorsítótár is gyorsítótár-.NET-objektumok, valamint az egyszerű adattípusok, de a .NET-objektum gyorsítótárba helyezhető előtt meg kell használhatók, amelyekről. Az alkalmazás fejlesztője feladata, és a fejlesztői rugalmasságot biztosít a szerializáló kiválasztásában.

Objektumok szerializálni egy egyszerű módja a `JsonConvert` [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) szerializálási módszerek és és a JSON szerializálni. A következő példa bemutatja a get- és beállítása használata egy `Employee` példány.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Következő lépések

Most, hogy már megtanulta alapvető, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure vgx.dll gyorsítótár.

-   Nézze meg a ASP.NET-szolgáltatók Azure vgx.dll gyorsítótárhoz.
    -   [Azure vgx.dll munkamenet-állapot-szolgáltató](cache-aspnet-session-state-provider.md)
    -   [Azure vgx.dll gyorsítótár ASP.NET kimeneti gyorsítótár-szolgáltató](cache-aspnet-output-cache-provider.md)
-   A [gyorsítótár diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) , így [monitor](cache-how-to-monitor.md) a gyorsítótár állapotának. A mérési módja miatt az Azure-portálon megtekintése és is [letöltése, és tekintse át](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) őket az eszközök lehetőség használatával.
-   Nézze meg a [StackExchange.Redis gyorsítótár ügyfél dokumentációt][].
    -   Azure vgx.dll gyorsítótár a vgx.dll ügyfelek és fejlesztési nyelvekhez érhetők el. További tudnivalókért olvassa el a [http://redis.io/clients][]című témakört.
-   Azure vgx.dll gyorsítótár is kínál külső szolgáltatások és eszközök, például Redsmin és az asztali kezelője vgx.dll.
    -   Redsmin kapcsolatos további tudnivalókért olvassa el a [lekérése az Azure vgx.dll kapcsolati karakterláncot, és vele együtt Redsmin][]című témakört.
    -   Az Azure, vgx.dll egy grafikus [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)használata a gyorsítótárban lévő adatok vizsgálata és hozzáférést.
-   A [Vgx.dll][] dokumentációjában és kapcsolatos [Vgx.dll adattípusok][] és [adattípusok vgx.dll a 15 perces bemutatása][].



<!-- INTRA-TOPIC LINKS -->
[Következő lépések]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[A gyorsítótár létrehozása]: #create-cache
[Configure the cache]: #enable-caching
[A gyorsítótár-ügyfelek konfigurálása]: #NuGet
[Working with Caches]: #working-with-caches
[A gyorsítótár csatlakoztatása]: #connect-to-cache
[Hozzáadása és objektumok beolvasni a gyorsítótárból]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Hogyan lekérése az Azure vgx.dll kapcsolati karakterláncot, és használata a Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis konfigurációs modell]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Gyorsítótár-árakról]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis gyorsítótár ügyfél dokumentáció]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Vgx.dll]: http://redis.io/documentation
[Adattípusok vgx.dll]: http://redis.io/topics/data-types
[adattípusok vgx.dll a 15 perces – bevezetés]: http://redis.io/topics/data-types-intro

[Alkalmazás karakterláncok és a kapcsolati karakterláncot működése]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


