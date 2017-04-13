<properties
   pageTitle="A hibrid hálózat architektúrája készült Azure ExpressRoute a végrehajtási |} Architektúra hivatkozás |} Microsoft Azure"
   description="Hogyan egy Azure virtuális hálózat átnyúlik végrehajtása egy biztonságos webhely hálózat architektúrája, és a készült Azure ExpressRoute használatával csatlakozik egy helyszíni hálózaton."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Végrehajtási egy hibrid hálózat architektúrája és a készült Azure ExpressRoute

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti a gyakorlati tanácsok a Kapcsolódás a helyszíni hálózaton Azure virtuális hálózatok készült ExpressRoute használatával. Készült ExpressRoute kapcsolatok hozhatók létre, egy harmadik fél kapcsolódási közvetítésével magánjellegű dedikált kapcsolaton keresztül. A személyes kapcsolat átnyúlik Azure hozzáférést biztosít a saját IaaS infrastruktúra PaaS services és az Office 365-ben szoftver szolgáltatások használt Azure, nyilvános végpontok a helyszíni hálózaton. A dokumentum koncentrál csatlakozás egyetlen Azure virtuális hálózathoz (VNet) használata a magánjellegű peering úgynevezett készült ExpressRoute használatával.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A tervrajz használja, erőforrás-kezelő Microsoft új telepítési javasolja.

Ez a felépítés esettel jellemző használata a következők:

- Hibrid alkalmazások hol munkaterhelésekből között egy helyszíni hálózaton és Azure van meghatározva.

- Nagyméretű, kulcsfontosságú munkaterhelésekből méretezhetőség magas fokú igénylő futó alkalmazásokat.

- Nagyméretű biztonsági mentési és visszaállítási létesítményekhez adatokért, amelyeket nem a helyszínen történő kell menteni.

- Nagy adatok munkaterhelésekből kezelése.

- Azure használatával katasztrófaelhárító helyként.

> [AZURE.NOTE] A [készült ExpressRoute technikai áttekintés] [ expressroute-technical-overview] készült ExpressRoute áttekintése.

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli a fontos e architektúra összetevőket:

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "Hibrid network - ER" lapra.

![[0]][0]

- **Azure virtuális hálózatok (VNets).** Minden egyes VNet egyetlen Azure területen található, és több alkalmazás rétegek üzemeltetheti. Alkalmazás rétegek alhálózat használ az egyes VNet kell szegmentált és/vagy a hálózati biztonsági csoportok (NSGs). 

- **Azure nyilvános szolgáltatások.** Ezek a Azure szolgáltatások, amelyek egy hibrid alkalmazáson belül fogja alkalmazni. Az alábbi szolgáltatások is elérhetők az nyilvános interneten, de elérése őket egy készült ExpressRoute áramkör keresztül biztosít kis késés és további kiszámíthatóan teljesítmény óta forgalom terjed ki az interneten keresztül. Kapcsolatok használata a **nyilvános peering**, címet, amely a szervezet által birtokolt, vagy az adatkapcsolat-szolgáltató által biztosított végzi. 

- **Az Office 365-szolgáltatásokat.** Ezek a nyilvánosan elérhető az Office 365-alkalmazások és a Microsoft által nyújtott szolgáltatások. Kapcsolatok használata a **Microsoft peering**, címet, amely a szervezet által birtokolt, vagy az adatkapcsolat-szolgáltató által biztosított végzi.

>[AZURE.NOTE] Is csatlakozhat közvetlenül a Microsoft CRM Online a Microsoft peering keresztül.

- **A helyszíni vállalati hálózathoz.** Ez a hálózat és eszközfüggetlen módon, a szervezeten belül futó helyi magánhálózaton keresztül csatlakozik.

- **Helyi él útválasztó.** Ezek a helyszíni hálózaton csatlakoztatása az áramkör, a szolgáltató által kezelt útválasztó. Attól függően, hogy hogyan a kapcsolat már kiépítve meg kell adnia a nyilvános az útválasztó által használt IP-címeket. 

- **A Microsoft edge útválasztó.** Ezek a könnyen hozzáférhető aktív-aktív konfigurációban két útválasztó. Ezek az útválasztó engedélyezése a áramkörök közvetlenül csatlakozhat az adatközponthoz kapcsolódási szolgáltató. Attól függően, hogy hogyan a kapcsolat már kiépítve meg kell adnia a nyilvános az útválasztó által használt IP-címeket.

