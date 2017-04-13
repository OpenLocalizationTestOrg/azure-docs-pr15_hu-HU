
<properties
   pageTitle="Hozzon létre egy teljes Linux környezet használatával az Azure CLI |} Microsoft Azure"
   description="Tárhely, egy Linux virtuális, egy virtuális hálózati és alhálózat, egy terheléselosztó, egy hálózati, egy nyilvános IP és hálózati biztonsági csoport, mind az Azure CLI használatával alapoktól létrehozása."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Egy teljes Linux környezet létrehozása az Azure CLI használatával

Ebben a cikkben egy egyszerű hálózati egy terheléselosztó és két VMs, amelyek hasznos fejlesztés és egyszerű számítások összeállítása azt. Végigvezetjük a folyamat parancs parancs, amíg el nem érte két dolgozik, biztonságos Linux VMs, amelyhez csatlakoztathatja a bárhol az interneten. Ezután áthelyezhetők összetettebb hálózatokhoz és a környezetben.

Menet, megismerheti a függőség hierarchia, hogy az erőforrás-kezelő telepítési modell lépve, és a mennyi információ a power azt. Követően a rendszer beépített hogyan jelenik meg, akkor gyorsan sokkal több használható [Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)használatával. Is megtudhatja, hogyan a környezet részei illeszkedjenek egymáshoz, automatizálhatja a őket-sablonok létrehozása lesz könnyebben.

A környezet tartalmazza:

- Két VMs belül egy elérhetőségének beállítása.
- A 80-as port terheléselosztási szabályok terheléselosztó.
- Biztonsági csoport (NSG) szabályok a virtuális megvédését a nemkívánatos forgalmat a hálózati.

