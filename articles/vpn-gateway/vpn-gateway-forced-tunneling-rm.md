<properties 
   pageTitle="Kényszerített tunneling hely közötti kapcsolatok az erőforrás-kezelő telepítési modell beállítása |} Microsoft Azure"
   description="Hogyan szeretné irányítani, illetve "kényszerítése" összes internetes kötött forgalmat a helyszíni helyére."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Az erőforrás-kezelő Azure telepítési modell kényszerített tunneling konfigurálása

> [AZURE.SELECTOR]
- [A PowerShell - klasszikus](vpn-gateway-about-forced-tunneling.md)
- [A PowerShell - erőforrás-kezelő](vpn-gateway-forced-tunneling-rm.md)

Kényszerített tunneling meghatározhatja, átirányítás vagy a "kötelező" összes internetes kötött forgalom biztonsági másolatot a helyszíni helyre a webhely VPN alagutas vizsgálati és naplózási keresztül. Ez a legtöbb nagyvállalati informatikai kritikus biztonsági követelmény házirendeket.

Kényszerített tunneling nélkül Internet kötött-forgalmat a VMs Azure-ban a fog mindig bejárása az Azure hálózati infrastruktúrát közvetlenül ki az internethez, a vezérlőt, amellyel lehetővé teszi, hogy vizsgálja meg, vagy a forgalmat a naplózandó nélkül. Ezzel az illetéktelen Internet-hozzáférés esetleg vezethet illetéktelen vagy más típusú biztonsági megsértése

Ez a cikk végigvezeti konfigurálása, kényszerített az erőforrás-kezelő telepítési modell alapján létre virtuális hálózatok tunneling.

**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Telepítési modellek és a kényszerített bújtatásra eszközök**

Kényszerített közötti kapcsolatot a klasszikus telepítési modell és az erőforrás-kezelő telepítési modell beállíthatók. További információt az alábbi táblázatban látható. Új cikkek, új telepítési modellek és további eszközök ebben a konfigurációban elérhetővé válnak, frissítjük a táblázatban. Egy cikk érhető el, ha azt csatolása közvetlenül az értékeket.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Névjegy kényszerített tunneling


A következő diagramon azt szemlélteti, hogyan kényszerített közötti működik. 