- **Készült ExpressRoute áramkör.** Ez az egy réteg 2, vagy az adatkapcsolat szolgáltató által biztosított layer 3 áramkör, hogy a helyszíni illesztések-hálózaton keresztül él útválasztó Azure. A kapcsolat a hardver infrastruktúra a csatlakozási szolgáltató által kezelt használja.

- **Csatlakozási szolgáltatók.** Ezek a vállalatok, amelyekkel a kapcsolat mindkét használatával 2 réteg vagy a adatközponthoz és az Azure adatközponthoz layer 3 összekapcsolását.

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúrája, kivéve, ha rendelkezik az adott követelmény, hogy nem férnek el a ajánlást célszerű követnie az alábbi javaslatokat.

### <a name="connectivity-providers"></a>Csatlakozási szolgáltatók

Válassza ki a megfelelő készült ExpressRoute kapcsolódási szolgáltatóját tartózkodási helyét. Szerezze be a csatlakozási szolgáltatót rendelkezésre álló a tartózkodási helyén, használja a következő Azure PowerShell-parancsot:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

Készült ExpressRoute kapcsolódási szolgáltatók az Adatközpont kapcsolatba lépni a Microsofttal az alábbi módon:

- **Dokumentumok közös egy felhőalapú exchange helyen található.** Az a felhő exchange intézményben közös tartózkodási helyének, sorrendbe is virtuális határokon-kapcsolatot a Microsoft cloud a helymegosztást szolgáltató Ethernet exchange keresztül. Helymegosztást szolgáltatók réteg 2 határokon-kapcsolatok vagy felügyelt Layer 3 határokon – közötti kapcsolatok az a helymegosztást intézményben infrastruktúra és a Microsoft cloud is kínálhatnak.

- **Pontok közötti Ethernet kapcsolatok.** A helyszíni adatközpontokkal/irodák pontok közötti Ethernet-kapcsolaton keresztül a Microsoft cloud csatlakozhat. Pontok közötti Ethernet szolgáltatók réteg 2 kapcsolatok is kínálhatnak, vagy a webhely és a Microsoft cloud Layer 3 kapcsolatának felügyelt.

- **Bármely-a-bármely (IPVPN) hálózatok.** A WAN integrálhatja a a Microsoft cloud. IPVPN szolgáltatók (általában MPLS VPN) a ág irodák és adatközpontokkal bármely-a-bármely összekapcsolását kínálnak. A Microsoft cloud a WAN, mint bármely más irodában külsőt össze kell kötni. WAN általában szolgáltatók felügyelt Layer 3-kapcsolatot.

Csatlakozási szolgáltatók kapcsolatos további tudnivalókért lásd: a [készült ExpressRoute áramkörök és útválasztási tartományok][connectivity-providers].

### <a name="expressroute-circuit"></a>Áramköri készült ExpressRoute

Győződjön meg arról, hogy a szervezet megfelelt [készült ExpressRoute kapcsolatban előzetesen szükséges követelményeknek] [ expressroute-prereqs] Azure csatlakozhat.

Ha, még nem tette meg, vegye fel a nevű alhálózat `GatewaySubnet` az Azure VNet szeretne, és hozzon létre egy készült ExpressRoute virtuális hálózati átjáró az Azure virtuális Magánhálózati átjáró szolgáltatással. Ez a folyamat kapcsolatos további tudnivalókért lásd a [készült ExpressRoute munkafolyamatok áramkör kiépítési és áramkör állapotát][ExpressRoute-provisioning].

Készült ExpressRoute áramkör létrehozása az alábbi képlettel történik:

1. Futtassa az alábbi PowerShell-parancsot:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Küldje el a `ServiceKey` az új kapcsolat szolgáltatónak.

3. Várja meg a szolgáltatót a kapcsolat kiépítése. A kapcsolat kiépítési állapotának ellenőrzéséhez az alábbi PowerShell-parancs használatával:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

A `Provisioning state` mezőjében a `Service Provider` a kimenet rész változik `NotProvisioned` való `Provisioned` amikor készen áll a áramkör.

>[AZURE.NOTE]Layer 3 kapcsolat használata, a szolgáltató beállítása és kezelése a továbbítás ki; a szükséges ahhoz, hogy a szolgáltató a megfelelő útvonalak végrehajtásához szükséges információkat.

Ha réteghez 2 kapcsolat használata esetén kövesse az alábbi lépéseket:

1. Két lefoglalása/30 alhálózat összehangolt érvényes nyilvános IP-címek, az egyes: a peering szeretné végrehajtani. Ezek/30 alhálózat szükséges IP-címek a áramkör használt útválasztó szolgálnak. Ha privát, a nyilvános és a Microsoft peering végrehajtási, 6 /30 alhálózat érvényes nyilvános IP-címek lesz szüksége.     

