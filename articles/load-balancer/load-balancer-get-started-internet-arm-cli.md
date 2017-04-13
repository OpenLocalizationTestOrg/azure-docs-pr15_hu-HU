<properties
   pageTitle="Hozzon létre egy internetes terheléselosztó az erőforrás-kezelő használatával az Azure CLI |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó az erőforrás-kezelő az Azure CLI használatával"
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

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Egy belső terheléselosztó használatával az Azure CLI létrehozása

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó klasszikus telepítési használatával](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>A használja az Azure CLI megoldást üzembe helyezné

A következő lépések bemutatják, hogyan hozhat létre egy internetes Azure erőforrás-kezelő használata CLI terheléselosztó. Az Azure erőforrás-kezelő minden erőforrás jön létre, és konfigurált külön-külön, helyezze erőforrás létrehozása.

Kell létrehozni, és állítsa be a következő objektumoknak egy terheléselosztó telepítése:

- Előtér-IP-konfiguráció – a bejövő hálózati forgalmat a nyilvános IP-címeket tartalmazza.
- Háttéradatbázis cím alkalmazáskészlet - a virtuális gépeken futó hálózati forgalmának engedélyezésére fogadjon a terheléselosztó hálózati kapcsolatok (NIC) tartalmaz.
- Egy nyilvános portjához a terheléselosztó hozzárendelése a háttéradatbázist cím készletben port szabályok terheléselosztás szabályok - tartalmazza.
- Bejövő hálózati Címfordítást szabályok – a terheléselosztó egy nyilvános portjához hozzárendelése egy adott virtuális gép a háttéradatbázist cím készletben port szabályokat tartalmaz.
- Ellenőrzi,-állapot szondákat elérhetőségének megállapítása a háttéradatbázist cím készletben virtuális gépeken futó példányok használt tartalmazza.

További információt az [Azure erőforrás-kezelő támogatási terheléselosztó](load-balancer-arm.md)témakörben talál.

## <a name="set-up-cli-to-use-resource-manager"></a>Erőforrás-kezelő CLI beállítása

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Futtassa az erőforrás-kezelő mód, váltson a **azure konfiguráció mód** parancsot, alább látható módon.

        azure config mode arm

    Várt kimenet:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Virtuális hálózat és a nyilvános IP-címet az előtér-IP-készlet létrehozása

1. Hozzon létre egy virtuális hálózati (VNet) nevű *NRPVnet* *NRPRG*nevű erőforráscsoport használatával kelet USA-beli hely.

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    *NRPVnetSubnet* a *NRPVnet*a 10.0.0.0/24 CIDR blokkja nevű alhálózat létrehozása.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Hozzon létre egy nyilvános IP-cím nevű *NRPPublicIP* DNS neve *loadbalancernrp.eastus.cloudapp.azure.com*az előtér-IP erőforráskészlethez tartozik való használatra. Az alábbi parancsot statikus felosztás típusa és 4 perc üresjárati időtúllépés használja.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]A teljesen minősített tartománynév a terheléselosztó használja a tartomány címke nyilvános IP. Ez a változás klasszikus telepítési, használja a felhőben szolgáltatás, a terheléselosztó teljesen minősített tartománynevét (FQDN).
    >Ebben a példában a teljesen minősített tartománynév *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Hozzon létre egy terheléselosztó

A következő parancs létrehoz egy *NRPlb* nevű *NRPRG* erőforrás csoportjában a *Kelet-Amerikai Egyesült Államok* terheléselosztó Azure helyét.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Hozzon létre egy előtér-IP-készlet és kódmentes cím erőforráskészlethez tartozik

Ez a példa bemutatja, hogyan hozhat létre az előtér-IP-készlet kapja a bejövő hálózati forgalmat a terheléselosztó és a kódmentes IP készletre, ahol az előtér-készlet küld-e a terheléselosztása hálózati forgalmának engedélyezésére.

