<properties
   pageTitle="Aktív – aktív S2S VPN-kapcsolatok konfigurálása Azure virtuális Magánhálózati átjárók Azure erőforrás-kezelő és a PowerShell használatával |} Microsoft Azure"
   description="Ez a cikk végigvezeti aktív-aktív kapcsolatok konfigurálása az Azure virtuális Magánhálózati átjárók Azure erőforrás-kezelő és a PowerShell használatával."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Aktív – aktív S2S VPN kapcsolatainak konfigurálása az Azure virtuális Magánhálózati átjárók Azure erőforrás-kezelő és a PowerShell használatával

Ez a cikk végigvezeti az aktív-aktív határokon helyszíni és az erőforrás-kezelő telepítési modell és a PowerShell használatával VNet-VNet kapcsolatok létrehozásához.


**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Könnyen hozzáférhető határokon helyszíni kapcsolatokról

Magas az idegen helyszíni és VNet-VNet kapcsolódási eléréséhez kell több VPN átjáró telepítése, és a hálózatok között az Azure több párhuzamos kapcsolatok hozhatók létre. Tekintse át a [nagyon elérhető határokon helyszíni és VNet-VNet kapcsolódási](./vpn-gateway-highlyavailable.md) csatlakozási beállítások és topológia áttekintése.

Ebben a cikkben az utasításokat követve egy helyszíni határokon aktív-aktív virtuális Magánhálózati kapcsolatot, és két virtuális hálózatok között aktív-aktív kapcsolat beállítása:

