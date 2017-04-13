<properties
   pageTitle="Hozzon létre egy belső terheléselosztó, az erőforrás-kezelő PowerShell használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy belső terheléselosztó, az erőforrás-kezelő PowerShell használatával"
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

# <a name="create-an-internal-load-balancer-using-powershell"></a>Hozzon létre egy belső terheléselosztó PowerShell használatával

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Az alábbi lépésekkel bemutatják, hogy miként hozhat létre egy belső terheléselosztó Azure-kezelő a PowerShell használata. Az Azure erőforrás-kezelő, hozzon létre egy belső terheléselosztó az elemeket egyenként konfigurálható és majd együttes használata egy terheléselosztó létrehozására.

Létrehozása és konfigurálása a következő objektumoknak üzembe egy terheléselosztó szükséges:

- Előrehozás vége IP-konfiguráció – konfigurálja a magánjellegű IP-címe bejövő hálózati forgalmának engedélyezésére
- Kódmentes cím alkalmazáskészlet - beállítja a hálózati kapcsolatokat, amely megkapja az IP-készlet előtér-ból érkező terheléselosztása forgalom
- A terheléselosztás szabályok - forrás- és a terheléselosztó helyi port konfigurációját.
- Ellenőrzi, – konfigurálja az állapot állapot vizsgálati a virtuális gép előfordulását.
- Bejövő szabályok hálózati Címfordítást - állítja be a port szabályokat, így közvetlenül elérheti a virtuális gép példányok.

Kaphat további információt a betöltés terheléselosztó összetevők az Azure erőforrás-kezelővel az [Azure erőforrás-kezelő támogatási terheléselosztó](load-balancer-arm.md).

A következő lépések bemutatják, hogy miként egy terheléselosztó között két virtuális gépeken futó konfigurálása.


## <a name="setup-powershell-to-use-resource-manager"></a>A PowerShell használata az erőforrás-kezelő beállítása

Ellenőrizze, hogy Powershellhez készült Azure a modul legújabb gyártási verziójával rendelkezik, és PowerShell beállítási megfelelően van az Azure előfizetés eléréséhez.

### <a name="step-1"></a>Lépés: 1

        Login-AzureRmAccount

### <a name="step-2"></a>Lépés: 2

Az előfizetések a fiók ellenőrzése

        Get-AzureRmSubscription

Kéri a hitelesítés a hitelesítő adatokkal.<BR>

### <a name="step-3"></a>3 lépés

A használandó Azure előfizetések kiválasztása. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Erőforráscsoport létrehozása terheléselosztó

### <a name="step-4"></a>Lépés: 4

Hozzon létre egy új erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ez az erőforrások erőforráscsoport az alapértelmezett helye szolgál. Ellenőrizze, hogy minden parancs hozhat létre egy terheléselosztó az azonos erőforráscsoport fogja használni.

A fenti példában létrehozott egy úgynevezett "NRP-RG" és "US nyugati" hely erőforráscsoport.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Virtuális hálózati és egy privát IP-cím előtér IP-készlet létrehozása


### <a name="step-1"></a>Lépés: 1

A virtuális hálózat alhálózat hoz létre, és változó $backendSubnet rendel

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Hozzon létre egy virtuális hálózati:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Hoz létre a virtuális hálózat, és hozzáadja a alhálózat lb alhálózat kell a virtuális hálózat NRPVNet és változó $vnet rendel



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Első IP készletre és kódmentes cím készlet létrehozása

Állítsa be a előtér IP-készletben a bejövő betöltés terheléselosztó hálózati forgalmat és kódmentes cím gyűjtő fogadásához a betöltés meghatározni forgalmat.

### <a name="step-1"></a>Lépés: 1

A magánjellegű 10.0.2.5 IP-cím használata a alhálózat 10.0.2.0/24 amelyek lesznek a bejövő hálózati forgalmának engedélyezésére végpont előtér IP-készlet létrehozása.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>lépés: 2

A visszahívási cím készletre előtér IP készletből bejövő forgalom fogadására használt beállítása:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>LB szabályokat, hálózati Címfordítást szabályokat, és hozhat létre vizsgálati terheléselosztó

Az előtér-IP készlet és a kódmentes cím készlet létrehozása, után szüksége lesz a betöltés terheléselosztó erőforráshoz tartozó szabályok létrehozása:

### <a name="step-1"></a>Lépés: 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


A fenti példa hoz létre az alábbiakat:

