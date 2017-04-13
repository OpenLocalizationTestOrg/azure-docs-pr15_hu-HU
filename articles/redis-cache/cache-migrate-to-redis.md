<properties 
    pageTitle="Gyorsítótár áttelepítése vgx.dll |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti át az Azure vgx.dll gyorsítótár alkalmazások gyorsítótár-szolgáltatás kezelése"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Azure vgx.dll gyorsítótár felügyelt gyorsítótár-szolgáltatás áttelepítése

Az alkalmazás, attól függően, hogy a gyorsítótár alkalmazás által használt gyorsítótár-szolgáltatás kezelt szolgáltatások minimális módosításokkal végezhető el az Azure vgx.dll gyorsítótár Azure felügyelt gyorsítótár-szolgáltatás használó alkalmazások áttelepítése. Miközben az API-k megegyeznek nem pontosan hasonlóak, és nagy, a meglévő kódot használó gyorsítótár-szolgáltatás kezelése a gyorsítótár eléréséhez újrafelhasználható minimális módosításokkal. Ez a témakör szemlélteti, hogyan lehet, hogy a szükséges konfigurációs és a gyorsítótár-szolgáltatás kezelése alkalmazásait Azure vgx.dll gyorsítótár áttelepítendő alkalmazás módosításait, és bemutatja, hogyan Azure vgx.dll gyorsítótár funkciók közül egyesek használható gyorsítótár-szolgáltatás kezelése a gyorsítótárban működésének végrehajtásához.

## <a name="migration-steps"></a>Az áttelepítési lépések

Az alábbi lépéseket a gyorsítótár-szolgáltatás kezelése-alkalmazások használata az Azure vgx.dll gyorsítótár áttelepítése van szükség.

-   Azure vgx.dll gyorsítótár gyorsítótár-szolgáltatás kezelt szolgáltatások megfeleltetése
-   Válassza ki a gyorsítótár felületek
-   Hozzon létre egy gyorsítótár
-   A gyorsítótár-ügyfelek konfigurálása
    -   Távolítsa el a felügyelt gyorsítótár-szolgáltatás beállításai
    -   A StackExchange.Redis NuGet csomag használata gyorsítótár ügyfél beállítása
-   Gyorsítótár-szolgáltatás felügyelt kódot áttelepítése
    -   Csatlakozás a gyorsítótárat a ConnectionMultiplexer osztály használatával
    -   Az Access egyszerű adattípusok gyorsítótár
    -   A gyorsítótárban lévő .NET-objektumok használata
-   ASP.NET-munkamenet állapot és a Weblapkimeneti gyorsítótárazás beállításával Azure vgx.dll gyorsítótárba áthelyezése 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Azure vgx.dll gyorsítótár gyorsítótár-szolgáltatás kezelt szolgáltatások megfeleltetése

Azure gyorsítótár-szolgáltatás kezelése és Azure vgx.dll gyorsítótár hasonlóak, de egyes a szolgáltatások különböző módokon végrehajtása. Ez a szakasz ismerteti a különbségeket részét és útmutatást Azure vgx.dll gyorsítótárban megvalósítási gyorsítótár-szolgáltatás kezelése a.

