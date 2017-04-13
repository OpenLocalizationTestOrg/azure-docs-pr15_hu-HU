<properties 
   pageTitle="Tudnivalók a virtuális Magánhálózati átjáró |} Microsoft Azure"
   description="Tudjon meg többet az Azure virtuális hálózatok virtuális Magánhálózati átjáró kapcsolatok."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Tudnivalók a virtuális Magánhálózati átjáró


A virtuális hálózati átjáró használják a hálózati forgalmat Azure virtuális hálózatok és a helyszíni helyek közötti és belül (VNet VNet) Azure virtuális hálózatok között. Ha konfigurálnia VPN az átjárók, létrehozása és konfigurálnia kell a virtuális hálózati átjáró és az átjáró virtuális hálózati kapcsolaton.

Az erőforrás-kezelő telepítési modell virtuális hálózati átjárót, az erőforrások létrehozásakor, adja meg, számos beállítást. A szükséges beállításokat egyik '-GatewayType'. Két virtuális hálózati átjáró típusba sorolhatók: Vpn és készült ExpressRoute. 

Hálózati forgalmának engedélyezésére saját személyes kapcsolat küldésekor az átjáró típus "Készült ExpressRoute" használata. Ez akkor is nevezik egy készült ExpressRoute átjáró. Hálózati forgalmának engedélyezésére nyilvános kapcsolaton keresztül küldött titkosított, az átjáró típus "Vpn" használata. Ez egy virtuális Magánhálózati átjárót nevezik. Webhely webhely, webhely pont- és VNet-VNet kapcsolatok összes használja egy virtuális Magánhálózati átjárót.

