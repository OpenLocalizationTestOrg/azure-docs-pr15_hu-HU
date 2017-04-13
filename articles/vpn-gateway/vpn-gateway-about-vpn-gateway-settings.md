<properties 
   pageTitle="A virtuális Magánhálózati átjáró beállításai virtuális hálózati átjárók |} Microsoft Azure"
   description="Tudjon meg többet az Azure virtuális hálózat beállításainak virtuális Magánhálózati átjárót."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>A virtuális Magánhálózati átjáró beállításai

Átjáró virtuális Magánhálózati kapcsolat megoldást alapul több erőforrások konfigurációja annak érdekében, hogy a hálózati forgalmat virtuális hálózatok között, és a helyszíni helyek. Az egyes erőforrások konfigurálható beállításokat tartalmaz. Az erőforrások és a beállítások kombinációja határozza meg, hogy az internetkapcsolat így.

Ebben a cikkben a szakaszokból megtudhatja, hogy az erőforrások és a beállítások, amely egy virtuális Magánhálózati átjárót, az **Erőforrás-kezelő** telepítési modell vonatkoznak. Akkor lehet hasznos lehet a rendelkezésre álló beállítások megtekintése az internetkapcsolat topológiájának diagramok segítségével. A leírásokat és topológia diagramok minden kapcsolat megoldást [Virtuális Magánhálózati átjáró](vpn-gateway-about-vpngateways.md) című témakörben talál. 

## <a name="gwtype"></a>Átjáró típusai

Minden egyes virtuális hálózati csak lehet egy virtuális hálózati átjáró minden típusú. A virtuális hálózati átjáró hoz létre, amikor győződjön meg róla, hogy az átjáró írja be az Ön konfigurációjának megfelelő.

A rendelkezésre álló - GatewayType értékei a következők: 

- Virtuális magánhálózati
- Készült ExpressRoute

A virtuális Magánhálózati átjáró van szükség a `-GatewayType` *Vpn*.  

Példa:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>Átjáró SKU


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Az átjáró Termékváltozat konfigurálása

**Az átjáró Termékváltozat megadása az Azure-portálon**

Ha az Azure portal segítségével erőforrás-kezelő virtuális hálózati átjáró létrehozása, választhat az átjáró Termékváltozat a legördülő menü használatával. A beállításokat, megjelenik az átjáró és VPN típusa választott felelnek meg.

Például ha bejelöli a átjáró "VPN", és a virtuális Magánhálózati típus "csoportházirend-alapú", látni az egyszerű"Termékváltozat csak az elérhető PolicyBased VPN adatai csak Termékváltozat, mivel. Ha "Útvonal-alapú" lehetőséget választja, választhat Basic, normál és HighPerformance termékváltozatok. 


**Az átjáró Termékváltozat megadása PowerShell használatával**


Az alábbi PowerShell példa Itt adhatja meg a `-GatewaySku` *szabvány*szerint.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Az átjárók Termékváltozat módosítása**

Ha szeretne egy nagyobb teljesítményű Termékváltozat az átjáró Termékváltozat frissítés (az HighPerformance Basic/normál) is használhatja a `Resize-AzureRmVirtualNetworkGateway` PowerShell-parancsmag. Az átjáró Termékváltozat méretét a parancsmaggal vissza is léptetheti.

Az alábbi PowerShell példában az átjárók HighPerformance átméretezése Termékváltozat.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>A becsült által átjáró Termékváltozat összesítő átviteli és típusa

A következő táblázat mutatja az átjáró-típusok és a becsült összesítő kapacitása. Az alábbi táblázat az erőforrás-kezelő és a klasszikus telepítési modellek vonatkozik.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Kapcsolatok típusai

Az erőforrás-kezelő telepítési modell minden konfiguráció szükséges egy adott virtuális hálózati átjáró kapcsolat típusa. A rendelkezésre álló erőforrás-kezelő PowerShell értékeinek `-ConnectionType` vannak:

- IPsec
- Vnet2Vnet
- Készült ExpressRoute
- VPNClient

A következő példában PowerShell hozzunk létre, amely a kapcsolat típusának *IPsec*igényel S2S kapcsolatot.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>Virtuális Magánhálózati típusai

A virtuális hálózati átjáró VPN átjáró konfiguráció létrehozásakor meg kell adnia a virtuális Magánhálózati típusát. A választott VPN-típus attól függ, hogy a létrehozni kívánt kapcsolatot topológiában. P2S kapcsolat például egy RouteBased VPN-típus van szükség. A hardver, amely fogja használni, akkor is befolyásolja a virtuális Magánhálózati típusú. S2S konfigurációk VPN-eszközhöz szükséges. Néhány virtuális Magánhálózati eszközök csak egy bizonyos típusú VPN támogatja.