![Egyszerű környezet áttekintése](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Ez az egyéni környezeti létrehozásához szükséges a legújabb [Azure CLI](../xplat-cli-install.md) erőforrás-kezelő módban (`azure config mode arm`). A JSON-elemzési eszköz is szükséges. Ez a példa [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Gyors parancsok
Ha gyorsan teljesítenie a tevékenységhez, az alábbiakat szakaszt a következő parancsokat tartalmaz egy virtuális feltöltendő Azure. További részletes információkat és a helyi minden egyes lépés a dokumentumot, a többi program kezdő [Itt](#detailed-walkthrough).

Győződjön meg arról, hogy rendelkezik-e jelentkezve, és erőforrás-kezelő üzemmód használata [Az Azure CLI](../xplat-cli-install.md) :

```bash
azure config mode arm
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméternevek olyan `myResourceGroup`, `mystorageaccount`, és `myVM`.

Az erőforráscsoport létrehozása. Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `westeurope` helyét:

```bash
azure group create -n myResourceGroup -l westeurope
```

Az erőforráscsoport ellenőrizze a JSON-elemzővel használatával:

```bash
azure group show myResourceGroup --json | jq '.'
```

Hozza létre a tárterület-fiókot. Az alábbi példa létrehoz egy tároló nevű fiókot `mystorageaccount` (a tárhely fióknév kell lennie egyedi, így a saját egyedi nevet rendelkeznek):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

A tárterület-fiók ellenőrzése a JSON-elemzővel használatával:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

A virtuális hálózat létrehozása. Az alábbi példa létrehoz egy virtuális hálózati nevű `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Hozzon létre egy alhálózat. Az alábbi példa létrehoz egy névvel ellátott alhálózat `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

A JSON-elemzővel használatával hitelesítse a virtuális hálózati és:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Hozzon létre egy nyilvános IP. Az alábbi példa létrehoz egy nyilvános IP-nevű `myPublicIP` DNS nevű `mypublicdns` (a tartománynév kell lennie egyedi, így a saját egyedi nevet rendelkeznek):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Hozzon létre a terheléselosztó. Az alábbi példa létrehoz egy névvel ellátott terheléselosztó `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

A terheléselosztó az előtér-IP-készlet létrehozása és hozzárendelése a nyilvános IP. Az alábbi példa létrehoz egy előtér-IP-készlet nevű `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

A terheléselosztó a háttéradatbázist IP-készlet létrehozása. Az alábbi példa létrehoz egy névvel ellátott háttéradatbázist IP-készlet `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Hozzon létre SSH bejövő hálózati a terheléselosztó címfordító szabályok címet. Az alábbi példa létrehoz két betöltés terheléselosztó szabály `myLoadBalancerRuleSSH1` és `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

A webhely létrehozása a terheléselosztó hálózati Címfordítást szabályairól bejövő. Az alábbi példa létrehoz nevű betöltés terheléselosztó szabály`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

A betöltés terheléselosztó állapot vizsgálati létrehozása. Az alábbi példa létrehoz egy névvel ellátott TCP-vizsgálati `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

A JSON-elemzővel használatával hitelesítse a terheléselosztó, IP-készletek és hálózati Címfordítást szabályok:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Az első hálózati kapcsolat kártya létrehozása. Cserélje le a `#####-###-###` szakaszok a saját Azure előfizetés azonosítójával. Az előfizetés azonosítója kell a kimeneti jegyezni `jq` hoz létre az erőforrások vizsgálatakor. Is megtekintheti és az előfizetés azonosítója `azure account list`. 

Az alábbi példa létrehoz egy hálózati kártya elnevezett `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

A második adaptert létrehozása Az alábbi példa létrehoz egy hálózati kártya elnevezett `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

A két kártyát ellenőrizze a JSON-elemzővel használatával:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

A hálózati biztonsági csoport létrehozása Az alábbi példa létrehoz egy hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Két a bejövő szabályok a hálózati biztonsági csoport hozzáadása Az alábbi példa létrehoz két szabályt `myNetworkSecurityGroupRuleSSH` és `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

A JSON-elemzővel használatával hitelesítse a hálózati biztonsági csoport és a bejövő szabályok:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

A két kártyát kötést létrehozni a hálózati biztonsági csoport:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Az elérhetőség készlet létrehozásához. Az alábbi példa létrehoz egy set named elérhetősége `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Az első Linux virtuális létrehozása. Az alábbi példa létrehoz egy virtuális nevű `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

A második Linux virtuális létrehozása. Az alábbi példa létrehoz egy virtuális nevű `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Használja a JSON-elemzővel annak ellenőrzésére, hogy minden készült:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Az új környezet exportálása egy sablont, újra gyorsan létrehozhat új példányok:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Részletes útmutató
Az alábbi részletes lépései bemutatják, hogy milyen minden parancs módon, akkor a környezet kiépítésekor. Ezek a következők hasznos, ha a saját egyéni környezetekben fejlesztési vagy gyártási generál.

Győződjön meg arról, hogy rendelkezik-e jelentkezve, és erőforrás-kezelő üzemmód használata [Az Azure CLI](../xplat-cli-install.md) :

```bash
azure config mode arm
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméternevek olyan `myResourceGroup`, `mystorageaccount`, és `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Erőforrás-csoportok létrehozása, és válassza a telepítési helye

Azure erőforráscsoport olyan logikai telepítési szervezetek, amelyeknél a konfigurációs adatok és metaadatok ahhoz, hogy az erőforrás-telepítések logikai kezelése. Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `westeurope` helyét:

```bash
azure group create --name myResourceGroup --location westeurope
```

Kimenet:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

A virtuális merevlemezeken és bármilyen további adatokat lemezre, amely a hozzáadni kívánt tároló fiókok szüksége. Tárterület-fiókokat hoz létre, szinte, azonnal után erőforrás csoportokat hoz létre.

Itt használjuk a `azure storage account create` parancsot, és milyen típusú tárterület-támogatást, amelyet, hogy hol található a fiókja, az erőforráscsoport áthaladó szabályozza. Az alábbi példa létrehoz egy tárolási nevű fiókot `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Kimenet:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Az erőforráscsoport megvizsgálni használatával a `azure group show` paranccsal, együtt a [jq](https://stedolan.github.io/jq/) eszköz használata a `--json` Azure CLI lehetőséget. (Használható **jsawk** , vagy inkább a JSON elemzésére nyelvi tárból.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Kimenet:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

A tárterület-fiókot a CLI vizsgálatához először kell beállítania a fiók nevét, és a billentyűk. Az alábbi példa a tárterület-fiók neve cserélje ki egy nevet, amely a kiválasztott:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Majd egyszerűen megtekintheti a tárhely adatok:

```bash
azure storage container list
```

Kimenet:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Hozzon létre egy virtuális hálózati és alhálózat

Ezután, hogy lesz szüksége létrehozása az Azure és a VMs hozhat létre alhálózat futtató virtuális hálózaton. Az alábbi példa létrehoz egy virtuális hálózati nevű `myVnet` együtt a `192.168.0.0/16` cím előtag:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Kimenet:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Ismét használata – json lehetőséget `azure group show` és **jq** hogyan azt szeretne összeállítani a források megjelenítéséhez. Most már rendelkezik egy `storageAccounts` erőforrás és egy `virtualNetworks` erőforrás.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Kimenet:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Most már az alhálózathoz létrehozása a `myVnet` virtuális hálózati, amelybe a VMs van telepítve. Használja a `azure network vnet subnet create` parancsot, és az erőforrások által már létrehozott: a `myResourceGroup` erőforráscsoport és a `myVnet` virtuális hálózat. A következő példában az alhálózathoz nevű hozzáadunk `mySubnet` az a cím előtagot `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Kimenet:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Mivel az alhálózathoz logikailag belül a virtuális hálózat, az alhálózathoz információt némileg eltérő paranccsal megnézi. A parancs használata `azure network vnet show`, de továbbra is, hogy vizsgálja meg a JSON kimeneti **jq**használatával.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Kimenet:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Hozzon létre egy nyilvános IP-cím (MEZŐPONT)

Most már a nyilvános IP-cím (MEZŐPONT), amely azt hozzárendelése a terheléselosztó létrehozása. Lehetővé teszi, hogy csatlakozzon a VMs az internetről használatával a `azure network public-ip create` parancsot. Mivel az alapértelmezett cím dinamikus, hozzunk létre elnevezett DNS-bejegyzés az **cloudapp.azure.com** tartomány használatával a `--domain-name-label` lehetőséget. Az alábbi példa létrehoz egy nyilvános IP-nevű `myPublicIP` DNS nevű `mypublicdns`. A DNS-nevének egyedinek kell lennie, mint adja meg a saját egyedi tartománynév:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Kimenet:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

A nyilvános IP-címet is a legfelső szintű erőforrás úgy is látható marad a `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Kimenet:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

További erőforrások adatait, beleértve a az altartomány, teljes tartománynevét (FQDN) a teljes segítségével kiderítheti `azure network public-ip show` parancsot. A nyilvános IP-cím erőforráshoz hozzárendelt logikailag, de egy adott cím még nem rendelt. Az IP-cím beszerzéséhez, hogy lesz szüksége terheléselosztó, amely azt még nem hozott létre.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Kimenet:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Hozzon létre egy terheléselosztó és IP-készletek
Amikor létrehoz egy terheléselosztó, lehetővé teszi a forgalom elosztása több VMs. Az alkalmazás redundancia több VMs, amely a karbantartás vagy nagy terhelés felhasználói kérelmekre válaszolni futtatásával is tartalmaz. Az alábbi példa létrehoz egy névvel ellátott terheléselosztó `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Kimenet:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

A terheléselosztó meglehetősen üres lesz, így érdemes létrehozni az egyes IP-készletek. Hozzon létre két IP-készleteket az terheléselosztó – egy, az előtér és a háttér egy szeretnénk. Az előtér-IP-készlet nyilvánosan látható. Érdemes emellett azt a MEZŐPONT, amely a korábban létrehozott hozzárendelése helyét. Ezután vesszük a háttéradatbázist készlet helyét a VMs való csatlakozáshoz. Úgy, hogy a forgalmat a terheléselosztó szeretne a VMs keresztül is flow.

Első lépésként az előtér-IP-készlet létrehozása. Az alábbi példa létrehoz egy előtér-készlet nevű `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Kimenet:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Figyelje meg, hogyan azt használja a `--public-ip-name` átadni a Váltás a `myPublicIP` , amely a korábban létrehozott. A nyilvános IP-cím hozzárendelése a terheléselosztó lehetővé teszi a VMs éri szerte az interneten.

Ezután létrehozása a második IP-készlet ez esetben a háttéradatbázist forgalmat. Az alábbi példa létrehoz egy névvel ellátott háttéradatbázist készlet `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Kimenet:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

A képen látható, hogyan hajtsa végre a terheléselosztó megjeleníti a `azure network lb show` és a JSON kimenet vizsgálata:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Kimenet:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Terheléselosztó hálózati Címfordítást szabályok létrehozása
Forgalmat a terheléselosztó keresztül halad megszerezni cím hálózati címfordító szabályok létrehozása, amelyek adja meg a bejövő és kimenő vagy műveletek szükség. Adja meg a protokoll használatára, majd a külső portok megfeleltetése belső portok tetszés szerint. A környezet létrehozása bizonyos szabályok, amelyek lehetővé teszik SSH a terheléselosztó szeretne a VMs keresztül. Azt beállítása 4222 és 4223 TCP-portokat közvetlen 22-es TCP-porthoz a VMs (amelyek később hozzunk létre). Az alábbi példa létrehoz egy szabályt, nevű `myLoadBalancerRuleSSH1` port 22-es TCP-port 4222 hozzárendelése:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Kimenet:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Ismételje meg az eljárást a második hálózati Címfordítást szabály SSH. Az alábbi példa létrehoz egy szabályt, nevű `myLoadBalancerRuleSSH2` port 22-es TCP-port 4223 hozzárendelése:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Lássunk is jóváhagyást és hálózati Címfordítást szabály létrehozása az internetes forgalmat a szabály csatlakoztatás felfelé az IP-készletek portot 80. Ha azt bekötése IP-kvótáját csatlakoztatása a saját VMs szabály külön-külön, hanem a szabály azt felvehet és VMs eltávolítása az IP-készlet. A terheléselosztó automatikus igazítása a forgalmat. Az alábbi példa létrehoz egy szabályt, nevű `myLoadBalancerRuleWeb` 80-as port TCP 80-as port hozzárendelése:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Kimenet:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>A betöltés terheléselosztó állapot vizsgálati létrehozása

Egészségügyi probe rendszeres ellenőrzésére a VMs meg, hogy éppen működő és meghatározott kérelmek megválaszolását terheléselosztó mögötti. Ha nem, ezek műveletet annak érdekében, hogy a felhasználók nem készül irányított rájuk is törlődnek. Egyéni keresi meg a rendszerállapot vizsgálati, intervallumok és az időtúllépés értékeket adhatja meg. Állapot szondákat kapcsolatos további tudnivalókért olvassa el a [terheléselosztó ellenőrzi](../load-balancer/load-balancer-custom-probe-overview.md)című témakört. Az alábbi példa létrehoz egy TCP állapota a vizsgált elnevezett `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Kimenet:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Ebben az esetben azt az állapot-ellenőrzései 15 másodperc elvégezve megadva. Azt is kihagy legfeljebb négy szondákat (egy perc) a terheléselosztó úgy véli, hogy a már nem működik a fogadó előtt.

## <a name="verify-the-load-balancer"></a>Ellenőrizze a terheléselosztó
A konfiguráció betöltése terheléselosztó kész. Az alábbiakban a lépéseket:

1. Először létrehozott egy terheléselosztó.
2. Ezután a előtér-IP-készlet létrehozása, és egy nyilvános IP rendelve.
3. A következő hozott létre, amely VMs csatlakozhat háttéradatbázist IP erőforráskészlethez tartozik.
4. Ezt követően hozott létre, amelyek lehetővé teszik a VMs funkcióival, egy szabályt, amely lehetővé teszi, hogy a saját webalkalmazás 80 portot együtt való SSH hálózati Címfordítást szabályokat.
5. Végül egy állapot vizsgálati rendszeresen ellenőrizni a VMs hozzáadott. A rendszerállapot vizsgálati biztosítja, hogy a felhasználók megpróbálnak nem egy virtuális, amely már nem működik, vagy felszolgálásához tartalom eléréséhez.

Tanulmányozzuk a terheléselosztó néz ki most:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Kimenet:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Hozzon létre egy hálózati a Linux virtuális használata

NIC programozás útján érhetők el, mert a szabályok alkalmazása is használatuk. Egynél több is is. Az alábbiakban `azure network nic create` parancsot, jelölje be a betöltés háttéradatbázist IP-kvótáját a hálózati a csatlakoztatni, és lehetővé teszi a SSH forgalmat a hálózati Címfordítást szabály társítása.
 
Cserélje le a `#####-###-###` szakaszok a saját Azure előfizetés azonosítójával. Az előfizetés azonosítója kell a kimeneti jegyezni `jq` hoz létre az erőforrások vizsgálatakor. Is megtekintheti és az előfizetés azonosítója `azure account list`. 

Az alábbi példa létrehoz egy hálózati kártya elnevezett `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Kimenet:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Az erőforrás közvetlenül vizsgálata megjelenítheti a részleteket. Az erőforrás használatával vizsgálja meg a `azure network nic show` parancsot:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Kimenet:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Ekkor a második hálózati kártya csatlakoztatás be újra a a háttéradatbázist IP-készlet hozzunk létre. Ez a második alkalommal a hálózati Címfordítást szabály lehetővé teszi, hogy SSH forgalmat. Az alábbi példa létrehoz egy hálózati kártya elnevezett `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Hálózati biztonsági csoport és a szabályok létrehozása

Most már hozzunk létre egy hálózati biztonsági csoport és a hozzáférést a adaptert meghatározó bejövő szabályok Hálózati biztonsági csoport hálózati vagy alhálózat alkalmazhatók. A forgalmat és kijelentkezés a VMs szabály megadása Az alábbi példa létrehoz egy hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

A port (SSH támogatásához) 22 bejövő kapcsolatok engedélyezésére NSG a bejövő szabály hozzáadása. Az alábbi példa létrehoz egy szabályt, nevű `myNetworkSecurityGroupRuleSSH` 22-es TCP-port engedélyezése:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Most már az NSG (internetes forgalmat támogatásához) 80-as port bejövő kapcsolatok engedélyezése a bejövő szabály hozzáadása. Az alábbi példa létrehoz egy szabályt, nevű `myNetworkSecurityGroupRuleHTTP` TCP 80-as port engedélyezése:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] A bejövő szabály egy bejövő hálózati kapcsolatok szűrőt. Ebben a példában a VMs virtuális hálózati kártya, ami azt jelenti, hogy, hogy a porttal 22 kérésének átadott keresztül a hálózati kártya a saját virtuális a NSG kötni azt. A bejövő szabályok kapcsolatos hálózati kapcsolaton, és nem kapcsolatos zárólap, amely olyan, milyen lenne kapcsolatban a klasszikus környezetekben. Olyan portot megnyitásához meg kell hagyni a `--source-port-range` meg a "\*" (az alapértelmezett érték) fogadja el a **minden olyan** portot kérő bejövő kérelmeket. Legyen a szokásos dinamikus.

## <a name="bind-to-the-nic"></a>A hálózati kártya kötése

A NSG kötni a NIC. A hálózati biztonsági csoport csatlakozás a NIC szükség. Mindkét parancsok, mind a NICS jelölje be a csatlakoztatni futtatásával:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Hozzon létre egy elérhetősége
Elérhetőség a VMs beállítása súgó várható hibafa tartományok és a frissítési tartományok keresztül. Létrehozása az elérhetőségét a VMs beállítása. Az alábbi példa létrehoz egy set named elérhetősége `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Hibafa-tartományok egy csoportja közös power forrás- és kapcsoló osztozó virtuális gépeken futó határozza meg. Alapértelmezés szerint a virtuális gépeken futó belül az elérhetőség konfigurált legfeljebb három hibafa tartományok neve. A rendszer a arról, hogy a hardver a probléma az alábbi hiba tartományok egyik nem befolyásolja az alkalmazást futtató minden virtuális. Ha úgy, hogy egy elérhetőségének beállítása Azure automatikusan elosztja VMs hibafa tartományok.

Frissítési tartományok azt jelzik, hogy virtuális gépeken futó és az alapul szolgáló fizikai hardver, hogy újra kell indítani egy időben. A sorrendben, amelyben a frissítési tartományok újraindítása után nem feltétlenül egymás után következő tervezett karbantartás során, de egyszerre csak egy frissítés újraindítása után. Ismét Azure automatikusan elosztja a VMs frissítési tartományok amikor úgy, hogy az elérhetőség webhelyen.

További információ [a VMs elérhetőségét kezelése](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>A Linux VMs létrehozása

A tárhely és a hálózati erőforrások a VMs Internet által elérhető támogatási létrehozott. Most vegyük létrehozása, amelyek VMs, és nem tartozik jelszó SSH kulccsal biztonságos őket. Ebben az esetben megyünk hozzon létre egy Ubuntu virtuális a legutóbbi LTS alapján. Azt keresse meg a kép információkat, a `azure vm image list`, [Azure virtuális képek keresése](virtual-machines-linux-cli-ps-findimage.md)leírt módon.

Azt a képet kijelölve a parancs használatával `azure vm image list westeurope canonical | grep LTS`. Ebben az esetben használjuk `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Az utolsó mezőnél, hogy adják át `latest` , hogy a jövőben azt eljuthassanak legutóbbi összeállítása. (A karakterlánc használjuk `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Ez a következő lépés nem jól ismert bárki megtekintheti, aki a már létrehozott egy ssh rsa nyilvános és titkos kulcs párosítás Linux és Mac gépre **ssh-keygen-t rsa -b 2048**használatával. Ha nem rendelkezik-e be a bármely tanúsítvány kulcs párban a `~/.ssh` címtár hozhat létre őket:

- Használatával automatikusan, a `azure vm create --generate-ssh-keys` lehetőséget.
- Kézzel használatával [őket saját maga hozza létre a megjelenő utasításokat](virtual-machines-linux-mac-create-ssh-keys.md).

Másik lehetőségként használhatja a `--admin-password` módszer hitelesítést végezni a SSH kapcsolatok, a virtuális létrehozását követően. Ez a módszer akkor általában kevésbé biztonságos.

A virtuális hozzunk létre a által a források és együtt információk arra a `azure vm create` parancsot:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Kimenet:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Csatlakoztathatja a virtuális közvetlenül az alapértelmezett SSH használatával. Győződjön meg arról, hogy a megfelelő port adja meg, mert azt, hogy a terheléselosztó áthaladó. (Az első virtuális azt állítsa be a hálózati Címfordítást szabály port 4222 továbbítani szeretné a virtuális):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Kimenet:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Jóváhagyást, és hozzon létre a második virtuális megegyező módon:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

És most már használhatja a `azure vm show myResourceGroup myVM1` vizsgálja meg, mi létrehozott parancsot. Ezen a ponton a Ubuntu VMs mögött egy terheléselosztó futtatja, jelentkezzen be csak a SSH kulcs pár (azért, mert a jelszavak le vannak tiltva) Azure-ban. Telepítse a nginx vagy httpd, webes alkalmazások terjesztése és lásd: a forgalmat a terheléselosztó mindkét a VMs keresztül.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Kimenet:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>A környezet exportálása sablonként
Most, hogy ki a környezet létrehozása, mi történik, ha szeretne egy további fejlesztői környezet létrehozása a paramétereket és megfelelő, akkor munkakörnyezetben? Erőforrás-kezelő JSON sablonokat, amelyek meghatározzák az összes paramétert, környezetben használja. A JSON-sablon hivatkozó kiépítésekor teljes környezetben. Választhat a [JSON-sablonok kézi összeállítása](../resource-group-authoring-templates.md) és exportálása egy már meglévő környezetbe a JSON-sablon létrehozásához:

```bash
azure group export --name myResourceGroup
```

Ez a parancs hoz létre a `myResourceGroup.json` az aktuális munkakönyvtár fájlt. A sablonból hoz létre olyan környezetben, amikor a rendszer kéri az összes az erőforrások nevei, beleértve a neveket a terheléselosztó, hálózati kapcsolatok és VMs. Feltöltésére ezeket a neveket a sablon fájl hozzáadásával a `-p` vagy `--includeParameterDefaultValue` paramétert a `azure group export` korábbi megjelenített parancsot. Az erőforrások nevei, vagy [Hozzon létre egy parameters.json fájlt](../resource-group-authoring-templates.md#parameters) , amely meghatározza az erőforrások nevei megadhatja a JSON-sablon szerkesztése.

Környezet létrehozása a sablon alapján:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Érdemes lehet [sablonok telepítéséről további](../resource-group-template-deploy-cli.md)olvasható. További tudnivalók fokozatosan környezetekben frissítése, használja a paraméterek fájlt és sablonok elérése egyetlen tárhelyéről.

## <a name="next-steps"></a>Következő lépések

Most már készen áll a hálózati összetevők és a VMs használatának megkezdéséhez. Ez a példa környezet kiépítésekor az alkalmazást, az alapvető alkatrészek jelent meg az alábbi használatával is használhatja.
