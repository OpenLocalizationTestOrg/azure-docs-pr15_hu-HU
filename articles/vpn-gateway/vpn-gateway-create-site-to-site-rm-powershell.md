<properties
   pageTitle="Hozzon létre egy virtuális hálózati hely közötti virtuális Magánhálózati kapcsolatot Azure erőforrás-kezelő és a PowerShell használatával |} Microsoft Azure"
   description="Ez a cikk végigvezeti létrehozása egy VNet, az erőforrás-kezelő telepítési modell használ, és csatlakoztatása az átjáró S2S virtuális Magánhálózati kapcsolat használata helyi a helyszíni hálózaton."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Hozzon létre egy VNet hely közötti kapcsolatot PowerShell használatával

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klasszikus – klasszikus portál](vpn-gateway-site-to-site-create.md)

Ez a cikk végigvezeti virtuális hálózat és a helyszíni hálózaton, az erőforrás-kezelő Azure telepítési modell webhely VPN átjáró kapcsolat létrehozásához. Hely közötti kapcsolatok használható határokon helyszíni és hibrid konfigurációk.

![Webhely diagram] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "webhely-webhely") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Telepítési modellek és a hely közötti kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

A következő táblázat mutatja a jelenleg elérhető telepítési modellek és a webhely konfigurációk módszereket. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>További beállítások

Ha szeretné VNets összekapcsolása, de ne hozzon létre kapcsolatot a helyszíni helyre, olvassa el a [VNet-VNet kapcsolat konfigurálása](vpn-gateway-vnet-vnet-rm-ps.md)című témakört. Szeretne hely közötti kapcsolat hozzáadása egy VNet, amely már rendelkezik egy kapcsolatot, lásd: [a meglévő VPN-átjáró kapcsolattal VNet S2S kapcsolat](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Első lépések

Ellenőrizze, hogy rendelkezik-e az alábbi elemek konfigurációs megkezdése előtt.

- Kompatibilis VPN eszköz és olyannak, aki tudja állítani. Című témakörben [olvashat a virtuális Magánhálózati eszközöket](vpn-gateway-about-vpn-devices.md). Ha nem ismeri a virtuális Magánhálózati eszköze konfigurálása, vagy nem ismeri a tartományokat található IP-címét a helyszíni hálózati konfigurációja, a koordinálása másokkal, akik nyújtanak a adatai jelennek meg a szükséges.

- Külső felhasználókkal szemben lévő nyilvános IP-címének a virtuális Magánhálózati eszközt. Az IP-cím nem található meg az mögött egy hálózati címfordítást.
    
- Egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
    
- Az Azure erőforrás-kezelő PowerShell-parancsmagok legújabb verzióját. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.


## <a name="Login"></a>1. csatlakoztatása az előfizetés 

Győződjön meg arról, hogy az erőforrás-kezelő parancsmagok használatához a PowerShell módba vált. További tudnivalókért olvassa el a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)című témakört.

Nyissa meg a PowerShell konzolt, és csatlakozni a fiókjához. Használja az alábbi példa csatlakozni:

    Login-AzureRmAccount

Jelölje be az előfizetések a fiókhoz.

    Get-AzureRmSubscription 

Adja meg az előfizetést, szeretné használni.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. a virtuális hálózati és az átjáró alhálózat létrehozása

A példákban /28 az átjáró alhálózat. Lehet létrehozni egy átjáró alhálózat minél kisebb méretű /29 lép, azt javasoljuk, hogy legalább /28 vagy /27 bejelölésével több címet tartalmazó nagyobb alhálózat hozza létre. Ez az esetleges további beállításokat, hogy szükség lehet a jövőben igazodik elég címek lehetővé teszi.

