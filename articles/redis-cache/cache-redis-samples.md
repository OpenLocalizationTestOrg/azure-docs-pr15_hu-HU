<properties 
    pageTitle="Azure vgx.dll gyorsítótár minták |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Azure vgx.dll gyorsítótár használata" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Azure vgx.dll gyorsítótár minták 

Ez a témakör az Azure vgx.dll gyorsítótár mintát, például a gyorsítótár, olvasási és írási adatok és a gyorsítótárból csatlakozás és a ASP.NET vgx.dll gyorsítótár-szolgáltatókkal esetek kiterjedő listáját. A minták letölthető projektek, és részletes útmutatást, és olyan kódtöredék azonban nem csatolása letölthető projekthez.

## <a name="hello-world-samples"></a>Helló világ minták

Ebben a szakaszban a minták megjelenítése az Azure vgx.dll gyorsítótár-példány kapcsolódik, és olvasása, és adatok írásához a gyorsítótár nyelvek számos alapjait, és ügyfelek vgx.dll.

[Helló, világ](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) minta megtudhatja, hogy miként hajtanak végre különféle gyorsítótárat a [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET ügyfélprogramban.

Ez a példa bemutatja hogyan:

-   Különböző csatlakozási beállítások használata
-   Olvasási és írási objektumok, a és a gyorsítótárból szinkron és aszinkron műveletek használata
-   Vgx.dll MGET/MSET parancsaival kulcsok megadott értéket ad vissza
-   Vgx.dll tranzakció alapú műveletek végrehajtása
-   A listák vgx.dll és a rendezett beállítása
-   .NET-objektumok használata JsonConvert objektumszerializáló tárolása
-   Címkézés végrehajtásához használja a vgx.dll beállítása
-   Vgx.dll fürt használata

További információt a [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) dokumentációja github, és további használatát esetek című [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) egység tesztek.

[Azure vgx.dll gyorsítótár Python való használatáról](cache-python-get-started.md) szemlélteti, hogyan veheti használatba az Azure vgx.dll gyorsítótár Python és a [Vgx.dll másolása](https://github.com/andymccurdy/redis-py) ügyfélprogram használatával.

[A .NET-objektumokkal a gyorsítótárban lévő munka](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) szerializálni a .NET-objektumok, írja be őket, és olvassa el őket az Azure vgx.dll gyorsítótár-példány egyik módja látható. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Gyorsítótár vgx.dll használnak ASP.NET SignalR ki hátlap skálával

A [Vgx.dll gyorsítótár figyelembe használata szerint méretaránnyal ki az ASP.NET SignalR hátlap](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) példa bemutatja, hogyan használhatja a SignalR hátlap Azure vgx.dll gyorsítótár. Hátlap kapcsolatos további tudnivalókért lásd: [A vgx.dll SignalR Scaleout](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Gyorsítótár felhasználói lekérdezés minta vgx.dll

Az alábbi példa bemutatja a összehasonlítja teljesítmény eléréséhez gyorsítótárból és az adatmegőrzési tárhelyről eléréséhez között. Ez a példa két projektet tartalmaz.

-   [Hogyan vgx.dll gyorsítótár javíthatja a teljesítményt, gyorsítótár-adatok demó](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Rendező az adatbázist, és a gyorsítótár a videoklip](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET-munkamenet állapot és a Weblapkimeneti gyorsítótárazás beállításával

A [Használható Azure vgx.dll gyorsítótár ASP.NET SessionState és OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) minta szemlélteti, hogyan ASP.NET-munkamenetet, és a kimeneti gyorsítótár vgx.dll a SessionState és OutputCache szolgáltatók használatának tárolására Azure vgx.dll gyorsítótár segítségével.

## <a name="manage-azure-redis-cache-with-maml"></a>Azure vgx.dll gyorsítótárat MAML kezelése

Az [Azure vgx.dll gyorsítótár kezelése az Azure-kezelés tárak](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) példa bemutatja, hogyan használhatók Azure Management tárak kezelése – (létrehozása és frissítése és törlése) a gyorsítótár. 

## <a name="custom-monitoring-sample"></a>Egyéni minta figyelése

Az [Access vgx.dll gyorsítótár figyelése adatok](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) példa bemutatja, hogyan lehet elérni az ellenőrzési adatokat az Azure vgx.dll gyorsítótár kívül az Azure-portálra.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>PHP és vgx.dll használatával írt Twitter stílusú átirattal

A [Retwis](https://github.com/SyntaxC4-MSFT/retwis) minta a vgx.dll Helló, világ. Érdemes vgx.dll és a [Predis](https://github.com/nrk/predis) ügyfélprogramban PHP használatával írt minimális Twitter stílusú közösségi hálózat átirattal. A forráskód rendkívül egyszerű és más megjelenítése egy időben vgx.dll adatstruktúrák lett tervezve.

## <a name="bandwidth-monitor"></a>A sávszélesség monitor

A [sávszélesség monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) minta lehetővé teszi, hogy figyelheti a sávszélesség az ügyfél által használt. Mérje le a sávszélesség, futtassa a minta a gyorsítótár ügyfélgépen, hogy hívásokat kezdeményezzenek a gyorsítótár és tekintse át a sávszélesség a sávszélesség monitor minta által jelzett.
