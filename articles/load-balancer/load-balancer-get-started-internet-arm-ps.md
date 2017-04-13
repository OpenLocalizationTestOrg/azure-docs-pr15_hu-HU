<properties
   pageTitle="Erőforrás-kezelő egy internetes terheléselosztó létrehozása PowerShell használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó az erőforrás-kezelő PowerShell használatával"
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

# <a name="get-started"></a>Erőforrás-kezelő egy internetes terheléselosztó létrehozása PowerShell használatával

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó a klasszikus telepítési modell használatával](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>A megoldást üzembe helyezné Azure PowerShell használatával

Az alábbi eljárások bemutatják, hogyan hozhat létre egy internetes terheléselosztó Azure erőforrás-kezelő a PowerShell használatával. Az Azure erőforrás-kezelő, az egyes erőforrások létrehozott és beállított külön-külön, és, majd írja a terheléselosztó létrehozása.

Kell létrehozni, és állítsa be a következő objektumoknak egy terheléselosztó telepítése:

- Előtér-IP-beállítások: a bejövő hálózati forgalmat a nyilvános IP (PIP) címeket tartalmazza.
- Készlet háttéradatbázist cím: a virtuális gépeken futó hálózati forgalmának engedélyezésére fogadjon a terheléselosztó hálózati kapcsolatok (NIC) tartalmazza.
- Szabályok terheléselosztási: a terheléselosztó egy nyilvános portjához feleltesse meg a háttéradatbázis cím készletben port szabályokat tartalmaz.
- Bejövő szabályok hálózati Címfordítást: a terheléselosztó egy nyilvános portjához feleltesse meg a háttéradatbázis cím készletben adott virtuális géphez port szabályokat tartalmaz.
- Szondákat: állapot szondákat elérhetőségének megállapítása a háttéradatbázist cím készletben virtuális gép példányainak használt tartalmazza.

További tudnivalókért lásd: [Azure erőforrás-kezelő terheléselosztó támogatása](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Állítsa be a PowerShell erőforrás-kezelő

Győződjön meg arról, hogy a Powershellhez készült Azure erőforrás-kezelő modul legújabb gyártási verziójával rendelkezik:

1. Jelentkezzen be az Azure.

        Login-AzureRmAccount

    Adja meg a hitelesítő adatait, amikor a rendszer kéri.

2. Jelölje be az előfizetések a fiókhoz.

        Get-AzureRmSubscription

3. A használandó Azure előfizetések kiválasztása.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Hozzon létre egy erőforrás csoportot. (Ugorja át ezt a lépést az erőforrás meglévő csoport használata.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Virtuális hálózat és a nyilvános IP-címet az előtér-IP-készlet létrehozása

1. Hozzon létre egy alhálózat és virtuális hálózat.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Hozzon létre egy Azure nyilvános IP-cím erőforrás, névvel ellátott **PublicIP**a DNS-neve **loadbalancernrp.westus.cloudapp.azure.com**az előtér-IP erőforráskészlethez tartozik való használatra. A következő parancsot a statikus terhelés típust használja.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]A terheléselosztó használja a tartomány címke nyilvános IP-előtaggal Tartománynevének. Ez különbözik a klasszikus telepítési modell, amely a felhőalapú szolgáltatást használja, mint a terheléselosztó FQDN.
    >Ebben a példában a teljesen minősített tartománynév **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Hozzon létre egy előtér-IP-készlet és a háttéradatbázist cím készletben

1. Használja az **PublicIp** erőforrást **LB-Frontend** nevű előtér-IP-készlet létrehozása.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Névvel ellátott **LB-kódmentes**háttéradatbázist cím készlet létrehozása.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Hálózati Címfordítást szabályokat, a betöltés terheléselosztó szabályok, egy vizsgálati és egy terheléselosztó létrehozása

Ez a példa hozza létre az alábbiakat:

- Ha port 3441 porthoz 3389 minden bejövő forgalmát hálózati Címfordítást szabály
- Ha port 3442 porthoz 3389 minden bejövő forgalmát hálózati Címfordítást szabály
- Állapot ellenőrzése egy lapon a vizsgálati szabályok nevű **HealthProbe.aspx**
- A címek a háttéradatbázist készletben 80-as portjához 80-as port minden bejövő forgalmát egyenleg a betöltés terheléselosztó szabályok
- Egy terheléselosztó ezeknek az objektumoknak használó

Használja az alábbi lépéseket:

1. A hálózati Címfordítást szabályok létrehozásához.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Hozzon létre egy állapot vizsgálati. Kétféleképpen egy vizsgálati konfigurálása:

    HTTP-vizsgálati

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    TCP-vizsgálati

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Hozzon létre egy betöltés terheléselosztó szabályt.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Hozzon létre a terheléselosztó a korábban létrehozott objektumok használatával.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>NIC létrehozása

Hálózati kapcsolatok létrehozása (vagy módosíthatja a meglévőket), és ezután hálózati Címfordítást szabályokat, betöltés terheléselosztó szabályok és szondákat társítson:

1. Hozzáférhet a virtuális hálózat és virtuális hálózati alhálózat, ahol a NIC hozható létre kell.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Hozzon létre egy hálózati kártya elnevezett **lb nic1 kell**, és a társítani hálózati Címfordítást szabályt, és az első (és csak) háttéradatbázist cím készlet.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Hozzon létre egy hálózati kártya elnevezett **lb nic2 kell**, és a társítani a második hálózati Címfordítást szabály, és az első (és csak) háttéradatbázist cím készlet.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Jelölje be a NIC.

        $backendnic1

    Várt kimenet:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
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
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Használja a `Add-AzureRmVMNetworkInterface` parancsmagot a NIC rendel a különböző VMs.

## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

Virtuális gép létrehozása és hozzárendelése egy hálózati, olvassa el [az Azure virtuális létrehozása a PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>A hálózati kapcsolat hozzáadása a terheléselosztó

1. A terheléselosztó lekérése az Azure.

    A betöltés terheléselosztó erőforrás betöltése változó (Ha még nem elkészült, amely még). A változó **$lb**neve. Ugyanaz a neve a betöltés terheléselosztó erőforrás, amely a korábban létrehozott használja.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Változó konfiguráció betöltése a háttéradatbázist.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. A korábban létrehozott kapcsolat betöltése változó. A változó neve **$nic**. A korábbi példából ugyanazt a hálózati kapcsolat neve lesz.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. A hálózati kapcsolaton háttéradatbázist beállításainak módosításához.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. A hálózati kapcsolat objektum mentéséhez.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    A hálózati kapcsolat hozzáadása a betöltés terheléselosztó háttéradatbázist kvótáját után elindul fogadása alapján terheléselosztási betöltés terheléselosztó anyagerőforráshoz, hálózati forgalmának engedélyezésére.

## <a name="update-an-existing-load-balancer"></a>Egy meglévő terheléselosztó frissítése

1. A korábbi példából a terheléselosztó használatával hozzárendelése egy betöltés terheléselosztó objektumot a változó **$slb** használatával `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. Az alábbi példában egy bejövő hálózati Címfordítást szabályt – a háttéradatbázist készlet – egy meglévő terheléselosztó előtér-készletben 81 és portot 8181 használatával hozzáadása.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Mentse az új konfiguráció `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Egy terheléselosztó eltávolítása

A paranccsal `Remove-AzureLoadBalancer` törlése egy korábban létrehozott terheléselosztó **NRP-LB** nevű erőforráscsoport nevű **NRP-RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] A választható kapcsoló használható **-hatályba** elkerülése érdekében a kérdés törlése.

## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
