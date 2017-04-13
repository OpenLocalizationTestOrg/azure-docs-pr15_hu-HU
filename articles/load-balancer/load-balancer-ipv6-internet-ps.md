<properties
    pageTitle="Hozzon létre egy internetes terheléselosztó, és az IPv6-erőforrás-kezelő PowerShell használatával |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó, és az IPv6-erőforrás-kezelő PowerShell használatával"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="az IPv6, azure terheléselosztó, kettős Papírhalom, nyilvános ip, natív ipv6, Mobiltelefonról, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Első lépések megtételében egy internetes terheléselosztó, és az IPv6-erőforrás-kezelő PowerShell használatával

> [AZURE.SELECTOR]
- [A PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sablon](./load-balancer-ipv6-internet-template.md)

Az Azure terheléselosztó az egy réteg-4-es (TCP, UDP) terheléselosztó. A terheléselosztó magas elérhetősége biztosítja terjesztése bejövő forgalom megfelelő szolgáltatás példányainak cloud Services vagy a betöltés terheléselosztó meg virtuális gépeken futó között. Azure terheléselosztó is bemutathatja több portok vagy több IP-címek szolgáltatások.

## <a name="example-deployment-scenario"></a>Példa telepítéshez

Az alábbi ábra szemlélteti a terheléselosztási megoldást üzembe helyezéséhez ebben a cikkben.

![Betöltési terheléselosztó eset](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Ebben az esetben a következő Azure erőforrások létrehoznia:

- az internetes terheléselosztó IPv4- és az IPv6 nyilvános IP-címet a
- két töltse be a nyilvános VIP hozzárendelése a magánjellegű végpontok terheléselosztó szabályok
- a két VMs egy elérhetősége értékre, amely tartalmazza.
- két virtuális gépeken futó (VMs)
- minden egyes virtuális társított IPv4 és az IPv6-címek a virtuális hálózati felhasználói felülete

## <a name="deploying-the-solution-using-the-azure-powershell"></a>A Powershellhez Azure használatával megoldást üzembe helyezné

A következő lépések bemutatják, hogyan hozhat létre egy internetes terheléselosztó Azure-kezelő a PowerShell használata. Az Azure erőforrás-kezelő, az egyes erőforrások jön létre, és konfigurált külön-külön, helyezze erőforrás létrehozása.

Egy terheléselosztó üzembe helyezéséhez létrehozása, és adja meg a következő objektumoknak:

- Előtér-IP-konfiguráció – a bejövő hálózati forgalmat a nyilvános IP-címeket tartalmazza.
- Háttéradatbázis cím alkalmazáskészlet - a virtuális gépeken futó hálózati forgalmának engedélyezésére fogadjon a terheléselosztó hálózati kapcsolatok (NIC) tartalmaz.
- Egy nyilvános portjához a terheléselosztó hozzárendelése a háttéradatbázist cím készletben port szabályok terheléselosztás szabályok - tartalmazza.
- Bejövő hálózati Címfordítást szabályok – a terheléselosztó egy nyilvános portjához hozzárendelése egy adott virtuális gép a háttéradatbázist cím készletben port szabályokat tartalmaz.
- Ellenőrzi,-állapot szondákat elérhetőségének megállapítása a háttéradatbázist cím készletben virtuális gépeken futó példányok használt tartalmazza.

További tudnivalókért lásd: [Azure erőforrás-kezelő terheléselosztó támogatása](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Állítsa be a PowerShell erőforrás-kezelő

Győződjön meg arról, hogy a Powershellhez készült Azure erőforrás-kezelő modul legújabb gyártási verziójával rendelkezik.

1. Jelentkezzen be az Azure

        Login-AzureRmAccount

    Adja meg a hitelesítő adatait, amikor a rendszer kéri.

2. Az előfizetések a fiók ellenőrzése

        Get-AzureRmSubscription

3. A használandó Azure előfizetések kiválasztása.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Erőforráscsoport (Ez a lépés erőforrás meglévő csoport használata kihagyja) létrehozása

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Virtuális hálózat és a nyilvános IP-címet az előtér-IP-készlet létrehozása

1. Hozzon létre egy virtuális hálózati alhálózat.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Azure nyilvános IP-cím (MEZŐPONT) erőforrások az előtér-IP-cím készlet létrehozása.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] A terheléselosztó használja a tartomány címke nyilvános IP-előtag a teljesen minősített tartománynév. Ebben a példában az a teljes tartományneve, hogy *lbnrpipv4.westus.cloudapp.azure.com* és *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Hozzon létre egy előfeldolgozó IP-beállításokat és a háttéradatbázist cím erőforráskészlethez tartozik

1. Hozzon létre használja a nyilvános IP-címeket létrehozott előtér-cím korlátozza.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Háttéradatbázis cím készletek létrehozása.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>LB szabályokat, hálózati Címfordítást szabályokat, a vizsgálati és egy terheléselosztó létrehozása

Ez a példa hozza létre az alábbiakat:

- hálózati Címfordítást szabály minden bejövő forgalmat a 443-as port porthoz 4443 fordítása
- betöltési terheléselosztó szabály minden bejövő forgalmat a címeket, a háttéradatbázist készletben 80-as portjához 80-as port egyenleg.
- a port 3389 VMs RDP kapcsolat engedélyezése betöltés terheléselosztó szabály.
- állapot ellenőrzése egy lapon a vizsgálati szabályok nevű *HealthProbe.aspx* vagy port 8080 szolgáltatás
- egy terheléselosztó ezeknek az objektumoknak használó

1. A hálózati Címfordítást szabályok létrehozásához.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Hozzon létre egy állapot vizsgálati. Kétféleképpen egy vizsgálati konfigurálása:

    HTTP-vizsgálati

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    vagy TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    Ebben a példában fogjuk használni a TCP szondákat.

3. Hozzon létre egy betöltés terheléselosztó szabályt.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Hozzon létre a terheléselosztó a korábban létrehozott objektumok használatával.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>A háttéradatbázist VMs NIC létrehozása

1. Ismerkedés a virtuális hálózati és virtuális alhálózathoz, ahol a NIC létre kívánja hozni.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. A VMs IP-beállításokat és a NIC hozhat létre.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Virtuális gépeken futó létrehozása és az újonnan létrehozott NIC hozzárendelése

A virtuális létrehozásával kapcsolatos további tudnivalókért lásd: [létrehozása, és adja meg a Windows Azure PowerShell-és erőforrás-kezelő virtuális gép előre](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Elérhetőség beállítása és a tárterület-fiók létrehozása

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Minden egyes virtuális létrehozása és hozzárendelése az előző NIC létrehozott

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