Ha már van egy átjáró alhálózat, amely egy virtuális hálózattal/29 vagy nagyobb, akkor ugorhat [a helyi hálózati átjáró hozzáadása](#localnet)gombra.


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>A virtuális hálózati és az átjáró alhálózat létrehozása

A következő példa használatával hozzon létre egy virtuális hálózati és az átjáró alhálózat. Helyett a saját értékeit. 

Első lépésként az erőforráscsoport létrehozása:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Ezután hozzon létre virtuális hálózatához. Győződjön meg arról, hogy a megadott cím szóközöket nem áll átfedésben bármelyik, hogy a helyszíni hálózaton van a cím szóközöket.

A következő példa létrehoz egy virtuális hálózati *testvnet* és két alhálózat, egy úgynevezett *GatewaySubnet* nevű fájlt, és a más néven *Alhalozat_1*. Fontos, hogy hozzon létre egy alhálózat kifejezetten *GatewaySubnet*nevű. Nevezze el mást, ha a kapcsolat beállítása sikertelen lesz. 

Állítsa az a változó.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Hozzon létre a VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Egy átjáró alhálózat hozzáadása a már létrehozott egy virtuális hálózaton

Ez a lépés nem kötelező, csak akkor, ha egy átjáró alhálózat hozzáadása egy korábban létrehozott VNet kell.

Átjáró alhálózathoz a következő példa használatával hozhat létre. Ügyeljen arra, hogy az átjáró alhálózat "GatewaySubnet" nevet. Ha, nevezze el mást, alhálózat létrehozása, de Azure nem tekinti azt egy átjáró alhálózat.

Állítsa az a változó.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Az átjáró alhálózat létrehozása.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

A konfiguráció beállítása. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. a <a name="localnet"> </a>a helyi hálózati átjáró hozzáadása

Egy virtuális hálózaton a helyi hálózati átjáró általában utal, amelyet a helyszíni helyre. A webhely adjon nevet időegységgel Azure adhat a hivatkozott és a cím helyet előtagot a helyi hálózaton átjáró is megadhatja. 

Azure használja a IP-cím azt megadhatja, hogy melyik forgalmat a helyszíni helyre küldése azonosítása előtagját. Ez azt jelenti, hogy be kell állítania az egyes a helyi hálózati átjáró társítani kívánt cím előtagját. Ezeket a prefixumokban egyszerűen hozzáigazítható, ha megváltozik a helyszíni hálózaton. 

Amikor a PowerShell példák, vegye figyelembe az alábbiakat:
    
- A *GatewayIPAddress* a helyszíni VPN eszköz IP-címét. A virtuális Magánhálózati eszköz nem található meg az mögött egy hálózati címfordítást. 
- A *AddressPrefix* az Ön a helyszíni címet.

A helyi hálózaton átjáró egyetlen cím előtaggal hozzáadása:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

A helyi hálózaton átjáró több címet előtaggal hozzáadása:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>A helyi hálózaton átjáró IP-cím prefixumokban módosítása

Előfordul, hogy a helyi hálózaton átjáró prefixumokban módosítása. Az IP-cím prefixumokban módosításának lépései attól függenek, hogy átjáró virtuális Magánhálózati kapcsolatot hozott létre. Ez a cikk [módosítása IP cím prefixumokban a helyi hálózaton átjáró](#modify) című.


## <a name="PublicIP"></a>4. a virtuális Magánhálózati átjárót egy nyilvános IP-címe kérése

Ezután kérése az Azure VNet virtuális Magánhálózati átjáró kiosztandó egy nyilvános IP-címet. Ez nem ugyanazt a címet, amely a virtuális Magánhálózati eszköz; hozzá van rendelve inkább ezt a jogosultságot az Azure virtuális Magánhálózati átjáró magát. Az IP-címet, amely a használni kívánt nem adhat meg. Dinamikusan oszlik a átjáróhoz. Használhatja az IP-cím a helyszíni VPN eszköz konfigurálásakor csatlakozni az átjárót.

Az Azure virtuális Magánhálózati átjárót, az erőforrás-kezelő telepítési modell jelenleg csak nyilvános IP-címek támogatja a dinamikus terhelés módszerrel. Ez azonban nem jelenti azt az IP-cím változik. A csak az Azure virtuális Magánhálózati gateway IP-cím változását, amikor az átjáró törlődik, és újból létrehozza. Az átjáró nyilvános IP-cím átméretezése alaphelyzetbe állítása és más belső karbantartási/frissítések az Azure virtuális Magánhálózati átjáró át nem változik.

Használja az alábbi PowerShell példa:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. az átjáró IP-címzésének konfiguráció létrehozása

Az átjáró konfiguráció határozza meg, az alhálózathoz és a nyilvános IP-címet használja. A következő példa használatával hozhat létre az átjáró konfigurációt.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. a virtuális hálózati átjáró létrehozása

Ebben a lépésben a virtuális hálózati átjáró hoz létre. Az átjárók létrehozása hosszú időt vesz igénybe vehet igénybe. Legalább gyakran 45 percet vesz igénybe. 

A következő értékeket használja:

- A webhely konfiguráció *– GatewayType* *Vpn*. Az átjáró típus értéke mindig a beállításokat, amelyek vannak végrehajtási alkalmazásra. Más átjáró konfigurációk például - GatewayType készült ExpressRoute lehet szükség. 

- A *-VpnType* lehet *RouteBased* (néven néhány dokumentációjában dinamikus átjárók), illetve *PolicyBased* (néven néhány dokumentációjában statikus átjárók). Virtuális Magánhálózati átjáró típusok kapcsolatos további tudnivalókért olvassa el a [Virtuális Magánhálózati átjárók](vpn-gateway-about-vpngateways.md#vpntype)című témakört.
- A *-GatewaySku* lehet *egyszerű*, *normál*vagy *HighPerformance*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. a virtuális Magánhálózati eszköz beállítása

Ezen a ponton van szüksége a nyilvános IP-címet a virtuális hálózati átjáró a helyszíni VPN eszköz beállítása. Dolgozhat a számítógépgyártó, bizonyos beállítások adatait. További információt a [Virtuális Magánhálózati eszközöket](vpn-gateway-about-vpn-devices.md) is hivatkozhat.

A nyilvános IP-címet a virtuális hálózati átjáró megkereséséhez használja a következő példa:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. a virtuális Magánhálózati kapcsolat létrehozása

Ezután a webhely virtuális Magánhálózati kapcsolat létrehozása a virtuális hálózati átjáró és között VPN eszközére. Ügyeljen arra, hogy az értékek lecserélése saját. A megosztott kulcs egyeznie kell a virtuális Magánhálózati eszköz konfigurációhoz használt értékét. Figyelje meg, hogy a `-ConnectionType` a webhely *IPsec*. 

Állítsa az a változó.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

A kapcsolat létrehozása.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Miután a kapcsolat során egy rövid jön létre. 

## <a name="toverify"></a>A virtuális Magánhálózati kapcsolat ellenőrzése

Ellenőrizze a virtuális Magánhálózati kapcsolat néhány másik módja van.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>IP cím prefixumokban a helyi hálózaton átjáró módosítása

Ha módosítania kell a prefixumokban a helyi hálózaton átjáró, kövesse az alábbi lépéseket. Két adathalmaz utasítások találhatók. A képernyőn megjelenő utasításokat, kiválaszthatja, hogy attól függenek, hogy már korábban elkészült az átjáró kapcsolat. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>A helyi hálózati átjáró gateway IP-címének módosítása

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Következő lépések

- A virtuális hálózatok virtuális gépeken futó vehet. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.

- BGP tudni lásd: a [BGP – áttekintés](vpn-gateway-bgp-overview.md) és [konfigurálásáról BGP](vpn-gateway-bgp-resource-manager-ps.md).

