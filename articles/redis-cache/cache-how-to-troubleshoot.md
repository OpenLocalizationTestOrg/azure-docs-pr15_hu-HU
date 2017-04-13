<properties 
    pageTitle="Azure vgx.dll gyorsítótár elhárítása |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Azure vgx.dll gyorsítótár kapcsolatos gyakori problémák megoldásához." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Azure vgx.dll gyorsítótár hibák elhárítása

Ez a cikk nyújt útmutatást a következő kategóriák Azure vgx.dll gyorsítótár-problémák elhárítása.

-   [Ügyfél egymás hibaelhárítás](#client-side-troubleshooting) – Ez a szakasz útmutatásokat tartalmaz azonosító, és az alkalmazás csatlakoztatása az Azure vgx.dll gyorsítótár okozta problémák megoldása.
-   [Kiszolgáló egymás hibaelhárítás](#server-side-troubleshooting) – Ez a szakasz ismertetése útmutató azonosítása és Azure vgx.dll gyorsítótár kiszolgálóoldalon okozta problémák megoldása
-   [StackExchange.Redis időtúllépés kivételek](#stackexchangeredis-timeout-exceptions) – Ez a szakasz ismerteti kapcsolatos problémák megoldásáról, amikor a StackExchange.Redis ügyfélprogramban.


>[AZURE.NOTE] Ebből az útmutatóból a hibaelhárítási lépéseket számos olyan vgx.dll parancsokat, és figyelemmel követheti a különböző teljesítménymutatók utasításokat. További információt és útmutatást olvassa el a [További tudnivalók](#additional-information) a szakaszban a cikkeket.

## <a name="client-side-troubleshooting"></a>Ügyfél egymás – hibaelhárítás


Ez a szakasz ismerteti, hogy az ügyfélalkalmazás a feltétel miatt hibaelhárítási problémák.

-   [A memória nyomása az ügyfélszámítógépen](#memory-pressure-on-the-client)
-   [A forgalom burst](#burst-of-traffic)
-   [Magas ügyfél processzorhasználata](#high-client-cpu-usage)
-   [Ügyfél egymás sávszélesség túllépve](#client-side-bandwidth-exceeded)
-   [Nagy kérés/válasz mérete](#large-requestresponse-size)
-   [Mi történt a vgx.dll az adataimat?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>A memória nyomása az ügyfélszámítógépen

#### <a name="problem"></a>A probléma

Memória nyomása az ügyfélszámítógépen, amely bármely késedelem nélkül vgx.dll példány által küldött adatok feldolgozásának késleltetheti teljesítményproblémákat mindenféle vezet. Találatok memória nyomása, amikor a rendszer a szokásos rendelkezik lap adatok a virtuális memória, amely a lemezen fizikai memória. Ez a *lap hibát okozó* hatására jelentősen csökkentheti a rendszer.

#### <a name="measurement"></a>A mérési 

1.  Győződjön meg arról, hogy nem haladja meg rendelkezésre álló memória számítógépen memóriahasználat figyelése 
2.  Monitor a `Page Faults/Sec` teljesítménymutató. A legtöbb rendszer fog lap hibákat normál működés közben is, a kiugrásainak, amelyek megfelelnek a időtúllépései megfelelő a lap hibák teljesítmény számláló, nézze meg.

#### <a name="resolution"></a>Megoldás

Az ügyfelek frissítése a virtuális memória méretet nagyobb ügyfél, illetve alaposabban meg azokat a memória használatát mintázatok csökkentéséről memóriahasználat consuption.


### <a name="burst-of-traffic"></a>A forgalom burst

#### <a name="problem"></a>A probléma

A forgalom felszakadásáig kombinálni gyenge `ThreadPool` beállításai már a vgx.dll kiszolgáló által küldött, de még nem ügyféloldali felhasznált adatok feldolgozása veheti eredményez.

#### <a name="measurement"></a>A mérési 

Monitor hogyan a `ThreadPool` statisztikák kód [ilyen](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs)használatával időbeli változását. Is megnézheti a `TimeoutException` StackExchange.Redis üzenete. Lássunk egy példát:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

A fenti üzenetben problémák lépnek fel több, amelyek érdekes:

 1. Figyelje meg, hogy az a `IOCP` szakasz és a `WORKER` szakasz van egy `Busy` érték, amely nagyobb, mint a `Min` értéket. Ez azt jelenti, hogy a `ThreadPool` beállításokat kell kiigazítására.
 2. Is `in: 64221`. Ez azt jelzi, hogy 64211 bájt a kernel szoftvercsatorna réteget nem érkezett, de még nem még lett olvassa el az alkalmazás (például StackExchange.Redis). Ez általában azt jelenti, hogy az alkalmazás nem adatok beolvasása a hálózatról gyorsan meg a kiszolgáló épp küldi.

#### <a name="resolution"></a>Megoldás

Állítsa be a [Szálkészlet beállításokat](https://gist.github.com/JonCole/e65411214030f0d823cb) , hogy győződjön meg arról, hogy a szál készlet fog szerkezetének kialakítása gyorsan burst esetek csoportban.


### <a name="high-client-cpu-usage"></a>Magas ügyfél processzorhasználata

#### <a name="problem"></a>A probléma

Az ügyfélgépen processzort jelzi, hogy a rendszer nem naprakész maradhat a munka elvégzéséhez felkérték. Ez azt jelenti, hogy az ügyfél meghiúsulhat, egy időben választ vár vgx.dll feldolgozása, akkor is, ha a választ nagyon gyorsan küldött vgx.dll.

#### <a name="measurement"></a>A mérési

Figyelje meg a rendszer széles processzorhasználata vagy a kapcsolódó teljesítmény számláló az Azure-portálon keresztül. Ügyeljen arra, hogy figyelheti a *folyamat* Processzor, mert egyetlen folyamat beállíthatja, hogy alacsony processzorhasználata általános rendszer Processzor nagy lehet egy időben. Megtekintés az kiugrásainak, amelyek megfelelnek a időtúllépései megfelelő processzorhasználata a. Magas Processzor, eredményeként is megjelenhet magas `in: XXX` szereplő értékek `TimeoutException` hibák elhárítása a [forgalom Burst](#burst-of-traffic) szakaszban leírt módon.

>[AZURE.NOTE] StackExchange.Redis 1.1.603, és később a `local-cpu` a metrikus `TimeoutException` hibaüzenetek jelennek meg. Győződjön meg róla, akkor a [StackExchange.Redis NuGet csomag](https://www.nuget.org/packages/StackExchange.Redis/)legújabb verzióját használja. Vannak olyan javított folyamatosan alatt a kódot, és hogy legyen megbízhatóbb időtúllépései ezért fontos problémákat a legújabb verzióra.

#### <a name="resolution"></a>Megoldás

További Processzor kapacitással virtuális nagyobb méretű frissítése vagy vizsgálja meg, mi okozza Processzor kiugrásainak megfelelő. 



### <a name="client-side-bandwidth-exceeded"></a>Ügyfél egymás sávszélesség túllépve

#### <a name="problem"></a>A probléma

Különböző méretű ügyfél gépek korlátozásokkal rendelkezik a mekkora sávszélesség rendelkeznek érhető el. Ha az ügyfél meghaladja a rendelkezésre álló sávszélességet, majd adatok nem dolgozza ügyféloldali gyorsan a kiszolgáló küld. Ez a vezethet időtúllépései.

#### <a name="measurement"></a>A mérési

Figyelje meg, hogyan a sávszélesség-használat kód [ilyen](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs)használatával időbeli változását. Figyelje meg, hogy ez a kód nem futtathatnak sikeresen bizonyos környezetekben engedélykorlátozásokkal (például Azure webhelyek).

#### <a name="resolution"></a>Megoldás 

Ügyfél virtuális méretét növelheti vagy csökkentheti a hálózati sávszélességet.


### <a name="large-requestresponse-size"></a>Nagy kérés/válasz mérete

#### <a name="problem"></a>A probléma

Egy nagy kérés/válasz okozhatják időtúllépései. Példaként tegyük fel, hogy a időtúllépés az ügyfélgépen beállított értéke 1 másodperc. Az alkalmazás egyszerre (az azonos fizikai hálózati kapcsolaton keresztül) kéri a két billentyű (például "A" és "B"). A legtöbb ügyfelek kérelmeket, "Pipelining" támogatják az, hogy mindkét "A" és "B" összehívásokat kattintson az egyik kiszolgálóra vezetékes után a másik Várakozás a válaszra nélkül. A kiszolgáló küld a válaszokat vissza ugyanabban a sorrendben. Ha "A" választ nagy ahhoz, hogy ezután valaki időtúllépés leggyakrabban is eszik. 

A következő példa bemutatja, hogyan ebben az esetben. Ebben az esetben "A" és "B" kérelem küld gyorsan, a kiszolgáló elindul, gyors "A" és "B" válaszok küldése, de adatátviteli idők, mert a "B" elakad kérés és időtúllépést mögött annak ellenére, hogy a kiszolgáló válasza gyorsan.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>A mérési

Ez a nehéz egy mérésére. Alapvetően kell nyomon követheti a nagy-összehívások és válaszok az ügyfél kód eszköz. 

#### <a name="resolution"></a>Megoldás

1.  Vgx.dll néhány nagy értékek helyett a kisebb értékek nagyszámú optimalizálva. A használni kívánt megoldás, a kisebb értékek kapcsolódó adatok osztására. Ismerje meg a [méret ideális értéket tartományát vgx.dll az? Érték túl nagy 100KB?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) További információ a kisebb értékek ajánlott miért körül bejegyzést.
2.  A virtuális (az ügyfél és vgx.dll gyorsítótár-kiszolgáló), a betűméret növelése nagyobb sávszélességre funkciók, adatátviteli idők nagyobb válaszok csökkentése eléréséhez. Előfordulhat, hogy több sávszélesség csak a kiszolgálón vagy csak az első az ügyfélnek nem elég. Mérje le a sávszélesség-használat, és hasonlítsa össze a virtuális verziójától méretének funkcióinak.
3.  Számának növelése `ConnectionMultiplexer` , különböző kapcsolatokhoz objektumok használata és ciklikus kérések.


### <a name="what-happened-to-my-data-in-redis"></a>Mi történt a vgx.dll az adataimat?

#### <a name="problem"></a>A probléma

Várt eredményt az egyes az Azure vgx.dll gyorsítótár-példány az adatokat, de azt nem tűnnek van.

##### <a name="resolution"></a>Megoldás

Lásd: [Mi történt a vgx.dll az adataimat?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) a lehetséges okok és megoldások.


## <a name="server-side-troubleshooting"></a>Kiszolgáló egymás – hibaelhárítás

Ez a szakasz ismerteti, hogy egy feltétel, a gyorsítótár-kiszolgáló miatt hibaelhárítási problémák.

-   [A memória működését a kiszolgálón](#memory-pressure-on-the-server)
-   [Processzort / kiszolgáló betöltése](#high-cpu-usage-server-load)
-   [Kiszolgáló egymás sávszélesség túllépve](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>A memória működését a kiszolgálón

#### <a name="problem"></a>A probléma

Memória nyomása a kiszolgálóoldalon vezet mindenféle teljesítményproblémákat, amely késleltetheti kérések feldolgozása. Találatok memória nyomása, amikor a rendszer a szokásos rendelkezik lap adatok a virtuális memória, amely a lemezen fizikai memória. Ez a *lap hibát okozó* hatására jelentősen csökkentheti a rendszer. Létezik a memória nyomása több lehetséges oka: 

1.  A teljes kapacitásának-gyorsítótár van töltve adatokat. 
2.  Vgx.dll megjelenik a magas memória töredezettség - leggyakrabban okozta nagy objektumokra tárolása (vgx.dll egy kis objektumok van optimalizálva – ismerje meg a [méret ideális értéket tartományát vgx.dll az? Érték túl nagy 100KB?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) tartalmakat tehet közzé részletekért). 

#### <a name="measurement"></a>A mérési

Vgx.dll közzététele két mértékek, amelyek segítséget nyújtanak a probléma azonosításához. Az első `used_memory` , a másik pedig `used_memory_rss`. [Ezeket a mértékek](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) érhetők el az Azure-portálon vagy a [Vgx.dll információ](http://redis.io/commands/info) parancsot.

#### <a name="resolution"></a>Megoldás

Létezik néhány lehetséges végzett módosítások is biztonságosabbá tételéről memóriahasználat megfelelő:

1. [A memória házirend beállítása](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) és a lejárati idő beállítása a kulcsokon. Előfordulhat, hogy ez nem elegendő, ha töredezettség van.
2. [Konfigurálása maxmemory fenntartott érték](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) , amely elég nagy memória töredezettség kompenzálja.
3. Oldaltörés a nagy gyorsítótárazott objektumainak kisebb kapcsolódó objektumok be.
4. [Méretezés](cache-how-to-scale.md) egy nagyobb használt gyorsítótár méretét.
5. Ha [engedélyezve vgx.dll fürthöz prémium gyorsítótár](cache-how-to-premium-clustering.md) használata [shards számának növelése](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache)is.

### <a name="high-cpu-usage--server-load"></a>Processzort / kiszolgáló betöltése

#### <a name="problem"></a>A probléma

Processzort is jelenti, hogy az ügyféloldali történhet a válaszát vgx.dll időben feldolgozása, akkor is, ha a választ nagyon gyorsan küldött vgx.dll.

#### <a name="measurement"></a>A mérési

Figyelje meg a rendszer széles processzorhasználata vagy a kapcsolódó teljesítmény számláló az Azure-portálon keresztül. Ügyeljen arra, hogy figyelheti a *folyamat* Processzor, mert egyetlen folyamat beállíthatja, hogy alacsony processzorhasználata általános rendszer Processzor nagy lehet egy időben. Megtekintés az kiugrásainak, amelyek megfelelnek a időtúllépései megfelelő processzorhasználata a.

#### <a name="resolution"></a>Megoldás

Egy nagyobb gyorsítótár [skála](cache-how-to-scale.md) az első csoportba tartozó további Processzor kapacitással, vagy vizsgálja meg, mi okozza Processzor kiugrásainak megfelelő. 

### <a name="server-side-bandwidth-exceeded"></a>Kiszolgáló egymás sávszélesség túllépve

#### <a name="problem"></a>A probléma

Különböző méretű gyorsítótár-példányok korlátozásokkal rendelkezik a mekkora sávszélesség rendelkeznek érhető el. Ha a kiszolgáló meghaladja a rendelkezésre álló sávszélességet, majd adatok nem küld amilyen gyorsan az ügyfél számára. Ez a vezethet időtúllépései.

#### <a name="measurement"></a>A mérési

Figyelheti a `Cache Read` mérőszám, amely a mennyiségű adat olvassa el a gyorsítótárból megabájtban (MB/s) másodpercenként a megadott jelentéskészítési időszak alatt. Ez a gyorsítótár által használt hálózati sávszélesség felel meg ezt az értéket. Ha szeretne állíthat be értesítéseket kiszolgáló egymás hálózati sávszélesség korlátozásairól, létrehozhat azokat meg `Cache Read` számláló. Hasonlítsa össze az értékek különböző gyorsítótár árak rétegek és a betűméretet a megfigyelt sávszélesség-korlátozások [az alábbi táblázat](cache-faq.md#cache-performance) értékű.

#### <a name="resolution"></a>Megoldás

Ha egységes közelében megfigyelt maximális sávszélességet árak réteg és a gyorsítótár méretét, fontolja meg a [Méretezés](cache-how-to-scale.md) árak réteg vagy, amelynek nagyobb hálózati sávszélesség, [a táblázatban](cache-faq.md#cache-performance) szereplő értékek használata útmutatóra méretét.


## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis időtúllépés kivételek

StackExchange.Redis használja a konfiguráció beállítása elnevezett `synctimeout` 1000 ms alapértelmezett értéke szinkron műveletekhez. Ha egy szinkronizált hívás a kikötött időpont nem elvégezni, a a StackExchange.Redis ügyfél okoz a következőhöz hasonló időtúllépésről szóló hibaüzenetet.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Ez a hibaüzenet tartalmazza a mértékek, mutasson a probléma okát, és a lehetséges felbontásának segítő. Az alábbi táblázat tartalmazza a hiba üzenet mérési módja miatt olvashat.

| Üzenet mutatója | Részletek                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| INST       | Az utolsó idő szelet a: 0 parancsok állítottak                                                                                                                                                                                              |
| kezelő        | A szoftvercsatorna kezelő végrehajtásához `socket.select` ami azt jelenti, hogy azt kéri, az operációs rendszer, amelynek valami ehhez; szoftvercsatornát jelzése alapvetően: az olvasó nem aktívan olvasó a hálózatról, mert a meg nem gondolja, hogy tartalmaz valamit, amit a teendő |
| a várakozási      | Vannak olyan 73 összes folyamatban lévő művelet                                                                                                                                                                                                        |
| qu         | 6 a folyamatban lévő műveletek az el nem küldött várakozási sorban található, és még nem szerepelnek a kimenő hálózati                                                                                                                                                           |
| QS         | 67 he-folyamatban műveletek a kiszolgálóra küldött, de választ még nem érhető el. A válasz lehet `Not yet sent by the server` vagy`sent by the server but not yet processed by the client.`                                                   |
| QC         | a folyamatban lévő műveletek 0 válaszok van látható, de van még nem jelölte meg a teljes teljesítése le Várakozás miatt                                                                                                                                      |
| wR         | Van egy aktív író (tehát, hogy a 6 el nem küldött kérések nem mellőzve) bájt/activewriters                                                                                                                                                   |
| a         | Nem aktív olvasók és nulla bájt elérhető legyen a hálózati kártya bájt/activereaders.                                                                                                                                               |


### <a name="steps-to-investigate"></a>Vizsgálja meg a lépéseket

1. A legjobb győződjön meg arról, a következő mintát szolgáltatást használ, ha a StackExchange.Redis ügyfélprogramban.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    További tudnivalókért olvassa el a [Csatlakozás a gyorsítótár StackExchange.Redis használatával](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Győződjön meg arról, hogy az Azure vgx.dll gyorsítótár kiürítése és az ügyfélalkalmazás vannak-e az ugyanazon régió az Azure-ban. Például akkor lehet, hogy kell első időtúllépései esetén a gyorsítótár kelet-Amerikai Egyesült Államokban van, de az ügyfél nyugati USA-beli és a kérelem belül nem befejezéséhez az `synctimeout` intervallumot, vagy előfordulhat, hogy kell kapott időtúllépései a helyi fejlesztési számítógépről hibakeresés alatt. 

    Erősen ajánlott, hogy a gyorsítótár és az ügyfél ugyanabban a Azure régióban. Ha régió közötti hívások tartalmazó példa, beállíthatja a `synctimeout` intervallum értéke nagyobb, mint az alapértelmezett 1000 ms intervallum felvételével egy `synctimeout` tulajdonság a kapcsolati karakterláncban. A következő példa bemutatja egy StackExchange.Redis gyorsítótár csatlakozási karakterlánc kódtöredékének az egy `synctimeout` 2000 MS.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Győződjön meg róla, akkor a [StackExchange.Redis NuGet csomag](https://www.nuget.org/packages/StackExchange.Redis/)legújabb verzióját használja. Vannak olyan javított folyamatosan alatt a kódot, és hogy legyen megbízhatóbb időtúllépései ezért fontos problémákat a legújabb verzióra.

4. Ha köti sávszélesség korlátozások, a kiszolgáló vagy az ügyfélprogram első összehívások, fejezze be, és egyúttal okozó időtúllépései hosszabb ideig tart. Ha a időtúllépés oka az, hogy a hálózati sávszélesség a kiszolgálón című témakör tartalmazza [kiszolgáló egymás sávszélesség túllépve](#server-side-bandwidth-exceeded). Ha a időtúllépés oka az, hogy az ügyfél hálózati sávszélesség talál további [ügyfél egymás sávszélesség túllépve](#client-side-bandwidth-exceeded).

6. Akkor Processzor első kötött a kiszolgálón vagy az ügyfél?
    -   Jelölje be, ha Ön az első köti össze Processzor az ügyfél, amelyek miatt nem lehet feldolgozni belül az értekezlet-összehívást a `synctimeout` időköze, így a időtúllépést okoz. Ügyfél nagyobb méretű áthelyezése vagy a betöltés terjesztése szabályozhatja a segítséget. 
    -   Jelölőnégyzetet, ha Processzor köti a kiszolgálón figyelése a `CPU` [gyorsítótár teljesítmény metrikus](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Állapotában vgx.dll kötött Processzor érkező kérések okozhatják időtúllépés e kérelmeket. Ez az a cím a betöltés elosztása prémium gyorsítótárban több shards, vagy nagyobb méretű vagy árak réteg frissítése. További tudnivalókért lásd: a [Kiszolgáló egymás sávszélesség túllépését](#server-side-bandwidth-exceeded).

7. Vannak olyan parancsok hosszú ideig tart a kiszolgálón feldolgozása? A vgx.dll kiszolgáló feldolgozása hosszú időt vesz parancsok hosszan futó okozhatják időtúllépései. Néhány példa a parancsok időigényes `mget` kulcsok, nagy számú `keys *` vagy rosszul írt lua parancsfájlokat. Az Azure vgx.dll gyorsítótár példányához a vgx.dll cli ügyfélprogramban, vagy a [Vgx.dll konzol](cache-configure.md#redis-console) és tartalmazza az kérések vártnál tovább tart a [SlowLog](http://redis.io/commands/slowlog) parancsot. A kiszolgáló vgx.dll és StackExchange.Redis kevesebb nagy kérelmeket, hanem sok kis kérelem vannak optimalizálva. Az adatok felosztása kisebb szövegadattömb be javíthatja az alábbi műveletet. 

    Az Azure-gyorsítótár SSL vgx.dll végpontot vgx.dll cli és stunnel való csatlakozással kapcsolatban további tudnivalókért lásd: a [Kihirdetése ASP.NET munkamenet állapot-szolgáltató az előnézeti Release vgx.dll](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blogbejegyzésből. További tudnivalókért olvassa el a [SlowLog](http://redis.io/commands/slowlog)című témakört.

8. Nagy vgx.dll kiszolgáló terhelés okozhatják időtúllépései. A kiszolgáló betöltés megfigyelésével figyelheti a `Redis Server Load` [gyorsítótár teljesítmény metrikus](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). A kiszolgáló terhelését 100 (legnagyobb érték) azt jelzi, hogy, a vgx.dll kiszolgáló már nincs üresjárati idejére elfoglalva kérelmek feldolgozása. Látható, ha bizonyos kérések összes a kiszolgáló képesség telik, futtassa a SlowLog parancs az előző bekezdés ismertetett módon. További tudnivalókért lásd: [magas processzorhasználata / kiszolgáló betölteni](#high-cpu-usage-server-load).

9. Volt minden más esetben a hálózati blip okozhatott ügyféloldali? Jelölje be az ügyfélszámítógépen (webes, dolgozó szerepkör vagy egy Iaas virtuális), ha volt egy eseményt, például az ügyfél példányainak száma méretezés, felfelé vagy lefelé, illetve engedélyezve van az ügyfél vagy automatikus méretezése új verziójának telepítésével? A tesztelés van talált Automatikus méretezéssel vagy a felfelé/lefelé méretezés okozhatják a kimenő a hálózati kapcsolat elveszhetnek néhány másodpercig. StackExchange.Redis kód rugalmassá ilyen eseményekre és újracsatlakozik. A megadott időszakban újbóli kapcsolat bármely kérelmek a várakozási sorban található is időkorlátja.

10. Egy nagy kérelmet több kis kérések megelőző időtúllépési vgx.dll gyorsítótárba volt? A paraméter `qs` a hibát az üzenet szerint hány kérést küld az ügyféltől a kiszolgáló ki, de még nem dolgozott választ. Ez az érték is megtartása növekvő, mert StackExchange.Redis egyetlen TCP-kapcsolatot használ, és csak olvasható egy válasz egyszerre. Annak ellenére, hogy az első művelet időtúllépési, nem akadályozza meg az adatok elküldése a és a kiszolgálóról, és más kérelmek blokkolt, amíg meg nem fejeződött, másolat idő okozza. Egy megoldás, ha az esélye annak időtúllépése összezárása biztosítva, hogy a gyorsítótár-ná a terhelést és nagy értékeket felosztása kisebb szövegadattömb be. Egy másik megoldást környezetbe erőforráskészlethez tartozik `ConnectionMultiplexer` az ügyfélnek az objektumokat, és válassza a legalább betöltött `ConnectionMultiplexer` Ha új kérelem küld. Ez gátolhatja egyetlen időtúllépés más kérelmek is időtúllépés okozza.

11. Ha használja `RedisSessionStateprovider`, győződjön meg róla, a kísérletek időtúllépése megfelelően van állítva. `retrytimeoutInMilliseconds`érdemes lehet nagyobb, mint `operationTimeoutinMilliseonds`, egyéb esetben nem újrapróbálkozások akkor fordul elő. A következő példában `retrytimeoutInMilliseconds` 3000 van beállítva. További tudnivalókért olvassa el a [ASP.NET munkamenet állapot-szolgáltató Azure vgx.dll gyorsítótár](cache-aspnet-session-state-provider.md) és [a konfigurációs paraméterek munkamenet állapot-szolgáltató és a kimeneti gyorsítótár-szolgáltató használata](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration)című témakört.


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Ellenőrzi az Azure vgx.dll gyorsítótár-kiszolgálón memóriahasználat [figyelése](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` és `Used Memory`. Ha az eviction házirend helyen, vgx.dll elindítja az kizárása kulcsok mikor `Used_Memory` eléri a gyorsítótár méretét. Ideális esetben `Used Memory RSS` kell lennie csak kicsit nagyobb, mint `Used memory`. Nagy különbség azt jelenti, hogy nincs memória töredezettség (belső és külső. Ha `Used Memory RSS` értéke kisebb, mint `Used Memory`, ez azt jelenti, hogy a gyorsítótár-memória része van már cserélni, az operációs rendszer. Ebben az esetben is a várt néhány lényeges késések. Mivel vgx.dll nem szabályozható a hozzárendelések hogyan vannak megfeleltetve memória lapok, magas `Used Memory RSS` oka gyakran egy memóriahasználat a Nyárs. Memória vgx.dll felszabadítások, amikor a memória vissza adandó a foglaló, és előfordulhat, hogy a foglaló, vagy nem adhat a memória vissza a rendszer. Lehetnek olyan közötti eltérést a `Used Memory` , az operációs rendszer által jelzett érték és a memóriában felhasználás. Lehet, hogy a FAKT miatt memória használt és megtörtént, amely szerint vgx.dll, de nem az adott vissza, a rendszer. Memória problémák kerülheti hajtsa végre az alábbi lépéseket.
    -   Frissítse a gyorsítótár nagyobb méretű, hogy nem futtatja az alábbi memória korlátozások a rendszer.
    -   A billentyűparancsok a lejárati idő megadhatja, hogy a régebbi értékek vannak kizárt ezzel kapcsolatban beérkező.
    -   Monitor a a `used_memory_rss` metrikus gyorsítótár. Amikor ezt az értéket megközelíti a gyorsítótár méretét, valószínűleg indítása, megjelenik a teljesítménnyel kapcsolatos problémákat. Ha egy prémium gyorsítótár használatával, vagy frissítése a gyorsítótár nagyobb méretű elosztása az adatok több shards.

    További tudnivalókért olvassa el a [Memória nyomása a kiszolgálón](#memory-pressure-on-the-server)című témakört.

## <a name="additional-information"></a>További információk

-   [Milyen vgx.dll gyorsítótár felkínálása és a méret érdemes használnom?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Hogyan lehet teljesítményteszt és tesztelje a gyorsítótár teljesítményét?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Hogyan lehet vgx.dll parancsai futtathatók?](cache-faq.md#how-can-i-run-redis-commands)
-   [Azure vgx.dll gyorsítótár figyelése](cache-how-to-monitor.md)