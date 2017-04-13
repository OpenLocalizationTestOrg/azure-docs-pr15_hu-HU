<properties
    pageTitle="Általános hálózati PowerShell parancsok VMs |} Microsoft Azure"
    description="Közös PowerShell-parancsok az első lépések az VMs virtuális hálózat és a hozzá kapcsolódó erőforrásokat létrehozását."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Általános hálózati Azure az PowerShell-parancsait VMs

Virtuális gép létrehozása szeretné, ha kell hozzon létre egy [virtuális hálózati](../virtual-network/virtual-networks-overview.md) vagy egy meglévő virtuális hálózat felvehetők a virtuális tudni. Általában egy virtuális létrehozásakor, is kell a jelen cikkben ismertetett erőforrások létrehozása.

Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) információt az Azure PowerShell legújabb verzióját, jelölje ki az előfizetés, és bejelentkezés az a fiókjába.

## <a name="create-network-resources"></a>Hálózati erőforrások létrehozása

Tevékenység | Parancs 
-------------- | -------------------------
Alhálózat konfigurációk létrehozása | $Alhalozat_1 [Új-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) =-neve "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$Alhalozat_2 új-AzureRmVirtualNetworkSubnetConfig =-neve "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Egy tipikus hálózati esetleg egy [internetes szemben lévő terheléselosztó](../load-balancer/load-balancer-internet-overview.md) alhálózat és egy [belső terheléselosztó](../load-balancer/load-balancer-internal-overview.md)külön alhálózat. |
Hozzon létre egy virtuális hálózaton | $vnet [Új-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) =-neve "virtual_network_name" - ResourceGroupName "resource_group_name"-hely "location_name" - AddressPrefix XX. X.X.X/XX-alhálózat $Alhalozat_1, $Alhalozat_2
Egy egyedi tartománynevet tesztelése | [Próba-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "tartománynév"-"location_name" helye<BR><BR>Megadhatja, hogy egy [nyilvános IP-erőforrás](../virtual-network/virtual-network-ip-addresses-overview-arm.md), amely hoz létre egy hozzárendelése a nyilvános IP-címére domainname.location.cloudapp.azure.com Azure kezelt DNS Servers tartománynévre. A név csak betűket, számokat és kötőjelet tartalmazhat. Az első és utolsó karakter betűvel kell lennie, vagy a szám és a tartománynév az Azure helyre belül egyedinek kell lennie. Ha **Igaz** van, a javasolt neve globálisan egyedi.
Hozzon létre egy nyilvános IP-cím | $pip [Új-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) =-neve "ip_address_name" - ResourceGroupName "resource_group_name" - DomainNameLabel "tartománynév"-hely "location_name" - AllocationMethod dinamikus<BR><BR>A nyilvános IP-címet a korábban már vizsgálni és a terheléselosztó frontend konfigurációja által használt tartomány nevét használja.
Hozzon létre egy frontend IP-beállítása | $frontendIP [Új-AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) =-neve "frontend_ip_name" - PublicIpAddress $pip<BR><BR>A frontend konfiguráció tartalmazza az a nyilvános IP-címet, amely a korábban létrehozott bejövő hálózati forgalmat.
Cím kódmentes készlet létrehozása | $beAddressPool [Új-AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) =-neve "backend_pool_name"<BR><BR>Belső címeket biztosít a kódmentes a terheléselosztó is elérhető, hálózati kapcsolaton keresztül.
Hozzon létre egy vizsgálati | $healthProbe [Új-AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) =-neve "probe_name" - RequestPath "HealthProbe.aspx"-Protocol http-80-as Port - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Állapot szondákat elérhetőségének megállapítása a kódmentes cím készletben virtuális gépeken futó példányok használt tartalmazza.
Terheléselosztási szabály létrehozása | $lbRule [Új-AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) =-neve HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-Probe $healthProbe-protokoll Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Egy nyilvános portjához a terheléselosztó hozzárendelése a kódmentes cím készletben porthoz szabályokat tartalmaz.
Bejövő hálózati Címfordítást szabály létrehozása | $inboundNATRule [Új-AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) =-neve "rule_name" - FrontendIpConfiguration $frontendIP-protokoll TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Egy nyilvános portjához a terheléselosztó hozzárendelése egy adott virtuális gép kódmentes cím készletben port szabályokat tartalmaz.
Hozzon létre egy terheléselosztó | $loadBalancer = "resource_group_name" [Új-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName-neve "load_balancer_name"-hely "location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-Probe $healthProbe
Hálózati kapcsolat létrehozása | $nic1 [Új-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName "resource_group_name" =-neve "network_interface_name"-hely "location_name" - PrivateIpAddress XX. X.X.X-alhálózat Alhalozat_2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>A nyilvános IP-cím és a korábban létrehozott virtuális alhálózathoz hálózati kapcsolat létrehozása.
    
## <a name="get-information-about-network-resources"></a>Hálózati erőforrások adatainak megtekintése

Tevékenység | Parancs 
-------------- | -------------------------
Lista virtuális hálózatok | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>A virtuális hálózatok az erőforráscsoport listája.
Virtuális hálózat adatainak megtekintése | Get-AzureRmVirtualNetwork-neve "virtual_network_name" - ResourceGroupName "resource_group_name"
Lista-alhálózat virtuális hálózatban | Get-AzureRmVirtualNetwork-neve "virtual_network_name" - ResourceGroupName "resource_group_name" & #124; Jelölje ki a alhálózat
Alhálózat adatainak megtekintése | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -neve "subnet_name" - VirtualNetwork $vnet<BR><BR>Az alhálózathoz információt megkapja a megadott virtuális hálózat. A $vnet értéket a Get-AzureRmVirtualNetwork által visszaadott objektum jelöli.
Lista IP-címek | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Az erőforráscsoport nyilvános IP-címek listája.
Lista terheléselosztókkal | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Az erőforráscsoport terheléselosztókkal listája.
Lista hálózati kapcsolatok | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Az erőforráscsoport minden hálózati kapcsolaton listája.
A hálózati kapcsolat adatainak megtekintése | Get-AzureRmNetworkInterface-neve "network_interface_name" - ResourceGroupName "resource_group_name"<BR><BR>Egy adott hálózati kapcsolat tájékoztatást kap.
Az IP-konfiguráció egy hálózati kapcsolat beszerzése | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -neve "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Az IP-konfiguráció megadott hálózati kapcsolat információt kap. A $nic értéket a Get-AzureRmNetworkInterface által visszaadott objektum jelöli.

## <a name="manage-network-resources"></a>Hálózati erőforrások kezelése

Tevékenység | Parancs 
-------------- | -------------------------
A virtuális hálózati alhálózat hozzáadása | [Adja hozzá AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX-neve "subnet_name" - VirtualNetwork $vnet<BR><BR>Alhálózat ad egy meglévő virtuális hálózathoz. A $vnet értéket a Get-AzureRmVirtualNetwork által visszaadott objektum jelöli.
Virtuális hálózat törlése | [Eltávolítás-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -neve "virtual_network_name" - ResourceGroupName "resource_group_name"<BR><BR>Az erőforráscsoport eltávolítja a megadott virtuális hálózat.
A hálózati kapcsolat törlése | [Eltávolítás-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -neve "network_interface_name" - ResourceGroupName "resource_group_name"<BR><BR>Az erőforráscsoport eltávolítja a megadott hálózati kapcsolaton.
Egy terheléselosztó törlése | [Eltávolítás-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -neve "load_balancer_name" - ResourceGroupName "resource_group_name"<BR><BR>Az erőforráscsoport eltávolítja a megadott terheléselosztó.
Egy nyilvános IP-cím törlése | [Eltávolítás-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-neve "ip_address_name" - ResourceGroupName "resource_group_name"<BR><BR>Az erőforráscsoport eltávolítja a megadott nyilvános IP-címet.

## <a name="next-steps"></a>Következő lépések

- Az imént létrehozott mikor hálózati felület használata [Hozzon létre egy virtuális](virtual-machines-windows-ps-create.md).
- További tudnivalók: hogyan létrehozhat [egy több hálózati kapcsolaton, virtuális](../virtual-network/virtual-networks-multiple-nics.md).
