<properties
   pageTitle="A klasszikus telepítési modell VNet-VNet kapcsolat beállítása |} Microsoft Azure"
   description="Hogyan lehet együttes használata a PowerShell és az Azure klasszikus portal Azure virtuális hálózatok csatlakoztatni."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>A klasszikus telepítési modell VNet-VNet kapcsolat beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klasszikus – klasszikus portál](virtual-networks-configure-vnet-to-vnet-connection.md)



Ez a cikk végigvezeti a hozhat létre és közös használata a klasszikus telepítési modell (más néven Szolgáltatáskezelés) virtuális hálózatok csatlakoztatása. Az alábbi lépéseket az Azure klasszikus portal segítségével hozza létre a VNets és átjárók és PowerShell állítsa be a VNet-VNet kapcsolatot. A kapcsolat nem konfigurálhatók a portálon.

![VNet VNet kapcsolódási-diagramhoz](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Telepítési modellek és VNet-VNet kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

A következő táblázat mutatja a jelenleg elérhető telepítési modellek és módszerek VNet-VNet konfigurációk. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>VNet-VNet kapcsolatokról

Virtuális hálózat egy másik virtuális hálózathoz csatlakozik (VNet VNet) hasonlít egy helyszíni hely kapcsolódás virtuális hálózat. Mindkét kapcsolat típusát a virtuális Magánhálózati átjáró segítségével adja meg a biztonságos alagutas szolgáltatással IPsec /. 

A csatlakozás VNets lehet a különböző előfizetések és különböző területei. Több elem webhely konfigurációjának VNet kommunikáció VNet is összevonhatja. Ezzel a radírral hálózati topológiák közötti virtuális a hálózati kapcsolat határokon helyszíni kapcsolatokkal kombináló megállapításához.


### <a name="why-connect-virtual-networks"></a>Miért kapcsolódás virtuális hálózatok?

Érdemes virtuális hálózatok csatlakoztatása az alábbi okok miatt:

- **Régió geo-redundancia és geo-jelenléti Cross**
    - Beállíthatja a saját geo replikációs vagy a szinkronizálás a biztonságos kapcsolatok túllépte a internetes végpontok nélkül.
    - Azure terheléselosztó és a Microsoft vagy harmadik fél fürtképző technológia beállíthatja a redundancia geo a könnyen hozzáférhető terhelést több Azure területek között. Egy fontos példája állíthatja be az SQL mindig a elérhetőség csoportokkal terjesztése több Azure területek között.

- **Erős elkülönítési határa területi több szálon futó alkalmazások**
    - Az azonos régión belüli állíthat be több szálon futó alkalmazások a több VNets együtt erős elkülönítési és biztonságos réteg közötti kommunikáció csatlakozik.

- **Idegen előfizetését, a szervezet közötti kommunikáció Azure-ban**
    - Több Azure-előfizetés birtokában csatlakoztathatja a különböző előfizetések munkaterhelésekből közös biztonságosan virtuális hálózatok között.
    - Nagyvállalatoknak vagy szolgáltatók engedélyezheti határokon-szervezet kommunikációs biztonságos VPN technológiával Azure belül.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>A klasszikus VNets VNet-VNet – gyakori kérdések

- A virtuális hálózatok az egyforma vagy különböző előfizetések lehet.

- A virtuális hálózatok lehet ugyanazon vagy másik Azure régióban (helyek).

- Egy felhőalapú szolgáltatásba vagy egy terheléselosztási végpont virtuális hálózatokon nem lehet akkor is, ha a közös csatlakozik.

- Több virtuális hálózatok összekapcsolása nincs szükség a virtuális Magánhálózati eszközöket.

- VNet-VNet összekötő Azure virtuális hálózatok támogatja. Nem támogatja az összekötő virtuális gépeken futó, és nem virtuális hálózathoz nem telepített felhőszolgáltatásokba.

- VNet-VNet dinamikus útválasztási átjárók szükséges. Azure statikus útválasztási átjárók nem támogatottak.

- Virtuális a hálózati kapcsolat kínál egyszerre több webhelyen VPN adatai. Legfeljebb 10 VPN bújtatás vagy más virtuális hálózatok, vagy a helyszíni webhelyek kapcsolódás virtuális hálózati VPN átjáró nem.

- A cím szóközöket virtuális hálózatok és a helyszíni helyi hálózati helyek a kell nem áll átfedésben. Egymást átfedő cím szóközöket okoz a virtuális hálózatok vagy meghiúsító feltöltése netcfg konfigurációs fájlok létrehozása.

- Felesleges alagutak két virtuális hálózatok között nem támogatottak.

- A VNet P2S VPN, beleértve az összes VPN bújtatás ossza meg az elérhető sávszélesség a virtuális Magánhálózati átjáró és ugyanazon VPN átjáró felmérést SLA Azure-ban.

- VNet-VNet forgalom az Azure Gerinchez keresztül létesített.


## <a name="step1"></a>Lépés: 1 – az IP-címtartományok megtervezése

Fontos döntse el, amely a virtuális hálózatok konfigurálása kell megadnia a tartomány. Ebben a konfigurációban gondoskodnia kell arról, hogy ne VNet tartományt fedjék egymást egymással, vagy a helyi hálózat ezeket bejelentkezzen bármelyikét.

A következő táblázat mutatja a példa a VNets megadása. A tartomány használata csak útmutatást. Jegyezze fel a virtuális hálózatok a tartományokat. Ez az információ újabb lépéseket szükség van.

**Példa beállításai**

|Virtuális hálózati  |Címterület használatára               |Régió      |Csatlakozik a helyi hálózati hely|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |Amerikai Egyesült Államok nyugati     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Japán keleti  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Lépés: 2 - VNet1 létrehozása

Ebben a lépésben VNet1 hozzunk létre. Amikor valamelyik példák, feltétlenül helyett a saját értékeket. Ha a VNet már létezik, akkor nem kell végezze el ezt a lépést. De, ellenőrizze, hogy az IP-címtartományok a második VNet nem átfedést a tartományokat, és bármelyik, amelyhez a többi VNets, amelyhez csatlakozni szeretne.

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com). Ez a cikk azt be a klasszikus portál, mert a szükséges konfigurációs beállítások egy része még nem érhetők el az Azure-portálon.