2. A készült ExpressRoute áramkör útválasztás konfigurálása. Az egyes: alatt parancsot kell a peering be szeretné állítani az (a lehetőséget választva személyes, a nyilvános és a Microsoft).

    >[AZURE.NOTE]Lásd: [létrehozása és módosítása a továbbítás egy készült ExpressRoute áramkör] [ configure-expressroute-routing] további információt. Az alábbi PowerShell parancsokkal útválasztási forgalmához peering hálózati hozzáadása:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Egy másik készlet érvényes nyilvános IP-címek használata a nyilvános hálózati címfordító Eszközig, majd a Microsoft peering foglalni. Javasoljuk, hogy az egyes peering különböző erőforráskészlethez tartozik. Adja meg a kapcsolat szolgáltatója a készlet, így azok adhatja meg az adott tartományok BGP hirdetések.

[A magánjellegű VNet(s) a felhőben csatolása a készült ExpressRoute áramkör][link-vnet-to-expressroute]. Az alábbi PowerShell parancsokat használhatja:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Vegye figyelembe az alábbiakat:

- Készült ExpressRoute szegéllyel átjáró Protocol (BGP) között a hálózat és Azure útválasztási adatok cseréjére használja.

- Az azonos készült ExpressRoute áramkör különböző régiókat található mindaddig, amíg az összes VNets és a készült ExpressRoute áramkör találhatók a azonos geopolitikai régión belüli több VNets lehet csatlakozni.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Készült ExpressRoute áramkörök adja meg a nagy sávszélesség elérési utat hálózatok között. Általánosságban elmondható annál nagyobb sávszélességre minél nagyobb a költség. Bár az egyes szolgáltatók engedélyezi a sávszélesség, feltétlenül választhat, egy kezdeti sávszélesség tartalmazza túllépi az igényeinek, és a NÖV számára. Abban az esetben, ha a jövőben növelése sávszélesség van szüksége, akkor két lehetőség közül maradnak.

A sávszélességének növelése Ne feledje, hogy nem az összes szolgáltatók lehetővé teszik, hogy dinamikusan. És lehetőleg ne kelljen végezze el az e szerint. De ha szükséges, az internetszolgáltatónál ellenőrzése után futtassa az alábbi parancsokat.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

A sávszélesség a kapcsolat veszteségmentes növelésével. Visszalépés a sávszélesség-ról eredményezi állásidőt azzal a kapcsolat. Ha törli a áramkör, és hozza létre ismét az új beállításokkal.

