<properties
   pageTitle="Azure virtuális Magánhálózati átjárók Azure erőforrás-kezelő és a PowerShell használatával BGP beállítása |} Microsoft Azure"
   description="Ez a cikk végigvezeti BGP konfigurálása az Azure virtuális Magánhálózati átjárók Azure erőforrás-kezelő és a PowerShell használatával."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Hogyan kell beállítania BGP Azure virtuális Magánhálózati átjárók Azure erőforrás-kezelő és a PowerShell használatával

Ez a cikk végigvezeti a BGP ahhoz, hogy a határokon helyszíni webhely (S2S) virtuális Magánhálózati kapcsolat és az erőforrás-kezelő telepítési modell és a PowerShell használatával VNet-VNet kapcsolatot.


**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Tudnivalók a BGP

BGP a szabványos útválasztási protokoll gyakran használt az interneten az exchange Útválasztás és elérhetőség információk két vagy több hálózatok között. BGP lehetővé teszi, hogy az Azure virtuális Magánhálózati átjárók és a helyszíni virtuális Magánhálózati eszközök néven BGP partnerek vagy szomszédok, az exchange "irányítja", amely tájékoztatja mindkét átjárók elérhetősége és elérhetőség az adott prefixumokban az átjárók és az útválasztó érintett folyamatát. BGP is engedélyezheti a hálózaton átvitt útválasztás több hálózatok között egy BGP peer az összes többi BGP partnerek számára folyamatosan tanul BGP az átjárók útvonalak propagálása szerint.

Tanulmányozza [BGP áttekintése az Azure virtuális Magánhálózati átjárók](./vpn-gateway-bgp-overview.md) előnyökkel jár a több hozzászólás BGP, és megtudhatja, hogy a műszaki követelmények és BGP használatával kapcsolatos megfontolások.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>A Azure virtuális Magánhálózati átjárók BGP használatába

Ez a cikk azt ismerteti, hogy a lépéseivel hajtsa végre az alábbi műveleteket:

- [Kijelző 1 – az Azure virtuális Magánhálózati átjárón BGP engedélyezése](#enablebgp)

- [Része a 2 - BGP határokon helyszíni kapcsolatot létesíteni](#crossprembgp)

- [Része a 3 - BGP VNet-VNet kapcsolatot létesíteni](#v2vbgp)

Minden részét a képernyőn megjelenő utasításokat a alapvető építőeleme BGP engedélyezése a hálózati kapcsolat az űrlapok. Ha befejezte az összes három részből, gyűjt a topológia, a következő ábrán látható módon:

![BGP topológiája](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Ezek a közös Szerkesztés meg az igényeinek megfelelő összetettebb, több Reménység, a hálózaton átvitt hálózaton is összevonhatja.

## <a name ="enablebgp"></a>Rész 1 – az Azure virtuális Magánhálózati átjáró BGP konfigurálása

Az alábbi konfigurálási lépéseket fog paramétereket állíthat be a BGP az Azure virtuális Magánhálózati átjáró a következő ábrán látható módon:

![BGP átjáró](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Első lépések

- Ellenőrizze, hogy rendelkezik-e egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési be egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
    
- Telepítse az Azure erőforrás-kezelő PowerShell-parancsmagok kell. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.

### <a name="step-1---create-and-configure-vnet1"></a>Lépés: 1 – létrehozása és konfigurálása VNet1 

#### <a name="1-declare-your-variables"></a>1. a változók deklarálása

Ebben a gyakorlatban lássuk először, a változók deklarálása szerint. Az alábbi példában a változók használatával értékeket ez gyakorlásához deklarálása. Ügyeljen arra, hogy az értékek helyettesítése a saját gyártási konfigurálásakor. Ezek a változók is használhatja, ha futtatja a segítségével megismerkedhet az ilyen típusú konfigurációt lépéseit. Módosítsa a változók, majd másolja és illessze be a PowerShell konzol.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Lépés: 2 - TestVNet1 BGP paraméterekkel a virtuális Magánhálózati átjáró létrehozása

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. a IP- és alhálózat konfigurációk létrehozása

Egy nyilvános IP-címet az átjáró hoz létre a VNet kiosztandó kérhet. Az alhálózathoz és IP-konfiguráció szükséges definiálása is fogja. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. a virtuális Magánhálózati átjáró létrehozása és a AS szám

A virtuális hálózati átjáró TestVNet1 hozhat létre. Figyelje meg, hogy BGP és elő kell készítenie egy útvonal-alapú virtuális Magánhálózati átjárót, is a összeadás paraméter - Asn, a ASN (mint száma) beállítása TestVNet1. Az átjárók létrehozása igénybe vesz (30 percen vagy több befejezéséhez).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. a Azure BGP Peer IP-cím beszerzése

Amikor az átjáró létrejött, szüksége lesz beszerzése Azure virtuális Magánhálózati átjáró BGP Peer IP-címét. A cím konfigurálja az Azure virtuális Magánhálózati átjáró a helyszíni virtuális Magánhálózati eszközök egy BGP Peer van szükség.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

A legutóbbi parancs jelennek meg a megfelelő BGP konfigurációk Azure virtuális Magánhálózati átjáró; Példa:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Az átjáró létrehozása után ezt az átjárót határokon helyszíni vagy a BGP VNet-VNet kapcsolatot létesíteni is használhatja. A következőkben lesz lépéseinek elvégzése a gyakorlat elvégzéséhez.

## <a name ="crossprembbgp"></a>Része a 2 - BGP határokon helyszíni kapcsolatot létesíteni

Idegen helyszíni kapcsolatot létesíteni a helyi hálózati átjáró a helyszíni VPN eszköz ábrázolására, és az Azure virtuális Magánhálózati átjáró kapcsolatba lépni a helyi hálózati átjáró kapcsolat létrehozásához szükséges. Ez a cikk utasításait közötti különbség a paramétert a BGP konfiguráció szükséges további tulajdonságokat.

![Az idegen helyszíni BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

A folytatás előtt ellenőrizze, hogy [részt 1](#enablebgp) ebben a gyakorlatban befejezése.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Lépés: 1 – létrehozása és konfigurálása a helyi hálózati átjáró

#### <a name="1-declare-your-variables"></a>1. a változók deklarálása

A gyakorlat továbbra is össze a konfiguráció az ábrán látható. Ügyeljen arra, hogy az értékek lecserélése az Ön konfigurációjának használni kívánt felhasználókat.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Néhány dolog, amit érdemes a helyi hálózaton átjáró paraméterek vonatkozó Megjegyzés:

- A helyi hálózati átjáró a virtuális Magánhálózati átjárót, az ugyanazon vagy másik helyre és erőforráscsoport lehet. Ez a példa jeleníti meg őket más más erőforrás-csoportokat.

- A minimális előtagot kell a helyi hálózaton átjáró deklarálása a BGP Peer IP-cím, a virtuális Magánhálózati készülékén az állomás címe. Ebben az esetben célszerű-e egy /32 "10.52.255.254/32" előtagot.

- Ne feledje a különböző BGP ASN-között a helyszíni hálózatok és Azure VNet EK kell használnia. Ha ugyanaz, módosíthatja a VNet ASN, ha a helyszíni VPN eszköz már a ASN más BGP szomszédoknak a peer szüksége.
    
A folytatás előtt ellenőrizze, hogy továbbra is csatlakozik előfizetés 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. a helyi hálózati átjáró létrehozása Site5

Ügyeljen arra, hogy az erőforrás csoport létrehozása: Ha nem létrehozása, a helyi hálózati átjáró létrehozása előtt. Figyelje meg a helyi hálózaton átjáró két további paramétereket: Asn és BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Lépés: 2 – az VNet átjáró és a helyi hálózati átjáró csatlakoztatása

#### <a name="1-get-the-two-gateways"></a>1. a két átjáró beszerzése

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. a TestVNet1 Site5 kapcsolat létrehozása

Ebben a lépésben hoz létre a kapcsolatot a TestVNet1 Site5 szeretne. Meg kell adnia "-EnableBGP $True" BGP ahhoz, hogy a kapcsolathoz. Korábbiakban tárgyalt, akkor lehet, hogy az azonos Azure virtuális Magánhálózati átjáró BGP és a nem BGP kapcsolatok. BGP a kapcsolat tulajdonság engedélyezve van, kivéve Azure nem lehetővé teszi BGP a kapcsolat annak ellenére, hogy BGP paraméterek van már be van állítva a mindkét átjáró.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


Az alábbi példában a paraméterek beírja a BGP konfigurációs szakaszba helyszíni VPN eszközén e feladatra sorolja fel:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Értesítés prefixumokban: (például) 10.51.0.0/16 és 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP: 10.12.255.30
    - Statikus útvonal: az éppen a virtuális Magánhálózati alagutas felület az eszközön nexthop 10.12.255.30/32, az út hozzáadása
    - Többszörös ugrási eBGP: eBGP engedélyezett az eszközön, ha szükséges, győződjön meg arról, hogy a "Többszörös ugrási" beállítás

Érdemes lehet kapcsolatot létesíteni néhány perc múlva, és a BGP peering munkamenet elindul, miután létrejött a IPsec kapcsolat.
 
## <a name ="v2vbgp"></a>Része a 3 - BGP VNet-VNet kapcsolatot létesíteni

Ez a szakasz hozzáadása a VNet-VNet kapcsolatot a BGP, az alábbi ábrán látható módon. 

![A VNet-VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Az alábbi utasításokat az előző lépésekben fent felsorolt továbbra is. Meg kell adnia a [i.](#enablebgp) létrehozása és konfigurálása TestVNet1 és a virtuális Magánhálózati átjáró BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Lépés: 1 – TestVNet2 és a virtuális Magánhálózati átjáró létrehozása

Fontos, hogy győződjön meg arról, hogy az IP-címterület új virtuális hálózat, TestVNet2, nem áll átfedésben bármelyik VNet tartományt.

Ebben a példában a virtuális hálózatok az azonos előfizetéshez tartozik. Beállíthatja, hogy VNet-VNet kapcsolatok között a különböző előfizetések; olvassa el a [VNet-VNet kapcsolat konfigurálása](./vpn-gateway-vnet-vnet-rm-ps.md) többet is megtudhat. Ellenőrizze, hogy felvette a "-EnableBgp $True" BGP ahhoz, hogy a kapcsolatok létrehozásakor.

#### <a name="1-declare-your-variables"></a>1. a változók deklarálása

Ügyeljen arra, hogy az értékek lecserélése az Ön konfigurációjának használni kívánt felhasználókat.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Hozzon létre TestVNet2 az új erőforráscsoport

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. a virtuális Magánhálózati átjáró létrehozása az TestVNet2 BGP paraméterrel

Egy nyilvános IP-címet az átjáró hoz létre a VNet kiosztandó kérhet. Az alhálózathoz és IP-konfiguráció szükséges definiálása is fogja. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

A virtuális Magánhálózati átjáró létrehozása a AS számot. Figyelje meg, hogy az Azure virtuális Magánhálózati átjárók kell bírálja felül az alapértelmezett ASN. A ASN-ek a csatlakoztatott VNets az eltérő BGP és a hálózaton átvitt Útválasztás engedélyezése kell lennie.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Lépés: 2 – az TestVNet1 és TestVNet2 átjárók csatlakoztatása

Ebben a példában mindkét átjáró szerepelnek, az azonos előfizetést. Ezt a lépést az azonos PowerShell-munkamenet.

#### <a name="1-get-both-gateways"></a>1. a két átjáró beolvasása

Győződjön meg arról, bejelentkezés, és csatlakozás előfizetés 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. a két kapcsolatok létrehozása

Ebben a lépésben létrehoz TestVNet2 a TestVNet1 a kapcsolatot, és a kapcsolat a TestVNet2 TestVNet1 szeretne.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Ügyeljen arra, hogy mindkét kapcsolatok BGP engedélyezése.

Az alábbi lépések elvégzése a kapcsolat kell meghatározzák néhány percet, és a BGP peering munkamenet egyszer be lesz a VNet-VNet kapcsolat befejeződik.

Ha befejezte a gyakorlat három része, alább látható módon a hálózati topológiája fog hozott létre:

![A VNet-VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Következő lépések

A kapcsolat elkészülte virtuális gépeken futó hozzáadhatja a virtuális hálózatok. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.

