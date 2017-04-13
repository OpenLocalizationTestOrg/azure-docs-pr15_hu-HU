<properties 
   pageTitle="Erőforrás-kezelő virtuális hálózatok PowerShell használatával klasszikus virtuális hálózatok keresztüli |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre a virtuális Magánhálózati kapcsolat közötti klasszikus VNets és erőforrás-kezelő VNets virtuális Magánhálózati átjáró és a PowerShell használatával"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Kapcsolódás virtuális hálózatok különböző telepítési modellekből PowerShell használatával

> [AZURE.SELECTOR]
- [Portál](vpn-gateway-connect-different-deployment-models-portal.md)
- [A PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure által jelzett két management modellek: klasszikus és erőforrás Manager (erőforrás-kezelő). Ha Ön használták Azure egy kis időt, valószínűleg van Azure VMs és a klasszikus VNet futó példányát szerepkörök. A újabb VMs és szerepkör-példányok egy erőforrás-kezelő létrehozott VNet kell futnia. Ez a cikk végigvezeti klasszikus VNets csatlakoztatása az erőforrás-kezelő VNets engedélyezése a külön telepítési modellekben kapcsolatba lépni a többi átjáró kapcsolaton keresztül erőforrásait.

Létrehozhat, amelyek a különböző előfizetések és különböző régiókban VNets közötti kapcsolat. Már a kapcsolatokat a helyszíni hálózatokon VNets mindaddig, amíg az átjáró a konfigurált dinamikus vagy útvonal is csatlakozhat. További információt a VNet-VNet kapcsolatok olvassa el az [VNet-VNet – gyakori kérdések](#faq) a Ez a cikk végén.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Telepítési modellek és csatlakozás VNets különböző telepítési modellekben módszerei

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Az alábbi táblázat frissítjük új cikkekben, és további eszközök lesznek elérhetők, ebben a konfigurációban. Egy cikk érhető el, ha azt csatolása közvetlenül az értékeket.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Mielőtt elkezdené

Az alábbi lépésekkel ismerteti, hogy az egyes VNet dinamikus vagy útvonal átjáró beállítása, és az átjárók közötti virtuális Magánhálózati kapcsolat létrehozásához szükséges beállításokat. Ez a beállítás nem támogatja a statikus vagy csoportházirend-alapú átjárókat.

### <a name="prerequisites"></a>Előfeltételek

 - Mindkét VNets már elkészült.
 - A cím tartományokat a VNets nem az egymást átfedő, vagy az egyéb kapcsolatot, amely az átjárók is kapcsolódik a tartományok bármelyikének diagramterület fölé.
 - Telepítve van a legújabb PowerShell-parancsmagok (1.0.2-es verziójú vagy újabb). Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) további információt. Győződjön meg arról, hogy a szolgáltatás felügyeleti (KOVÁ) és a az erőforrás Manager (erőforrás-kezelő) parancsmag telepítése. 

### <a name="exampleref"></a>Példa beállításai

A példa beállítások hivatkozásként is használhatja.

**Klasszikus VNet beállításai**

VNet neve = ClassicVNet <br>
Hely = US nyugati <br>
Virtuális hálózati cím szóközöket = 10.0.0.0/24 <br>
Alhálózat-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Helyi Network Name = RMVNetLocal <br>
GatewayType = DynamicRouting

**Erőforrás-kezelő VNet beállításai**

VNet neve = RMVNet <br>
Erőforráscsoport = RG1 <br>
Virtuális hálózati IP-cím szóközöket = 192.168.0.0/16 <br>
Alhálózat-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Hely = kelet-Amerikai Egyesült Államok <br>
Átjáró nyilvános IP-neve = gwpip <br>
Helyi hálózaton átjáró = ClassicVNetLocal <br>
Virtuális hálózati átjáró neve = RMGateway <br>
Gateway IP-címzésének konfiguráció = gwipconfig


## <a name="createsmgw"></a>Szakasz 1 – a klasszikus VNet konfigurálása

### <a name="part-1---download-your-network-configuration-file"></a>Kijelző 1 – a hálózat konfigurációs fájl letöltése

1. Jelentkezzen be a PowerShell konzolban magasabb szintű jogosultságokkal rendelkező Azure fiókját. A következő parancsmagot, a bejelentkezési hitelesítő adatokat kér az Azure-fiók. Naplózás után letöltés a Fiókbeállítások úgy, hogy Azure PowerShell elérhetők legyenek. Fogja használni, akkor a KOVÁ PowerShell-parancsmagok ezen részének a konfiguráció befejezéséhez.

        Add-AzureAccount

2. A hálózati Azure konfigurációs fájl exportálása a következő parancs futtatásával. Módosíthatja a egy másik helyre, szükség esetén az exportálandó fájl helyét. Szerkessze a fájlt, és importálja Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Nyissa meg szerkesztésre a dokumentumtárból letöltött .xml fájlt. Egy példa a hálózati konfigurációs fájl című témakörben a [Hálózati konfigurációja séma](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Kijelző 2 – Ellenőrizze az átjáró alhálózat

**VirtualNetworkSites** elem hozzáadása egy átjáró alhálózat a VNet, ha egy nem már lett létrehozva. Ha a hálózati konfigurációs fájl dolgozik, az átjáró alhálózat kell neve "GatewaySubnet" vagy Azure nem ismeri fel, és az átjáró alhálózat legyen.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Példa:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Rész 3 – a helyi hálózati hely hozzáadása

A helyi hálózati hely felvétele felel meg az erőforrás-kezelő VNet, amelyhez szeretne csatlakozni. Előfordulhat, hogy egy **LocalNetworkSites** elemet a fájlt, ha még nem létezik hozzáadni. Ezen a ponton a konfigurációban a VPNGatewayAddress lehet, bármely érvényes nyilvános IP-cím mert azt még nem hozott létre az átjáró-az erőforrás-kezelő VNet. Az átjáró hozzunk létre, miután azt cserélje le a helyőrző IP-cím a megfelelő nyilvános IP-címet, az erőforrás-kezelő átjáró rendelt.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Rész 4 – a VNet társítani a helyi hálózati hely

Ebben a részben azt adja meg a helyi hálózati helyet a VNet szeretne csatlakozni szeretne. Ebben az esetben célszerű az erőforrás-kezelő VNet, a hivatkozott korábbi verziójában. Győződjön meg arról, hogy a megfelelő neveket. Ez a lépés nem hoz létre egy átjárót. Adja meg a helyi hálózaton, hogy az átjáró fog csatlakozni.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>5 rész - mentse a fájlt, és töltse fel

Mentse a fájlt, majd importálja a következő parancs futtatásával Azure. Győződjön meg arról, hogy a fájl elérési útja környezetben szükség szerint módosíthatja.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Meg kell jelennie hasonló látható, hogy az importálás sikeres ezt az eredményt.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Része a 6 - az átjáró létrehozása 

Az VNet átjáró hozhat létre, vagy a klasszikus portál használatával, vagy PowerShell használatával.

Az alábbi példa segítségével dinamikus útválasztási átjáró létrehozása:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Az átjáró állapotát a használatával ellenőrizheti a `Get-AzureVNetGateway` parancsmag.


## <a name="creatermgw"></a>Szakasz 2: Az erőforrás-kezelő VNet átjáró beállítása

Az erőforrás-kezelő VNet VPN átjáró létrehozásához kövesse az alábbi utasításokat. Ne kezdje a addig, amíg a lépések a nyilvános IP-címe a klasszikus VNet átjáró beolvasása után. 

1. **Jelentkezzen be az Azure-fiókjába** a PowerShell konzolban. A következő parancsmagot, a bejelentkezési hitelesítő adatokat kér az Azure-fiók. Naplózás után a Fiókbeállítások, hogy elérhetők Azure PowerShell töltődnek le.

        Login-AzureRmAccount 

    Ismerkedés az Azure-előfizetések listáját, egynél több szóló előfizetés.

        Get-AzureRmSubscription

    Adja meg az előfizetést, szeretné használni. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **A helyi hálózati átjáró létrehozása**. Egy virtuális hálózaton a helyi hálózati átjáró általában utal, amelyet a helyszíni helyre. A helyi hálózati átjáró ebben az esetben a klasszikus VNet hivatkozik. Adja meg a nevet, amellyel Azure adhat a hivatkozott és a cím helyet előtagot is megadhatja. Azure használja a IP-cím azt megadhatja, hogy melyik forgalmat a helyszíni helyre küldése azonosítása előtagját. Ha módosítani szeretné az adatok itt később, módosítsa az átjáró létrehozása előtt módosítsa az értékeket, és futtassa újra a minta gombra.<br><br>
    - `-Name`az a név hozzárendelése a helyi hálózati átjáró hivatkozni szeretne.<br>
    - `-AddressPrefix`a klasszikus VNet a címterületet nem.<br>
    - `-GatewayIpAddress`a klasszikus VNet átjáró nyilvános IP-címe van. Ne felejtse el az alábbi példa a helyes IP-cím megfelelően módosíthatja.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Egy nyilvános IP-cím kérése** a virtuális hálózati átjáró az erőforrás-kezelő VNet kiosztandó. Az IP-címet, amely a használni kívánt nem adhat meg. A virtuális hálózati átjáró dinamikusan rendelt IP-címét. Ez azonban nem jelenti azt az IP-cím változik. A csak a virtuális hálózati gateway IP-cím változását, amikor az átjáró törlődik, és az ismételt megadásukra. Azt átméretezése alaphelyzetbe állítása és más belső karbantartási/frissítések az átjáró át nem változik.<br>Ebben a lépésben azt is beállíthatja a egy változó, amely a leendő szolgál.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Annak ellenőrzése, hogy a virtuális hálózat van-e az átjáró alhálózat**. Ha nincs átjáró alhálózat létezik, adja hozzá. Győződjön meg arról, hogy az átjáró alhálózat *GatewaySubnet*neve.

5. **Az alhálózathoz beolvasni** a következő parancs futtatásával az átjáró használt. Ebben a lépésben azt is beállíthatja a változó használható a következő lépésben.
    - `-Name`pedig az erőforrás-kezelő VNet neve.
    - `-ResourceGroupName`az erőforráscsoport, amely a VNet van társítva van. Az átjáró alhálózat már léteznie kell-e VNet és *GatewaySubnet* működéséhez nevű kell.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Az átjáró IP-címzésének konfiguráció létrehozása**. Az átjáró konfiguráció határozza meg, az alhálózathoz és a nyilvános IP-címet használja. A következő példa használatával hozhat létre az átjáró konfigurációt.<br>Ebben a lépésben a `-SubnetId` és `-PublicIpAddressId` paramétereket kell átadni az id tulajdonság alhálózat és IP-cím objektumok, illetve. Egy egyszerű karakterlánc nem használható. Ezek a változók kérni egy nyilvános IP és a lépést az alhálózathoz beolvasni a fenti lépésben vannak beállítva.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Az erőforrás-kezelő virtuális hálózati átjáró létrehozása** a következő parancs futtatásával. A `-VpnType` *RouteBased*kell lennie. 45 perc, vagy több igénybe vehet.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **A nyilvános IP-cím másolása** egyszer a virtuális Magánhálózati átjáró létrehozva. Használhatja, ha a helyi hálózat beállításainak megadása a klasszikus VNet. A következő parancsmagot beolvasása nyilvános IP-címét is használhatja. A nyilvános IP-címet a return *IP-cím*néven jelenik meg.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Szakasz 3: A klasszikus VNet módosíthatja a helyi hálózaton

Ezt a lépést, vagy pedig a hálózati konfigurációs fájl exportálása, szerkesztését, és kattintson a fájl importálása az Azure biztonsági műveleteket hajthat végre. Vagy módosíthatja a klasszikus portál ezt a beállítást. 

### <a name="to-modify-in-the-portal"></a>A portál módosítása

1. Nyissa meg a [Klasszikus portálon](https://manage.windowsazure.com).

2. A klasszikus portálon görgessen lefelé, a bal oldalon, és kattintson a **hálózatok**. A **hálózatok** lapon kattintson a **Helyi hálózat** elemre a lap tetején. 

3. A **Helyi hálózat** lapon kattintással jelölje ki a helyi hálózaton meg, hogy konfigurálva a kijelző 1, majd nyissa meg az oldal aljára, és kattintson a **Szerkesztés**gombra.

4. **A helyi hálózaton részletek megadása** lapon a nyilvános IP-címet az erőforrás-kezelő átjáró az előző részben létrehozott cserélje le a helyőrző IP-címét. Kattintson a nyílra kattintva helyezze át a következő szakaszban. Ellenőrizze, hogy helyes-e a **Címterület** , és válassza a be van jelölve a módosítások elfogadásához.

### <a name="to-modify-using-the-network-configuration-file"></a>A hálózati konfigurációs fájl használatával módosítása

1. A hálózati konfigurációs fájl exportálása
2. Egyszerű szövegszerkesztőben módosítsa a virtuális Magánhálózati gateway IP-címet.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. A módosítások mentéséhez és a szerkesztett-fájl importálása vissza az Azure.


## <a name="connect"></a>Szakasz 4: Az átjárók közötti kapcsolat létrehozása

Az átjárók közötti kapcsolat létrehozásához PowerShell. Szükség lehet a klasszikus PowerShell-parancsmagok használata az Azure-fiók felvétele. Ha igen, használhatja a következő parancsmagot: 
        
    Add-AzureAccount

1. **A megosztott kulcs beállítása** a következő parancs futtatásával. Ez a példa a `-VNetName` a klasszikus VNet neve és `-LocalNetworkSiteName` a helyi hálózaton, ha a klasszikus portálon indításra megadott neve. A `-SharedKey` érték, amely készíthet, és adja meg. Az itt megadott érték, akkor adja meg a következő lépés a kapcsolat létrehozásakor ugyanazt az értéket kell lennie. Ez a példa a Németországi meg kell jelennie **állapota: a sikeres**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. A virtuális Magánhálózati kapcsolat létrehozása a következő parancs futtatásával.

    **A változók beállítása**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **A kapcsolat létrehozása**<br> Megjegyzendő, hogy a `-ConnectionType` IPsec, nem Vnet2Vnet van.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. A kapcsolat mindkét portálon vagy használatával megtekintheti a `Get-AzureRmVirtualNetworkGatewayConnection` parancsmag.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>VNet-VNet – gyakori kérdések

További információ a VNet-VNet kapcsolatok – gyakori kérdések részleteinek megtekintése.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


