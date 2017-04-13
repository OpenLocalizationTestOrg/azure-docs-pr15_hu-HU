<properties
   pageTitle="A helyszíni hálózaton csatlakoztatása az Azure |} Microsoft Azure"
   description="Ebből a cikkből megtudhatja, és összehasonlítja történő csatlakozás Microsoft cloud services például Azure, Office 365-ben és a Dynamics CRM Online rendelkezésre álló különféle módon."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>A helyszíni hálózaton csatlakoztatása az Azure

A Microsoft cloud services különböző típusú biztosít. A szolgáltatások nyilvános interneten keresztül csatlakozhat, miközben még csatlakozhat egyes szolgáltatások használata egy virtuális magánhálózaton (VPN) alagutas, az interneten keresztül, vagy egy közvetlen, személyes Microsoft-kapcsolaton keresztül. Ez a cikk segít annak eldöntésében, hogy melyik kapcsolódási lehetőség a lesz, felhasználása Microsoft felhőszolgáltatásokhoz típusú alapján igényeknek leginkább megfelelő. A legtöbb szervezet leírása alább többféle kapcsolatot használja.

## <a name="connecting-over-the-public-internet"></a>Csatlakozás a nyilvános interneten keresztül

A kapcsolat típusa Microsoft cloud services hozzáférést biztosít a közvetlenül az interneten keresztül, alább látható módon.

![Internetes] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internetes")

A kapcsolat általában a Microsoft-felhőszolgáltatások való csatlakozáshoz használt első típus. Az alábbi táblázat felsorolja a előnyei és hátrányai kapcsolat elemtípushoz.



| **Legfontosabb előny**| **Megfontolandó szempontok**|
|---------|---------|
|A helyszíni hálózaton módosítás nem igényel, mindaddig, amíg az összes ügyféleszközök van korlátlan hozzáférés az összes IP-címek és portok az interneten.|Bár forgalom gyakran titkosítva van a HTTPS, azt is titkosítatlan a hálózaton átvitt óta bejárja a nyilvános internetkapcsolat.|
|Nyilvános internetkapcsolat elérhetővé tett minden Microsoft felhőszolgáltatásokhoz csatlakozhat.|Előre késés, mert a kapcsolat rajta áthaladó az internethez.|
|A meglévő Internet kapcsolatot használ.||
|Minden kapcsolat eszközök kezelésének használatához nincs szükség.||

A kapcsolat nem rendelkezik kapcsolódási vagy sávszélesség-költségek, mivel a meglévő internetkapcsolat használatára. 

## <a name="connecting-with-a-point-to-site-connection"></a>Webhely-pont kapcsolat összekapcsolása

A kapcsolat típusa hozzáférést biztosít az egyes Microsoft felhőszolgáltatásokhoz keresztül egy biztonságos szoftvercsatorna Tunneling Protocol (SSTP) alagutas az interneten alább látható módon.

![P2S] (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "Pont webhely kapcsolat")

A kapcsolat történik át a meglévő internetkapcsolat, de az Azure virtuális Magánhálózati átjáró használatát igényli. Az alábbi táblázat felsorolja a előnyei és hátrányai a kapcsolat típusú.

| **Legfontosabb előny**| **Megfontolandó szempontok**|
|---------|---------|
|A helyszíni hálózaton módosítás nem igényel, mindaddig, amíg az összes ügyféleszközök van korlátlan hozzáférés az összes címek és IP-portok az interneten.|Bár a forgalmat a IPSec titkosítva, azt is titkosítatlan a hálózaton átvitt óta bejárja a nyilvános internetkapcsolat.|
|A meglévő Internet kapcsolatot használ.|Előre késés, mert a kapcsolat rajta áthaladó az internethez.|
|Legfeljebb 200 átjáró per Mb/s kapacitása.|Szükséges létrehozásának és kezelésének külön kapcsolatok a helyszíni hálózaton egyes eszközök és minden egyes csatlakoznia kell átjáró között.|
|Használható az Azure virtuális hálózatok (VNet) kapcsolhat össze Azure szolgáltatások összekapcsolása például Azure virtuális gépeken futó és Azure Cloud Services.|Szükséges minimális mindennapi felügyeleti az Azure virtuális Magánhálózati átjáró.|
||Kapcsolódás a Microsoft Office 365-ben vagy a Dynamics CRM Online nem használhatók.
||Kapcsolódni, nem kell csatlakoztatni egy VNet Azure szolgáltatások nem használhatók.|

