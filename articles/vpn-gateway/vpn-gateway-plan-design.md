<properties 
   pageTitle="Virtuális Magánhálózati átjárók tervezése és kialakítása |} Microsoft Azure"
   description="Tudnivalók a virtuális Magánhálózati átjárók tervezése és kialakítása az idegen helyszíni, hibrid és VNet-VNet kapcsolatok"
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
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Tervezése és kialakítása VPN átjáró

Tervezését és kialakítását az idegen helyszíni és VNet-VNet konfigurációk lehet egyszerű vagy összetett, hálózati igényeitől függően. Ez a cikk részletes információt tartalmaz egyszerű tervezése és kialakítása szempontról.

## <a name="planning"></a>Megtervezése


### <a name="compare"></a>Idegen helyszíni csatlakozási beállítások

Ha szeretné a helyszíni webhelyek biztonságos kapcsolódás virtuális hálózatot, ezt háromféleképpen van:-webhelyek, webhely pont- és készült ExpressRoute. Összehasonlíthatja a rendelkezésre álló különböző határokon helyszíni kapcsolatokkal. A lehetőséget választja is függnek különböző szempontok, például:


- Milyen típusú átviteli a megoldás kell-e?
- Szeretne kommunikálni az nyilvános interneten keresztül biztonságos VPN vagy magánjellegű kapcsolaton keresztül?
- Van egy nyilvános IP-cím használható?
- Tervezi VPN-eszköz használata? Ha igen, az kompatibilis?
- Néhány számítógépek csatlakozik, vagy szeretné az állandó kapcsolatot a webhelyen?
- Milyen típusú virtuális Magánhálózati átjáró szükség a megoldáshoz szeretne létrehozni?
- Melyik átjáró Termékváltozat kell használni?