|Felügyelt gyorsítótár-szolgáltatás|Felügyelt gyorsítótár-szolgáltatás támogatása|Azure vgx.dll gyorsítótár-támogatás|
|---|---|---|
|Névvel ellátott gyorsítótárát|Be van állítva egy alapértelmezett gyorsítótár, és a normál és prémium gyorsítótár szeretne rendelni, felfelé kilenc további gyorsítótárát nevű beállítható úgy, ha szükségesnek látja.|Az adatbázisok (alapértelmezett 16) az elnevezett gyorsítótárát hasonló funkciót végrehajtásához használható konfigurálható Azure vgx.dll gyorsítótárát van. További tudnivalókért olvassa el a [alapértelmezett vgx.dll kiszolgáló konfigurálása](cache-configure.md#default-redis-server-configuration)című témakört.|
|Magas elérhetősége|Itt magas elérhetősége elemek a gyorsítótár kiürítése a szokásos és prémium gyorsítótár szeretne rendelni. Ha egy hiba miatt elvesznek a cikkek, a gyorsítótárban lévő elemek biztonsági másolatait továbbra is elérhetők. A másodlagos gyorsítótár ír a szinkron történik.|A szokásos és prémium gyorsítótár szeretne rendelni, amelyek a két csomópont elsődleges/replika konfiguráció (a prémium gyorsítótár minden shard az elsődleges/replika két) magas elérhetősége érhető el. A replika ír a aszinkron történik. További tudnivalókért olvassa el a [Azure vgx.dll gyorsítótár árak](https://azure.microsoft.com/pricing/details/cache/)című témakört.|
|Értesítések|Lehetővé teszi az ügyfelek aszinkron értesítést kapni, egy névvel ellátott gyorsítótáron számos gyorsítótár műveletek végrehajtásakor.|Ügyfélalkalmazások segítségével vgx.dll pub/sub vagy [Keyspace értesítések](cache-configure.md#keyspace-notifications-advanced-settings) elérése értesítések hasonló funkciót.|
|Helyi gyorsítótár|A gyorsítótárban tárolt objektumok másolatának helyileg tárolja az ügyfelet a létre nagyon gyors hozzáférést.|Ügyfélalkalmazások kellene ezt a funkciót a szótár vagy hasonló adatstruktúra végrehajtásához.|
|Eviction házirend|Nincs vagy LRU. Az alapértelmezett házirend LRU.|Azure vgx.dll gyorsítótár támogatja a következő eviction házirendek: ideiglenes-lru, allkeys-lru, ideiglenes véletlen, allkeys-véletlen ideiglenes ttl, noeviction. Az alapértelmezett házirend ideiglenes-lru. További tudnivalókért olvassa el a [alapértelmezett vgx.dll kiszolgáló konfigurálása](cache-configure.md#default-redis-server-configuration)című témakört.|
|Érvényességi szabály|Az alapértelmezett jelszólejárati házirendet abszolút és az alapértelmezett lejárati intervallum tíz perccel. Csúszó és soha házirendek is elérhetők.|Alapértelmezés szerint a gyorsítótárban lévő elemek ne járjon le, de az elévülési gyorsítótár beállítása túlterhelések használatával / írási alapon beállíthatók. További tudnivalókért lásd: a [Hozzáadás gombra, és a lekérés objektumok a gyorsítótárból](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Régiók és a Címkézhetőség.|Érhető el a gyorsítótárban tárolt elemek alcsoportok. Régiók is támogatja a gyorsítótárban tárolt elemek széljegyzet címkék nevű további leíró karakterláncok. Régiók támogatja az azt jelenti, hogy az adott régióban bármely címkézett elemek keresési műveletek hajthatók végre. Egy régión belüli összes elemek találhatók a gyorsítótár fürt egyetlen csomópont belül.|Egy vgx.dll gyorsítótár, ahol a egyetlen csomópont (kivéve, ha engedélyezve van a vgx.dll fürtre), amely a gyorsítótár-szolgáltatás kezelése régiók nem vonatkoznak. Vgx.dll támogatja a Keresés és a műveletek helyettesítő kulcsok beolvasása közben, így leíró címkék ágyazott kulcs nevét, és az elemek később beolvasásához használt. Egy példa végrehajtási címkézési megoldást vgx.dll használatával című témakörben [végrehajtási gyorsítótár címkézés a vgx.dll](http://stackify.com/implementing-cache-tagging-redis/).|
|Szerializálási|Felügyelt gyorsítótár NetDataContractSerializer BinaryFormatter és egyéni objektumszerializáló használatát támogatja. Az alapértelmezett érték NetDataContractSerializer.|Feladata az ügyfélalkalmazás .NET objektumok szerializálni úgy, hogy gyorsítótárba, a választható a szerializáló felfelé az ügyfél alkalmazás fejlesztője előtt. További információk és példakódot a [gyorsítótár .NET-objektumok használata](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)című rész tartalmaz.|
| Gyorsítótár irányító | Felügyelt gyorsítótár egy helyi gyorsítótár irányító biztosít. | Azure vgx.dll gyorsítótár-irányító nem rendelkezik, de a ahhoz, hogy egy irányító élmény [futtatása vgx.dll-server.exe helyileg MSOpenTech változatát](cache-faq.md#cache-emulator) is. |

## <a name="choose-a-cache-offering"></a>Válassza ki a gyorsítótár felületek

Microsoft Azure vgx.dll gyorsítótárat a következő rétegek érhető el:

-   **Egyszerű** – egyetlen csomópontot. Több 53 GB méretű.
-   **Szabványos** – két-csomópont elsődleges/replika. Több 53 GB méretű. 99,9 % SLA.
-   **Prémium verzió** – az elsődleges két-csomópont replika legfeljebb 10 shards a. 6 GB betűméretek 530 GB (kapcsolatfelvétel további). Minden, szabványos réteg funkció és [fürt vgx.dll](cache-how-to-premium-clustering.md) [Vgx.dll adatmegőrzési](cache-how-to-premium-persistence.md)és [Azure virtuális hálózati](cache-how-to-premium-vnet.md)további beleértve támogatása. 99,9 % SLA.

Minden réteg eltérő funkciói és árak értelmez. A szolgáltatások tartoznak, és árak további információt az útmutató a, a [Gyorsítótár árak részletei](https://azure.microsoft.com/pricing/details/cache/)című részben találja.

Áttelepítési kiindulási pont, hogy válassza ki a megfelelő méretet az előző gyorsítótár-szolgáltatás kezelése gyorsítótár méretét, majd méretezze felfelé vagy lefelé attól függően, hogy az alkalmazás követelményeinek. További kiválasztása a megfelelő Azure vgx.dll gyorsítótár ajánlja fel, olvassa el [milyen vgx.dll gyorsítótár felkínálása és a méret érdemes használnom?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Hozzon létre egy gyorsítótár

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>A gyorsítótár-ügyfelek konfigurálása

A gyorsítótár létrehozása és konfigurálása után a következő lépésben, hogy távolítsa el a felügyelt gyorsítótár-szolgáltatás beállításai, a Hozzáadás az Azure vgx.dll gyorsítótár konfiguráció hozzáadása, és, hogy a gyorsítótár-ügyfélprogramok hozzáférhetnek a gyorsítótár hivatkozik.

-   Távolítsa el a felügyelt gyorsítótár-szolgáltatás beállításai
-   A StackExchange.Redis NuGet csomag használata gyorsítótár ügyfél konfigurálása

### <a name="remove-the-managed-cache-service-configuration"></a>Távolítsa el a felügyelt gyorsítótár-szolgáltatás beállításai

Mielőtt a ügyfélalkalmazásokban Azure vgx.dll gyorsítótár, a meglévő gyorsítótár-szolgáltatás kezelése konfigurációt állítható be, és összeállítási hivatkozások is el kell távolítani a gyorsítótár-szolgáltatás NuGet felügyelt csomag eltávolítása.

A gyorsítótár-szolgáltatás NuGet felügyelt csomag eltávolításához kattintson a jobb gombbal a **Megoldást Explorer** ügyfél projekt, és válassza a **NuGet csomagok kezelése**. Jelölje ki a **telepítendő csomagok** csomópontot, és írja be a keresőmezőbe a telepített csomagok W**indowsAzure.Caching** . Jelölje be a **Windows** **Azure-gyorsítótár** (vagy **a Windows** **Azure gyorsítótárazás** attól függően, hogy a NuGet csomag verziójának), kattintson az **Eltávolítás**gombra, és kattintson a **Bezárás**gombra.

![Azure felügyelt gyorsítótár-szolgáltatás NuGet csomag eltávolítása](./media/cache-migrate-to-redis/IC757666.jpg)

A felügyelt gyorsítótár-szolgáltatás NuGet csomag eltávolítása a gyorsítótár-szolgáltatás kezelése szerelvények és a gyorsítótár-szolgáltatás kezelése a app.config vagy az ügyfélalkalmazás web.config. Néhány testre szabott beállításait nem lehet eltávolítani, amikor a NuGet csomag, mert web.config vagy app.config megnyitásához, és biztosítani, hogy a program teljesen eltávolítja az alábbi elemeket.

Győződjön meg arról, hogy a `dataCacheClients` bejegyzés törlődik a `configSections` elemet. Ne távolítsa el a teljes `configSections` elem; csak eltávolítása a `dataCacheClients` bejegyzést, ha a bemutató.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Győződjön meg arról, hogy a `dataCacheClients` szakasz törlődik. A `dataCacheClients` szakaszt a következő példa hasonló lesz.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Miután a gyorsítótár-szolgáltatás kezelése konfiguráció törlődik, beállíthatja a gyorsítótár-ügyfél, a következő szakaszban leírt módon.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>A StackExchange.Redis NuGet csomag használata gyorsítótár ügyfél beállítása

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Gyorsítótár-szolgáltatás felügyelt kódot áttelepítése

Az a StackExchange.Redis gyorsítótár ügyfélprogram API hasonlít a felügyelt gyorsítótár-szolgáltatás. Ez a témakör a különbségeket áttekintése.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Csatlakozás a gyorsítótárat a ConnectionMultiplexer osztály használatával

Felügyelt gyorsítótár-szolgáltatás, a gyorsítótár-kapcsolatot bonyolította a `DataCacheFactory` és `DataCache` osztályok. Azure vgx.dll gyorsítótár, ezek a kapcsolatok kezeli a `ConnectionMultiplexer` osztály.

Adja hozzá a következő utasítással bármely fájl, amelyből el szeretne érni a gyorsítótár tetején.

    using StackExchange.Redis
                                
Ha ez a névtér nem oldja meg, ügyeljen arra, hogy hozzáadta a StackExchange.Redis NuGet csomag [beállítása a gyorsítótár-ügyfelek](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)ismertetett módon.

>[AZURE.NOTE] Figyelje meg, hogy a StackExchange.Redis ügyfél .NET-keretrendszer 4-es vagy újabb szükséges-e.

Azure vgx.dll gyorsítótár-példány szeretne csatlakozni, hívja fel a statikus `ConnectionMultiplexer.Connect` módot és a fázis, a végpontok és billentyűt. Megosztási egyik megközelítés egy `ConnectionMultiplexer` példányt az alkalmazásban, hogy egy statikus tulajdonságot, amely visszaadja a csatlakoztatott példány, a következőhöz hasonló. Egyetlen kapcsolódó inicializálni szál vannak ezáltal `ConnectionMultiplexer` példány. Ebben a példában `abortConnect` értéke HAMIS, ami azt jelenti, hogy a hívás sikeres lesz, ha nem a gyorsítótár kapcsolatot létesíteni. Egy fontos szolgáltatása `ConnectionMultiplexer` , hogy csatlakozási automatikusan visszaállítja a gyorsítótár egyszer a hálózati probléma, vagy más okai megszűnt-e.

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

A gyorsítótár végpontot, kulcsok és portok szerezhető be a gyorsítótár-példány a **Gyorsítótár vgx.dll** lap. További tudnivalókért olvassa el a [gyorsítótár vgx.dll tulajdonságai](cache-configure.md#properties)című témakört.

Miután a kapcsolat létrejött, hivatkozását adja eredményül a gyorsítótár vgx.dll adatbázis hívja fel a `ConnectionMultiplexer.GetDatabase` módot. Az objektum által visszaadott a `GetDatabase` módszer könnyű átadó objektum, és nem kell tárolni.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

A StackExchange.Redis ügyfélprogram használja a `RedisKey` és `RedisValue` elérése és elem tárolása a gyorsítótár típusai. Ilyen leképezése leginkább egyszerű nyelvi típusokhoz, többek között a karakterláncot, és gyakran nem használatosak közvetlenül. Vgx.dll karakterláncok vgx.dll érték legalapvetőbb típusú, és számos különböző típusú adatokat, többek között a szerializált bináris adatfolyamok, tartalmazhatnak, és nem használhat a típus közvetlenül, miközben tartalmazó módszerek használni kívánt `String` nevét. Leginkább egyszerű adattípusokhoz tárolására és elemek beolvashatja a gyorsítótár használata a `StringSet` és `StringGet` módszerek, kivéve, ha tárolja gyűjtemények vagy más vgx.dll adattípusok gyorsítótár. 

`StringSet`és `StringGet` nagyban hasonlítanak ahhoz a felügyelt gyorsítótár-szolgáltatás `Put` és `Get` módszerek egy fő különbség, hogy beállítása és a .NET-objektum foglalkozni a gyorsítótár előtt meg kell szerializálni azt először. 

Amikor hívása `StringGet`, ha az objektum létezik, azt adja vissza, és ha igen, nem null ad vissza. Ebben az esetben az érték beolvasása a kívánt adatforrásból, tárolja azt a későbbi felhasználásra a gyorsítótár. A gyorsítótár-függetlenül minta nevezik.

A gyorsítótárban lévő elem lejártát megadásához használja a `TimeSpan` paramétere: `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure vgx.dll gyorsítótár .NET-objektumok, valamint az egyszerű adattípusok is dolgozhat, de a .NET-objektum gyorsítótárba helyezhető előtt meg kell használhatók, amelyekről. Ez az alkalmazás fejlesztője feladata. Ez a szerializáló választása a fejlesztői rugalmasság adja vissza. További információk és példakódot a [gyorsítótár .NET-objektumok használata](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)című rész tartalmaz.

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>ASP.NET-munkamenet állapot és a Weblapkimeneti gyorsítótárazás beállításával Azure vgx.dll gyorsítótárba áthelyezése

Azure vgx.dll gyorsítótár ASP.NET munkamenet-állapot és a lap Weblapkimeneti gyorsítótárazás szolgáltatók tartalmaz. Az alkalmazás, a fenti szolgáltatók gyorsítótár-szolgáltatás kezelése verzióit használó áttelepítéséhez először távolítsa el a meglévő szakaszokban a web.config, és adja meg a szolgáltatók Azure vgx.dll gyorsítótár verziója. Az Azure vgx.dll gyorsítótár ASP.NET-szolgáltatókkal, tanulmányozza [ASP.NET munkamenet állapot-szolgáltató Azure vgx.dll gyorsítótár](cache-aspnet-session-state-provider.md) és [ASP.NET kimeneti gyorsítótár-szolgáltató Azure vgx.dll gyorsítótár](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Következő lépések

Ismerkedjen meg a [Azure vgx.dll gyorsítótár dokumentáció](https://azure.microsoft.com/documentation/services/cache/) oktatóanyagok, minták, videók és sok másra.

