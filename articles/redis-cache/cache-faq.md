<properties 
    pageTitle="Azure vgx.dll gyorsítótár gyakran ismételt kérdések |} Microsoft Azure" 
    description="Ismerje meg, a válaszok a gyakori kérdésekre, a mintázatok és a gyakorlati tanácsok az Azure vgx.dll gyorsítótár" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Azure vgx.dll gyorsítótár – gyakori kérdések

A válaszok a gyakori kérdésekre, a mintázatok és a gyakorlati tanácsok az Azure vgx.dll gyorsítótár című témakörben. 


## <a name="what-if-my-question-isnt-answered-here"></a>Mi történik, ha szeretnék nem érkezett válasz Itt?

Ha kérdésére nem találja meg itt, tudassa velünk, és segítünk talál választ.

-   Tegyen közzé kérdést a [Disqus szál](#comments) Ez a cikk végén, és az Azure-gyorsítótár csoport és más közösségi tagok erről a cikkről vesznek.
-   Egy nem elég széles közönség eléréséhez tegye közzé kérdését az [Azure gyorsítótár fórum az MSDN webhelyen](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) , és az Azure-gyorsítótár csoport és a Közösség más tagjai vesznek.
-   Ha azt szeretné, hogy a szolgáltatás kérése, a kérések és [Azure vgx.dll gyorsítótár felhasználói hangposta](https://feedback.azure.com/forums/169382-cache)ötletek beküldheti.
-   Az [Azure-gyorsítótár külső visszajelzést](mailto:azurecache@microsoft.com)szeretne velünk is küldhet e-mailben.

## <a name="azure-redis-cache-basics"></a>Azure vgx.dll gyorsítótár alapjai

A gyakori kérdések ebben a részben néhány alapvető Azure vgx.dll gyorsítótár terjed ki.

-    [Mi az Azure vgx.dll gyorsítótár?](#what-is-azure-redis-cache)
-    [Hogyan tudom elkezdeni használni az Azure vgx.dll gyorsítótár?](#how-can-i-get-started-with-azure-redis-cache)

A következő gyakori kérdések Alapfogalmak és Azure vgx.dll gyorsítótár kapcsolatos kérdések terjed ki, majd és válaszok a gyakori kérdések szakaszt egy.

-   [Milyen vgx.dll gyorsítótár felkínálása és a méret érdemes használnom?](#what-redis-cache-offering-and-size-should-i-use)
-   [Milyen vgx.dll gyorsítótár ügyfeleket is használni?](#what-redis-cache-clients-can-i-use)
-   [Van-e egy helyi irányító Azure vgx.dll gyorsítótár?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Hogyan figyelni a rendszerállapot és a teljesítmény a gyorsítótár?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Gyakori kérdések tervezése

-   [Milyen vgx.dll gyorsítótár felkínálása és a méret érdemes használnom?](#what-redis-cache-offering-and-size-should-i-use)
-   [Azure vgx.dll gyorsítótár teljesítmény](#azure-redis-cache-performance)
-   [Milyen régióban lehet, keresse meg a gyorsítótár?](#in-what-region-should-i-locate-my-cache)
-   [Hogyan vagyok számlát kapni az Azure vgx.dll gyorsítótár?](#how-am-i-billed-for-azure-redis-cache)
-   [Használhatom-e Azure vgx.dll gyorsítótár Azure Government Cloud vagy Azure Kína felhő?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Fejlesztési gyakori kérdések

-   [Mire végezze el a StackExchange.Redis konfigurációs beállítások?](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Milyen vgx.dll gyorsítótár ügyfeleket is használni?](#what-redis-cache-clients-can-i-use)
-   [Van-e egy helyi irányító Azure vgx.dll gyorsítótár?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Hogyan lehet vgx.dll parancsai futtathatók?](#how-can-i-run-redis-commands)
-   [Miért nem tartozik egy MSDN osztály tár hivatkozást, például egyes Azure szolgáltatások Azure vgx.dll gyorsítótár?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Használhatom-e Azure vgx.dll gyorsítótár PHP munkamenet gyorsítótárat szerint?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Biztonsággal kapcsolatos gyakori kérdések

-   [Mikor célszerű engedélyezni a-SSL port vgx.dll való csatlakozáshoz?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Gyártási gyakori kérdések

-   [Mik azok a gyártási gyakorlati tanácsokat?](#what-are-some-production-best-practices)
-   [Melyek a megfontolások közös vgx.dll parancsok használata esetén?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Hogyan lehet teljesítményteszt és tesztelje a gyorsítótár teljesítményét?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Fontos tudnivalókat a szálkészlet növekedés](#important-details-about-threadpool-growth)
-   [Kiszolgáló globális Katalógus lehető további átviteli az ügyfél StackExchange.Redis engedélyezése](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Figyelés és gyakori kérdések – hibaelhárítás

Ebben a szakaszban a gyakori kérdések a közös figyelése és kérdések hibaelhárítás terjed ki. Figyelése és hibaelhárítás az Azure vgx.dll gyorsítótár-példányok kapcsolatos további tudnivalókért olvassa el a [figyelése Azure vgx.dll gyorsítótár](cache-how-to-monitor.md) és [Azure vgx.dll gyorsítótár hibáinak elhárítása](cache-how-to-troubleshoot.md)című témakört.

-   [Hogyan figyelni a rendszerállapot és a teljesítmény a gyorsítótár?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [A gyorsítótár diagnosztika tároló Fiókbeállítások megváltozott, hová tűnt?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Miért diagnosztika engedélyezve van néhány új gyorsítótárát, de nem az összes?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Miért jelennek meg időtúllépései?](#why-am-i-seeing-timeouts)
-   [Miért megszakadt az ügyfél, a gyorsítótárból?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Előzetes gyorsítótár felületek gyakori kérdések

-   [Melyik Azure gyorsítótár ajánlja fel a megfelelő számomra?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Mi az Azure vgx.dll gyorsítótár?

Azure vgx.dll gyorsítótár [gyorsítótár vgx.dll](http://redis.io)népszerű Megnyitás forrás alapul. Azt férhet hozzá egy biztonságos, célorientált vgx.dll gyorsítótár, a Microsoft által felügyelt és bármely alkalmazásban belül Azure-ból elérheti. A részletes ismertetése című témakörben található az [Azure vgx.dll gyorsítótár](https://azure.microsoft.com/services/cache/) -termék lapon a Azure.com.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Hogyan tudom elkezdeni használni az Azure vgx.dll gyorsítótár?

Többféle módon az Azure vgx.dll gyorsítótár használatának megkezdéséhez.

-    Érdemes egyet a rendelkezésre álló oktatóanyagok [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md)és [Python](cache-python-get-started.md). 
-    [Nagy teljesítmény alkalmazások használata a Microsoft Azure vgx.dll gyorsítótár készítése hogyan](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/)nézze meg.
-    Ügyfél dokumentációjában találhat az ügyfelek, amelyek megegyeznek a projekt megtudhatja, hogy miként vgx.dll használandó fejlesztési nyelvének érdemes. Azure vgx.dll gyorsítótár használható sok vgx.dll ügyfelek is vannak. Ügyfelek vgx.dll listáját olvassa el a [http://redis.io/clients](http://redis.io/clients)című témakört.


Ha még nincs Azure-fiók, a következőkre van lehetősége:

-    [Nyissa meg az ingyenes Azure-fiók](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Próbálja ki az Azure szolgáltatások fizetett használható jóváírások kap. Után a credits használnak, megtarthatja a fiókot, és ingyenes Azure szolgáltatások és funkciók.
-    [Visual Studio aktiválása előfizetői előnyeit](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Az MSDN-előfizetés lépve credits havonta fizetett Azure szolgáltatások használható.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Milyen vgx.dll gyorsítótár felkínálása és a méret érdemes használnom?
Minden egyes Azure vgx.dll gyorsítótár **méretét**, **sávszélesség**, **magas rendelkezésre állásának**és **SZOLGÁLTATÁSISZINT** -beállítások különböző mértékű kínál.

Az alábbiakban megfontolandó szempontok kiválasztása a gyorsítótár egyesíti.

-   **Memória**: az alapszintű és a szokásos rétegek kínálnak 250 MB-os – 53 GB. A prémium réteg nyújt további rendelkezésre álló [kérésre](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)530 GB. További tudnivalókért olvassa el a [Azure vgx.dll gyorsítótár árak](https://azure.microsoft.com/pricing/details/cache/)című témakört.
-   **A hálózati teljesítmény**: Ha van egy nagy átviteli igénylő terhelést a prémium réteg kínál normál vagy Basic összehasonlítani több sávszélesség. Minden réteg belül (GB) nagyobb méretű gyorsítótárát is több sávszélesség a mögöttes virtuális a gyorsítótár üzemeltető miatt. Tanulmányozza az [alábbi táblázatban](#cache-performance) további információt.
-   **Átviteli**: A prémium réteg felajánlja a rendelkezésre álló maximális sebesség. Ha a gyorsítótár-kiszolgáló és az ügyfél eléri a sávszélesség korlátozások, ügyféloldali időtúllépése kap. További tudnivalókért lásd: az alábbi táblázatban.
-   **Magas elérhetősége/SLA**: Azure vgx.dll gyorsítótár biztosítja, hogy egy Standard/prémium gyorsítótár elérhető lesz legalább az idő 99,9 %. Többet szeretne tudni a SLA, lásd: [Azure vgx.dll gyorsítótár árak](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). A SLA csak kapcsolata a gyorsítótár végpontok terjed ki. Adatvesztés elleni védelem az SLA nem foglalkozik. Azt javasoljuk, hogy funkcióval vgx.dll adatok megőrzése a prémium réteg adatvesztés tűrőképessége növeléséhez.
-   **Adatok állandó vgx.dll**: A prémium réteg lehetővé teszi az Azure tároló fiók gyorsítótár adatok megmaradnak. Basic/Standard gyorsítótárat az összes adatot tároló csak a memóriában. Abban az esetben, ha mögöttes infrastruktúra problémák vannak adatvesztést is lehet. Azt javasoljuk, hogy funkcióval vgx.dll adatok megőrzése a prémium réteg adatvesztés tűrőképessége növeléséhez. Azure vgx.dll gyorsítótár Rekordadatbázis és (hamarosan) AOF lehetőséget kínál a vgx.dll adatmegőrzési. További információért megtudhatja, [hogy miként adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-persistence.md).
-   **Vgx.dll fürt**: létrehozása gyorsítótárát 53 GB-nál nagyobb vagy shard adatokhoz több vgx.dll csomópontok keresztül, a fürtözéssel vgx.dll, amely érhető el a támogatási réteg. Az egyes csomópontok magas elérhetőség elsődleges/replika gyorsítótár két áll. További információért megtudhatja, [hogy miként fürtözőszoftverét prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-clustering.md).
-   **Fokozott biztonság és a hálózati elkülönítési**: Azure virtuális hálózati (VNET) telepítési fokozott biztonság és elkülönítési az Azure vgx.dll gyorsítótár, valamint a alhálózat, hozzáférési szabályok, az itt és más funkciók további korlátozhatja a hozzáférést. További információért megtudhatja, [hogy miként virtuális hálózat támogatása prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-vnet.md).
-   **Vgx.dll beállítása**: a Standard és a prémium rétegek adhatja vgx.dll Keyspace értesítéseket.
-   **Az ügyfélkapcsolatok maximális száma**: A prémium réteg kínál az ügyfelek csatlakozhat, és egy nagyobb méretű gyorsítótárát kapcsolatok magasabb vgx.dll maximális számát. [, Kérjük, olvassa el a részleteket árak lapra](https://azure.microsoft.com/pricing/details/cache/).
-   **Saját Core vgx.dll kiszolgáló**: a prémium réteg összes gyorsítótár méretét telepítve van egy dedikált core vgx.dll. A Basic Standard rétegek a C1 méretezéséhez és fent még egy dedikált core vgx.dll kiszolgáló.
-   **Egy téma szerint rendezett vgx.dll** így több mint két grafikon nem nyújt további előnye csak két grafikon problémákat fölé, de nagyobb virtuális méretek általában-nél kisebb méretű további sávszélességgel rendelkezik-e. Ha a gyorsítótár-kiszolgáló és az ügyfél eléri a sávszélesség korlátozások, majd kap ügyféloldali időtúllépése.
-   **Teljesítménybeli fejlesztések**: a prémium réteg gyorsítótárát hardveren, amelynek gyorsabb processzor van telepítve, és ad a legjobb teljesítmény és a Basic vagy normál réteg összehasonlítása. Prémium réteg gyorsítótárát nagyobb teljesítmény és az alsó késések van.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Azure vgx.dll gyorsítótár teljesítmény

A következő táblázat mutatja a maximális sávszélesség értékek standard különböző méretű tesztelése közben megfigyelt és prémium gyorsítótárát használatával `redis-benchmark.exe` a egy Iaas virtuális szemben az Azure vgx.dll gyorsítótár végpontot. Ne feledje, hogy ezeket az értékeket nem garantált, és nincs-e SZOLGÁLTATÁSISZINT számok, de elvileg tipikus van. Be kell tölteni a saját alkalmazás határozza meg a megfelelő gyorsítótár méretét az alkalmazás tesztelése.

Ebből a táblából az alábbi következtetéseket is elvégezheti.

-   Átviteli a gyorsítótárát, amelyek azonos méretű az értéke nagyobb, mint a szokásos réteg a prémium réteg. Például egy 6 GB-gyorsítótár P1 átviteli 140K RPS képest 49 K C3.
-   A vgx.dll fürtözés átviteli növekszik lineárisan shards (csomópontok) a fürt számának növelése. Például ha 10 shards P4 fürt hoz létre, majd a rendelkezésre álló átviteli is 250K * 10 = 2,5 millió RPS.
-   Nagyobb kulcs méretű átviteli értéke nagyobb, mint a szokásos réteg a prémium réteg.

| Réteg árak             | Mérete   | Processzormagok | Rendelkezésre álló sávszélesség                                    | 1 KB kulcs mérete                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Szabványos gyorsítótár méretét.** |        |           | **MB / sec (Mb/s) / megabájt (MB/s) másodpercenként** | **Kérelmek egy második (RPS)**            |
| C0                       | 250 MB | A megosztott    | 5 / 0.625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| A C2                       | 2,5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **Prémium gyorsítótár méretét**  |        | **Egy shard Processzormagok**  |                                         | **Kérelmek egy második (RPS), a használati shard** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000-ig terjednek                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Útmutatást a vgx.dll eszközök például letöltése `redis-benchmark.exe`, lásd: a [hogyan is futtatása vgx.dll parancsok?](#cache-commands) szakaszban.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>Milyen régióban lehet, keresse meg a gyorsítótár?

A legjobb teljesítmény elérése érdekében és a legalacsonyabb késés keresse meg a gyorsítótár ügyfélalkalmazás azonos régió az Azure vgx.dll gyorsítótár.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Hogyan vagyok számlát kapni az Azure vgx.dll gyorsítótár?

Az Azure vgx.dll gyorsítótár árak [Itt](https://azure.microsoft.com/pricing/details/cache/). A árak lapon találhatók, mint egy óradíj értéket árak. Gyorsítótárát a rendszer a időpontot, amely a gyorsítótár jön létre, amely a gyorsítótár törlődik a időpontig perc alapon számlát kapni. Nincs leállítása vagy felfüggesztése a számlázási a gyorsítótár lehetőség.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Használhatom-e Azure vgx.dll gyorsítótár Azure Government Cloud vagy Azure Kína felhő?

Igen, Azure vgx.dll gyorsítótár érhető el az Azure kormányzati felhő és az Azure Kína felhő. Ne feledje, hogy az URL-címeit, elérésére és Azure vgx.dll gyorsítótár kezelése eltérő Azure kormányzati felhőben, és Azure Kína felhő Azure nyilvános felhő képest. Azure vgx.dll gyorsítótár használata az Azure kormányzati felhő és Azure Kína felhő szempontjai kapcsolatos további tudnivalókért olvassa el a [Kormányzati Azure - adatbázisok Azure vgx.dll gyorsítótár](../azure-government/documentation-government-services-database.md#azure-redis-cache) és [Azure Kína Cloud - Azure vgx.dll gyorsítótár](https://www.azure.cn/documentation/services/redis-cache/)című témakört.

Információ PowerShell az Azure kormányzati felhő és Azure Kína felhőben való használatáról a Azure vgx.dll gyorsítótár elolvashatja, [hogyan Azure Government Cloud vagy Azure Kína felhőben való csatlakozásra](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Mire végezze el a StackExchange.Redis konfigurációs beállítások?

StackExchange.Redis tartalmaz számos beállítást. Ebben a szakaszban olvashat néhány gyakori beállításainak folytatott beszélgetést. StackExchange.Redis beállításokkal kapcsolatos további információért olvassa el a [StackExchange.Redis konfigurálása](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md)című témakört.

ConfigurationOptions|Leírás|Javaslat
---|---|---
AbortOnConnectFail|IGAZ, a kapcsolat esetén nem újracsatlakozik egy hálózati hiba után.|Hamis értékre van állítva, és mondja el StackExchange.Redis automatikusan újra.
ConnectRetry|Csatlakozás kapcsolatfelvételi során kezdeti ismétlődésének száma.| Az alábbi útmutatókat talál. |
ConnectTimeout|Az ms-időtúllépése műveletek csatlakozni.| Az alábbi útmutatókat talál. |

Az alapértelmezett értékeket az ügyfél legtöbbször megfelelőek. A beállítások, a terhelést alapján finomhangolására.

-   **Újrapróbálkozások**
    -   ConnectRetry és az általános útmutatás a meghiúsító ConnectTimeout gyors, majd próbálja újra. Ez a terhelést, és hogy mennyi idő átlagos azt veszi fel az ügyfél hiba vgx.dll parancsot, és arra választ kap alapul.
    -   Tudassa az automatikusan újra helyett a kapcsolati állapot ellenőrzése és a saját maga újracsatlakozás StackExchange.Redis. **Ne a ConnectionMultiplexer.IsConnected tulajdonát felhasználni**.
    -   Snowballing - néha, előfordulhat, hogy belefutna egy problémába, ahol van újraküldése, és ez snowballs, és soha nem helyreállítása. Ebben az esetben vegye figyelembe a Microsoft Patterns és eljárások csoport által közzétett exponenciális visszalépési újrapróbálkozási algoritmus használatával, [próbálja meg újra az általános útmutató](best-practices-retry-general.md) leírt módon.
-   **Időtúllépési értékek**
    -   Fontolja meg a terhelést, és ennek megfelelően beállítani az értékeket. Ha nagy értékeket tárolja, állítsa az időkorlát nagyobb értéket.
    -   Beállítása `AbortOnConnectFail` hamis és küldő StackExchange.Redis meg újra.
    -   Egyetlen ConnectionMultiplexer példány használni az alkalmazást. Egy LazyConnection segítségével egy példányát a kapcsolat tulajdonság, által visszaküldött létrehozása, [Csatlakozás a gyorsítótárat a ConnectionMultiplexer osztály használatával](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)látható módon.
    -   Állítsa a `ConnectionMultiplexer.ClientName` -példány egyedi alkalmazásnév diagnosztikai célokra tulajdonságot.
    -   Használjon többszörös `ConnectionMultiplexer` egyéni munkaterhelésekből-példányok.
    -   Kövesse a modell az alkalmazásban többféle betöltés esetén. Példa:
    -   Egy nagy kulcsok kezelésének multiplexer is.
    -   Egy kis kulcsok kezelésének multiplexer is.
    -   Kapcsolat időtúllépései különböző értékének beállítása és a logika újra az egyes ConnectionMultiplexer használt.
    -   Állítsa a `ClientName` tulajdonság minden multiplexer diagnosztika segítséget.
    -   Ez a Továbbiak vezet egyszerűsödött, késés / `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Milyen vgx.dll gyorsítótár ügyfeleket is használni?

Az információ vgx.dll remek dolgot egyik, hogy nincsenek-e sok ügyfél számos különböző fejlesztési nyelvek támogatására. Ügyfelek aktuális listáját olvassa el a [ügyfelek vgx.dll](http://redis.io/clients)című témakört. Megtudhatja, [hogy miként Azure vgx.dll gyorsítótár használata](cache-dotnet-how-to-use-azure-redis-cache.md) , amely több különböző nyelvű és ügyfelek terjed ki oktatóanyagok, és kattintson a kívánt nyelvet a nyelv kapcsoló tetején látható a cikk a.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Van-e egy helyi irányító Azure vgx.dll gyorsítótár?

Nincs helyi irányító Azure vgx.dll gyorsítótár nem, de lebonyolítása vgx.dll server.exe MSOpenTech verziójának [Vgx.dll parancssori eszközök](https://github.com/MSOpenTech/redis/releases/) a helyi számítógépen, és csatlakozhat hozzá egy hasonló élmény eléréséhez kattintson a helyi gyorsítótár irányító vagy az alábbi példában látható módon.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Igény szerint állítsa be egy [redis.conf](http://redis.io/topics/config) fájlt, hogy jobban megfeleljen az [alapértelmezett gyorsítótár beállításai](cache-configure.md#default-redis-server-configuration) a online Azure vgx.dll gyorsítótár, ha szükségesnek látja.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Hogyan lehet vgx.dll parancsai futtathatók?

Bármelyik [parancsok vgx.dll](http://redis.io/commands#) webhelyén állítsa be a felsorolt [Vgx.dll parancsok az Azure vgx.dll gyorsítótár nem támogatja](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache)a parancsok kivételével parancsot használhatja. Parancsok vgx.dll futtatásához számos lehetőség közül választhat.

-   Ha a normál vagy prémium gyorsítótár, [Vgx.dll konzol](cache-configure.md#redis-console)használata vgx.dll parancsok futtathatók. Ez a vgx.dll parancsai futtathatók az Azure-portálon biztonságos lehetőséget nyújt.
-   A parancssor vgx.dll eszközöket is használhatja. Használja őket, hajtsa végre az alábbi lépéseket.
-   Töltse le a [parancssor eszközök vgx.dll](https://github.com/MSOpenTech/redis/releases/).
-   Kapcsolódás a gyorsítótár `redis-cli.exe`. A sikeres a gyorsítótár végpont használatával válthat a -h és a billentyűvel beírt karaktert a - a következő példában látható módon.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Megjegyzés: a vgx.dll parancssori eszközök nem működnek az SSL port, de az használhatja például egy segédprogramot `stunnel` hogy biztonságosan csatlakoztathassa az eszközöket az SSL port [Kihirdetése ASP.NET munkamenet állapot-szolgáltató az előnézeti Release vgx.dll](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbejegyzés utasításait követve.

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Miért nem tartozik egy MSDN osztály tár hivatkozást, például egyes Azure szolgáltatások Azure vgx.dll gyorsítótár?

Microsoft Azure vgx.dll gyorsítótár a népszerű Megnyitás vgx.dll gyorsítótár alapul, és [ügyfelek vgx.dll](http://redis.io/clients) számos programozási nyelven érhetők el, amely számos különböző érhetik el. Minden egyes ügyfél rendelkezik-e saját API-val, amely lehetővé teszi a hívások átirányítása a vgx.dll gyorsítótár-példány [Vgx.dll parancsok](http://redis.io/commands)használatával.

Mivel minden ügyfél más, nem egyetlen központi osztály hivatkozást van; az MSDN webhelyen egyes ügyfelek helyett a saját hivatkozási dokumentáció kezeli. A hivatkozás dokumentációt kívül több oktatóanyagok ábrázoló Azure vgx.dll gyorsítótár nyelvek és a gyorsítótár-ügyfelek használata – első lépések Oktatóanyagokban eléréséhez megtudhatja, [hogy miként Azure vgx.dll gyorsítótár használata](cache-dotnet-how-to-use-azure-redis-cache.md) , és kattintson a kívánt nyelvet a nyelv kapcsoló a témakör tetején a.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Használhatom-e Azure vgx.dll gyorsítótár PHP munkamenet gyorsítótárat szerint?

Igen, egy PHP-munkamenet gyorsítótár Azure vgx.dll gyorsítótár használni, adja meg a kapcsolati karakterláncot az Azure vgx.dll gyorsítótár-példány a `session.save_path`.

>[AZURE.IMPORTANT] Azure vgx.dll gyorsítótár PHP munkamenet gyorsítótárat a használja, ha kell-e URL-cím kódolása a kulcs, használja a gyorsítótár, az alábbi példában látható módon.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Ha a kulcs nem URL-címként kódolandó, az alábbihoz hasonló kivételt jelenhet meg:`Failed to parse session.save_path`

Gyorsítótár vgx.dll PHP munkamenet gyorsítótárat a PhpRedis ügyfélprogrammal használatával kapcsolatos további tudnivalókért olvassa el a [PHP-munkamenet kezelő](https://github.com/phpredis/phpredis#php-session-handler)című témakört.



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Mikor célszerű engedélyezni a-SSL port vgx.dll való csatlakozáshoz?

Vgx.dll server nem támogatja az SSL beépített, de az Azure vgx.dll gyorsítótár tartalmaz. Ha Azure vgx.dll gyorsítótár csatlakozik, és az ügyfelek StackExchange.Redis, például az SSL támogatja, akkor használja az SSL.

Figyelje meg, hogy az-SSL port le van tiltva, alapértelmezés szerint az új Azure vgx.dll gyorsítótár-példányok. Ha az ügyfélnek nem támogatja az SSL, majd engedélyeznie kell az-SSL port a [konfigurálása az Azure vgx.dll gyorsítótárban gyorsítótárat](cache-configure.md) a [hozzáférés-portok](cache-configure.md#access-ports) szakaszban megjelenő utasításokat követve.

Például: vgx.dll eszközök `redis-cli` nem működnek az SSL port, de használhatja például egy segédprogramot `stunnel` hogy biztonságosan csatlakoztathassa az eszközöket az SSL port [Kihirdetése ASP.NET munkamenet állapot-szolgáltató az előnézeti Release vgx.dll](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbejegyzés utasításait követve.

A vgx.dll eszközök letöltését, tanulmányozza a [hogyan is futtatása vgx.dll parancsok?](#cache-commands) szakaszban.



### <a name="what-are-some-production-best-practices"></a>Mik azok a gyártási gyakorlati tanácsokat?

-   [Ajánlott eljárások a StackExchange.Redis](#stackexchangeredis-best-practices)
-   [Konfigurációs és fogalmak](#configuration-and-concepts)
-   [A teljesítmény tesztelése](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Ajánlott eljárások a StackExchange.Redis

-   Beállítása `AbortConnect` hamis, majd hagyja, hogy az automatikusan újra ConnectionMultiplexer. [További információt itt talál](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   A ConnectionMultiplexer újrafelhasználása – ne hozzon létre egy újat szeretne minden egyes kérelem. A `Lazy<ConnectionMultiplexer>` mintát [itt ismertetett](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) ajánlott.
-   Vgx.dll működik legjobban, amelyeken a kisebb értékek, így fontolja meg a több billentyűk be nagyobb másolatokat aprító. [Ez a vitafórum vgx.dll](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)100 kb "nagy" számít. Példa problémához okozhatott nagy értékeket [Ez a cikk](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) további.
-   Állítsa be a [szálkészlet beállítások](#important-details-about-threadpool-growth) időtúllépései elkerülése érdekében.
-   Használjon legalább az alapértelmezett connectTimeout 5 másodperc. Ez egy hálózati blip esetén, a kapcsolatot létesíteni a megfelelő időben StackExchange.Redis adna.
-   A felhasználónevek futtatja a különböző műveletek társított teljesítmény költségek. Ha például a `KEYS` parancs O(n) műveletet, és el kell kerülni. A [webhely redis.io](http://redis.io/commands/) körül az egyes műveletek, amely támogatja az idő összetettségétől részletek tartalmaz. Minden egyes parancsra kattintva megtekintheti az összes művelet összetettségétől.

#### <a name="configuration-and-concepts"></a>Konfigurációs és fogalmak

-   Normál vagy prémium réteg használata munkakörnyezetben. Az egyszerű réteg egyetlen csomópont rendszer nincs adatok replikáció és nincs SLA. Legalább egy C1 gyorsítótár is használja. C0 gyorsítótárát egyszerű fejlesztők/vizsgálatot felhasználási területei tényleg vannak.
-   Ne feledje, hogy vgx.dll egy **A memóriában** adatokat tároló. [Ez a cikk](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) további, úgy, hogy az adatok elvesztését is előfordulhatnak esetek tudatában.
-   A rendszer fejlesztése, úgy, hogy is képes kezelni kapcsolat blips [javítása és feladatátvételi miatt](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>A teljesítmény tesztelése

-   Indítsa el a `redis-benchmark.exe` teszteli a megtudhatja, lehetséges átviteli saját perf írása előtt. Figyelje meg, hogy vgx.dll javasolt nem támogatja az SSL, úgy fog [keresztül az Azure portálon a nem SSL - port engedélyezése](cache-configure.md#access-ports) előtt futtatása a tesztet. Példák [hogyan teljesítményteszt és tesztelje a gyorsítótár teljesítményét?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   Az ügyfél teszteléshez használt virtuális ugyanabban a régióban a gyorsítótár vgx.dll példányként kell lennie.
-   Azt javasoljuk, hogy segítségével Dv2 virtuális sorozat az ügyfél jobb hardverre van szükség, és a legjobb eredményt ad.
-   Ellenőrizze, hogy az ügyfél virtuális kiválaszthatja, hogy rendelkezik-e legalább sok számítások és sávszélesség lehetőséget, mint a gyorsítótár tesztelése. 
-   Az ügyfélgépen VRSS engedélyezése, ha Windows. [További információt itt talál](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium réteg vgx.dll példányok fog van jobban hálózati késés és az átvitt mivel jobb hardveren Processzor és a hálózati futnak.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Melyek a megfontolások közös vgx.dll parancsok használata esetén?

-   Nem futtatnia kell bizonyos vgx.dll parancsok, amelyek sokáig anélkül, hogy milyen következményekkel járnak ezek a parancsok ismertetése befejezéséhez.
    -   Például nem működnek a [BILLENTYŰK](http://redis.io/commands/keys) parancs gyártási, a visszatérési billentyűk számától függően túl hosszú ideig is eltelhet. Vgx.dll egy egyetlen összefűzött kiszolgáló és egyszerre feldolgozásával parancsokat. Ha más kulcsok után kiadott parancsokat, ezek nem dolgozza mindaddig, amíg a BILLENTYŰK parancs vgx.dll dolgozza fel. A [webhely redis.io](http://redis.io/commands/) körül az egyes műveletek, amely támogatja az idő összetettségétől részletek tartalmaz. Minden egyes parancsra kattintva megtekintheti az összes művelet összetettségétől.
-   Fő méretek - használjam kis kulcs/vagy nagy kulcs/értékeket? Az általános attól függ az alkalmazási példát. Ha igényektől nagyobb kulcsok van szüksége, majd módosíthatja a ConnectionTimeout, és ismételje meg az értékeket és módosítsa az újraküldés logikája. A szemszögéből vgx.dll server a kisebb értékek, hogy a jobb teljesítmény figyelhető meg.
-   Ez nem jelenti, hogy Ön nem nagyobb érték tárolása vgx.dll; a következő szempontokat mérlegelve tudatában kell lennie. Késések nagyobb lesz. Egyik adatkészletet, amely nagyobb és egy kisebb, ha több ConnectionMultiplexer példányok is használhatja, minden egyes konfigurált időtúllépés, majd próbálkozzon újra értékek, más beállítási [végezze el a StackExchange.Redis beállítási lehetőségek mire](#cache-configuration) az előző szakaszban leírtak szerint.



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Hogyan lehet teljesítményteszt és tesztelje a gyorsítótár teljesítményét?

-   A [gyorsítótár diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) , így [monitor](cache-how-to-monitor.md) a gyorsítótár állapotának. A mérési módja miatt az Azure-portálon megtekintése és is [letöltése, és tekintse át](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) őket az eszközök lehetőség használatával.
-   Használhatja vgx.dll benchmark.exe próba betöltése vgx.dll kiszolgálóját.
-   Győződjön meg arról, hogy vannak-e a betöltés, tesztelje a ügyfél és a vgx.dll gyorsítótár ugyanabban a régióban.
-   Vgx.dll cli.exe használja, és figyelemmel követheti a a gyorsítótár az információ parancsot.
-   Ha a betöltés okoz a magas memória töredezettség majd meg kell méretezhető gyorsítótár nagyobb méretű.
-   A vgx.dll eszközök letöltését, tanulmányozza a [hogyan is futtatása vgx.dll parancsok?](#cache-commands) szakaszban.

Az alábbi képen a vgx.dll benchmark.exe használatára. A pontos eredmény érdekében ezt-parancsokat futtathat egy virtuális ugyanabban a régióban, mint a gyorsítótár.

-   Egy 1 k-tartalom használata próba technikájú SET kérések

    vgx.dll benchmark.exe - h **yourcache**. 1024 - P 50 - n 1000000 -d a redis.cache.windows.net- **yourAccesskey** -t beállítása
    
-   Egy 1 k-tartalom használata próba technikájú első kéri. 
    Megjegyzés: Futtatása megadása tesztelése először gyorsítótár feltöltéséhez felett látható
    
    vgx.dll benchmark.exe - h **yourcache**. redis.cache.windows.net- **yourAccesskey** -t GET -n 1000000 - d 1024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Fontos tudnivalókat a szálkészlet növekedés

A CLR szálkészlet kétféle szálak - "Dolgozó" és "Bemeneti/kimeneti teljesítése Port" (más néven IOCP) beszélgetésekben. 

-   Dolgozó szál használható az összetevőjét, például a feldolgozás `Task.Run(…)` vagy `ThreadPool.QueueUserWorkItem(…)` módszereket. Munka kell háttér szálon fordulhat elő, ha ezek a szálak is használják a CLR a különféle összetevőket.
-   IOCP szálak használnak aszinkron IO történik (például a hálózatról olvasás). 

A szál készlet közre új dolgozó szálak vagy I/O teljesítése szálak igény (bármely szabályozása) vagy a "Legalább" beállítás szál hibatípusonként eléréséig. Alapértelmezés szerint a lehető legkevesebb szálak rendszeren processzorok számának értéke. 

Miután meglévő (elfoglalt) számát szálak találatok szálak, a szálkészlet "minimális" száma lesz az érték, amelynél szabályozási beszúrhatja a egy szálhoz 500 ezredmásodperc egy új beszélgetésekben. Ez azt jelenti, hogy ha a rendszer erre szolgáló egy IOCP szál munka burst kap, akkor a program dolgozza használható nagyon gyorsan. Jó helyen jár Ha a munka burst több, mint a beállított "Minimális" beállítást, lesz néhány késleltetés feldolgozása néhány a munkát, a szálkészlet megvárja, amíg fordulhat elő, hogy két dolgot.

1. Egy meglévő szál lesz szabad munka feldolgozása.
1. Nincs meglévő szál 500ms, ingyenesen lesz, így egy új szál jön létre.

Alapvetően azt jelenti, hogy elfoglalt szálak értéke nagyobb, mint a Min szálak, amikor is valószínűleg fizet 500ms késleltetést előtt az alkalmazás által feldolgozott hálózati forgalmának engedélyezésére. Fontos is, vegye figyelembe, hogy amikor egy meglévő szál hosszabb, mint (a lehet emlékszik alapján) 15 másodperc üresjárati maradjon, akkor fogja kiüríteni és a NÖV és csökkenés ciklus megismételheti.

Ha megnézi az egy példa a hibaüzenetre a StackExchange.Redis (1.0.450 összeállítása vagy újabb), látni fogja a most nyomtatása szálkészlet statisztika (lásd a lenti IOCP és dolgozó részletes).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

A fenti példában megtekintheti, hogy IOCP szálhoz 6 elfoglalt szálak vannak, és a rendszer konfigurációja 4 minimális szálak engedélyezni. Ebben az esetben az ügyfél volna egyszer valószínűleg látott két 500 ms késések mivel 6 > 4.

Figyelje meg, hogy StackExchange.Redis időtúllépései is találati, ha IOCP vagy a dolgozó szál értéknövekedésével szabályozott kap.

### <a name="recommendation"></a>Javaslat

Adott ezt az információt, javasoljuk, hogy a vevők IOCP és dolgozó szálak minimális konfiguráció értékének beállítása valamire nagyobb, mint az alapértelmezett értéket. Milyen ezt az értéket kell mert egy alkalmazáshoz a megfelelő értéket lesz túl magas vagy alacsony egy másik alkalmazás one-size-fits-all útmutatást ad a nem. Ezt a beállítást is hatással lehet a teljesítmény bonyolult alkalmazások, más részeinek ügyfelek van szüksége, testre szabhatja a saját igényeinek ezt a beállítást. A helyes kezdő hely 200 vagy 300, tesztelje és szükség szerint működését.

Ez a beállítás Útmutató:

-   ASP.NET, használja a ["minIoThreads" konfigurációs beállítások][] csoportban a `<processModel>` web.config konfigurációs elemét. Azure webhelyek belül futtat, ha ez a beállítás nem elérhetővé tett a beállítási lehetőségek között. Azonban továbbra is ezeket a beállításokat programozás útján kell (lásd alább) a global.asax.cs Application_Start módszert a.

> **Fontos megjegyzés:** a konfigurációs elem a megadott érték egy *- core* beállítást. Ha például egy 4 core számítógépre, és szeretné futásidőben 200 kell a minIOThreads beállítást, ha használja `<processModel minIoThreads="50"/>`.

-   ASP.NET-Ön kívül a [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) használata API-VAL.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Kiszolgáló globális Katalógus lehető további átviteli az ügyfél StackExchange.Redis engedélyezése

Kiszolgáló globális Katalógus engedélyezése az ügyfél optimalizálása, és adja meg a jobb teljesítményt és átviteli StackExchange.Redis használata esetén. A globális Katalógus server és a ahhoz, hogy miként további tudnivalókért lásd: az alábbi cikkekben.

-   [Ahhoz, hogy kiszolgáló globális Katalógus](https://msdn.microsoft.com/library/ms229357.aspx)
-   [A szemétgyűjtő alapjai](https://msdn.microsoft.com/library/ee787088.aspx)
-   [Szemétgyűjtő és a teljesítmény](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Hogyan figyelni a rendszerállapot és a teljesítmény a gyorsítótár?

Microsoft Azure vgx.dll gyorsítótár-példányok figyelhető az [Azure-portálon](https://portal.azure.com). Is mértékek megtekintése, rögzíteni a Startboard mértékek diagramok, testre szabhatja a dátum és idő cellatartományt figyelése diagramok, hozzáadása és mérőszámok eltávolítása a diagramok és értesítések beállítása, ha bizonyos feltételek teljesülése esetén. További tudnivalókért olvassa el a [Monitor Azure vgx.dll gyorsítótár](cache-how-to-monitor.md)című témakört.

A vgx.dll gyorsítótár- **Beállítások** lap a **támogatási + – hibaelhárítás** című szakaszát is megtalálható számos eszközt figyelemmel kísérésére és a gyorsítótárát hibaelhárítás. 

-   **Hibaelhárítás** a gyakori problémák és a megoldásukat stratégiák információt tartalmaz.
-   **Naplókat** a gyorsítótár végrehajtott műveletek ismertetése. Akkor is használhatja szűrés más erőforrások, ez a nézet kibontásához.
-   **Erőforrás-állapot** se nézze, amikor az erőforrás, és jelzi, hogy ha a várt módon fut. Többet szeretne tudni az Azure erőforrás állapot szolgáltatás [Azure erőforrás állapot áttekintése](../resource-health/resource-health-overview.md)című témakörben találhat.
-   **Új támogatási kérelem** kattintva nyissa meg a gyorsítótár támogatási kérelmet lehetőségeket is biztosít.

Ezek az eszközök lehetővé teszi az Azure vgx.dll gyorsítótár-példányok állapotának figyelheti, és a gyorsítótár alkalmazások kezeléséhez nyújt segítséget. További tudnivalókért olvassa el a [támogatási és hibaelhárítási beállítások](cache-configure.md#support-amp-troubleshooting-settings)című témakört.

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>A gyorsítótár diagnosztika tároló Fiókbeállítások megváltozott, hová tűnt?

Gyorsítótárát az ugyanazon régió és az előfizetés megosztása diagnosztika tároló ugyanazokat a beállításokat, és ha a beállítások módosítása (diagnosztika engedélyezett/letiltott vagy a tárterület-fiók módosítása) az adott régióban az előfizetés minden gyorsítótárát vonatkozik. Ha a gyorsítótár diagnosztika beállításainak módosította, ellenőrizze, hogy megváltozott-e a diagnosztikai az előfizetést és a régió egy másik gyorsítótár beállításainak. Ellenőrizze, hogy egy úgy, hogy a gyorsítótár beállítása a naplófájlok megtekintése egy `Write DiagnosticSettings` esemény. Naplókat használata a további tudnivalókért lásd: [események megtekintése és egyéb naplókat](../monitoring-and-diagnostics/insights-debugging-with-events.md) és [erőforrás-kezelő műveletek naplózási](../resource-group-audit.md). Azure vgx.dll gyorsítótár eseményeinek figyelése a további tudnivalókért lásd: a [műveletek és a riasztások](cache-how-to-monitor.md#operations-and-alerts)lehetőséget.

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Miért diagnosztika engedélyezve van néhány új gyorsítótárát, de nem az összes?

Gyorsítótárát a terület és az előfizetés megosztása diagnosztika tároló ugyanazokat a beállításokat. A terület és a előfizetés, amely tartalmazza a diagnostics engedélyezve van egy másik gyorsítótár új gyorsítótár hoz létre, ha diagnosztika engedélyezve van az új gyorsítótár ugyanazokat a beállításokat használja.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Miért jelennek meg időtúllépései?

Időtúllépései fordulhat elő, az ügyfél, ha társalogni szeretne vgx.dll használt. Az esetek többségében a vgx.dll kiszolgáló nem nem időkorlátja. Parancs a vgx.dll kiszolgálóra küldött, a parancs az aszinkron, és vgx.dll kiszolgáló ahányat felveszi a parancsot, és végrehajtja a azt. Azonban az ügyfél időkorlátja folyamat során, és ha igen, kivételt is következik a hívó oldalon. Időtúllépés kapcsolatos problémák megoldásáról bővebben a [ügyfél, oldal hibaelhárítási](cache-how-to-troubleshoot.md#client-side-troubleshooting) és [StackExchange.Redis időtúllépés kivételek](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions)témakörben talál.


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Miért megszakadt az ügyfél, a gyorsítótárból?

Az alábbiakban néhány gyakori oka egy gyorsítótár kapcsolat bontása.

-   Ügyféloldali okoz
    -   Az ügyfélalkalmazás üzlethez volt.
    -   Az ügyfélalkalmazás olyan méretezési műveletet végre.
        -   Cloud Services vagy a Web Apps alkalmazások okai lehetnek az automatikus méretezés.
    -   A hálózati réteg ügyféloldali változik.
    -   Ideiglenes (tranziens) hiba történt, az ügyfél vagy a hálózati csomópontok között az ügyfél és a kiszolgálón.
    -   A sávszélesség küszöb korlátai voltak elérhető.
    -   Processzor kötött műveletek elvégzéséhez túl sokáig tartott.
-   Kiszolgálóoldali okoz
    -   A szabványos gyorsítótár kínáló az Azure vgx.dll gyorsítótár-szolgáltatás kezdeményezni egy veszi át az elsődleges csomópontra a másodlagos csomópontot.
    -   Azure a példány, ahol a gyorsítótár telepítették lett javítása
        -   Ez lehet vgx.dll kiszolgáló frissítések vagy általános virtuális karbantartás.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Melyik Azure gyorsítótár ajánlja fel a megfelelő számomra?

>[AZURE.IMPORTANT]Szerinti utolsó évben [bejelentése](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)az Azure felügyelt gyorsítótár-szolgáltatás és Azure szerepkör a gyorsítótár-szolgáltatás a November 30 2016 lehet visszavonni. Az ajánlási [Azure vgx.dll gyorsítótár](https://azure.microsoft.com/services/cache/)környezetbe. Áttelepítése a további tudnivalókért lásd [a gyorsítótár-szolgáltatás kezelése Azure vgx.dll gyorsítótár áttelepítése](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Azure vgx.dll gyorsítótár
Azure vgx.dll gyorsítótár pedig általában elérhető 53 GB méretű az elérhetőség SLA 99,9 %-át foglalja magában. Az új [prémium réteg](cache-premium-tier-intro.md) kínál mérete legfeljebb 530 GB és támogatási VNET, és az adatmegőrzési 99,9 % szolgáltatásiszint-szerződés a fürtözéshez.

Azure vgx.dll gyorsítótár amelyekkel a felhasználók egy biztonságos, célorientált vgx.dll gyorsítótárat használja, a Microsoft kezeli. Ezt az ajánlatot kap a gazdag szolgáltatáskészlet és vgx.dll, és a megbízható szolgáltatója és figyelése a Microsoft által nyújtott ökológiai használhatja.

Hagyományos gyorsítótárát, amely kulcs – érték párokká foglalkozik vgx.dll wordtől elérően – a nagyon performant adattípusokhoz népszerű. Vgx.dll is támogatja az alábbi típusát, például; karakterlánccá fűz atomi műveleteket fut növekvő kivonat; értéke listához; terjesztése számítógépes beállítása metszet, Unió és különbség; vagy a tag első rendezett az argumentumlista legnagyobb prioritású. Egyéb funkciók tranzakciók, pub/sub, Lua scripting korlátozott billentyűk time-to-live, és a beállításokat, hogy több úgy viselkednek, mint a hagyományos gyorsítótár vgx.dll tartalmazzák.

Egy másik kulcsfontosságú vgx.dll sikeres foglalhatja keretbe beépített megfelelő, ragyogó Megnyitás ökológiai. Ez is megjelenik a rendelkezésre álló vgx.dll ügyfél különböző megadása több nyelv keresztül. Lehetővé teszi, hogy szinte bármelyik terhelést Azure rendszer generál szeretné használni. 

Azure vgx.dll gyorsítótár – első lépések kapcsolatos további tudnivalókért olvassa el a [hogyan használata Azure vgx.dll gyorsítótár](cache-dotnet-how-to-use-azure-redis-cache.md) és [Azure vgx.dll gyorsítótár dokumentáció](https://azure.microsoft.com/documentation/services/redis-cache/)című témakört.

### <a name="managed-cache-service"></a>Felügyelt gyorsítótár-szolgáltatás
[Felügyelt gyorsítótár-szolgáltatás beállítása November 30 2016 inaktívvá tenni.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>Szerepkör a gyorsítótár
[Szerepkör a gyorsítótár beállítása November 30 2016 inaktívvá tenni.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





["minIoThreads" kereséskonfigurációs beállítás]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
