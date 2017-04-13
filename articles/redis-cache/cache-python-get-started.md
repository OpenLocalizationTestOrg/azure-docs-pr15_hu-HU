<properties
    pageTitle="Azure vgx.dll gyorsítótár használata Python |} Microsoft Azure"
    description="Azure vgx.dll gyorsítótár Python használata – első lépések"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Azure vgx.dll gyorsítótár használata Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [NODE.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Ez a témakör bemutatja, hogyan veheti használatba az Azure vgx.dll gyorsítótár Python használatával.


## <a name="prerequisites"></a>Előfeltételek

Telepítse a [Vgx.dll másolása](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Azure vgx.dll gyorsítótárat létrehozása

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>A host nevét és az access billentyűk beolvasása

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>Az-SSL végpontot engedélyezése

Egyes vgx.dll ügyfelek nem támogatja az SSL, és alapértelmezés szerint a [-SSL port új Azure vgx.dll gyorsítótár-példányok le van tiltva](cache-configure.md#access-ports). Az írás időben a [Vgx.dll másolása](https://github.com/andymccurdy/redis-py) ügyfél SSL nem támogatja. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>A gyorsítótár venni valamit, és megtalálja


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Csere `<name>` a gyorsítótár nevű és `key` az access használatával.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