Ha használja a szabványos Termékváltozat készült ExpressRoute és a szükséges prémium frissítése vagy módosítása, hogy a árak terv (forgalmi díjas vagy korlátlan), az alábbi parancsokat. A `Sku.Tier` tulajdonság lehet `Standard` vagy `Premium`; a `Sku.Name` tulajdonság lehet `MeteredData` vagy `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Győződjön meg arról, hogy a `Sku.Name` tulajdonság találat a `Sku.Tier` és `Sku.Family`. Ha módosítja a család és szint, de nem a név, a kapcsolat le lesznek tiltva.

A Termékváltozat megszakítása nélkül tudja frissíteni, de a forgalmi korlátlan árak tervet, nem lehet váltani. Visszalépés a Termékváltozat-ról, amikor a sávszélességet kell maradnia, az alapértelmezett határértékén normál SKU belül.

> [AZURE.NOTE] Készült ExpressRoute két árak csomagok kínál ügyfeleinek, mérési vagy korlátlan adatok alapján. Lásd: a [készült ExpressRoute árak] [ expressroute-pricing] további információt. A díjak áramkör sávszélesség függően változnak. Rendelkezésre álló sávszélesség valószínűleg változhatnak szolgáltató szolgáltató. Használja a `Get-AzureRmExpressRouteServiceProvider` megjelenítéséhez a terület és a sávszélessége tartalmazó azokat az elérhető szolgáltatók parancsmag.

Egy egyetlen készült ExpressRoute áramkör számos különböző peerings és VNet hivatkozások is támogatja. Című cikkben találhat [készült ExpressRoute] [ expressroute-limits] további információt.

További, ingyenesen készült ExpressRoute prémium bővítményt tartalmaz:

- Legfeljebb 10 000 útvonalak per áramkör. Nélkül készült ExpressRoute prémium bővítmény a korlát per áramkör 4000 útvonalak.

- Globális kapcsolat engedélyezése egy készült ExpressRoute áramkör bárhol a világ bármely régió, hanem az csak az azonos kontinens területeire erőforrások eléréséhez.

- Nagyobb korlát, attól függően, hogy a sávszélesség a áramkör áramkör 10 dB / VNet hivatkozások száma növekedését.

Ideiglenes hálózati felszakadásáig felfelé kétszer a sávszélesség korlátozást, a külön költség nélkül beszerzett engedélyezése készült ExpressRoute áramkörök készült. A felesleges hivatkozások használatával érhető el. Azonban nem minden kapcsolat szolgáltatók támogatja a ezt a funkciót; Győződjön meg arról, hogy a kapcsolat szolgáltatója lehetővé teszi, hogy ez a funkció előtt attól függően, hogy azt.

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Beállíthatja magas elérhetőség az Azure-kapcsolat különböző módokon, attól függően, hogy használja a szolgáltató típusát, és készült ExpressRoute áramkörök és virtuális hálózati átjáró kapcsolatok konfigurálása hajlandó számát. A címzett pontok összefoglalva a elérhetőségi beállítások:

- Készült ExpressRoute nem támogatja az útválasztó redundancia protokollok, például magas elérhetősége HSRP és VRRP. Ilyenkor a felesleges két BGP a munkamenet peering használja. A hálózat könnyen hozzáférhető kapcsolatok megkönnyítésére Microsoft Azure kiépítése, és két felesleges portokat aktív-aktív konfigurációban két protokollt (a Microsoft edge része).

- Réteg 2 kapcsolat használata, telepítse a felesleges útválasztó aktív-aktív konfigurációban a helyszíni hálózaton. Csatlakozás az elsődleges áramkör egy útválasztó és a másodlagos áramkör, a másikkal. Meg fogalmat ad a kapcsolat mindkét oldalán könnyen hozzáférhető kapcsolatot. Ez a szükséges, ha szüksége van a készült ExpressRoute SLA. Lásd: [az Azure készült ExpressRoute SLA] [ sla-for-expressroute] további információt.

Az alábbi ábra mutatja a konfigurációban helyszíni felesleges útválasztó az elsődleges és másodlagos áramkörök csatlakozik. Minden áramkör kezeli a forgalmat a nyilvános peering és a személyes peering (minden peering kijelölt /30 két szóközt, cím, az előző szakaszban leírtak szerint).

![[1]][1]

Ha layer 3 kapcsolatot használ, győződjön meg arról, hogy tartalmaz-e felesleges BGP munkamenetek elérhetősége kezelő meg.

Virtuális hálózatok kapcsolhat össze több készült ExpressRoute áramkörök és is a különböző szolgáltató által biztosított az egyes áramkörök. Ez a beállítás további magas rendelkezésre állás és katasztrófa helyreállítási szolgáltatásokat nyújt.

Állítsa be a webhely VPN feladatátvevő elérési készült ExpressRoute. Ez csak a saját peering alkalmazandó. Az Azure és az Office 365-szolgáltatások az Internet nem csak feladatátvevő elérési.

Alapértelmezés szerint a BGP munkamenetek használja egy 60 másodperc üresjárati időtúllépés értékét. Miután a munkamenet időtúllépés 3 időpontok, az útvonal van megjelölve nem érhető el, és minden forgalom a rendszer átirányítja a hátralévő útválasztóhoz. Lehet, hogy az 180 másodperces időtúllépés túl hosszú a kritikus alkalmazásokat. Ebben az esetben módosíthatja a BGP időtúllépés beállításainak a helyszíni útválasztó egy kisebb értéket.

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

Használhatja az [Azure kapcsolódási eszközkészlet (AzureCT)] [ azurect] Lync-kapcsolat a helyszíni adatközponthoz és Azure között.

## <a name="security-considerations"></a>Biztonsági megfontolások

Biztonsági beállítások az Azure-kapcsolat különböző módokon, attól függően, hogy a biztonsági kérdések és a megfelelőség igényeinek konfigurálása. A címzett pontok összegzés a biztonsági beállítások.

Készült ExpressRoute layer 3 működik. Az alkalmazás rétegben veszélyek megelőzhető a hálózati forgalmat korlátozza a jogos erőforrások biztonsági készüléket használ. Ezenkívül nyilvános peering készült ExpressRoute hibák csak olyan indítható helyszíni. Megakadályozza, hogy a elérése és a helyszíni adatok nyilvános internetes betörjön engedélyezetlen szolgáltatás.

A biztonság fokozása, vegye fel a hálózati biztonsági készülékek a helyszíni hálózaton és a szolgáltató él útválasztó között. Ez segít a VNet jogosulatlan forgalmat a vízátfolyás korlátozása:

![[2]][2]

Naplózás és a megfelelőség céljából közvetlen hozzáférés tiltása a összetevők futtatása a VNet az internethez, és végrehajtja a [kényszerített tunneling]szükség lehet[forced-tuneling]. Ebben az esetben az internetes forgalmat irányítani futó helyszíni, ahol naplózható proxy belül hátra. A proxy beállítható úgy, hogy tiltani a nem engedélyezett forgalom halad meg, illetve szűrés a potenciálisan kártékony bejövő forgalmat.

![[3]][3]

Alapértelmezés szerint az Azure VMs hozzáférhetővé bejelentkezési kezelése céljából - RDP és a Windows VMs távoli Powershellt, és a SSH VMs Linux-alapú, amikor csoportházirenden keresztül az Azure-portálra a használt végpontokat elérhetővé tenni. A biztonság fokozása nem egy nyilvános IP-cím engedélyezése a VMs, és győződjön meg arról, hogy ezek VMs nem nyilvánosan hozzáférhető NSGs használatával. VMs csak akkor érhető el a belső IP-cím használatával. Ezeket a címeket is elérhetővé válik a készült ExpressRoute hálózaton keresztül, a helyszíni DevOps oktatói végrehajtásához szükséges konfigurációs vagy a karbantartás engedélyezése.

Ha egy külső hálózatot, elérhetővé tenni management végpontok az VMs, NSGs használata és/vagy a hozzáférés-vezérlési listák szeretné korlátozni a következő portokat olvashatóságát egy whitelist IP-címek vagy hálózatok.

## <a name="troubleshooting-considerations"></a>Hibaelhárítási kapcsolatos szempontok

Ha egy korábban működő készült ExpressRoute áramkör most nem szeretne csatlakozni, távollétében bármely konfigurációs módosításokat a helyszíni, vagy a személyes VNet belül szükség lehet a kapcsolat szolgáltatója kapcsolatba, és elérheti őket, a probléma javításához. A következő Azure PowerShell-parancsokkal segítségével néhány korlátozott ellenőrzése és megállapíthatja, hogy hol lehet elhelyezkednie a problémákat:

- Győződjön meg arról, hogy kiépítve a áramkör:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Ez a parancs megjeleníti a több tulajdonságait a áramkör, beleértve a `ProvisioningState`, `CircuitProvisioningState`, és `ServiceProviderProvisioningState` alább látható módon.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Ha a `ProvisioningState` értéke nem `Succeeded` próbált hozzon létre egy új áramkör, miután a kapcsolat eltávolítása a lenti parancs használatával, és próbáljon meg ismét létrehozni.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Ha a szolgáltató volna, már kiépítve a áramkör, és a `ProvisioningState` értéke `Failed`, vagy a `CircuitProvisioningState` nem `Enabled`, további segítségért forduljon a szolgáltatójához.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

Ha egy meglévő helyszíni infrastruktúra már be van állítva egy megfelelő hálózati készülék, ezeket a lépéseket követve telepítheti a hivatkozás architektúra:

Ha egy meglévő helyszíni infrastruktúra már be van állítva egy virtuális Magánhálózati készülék, ezeket a lépéseket követve telepítheti a hivatkozás architektúra:

1. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Megvárja, amíg az Azure-portálon nyissa meg a hivatkozásra, majd tegye a következőket: 
  - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-hybrid-er-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

3. Várja meg, a telepítés befejezéséhez.

4. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Megvárja, amíg az Azure-portálon nyissa meg a hivatkozásra, majd tegye a következőket: 
  - Jelölje ki a **meglévő használata** **erőforráscsoport** szakaszában, és adja meg `ra-hybrid-er-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

6. Várja meg, a telepítés befejezéséhez.

## <a name="next-steps"></a>Következő lépések

- Lásd: a [végrehajtási egy könnyen hozzáférhető hibrid hálózat architektúrája] [ highly-available-network-architecture] információt a hibrid hálózat elérhetőségének növelésével alapján készült ExpressRoute hibás fölé a virtuális Magánhálózati kapcsolatot.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hibrid hálózat architektúrája, használja a készült Azure ExpressRoute"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Felesleges útválasztó használata elsődleges és másodlagos áramkörök készült ExpressRoute"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Biztonsági eszközök hozzáadása a helyszíni hálózaton"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Internetes kötött forgalom naplózandó tunneling használatával kényszerített"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "A ServiceKey egy készült ExpressRoute áramkör megkeresése"
