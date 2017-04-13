<properties 
   pageTitle="Erőforrás-kezelő virtuális hálózatok a portálon klasszikus virtuális hálózatok keresztüli |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre a virtuális Magánhálózati kapcsolat klasszikus VNets és erőforrás-kezelő VNets virtuális Magánhálózati átjáró és a portál használata között"
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
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>A portál különböző telepítési modellekből virtuális hálózatok csatlakoztatása

> [AZURE.SELECTOR]
- [Portál](vpn-gateway-connect-different-deployment-models-portal.md)
- [A PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure által jelzett két management modellek: klasszikus és erőforrás Manager (erőforrás-kezelő). Ha Ön használták Azure egy kis időt, valószínűleg van Azure VMs és a klasszikus VNet futó példányát szerepkörök. A újabb VMs és szerepkör-példányok egy erőforrás-kezelő létrehozott VNet kell futnia. Ez a cikk végigvezeti klasszikus VNets csatlakoztatása az erőforrás-kezelő VNets engedélyezése a külön telepítési modellekben kapcsolatba lépni a többi átjáró kapcsolaton keresztül erőforrásait. 

Létrehozhat, amelyek a különböző előfizetések és különböző régiókban VNets közötti kapcsolat. Már a kapcsolatokat a helyszíni hálózatokon VNets mindaddig, amíg az átjáró a konfigurált dinamikus vagy útvonal is csatlakozhat. További információt a VNet-VNet kapcsolatok olvassa el az [VNet-VNet – gyakori kérdések](#faq) a Ez a cikk végén.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Telepítési modellek és VNet-VNet kapcsolatok módszerei

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Az alábbi táblázat frissítjük új cikkekben, és további eszközök lesznek elérhetők, ebben a konfigurációban. Egy cikk érhető el, ha azt csatolása közvetlenül az értékeket.<br><br>

**VNet-VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Mielőtt elkezdené

Az alábbi lépésekkel ismerteti, hogy az egyes VNet dinamikus vagy útvonal átjáró beállítása, és az átjárók közötti virtuális Magánhálózati kapcsolat létrehozásához szükséges beállításokat. Ez a beállítás nem támogatja a statikus vagy csoportházirend-alapú átjárókat. 

Ez a cikk használjuk, a klasszikus portál, az Azure-portál és PowerShell. Jelenleg még nem lehet létrehozni, ebben a konfigurációban az Azure portálon.

### <a name="prerequisites"></a>Előfeltételek

 - Mindkét VNets már elkészült.
 - A cím tartományokat a VNets nem az egymást átfedő, vagy az egyéb kapcsolatot, amely az átjárók is kapcsolódik a tartományok bármelyikének diagramterület fölé.
 - Telepítve van a legújabb PowerShell-parancsmagok (1.0.2-es verziójú vagy újabb). Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) további információt. Győződjön meg arról, hogy a szolgáltatás felügyeleti (KOVÁ) és a az erőforrás Manager (erőforrás-kezelő) parancsmag telepítése. 

### <a name="values"></a>Példa beállításai

A példa beállítások hivatkozásként is használhatja.

**Klasszikus VNet beállításai**

VNet neve = ClassicVNet <br>
Hely = US nyugati <br>
Virtuális hálózati cím szóközöket = 10.0.0.0/24 <br>
Alhálózat-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Helyi Network Name = RMVNetLocal <br>

**Erőforrás-kezelő VNet beállításai**

VNet neve = RMVNet <br>
Erőforráscsoport = RG1 <br>
Virtuális hálózati IP-cím szóközöket = 192.168.0.0/16 <br>
Alhálózat-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Hely = kelet-Amerikai Egyesült Államok <br>
Virtuális hálózati átjáró neve = RMGateway <br>
Átjáró nyilvános IP-neve = gwpip <br>
Átjáró típus = VPN <br>
VPN-típus = útvonal-alapú <br>
Helyi hálózaton átjáró = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Szakasz 1: Klasszikus VNet beállításainak konfigurálása

Ebben a részben hozzunk létre a helyi hálózat és az átjáró a klasszikus VNet számára. Ez a szakasz utasításait a klasszikus portálon használja. Jelenleg az Azure portálon nem ajánlja fel a beállításokat, amelyek a klasszikus VNet vonatkoznak.

### <a name="part-1---create-a-new-local-network"></a>1 rész - hozzon létre egy új helyi hálózaton

Nyissa meg a [Klasszikus portál](https://manage.windowsazure.com) , és jelentkezzen be az Azure-fiókjával.

1. A bal alsó sarkában a képernyő, kattintson az **Új** > **Hálózati szolgáltatások** > **Virtuális hálózati** > **hozzáadása a helyi hálózaton**.

2. **Adja meg a helyi hálózaton részleteket** ablakban írja be a nevét az erőforrás-kezelő VNet, amelyhez csatlakozni szeretne. A **virtuális Magánhálózati eszköz IP-cím (nem kötelező)** mezőbe írja be a bármely érvényes nyilvános IP-cím. Ez egy ideiglenes helyőrző. Az IP-cím később módosíthatja. Kattintson a jobb alsó sarkában az ablakban, a nyílra.
 
3. **A címterület megadása** lapon **Kezdő IP-** mezőbe írja be a hálózati előtag és CIDR blokkokból álló az erőforrás-kezelő VNet, amelyhez csatlakozni szeretne. Ez a beállítás a címterület és az erőforrás-kezelő VNet irányítja megadására szolgál.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>A helyi hálózat a VNet társítása 2 - kijelző

1. Kattintson a **Virtuális hálózatok** elemre a lap tetején a virtuális hálózatok képernyőre váltáshoz, majd kattintással jelölje ki a klasszikus VNet. A VNet lapján kattintson a konfiguráció lapjának **beállítása** gombra.

2. **Hely közötti kapcsolat** kapcsolat csoportjában jelölje be a **Csatlakozás a helyi hálózathoz** jelölőnégyzet. Válassza a helyi hálózaton, Ön által létrehozott. Ha több helyi hálózat, Ön által létrehozott, feltétlenül jelölje ki a létrehozott az erőforrás-kezelő VNet jelenítik meg a legördülő listából.

3. A lap alján a **Mentés** gombra.

### <a name="part-3---create-the-gateway"></a>Rész 3 – az átjáró létrehozása

1. Miután mentette a beállításokat, kattintson az **Irányítópult** , módosíthatja az irányítópult lapra a lap tetején. Kattintson az irányítópult lap alján **Átjáró létrehozása**, majd kattintson a **Dinamikus útválasztás**. Kattintson az **Igen gombra** a kezdéshez az átjáró létrehozása. Egy dinamikus útválasztás átjáró, a fenti konfiguráció szükséges.

2. Várja meg az átjáró hozható létre. Előfordul, hogy jelképező 45 perc, vagy több befejezéséhez.

### <a name="ip"></a>4 – megtekintése az átjáró nyilvános IP-cím része

Az átjáró létrehozása után, az **Irányítópult** lapon megtekintheti az átjáró IP-cím. Ez az, hogy az átjáró nyilvános IP-címét. Írja le vagy a nyilvános IP-címet másolja. Használhatja a későbbi lépéseket az erőforrás-kezelő VNet konfigurációjában a helyi hálózaton létrehozásakor.


## <a name="creatermgw"></a>Szakasz 2: Az erőforrás-kezelő VNet beállításainak konfigurálása

Ebben a részben hozzunk létre virtuális hálózati átjáró és a helyi hálózaton az erőforrás-kezelő VNet számára. A nyilvános IP-címe a klasszikus VNet átjáró beolvasása után nem indítsa el addig, amíg az alábbi lépéseket.

A számítógépen készített képernyőképeket példaként szolgálnak. Ügyeljen arra, hogy az értékek lecserélése saját. Ha ebben a konfigurációban, mint egy torna hoz létre, kövesse ezeket az [értékeket](#values).


### <a name="part-1---create-a-gateway-subnet"></a>1 rész - alhálózat átjáró létrehozása

Mielőtt az átjárók a virtuális hálózathoz csatlakozik, először kell a virtuális hálózat, amelyhez csatlakozni az átjárót alhálózat létrehozása. Hozzon létre egy átjáró alhálózat CIDR darab /28 vagy nagyobb (27, / / 26, stb.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

A böngészőben nyissa meg az [Azure portál](http://portal.azure.com) , és jelentkezzen be az Azure-fiók.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>2 rész – ezek olyan virtuális hálózati átjáró létrehozása


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Rész 3 – a helyi hálózaton átjáró létrehozása

A helyi hálózaton átjáró a helyszíni hely általában hivatkozik. Azure közli, mely IP-cím csak jelszóval módosítható tartományok útvonalat a helyet, és a nyilvános IP-címét az adott helyet az eszközön. Jó helyen jár ebben az esetben, amire hivatkozik a cím tartomány és nyilvános IP-címét a klasszikus VNet és a virtuális hálózati átjáró.

Adjon meg egy nevet, amellyel Azure is a hivatkozott a helyi hálózati átjáró. A helyi hálózati átjáró hozhat létre, a virtuális hálózati átjáró létrehozása közben. Ebben a konfigurációban használhatja a nyilvános IP-címet, amely az [előző szakaszban](#ip)a klasszikus VNet átjáró van beállítva.

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Kijelző 4 – a nyilvános IP-cím másolása

Amikor a virtuális hálózati átjáró befejeződik, létrehozása, másolja a nyilvános IP-címet, amely a társított átjáró. Használhatja, ha a helyi hálózat beállításainak megadása a klasszikus VNet. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Szakasz 3: A klasszikus VNet módosíthatja a helyi hálózaton

Nyissa meg a [Klasszikus portálon](https://manage.windowsazure.com).

2. A klasszikus portálon görgessen lefelé, a bal oldalon, és kattintson a **hálózatok**. A **hálózatok** lapon kattintson a **Helyi hálózat** elemre a lap tetején. 

3. Kattintással jelölje ki a helyi hálózaton konfigurált rész 1. A lap alján kattintson a **Szerkesztés**gombra.

4. **A helyi hálózaton részletek megadása** lapon a nyilvános IP-címet az erőforrás-kezelő átjáró az előző részben létrehozott cserélje le a helyőrző IP-címét. Kattintson a nyílra kattintva helyezze át a következő szakaszban. Ellenőrizze, hogy helyes-e a **Címterület** , és válassza a be van jelölve a módosítások elfogadásához.

## <a name="connect"></a>Szakasz 4: A kapcsolat létrehozása

Ebben a szakaszban a VNets közötti kapcsolat hozzunk létre. A lépések PowerShell. A kapcsolat a portálokról egyikével nem hozható létre. Győződjön meg arról, le, és telepítve van a klasszikus (KOVÁ) és az erőforrás Manager (erőforrás-kezelő) PowerShell-parancsmagok.


1. Jelentkezzen be a PowerShell konzolban Azure fiókját. A következő parancsmagot, a bejelentkezési hitelesítő adatokat kér az Azure-fiók. Naplózás után a Fiókbeállítások, hogy elérhetők Azure PowerShell töltődnek le.

        Login-AzureRmAccount 

    Ismerkedés az Azure-előfizetések listáját, egynél több szóló előfizetés.

        Get-AzureRmSubscription

    Adja meg az előfizetést, szeretné használni. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. A klasszikus PowerShell-parancsmagok használata az Azure-fiók felvétele. Ehhez a következő parancsot is használhatja:

        Add-AzureAccount

3. A megosztott kulcs beállítása a következő példa futtatásával. Ez a példa a `-VNetName` a klasszikus VNet neve és `-LocalNetworkSiteName` a helyi hálózaton, ha a klasszikus portálon indításra megadott neve. A `-SharedKey` érték, amely készíthet, és adja meg. Az itt megadott érték, akkor adja meg a következő lépés a kapcsolat létrehozásakor ugyanazt az értéket kell lennie.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. A virtuális Magánhálózati kapcsolat létrehozása a következő parancs futtatásával:
    
    **A változók beállítása**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **A kapcsolat létrehozása**<br> Megjegyzendő, hogy a `-ConnectionType` "IPsec" nem "Vnet2Vnet" van. Ez a példa a `-Name` van a nevet, amelyet fel szeretne hívni a kapcsolatot. A következő példa létrehoz egy "*erőforrás-kezelő a klasszikus kapcsolat*" nevű kapcsolatot.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Ellenőrizze a kapcsolat

A kapcsolat ellenőrizheti a klasszikus portál, az Azure portálon vagy a PowerShell használatával. Az alábbi lépésekkel ellenőrizze a kapcsolat használható. Cserélje ki az értékeket a saját.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>VNet-VNet – gyakori kérdések

További információ a VNet-VNet kapcsolatok – gyakori kérdések részleteinek megtekintése.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


