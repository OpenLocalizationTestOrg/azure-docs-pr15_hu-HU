<properties 
    pageTitle="Hogyan hozhat létre és kezelhet az Azure parancssori kezelőfelületről Azure használatával Azure vgx.dll gyorsítótár |} Microsoft Azure" 
    description="Megtudhatja, hogyan telepítheti az Azure CLI bármely platformon használatához az Azure fiókhoz való csatlakozáshoz és létrehozása és kezelése az Azure CLI vgx.dll gyorsítótárat." 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Hogyan hozhat létre, és használja az Azure parancssori kezelőfelületről Azure Azure vgx.dll gyorsítótár kezelése

> [AZURE.SELECTOR]
- [A PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Az Azure CLI kezelheti az Azure-infrastruktúrát bármely platform kiválóan használhatók. Ez a cikk bemutatja, hogyan hozhat létre és kezelhet az Azure vgx.dll gyorsítótár-példányok az Azure CLI használatával.

## <a name="prerequisites"></a>Előfeltételek

Szeretne létrehozni, és Azure vgx.dll gyorsítótár-példányok Azure CLI használatával kezelheti, az alábbi lépéseket kell elvégeznie.

-   Azure fiókkal kell rendelkeznie. Ha nincs telepítve egyik, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.
-   [Telepítse az Azure CLI](../xplat-cli-install.md).
-   Az Azure CLI telepítése kapcsolódni, személyes Azure-fiókkal, vagy egy munkahelyi vagy iskolai Azure-fiók, és jelentkezzen be az Azure CLI használja a `azure login` parancsot. Megérteni a különbségeket, és válassza ki, hogy olvassa el a [Csatlakozás a az Azure parancssori kezelőfelületről Azure Azure-előfizetésbe](../xplat-cli-connect.md)elemet.
-   Mielőtt operációs rendszert futtató, az alábbi parancsok egyikét, váltson az Azure CLI erőforrás-kezelő módba futtatásával a `azure config mode arm` parancsot. További tudnivalókért olvassa el a [Azure erőforrás-kezelő módjának beállítása](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode)című témakört.

## <a name="redis-cache-properties"></a>Vgx.dll gyorsítótár tulajdonságai

A következő tulajdonságokat használatosak létrehozásakor és vgx.dll gyorsítótár-példány frissítésekor.

| A tulajdonság            | Váltás                                  | Leírás                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| név                | -n, – neve                              | A vgx.dll gyorsítótár neve.                                                                                                                                                                                                                             |
| Erőforráscsoport      | -g, – erőforrás-csoport                    | A csoport nevét, az erőforrás.                                                                                                                                                                                                                          |
| hely            | -l, – helyre                          | Hely gyorsítótár létrehozásához.                                                                                                                                                                                                                            |
| mérete                | -z, – mérete                              | A vgx.dll gyorsítótár méretét. Az érvényes értékek: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| Raktári szám                 | x – – termékváltozat                               | Termékváltozat vgx.dll. Valamelyikét kell megadni: [egyszerű, normál, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e, – engedélyezése – nem-ssl-port               | A vgx.dll gyorsítótár EnableNonSslPort tulajdonsága. Ahhoz, hogy a gyorsítótár nem SSL-Port igény szerint adja hozzá a jelző                                                                                                                                    |
| Konfigurációs vgx.dll | -c, – vgx.dll konfigurálása               | Konfigurációs vgx.dll. Írja be a formázott JSON karakterlánc konfigurációs kulcsok és az alábbi értékeket. Formátum: "{" ":""," ":" "}"                                                                                                                                     |
| Konfigurációs vgx.dll | az f-, – vgx.dll konfigurációs-fájl          | Konfigurációs vgx.dll. Írja be a konfigurációs kulcsok és az alábbi értékeket tartalmazó fájl elérési útját. A fájl neve formátuma: {"": "","": ""}                                                                                                                |
| Shard száma         | -r, – shard száma                       | Létrehozni a fürtözés prémium fürt gyorsítótárat a Shards száma.                                                                                                                                                                               |
| Virtuális hálózati     | -v szolgáltatást úgy, – ezek olyan virtuális hálózati                   | A gyorsítótár egy VNET szolgáltatónál, itt adhatja meg a pontos ARM erőforrás-azonosító virtuális hálózat bevezetését tervezi a vgx.dll gyorsítótár kiürítése. Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| kulcs típusa            | -t, – kulcs-típus                          | Kulcs megújítása típusát. Az érvényes értékek: [elsődleges, másodlagos]                                                                                                                                                                                             |
| StaticIP            | -p, – statikus ip-< ip-statikus >             | A gyorsítótár egy VNET szolgáltatónál, ha a gyorsítótár a alhálózat adja meg egy egyedi IP-címet. Ha nincs megadva, egy választja ki a alhálózat.                                                                                                |
| Alhálózat              | t, – alhálózat<subnet>                    | A gyorsítótár egy VNET szolgáltatónál, ha a neve, amelyben a gyorsítótár üzembe alhálózat.                                                                                                                                                    |
| VirtualNetwork      | -v szolgáltatást úgy, – ezek olyan virtuális-< virtuális hálózatok > | A gyorsítótár egy VNET szolgáltatónál, itt adhatja meg a pontos ARM erőforrás-azonosító virtuális hálózat bevezetését tervezi a vgx.dll gyorsítótár kiürítése. Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Előfizetés        | -s, – előfizetés                      | Előfizetés azonosítója.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Lásd: minden vgx.dll gyorsítótár parancs

Minden vgx.dll gyorsítótár parancs és a paraméterek megjelenítéséhez használja a `azure rediscache -h` parancsot.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Hozzon létre egy vgx.dll gyorsítótár

Hozzon létre egy vgx.dll gyorsítótár, használja az alábbi parancsot:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

További információt a Ez a parancs futtatása a `azure rediscache create -h` parancsot.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Egy meglévő vgx.dll gyorsítótár kiürítése

Egy vgx.dll gyorsítótár törléséhez használja a következő parancsot:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

További információt a Ez a parancs futtatása a `azure rediscache delete -h` parancsot.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>A lista összes vgx.dll gyorsítótárát az előfizetést vagy erőforráscsoport belül

A lista összes vgx.dll gyorsítótárát az előfizetést vagy erőforráscsoport belül, használja az alábbi parancsot:

    azure rediscache list [options]

További információt a Ez a parancs futtatása a `azure rediscache list -h` parancsot.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Egy meglévő vgx.dll gyorsítótár tulajdonságainak megjelenítése

Egy meglévő vgx.dll gyorsítótár tulajdonságainak megjelenítéséhez használja a következő parancsot:

    azure rediscache show [--name <name> --resource-group <resource-group>]

További információt a Ez a parancs futtatása a `azure rediscache show -h` parancsot.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Egy meglévő vgx.dll gyorsítótár beállításainak módosítása

Egy meglévő vgx.dll gyorsítótár beállításainak módosításához használja a következő parancsot:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

További információt a Ez a parancs futtatása a `azure rediscache set -h` parancsot.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Egy meglévő vgx.dll gyorsítótárat a hitelesítési kulcs megújítása

Egy meglévő vgx.dll gyorsítótárat a hitelesítési kulcs megújításához használja az alábbi parancsot:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Adja meg, `Primary` vagy `Secondary` az `key-type`.

További információt a Ez a parancs futtatása a `azure rediscache renew-key -h` parancsot.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Egy meglévő vgx.dll gyorsítótár lista elsődleges és másodlagos kulcsok

Lista elsődleges és másodlagos hívóbetűk egy meglévő vgx.dll gyorsítótár használja az alábbi parancsot:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

További információt a Ez a parancs futtatása a `azure rediscache list-keys -h` parancsot.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