- Hálózati Címfordítást szabály, ezért a porttal 3441 összes bejövő forgalom port 3389 ugrik.
- második hálózati Címfordítást szabályt, amely a porttal 3442 összes bejövő forgalom port 3389 ugrik.
- a betöltés terheléselosztó szabály, ezért fogja tölteni egyenleg nyilvános port 80 80 helyi portot a hátsó cím készletre összes bejövő forgalmát.
- egy vizsgálati szabályt, amely ellenőrzi a elérési út "HealthProbe.aspx" állapot állapota



### <a name="step-2"></a>Lépés: 2

Az összes objektumot (hálózati Címfordítást szabályokat, betöltés terheléselosztó szabályokat, vizsgálati konfigurációk) összeadva terheléselosztó létrehozása:

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Hálózati kapcsolatok létrehozása

Miután létrehozta a belső terheléselosztó, szükséges definiálása mely hálózati kapcsolatok fog kapni a bejövő terheléselosztása hálózati forgalmának engedélyezésére, hálózati Címfordítást szabályok és vizsgálati. Ebben az esetben a hálózati kapcsolaton egyenként be van állítva, és később virtuális géphez rendelhetők.


### <a name="step-1"></a>Lépés: 1


Kap, az erőforrás virtuális hálózati és alhálózat hálózati kapcsolatok létrehozásához:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Ebben a lépésben létrehoz egy hálózati illesztő, amely a betöltés terheléselosztó vissza készletre tartoznak, és az első hálózati Címfordítást szabály társítása RDP-a hálózati kapcsolat:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Lépés: 2

Hozzon létre egy úgynevezett második hálózati felület LB Nic2 kell:

Ebben a lépésben létrehoz egy második hálózati kapcsolat, az azonos betöltés terheléselosztó vissza készletre hozzárendelése, és a második hálózati Címfordítást szabály társítása RDP jött létre:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Ennek eredményeképpen a következő jelennek meg:

    $backendnic1

Várt kimenet:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>3 lépés

A paranccsal hozzáadása-AzureRmVMNetworkInterface a hálózati kártya hozzárendelése egy virtuális számítógépre.

A lépésenkénti utasításokat virtuális gép létrehozása és hozzárendelése egy hálózati követő dokumentációjában találhat: [létrehozása az Azure virtuális PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md).

Ha már létrehozott virtuális géphez, felveheti az alábbi lépéseket a hálózati kapcsolat:

#### <a name="step-1"></a>Lépés: 1

A betöltés terheléselosztó erőforrás betöltése változó (Ha még nem elkészült, amely még). A változó használható $lb neve, és használja a ugyanazt az előbb létrehozott betöltés terheléselosztó erőforrás nevét.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Lépés: 2

Töltse be a változó kódmentes konfigurációt.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>3 lépés

A korábban létrehozott kapcsolat betöltése változó. a változó használható nevem $adaptert A hálózati kapcsolat használt név megegyezik a fenti példában.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Lépés: 4

A hálózati kapcsolaton kódmentes beállításainak módosításához.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>5 lépésben

A hálózati kapcsolat objektum mentéséhez.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

A hálózati kapcsolat hozzáadása a betöltés terheléselosztó kódmentes kvótáját után fogadása hálózati forgalmának engedélyezésére, a terheléselosztás anyagerőforráshoz, betöltés terheléselosztó szabályok alapján kezdi.

## <a name="update-an-existing-load-balancer"></a>Egy meglévő terheléselosztó frissítése


### <a name="step-1"></a>Lépés: 1

A fenti példa a terheléselosztó használ, betöltés terheléselosztó objektum hozzárendelése változó $slb Get-AzureRmLoadBalancer használatával

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Lépés: 2

A következő példában fog bejövő hálózati Címfordítást 81 az előtér és portot 8181 használata a hátsó készletre egy meglévő terheléselosztó az új szabály hozzáadása

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>3 lépés

Az új konfiguráció használata a Set-AzureLoadBalancer mentéséhez

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Egy terheléselosztó eltávolítása

A paranccsal eltávolítása-AzureRmLoadBalancer egy korábban létrehozott terheléselosztó "NRP-LB" erőforráscsoport az úgynevezett "NRP-RG" nevű törlése

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] A választható használható váltás - elkerülése érdekében a figyelmeztető üzenet törlés hatályba.



## <a name="next-steps"></a>Következő lépések

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)