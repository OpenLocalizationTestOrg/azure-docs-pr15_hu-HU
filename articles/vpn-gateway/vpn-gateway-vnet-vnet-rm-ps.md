<properties
   pageTitle="Azure VNets kapcsolatba virtuális Magánhálózati átjáró és PowerShell |} Microsoft Azure"
   description="Ez a cikk végigvezeti virtuális hálózatok összekapcsolása Azure erőforrás-kezelő és a PowerShell használatával."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Az erőforrás-kezelő PowerShell használatával VNet-VNet kapcsolat beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasszikus – klasszikus portál](virtual-networks-configure-vnet-to-vnet-connection.md)

Ez a cikk végigvezeti a virtuális Magánhálózati átjáró segítségével erőforrás-kezelő telepítési modellben VNets közötti kapcsolat létrehozásához. A virtuális hálózatok lehet az ugyanazon vagy másik régiókban és ugyanazon vagy másik előfizetésekről.


![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Telepítési modellek és VNet-VNet kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

A következő táblázat mutatja a jelenleg elérhető telepítési modellek és módszerek VNet-VNet konfigurációk. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>VNet-VNet kapcsolatokról

Virtuális hálózat egy másik virtuális hálózathoz csatlakozik (VNet VNet) hasonlít egy VNet csatlakozik egy helyszíni hely. Mindkét adatkapcsolat-típusokat az Azure virtuális Magánhálózati átjáró segítségével adja meg a biztonságos alagutas szolgáltatással IPsec /. A csatlakozás VNets különböző régiókban lehet. Vagy a különböző előfizetések. Még több webhelyen konfigurációk VNet-VNet kommunikációt is összevonhatja. Hálózati topológiák kombináló határokon helyszíni közötti virtuális a hálózati kapcsolat, kapcsolatot létrehozni, a következő ábrán látható módon lehetővé:


![Kapcsolatok](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Miért kapcsolódás virtuális hálózatok?

Érdemes virtuális hálózatok csatlakoztatása az alábbi okok miatt:

- **Régió geo-redundancia és geo-jelenléti Cross**
    - Beállíthatja a saját geo replikációs vagy a szinkronizálás a biztonságos kapcsolatok túllépte a internetes végpontok nélkül.
    - Azure forgalom felettes és terheléselosztó beállíthatja a redundancia geo a könnyen hozzáférhető terhelést több Azure területek között. Egy fontos példája állíthatja be az SQL mindig a elérhetőség csoportokkal terjesztése több Azure területek között.

- **Területi több szálon futó alkalmazások elkülönítési vagy felügyeleti határérték**
    - Az azonos régión belüli állíthat be több szálon futó alkalmazások több virtuális hálózatokkal összekapcsolása elkülönítési vagy felügyeleti követelmények miatt.


### <a name="vnet-to-vnet-faq"></a>VNet-VNet – gyakori kérdések

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Mely lépést érdemes használnom?

Ez a cikk a két különböző csoportjaihoz lépések láthatók. Egy lépéscsoportra az [, hogy az azonos előfizetés tárolnak VNets](#samesub), a másik pedig a [különböző előfizetések tárolódnak VNets](#difsub). A fő a adathalmazok közötti különbség e létrehozása és konfigurálása az összes virtuális hálózati és az átjáró-erőforrások belül az azonos PowerShell-munkamenetet.

Az ebben a cikkben leírt lépéseket az egyes szakaszok elején deklarált változók használata. Ha már dolgozunk meglévő VNets, módosítsa a a változók megfelelően a beállításokat a saját környezetben. 

![Mindkét kapcsolatok](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Csatlakozás VNets, amelyek az azonos előfizetés

![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Első lépések
    
Mielőtt elkezdené, az Azure erőforrás-kezelő PowerShell-parancsmagok telepítéséhez szükséges. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.

### <a name="Step1"></a>Lépés: 1 – az IP-címtartományok megtervezése


Az alábbi lépéseket a megfelelő átjáró alhálózat és konfigurációk két virtuális hálózatok hozzunk létre. Hozzunk létre a virtuális Magánhálózati kapcsolat között a két VNets majd. Fontos, hogy az IP-címtartományok esetében a hálózati konfigurálásról megtervezése. Ne feledje, hogy meg kell győződnie arról, hogy a VNet tartományok vagy a helyi hálózati tartományok ne fedjék egymást semmilyen módon.

Az alábbi példákban használjuk az alábbi értékeket:

**TestVNet1 értékeit:**

- VNet neve: TestVNet1
- Erőforráscsoport: TestRG1
- Hely: Kelet-Amerikai Egyesült Államok
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Kódmentes: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS-kiszolgáló: 8.8.8.8
- Átjárónév: VNet1GW
- Nyilvános IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**TestVNet4 értékeit:**

- VNet neve: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Kódmentes: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Erőforráscsoport: TestRG4
- Hely: Nyugati Amerikai Egyesült Államok
- DNS-kiszolgáló: 8.8.8.8
- Átjárónév: VNet4GW
- Nyilvános IP: VNet4GWIP
- VPNType: RouteBased
- Kapcsolat: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Lépés: 2 - létrehozása és konfigurálása TestVNet1

1. A változók deklarálása

    Első lépésként változók deklarálása. Ebben a példában a változók használatával értékeket ez gyakorlásához deklarálása. A legtöbb esetben meg kell cserélni az értékeket a saját. Azonban ezek változót használhat végig, és segítségével megismerkedhet az ilyen típusú konfiguráció használata esetén. A változók, ha szükséges, módosítsa, majd másolja és illessze be a PowerShell konzolt.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Az előfizetés csatlakoztatása

    Az erőforrás-kezelő parancsmagok használatához a PowerShell módba vált. Nyissa meg a PowerShell konzolt, és csatlakozni a fiókjához. Használja az alábbi példa csatlakozni:

        Login-AzureRmAccount

    Jelölje be az előfizetések a fiókhoz.

        Get-AzureRmSubscription 

    Adja meg az előfizetést, szeretné használni.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Hozzon létre egy új erőforráscsoport

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Az alhálózathoz konfigurációk TestVNet1 létrehozása

    Ez a példa egy virtuális hálózati TestVNet1 és három alhálózathoz, egy úgynevezett GatewaySubnet, egy úgynevezett FrontEnd és egy úgynevezett Kódmentes nevű hoz létre. Amikor értékek helyettesítése, fontos mindig alhálózathoz átjáró neve kifejezetten GatewaySubnet. Nevezze el mást, ha az átjáró létrehozása sikertelen lesz. 

    Az alábbi példában a korábban beállított változók. Ebben a példában az átjáró alhálózat egy /27 használ. Lehet létrehozni egy átjáró alhálózat minél kisebb méretű /29 lép, azt javasoljuk, hogy legalább /28 vagy /27 bejelölésével több címet tartalmazó nagyobb alhálózat hozza létre. Ez az esetleges további beállításokat, hogy szükség lehet a jövőben igazodik elég címek lehetővé teszi. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. TestVNet1 létrehozása

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Egy nyilvános IP-cím kérése

    Egy nyilvános IP-címet az átjáró hoz létre a VNet kiosztandó kérhet. Figyelje meg, hogy a AllocationMethod dinamikus. Az IP-címet, amely a használni kívánt nem adhat meg. Dinamikusan oszlik a átjáróhoz. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Az átjáró konfiguráció létrehozása

    Az átjáró konfiguráció határozza meg, az alhálózathoz és a nyilvános IP-címet használja. A példában az átjáró konfigurációs létrehozásához használhatja. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Az átjáró létrehozása a TestVNet1

    Ebben a lépésben a TestVNet1 hozza létre a virtuális hálózati átjáró. VNet-VNet konfigurációk egy RouteBased VpnType szükség. Az átjárók létrehozása igénybe vesz (45 perc vagy több befejezéséhez).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>3 – lépés létrehozása és konfigurálása TestVNet4

Miután beállította az TestVNet1, hozzon létre TestVNet4. Kövesse az alábbi értékek cseréje a saját szükség esetén. Ezt a lépést elvégezhető belül az azonos PowerShell-munkamenetet, mert az azonos előfizetés.

1. A változók deklarálása

    Ügyeljen arra, hogy az értékek lecserélése az Ön konfigurációjának használni kívánt felhasználókat.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    A folytatás előtt feltétlenül előfizetés 1 továbbra is csatlakozik.

2. Hozzon létre egy új erőforráscsoport

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Az alhálózathoz konfigurációk TestVNet4 létrehozása

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. TestVNet4 létrehozása

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Egy nyilvános IP-cím kérése

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Az átjáró konfiguráció létrehozása

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. A TestVNet4 átjáró létrehozása

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Lépés: 4 – az átjárók csatlakoztatása

1. Mindkét virtuális hálózati átjárók beszerzése

    Ebben a példában mindkét átjáró ugyanabban az előfizetésben, mert ez a lépés hajtható az azonos PowerShell-munkamenet.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. A TestVNet1 TestVNet4 kapcsolat létrehozása

    Ebben a lépésben készíthet a kapcsolat TestVNet1 TestVNet4 szeretne. Megjelenik egy megosztott kulcsot, az alábbi példákban hivatkozik. Saját értékek használhatja a megosztott billentyűt is. Figyeljen arra, hogy a megosztott kulcs egyeznie kell a két kapcsolatok. Kapcsolat létrehozása készíthet egy rövid ideig befejezéséhez.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. A TestVNet4 TestVNet1 kapcsolat létrehozása

    Ez a lépés nem hasonló a fent, azzal a különbséggel hoz létre a kapcsolatot a TestVNet4 TestVNet1 szeretne. Ellenőrizze, hogy a megosztott billentyűk megfelelően.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    A kapcsolat néhány perc múlva.

4. Ellenőrizze a kapcsolat. A [ellenőrzése a kapcsolat](#verify)szakaszban olvashat.


## <a name="difsub"></a>Hogyan lehet, hogy a különböző előfizetések VNets csatlakoztatni


![v2v diagram](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

Ebben az esetben azt csatlakozás TestVNet1 és TestVNet5. TestVNet1 és TestVNet5 lennie kell egy másik előfizetést. Ez a beállítás lépései további VNet-VNet kapcsolat hozzáadása, hogy csatlakoztatni TestVNet1 TestVNet5. 

Az alábbi különbség, hogy néhány konfigurálási lépéseket kell elvégezni, külön PowerShell-munkamenetet a második előfizetés keretében. Különösen ha a két előfizetéseket tartozik más szervezetek. 

Az előző lépéseket, a fent felsorolt folytatni a képernyőn megjelenő utasításokat. [Lépés 1](#Step1) és a [lépés 2](#Step2) létrehozása és konfigurálása TestVNet1, és a virtuális Magánhálózati átjáró TestVNet1 végre kell hajtania. Lépés 1 és 2 lépés elvégzése után továbbra is lépés 5 TestVNet5 létrehozásához.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Lépés az 5 - ellenőrizze a további IP-címtartományok

Fontos, hogy győződjön meg arról, hogy az IP-címterület új virtuális hálózat, TestVNet5, nem áll átfedésben a VNet tartományok vagy a helyi hálózati átjáró tartományok bármelyikével. 

Ebben a példában a virtuális hálózatok más szervezetek tartozhat. Ebben a gyakorlatban a TestVNet5 használhatja az alábbi értékeket:

**TestVNet5 értékeit:**

- VNet neve: TestVNet5
- Erőforráscsoport: TestRG5
- Hely: Japán keleti
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Kódmentes: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS-kiszolgáló: 8.8.8.8
- Átjárónév: VNet5GW
- Nyilvános IP: VNet5GWIP
- VPNType: RouteBased
- Kapcsolat: VNet5toVNet1
- ConnectionType: VNet2VNet

**További TestVNet1 értékeit:**

- Kapcsolat: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Lépés a 6 - létrehozása és konfigurálása TestVNet5

Ezt a lépést kell elvégezni az új előfizetés keretében. Ebben a részben egy másik szervezet, amely az előfizetés tulajdonosa a rendszergazda által is elvégezhető.

1. A változók deklarálása

    Ügyeljen arra, hogy az értékek lecserélése az Ön konfigurációjának használni kívánt felhasználókat.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Előfizetés 5 csatlakoztatása

    Nyissa meg a PowerShell konzolt, és csatlakozni a fiókjához. Használja az alábbi példa csatlakozni:

        Login-AzureRmAccount

    Jelölje be az előfizetések a fiókhoz.

        Get-AzureRmSubscription 

    Adja meg az előfizetést, szeretné használni.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Hozzon létre egy új erőforráscsoport

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Az alhálózathoz konfigurációk TestVNet4 létrehozása
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. TestVNet5 létrehozása

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Egy nyilvános IP-cím kérése

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Az átjáró konfiguráció létrehozása

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. A TestVNet5 átjáró létrehozása

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>7 – az átjárók csatlakozás lépés

Ebben a példában az átjárók a különböző előfizetések, mert azt már osztani ezt a lépést két PowerShell-munkamenetek megjelölt elemekre, [előfizetés 1] és [előfizetés 5].

1. **[1 előfizetés]** A virtuális hálózati átjáró beszerzése előfizetés 1

    Ellenőrizze, hogy jelentkezzen be, és az előfizetés 1 csatlakozni.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Másolja az alábbi elemekre közül a kimenet, és küldje el e-mailhez vagy más módon az előfizetés 5 a rendszergazdák ezeket.

        $vnet1gw.Name
        $vnet1gw.Id

    A két elem értékeket a következő példa eredménye hasonló lesz:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Előfizetés 5]** A virtuális hálózati átjáró beszerzése előfizetés 5

    Ellenőrizze, hogy jelentkezzen be, és az előfizetés 5 csatlakozni.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Másolja az alábbi elemekre közül a kimenet, és küldje el ezeket a rendszergazdák az előfizetés 1 e-mailhez vagy más módon.

        $vnet5gw.Name
        $vnet5gw.Id

    A két elem értékeket a következő példa eredménye hasonló lesz:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[1 előfizetés]** A TestVNet1 TestVNet5 kapcsolat létrehozása

    Ebben a lépésben készíthet a kapcsolat TestVNet1 TestVNet5 szeretne. Az alábbi különbség, hogy a $vnet5gw nem lehet közvetlenül megszerezni, mert a másik előfizetést. Meg kell hozzon létre egy új PowerShell-objektumot értékekkel, közölt előfizetés 1-től a fenti lépéseket. Használja az alábbi példában. Cserélje ki a nevét, az azonosító és a megosztott kulcsot a kívánt értékét. Figyeljen arra, hogy a megosztott kulcs egyeznie kell a két kapcsolatok. Kapcsolat létrehozása készíthet egy rövid ideig befejezéséhez.

    Ellenőrizze, hogy az előfizetés 1 csatlakoztatja. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Előfizetés 5]** A TestVNet5 TestVNet1 kapcsolat létrehozása

    Ez a lépés nem hasonló a fent, azzal a különbséggel hoz létre a kapcsolatot a TestVNet5 TestVNet1 szeretne. Ugyanezt az eljárást, az előfizetés 1-től kapott értékek alapján egy PowerShell-objektum létrehozása érvényes itt is látható. Ebben a lépésben ügyeljen arra, hogy egyezik-e a megosztott billentyűket.

    Ellenőrizze, hogy az előfizetés 5 csatlakoztatja.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Kapcsolat ellenőrzése


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Következő lépések

- A kapcsolat elkészülte virtuális gépeken futó hozzáadhatja a virtuális hálózatok. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.
- BGP tudni lásd: a [BGP – áttekintés](vpn-gateway-bgp-overview.md) és [konfigurálásáról BGP](vpn-gateway-bgp-resource-manager-ps.md). 