- [1 – rész létrehozása és konfigurálása az Azure virtuális Magánhálózati átjáró aktív-aktív módban](#aagateway)

- [Része a 2 - határokon helyszíni aktív-aktív kapcsolat](#aacrossprem)

- [Rész 3 – az aktív-aktív VNet-VNet kapcsolatokat](#aav2v)

- [Rész 4 – az aktív-aktív és az aktív készenléti között létező átjáró frissítése](#aaupdate)

Ezek a közös Szerkesztés egy összetettebb, könnyen hozzáférhető hálózati topológiája, az igényeinek megfelelő is összevonhatja.

>[AZURE.IMPORTANT] Ne feledje, hogy az aktív-aktív mód csak akkor működik, a HighPerformance Termékváltozat


## <a name ="aagateway"></a>1 – rész létrehozása és konfigurálása az aktív-aktív VPN átjárók

Az alábbi lépésekkel beállítja az Azure virtuális Magánhálózati átjáró aktív-aktív elrendezésű naptárakra. Az aktív-aktív és az aktív készenléti átjárók közötti főbb különbségek vannak:

- Létre kell hoznia a két átjáró IP-konfigurációjának a két nyilvános IP-címek
- A EnableActiveActiveFeature jelző szükséges beállítása
- Az átjáró Termékváltozat HighPerformance kell lennie.

Az egyéb tulajdonságokat ugyanazok, mint az nem aktív aktív átjárók. 

### <a name="before-you-begin"></a>Első lépések

- Ellenőrizze, hogy rendelkezik-e egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
    
- Telepítse az Azure erőforrás-kezelő PowerShell-parancsmagok kell. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.

### <a name="step-1---create-and-configure-vnet1"></a>Lépés: 1 – létrehozása és konfigurálása VNet1

#### <a name="1-declare-your-variables"></a>1. a változók deklarálása

Ebben a gyakorlatban lássuk először, a változók deklarálása szerint. Az alábbi példában a változók használatával értékeket ez gyakorlásához deklarálása. Ügyeljen arra, hogy az értékek helyettesítése a saját gyártási konfigurálásakor. Ezek a változók is használhatja, ha futtatja a segítségével megismerkedhet az ilyen típusú konfigurációt lépéseit. Módosítsa a változók, majd másolja és illessze be a PowerShell konzol.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. a előfizetéshez hozhat létre, és új erőforráscsoport

Győződjön meg arról, hogy az erőforrás-kezelő parancsmagok használatához a PowerShell módba vált. További tudnivalókért olvassa el a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)című témakört.

Nyissa meg a PowerShell konzolt, és csatlakozni a fiókjához. Használja az alábbi példa csatlakozni:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. a TestVNet1 létrehozása

Az alábbi példa létrehoz egy virtuális hálózati TestVNet1 és három alhálózathoz, egy úgynevezett GatewaySubnet, egy úgynevezett FrontEnd és egy úgynevezett Kódmentes nevű. Amikor értékek helyettesítése, fontos mindig alhálózathoz átjáró neve kifejezetten GatewaySubnet. Nevezze el mást, ha az átjáró létrehozása sikertelen lesz. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Lépés: 2 – az aktív-aktív mód TestVNet1 a virtuális Magánhálózati átjáró létrehozása

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. a nyilvános IP-címek és az átjáró IP konfigurációk létrehozása

Kérése a két nyilvános IP-címek kiosztandó az átjáró a VNet hoz létre. Az alhálózathoz és IP-konfiguráció szükséges definiálása is fogja. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. a virtuális Magánhálózati átjáró létrehozása aktív-aktív konfiguráció

A virtuális hálózati átjáró TestVNet1 hozhat létre. Ne feledje, hogy vannak két GatewayIpConfig bejegyzései, a EnableActiveActiveFeature jelző be van állítva. Aktív – aktív módhoz HighPerformance Termékváltozat az útvonal alapú virtuális Magánhálózati átjáró. Az átjárók létrehozása igénybe vesz (30 percen vagy több befejezéséhez).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. szerezze be az átjáró nyilvános IP-címek és BGP Peer IP-címe

Amikor az átjáró létrejött, szüksége lesz beszerzése Azure virtuális Magánhálózati átjáró BGP Peer IP-címét. A cím konfigurálja az Azure virtuális Magánhálózati átjáró a helyszíni virtuális Magánhálózati eszközök egy BGP Peer van szükség.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Az alábbi parancsmag használatával a két nyilvános IP-címek a virtuális Magánhálózati átjárót, és a megfelelő BGP Peer IP-címek minden átjárópéldány kiosztott megjelenítése:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

A sorrend nyilvános IP-címei az átjáró-példányok és a megfelelő BGP Peering címeket megegyeznek. Ebben a példában az átjáró nyilvános IP-40.112.190.5 a virtuális 10.12.255.4 BGP Peering címének használja, és az átjárónak 138.91.156.129 10.12.255.5 fogja használni. Ezt az információt a helyszíni környezetbe VPN eszközén kapcsolódás az aktív-aktív átjáró beállításakor van szükség. Az átjáró a diagram összes címekkel alatt látható:

![aktív – aktív átjáró](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Az átjáró létrehozása után ezt az átjárót aktív-aktív határokon helyszíni vagy VNet-VNet kapcsolatot létesíteni is használhatja. A következőkben lesz lépéseinek elvégzése a gyakorlat elvégzéséhez.


## <a name ="aacrossprem"></a>Része a 2 - határokon helyszíni aktív-aktív kapcsolat létrehozása

Idegen helyszíni kapcsolatot létesíteni a helyi hálózati átjáró a helyszíni VPN eszköz ábrázolására, és az Azure virtuális Magánhálózati átjáró kapcsolatba lépni a helyi hálózati átjáró kapcsolat létrehozásához szükséges. Ebben a példában az Azure virtuális Magánhálózati átjáró aktív-aktív üzemmódban van. Eredményt adja akkor is, ha csak egy helyszíni VPN eszköz (helyi hálózaton átjáró), és egy kapcsolattal az erőforrás van, mindkét Azure virtuális Magánhálózati átjárópéldány hoznak S2S VPN bújtatás a helyszíni eszközzel.

A folytatás előtt ellenőrizze, hogy [részt 1](#aagateway) ebben a gyakorlatban befejezése.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Lépés: 1 – létrehozása és konfigurálása a helyi hálózati átjáró

#### <a name="1-declare-your-variables"></a>1. a változók deklarálása

A gyakorlat továbbra is össze a konfiguráció az ábrán látható. Ügyeljen arra, hogy az értékek lecserélése az Ön konfigurációjának használni kívánt felhasználókat.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Néhány dolog, amit érdemes a helyi hálózaton átjáró paraméterek vonatkozó Megjegyzés:

- A helyi hálózati átjáró a virtuális Magánhálózati átjárót, az ugyanazon vagy másik helyre és erőforráscsoport lehet. Ebben a példában őket a különböző erőforrás csoportokat, de Azure ugyanazon a helyen.

- Ha csak egy helyszíni VPN eszköz ahogy alább látható, az aktív-aktív kapcsolat vagy BGP protokoll nélkül is dolgozhat. Ez a példa BGP a határokon helyszíni kapcsolatot.

- BGP engedélyezve van, az előtag kell a helyi hálózaton átjáró deklarálhatnak esetén a BGP Peer IP-cím, a virtuális Magánhálózati készülékén host címét. Ebben az esetben célszerű-e egy /32 "10.52.255.253/32" előtagot.

- Ne feledje a különböző BGP ASN-között a helyszíni hálózatok és Azure VNet EK kell használnia. Ha ugyanaz, módosíthatja a VNet ASN, ha a helyszíni VPN eszköz már a ASN használja az egyéb BGP szomszédok peer szüksége.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. a helyi hálózati átjáró létrehozása Site5
    
A folytatás előtt ellenőrizze, hogy továbbra is csatlakozik előfizetés 1. Az erőforráscsoport létrehozása, ha nem hozott létre.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Lépés: 2 – az VNet átjáró és a helyi hálózati átjáró csatlakoztatása

#### <a name="1-get-the-two-gateways"></a>1. a két átjáró beszerzése

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. a TestVNet1 Site5 kapcsolat létrehozása

Ebben a lépésben hoz létre a kapcsolatot a TestVNet1 való Site5_1 a "EnableBGP" értékűre $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. a helyszíni VPN eszköz a virtuális Magánhálózati és BGP paraméterei

Az alábbi példában a paraméterek beírja a BGP konfigurációs szakaszba az ebben a gyakorlatban a helyszíni VPN eszközön sorolja fel:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Értesítés prefixumokban: (például) 10.51.0.0/16 és 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: a 40.112.190.5 alagutas az 10.12.255.4
    - Azure VNet BGP IP 2: a 138.91.156.129 alagutas az 10.12.255.5
    - Statikus útvonalak: cél 10.12.255.4/32, nexthop a virtuális Magánhálózati alagutas felület 40.112.190.5 a cél 10.12.255.5/32, a kapcsolat a 138.91.156.129 a virtuális Magánhálózati alagutas nexthop
    - Többszörös ugrási eBGP: eBGP engedélyezett az eszközön, ha szükséges, győződjön meg arról, hogy a "Többszörös ugrási" beállítás

Érdemes lehet kapcsolatot létesíteni néhány perc múlva, és a BGP peering munkamenet elindul, miután létrejött a IPsec kapcsolat. Ebben a példában az eddigi beállította csak egy helyszíni VPN eszközt, így a diagram alább látható módon:

![aktív – aktív-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Lépés a 3 - két helyszíni virtuális Magánhálózati eszközök csatlakoztatása az aktív-aktív VPN átjáró

Két virtuális Magánhálózati eszközök ugyanazon a helyszíni hálózaton, ha a második VPN eszköz az Azure virtuális Magánhálózati átjárót útján kettős redundancia érhet el.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. a második helyi hálózaton átjáró létrehozása Site5

Figyelje meg, hogy a gateway IP-cím, a cím előtag és a második helyi hálózaton átjáró BGP peering címe kell nem áll átfedésben előző helyi hálózaton átjáró a helyszíni hálózaton. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. a VNet átjáró és a helyi hálózaton második átjáró csatlakoztatása

Hozza létre a kapcsolatot TestVNet1 Site5_2 a "EnableBGP" értékűre $True

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. a második a helyszíni VPN eszköz a virtuális Magánhálózati és BGP paraméterei

Hasonlóképpen listák alatti paraméterek meg fog írja be a második VPN eszköz:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Értesítés prefixumokban: (például) 10.51.0.0/16 és 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: a 40.112.190.5 alagutas az 10.12.255.4
    - Azure VNet BGP IP 2: a 138.91.156.129 alagutas az 10.12.255.5
    - Statikus útvonalak: cél 10.12.255.4/32, nexthop a virtuális Magánhálózati alagutas felület 40.112.190.5 a cél 10.12.255.5/32, a kapcsolat a 138.91.156.129 a virtuális Magánhálózati alagutas nexthop
    - Többszörös ugrási eBGP: eBGP engedélyezett az eszközön, ha szükséges, győződjön meg arról, hogy a "Többszörös ugrási" beállítás

Miután a kapcsolat (alagutak) legyenek kialakítva, kettős felesleges virtuális Magánhálózati eszközök és csatlakozás a helyszíni hálózaton és Azure alagutak választania kell:

![kettős-redundancia-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Része a 3 - VNet-VNet aktív-aktív kapcsolat létrehozása

Ez a szakasz aktív-aktív VNet-VNet származó BGP hoz létre. 

Az alábbi utasításokat az előző lépésekben fent felsorolt továbbra is. [Kijelző 1](#aagateway) létrehozása és konfigurálása TestVNet1 és a virtuális Magánhálózati átjáró BGP be kell fejezni. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Lépés: 1 – TestVNet2 és a virtuális Magánhálózati átjáró létrehozása

Fontos, hogy győződjön meg arról, hogy az IP-címterület új virtuális hálózat, TestVNet2, nem áll átfedésben bármelyik VNet tartományt.

Ebben a példában a virtuális hálózatok az azonos előfizetéshez tartozik. Beállíthatja a különböző előfizetések; VNet-VNet kapcsolatának olvassa el a [VNet-VNet kapcsolat konfigurálása](./vpn-gateway-vnet-vnet-rm-ps.md) többet is megtudhat. Ellenőrizze, hogy felvette a "-EnableBgp $True" BGP ahhoz, hogy a kapcsolatok létrehozásakor.

#### <a name="1-declare-your-variables"></a>1. a változók deklarálása

Ügyeljen arra, hogy az értékek lecserélése az Ön konfigurációjának használni kívánt felhasználókat.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Hozzon létre TestVNet2 az új erőforráscsoport

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. a aktív-aktív VPN-átjáró létrehozása TestVNet2

Kérése a két nyilvános IP-címek kiosztandó az átjáró a VNet hoz létre. Az alhálózathoz és IP-konfiguráció szükséges definiálása is fogja. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

A virtuális Magánhálózati átjáró létrehozása a AS számot és a "EnableActiveActiveFeature" jelölőre. Figyelje meg, hogy az Azure virtuális Magánhálózati átjárók kell bírálja felül az alapértelmezett ASN. A ASN-ek a csatlakoztatott VNets az eltérő BGP és a hálózaton átvitt Útválasztás engedélyezése kell lennie.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Lépés: 2 – az TestVNet1 és TestVNet2 átjárók csatlakoztatása

Ebben a példában mindkét átjáró szerepelnek, az azonos előfizetést. Ezt a lépést az azonos PowerShell-munkamenet.

#### <a name="1-get-both-gateways"></a>1. a két átjáró beszerzése

Ellenőrizze, hogy jelentkezzen be, és az előfizetés 1 csatlakozni.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. a két kapcsolatok létrehozása

Ebben a lépésben létrehoz TestVNet2 a TestVNet1 a kapcsolatot, és a kapcsolat a TestVNet2 TestVNet1 szeretne.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Ügyeljen arra, hogy mindkét kapcsolatok BGP engedélyezése.

Peering munkamenet befejezése ezeket a lépéseket a kapcsolat meghatározzák kell néhány percet, és a BGP után lesz felfelé a VNet-VNet kapcsolatot a kettős redundancia befejezése után:

![aktív – aktív-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Rész 4 – az aktív-aktív és az aktív készenléti között létező átjáró frissítése

Az utolsó szakasz bemutatja, hogyan konfigurálható egy meglévő Azure virtuális Magánhálózati átjáró aktív készenléti az aktív-aktív módba, vagy fordítva.

>[AZURE.IMPORTANT] Ne feledje, hogy az aktív-aktív mód csak akkor működik, a HighPerformance Termékváltozat

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Az aktív-aktív átjáró egy aktív készenléti átjáró beállítása

#### <a name="1-gateway-parameters"></a>1. paraméterek átjáró

Az alábbi példa az aktív készenléti átjáró egy aktív-aktív átjáró alakítja át. Hozzon létre egy másik nyilvános IP-címet, majd adja hozzá a második Gateway IP-konfiguráció szükséges. Alább látható használt paraméterek:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. a nyilvános IP-cím létrehozása, majd a második gateway IP-konfigurációja hozzáadása

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. a aktív-aktív mód engedélyezése, és az átjáró frissítése

Meg kell adnia az átjáró objektum PowerShell indíthatja el a tényleges frissítés. Az átjáró objektum SKU is kell változtatni HighPerformance standard korábban létrehozása óta.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

A frissítés 30-45 perc is eltelhet.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Az aktív-aktív átjáró az aktív készenléti átjáró beállítása

#### <a name="1-gateway-parameters"></a>1. paraméterek átjáró

A fenti használni a paramétereket, nevének megállapítása az IP-konfiguráció el szeretné távolítani.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. a gateway IP-konfigurációja eltávolítása és az aktív-aktív üzemmód letiltása

Hasonlóképpen meg kell adnia az átjáró objektum PowerShell indíthatja el a tényleges frissítés.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

A frissítés legfeljebb 30 45 perc is eltarthat.


## <a name="next-steps"></a>Következő lépések

A kapcsolat elkészülte virtuális gépeken futó hozzáadhatja a virtuális hálózatok. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.