Minden egyes virtuális hálózati beállíthatja, hogy csak egy virtuális hálózati átjáró átjáró típusonként. Ha például lehet használó - GatewayType készült ExpressRoute átjáró egy virtuális hálózati és - GatewayType Vpn használó. Ez a cikk elsősorban a virtuális Magánhálózati átjáró koncentrál. Készült ExpressRoute kapcsolatos további tudnivalókért lásd: a [Készült ExpressRoute technikai áttekintés](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Árak

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Átjáró SKU

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Átjáró termékváltozatok kapcsolatos további tudnivalókért olvassa el a [Átjáró termékváltozatok](vpn-gateway-about-vpn-gateway-settings.md#gwsku)című témakört.

### <a name="estimated-aggregate-throughput-by-sku"></a>Becsült összesítő átviteli Termékváltozat szerint

A következő táblázat mutatja az átjáró-típusok és a becsült összesített kapacitása. Az alábbi táblázat az erőforrás-kezelő és a klasszikus telepítési modellek vonatkozik.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>A virtuális Magánhálózati átjáró beállítása

Konfigurálnia VPN az átjárók, amikor a képernyőn megjelenő utasításokat, használja a telepítési modell hozhat létre virtuális hálózatához használt függ. Ha például a VNet a klasszikus telepítési felhasználva hozta létre, ha használatával a útmutatások és az utasításokat a klasszikus telepítési modell létrehozása és konfigurálása a virtuális Magánhálózati átjáró beállításainak. Telepítési modellek kapcsolatos további tudnivalókért lásd: a [ismertetése erőforrás-kezelő és klasszikus telepítési modellek](../resource-manager-deployment-model.md).

A virtuális Magánhálózati átjáró kapcsolat egyedi beállításokkal rendelkező beállított több erőforrások támaszkodik. Az erőforrások többsége külön-külön beállítható, bár bizonyos esetekben meghatározott sorrendben kell konfigurálni. Ki hozhat létre és erőforrások eszközzel egy konfigurációja, például az Azure portál beállítása elindíthatja. Akkor is később úgy dönt, váltani szeretne egy másik eszközt, például a PowerShell, a további erőforrások konfigurálása, vagy módosíthatja a meglévő erőforrások, ha az felel meg Önnek. Jelenleg minden erőforrás- és erőforrás-beállítás nem tudja beállítani az Azure-portálon. Az egyes internetkapcsolat topológiájának a cikkek útmutatását adja meg, ha egy adott konfigurációs eszközt van szükség. Az egyes erőforrások és a virtuális Magánhálózati átjáró beállításainak információt tekintse meg [virtuális Magánhálózati átjáró beállításait](vpn-gateway-about-vpn-gateway-settings.md).

Az alábbi szakaszok a táblákat, a lista tartalmazza:

- rendelkezésre álló telepítési modell
- rendelkezésre álló konfigurációs eszközök
- hivatkozásokra közvetlenül az egy cikk, ha elérhető

Használatával a diagramok és a leírásokat jelölje ki a internetkapcsolat topológiájának az igényeknek megfelelően alakíthatja. A diagramok jelenítik meg az eredeti fő topológiák, de lehetséges útmutatást a diagramok használatával összetettebb konfigurációk össze.

## <a name="site-to-site-and-multi-site"></a>Webhely és több webhelyen

### <a name="site-to-site"></a>Webhely-webhely

A webhely (S2S) virtuális Magánhálózati átjáró kapcsolat kapcsolat IPsec/IKE (IKEv1 vagy IKEv2) VPN-csatorna fölé. Ilyen típusú kapcsolat megköveteli a VPN-eszközhöz a helyszíni, amely tartalmaz egy nyilvános IP-cím rendelt található, és nem található mögött egy hálózati címfordítást. S2S kapcsolatok használható határokon helyszíni és hibrid konfigurációk.   

![S2S kapcsolat] (./media/vpn-gateway-about-vpngateways/demos2s.png "webhely-webhely")


### <a name="multi-site"></a>Több webhelyen

Hozzon létre, és állítsa be az átjáró virtuális Magánhálózati kapcsolatot a VNet és több helyszíni hálózatok között. Ha több kapcsolat használata esetén a RouteBased VPN típusa (klasszikus VNets a dinamikus átjáró) kell használnia. Egy VNet csak is van egy virtuális Magánhálózati átjáró, mert az átjáró keresztül minden kapcsolat ossza meg az elérhető sávszélesség. Ez gyakran kapcsolat "több webhelyen" nevezik.
 

![Több elem webhely kapcsolat] (./media/vpn-gateway-about-vpngateways/demomulti.png "több webhelyen")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Telepítési modellek és a webhely és több webhelyen módszerei

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet-VNet

Virtuális hálózat egy másik virtuális hálózathoz csatlakozik (VNet VNet) hasonlít egy VNet csatlakozik egy helyszíni hely. Mindkét kapcsolat típusát a virtuális Magánhálózati átjáró segítségével adja meg a biztonságos alagutas szolgáltatással IPsec /. Még több webhelyen kapcsolat konfigurációk VNet-VNet kommunikációt is összevonhatja. Ezzel a radírral hálózati topológiák közötti virtuális a hálózati kapcsolat határokon helyszíni kapcsolatokkal kombináló létrehozni.

A csatlakozás VNets lehet:

- az egyforma vagy különböző régiókban
- az egyforma vagy különböző előfizetések a 
- az azonos különböző telepítési modellekben


![VNet VNet kapcsolat] (./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-vnet")

#### <a name="connections-between-deployment-models"></a>Telepítési modelljei közötti kapcsolatok

Azure által jelzett a két környezetben modellek: klasszikus és erőforrás-kezelő. Ha, használták Azure egy kis időt, akkor valószínűleg Azure VMs és a klasszikus VNet futó példányát szerepkörök. A újabb VMs és szerepkör-példányok egy erőforrás-kezelő létrehozott VNet kell futnia. Az erőforrások közvetlenül egy másik erőforrások kommunikálni egy VNet engedélyezni a VNets közötti kapcsolatot hozhat létre.

#### <a name="vnet-peering"></a>VNet peering

Előfordulhat, hogy a kapcsolat létrehozásához peering mindaddig, amíg a virtuális hálózat megfelel bizonyos VNet használhatja. VNet peering nem használja a virtuális hálózati átjáró. További tudnivalókért olvassa el a [VNet peering](../virtual-network/virtual-network-peering-overview.md)című témakört.


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Telepítési modellek és VNet-VNet módszerei

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Pont-webhely

Átjáró pont webhely (P2S) virtuális Magánhálózati kapcsolat lehetővé teszi, hogy az egyes ügyfélszámítógép virtuális hálózatához biztonságos kapcsolatot hozzon létre. P2S a virtuális Magánhálózati kapcsolat SSTP (biztonságos szoftvercsatorna protokoll) keresztül. P2S kapcsolatok nem szükséges VPN-eszközhöz vagy egy nyilvános IP-címet a munkát. A virtuális Magánhálózati kapcsolatot az ügyfélszámítógép indításával. A megoldás akkor hasznos, amelyhez csatlakozni a VNet távoli helyről, például a kezdőlap vagy a konferenciához, amikor, vagy ha csak néhány ügyfélprogramokat sorolja fel kell csatlakoznia egy VNet. P2S kapcsolatok S2S kapcsolatok VPN átjárón keresztül együtt használható, feltéve, hogy mindkét kapcsolatok konfigurációs követelményei összes kompatibilis.


![Pont webhely kapcsolat] (./media/vpn-gateway-about-vpngateways/demop2s.png "pont-webhely")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Telepítési modellek és a webhely-pont módszerei

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>Készült ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

A készült ExpressRoute internetkapcsolat esetén a virtuális hálózati átjáró "Vpn" helyett "készült" ExpressRoute átjáró típusú van beállítva. Készült ExpressRoute kapcsolatos további tudnivalókért lásd: a [készült ExpressRoute technikai áttekintés](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Webhely és készült ExpressRoute coexisting kapcsolatok

Készült ExpressRoute egy közvetlen, célorientált kapcsolatot a WAN (nem az interneten nyilvános) és a Microsoft Services, beleértve az Azure. Webhely VPN forgalmat a nyilvános interneten titkosított utazás. Az azonos virtuális hálózati hely közötti VPN és készült ExpressRoute kapcsolatainak konfigurálása való számos előnye van.

Biztonságos feladatátvevő elérési beállítása webhely VPN készült ExpressRoute, vagy csatlakozhat webhely VPN adatai, amelyek nem részei a hálózat, de készült ExpressRoute keresztül kapcsolódik, amely a webhelyek. A közlemény virtuális hálózatához, az ehhez a két virtuális hálózati átjáró - GatewayType Vpn és a többi - GatewayType készült ExpressRoute használata egy használatával.


![Coexist kapcsolat] (./media/vpn-gateway-about-vpngateways/demoer.png "készült expressroute-site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Telepítési modellek és és módszerei S2S készült ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Következő lépések

A virtuális Magánhálózati átjáró konfiguráció megtervezése. Lásd: a [virtuális Magánhálózati átjárók tervezése és kialakítása](vpn-gateway-plan-design.md) és [Csatlakozás a helyszíni hálózaton Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