2. Kattintson a képernyő bal alsó sarkában az **Új** > **Hálózati szolgáltatások** > **Virtuális hálózati** > **Egyéni létrehozása** a konfigurálása varázsló elindításához. Nyissa meg a varázsló lépéseit, mint hozzáadása a megadott értékek minden lapjához.

### <a name="virtual-network-details"></a>Virtuális hálózati részletei

A virtuális hálózati részletek lapon adja meg a következő adatokat:

  ![Virtuális hálózati részletei](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Név** - a virtuális hálózat nevét. Ha például VNet1.
  - **Hely** – egy virtuális hálózati létrehozásakor, kapcsolhatja azt az Azure helyeket (régió). Például ha azt szeretné, hogy a nyugati USA-beli fizikailag található a virtuális hálózathoz telepített VMs, jelölje ki azt a helyet. A helye a virtuális hálózat létrehozásuk után nem módosítható.

### <a name="dns-servers-and-vpn-connectivity"></a>A DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat

A DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat lapon írja be az alábbi adatokat, és kattintson a jobb alsó sarkában lévő következő nyílra.

  ![A DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **A DNS-kiszolgálók** - adja meg a DNS-kiszolgáló neve és IP-címet, vagy a legördülő listából válassza ki a korábban már regisztrált a DNS-kiszolgáló. Ez a beállítás nem hoz létre a DNS-kiszolgáló. Lehetővé teszi, hogy adja meg a DNS-kiszolgálók névfeloldás a virtuális hálózathoz használni kívánt. Ha azt szeretné, hogy a névfeloldás a virtuális hálózatok között, akkor állítsa be a saját DNS-kiszolgáló helyett használja az Azure által biztosított névfeloldás.
- Nem jelöli ki bármelyik P2S vagy S2S kapcsolatot a jelölőnégyzetét. Kattintson az Ugrás a következő képernyő jobb alsó sarkában lévő nyílra.

### <a name="virtual-network-address-spaces"></a>Virtuális hálózati cím szóköz

A virtuális hálózati cím szóközöket lapon adja meg a virtuális hálózathoz használni kívánt címet tartományt. Ezek a dinamikus IP-címek (IMMERZIÓBAN), amely a VMs és a többi szerepkör példányát, amely a virtuális hálózati rendszerbe fog kiosztani. 

Ha a kapcsolat a helyszíni hálózaton is rendelkező VNet hoz létre, nagyon fontos jelölje ki a tartományt, amely nem áll átfedésben a helyszíni hálózaton használt tartományt. Ebben az esetben meg kell a hálózati rendszergazda koordinálása. A hálózati rendszergazda meg egy a helyszíni hálózaton cím adhatja meg a VNet használni az IP-címeket carve szükséges.

  ![Virtuális hálózati cím szóközöket lap](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - A **Címterület** – többek között a kezdő IP- és a cím szám. Győződjön meg arról, hogy a megadott cím szóközöket nem diagramterület fölé, hogy a helyszíni hálózaton van a cím szóközöket közül. Ebben a példában a 10.1.0.0/16 az VNet1 használjuk.
  - **Hozzáadás alhálózat** – többek között a kezdő IP- és a cím szám. További alhálózathoz nem szükséges, de érdemes hozzon létre külön alhálózathoz VMs, akihez statikus IMMERZIÓBAN. Vagy van-e a VMs alhálózat különböző nézeten a többi szerepkör futó példányát, érdemes lehet.
 
**Kattintson a pipajeles gombra** a jobb alsó sarkában az oldal és a virtuális hálózat meg is kezdi hozhat létre. Ha elkészült, látni fogja az állapot "Létrehozott" megjelenik a hálózatok lapon.

## <a name="step-3---create-vnet2"></a>Lépés a 3 - VNet2 létrehozása

Ezután ismételje meg az előző lépéseket, egy másik VNet létrehozásához. Újabb lépéseket a két VNets fog kapcsolódni. A [Példa beállítások](#step1) az 1 is hivatkozhat. Ha a VNet már létezik, akkor nem kell végezze el ezt a lépést. Azonban ellenőrizni szeretné, hogy mely IP-címtartományokat nem átfedést bármely más VNets vagy a helyszíni hálózatok, amelyhez csatlakozni szeretne.

## <a name="step-4---add-the-local-network-sites"></a>Lépés: 4 – a helyi hálózati helyek hozzáadása

Amikor létrehoz egy VNet-VNet konfigurációt, kell jelennek meg a **Helyi hálózat** lapon a portál helyi hálózati helyek konfigurálása. Azure annak irányítja a forgalmat a VNets között az egyes helyi hálózati helyek megadott beállításokat használja. Egyes helyi hálózati helyek hivatkozni használni kívánt nevének megállapításához. Célszerű használni egy leíró üzenetet egy újabb lépéseket a legördülő listából válassza ki a értéket.

Ha például VNet1 helyi hálózati webhelyre létrehozott "VNet2Local" nevű csatlakozik. VNet2Local beállításainak tartalmazzák, a cím prefixumokban a VNet2 és a VNet2 átjáró egy nyilvános IP-címét. Helyi hálózaton webhely hoz létre elnevezett "VNet1Local" VNet1 és az VNet1 átjáró nyilvános IP-címét a cím előtagot tartalmazó VNet2 csatlakozik.

### <a name="localnet"></a>A helyi hálózati hely VNet1Local hozzáadása

1. Kattintson a képernyő bal alsó sarkában az **Új** > **Hálózati szolgáltatások** > **Virtuális hálózati** > **Hozzáadása a helyi hálózaton**.

2. **A helyi hálózaton részletek megadása** lapon **nevét**, írja be egy nevet, amelyet a hálózat, amelyhez csatlakozni szeretne képviseletére. Ebben a példában a "VNet1Local" használhatja az IP-címtartományok és az átjáró-VNet1 hivatkozni.

3. A **virtuális Magánhálózati eszköz IP-cím (nem kötelező)**adja meg, bármely érvényes nyilvános IP-cím. A tényleges külső IP-cím általában VPN-eszközhöz szeretné használni. VNet-VNet konfigurációk esetén használhatja az átjáró a VNet rendelt nyilvános IP-címét. De tekintve, hogy az átjáró még nem létrehozott, bármely érvényes nyilvános IP-cím helyőrző megadhatja. Ne hagyja üresen a mezőt, – még nem választható ebben a konfigurációban. A leendő térjen vissza be ezeket a beállításokat, és állítson be őket a megfelelő átjáró IP-címek Azure hoz létre, miután. Kattintson a nyílra kattintva jelenítheti meg a következő képernyőn.

4. Az **Adja meg a cím lapon**adja meg a IP cím tartomány és cím darabszáma az VNet1. Ezt meg kell felelnie pontosan a tartományban, amely VNet1 van beállítva. Azure használja az IP-címtartományok megadott VNet1 szánt forgalmat. Kattintson a helyi hálózat létrehozása a bejelölést.

### <a name="add-the-local-network-site-vnet2local"></a>A helyi hálózati hely VNet2Local hozzáadása

A fenti lépések segítségével hozza létre a helyi hálózaton "VNet2Local". Is hivatkozhat az 1, [például beállítások](#step1) értékeket, ha szükséges.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Mutasson a helyi hálózaton minden VNet konfigurálása

A megfelelő helyi hálózaton irányítja a forgalmat a kívánt minden egyes VNet kell mutatnia. 

#### <a name="for-vnet1"></a>A VNet1

1. Nyissa meg **a virtuális hálózat **VNet1**lap** . 
2. A webhely módja csoportban jelölje be a "Csatlakozás a helyi hálózathoz", és válassza a legördülő listából a helyi hálózaton **VNet2Local** . 
3. A beállítások mentéséhez.

#### <a name="for-vnet2"></a>A VNet2

1. Nyissa meg **a virtuális hálózat **VNet2**lap** . 
2. A webhely módja csoportban jelölje be a "Csatlakozás a helyi hálózathoz", majd a helyi hálózaton, a legördülő listából válassza ki a **VNet1Local** . 
3. A beállítások mentéséhez.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Lépés az 5 - minden VNet az átjáró beállítása

Az egyes virtuális hálózatok dinamikus útválasztás átjáró beállítása. Ez a beállítás nem támogatja az átjárók statikus útválasztás. Ha korábban beállított, és már van dinamikus útválasztás átjárók VNets használ, akkor nem kell végezze el ezt a lépést. Ha az átjárók statikus útválasztás, kell törölheti őket, és hozza újra létre őket, dinamikus útválasztás átjárók. Ha töröl egy átjárót, a nyilvános IP-cím rendelt kiadott kap, és térjen vissza kell a helyi hálózat és a virtuális Magánhálózati eszközök az új nyilvános IP-címet az új átjáró átkonfigurálása.

1. A **hálózatok** lapon ellenőrizze, hogy az Állapot oszlopban a virtuális hálózat **létrehozott**.

2. Kattintson a **név** oszlopban a virtuális hálózat nevét. Ebben a példában a "VNet1" használjuk.

3. Az **Irányítópult** lapon figyelje meg, hogy a VNet nincs konfigurálva még az átjárók. Ez az állapot módosítása a műveletek elvégzéséhez az átjáró beállítása menet közben láthatja.

4. A lap alján kattintson az **Átjáró létrehozása** és **Dinamikus útválasztás**. Amikor a rendszer kéri, hogy erősítse meg, hogy az átjáró létrehozott, kattintson az Igen gombra.

    ![Átjáró típusa](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Amikor az átjáró hoz létre, és figyelje meg a lapon az átjáró ábra sárga változik, és megjelenik a "Átjáró létrehozása". Általában az átjáró létrehozásához körülbelül 30 percet vesz igénybe.

6. Ismételje meg ezeket a lépéseket VNet2. Mielőtt elkezdené az átjáró létrehozása a más VNet befejezéséhez az első VNet átjáró nem szükséges.

7. Amikor az átjáró állapota "Csatlakozás" változik, az egyes átjáró nyilvános IP-címe látható az irányítópult. Jegyezze fel az IP-címet, amely megfelel az egyes VNet ügyelve nem arra, hogy keverjen ki őket. Ezek az IP-címek, amely a helyőrző IP-címek szerkesztésre a virtuális Magánhálózati eszköz egyes helyi hálózaton használnak.

## <a name="step-6---edit-the-local-network"></a>Lépés a 6 - a helyi hálózaton szerkesztése

1. **Helyi hálózatok** lapon kattintson a helyi hálózat nevét, amely a szerkeszteni kívánt nevére, majd a lap alján a **Szerkesztés** gombra. **Virtuális Magánhálózati eszköz IP-cím**a szövegbeviteli az átjáró, amely megfelel a VNet IP-címét. Például az VNet1Local, akkor a VNet1 hozzárendelt gateway IP-címet. Kattintson a lap alján lévő nyílra.

2. **Adja meg a címterületet** lapján kattintson a képernyő jobb alsó sarkában kattintson a bejelölést anélkül, hogy a módosításokat.

## <a name="step-7---create-the-vpn-connection"></a>Lépés a 7 – a virtuális Magánhálózati kapcsolat létrehozása

Az előző lépések elvégzése után, amikor kívánja állítani a IPsec/IKE előre megosztott kulcsokat, és hozza létre a kapcsolatot. A lépéscsoportra PowerShell használ, és nem állítható be a portálon. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) Azure PowerShell-parancsmagok telepítésével kapcsolatos további információt. Győződjön meg arról, hogy a szolgáltatás felügyeleti (KOVÁ) parancsmagok legfrissebb verziójának letöltéséhez. 

1. Nyissa meg a Windows Powershellt, és jelentkezzen be.

        Add-AzureAccount

2. Jelölje ki azt az előfizetést, a VNets tárolnak.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. A kapcsolatok létrehozása A példákban figyelje meg, hogy a megosztott kulcs pontosan megegyezik. A megosztott kulcs mindig kell lennie.


    VNet1 VNet2 kapcsolat

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1 kapcsolat

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Várja meg a kapcsolatok inicializálni. Miután az átjáró inicializálni tartalmaz, az átjáró néz ki az alábbi ábrán.

    ![Átjáró állapota - csatlakoztatott](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Következő lépések

A virtuális hálózatok virtuális gépeken futó vehet. További információt a [virtuális gépeken futó dokumentációjában](https://azure.microsoft.com/documentation/services/virtual-machines/) találhat.



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
