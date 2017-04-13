<properties
   pageTitle="Azure vgx.dll gyorsítótár használata Java |} Microsoft Azure"
    description="Azure vgx.dll gyorsítótár Java használata – első lépések"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Java Azure vgx.dll gyorsítótár használata

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [NODE.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure vgx.dll gyorsítótár elérheti a egy dedikált vgx.dll gyorsítótár, a Microsoft kezeli. A gyorsítótár minden olyan alkalmazásból belül a Microsoft Azure érhető el.

Ez a témakör bemutatja, hogyan veheti használatba az Azure vgx.dll gyorsítótár Java használatával.

## <a name="prerequisites"></a>Előfeltételek

[Jedis](https://github.com/xetorthio/jedis) - Java-ügyfélprogram vgx.dll

Ebben az oktatóanyagban Jedis használ, de [http://redis.io/clients](http://redis.io/clients)felsorolt Java jegyzetlapon is használhatja.

## <a name="create-a-redis-cache-on-azure"></a>Azure vgx.dll gyorsítótárat létrehozása

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>A host nevét és az access billentyűk beolvasása

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Az-SSL végpontot engedélyezése

Egyes vgx.dll ügyfelek nem támogatja az SSL, és alapértelmezés szerint a [-SSL port új Azure vgx.dll gyorsítótár-példányok le van tiltva](cache-configure.md#access-ports). Az írás időben a [Jedis](https://github.com/xetorthio/jedis) ügyfél SSL nem támogatja. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>A gyorsítótár venni valamit, és megtalálja

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Következő lépések

- A [gyorsítótár diagnosztika engedélyezése](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) , így [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) a gyorsítótár állapotának.
- Olvassa el a [dokumentáció vgx.dll](http://redis.io/documentation)hivatalos.