Az alábbi táblázat segítségével könnyebben eldöntheti, a legjobb kapcsolódási lehetőség a megoldás.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Átjáró követelmények VPN-típus és Termékváltozat

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Átjáró termékváltozatok kapcsolatos további tudnivalókért olvassa el a [VPN átjáró beállításai](vpn-gateway-about-vpn-gateway-settings.md#gwsku)című témakört.

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Összesítő átviteli Termékváltozat és VPN-típus szerint

A következő táblázat mutatja az átjáró-típusok és a becsült összesített kapacitása. A becsült összesítő átviteli a tervezésekor döntési tényező lehet.
Árak átjáró termékváltozatok eltérnek. Tudni árak olvassa el a [Virtuális Magánhálózati átjáró árak](https://azure.microsoft.com/pricing/details/vpn-gateway/)című témakört. Az alábbi táblázat az erőforrás-kezelő és a klasszikus telepítési modellek vonatkozik.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Írja be a Termékváltozat és a virtuális Magánhálózati által támogatott konfigurációk

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Munkafolyamat

Az alábbi lista ismertet a közös munkafolyamat felhőbeli kapcsolat:

1.  Tervezése és a kapcsolati topológia megtervezése, és a lista összes hálózat, amelyhez csatlakozni a cím szóközöket.
2.  Hozzon létre egy Azure virtuális hálózat. 
3.  A virtuális hálózat VPN átjáró létrehozása.
4.  Létrehozása és konfigurálása a kapcsolatokat a helyszíni hálózatok vagy más virtuális hálózatok (szükség szerint).
5.  Létrehozása és konfigurálása az Azure virtuális Magánhálózati átjáró (szükség szerint) pont a webhely-kapcsolatot.
 

## <a name="design"></a>Tervező

### <a name="topologies"></a>Kapcsolat topológiák

Indítsa el a [Virtuális Magánhálózati átjáró](vpn-gateway-about-vpngateways.md) című témakörben diagramok megjeleníti. A cikkben alapvető diagramok, a telepítési modellek az egyes topológia (erőforrás-kezelő vagy klasszikus), és milyen telepítési eszközök segítségével segítségével telepítheti a konfigurációban.   

### <a name="designbasics"></a>Tervezésének alapjai

Az alábbi szakaszokból megtudhatja, hogy a virtuális Magánhálózati átjáró alapjai. [Hálózati szolgáltatások korlátozások](../articles/azure-subscription-service-limits.md#networking-limits)célszerű is.


#### <a name="subnets"></a>Tudnivalók a alhálózat

Amikor kapcsolatokat hoz létre, figyelembe kell vennie alhálózat tartományt. Egymást átfedő alhálózat címtartományokat folytathat. Az egymást átfedő alhálózat esetén, hogy egy virtuális hálózati vagy a helyszíni helyét tartalmazza az azonos címterület, amely tartalmazza a másik helyre. Ez azt jelenti, hogy szüksége van-e a hálózati szakemberek a helyi helyszíni hálózatok carve ki egy tartományt szeretne használni az Azure IP-terület/alhálózat megcímezheti. A helyi a helyszíni hálózaton nem használt címterület használatára van szüksége. 

Egymást átfedő alhálózat elkerülése esetén is fontos VNet-VNet kapcsolatok használata. Az alhálózathoz átfedik egymást, és IP-címe megtalálható a küldő és céllistában VNets, ha sikertelen VNet-VNet kapcsolatok. Azure nem átirányítja az adatokat a többi VNet, mivel a címzett címét a küldő VNet része. 

Virtuális Magánhálózati átjárók átjáró alhálózat nevű adott alhálózat szükséges. Az összes átjárót alhálózat névvel kell GatewaySubnet működik megfelelően. Ne felejtse el nem név átjáró alhálózathoz egy másik nevet, és az átjáró alhálózathoz nem VMs vagy bármely más telepítése. Lásd: az [átjáró alhálózat](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Tudnivalók a helyi hálózaton átjárók

A helyi hálózati átjáró a helyszíni hely általában hivatkozik. A klasszikus telepítési modell a helyi hálózati átjáró nevezik a helyi hálózati webhelyen. A helyi hálózati átjáró konfigurálásakor, adja meg egy nevet, adja meg a nyilvános IP-cím a helyszíni VPN eszköz, és adja meg a cím prefixumokban, amelyek a helyszíni helyen. Azure vizsgálja meg a célként megadott cím prefixumokban hálózati forgalmának beállítása, a beállításokat, amelyek a megadott olvas, a helyi hálózaton átjáró és csomagok megfelelően irányítja. A cím prefixumokban igény szerint módosíthatja. További tudnivalókért olvassa el a [helyi hálózaton átjárók](vpn-gateway-about-vpn-gateway-settings.md#lng)című témakört.


#### <a name="gwtype"></a>Átjáró típusával kapcsolatban

Jelölje ki a megfelelő átjáró típusát a topológia fontos. Ha bejelöli a megfelelő típusú, az átjáró nem megfelelően működik. Az átjáró típusa határozza meg, hogyan csatlakozik az átjáró magát, és egy szükséges konfigurációs beállítás, az erőforrás-kezelő telepítési modell.

Az átjáró típusok a következők:

- Virtuális magánhálózati
- Készült ExpressRoute

#### <a name="connectiontype"></a>Tudnivalók a kapcsolatok típusai

Minden konfiguráció szükséges egy adott kapcsolat típusát. A kapcsolatok típusai a következők:

- IPsec
- Vnet2Vnet
- Készült ExpressRoute
- VPNClient


#### <a name="vpntype"></a>Tudnivalók a virtuális Magánhálózati típusai

Minden egyes konfiguráció szükséges a megadott VPN típusú. Ha a rendszer együttes alkalmazása a két beállításokat, például a webhely kapcsolat és az azonos VNet pont a webhely-kapcsolat létrehozása VPN típusú, amely megfelel a két kapcsolat kell használnia.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

Az alábbi táblázatokban megjelenítése a VPN-típus, mint azt a minden kapcsolat beállítása Győződjön meg arról, hogy az átjáró a virtuális Magánhálózati típusának megfelelő a létrehozni kívánt konfiguráció. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>A hely közötti kapcsolatok virtuális Magánhálózati eszközök

Állítsa be a hely közötti kapcsolat telepítési modell, függetlenül van szükség az alábbiakat:

- Azure virtuális Magánhálózati átjárók kompatibilis VPN eszközt
- A nyilvános elérésű IPv4 IP-címet, amely nem hálózati Címfordítást mögött

Kell a virtuális Magánhálózati eszköz beállítása élmény rendelkeznek, illetve mások, hogy az eszköz adhatja meg. További információt a virtuális Magánhálózati eszközök [kapcsolatos VPN eszköz](vpn-gateway-about-vpn-devices.md)megjelenítéséhez. A virtuális Magánhálózati eszközök cikk ellenőrzött eszközök, nem érvényesíteni eszközök szoftverkövetelményei és mutató hivatkozások eszköz konfigurációs Ha elérhető információkat tartalmaz.

### <a name="forcedtunnel"></a>Fontolja meg a kényszerített alagutas továbbítása

A legtöbb konfigurációk esetén is beállíthatja a kényszerített tunneling. Kényszerített tunneling meghatározhatja, átirányítás vagy a "kötelező" összes internetes kötött forgalom biztonsági másolatot a helyszíni helyre a webhely VPN alagutas vizsgálati és naplózási keresztül. Ez a legtöbb nagyvállalati informatikai kritikus biztonsági követelmény házirendeket. 

Kényszerített tunneling nélkül Internet kötött-forgalmat a VMs Azure-ban a fog mindig bejárása az Azure hálózati infrastruktúrát közvetlenül ki az internethez, a vezérlőt, amellyel lehetővé teszi, hogy vizsgálja meg, vagy a forgalmat a naplózandó nélkül. Ezzel az illetéktelen Internet-hozzáférés esetleg vezethet illetéktelen vagy más típusú biztonsági szabályok megsértésével.

**Kényszerített közötti diagram**

![Tunneling kényszerített kapcsolat] (./media/vpn-gateway-plan-design/forced-tunnel.png "kényszerített tunneling")

Kényszerített közötti kapcsolat mindkét telepítési modellekben és a különböző eszközök segítségével konfigurálhatja. További információt az alábbi táblázatban látható. Új cikkek, új telepítési modellek és további eszközök ebben a konfigurációban elérhetővé válnak, frissítjük a táblázatban. Egy cikk érhető el, ha azt csatolása közvetlenül az értékeket.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Következő lépések

Cikkek a [Virtuális Magánhálózati átjáró kapcsolatos Gyakori](vpn-gateway-vpn-faq.md) és a [Virtuális Magánhálózati átjáró](vpn-gateway-about-vpngateways.md) segítséget nyújt a tervezés további információt.

Adott átjáró beállításokkal kapcsolatos további tudnivalókért tekintse meg [A virtuális Magánhálózati átjáró beállításait](vpn-gateway-about-vpn-gateway-settings.md).




