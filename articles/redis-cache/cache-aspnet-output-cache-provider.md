<properties
    pageTitle="Gyorsítótár ASP.NET kimeneti gyorsítótár-szolgáltató"
    description="Megtudhatja, hogy miként gyorsítótár ASP.NET lap kimeneti Azure vgx.dll gyorsítótár használata"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.NET kimeneti gyorsítótár-szolgáltató az Azure vgx.dll gyorsítótár

A vgx.dll Weblapkimeneti gyorsítótár szolgáltató egy folyamaton-tároló mechanizmusa weblapkimeneti gyorsítótár adatokhoz. Kifejezetten a teljes HTTP-válaszok el az adatok (weblapkimeneti gyorsítótárazás beállításával oldal). A szolgáltató be az új weblapkimeneti gyorsítótár szolgáltató bővítési pont megjelent ASP.NET-4-es csatlakozik.

A Weblapkimeneti gyorsítótár vgx.dll szolgáltató, először állítsa be a gyorsítótár, és adja meg a ASP.NET-alkalmazások használata a Weblapkimeneti gyorsítótár szolgáltató NuGet vgx.dll csomag. Ez a témakör útmutatást a Weblapkimeneti gyorsítótár vgx.dll szolgáltatót szeretne használni az alkalmazás beállítása. További információt a létrehozása és konfigurálása az Azure vgx.dll gyorsítótár-példány a [gyorsítótár létrehozása](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)című témakör tartalmaz.

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET lap kimeneti tárolni a gyorsítótárban

Visual Studio segítségével a Weblapkimeneti gyorsítótár szolgáltató NuGet vgx.dll csomag ügyfélalkalmazás beállításához kattintson a jobb gombbal a projekt a **Megoldást Intézőben** , és válassza a **NuGet csomagok kezelése**.

![Azure vgx.dll gyorsítótár NuGet csomagok kezelése](./media/cache-aspnet-output-cache-provider/redis-cache-manage-nuget-menu.png)

**RedisOutputCacheProvider** írja be a keresett szöveg mezőbe, válassza ki a közül, és kattintson a **telepítés**gombra.

![Azure vgx.dll gyorsítótár Weblapkimeneti gyorsítótár szolgáltató](./media/cache-aspnet-output-cache-provider/redis-cache-page-output-provider.png)

A Weblapkimeneti gyorsítótár szolgáltató NuGet vgx.dll csomagot függőség a StackExchange.Redis.StrongName csomagolásán látható termékszámmal. Ha a StackExchange.Redis.StrongName csomag nem szerepel a projekt települ. Ne feledje, hogy a erős nevű StackExchange.Redis.StrongName csomag mellett nincs is a StackExchange.Redis nem erős-névvel ellátott verziót. A projekt el kell távolítania, előtt vagy a Weblapkimeneti gyorsítótár szolgáltató NuGet vgx.dll csomag telepítését követően nem erős-névvel ellátott StackExchange.Redis verzióját használja, ha egyébként meg fog első névütközések a projektben. Ezekről a csomagokról további tudnivalókért lásd: a [.NET konfigurálása gyorsítótár ügyfelek](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

A NuGet csomag tölthető le, és a szükséges összeállítás hivatkozások hozzáadása, és hozzáadja a következő szakaszban az web.config fájlba, amely tartalmazza a szükséges konfigurációs Weblapkimeneti gyorsítótár vgx.dll szolgáltatót szeretne használni a ASP.NET-alkalmazáshoz.

    <caching>
      <outputCachedefault Provider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache"
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider" host="127.0.0.1" accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

A megjegyzéssel ellátott rész az attribútumok és a minta beállításokat minden attribútum példát.

Az attribútumok a gyorsítótár fel a Microsoft Azure portál értékeivel rendelkező, valamint az egyéb értékek konfigurálni tetszés szerint. A gyorsítótár-tulajdonságok megnyitása, tanulmányozza [gyorsítótár beállításainak konfigurálása vgx.dll](cache-configure.md#configure-redis-cache-settings).

-   **a host** – adja meg a gyorsítótár végpontot.
-   **port** – az-SSL port vagy az SSL port, attól függően, hogy az ssl-beállításokat használja.
-   **accessKey** – az elsődleges és másodlagos kulcsa a gyorsítótár használni.
-   **az ssl** – igaz, ha a kívánt biztonságos gyorsítótár/ügyfél kommunikáció SSL; egyéb esetben hamis. Ügyeljen arra, hogy adja meg a megfelelő port.
    -   Az-SSL port új gyorsítótárát alapértelmezés szerint le van tiltva. Adja meg, igaz, az SSL portot használja ezt a beállítást. Az-SSL port engedélyezéséről további információt az [Access-portok](cache-configure.md#access-ports) a [konfigurálása a gyorsítótár](cache-configure.md) témakör című.
-   **databaseId** – gyorsítótár kimeneti adatokhoz használandó adatbázis megadva. Ha nincs megadva, az alapértelmezett érték 0 használják.
-   **applicationName** – billentyűparancsok tárolja, vgx.dll <AppName>_<SessionId>_Data. Több alkalmazások megosztani az ugyanazt a kulcsot lehetővé teszi. Ez a paraméter megadása nem kötelező, és nem ad meg, ha egy alapértelmezett értéket használja.
-   **connectionTimeoutInMilliseconds** – ezzel a beállítással, hogy felülírja az StackExchange.Redis ügyfél connectTimeout beállítása. Ha nincs megadva, az alapértelmezett connectTimeout 5000 használják. További tudnivalókért olvassa el a [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705)című témakört.
-   **operationTimeoutInMilliseconds** – ezzel a beállítással, hogy felülírja az StackExchange.Redis ügyfél syncTimeout beállítása. Ha nincs megadva, az alapértelmezett syncTimeout 1000 használják. További tudnivalókért olvassa el a [StackExchange.Redis konfigurációs modell](http://go.microsoft.com/fwlink/?LinkId=398705)című témakört.

Minden lapra, amelyhez hozzá kívánja a weblapkimeneti gyorsítótár-OutputCache irányelv hozzáadása

    <%@ OutputCache Duration="60" VaryByParam="*" %>

Ebben a példában a gyorsítótárban tárolt lap adatok marad a gyorsítótárban lévő 60 másodpercig, és mindegyik paraméter kombináció gyorsítótárazott az oldal egy másik verzióját. Az OutputCache irányelv kapcsolatos további tudnivalókért lásd: [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837).

Ezeket a lépéseket kell végrehajtani, amikor az alkalmazás Weblapkimeneti gyorsítótár vgx.dll szolgáltató van konfigurálva.

## <a name="next-steps"></a>Következő lépések

Nézze meg az [ASP.NET munkamenet állapot-szolgáltató Azure vgx.dll gyorsítótár](cache-aspnet-session-state-provider.md).
