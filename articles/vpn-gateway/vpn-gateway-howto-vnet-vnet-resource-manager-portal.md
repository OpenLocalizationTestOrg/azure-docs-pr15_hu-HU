<properties
   pageTitle="Az erőforrás-kezelő telepítési modell és az Azure portal segítségével VNets csatlakozás |} Microsoft Azure"
   description="Kapcsolat létrehozása a virtuális Magánhálózati átjáró között VNets erőforrás-kezelő és az Azure portal segítségével."
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
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Az Azure portálon VNet-VNet kapcsolat beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasszikus – klasszikus portál](virtual-networks-configure-vnet-to-vnet-connection.md)


Ez a cikk végigvezeti a virtuális Magánhálózati átjáró és az Azure portal segítségével erőforrás-kezelő telepítési modellben VNets közötti kapcsolat létrehozásához.

Az Azure portal segítségével virtuális hálózatok csatlakoztatása, amikor a VNets ugyanabban az előfizetésben kell lennie. A virtuális hálózatok a különböző előfizetések esetén továbbra is csatlakoztathatja őket az [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) lépésekkel.

![v2v diagram](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Telepítési modellek és VNet-VNet kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

A következő táblázat mutatja a jelenleg elérhető telepítési modellek és módszerek VNet-VNet konfigurációk. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>VNet-VNet kapcsolatokról

Virtuális hálózat egy másik virtuális hálózathoz csatlakozik (VNet VNet) hasonlít egy VNet csatlakozik egy helyszíni hely. Mindkét adatkapcsolat-típusokat az Azure virtuális Magánhálózati átjáró segítségével adja meg a biztonságos alagutas szolgáltatással IPsec /. A csatlakozás VNets lehet különböző területei vagy a különböző előfizetések.

Még több webhelyen konfigurációk VNet-VNet kommunikációt is összevonhatja. Hálózati topológiák kombináló határokon helyszíni közötti virtuális a hálózati kapcsolat, kapcsolatot létrehozni, a következő ábrán látható módon lehetővé:

![Kapcsolatok] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Kapcsolatok")

### <a name="why-connect-virtual-networks"></a>Miért kapcsolódás virtuális hálózatok?

Érdemes virtuális hálózatok csatlakoztatása az alábbi okok miatt:

- **Régió geo-redundancia és geo-jelenléti Cross**
    - Beállíthatja a saját geo replikációs vagy a szinkronizálás a biztonságos kapcsolatok túllépte a internetes végpontok nélkül.
    - Azure forgalom felettes és terheléselosztó beállíthatja a redundancia geo a könnyen hozzáférhető terhelést több Azure területek között. Egy fontos példája állíthatja be az SQL mindig a elérhetőség csoportokkal terjesztése több Azure területek között.

- **Területi több szálon futó alkalmazások elkülönítési vagy felügyeleti határérték**
    - Az azonos régión belüli állíthat be több szálon futó alkalmazások több virtuális hálózatokkal összekapcsolása elkülönítési vagy felügyeleti követelmények miatt.

További információt a VNet-VNet kapcsolatok olvassa el az [VNet-VNet – gyakori kérdések](#faq) a Ez a cikk végén.

### <a name="values"></a>Példa beállításai

A gyakorlat használja ezeket a lépéseket, ha a minta konfigurációs értékek is használhatja. Példa céljából a szóközöket címet az egyes VNet használjuk. VNet-VNet konfigurációk azonban több címet szóköz nincs szükség.

**TestVNet1 értékeit:**

- VNet neve: TestVNet1
- Hely címe: 10.11.0.0/16
    - Alhálózat neve: FrontEnd
    - Alhálózat címtartományokat: 10.11.0.0/24
- Erőforráscsoport: TestRG1
- Hely: Kelet-Amerikai Egyesült Államok
- Hely címe: 10.12.0.0/16
    - Alhálózat neve: Kódmentes
    - Alhálózat címtartományokat: 10.12.0.0/24
- Átjáró alhálózat neve: GatewaySubnet (ennek hatására a portálon AutoKitöltés)
    - Átjáró alhálózat címtartományokat: 10.11.255.0/27
- DNS-kiszolgáló: Használja a DNS-kiszolgáló IP-címe
- Virtuális hálózati átjáró neve: TestVNet1GW
- Átjáró típusa: VPN
- VPN típusa: útvonal-alapú
- Termékváltozat: Válassza ki a használni kívánt átjáró Termékváltozat
- Nyilvános IP-cím neve: TestVNet1GWIP
- Kapcsolat értékeket:
    - Név: TestVNet1toTestVNet4
    - Megosztott kulcs: a megosztott kulcs egyedül is létrehozhatja. Ebben a példában a abc123 használjuk. Figyeljen arra, hogy a VNets közötti kapcsolat létrehozásakor az értéket kell lennie.

**TestVNet4 értékeit:**

- VNet neve: TestVNet4
- Hely címe: 10.41.0.0/16
    - Alhálózat neve: FrontEnd
    - Alhálózat címtartományokat: 10.41.0.0/24
- Erőforráscsoport: TestRG1
- Hely: Nyugati Amerikai Egyesült Államok
- Hely címe: 10.42.0.0/16
    - Alhálózat neve: Kódmentes
    - Alhálózat címtartományokat: 10.42.0.0/24
- GatewaySubnet neve: GatewaySubnet (ennek hatására a portálon AutoKitöltés)
    - GatewaySubnet címtartományokat: 10.41.255.0/27
- DNS-kiszolgáló: Használja a DNS-kiszolgáló IP-címe
- Virtuális hálózati átjáró neve: TestVNet4GW
- Átjáró típusa: VPN
- VPN típusa: útvonal-alapú
- Termékváltozat: Válassza ki a használni kívánt átjáró Termékváltozat
- Nyilvános IP-cím neve: TestVNet4GWIP
- Kapcsolat értékeket:
    - Név: TestVNet4toTestVNet1
    - Megosztott kulcs: a megosztott kulcs egyedül is létrehozhatja. Ebben a példában a abc123 használjuk. Figyeljen arra, hogy a VNets közötti kapcsolat létrehozásakor az értéket kell lennie.


## <a name="CreatVNet"></a>1. a létrehozása és konfigurálása TestVNet1

Ha már van egy VNet, győződjön meg róla, hogy a beállításokat a virtuális Magánhálózati átjárók tervezése kompatibilis. Adott figyelmet bármely alhálózathoz, előfordulhat, hogy a más hálózatokon átfedést. Ha egymást átfedő alhálózat, a kapcsolat nem megfelelően működik. Ha a megfelelő beállításokat a VNet van beállítva, külön előkészületek nélkül használhatja a [DNS-kiszolgáló megadása](#dns) című szakasz lépéseit.

### <a name="to-create-a-virtual-network"></a>Virtuális hálózat létrehozása

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2. a további címterület hozzáadása és alhálózat létrehozása

További címterület hozzáadása, és hozzon létre alhálózat, a VNet létrehozása után.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>3. a átjáró alhálózat létrehozása

Mielőtt az átjárók a virtuális hálózathoz csatlakozik, először kell a virtuális hálózat, amelyhez csatlakozni az átjárót alhálózat létrehozása. Ha lehetséges érdemes létrehozni egy átjáró alhálózat alapcím CIDR /28 vagy /27 elég IP-címek további jövőbeli konfigurációs követelmények igazodik biztosítása érdekében.

Ha ez a beállítás, mint egy torna szeretne létrehozni, olvassa el a következő [Példa beállítások](#values) átjáró alhálózathoz létrehozásakor.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Átjáró alhálózat létrehozása

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="DNSServer"></a>4. Adja meg a DNS-kiszolgáló (nem kötelező)

Ha a VNets telepített virtuális gépeken futó névfeloldás szeretné használni, akkor kell megadnia a DNS-kiszolgáló.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="VNetGateway"></a>5. virtuális hálózati átjáró létrehozása

Ebben a lépésben a VNet hozza létre a virtuális hálózati átjáró. Ezt a lépést is ki 45 percet igénybe vehet. Ez a beállítás, mint egy torna hoz létre, ha, [például beállításokat](#values)is hivatkozhat.

### <a name="to-create-a-virtual-network-gateway"></a>A virtuális hálózati átjáró létrehozása

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="CreateTestVNet4"></a>6. létrehozása és konfigurálása TestVNet4

Miután beállította az TestVNet1, hozzon létre TestVNet4 az előző lépéseket, az értékek cseréje azok TestVNet4 megismétlésével. Nem kell várnia, amíg a virtuális hálózati átjáró TestVNet1 az befejeződött TestVNet4 konfigurálása előtt létrehozása. Ha a saját értékek használata esetén ellenőrizze, hogy a cím szóközöket nem átfedést csatlakozni, amelyet a VNets közül.


## <a name="TestVNet1Connection"></a>7. a TestVNet1 kapcsolat beállítása

Ha a virtuális hálózati átjárók TestVNet1 és a TestVNet4 befejeződött, átjáró kapcsolatok a virtuális hálózat hozhatnak létre. Ebben a részben létrehoznia a kapcsolatot a VNet1 VNet4 szeretne.

1. Az **összes erőforrás**lépjen a virtuális hálózati átjáró a VNet. Ha például **TestVNet1GW**. Kattintson a **TestVNet1GW** a virtuális hálózati átjárók lap megnyitásához.

    ![Kapcsolatok lap] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Kapcsolatok lap")

2. Válassza a **+ Hozzáadás gombra** kattintva nyissa meg a **kapcsolat hozzáadása** lap.

3. A **kapcsolat hozzáadása** lap Név mezőbe írja be egy nevet a kapcsolathoz. Ha például **TestVNet1toTestVNet4**.

    ![A kapcsolat neve] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "A kapcsolat neve")

4. **Kapcsolat típusa**. Jelölje ki a **VNet-VNet** a legördülő listából.

5. Az **első virtuális hálózati átjáró** mező értékét automatikusan kitölti, mivel a megadott virtuális hálózati átjáró hoz létre a kapcsolatot.

6. A **második virtuális hálózati átjáró** mező a virtuális hálózati átjáró, a kapcsolat létrehozása kívánt VNet. Kattintson a **Válasszon egy másik virtuális hálózati átjárót** a **Válassza ki a virtuális hálózati átjáró** lap megnyitásához.

    ![Kapcsolat hozzáadása] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Kapcsolat hozzáadása")

7. Ez a lap a virtuális hálózati átjárók szerint vannak listázva megtekintése. Figyelje meg, hogy csak az előfizetés a virtuális hálózati átjárók felsorolt. Ha szeretne egy virtuális hálózati átjárón, amely nem szerepel az előfizetést, használja a [PowerShell cikk](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Kattintson a virtuális hálózati átjáró, amely, amelyhez csatlakozni szeretne.
 
9. **A megosztott kulcs** mezőbe írja be a kapcsolat a megosztott kulcsot. Hozza létre, vagy a kulcs saját magának kell létrehoznia. A webhely kapcsolat használt kulcs lenne pontosan ugyanezt a helyszíni és a virtuális hálózati átjáró kapcsolat. Amely hasonló ebben az esetben kivételével, hanem VPN eszközt használ, egy másik virtuális hálózati átjáró csatlakozik.

    ![A megosztott kulcs] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "A megosztott kulcs")

10. Kattintson az **OK gombra** a lap alján a módosítások mentéséhez.

## <a name="TestVNet4Connection"></a>8. a TestVNet4 kapcsolat beállítása

Ezután hozzon létre kapcsolatot TestVNet4 TestVNet1. Ugyanazt a módszert, amellyel hozza létre a kapcsolatot TestVNet1 TestVNet4 használni. Győződjön meg arról, hogy ugyanazt a megosztott kulcsot használható.

## <a name="VerifyConnection"></a>9. a kapcsolat ellenőrzése

Ellenőrizze a kapcsolatot. Az egyes virtuális hálózati átjárót tegye a következőket:

1. Keresse meg a virtuális hálózati átjáró lap. Ha például **TestVNet4GW**. 
2. A virtuális hálózati átjárók lap kattintson a **kapcsolatok** a kapcsolatok lap a virtuális hálózati átjáró megtekintése gombra.

A kapcsolatok és állapotát. Miután a kapcsolat létrejött, látni fogja **sikerült** és **Connected** állapot értékeiként.

![Sikeres] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Sikeres")

Kattintson duplán minden kapcsolat külön-külön további információt a kapcsolat megtekintéséhez.

![Alapjai] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Alapjai")

## <a name="faq"></a>VNet-VNet – gyakori kérdések

További információ a VNet-VNet kapcsolatok – gyakori kérdések részleteinek megtekintése.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Következő lépések

A kapcsolat elkészülte virtuális gépeken futó hozzáadhatja a virtuális hálózatok. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.
