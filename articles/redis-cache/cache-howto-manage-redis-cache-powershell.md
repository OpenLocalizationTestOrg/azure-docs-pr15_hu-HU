<properties
    pageTitle="Azure vgx.dll gyorsítótár Azure PowerShell kezelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként felügyeleti feladatok Azure vgx.dll gyorsítótár Azure PowerShell használatával."
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

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Azure vgx.dll gyorsítótár Azure PowerShell kezelése

> [AZURE.SELECTOR]
- [A PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Ez a témakör azt szemlélteti, hogyan végezze el a gyakori feladatok például létrehozása, frissítése, és méretezni a Azure vgx.dll gyorsítótár-példányok, hogy miként hívóbetűk újbóli, illetve a gyorsítótárát adatainak megtekintéséhez. Azure vgx.dll gyorsítótár PowerShell-parancsmagok listáját olvassa el az [Azure vgx.dll gyorsítótár-parancsmagok](https://msdn.microsoft.com/library/azure/mt634513.aspx)című témakört.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus modell](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Előfeltételek

Ha már telepítette az Azure PowerShell, rendelkeznie kell Azure PowerShell verzió 1.0.0 vagy újabb verziójában. Ellenőrizheti, hogy az Azure PowerShell ezzel a paranccsal a Azure PowerShell parancssorablakban telepített verziója.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Először be kell jelentkezni Azure ezzel a paranccsal.

    Login-AzureRmAccount

A Microsoft Azure bejelentkezési párbeszédpanelen adja meg, az Azure-fiók és a jelszó annak az e-mail címét.

Ezután ha Azure több előfizetéssel rendelkezik, kell beállítania az Azure előfizetés. Az aktuális előfizetések listáját, futtassa a parancsot.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Adja meg az előfizetést, futtassa a következő parancsot. A következő példában az előfizetés neve van `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Mielőtt használatba vehetné a Windows PowerShell Azure erőforrás-kezelővel, az alábbiakra van szükség:

- A Windows PowerShell, 3.0-s vagy 4.0-s verziója. Keresse meg a Windows PowerShell verziója, írja be a következőt:`$PSVersionTable` , és ellenőrizze a értékének `PSVersion` 3.0-s vagy 4.0. Kompatibilis verzió telepítéséhez olvassa el a [Windows Management Framework 3.0-s](http://www.microsoft.com/download/details.aspx?id=34595) vagy a [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)című témakört.

Részletes bármely parancsmag ebben az oktatóanyagban megjelenik a, használja a Get-Help parancsmag.

    Get-Help <cmdlet-name> -Detailed

Segítség kérése a példában az a `New-AzureRmRedisCache` parancsmag, írja be:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Azure Government Cloud vagy Azure Kína felhő keresztüli

Az Azure alapértelmezés szerint a környezete `AzureCloud`, amely jelenti, hogy a globális Azure felhő példányt. Szeretne kapcsolódni egy másik példányát, használja a `Add-AzureRmAccount` parancsot a a `-Environment` vagy -`EnvironmentName` parancssori kapcsoló a kívánt környezet vagy a környezet nevét.

A rendelkezésre álló környezetekben listájának megjelenítéséhez, futtassa a `Get-AzureRmEnvironment` parancsmag.

### <a name="to-connect-to-the-azure-government-cloud"></a>Az Azure kormányzati felhőben való kapcsolódáshoz

Az Azure Government Cloud csatlakozik, használja az alábbi parancsok egyikére.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

vagy

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

A gyorsítótár létrehozása az Azure kormányzati felhőben, használja az alábbi helyek közül.

-   USGov Virginia
-   USGov Iowa

Az Azure Government Cloud kapcsolatos további tudnivalókért olvassa el a [Microsoft Azure kormányzati szerveknek](https://azure.microsoft.com/features/gov/) és a [Microsoft Azure kormányzati Fejlesztőeszközök útmutató](../azure-government-developer-guide.md)című témakört.

### <a name="to-connect-to-the-azure-china-cloud"></a>Az Azure Kína felhőben való kapcsolódáshoz

Az Azure Kína felhő csatlakozik, használja az alábbi parancsok egyikére.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

vagy

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

A gyorsítótár létrehozása az Azure Kína felhőben, használja az alábbi helyek közül.

-   Kína keleti
-   Kína Észak

Az Azure Kína felhő kapcsolatos további tudnivalókért olvassa el a [AzureChinaCloud az Azure Kínában a 21Vianet által üzemeltetett](http://www.windowsazure.cn/)című témakört.

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Azure-gyorsítótár PowerShell vgx.dll használt tulajdonságai

Az alábbi táblázat tulajdonságokat és a gyakran használt paraméterek létrehozása és kezelése az Azure vgx.dll gyorsítótár-példányok Azure PowerShell használatával leírását tartalmazza.

| Paraméter          | Leírás                                                                                                                                                                                                        | Alapértelmezett  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| név               | A gyorsítótár neve                                                                                                                                                                                                  |          |
| Hely           | A gyorsítótár helye                                                                                                                                                                                              |          |
| ResourceGroupName  | Erőforrás csoport nevére, amelyben létre szeretné hozni a gyorsítótárban lévő                                                                                                                                                                   |          |
| Mérete               | A gyorsítótár méretét. Érvényes értékek a következők: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250 MB-os, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Hozhat létre egy prémium gyorsítótár fürtözés engedélyezve van a létrehozásakor shards száma. Érvényes érték: 1, 2, 3, 4, 5, 6, 7, 8, 9 vagy 10                                                                                                      |          |
| RAKTÁRI SZÁM                | Adja meg a Termékváltozat a gyorsítótár. Érvényes értékek a következők: egyszerű, normál, Premium                                                                                                                                         | Normál |
| RedisConfiguration | Adja meg a beállításokat vgx.dll. Az egyes beállítások részleteit az alábbi [RedisConfiguration tulajdonságok](#redisconfiguration-properties) táblázat megtekintése |          |
| EnableNonSslPort   | Azt jelzi, hogy engedélyezve van-e az-SSL port.                                                                                                                                                                     | Hamis    |
| MaxMemoryPolicy    | Ez a paraméter elavult – RedisConfiguration használja helyette.                                                                                                                                              |          |
| StaticIP           | A gyorsítótár egy VNET szolgáltatónál, ha a gyorsítótár a alhálózat adja meg egy egyedi IP-címet. Ha nincs megadva, egy választja ki a alhálózat.                                                                                                                     |          |
| Alhálózat             | A gyorsítótár egy VNET szolgáltatónál, ha a neve, amelyben a gyorsítótár üzembe alhálózat.                                                                                                                  |          |
| VirtualNetwork     | A gyorsítótár egy VNET szolgáltatónál, ha megadja a VNET, amelyben a gyorsítótár terjesztése az erőforrás-azonosító.                                                                                                             |          |
| KeyType            | Itt adhatja meg, mely hívóbetű hívóbetűk megújításakor újbóli. Érvényes értékek a következők: elsődleges, másodlagos |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>RedisConfiguration tulajdonságai

| A tulajdonság                      | Leírás                                                                                                          | Réteg árak       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| rekordadatbázis biztonsági mentés engedélyezve            | [Adatok állandó vgx.dll](cache-how-to-premium-persistence.md) engedélyezve van-e                                     | Csak a prémium verzió        |
| rekordadatbázis – tárterület-kapcsolat-karakterlánc | A kapcsolati karakterlánc [adatok adatmegőrzési vgx.dll](cache-how-to-premium-persistence.md) tároló fiókjában       | Csak a prémium verzió        |
| rekordadatbázis-biztonságimásolat-gyakorisága          | Az [adatok adatmegőrzési vgx.dll](cache-how-to-premium-persistence.md) biztonsági gyakorisága                               | Csak a prémium verzió        |
| fenntartott maxmemory            | A [foglalt memóriát](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) nem gyorsítótár folyamatok konfigurálása | Normál és Premium |
| maxmemory-házirend              | A gyorsítótár [eviction házirend](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) beállítása           | Az összes árak rétegek   |
| Ha értesítést keyspace-események        | [Keyspace értesítések](cache-configure.md#keyspace-notifications-advanced-settings) konfigurálása                     | Normál és Premium |
| kivonat-max-ziplist-bejegyzések      | [A memória optimalizálása](http://redis.io/topics/memory-optimization) kis összesítő adattípusokhoz konfigurálása          | Normál és Premium |
| kivonat-max-ziplist – érték        | [A memória optimalizálása](http://redis.io/topics/memory-optimization) kis összesítő adattípusokhoz konfigurálása          | Normál és Premium |
| set-max-intset-bejegyzések        | [A memória optimalizálása](http://redis.io/topics/memory-optimization) kis összesítő adattípusokhoz konfigurálása          | Normál és Premium |
| zset-max-ziplist-bejegyzések      | [A memória optimalizálása](http://redis.io/topics/memory-optimization) kis összesítő adattípusokhoz konfigurálása          | Normál és Premium |
| zset-max-ziplist – érték        | [A memória optimalizálása](http://redis.io/topics/memory-optimization) kis összesítő adattípusokhoz konfigurálása          | Normál és Premium |
| adatbázisok                     | Adja meg az adatbázisok száma. Ezt a tulajdonságot csak a gyorsítótár létrehozási beállíthatók.                          | Normál és Premium |

## <a name="to-create-a-redis-cache"></a>Egy vgx.dll gyorsítótár létrehozása

Új Azure vgx.dll gyorsítótár-példányok jönnek létre az [Új-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag használatával.

>[AZURE.IMPORTANT] Az első alkalommal az Azure portálon előfizetés hoz létre egy vgx.dll gyorsítótárat a portál és a `Microsoft.Cache` névtér az adott előfizetés. Ha az első vgx.dll gyorsítótár létrehozása a PowerShell használatá előfizetést próbál, regisztrálnia kell, hogy az alábbi paranccsal; névtere olyan, különben a program parancsmagok `New-AzureRmRedisCache` és `Get-AzureRmRedisCache` sikertelen lesz.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `New-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Hozzon létre egy gyorsítótár alapértelmezett paraméterekkel, futtassa a következő parancsot.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, és `Location` szükséges paramétereket, de a többi nem kötelező, és van alapértelmezett értéke. Az előző parancs futtatása hoz létre egy szokásos Termékváltozat Azure vgx.dll gyorsítótár-példány a megadott névvel, helyét és erőforráscsoport, amely 1 GB méretű az-SSL port le van tiltva a.

Hozzon létre egy prémium gyorsítótár, adja meg a méretet P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), vagy P4 (53 GB - 530 GB). Ahhoz, hogy fürtözés, adja meg a shard darab használata a `ShardCount` paraméter. Az alábbi példa létrehoz egy P1 prémium gyorsítótár 3 shards. Egy P1 prémium gyorsítótár 6 GB méretű, és mivel a megadott három shards azt a teljes méret-e a 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Értékek megadása a `RedisConfiguration` paraméter belül az érték `{}` kulcs/érték párokká például `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Az alábbi példa létrehoz egy szabványos 1 GB gyorsítótárat a `allkeys-random` maxmemory házirend keyspace értesítések és konfigurált `KEA`. További tudnivalókért lásd: [Keyspace értesítések (Speciális beállítások)](cache-configure.md#keyspace-notifications-advanced-settings) és [Maxmemory-házirend és maxmemory fenntartott](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>A beállítás alatt gyorsítótárának létrehozása adatbázis konfigurálása

A `databases` beállítást csak a gyorsítótár létrehozása során beállíthatók. Az alábbi példa létrehoz egy prémium P3 (26 GB) gyorsítótárat a [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmaggal 48 adatbázisokkal.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

További információt a `databases` tulajdonság, lásd: [Azure vgx.dll gyorsítótár alapértelmezett kiszolgáló konfigurálása](cache-configure.md#default-redis-server-configuration). A [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmaggal gyorsítótárat létrehozásával kapcsolatos további tudnivalókért lásd: az előző [Vgx.dll gyorsítótár létre](#to-create-a-redis-cache) szakaszban.

## <a name="to-update-a-redis-cache"></a>Egy vgx.dll gyorsítótár frissítése

Azure vgx.dll gyorsítótár-példányok frissülnek a [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) parancsmaggal.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `Set-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

A `Set-AzureRmRedisCache` parancsmag használható például tulajdonság frissítése `Size`, `Sku`, `EnableNonSslPort`, és a `RedisConfiguration` értékeket. 

A következő parancsot a maxmemory házirend myCache nevű vgx.dll gyorsítótár frissítéseket.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Ha át kívánja méretezni vgx.dll gyorsítótárban

`Set-AzureRmRedisCache`is használható, ha át kívánja méretezni az Azure vgx.dll gyorsítótár példány, amikor a `Size`, `Sku`, vagy `ShardCount` tulajdonságok módosulnak. 

>[AZURE.NOTE]A PowerShell használatá gyorsítótár méretezés van érvényes azonos korlátai és útmutatók az Azure portálról gyorsítótárat méretezés. A következő korlátozásokkal egy másik árak réteg méretezheti.
>
>-  Egy alsó árak réteg szeretné méretezni nem egy újabb árak réteg a.
>    -    Nem lehet méretezni, le egy **szabványos** **prémium** gyorsítótárat vagy egy **egyszerű** gyorsítótárból.
>    -    Nem lehet méretezni, le egy **egyszerű** gyorsítótár **szabványos** gyorsítótárból.
>-  Egy **egyszerű** gyorsítótárból a **szabványos** gyorsítótárat méretezheti, de egyszerre nem módosíthatja a méretét. A különböző méretű van szüksége, ha műveleteket hajthat végre a következő méretezési műveletre a megfelelő méretre.
>-  Közvetlenül az **prémium** gyorsítótárat méretezni nem egy **egyszerű** gyorsítótárból. Meg kell méretezni **egyszerű** **szabványos** méretezési műveletben, majd a **szabványos** **prémium** későbbi méretezési művelet.
>-  A lefelé nagyobb méretű nem méretezni a **C0 (250 MB)** méretét.
>
>További információért tájékozódhat [skála Azure vgx.dll gyorsítótár](cache-how-to-scale.md).

A következő példa bemutatja, hogyan méretezheti nevű gyorsítótárat `myCache` 2,5 GB-gyorsítótár. Figyelje meg, hogy működik-e ez a parancs az alap- vagy egy szabványos gyorsítótárat.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Ez a parancs kibocsátott, miután a gyorsítótár állapotának ad vissza (hívó hasonló `Get-AzureRmRedisCache`). Megjegyzendő, hogy a `ProvisioningState` van `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Amikor a méretezési művelet befejeződött, a `ProvisioningState` módosításaira `Succeeded`. Ha szükséges, a következő méretezési műveletre, például módosítása egyszerű szabványos, és ezután módosíthatja a méretét, hogy meg kell várnia, amíg az előző művelet befejeződött, vagy az alábbihoz hasonló hibaüzenetet kap.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Ha egy vgx.dll gyorsítótár információkra

A [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) parancsmaggal gyorsítótárat információt meghallgathatja.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `Get-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Az összes gyorsítótárát információt vissza az aktuális előfizetés, futtassa a `Get-AzureRmRedisCache` paraméter nélkül.

    Get-AzureRmRedisCache

Minden gyorsítótárát információt vissza az adott erőforrás csoport, futtassa a `Get-AzureRmRedisCache` együtt a `ResourceGroupName` paraméter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Egy adott gyorsítótár kapcsolatos adatok visszaadására, futtassa a `Get-AzureRmRedisCache` a a `Name` a gyorsítótárát, nevét tartalmazó paraméter és a `ResourceGroupName` az adott gyorsítótár tartalmazó erőforráscsoport paramétert.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>A hívóbetűk vgx.dll gyorsítótárhoz beolvasása

A hívóbetűk a gyorsítótár megszerezni a [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) parancsmag is használhatja.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `Get-AzureRmRedisCacheKey`, futtassa a következő parancsot.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

A gyorsítótár billentyűparancsok beolvasásához, hívja fel a `Get-AzureRmRedisCacheKey` parancsmag adják a gyorsítótár neve, amely tartalmazza a gyorsítótár erőforráscsoport a nevét.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>A vgx.dll gyorsítótár hívóbetűk újbóli

A hívóbetűk a gyorsítótár újragenerálása, a [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) parancsmag is használhatja.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `New-AzureRmRedisCacheKey`, futtassa a következő parancsot.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Az elsődleges és másodlagos kulcsa a gyorsítótár újbóli, hívja fel a `New-AzureRmRedisCacheKey` parancsmag adják át a nevében, erőforráscsoport, és adja meg vagy `Primary` vagy `Secondary` az a `KeyType` paraméter. A következő példában a másodlagos hívóbetű a gyorsítótár jön.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Egy vgx.dll gyorsítótár törlése

Egy vgx.dll gyorsítótár törléséhez használja a [Eltávolítása-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) parancsmag.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `Remove-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

A következő példában a gyorsítótár nevű `myCache` törlődik.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Importálhat egy vgx.dll gyorsítótár

Adatok importálása az Azure vgx.dll gyorsítótár példány használata a `Import-AzureRmRedisCache` parancsmag.

>[AZURE.IMPORTANT] Az importálás/exportálás [prémium réteg](cache-premium-tier-intro.md) gyorsítótárát csak érhető el. Az importálás/exportálás kapcsolatos további tudnivalókért olvassa el az [Azure vgx.dll gyorsítótárban lévő adatok importálása és exportálása](cache-how-to-import-export-data.md)című témakört.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `Import-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

A következő parancsot a blob Azure vgx.dll gyorsítótárba a Társítások uri által megadott adatok importálása.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Egy vgx.dll gyorsítótár exportálása

Adatok exportálása az Azure vgx.dll gyorsítótár példány használata a `Export-AzureRmRedisCache` parancsmag.

>[AZURE.IMPORTANT] Az importálás/exportálás [prémium réteg](cache-premium-tier-intro.md) gyorsítótárát csak érhető el. Az importálás/exportálás kapcsolatos további tudnivalókért olvassa el az [Azure vgx.dll gyorsítótárban lévő adatok importálása és exportálása](cache-how-to-import-export-data.md)című témakört.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `Export-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


A következő parancsot a Társítások uri által megadott tároló be egy Azure vgx.dll gyorsítótár-példányával exportálja az adatokat.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Indítsa újra a vgx.dll gyorsítótárban való

Az Azure vgx.dll gyorsítótár példány használt újraindítja az `Reset-AzureRmRedisCache` parancsmag.

>[AZURE.IMPORTANT] Indítsa újra a rendszert a [prémium réteg](cache-premium-tier-intro.md) gyorsítótárát csak érhető el. A gyorsítótár újraindítása kapcsolatos további tudnivalókért olvassa el a [gyorsítótár felügyelete – indítsa újra](cache-administration.md#reboot)című témakört.

A rendelkezésre álló paramétereket és azok leírását listájának megjelenítéséhez `Reset-AzureRmRedisCache`, futtassa a következő parancsot.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

A következő parancsot a megadott gyorsítótár mindkét csomópontok újraindítja.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Következő lépések

További tudnivalók a Windows PowerShell használata az Azure, az alábbi forrásokban talál:

- [Azure vgx.dll gyorsítótár parancsmag dokumentáció MSDN webhelyen](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure erőforrás-kezelő parancsmagok](http://go.microsoft.com/fwlink/?LinkID=394765): További tudnivalók a parancsmagok használatához a AzureResourceManager modulban.
- [Az erőforrás használatával csoportok az Azure erőforrások kezelése](../resource-group-template-deploy-portal.md): megtudhatja, hogy miként hozhat létre és kezelhet az erőforrás-csoportok az Azure-portálon.
- [Azure blog](http://blogs.msdn.com/windowsazure): Azure az új funkciókról.
- [A Windows PowerShell-blog](http://blogs.msdn.com/powershell): a Windows PowerShell új funkciókról.
- ["Fontosnak, parancsfájlok változásairól!" Blog](http://blogs.technet.com/b/heyscriptingguy/): valós életből tippeket és trükköket beolvasása a Windows PowerShell-Közösség.
