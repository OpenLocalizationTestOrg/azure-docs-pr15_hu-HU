<properties
   pageTitle="Hozzon létre egy belső terheléselosztó az Azure CLI használatával az erőforrás-kezelő |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy belső terheléselosztó az erőforrás-kezelő az Azure CLI használatával."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Egy belső terheléselosztó létrehozása az Azure CLI használatával

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>A megoldást üzembe helyez a az Azure CLI használatával

A következő lépések bemutatják, hogyan hozhat létre egy internetes terheléselosztó CLI-Azure erőforrás-kezelő használatával. Az Azure erőforrás-kezelő, az egyes erőforrások létrehozott és beállított külön-külön, és, majd írja erőforrás létrehozása.

Létrehozása és konfigurálása a következő objektumoknak üzembe egy terheléselosztó szükséges:

- **Előfeldolgozó IP-beállítások**: a bejövő hálózati forgalmat a nyilvános IP-címeket tartalmazza
- **Háttéradatbázis cím készlet**: tartalmazza, amelyek lehetővé teszik a virtuális gépeken futó hálózati forgalmának engedélyezésére fogadjon a terheléselosztó hálózati kapcsolatok (NIC)
- **Szabályok terheléselosztási**: a terheléselosztó egy nyilvános portjához feleltesse meg a háttéradatbázis cím készletben port szabályokat tartalmaz
- **Hálózati címfordító Eszközig bejövő szabályok**: a terheléselosztó egy nyilvános portjához feleltesse meg a háttéradatbázis cím készletben adott virtuális géphez port szabályokat tartalmaz
- **Ellenőrzi**: állapot szondákat való elérhetőségének megállapítása a virtuális gépeken futó példányok a háttéradatbázist cím készletben használt tartalmaz

További tudnivalókért lásd: [Azure erőforrás-kezelő terheléselosztó támogatása](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Erőforrás-kezelő CLI beállítása

1. Ha még sosem használt Azure CLI, olvassa el a [Telepítse és állítsa be az Azure CLI](../../articles/xplat-cli-install.md). Kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Az **azure konfiguráció mód** parancsot módba vált, erőforrás-kezelő, az alábbi képlettel történik:

        azure config mode arm

    Várt kimenet:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Hozzon létre egy belső terheléselosztó lépésről lépésre

1. Jelentkezzen be az Azure.

        azure login

    Amikor a rendszer kéri, adja meg az Azure hitelesítő adatait.

2. Módosítsa a parancs eszközök Azure erőforrás-kezelő módra.

        azure config mode arm

## <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása

Az Azure erőforrás-kezelő erőforrások erőforráscsoport kapcsolódnak. Ha eddig még, hozzon létre egy erőforrás csoportot.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Hozzon létre egy belső betöltés terheléselosztó

1. Hozzon létre egy belső terheléselosztó

    A következő esetben nrprg nevű erőforráscsoport kelet-Amerikai Egyesült Államok régió jön létre.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Belső terheléselosztókkal, például a virtuális hálózatok és a virtuális hálózati alhálózat, az összes erőforrás az azonos erőforráscsoport és ugyanabban a régióban kell lennie.

2. Hozzon létre egy a belső terheléselosztó előtér-IP-címet.

    Az Ön által használt IP-címet a virtuális hálózat alhálózat tartományon belül kell lennie.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. A háttéradatbázist cím készlet létrehozása.

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Egy előtér-IP-cím és a háttéradatbázist cím erőforráskészlethez tartozik definiálása után betöltés terheléselosztó szabályokat, a bejövő szabályok hálózati Címfordítást, és testre szabott állapot ellenőrzi.

4. A belső terheléselosztó a betöltés terheléselosztó szabály létrehozása.

    Ha az előző lépésekkel a parancs a portot 1433 előtér-készletben és is küld terheléselosztás forgalmat a háttéradatbázist cím kvótáját, használja a port 1433 terheléselosztó szabály hoz létre.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Hálózati Címfordítást a bejövő szabályok létrehozása.

    Hálózati Címfordítást a bejövő szabályok egy terheléselosztó, amely egy adott virtuális gép példány lépjen a végpontok létrehozásához használt. Az előző lépésekben létrehozott két hálózati Címfordítást szabályok távoli asztali.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Hozzon létre a terheléselosztó az állapot szondákat.

    A rendszerállapot vizsgálati ellenőrzi, hogy a hálózati forgalmának engedélyezésére küldhetnek példányait virtuális gép. A virtuális gép példányok sikertelen vizsgálati ellenőrzések távolítja el a terheléselosztó mindaddig, amíg az online állapotba kerül, és a vizsgálati ellenőrzése határozza meg, hogy az megfelelő.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]A Microsoft Azure platform felügyeleti esetek számos egy statikus, nyilvánosan átirányítható IPv4-címet használja. Az IP-cím 168.63.129.16. Az IP-cím nem kell blokkolta bármely a tűzfalak, mivel ez okozhat szokatlan viselkedést tapasztal.
    >Részletez Azure belső terheléselosztás IP-cím használja a terheléselosztó a szondákat figyelése határozza meg a rendszerállapot a virtuális gépeken futó terheléselosztás meg. Ha hálózati biztonsági csoport forgalmának korlátozása egy belső terheléselosztás beállítása Azure virtuális gépeken futó használják, vagy egy virtuális hálózati alhálózat alkalmazott, győződjön meg arról, hogy a hálózati biztonsági szabály szerepel-e 168.63.129.16 forgalmának engedélyezésére.

## <a name="create-nics"></a>NIC létrehozása

Kell NIC létrehozása (vagy módosíthatja a meglévőket), és a társítani a hálózati Címfordítást szabályokat, betöltés terheléselosztó szabályok és szondákat.

1. Hozzon létre egy hálózati kártya elnevezett *lb nic1 kell*, és majd társítása a *rdp1* hálózati Címfordítást szabály, és a *beilb* háttéradatbázist cím készlet.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

    Várt kimenet:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Hozzon létre egy hálózati kártya elnevezett *lb nic2 kell*, és majd társítása a *rdp2* hálózati Címfordítást szabály, és a *beilb* háttéradatbázist cím készlet.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. *D1*nevű virtuális gép létrehozása és a társítani a hálózati kártya elnevezett *lb nic1 kell*. A tároló *web1nrp* nevű hozza létre fiókot a következő parancs futtatása előtt:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Az egy terheléselosztó VMs kell elérhetősége mindazon. Használat `azure availset create` létrehozása az elérhetőségét.

4. Hozzon létre egy virtuális gép (virtuális) nevű *DB2*, és a társítani a hálózati kártya elnevezett *lb nic2 kell*. Egy *web1nrp* nevű tárterület-fiókot a következő parancs futtatása előtt hozták létre.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Egy terheléselosztó törlése

Ha el szeretne távolítani egy terheléselosztó, használja az alábbi parancsot:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Következő lépések

[A betöltés terheléselosztó terjesztési mód konfigurálása a forrás IP affinitás használatával](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