További tudnivalók a [Virtuális Magánhálózati átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md) szolgáltatás, a [árak](https://azure.microsoft.com/pricing/details/vpn-gateway)és kimenő adatok átadás [árak](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Webhely kapcsolat összekapcsolása

A kapcsolat típusa hozzáférést biztosít az egyes Microsoft felhőszolgáltatásokhoz keresztül egy IPSec alagutas az interneten alább látható módon.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Hely közötti kapcsolat")

A kapcsolat történik át a meglévő internetkapcsolat, de az Azure virtuális Magánhálózati átjáró társított árak és árak kimenő adatátvitel használatát igényli. Az alábbi táblázat felsorolja a előnyei és hátrányai kapcsolat elemtípushoz.

| **Legfontosabb előny**| **Megfontolandó szempontok**|
|---------|---------|
|A helyszíni hálózaton minden eszköz csatlakozik egy VNet, ezért nincs szükség külön az egyes kapcsolatainak konfigurálása Azure szolgáltatások kommunikálhat.|Bár a forgalmat a IPSec titkosítva, azt is titkosítatlan a hálózaton átvitt óta bejárja a nyilvános internetkapcsolat.|
|A meglévő Internet kapcsolatot használ.|Előre késés, mert a kapcsolat rajta áthaladó az internethez.|
|Azure virtuális gépeken futó például egy VNet kapcsolhat össze szolgáltatásokat és a Cloud Services csatlakozni használható.|Kell beállítása és kezelése egy érvényesített VPN eszköz * helyszíni.|
|Legfeljebb 200 átjáró per Mb/s kapacitása.|Az Azure virtuális Magánhálózati átjáró minimális mindennapi felügyeleti szükséges.|
|Az felhő virtuális gépeken futó ellenőrzés és a felhasználó által definiált útvonalak vagy a szegély átjáró Protocol (BGP) naplózás a helyszíni hálózaton keresztül által kezdeményezett kimenő forgalmának kényszerítheti **.|Kapcsolódás a Microsoft Office 365-ben vagy a Dynamics CRM Online nem használhatók.|
||Kapcsolódni, nem kell csatlakoztatni egy VNet Azure szolgáltatások nem használhatók.|
||Vissza a helyszíni eszközök kapcsolatok kezdeményezzen szolgáltatásokat használja, és a biztonsági házirendek csak akkor, ha előfordulhat, hogy a helyszíni hálózaton és Azure között tűzfalat.|

- * A [virtuális Magánhálózati eszközök érvényesített](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices)listájának megtekintése.
- ** Használatáról további információt [felhasználói útvonalak](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) vagy [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) kényszerítése továbbítás az Azure VNets egy helyszíni eszközön.

## <a name="connecting-with-a-dedicated-private-connection"></a>Saját személyes kapcsolat összekapcsolása

A kapcsolat típusa hozzáférést biztosít az összes Microsoft felhőszolgáltatásokhoz saját személyes kapcsolaton keresztül, amely nem az interneten, Microsoft alább látható módon.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "Kapcsolat készült ExpressRoute")

A kapcsolat a készült ExpressRoute szolgáltatás, és a kapcsolat szolgáltatója kapcsolat használatát igényli. Az alábbi táblázat felsorolja a előnyei és hátrányai kapcsolat elemtípushoz.

| **Legfontosabb előny**| **Megfontolandó szempontok**|
|---------|---------|
|Adatforgalom nem lehet kezekbe a hálózaton átvitt nyilvános internetkapcsolat óta egy szolgáltatón keresztül egy dedikált kapcsolatot használja.|A helyszíni útválasztó management szükséges.|
|Legfeljebb 10 Gb/s / készült ExpressRoute áramkör és legfeljebb 2 Gb/s az egyes átjáró átviteli sávszélesség.|A kapcsolat szolgáltatója dedikált kapcsolatot igényel.|
|Kiszámíthatóan késés mert dedikált Microsoft vállalattól, amely nem járja be az internetes kapcsolat.|Megkövetelheti egy vagy több Azure virtuális Magánhálózati átjárók minimális mindennapi felügyeleti (Ha a áramkör kapcsolódás VNets).|
|Titkosíthatja a forgalmat, ha azt szeretné, hogy nincs szüksége titkosított kapcsolati.| Vissza a helyszíni eszközök kapcsolatok kezdeményezzen felhőalapú szolgáltatások használata, előfordulhat, hogy tűzfalat a helyszíni hálózaton és Azure között.|
|Közvetlenül csatlakozhat az összes Microsoft cloud-szolgáltatások, néhány kivétel *.|Hálózati cím címfordító kell a helyszíni IP-címet ad meg, hogy nem kell csatlakoztatni egy VNet.* * szolgáltatások a Microsoft adatközpontokban|
|Kényszerítheti a felhőben virtuális gépeken futó vizsgálati és naplózás BGP használata a helyszíni hálózaton keresztül által kezdeményezett kimenő forgalmának engedélyezésére.|

- * Készült ExpressRoute együtt nem használható [szolgáltatások listájának](../expressroute/expressroute-faqs.md#supported-services) megtekintése. Azure-előfizetése engedélyeznie kell az Office 365-ben való kapcsolódáshoz.  [Az Office 365-höz készült Azure-ExpressRoute](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) témakörben további információt.
- ** Többet megtudhat készült ExpressRoute [hálózati Címfordítást](../expressroute/expressroute-nat.md) követelményeknek.

További tudnivalók a [készült ExpressRoute](../expressroute/expressroute-introduction.md), a társított [árak](https://azure.microsoft.com/pricing/details/expressroute), és [csatlakozási szolgáltatók](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>További megfontolandó szempontok

- A fenti lehetőségek számos van VNet kapcsolatok, a kapcsolatok átjáró és az egyéb feltétel szerint támogatása is különböző maximális számával. Olvassa el az Azure [korlátozza a hálózat](../azure-subscription-service-limits.md#networking-limits) megérthető, ha bármelyiküket hatással lehet a kapcsolat típusú úgy dönt, hogy használata ajánlott. 
- Ha a webhely virtuális Magánhálózati kapcsolat az átjárók csatlakozhat az azonos VNet, mint egy készült ExpressRoute átjáró, akkor célszerű megismerkednie fontos kényszerek először. További részletekért [konfigurálása készült ExpressRoute és a webhely coexisting kapcsolatok](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) témakörben olvasható.

## <a name="next-steps"></a>Következő lépések

Az alábbi forrásokban bemutatják, hogyan tudnak megvalósítani a kapcsolatok típusai, a jelen cikkben szereplő.

-   [Webhely-pont kapcsolat megvalósítása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Webhely kapcsolat megvalósítása](guidance-hybrid-network-vpn.md)
-   [Saját személyes kapcsolat megvalósítása](guidance-hybrid-network-expressroute.md)
-   [Az elérhetőség magas hely közötti kapcsolatot dedikált személyes kapcsolat megvalósítása](guidance-hybrid-network-expressroute-vpn-failover.md)