1. A nyilvános IP-a terheléselosztó és az előző lépésben létrehozott társítása előtér-IP-készlet létrehozása.

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Állítsa be a bejövő forgalom az előtér-IP-készletből fogadására használt háttéradatbázist cím erőforráskészlethez tartozik.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>LB szabályokat, a hálózati Címfordítást szabályok és a vizsgálati létrehozása

Ebben a példában az alábbi elemek hoz létre.

- Ha port 21 port 22<sup>1</sup> minden bejövő forgalmát hálózati Címfordítást szabály
- fordítása porthoz 22 23-as port összes bejövő forgalmat a hálózati Címfordítást szabályok
- betöltési terheléselosztó szabály minden bejövő forgalmat a címeket, a háttéradatbázist készletben 80-as portjához 80-as port egyenleg.
- állapot ellenőrzése egy lapon a vizsgálati szabályok nevű *HealthProbe.aspx*.

<sup>1</sup> a hálózati Címfordítást szabályokat kell egy adott virtuális gép példány a terheléselosztó mögött van társítva. A port 21 érkező forgalmat a hálózati Címfordítást szabályhoz társított 22 port adott virtuális géphez küldi. Egy hálózati Címfordítást szabályhoz (UDP vagy TCP) protokoll kell megadnia. Nem lehet két protokoll hozzárendelni ugyanazt a portot.

1. A hálózati Címfordítást szabályok létrehozásához.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Hozzon létre egy betöltés terheléselosztó szabályt.

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Hozzon létre egy állapot vizsgálati.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Ellenőrizze a beállításait.

        azure network lb show nrprg nrplb

    Várt kimenet:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>NIC létrehozása

Kell NIC létrehozása (vagy módosíthatja a meglévőket), és a társítani a hálózati Címfordítást szabályokat, betöltés terheléselosztó szabályok és szondákat.

1. Hozzon létre egy hálózati kártya elnevezett *lb nic1 kell*, és a társítani a *rdp1* hálózati Címfordítást szabály, és a *NRPbackendpool* háttéradatbázist cím készlet.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

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

2. Hozzon létre egy hálózati kártya elnevezett *lb nic2 kell*, és a társítani a *rdp2* hálózati Címfordítást szabály, és a *NRPbackendpool* háttéradatbázist cím készlet.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Hozzon létre egy virtuális gép (virtuális) nevű a *weben 1*, és a hálózati kártya elnevezett társítása *lb nic1 kell*. Egy *web1nrp* nevű tárterület-fiókot az alábbi parancs futtatása előtt hozták létre.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Az egy terheléselosztó VMs kell elérhetősége mindazon. Használat `azure availset create` létrehozása az elérhetőségét.

    A kimenet kell lennie az alábbihoz hasonló:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] A **most konfigurált publicIP nélkül egy hálózati** tájékoztató üzenet várhatóan a hálózati kártya a terheléselosztó internetcsatlakozást a betöltés terheléselosztó nyilvános IP-cím használatával létrehozott óta.

    Mivel a *lb nic1 kell* hálózati kártya társítva a *rdp1* hálózati Címfordítást szabályt, a *weben 1* RDP segítségével a terheléselosztó 3441 portjához keresztül csatlakozhat.

4. Hozzon létre egy virtuális gép (virtuális) nevű a *weben 2*, és a hálózati kártya elnevezett társítása *lb nic2 kell*. Egy *web1nrp* nevű tárterület-fiókot az alábbi parancs futtatása előtt hozták létre.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Egy meglévő terheléselosztó frissítése

Szabályok hivatkozó egy meglévő terheléselosztó is hozzáadhat. A következő példában betöltés terheléselosztó új szabály egy meglévő terheléselosztó **NRPlb** hozzáadva

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Egy terheléselosztó törlése

Ha el szeretne távolítani egy terheléselosztó használja a az alábbi parancsot:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-cli.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