A virtuális Magánhálózati típustól meg kell felelniük a minden kapcsolat a megoldáshoz szeretne létrehozni. Például ha szeretné az átjáró S2S virtuális Magánhálózati kapcsolat és virtuális hálózatához átjáró P2S virtuális Magánhálózati kapcsolat létrehozása, használja VPN-típus *RouteBased* mert P2S csak egy RouteBased VPN-típus. Ellenőrizze, hogy a virtuális Magánhálózati eszköz támogatott RouteBased virtuális Magánhálózati kapcsolat is kellene. 

A virtuális hálózati átjáró létrehozása után a VPN-típus nem módosítható. Ha törli a virtuális hálózati átjáró, és hozzon létre egy újat. Két VPN típusa létezik:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Az alábbi PowerShell példa Itt adhatja meg a `-VpnType` *RouteBased*szerint. Szeretne létrehozni egy átjáró, amikor győződjön meg róla, hogy a VpnType - az Ön konfigurációjának megfelelő. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Átjáró vonatkozó követelmények

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Átjáró alhálózat

Virtuális hálózati átjáró beállítása, először kell létrehozni egy átjáró alhálózat a VNet. Az átjáró alhálózat névvel kell *GatewaySubnet* működik megfelelően. Ez a név lehetővé teszi, hogy tudja, hogy az alhálózathoz kell használni az átjáró Azure.

Átjáró alhálózathoz minimális méretének teljesen ki a létrehozni kívánt konfigurációjától függ. Bár lehet létrehozni egy átjáró alhálózat minél kisebb méretű /29, azt javasoljuk, hogy hoz létre egy átjáró alhálózat /28 vagy nagyobb (/ 28, /27, /26, stb.). 

Átjáró nagyobb méretű létrehozása nem lehet az átjáró méretkorlátja futó. Például esetleg létrehozott egy virtuális hálózati átjáró átjáró alhálózat méretű /29 S2S kapcsolat. Most szeretne beállítani egy S2S/készült ExpressRoute konfigurációs megtalálhatók. A konfiguráció szükséges egy átjáró alhálózat minimális méret /28. A konfigurációban létrehozásához, szeretné, hogy az átjáró alhálózat kiterjesztve azt a kapcsolatot, amely /28 minimális kötelező módosításához szükséges engedélyekkel.

Az alábbi erőforrás-kezelő PowerShell példa bemutatja a GatewaySubnet nevű átjáró alhálózat. Megjelenik a CIDR jelölés meghatározza, hogy egy /27, lehetővé teszi, hogy elég IP-címek a legtöbb konfiguráció jelenleg megtalálható.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Helyi hálózaton átjárók

A virtuális Magánhálózati átjáró konfiguráció létrehozásakor a helyi hálózati átjáró gyakran jelöli a helyszíni helyét. A klasszikus telepítési modell a helyi hálózati átjáró is megtekinthetők és a helyi webhelyen. 

Nevezze el a helyi hálózati átjáró, a helyszíni VPN eszköz nyilvános IP-címét, és adja meg a cím prefixumokban a helyszíni az adott helyen található. Azure vizsgálja meg a célként megadott cím prefixumokban hálózati forgalmának beállítása, a beállításokat, amelyek a helyi hálózaton átjáró megadott olvas és csomagok megfelelően irányítja. Az átjáró virtuális Magánhálózati kapcsolatot használó VNet-VNet konfigurációk helyi hálózaton átjárók is adja meg.

Az alábbi PowerShell-példa létrehoz egy új helyi hálózaton átjáró:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Előfordul, hogy módosítania kell a helyi hálózati átjáró beállításait. Például amikor hozzáadása vagy módosítása a tartománya, vagy, ha a virtuális Magánhálózati eszköz IP-címére változik. A klasszikus VNet módosíthatja ezeket a beállításokat, a helyi hálózat lapon a klasszikus portálon. Az erőforrás-kezelő [Módosítás helyi hálózaton átjáróbeállítások PowerShell](vpn-gateway-modify-local-network-gateway.md)látható.

## <a name="resources"></a>REST API-hoz és a PowerShell-parancsmagok

További technikai források és adott szintaxiskövetelmények REST API-hoz és a PowerShell-parancsmagok használata a virtuális Magánhálózati átjáró konfigurációk esetén című témakörben talál az alábbi lapokon:

|**Klasszikus** | **Erőforrás-kezelő**|
|-----|----|
|[A PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[A PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API-VAL](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API-VAL](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Következő lépések

Lásd: a [Virtuális Magánhálózati átjáró](vpn-gateway-about-vpngateways.md) elérhető kapcsolat konfigurációk kapcsolatban további tudnivalókat. 







 
