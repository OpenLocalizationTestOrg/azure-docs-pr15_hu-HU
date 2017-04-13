<properties
    pageTitle="Azure vgx.dll gyorsítótár használata Node.js |} Microsoft Azure"
    description="Első lépések az Azure vgx.dll gyorsítótár Node.js és node_redis használatával."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Azure vgx.dll gyorsítótár használata Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [NODE.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure vgx.dll gyorsítótár férhet hozzá egy biztonságos, célorientált vgx.dll gyorsítótár, a Microsoft kezeli. A gyorsítótár minden olyan alkalmazásból belül a Microsoft Azure érhető el.

Ez a témakör bemutatja, hogyan veheti használatba az Azure vgx.dll gyorsítótár Node.js használatával. Egy másik példa az Azure vgx.dll gyorsítótár használata Node.js című témakörben [egy Socket.IO az Azure webhelyen Node.js Csevegés alkalmazás összeállítása](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Előfeltételek

[Node_redis](https://github.com/mranney/node_redis)telepítése:

    npm install redis

Ebben az oktatóanyagban [node_redis](https://github.com/mranney/node_redis)használja. Példák más Node.js ügyfelek dokumentációjában olvasható az egyes [ügyfelek Node.js vgx.dll](http://redis.io/clients#nodejs)felsorolt Node.js ügyfelei számára.

## <a name="create-a-redis-cache-on-azure"></a>Azure vgx.dll gyorsítótárat létrehozása

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>A host nevét és az access billentyűk beolvasása

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Csatlakozás a gyorsítótár biztonságosan az SSL használatával

[Node_redis](https://github.com/mranney/node_redis) legújabb verziói támogatnak csatlakoztatása az Azure vgx.dll gyorsítótár SSL-Kapcsolatot használ. A következő példa bemutatja az Azure vgx.dll gyorsítótár keresztüli 6380 végpontját az SSL használatával. Csere `<name>` a gyorsítótárat a nevet és `<key>` vagy az elsődleges és másodlagos kulcs megjelölése az előző [letölteni a szolgáltató neve és az access kulcsokat](#retrieve-the-host-name-and-access-keys) szakaszban ismertetett.

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>A gyorsítótár venni valamit, és megtalálja

A következő példa bemutatja, hogyan az Azure vgx.dll gyorsítótár példányához és tárolni, valamint egy elemet a gyorsítótárból. További példákat vgx.dll használata a [node_redis](https://github.com/mranney/node_redis) ügyfél olvassa el a [http://redis.js.org/](http://redis.js.org/)című témakört.

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Kimenet:

    OK
    value


## <a name="next-steps"></a>Következő lépések

- A [gyorsítótár diagnosztika engedélyezése](cache-how-to-monitor.md#enable-cache-diagnostics) , így [monitor](cache-how-to-monitor.md) a gyorsítótár állapotának.
- Olvassa el a [dokumentáció vgx.dll](http://redis.io/documentation)hivatalos.



