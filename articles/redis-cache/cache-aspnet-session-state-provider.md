<properties
    pageTitle="ASP.NET-munkamenet állapot gyorsítótár-szolgáltató |} Microsoft Azure"
    description="Megtudhatja, hogy miként tárolása ASP.NET munkamenet-állapot Azure vgx.dll gyorsítótár használata"
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
    ms.date="09/01/2016"
    ms.author="sdanie" />

# <a name="aspnet-session-state-provider-for-azure-redis-cache"></a>ASP.NET-munkamenet állapot-szolgáltató az Azure vgx.dll gyorsítótár

Azure vgx.dll gyorsítótár biztosít, amelyek a munkamenet-állapot tárolására, a memóriában nem pedig annak egy gyorsítótár vagy SQL Server-adatbázishoz, munkamenet állapot-szolgáltató. Szolgáltatót szeretne használni a gyorsítótár munkamenet állapot, először állítsa be a gyorsítótár, és adja meg a ASP.NET-alkalmazások használata a gyorsítótár munkamenet állam NuGet vgx.dll csomag gyorsítótárban.

Gyakran nem célszerű az alkalmazás a való életben felhőn bizonyos űrlap szerinti tárolásáról felhasználói munkamenet elkerülése érdekében, de néhány módszer a teljesítmény és méretezhetőség: több, mint a többi hatással. Ha állapot tárolási, a legjobb megoldás, állapot mennyiségét kis megtartása, és tárolja azt a cookie-k. Ha, amely nem lehetséges, a következő legjobb megoldás ASP.NET munkamenet-állapot egy szolgáltatót elosztott, a memóriában gyorsítótárhoz. A legrosszabb megoldást a teljesítmény és méretezhetőség szempontból, az adatbázis biztonsági a munkamenet-állapot szolgáltató. Ez a témakör útmutatást a ASP.NET-munkamenet állam szolgáltató Azure vgx.dll gyorsítótár használatáról. Információt más munkamenet állam [ASP.NET munkamenet-állapot beállítások](#aspnet-session-state-options)című cikkben olvashat.

## <a name="store-aspnet-session-state-in-the-cache"></a>ASP.NET-munkamenet állapot tárolni a gyorsítótárban

Visual Studio segítségével a gyorsítótár munkamenet állam NuGet vgx.dll csomag ügyfélalkalmazás beállításához kattintson a jobb gombbal a projekt a **Megoldást Intézőben** , és válassza a **NuGet csomagok kezelése**.

![Azure vgx.dll gyorsítótár NuGet csomagok kezelése](./media/cache-aspnet-session-state-provider/redis-cache-manage-nuget-menu.png)

**RedisSessionStateProvider** írja be a keresett szöveg mezőbe, válassza ki a közül, és kattintson a **telepítés**gombra.

>[AZURE.IMPORTANT] A prémium réteg a fürtképző funkció használatakor [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 kell használnia, vagy nagyobb vagy a kivétel elő. Ez a legfrissebb módosítások; További információt talál az [v2.0.0 megszakítása Részletek módosítása](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

![Azure vgx.dll gyorsítótár munkamenet-állapot-szolgáltató](./media/cache-aspnet-session-state-provider/redis-cache-session-state-provider.png)

A munkamenet állam szolgáltató NuGet vgx.dll csomagot függőség a StackExchange.Redis.StrongName csomagolásán látható termékszámmal. Ha a StackExchange.Redis.StrongName csomag nem szerepel a projekt települ. Ne feledje, hogy a erős nevű StackExchange.Redis.StrongName csomag mellett nincs is a StackExchange.Redis nem erős-névvel ellátott verziót. A projekt el kell távolítania, előtt vagy a munkamenet állam szolgáltató NuGet vgx.dll csomag telepítését követően nem erős-névvel ellátott StackExchange.Redis verzióját használja, ha egyébként meg fog első névütközések a projektben. Ezekről a csomagokról további tudnivalókért lásd: a [.NET konfigurálása gyorsítótár ügyfelek](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

A NuGet csomag tölthető le, és hozzáadja a szükséges összeállítás hivatkozik, és hozzáadja a következő felveszi a következő szakaszban az web.config fájlba, amely tartalmazza a szükséges konfigurációs a ASP.NET-alkalmazás Vgx.dll gyorsítótár munkamenet állam szolgáltatót szeretne használni.

    <sessionState mode="Custom" customProvider="MySessionStateStore">
        <providers>
        <!--
        <add name="MySessionStateStore"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            throwOnError = "true" [true|false]
            retryTimeoutInMilliseconds = "0" [number]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
        />
        -->
        <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
    </sessionState>

A megjegyzéssel ellátott rész az attribútumok és a minta beállításokat minden attribútum példát.

Az attribútumok a gyorsítótár fel a Microsoft Azure portál értékeivel rendelkező, valamint az egyéb értékek konfigurálni tetszés szerint. A gyorsítótár-tulajdonságok megnyitása, tanulmányozza [gyorsítótár beállításainak konfigurálása vgx.dll](cache-configure.md#configure-redis-cache-settings).

-   **a host** – adja meg a gyorsítótár végpontot.
-   **port** – az-SSL port vagy az SSL port, attól függően, hogy az ssl-beállításokat használja.
-   **accessKey** – az elsődleges és másodlagos kulcsa a gyorsítótár használni.
-   **az ssl** – igaz, ha a kívánt biztonságos gyorsítótár/ügyfél kommunikáció SSL; egyéb esetben hamis. Ügyeljen arra, hogy adja meg a megfelelő port.
    -   Az-SSL port új gyorsítótárát alapértelmezés szerint le van tiltva. Adja meg, igaz, az SSL portot használja ezt a beállítást. Az-SSL port engedélyezéséről további információt az [Access-portok](cache-configure.md#access-ports) a [konfigurálása a gyorsítótár](cache-configure.md) témakör című.
-   **throwOnError** – igaz, ha azt szeretné, hogy a kivétel elő hiba vagy hamis, ha azt szeretné, hogy a művelet értesítés nélkül sikertelen lesz. Érdemes a egy hiba, jelölje be a statikus Microsoft.Web.Redis.RedisSessionStateProvider.LastException tulajdonságot. Az alapértelmezett érték igaz.
-   **retryTimeoutInMilliseconds** – műveleteket, amelyek nem a megadott időintervallum ezredmásodpercben alatt vannak újból. Az első újrapróbálkozási 20 ezredmásodperc után következik be, és ezután újrapróbálkozások fordulhat elő, minden második mindaddig, amíg a retryTimeoutInMilliseconds időköz lejár. A művelet megismétlése közvetlenül azután, hogy az intervallum egy záró időpont. Ha még nem sikerül a művelet, a kivétel történt vissza a hívó attól függően, hogy a throwOnError beállítást. Az alapértelmezett értéke 0, ami azt jelenti, nem újrapróbálkozások.
-   **databaseId** – Itt adhatja meg, mely adatbázisról, hogy a gyorsítótár kimeneti adatok. Ha nincs megadva, az alapértelmezett érték 0 használják.
-   **applicationName** – billentyűparancsok tárolja, vgx.dll `{<Application Name>_<Session ID>}_Data`. Több alkalmazások megosztani az ugyanazt a kulcsot lehetővé teszi. Ez a paraméter megadása nem kötelező, és nem ad meg, ha egy alapértelmezett értéket használja.
-   **connectionTimeoutInMilliseconds** – ezzel a beállítással, hogy felülírja az StackExchange.Redis ügyfél connectTimeout beállítása. Ha nincs megadva, az alapértelmezett connectTimeout 5000 használják. További tudnivalókért olvassa el a [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705)című témakört.
-   **operationTimeoutInMilliseconds** – ezzel a beállítással, hogy felülírja az StackExchange.Redis ügyfél syncTimeout beállítása. Ha nincs megadva, az alapértelmezett syncTimeout 1000 használják. További tudnivalókért olvassa el a [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705)című témakört.

Tulajdonságokból kapcsolatos további tudnivalókért olvassa el a szolgáltatónál [kihirdetése ASP.NET munkamenet állam vgx.dll az](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)eredeti blog bejegyzés bejelentése című témakört.

Ne felejtse el a szokásos InProc munkamenet state szolgáltató szakasz a Web.config fűzni.

    <!-- <sessionState mode="InProc"
         customProvider="DefaultSessionProvider">
         <providers>
            <add name="DefaultSessionProvider"
                  type="System.Web.Providers.DefaultSessionStateProvider,
                        System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                        PublicKeyToken=31bf3856ad364e35"
                  connectionStringName="DefaultConnection" />
          </providers>
    </sessionState> -->

Ezeket a lépéseket kell végrehajtani, amikor az alkalmazás be van állítva munkamenet vgx.dll gyorsítótár állam szolgáltatót szeretne használni. Az alkalmazás használatakor a munkamenet-állapot tárolja az Azure vgx.dll gyorsítótár-példány történik.

>[AZURE.NOTE] Figyelje meg, hogy a gyorsítótárban tárolt adatokat szerializálható kell lennie, az alapértelmezett tárolt adatok eltérően a memóriában ASP.NET munkamenet megye szolgáltató. A munkamenet-állapot Konferenciaszolgáltatóját vgx.dll használatakor kell arról, hogy a munkamenet állapotban tárolt adattípusok szerializálható.

## <a name="aspnet-session-state-options"></a>ASP.NET-munkamenet állapot beállításai

- A memória munkamenet-állapot szolgáltató – a szolgáltató a munkamenet-állapot a memóriában tárolja. Az e-szolgáltató használata előnye gyorsan és egyszerűen. Azonban meg nem méretezze át a Web Apps alkalmazások használata a memória szolgáltató óta nem van meghatározva.

- Az SQL Server munkamenet-állapot szolgáltató - szolgáltató Sql Server tárolja a munkamenet-állapot. Ez a szolgáltató kell használnia, ha a probléma továbbra is fennáll a munkamenet-állapot állandó-tárolóban lévő szeretne. A Web App méretezheti, de az Sql Server használatának munkamenet lesz a teljesítményre gyakorolt hatás a Web App.

- Elosztott memória munkamenet állam szolgáltatótól vgx.dll gyorsítótár munkamenet állam szolgáltató – Ez a szolgáltató például mind a világon a legjobb biztosít. A Web App beállíthatja, hogy egy egyszerű, gyors és méretezhető munkamenet állam szolgáltatóval. De ha érdemes figyelembe, hogy a szolgáltató gyorsítótárban tárolja a munkamenet-állapot és az alkalmazás kell vesz figyelembe beszélgetésben elosztott a memória-gyorsítótár tranziens hálózati hibák például amikor kapcsolódó jellemzőiket befolyásolják. Ajánlott eljárások a gyorsítótár használatáról a [gyorsítótár útmutató](../best-practices-caching.md) a Microsoft Patterns és eljárások [Azure felhő alkalmazás tervezése és végrehajtása útmutatást](https://github.com/mspnp/azure-guidance)című témakör tartalmaz.

További információt a munkamenet és az egyéb ajánlott eljárások a [Webes fejlesztési gyakorlati tanácsok (épület valós Életből felhő alkalmazások az Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices)című.

## <a name="next-steps"></a>Következő lépések

Nézze meg az [ASP.NET kimeneti gyorsítótár-szolgáltató Azure vgx.dll gyorsítótár](cache-aspnet-output-cache-provider.md).