![Kényszerített Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

A fenti példában az alhálózathoz nem kényszerített. Frontend bújtatott. A feladatok a Frontend alhálózat az elfogadás és ügyfél kérelmekre válaszolni az internetről közvetlenül is. A réteg közép- és Kódmentes alhálózat tudnak bújtatott. Minden kimenő kapcsolatok, a következő két alhálózat az internethez kényszerített vagy egy helyszíni webhelyen keresztül az S2S VPN alagutakat átirányítva lesz.

Lehetővé teszi, hogy korlátozza, és nézze meg a virtuális gépeken futó Internet hozzáférést vagy felhőszolgáltatásokba az Azure, miközben a több szálon szolgáltatás architektúra szükséges ahhoz, hogy továbbra. Is alkalmazhat kényszerített tunneling a teljes virtuális hálózatokhoz ha vannak olyan nincs internetes munkaterhelésekből a virtuális hálózatok.

## <a name="requirements-and-considerations"></a>Követelmények és kapcsolatos szempontok

Kényszerített tunneling Azure virtuális hálózati a felhasználó által definiált útvonalak keresztül van beállítva. Forgalom átirányítása a helyszíni webhely alapértelmezett útvonalat az Azure virtuális Magánhálózati átjáró kifejezve. A felhasználó által definiált Útválasztás és a virtuális hálózatok kapcsolatos további tudnivalókért olvassa el a [felhasználó által definiált útvonalak és IP-továbbítás](../virtual-network/virtual-networks-udr-overview.md)című témakört.

- Minden egyes virtuális hálózati alhálózat rendszer beépített, útválasztási táblázat tartalmaz. A rendszer útválasztási táblázat tartalmaz, a következő három csoport útvonalak:

    - **Helyi VNet irányítja:** Közvetlenül az a cél VMs virtuális ugyanabba a hálózatba
    
    - **Helyszíni útvonalak:** Az Azure virtuális Magánhálózati átjáró
    
    - **Alapértelmezett útvonal:** Közvetlenül az internetkapcsolat. A saját IP-címek nem vonatkozik az előző két útvonalak szánt csomagokat, azokat a program eltávolítja.

-  Ez az eljárás útválasztási tábla alapértelmezett út hozzáadása és társíthatja az útválasztási táblázat létrehozása a felhasználó által definiált útvonalak (UDR) segítségével a VNet subnet(s) ahhoz, hogy ezek alhálózat kényszerített tunneling.

- Kényszerített tunneling kell társítani egy VNet, amely tartalmaz egy útvonal-alapú virtuális Magánhálózati átjárót. Kell beállítania egy "alapértelmezett hely" az idegen helyszíni helyi webhelyek között a virtuális hálózathoz csatlakoztatva.

- Kényszerített tunneling készült ExpressRoute van beállítva, ez az eljárás keresztül, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat a készült ExpressRoute BGP peering munkamenetek keresztül hirdetési. Talál további információt a [Készült ExpressRoute dokumentációt](https://azure.microsoft.com/documentation/services/expressroute/) .

## <a name="configuration-overview"></a>Beállítások áttekintése

Az alábbi eljárás segít hozzon létre egy erőforrás csoportot, és egy VNet. Kell majd létrehozott egy virtuális Magánhálózati átjárót és kényszerített tunneling. Ez az eljárás a virtuális hálózat "MultiTier-VNet" 3 alhálózat tartalmaz: *Frontend* *Midtier*és *Kódmentes*4 határokon helyszíni kapcsolatok: *DefaultSiteHQ*, és a 3 *használja*.

A lépésekkel a *DefaultSiteHQ* legyen az alapértelmezett webhely kapcsolat a kényszerített tunneling és a Midtier beállítása és használata Kódmentes alhálózat kényszerített tunneling.

    
## <a name="before-you-begin"></a>Első lépések

Ellenőrizze, hogy rendelkezik-e az alábbi elemek a konfigurációban megkezdése előtt.

- Egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).

- Telepítse az Azure erőforrás-kezelő PowerShell-parancsmagok (1.0 vagy újabb) legújabb verzióját kell. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.


## <a name="configure-forced-tunneling"></a>Kényszerített tunneling konfigurálása

1. A PowerShell konzolban jelentkezzen be az Azure-fiókjába. Ezzel a parancsmaggal, a bejelentkezési hitelesítő adatokat kér az Azure-fiók. Naplózás után letöltés a Fiókbeállítások, hogy Azure PowerShell elérhetők legyenek.

        Login-AzureRmAccount 

2. Ismerkedés az Azure-előfizetések listáját.

        Get-AzureRmSubscription

2. Adja meg az előfizetést, szeretné használni. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Hozzon létre egy erőforrás csoportot.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Hozzon létre egy virtuális hálózatot, és adja meg a alhálózat. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. A helyi hálózaton átjárók létrehozása.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Az útvonal tábla és az útvonal szabály létrehozása

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Rendelje hozzá az útvonal táblázat a Midtier és Kódmentes alhálózathoz.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Hozzon létre egy alapértelmezett hely az átjárót. Ezt a lépést befejezéséhez, időnként 45 perc, vagy több, mert létrehozását, és az átjáró beállítása egy kis időt vesz igénybe.<br> A `-GatewayDefaultSite` a parancsmag paraméter, amely lehetővé teszi, hogy a kényszerített útválasztási konfiguráció dolgozik, ezért ügyeljen arra, állítsa be a megfelelő ezt a beállítást. Ezt a paramétert megtalálható PowerShell 1.0 vagy újabb verziója.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. A webhely virtuális Magánhálózati kapcsolatot létrehozni.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



