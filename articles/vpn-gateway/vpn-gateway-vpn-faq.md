<properties 
   pageTitle="Virtuális hálózati VPN átjáró gyakran ismételt kérdések |} Microsoft Azure"
   description="A virtuális Magánhálózati átjáró – gyakori kérdések. Gyakori kérdések a Microsoft Azure virtuális hálózati határokon helyszíni kapcsolatok, a hibrid konfigurációs kapcsolatok és a virtuális Magánhálózati átjárók"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>Virtuális Magánhálózati átjáró – gyakori kérdések

## <a name="connecting-to-virtual-networks"></a>Kapcsolódás virtuális hálózatok

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Csatlakoztathatja virtuális hálózatok különböző Azure régiókban?
igen. Erre valójában nincs régió korlátozás van. Egy virtuális hálózati csatlakoztathatja virtuális egy másik hálózati ugyanabban a régióban vagy más Azure területére.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Csatlakoztathatja virtuális hálózatok a különböző előfizetések?
igen.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Csatlakozhatok több webhelyen virtuális hálózatról?

Több webhely csatlakozhat az Azure REST API-k és a Windows PowerShell használatával. Lásd: a [több webhely és a VNet-VNet kapcsolódási](#multi-site-and-vnet-to-vnet-connectivity) – gyakori kérdések szakaszt.
## <a name="what-are-my-cross-premises-connection-options"></a>Mik azok a határokon helyszíni csatlakozási beállítások?

Az alábbi cross helyszíni kapcsolatok támogatottak:

- [Webhely](vpn-gateway-site-to-site-create.md) – virtuális Magánhálózati kapcsolat IPSec (IKE v1 és IKE v2). A kapcsolat típusát a virtuális Magánhálózati eszköz vagy az Útválasztás és szükséges.

- [Pont webhely](vpn-gateway-point-to-site-create.md) – VPN-kapcsolaton keresztül SSTP (biztonságos szoftvercsatorna protokoll). A kapcsolat egy virtuális Magánhálózati eszköz nincs szükség.

- [VNet-VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – ilyen típusú kapcsolat megegyezik a webhely konfiguráció. A VNet VNet a virtuális Magánhálózati kapcsolat, IPSec (IKE v1 és IKE v2). Nem igényel egy virtuális Magánhálózati eszközt.

- [Több webhelyen](vpn-gateway-multi-site.md) – Ez a változata, a webhely konfiguráció, amely lehetővé teszi, hogy több a helyszíni webhely egy virtuális hálózathoz csatlakozik.

- [Készült ExpressRoute](../expressroute/expressroute-introduction.md) – készült ExpressRoute az Azure közvetlen kapcsolatot a WAN, nem a nyilvános interneten keresztül. Lásd: a [Készült ExpressRoute technikai áttekintés](../expressroute/expressroute-introduction.md) és további információt a [Készült ExpressRoute – gyakori kérdések](../expressroute/expressroute-faqs.md) .

Kapcsolatok kapcsolatos további tudnivalókért olvassa el a [Virtuális Magánhálózati átjáró](vpn-gateway-about-vpngateways.md)című témakört.

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Mi a különbség a webhely kapcsolatot, és a pont-webhely között?

**Hely közötti** kapcsolatok adhatja meg a virtuális gép vagy szerepkör-példányt, állítsa be a továbbítás módjától függően a virtuális hálózaton belül a helyszíni található számítógépeken közötti kapcsolat. Mindig elérhető határokon helyszíni kapcsolat nagyszerű lehetőséget, és jól illeszkedik hibrid konfigurációk. A kapcsolat típusú IPsec VPN készülék (hardvereszközt vagy finom készülék), amely a hálózat szélén kell telepíthető támaszkodik. Az ilyen típusú kapcsolat létrehozásához rendelkeznie kell a szükséges VPN hardver- és egy külső felekkel szemben IPv4-címet.

**Pont webhely** kapcsolatok látványosan bárhonnan egyetlen számítógépről történő semmit, amely a virtuális hálózata. A Windows beépített VPN-ügyfelet használ. A webhely-pont konfiguráció részeként telepítse a tanúsítvány és a virtuális Magánhálózati konfigurációs ügyfélcsomag, amely tartalmazza a beállításokat, amelyek lehetővé teszik a számítógép bármely virtuális gép vagy szerepkör-példány a virtuális hálózaton belül. Azt kiválóan, ha szeretne egy virtuális hálózathoz csatlakozik, de nem található a helyszíni. Érdemes emellett jó lehetőséget, ha nincs hozzáférése a virtuális Magánhálózati hardver vagy külső felekkel szemben IPv4-cím, hely közötti kapcsolat szükségesek, amelynek mindkét. 

Beállíthatja a webhely és a webhely-pont egyidejűleg, segítségével virtuális hálózat, feltéve, hogy a webhely kapcsolatot egy útvonal-alapú VPN-típus használatával az átjáró hoz létre. Dinamikus átjárók a klasszikus telepítési modell útvonal-alapú VPN-típusokat nevezik.

### <a name="what-is-expressroute"></a>ActiveX-vezérlőket készült ExpressRoute

Készült ExpressRoute segítségével saját kapcsolatot Microsoft adatközpontokkal és a helyszíni, vagy egy közös helyre környezetben infrastruktúra között. Készült ExpressRoute, a Microsoft felhőszolgáltatásokhoz például a Microsoft Azure és az Office 365-készült ExpressRoute partner helymegosztást létesítmény a kapcsolatokat, vagy közvetlenül beolvasása a meglévő WAN hálózatban (például egy olyan hálózati szolgáltató által biztosított MPLS VPN).

Készült ExpressRoute kapcsolatok ajánlja fel a nagyobb biztonság, további megbízhatóságot, nagyobb sávszélességre és tipikus kapcsolatok-nél kisebb késések az interneten keresztül. Egyes esetekben az adatok átvitele a helyszíni hálózaton és Azure készült ExpressRoute kapcsolatok használatával is is hozam költség jelentős előnyökkel jár. Ha már létrehozott határokon helyszíni kapcsolat a helyszíni hálózaton az Azure, áttelepítheti egy készült ExpressRoute kapcsolat, miközben a virtuális hálózat sértetlen.

Lásd: további információt a [Készült ExpressRoute – gyakori kérdések](../expressroute/expressroute-faqs.md) .

## <a name="site-to-site-connections-and-vpn-devices"></a>Hely közötti kapcsolatok és a virtuális Magánhálózati eszközök

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Mit kell figyelembe venni a virtuális Magánhálózati eszköz kijelölésekor?

Eszköz szállítókkal azt van érvényesíteni partnerség szabványos webhely VPN eszközök csoportja. Ismert kompatibilis virtuális Magánhálózati eszközök, a megfelelő konfigurációs utasításokat vagy minták és eszköz specs listája megtalálható [Itt](vpn-gateway-about-vpn-devices.md). Az eszköz családok ismert kompatibilis szerepelni az összes eszközön virtuális hálózati együtt kell működniük. Szeretné beállítani a virtuális Magánhálózati eszközt, olvassa el az eszköz konfigurációs minta vagy a hivatkozás, amelyre a megfelelő eszköz család felel meg.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Mit tegyek, ha egy virtuális Magánhálózati eszköz nem kompatibilis eszköz ismert listában szereplő rendelkezem?

Ha nem látható az eszköz kompatibilis ismert virtuális Magánhálózati eszköz állapotúként, és meg szeretné használni a virtuális Magánhálózati kapcsolat, kell ellenőrzése, hogy a támogatott IPsec/IKE konfigurációs beállítások és a paraméterek felsorolt [Itt](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list)megfelel-e. Az eszköz megfelel a minimális jól a virtuális Magánhálózati átjárók együtt kell működniük. A számítógépgyártó forduljon a támogatási és konfigurációs további tudnivalókat.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Miért nem a csoportházirend-alapú VPN-csatorna nyissa üresjárati forgalom esetén?

Ez a csoportházirend-alapú (más néven statikus útválasztás) elvárt működés VPN átjárók. Ha több, mint 5 percig a forgalmat a alagutas fölé üresjárati, a alagutas bontva lesz. Indításakor forgalom mindkét irányba halad, a alagutas azonnal hozni.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>A szoftver VPN adatai segítségével Azure csatlakozni?

Webhely határokon helyszíni konfiguráció a Windows Server 2012 Útválasztás és az Access (RRAS) kiszolgálók támogatjuk.

Más szoftver VPN-megoldások működnie kell az átjáró mindaddig, amíg iparágban szabványos IPsec megvalósítás megfelelnek. Forduljon a szoftver szállítójával konfigurálása és a támogatási utasításokat.

## <a name="point-to-site-connections"></a>A kapcsolatok pont-webhely

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Mely operációs rendszerekre van lehetőség a pont-webhely?

Az alábbi operációs rendszerekre telepíthető:

- Windows 7 (32 bites és 64 bites)

- A Windows Server 2008 R2 rendszerben (csak 64 bites verzió esetén)

- Windows 8 (32 bites és 64 bites)

- Windows 8.1 (32 bites és 64 bites)

- A Windows Server 2012 (csak 64 bites verzió esetén)

- A Windows Server 2012 R2 (csak 64 bites verzió esetén)

- Windows 10-ben

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Használhatom-e bármilyen szoftver VPN-ügyfél pont-a-webhely, amely támogatja az SSTP?

nem. Támogatás az csak a Windows operációs rendszereken fent felsorolt korlátozott.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hány VPN ügyfél végpontok lehet az a pont-webhely konfigurálása?

128 VPN-ügyfelek engedélyezni szeretné a csatlakozás egyszerre virtuális hálózathoz támogatjuk.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Használhatom a saját belső nyilvános kulcsú infrastruktúra legfelső szintű hitelesítésszolgáltató csatlakozási pont-webhelyre?

igen. Korábban csak önaláírt legfelső szintű tanúsítványok felhasználható. Továbbra is feltöltheti 20 legfelső szintű tanúsítványok.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Proxykiszolgálók és a tűzfalak pont-webhely lehetőség használatával is bejárása?

igen. SSTP (biztonságos szoftvercsatorna protokoll) alkalmazásával alagutas tűzfalon keresztül. A alagutas fogja HTTPs-kapcsolat jelennek meg.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Ha egy olyan ügyfélgépet beállítva pont-webhelyen újraindításához a virtuális Magánhálózati automatikusan újracsatlakozik?

Alapértelmezés szerint az ügyfélszámítógép fog nem hozza létre a virtuális Magánhálózati kapcsolatot automatikusan.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Nem pont a webhely-támogatás automatikus-újracsatlakozáshoz és a virtuális Magánhálózati ügyfélalkalmazásokon DDNS?

Automatikus újracsatlakozáshoz és DDNS jelenleg nem támogatottak a pont-webhely VPN adatai.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Felvehetek-e a webhely és webhely-pont konfigurációk megtalálhatók ugyanazon virtuális hálózatok?

igen. A két megoldás fog működni, ha az átjáró egy RouteBased VPN-típus van. A klasszikus telepítési modell dinamikus az átjárók szüksége van. Mi nem támogatási pont-pont statikus útválasztási VPN átjárók vagy átjárók - VpnType PolicyBased használatával.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Egyszerre több virtuális hálózatok csatlakoztatása pont a webhely-ügyfél is konfigurálása?

Igen, lehetőség. De a virtuális hálózatok nem lehet IP prefixumokban egymást átfedő, és a pont-webhely címét szóközöket nem áll átfedésben a virtuális hálózatok között.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Mennyi átviteli tekinthetek webhely vagy webhely pont-kapcsolaton keresztül?

A pontos kapacitásának a virtuális Magánhálózati alagutak karbantartása nehezen. IPsec és SSTP crypto-nehéz VPN protokollok. Átviteli is korlátozza a késleltetés és a sávszélesség a helyiségek és az Internet között.

## <a name="gateways"></a>Átjárók

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Mi az, hogy egy csoportházirend-alapú (statikus útválasztás) átjáró?

Házirend-alapú átjárók csoportházirend-alapú VPN adatai hajtja végre. Házirend-alapú VPN adatai titkosítása és közvetlen csomagjait IPsec alagutak cím prefixumokban a helyszíni hálózaton és az Azure VNet között a különféle alapján. A házirend (vagy a forgalmat a választó) általában definíciója a virtuális Magánhálózati konfigurációs-hozzáférési listájára.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Mi az útvonal-alapú (dinamikus útválasztás) az átjárók?

Az átjárók útvonal-alapú az útvonal-alapú VPN adatai hajtja végre. Útvonal-alapú VPN adatai a IP továbbít, illetve útválasztási tábla közvetlen csomagok be a megfelelő alagutas felületek "útvonal" használja. A kapcsolatok ezután titkosítása és a csomagok és kijelentkezés a alagutak visszafejtése. A házirend vagy forgalom sorkijelölőre alapú útvonal VPN van konfigurálva, bármely-a-bármely (vagy helyettesítő karakterek).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Kaphatok a virtuális Magánhálózati gateway IP-cím, mielőtt lehet létrehozni a dokumentumot?

nem. Meg kell az IP-cím megszerezni először az átjáró létrehozása. Az IP-cím változik, ha törli, és hozza létre a virtuális Magánhálózati átjárót.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Hogyan nem első hitelesíti a virtuális Magánhálózati alagutas?

Azure virtuális Magánhálózati PSK (előmegosztott kulcs) hitelesítést használ. Egy előre megosztott kulcs (PSK) azt létrehozása, amikor hozzunk létre, a virtuális Magánhálózati alagutas. Módosíthatja az automatikusan generált PSK saját beállítása előmegosztott kulcs PowerShell-parancsmag vagy REST API-t.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Használhatom-e a előmegosztott kulcs beállítása API konfigurálása a csoportházirend-alapú (statikus útválasztás) átjáró VPN?

Igen, a előmegosztott kulcs API beállítása és a PowerShell parancsmaggal használható Azure csoportházirend-alapú (statikus) VPN adatai és az útvonal-alapú (dinamikus) útválasztási VPN adatai beállításáról.

### <a name="can-i-use-other-authentication-options"></a>Használhatom-e más hitelesítési beállítások?

Kulcsokkal előre megosztott (PSK) hitelesítéshez azt korlátozódik.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Mi az, hogy a "átjáró alhálózat", és miért van rá szükség?

Van még egy átjáró szolgáltatás, amely ahhoz, hogy határokon helyszíni kapcsolódási Futtatás. 

A virtuális Magánhálózati átjáró beállítása VNet az átjáró alhálózat létrehozása kell. Az összes átjárót alhálózat névvel kell GatewaySubnet működik megfelelően. Nem adjon nevet az átjáró alhálózathoz mást. És az átjáró alhálózathoz nem VMs vagy bármely más telepítése.

Az átjáró-alhálózat minimális méret teljesen ki a létrehozni kívánt konfigurációjától függ. Lehet létrehozni egy átjáró alhálózat néhány konfigurációk /29 minél kisebb méretű, de azt javasoljuk, hogy hoz létre egy átjáró alhálózat /28 vagy nagyobb (/ 28, /27, /26 stb.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Telepíthetem-virtuális gépeken futó vagy szerepkör-példányok az átjáró alhálózathoz?

nem.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Hogyan adja meg amely forgalom halad át a virtuális Magánhálózati átjáró?

A klasszikus Azure-portálon használja, ha minden tartomány, amelyet a hálózatok lapon a helyi hálózat a virtuális hálózaton keresztül az átjáró küldött hozzáadása.

### <a name="can-i-configure-forced-tunneling"></a>Is állítható be a kényszerített Tunneling?

igen. Lásd: a [Configure kényszerített tunneling](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Állítsa be a saját VPN server Azure-ban és a helyszíni hálózaton csatlakozhat használni?

Igen, telepítheti a saját VPN átjárók és -kiszolgálók az Azure piactéren elérhető, vagy a saját VPN útválasztó létrehozása Azure-ban. Szüksége lesz a felhasználó által definiált útvonalak beállítása a virtuális hálózati forgalmat megfelelően van-e irányítva a helyszíni hálózatok között a virtuális hálózati alhálózat biztosítása érdekében.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Miért nyílik meg a virtuális Magánhálózati átjáró bizonyos portok?

Azok a Azure infrastruktúra kommunikációhoz szükséges. Azok a védett (zárolva) Azure tanúsítványok. Anélkül, hogy megfelelő tanúsítványok külső szervezetek, beleértve azokat az átjárók az ügyfelek nem tudnak semmilyen hatása, ezeket végponton okozhatnak.

Az átjáró VPN az alapvetően egy hálózati kártya koppintás az magánjellegű ügyfélhálózatához és a nyilvános hálózati szemben lévő egyik hálózati kártya több környezetbe áthelyezett eszközt. Azure infrastruktúra szervezetek nem koppintson az ügyfél magánhálózat megfelelőségi okokból, így azokat meg kell infrastruktúra kommunikáció nyilvános végpontok csatlakozást. A nyilvános végpontok rendszeres beolvasott Azure biztonsági naplózás.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>További információt az átjáró típusú, a követelmények és átviteli

További információért tekintse meg [A virtuális Magánhálózati átjáró beállításait](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Több webhely és a VNet-VNet kapcsolódási

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Milyen típusú átjárók támogatniuk több webhely és a VNet-VNet kapcsolódási?

Csak útvonal alapú VPN (dinamikus útválasztás).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Csatlakozhatok egy VNet egy RouteBased VPN típus egy másik VNet PolicyBased VPN-típus?

Nem, mindkét virtuális hálózatok kell használnia, útvonal-alapú (dinamikus útválasztás) VPN adatai.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>A VNet-VNet forgalom az biztonságos?

Igen, akkor titkosítással védett IPsec/IKE.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Nem utazni VNet-VNet forgalom az Azure Gerinchez?

igen.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Hány helyszíni webhelyek és virtuális hálózatok egy virtuális hálózati csatlakozhat?

Max. 10 kombinált Basic és a szokásos dinamikus útválasztás átjárók; a nagy teljesítmény VPN átjárók 30.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Is lehet használata pont webhely VPN adatai a több VPN bújtatás a virtuális hálózattal?

Igen, pont-webhely (P2S) VPN adatai kínál a virtuális Magánhálózati átjárók több a helyszíni webhely és más virtuális hálózatok csatlakoztatása.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Is állítható be a több alagutak a virtuális hálózat és a több elem webhely VPN használata a helyszíni webhely között?

Nem, az Azure virtuális hálózat és egy helyszíni webhely között a felesleges alagutak nem támogatottak.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Is van, hogy átfedik egymást cím szóközöket a csatlakoztatott virtuális hálózatok és a helyszíni helyi webhelyek között?

nem. Egymást átfedő cím szóközöket okozhat a hálózati konfigurációs fájl feltöltése vagy a "Létrehozása virtuális hálózati" sikertelen lesz.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Kapok több sávszélesség-nél több hely közötti VPN a virtuális egyetlen hálózat?

Nem, minden VPN bújtatás, pont-webhely VPN, beleértve az Azure virtuális Magánhálózati átjárón és a rendelkezésre álló sávszélesség megosztása.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Azure virtuális Magánhálózati átjáró segítségével továbbítási a forgalmat a helyszíni webhelyek között, vagy egy másik virtuális hálózatot?

**Klasszikus telepítési modell**<br>
Azure virtuális Magánhálózati átjáró keresztül hálózaton átvitt forgalom lehetséges, használja a klasszikus telepítési modell, de a hálózati konfigurációs fájl statikus megadott cím szóközt támaszkodik. BGP még nem támogatott Azure virtuális hálózatok és a virtuális Magánhálózati átjárók klasszikus telepítési modellt használja. Nélkül BGP nagyon hiba jobban, és nem ajánlott manuálisan a hálózaton átvitt cím szóközöket definiáló.<br>
**Erőforrás-kezelő telepítési modell**<br>
Az erőforrás-kezelő telepítési modell használatakor című [BGP](#bgp) további információt.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Nem Azure az azonos IPsec/IKE előre megosztott kulcs generálása az összes a virtuális Magánhálózati kapcsolatot a virtuális hálózatához?

Nem, alapértelmezés szerint Azure az eltérő virtuális Magánhálózati kapcsolatot különböző előre megosztott kulcsokat hoz létre. A virtuális Magánhálózati átjáró kulcsa REST API beállítása vagy PowerShell-parancsmag használatával azonban inkább elsődlegeskulcs-értékének beállítása. A kulcs alfanumerikus karakterlánc hossza 1-128 karakterei között kell lennie.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Nem Azure díjat számít fel forgalom virtuális hálózatok között?

Különböző Azure virtuális hálózatok között forgalmához Azure díjai csak a másikra Azure régióból áthaladó forgalmat. A költség ráta szerepel az Azure [Virtuális Magánhálózati átjáró árak](https://azure.microsoft.com/pricing/details/vpn-gateway/) lapon.


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Tudok csatlakozni a virtuális hálózati IPsec VPN adatai és a készült ExpressRoute áramkör?

Igen, ez támogatott. További tudnivalókért lásd: [konfigurálása készült ExpressRoute és futhatnak, hogy a webhely VPN kapcsolatok](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Idegen helyszíni kapcsolódási és a VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Ha a virtuális számítógépemre virtuális hálózat és idegen helyszíni kapcsolat van, hogyan kell tudok csatlakozni a virtuális?

Néhány lehetőség közül választhat. Ha létrehozott zárólap RDP engedélyezve van, csatlakozhat a virtuális gép a virtuális használatával. Ebben az esetben a virtuális és csatlakozni, amelyet a port volna megadása. Állítsa be a port a forgalmat a virtuális gépen kell. Általában azt, akinek az Azure klasszikus portált és a RDP-kapcsolat beállításainak mentése a számítógépre. A beállítások a szükséges kapcsolati adatokat tartalmaznak.

Virtuális hálózat határokon helyszíni kapcsolódási konfigurálva a Ha csatlakozhat a virtuális gép a belső DIP vagy saját IP-cím használatával. Is csatlakozhat a virtuális gép szerint belső DIP, amely a virtuális hálózaton található egy másik virtuális számítógépéről. Ha egy helyről virtuális hálózaton kívüli csatlakozik a DIP használatával a virtuális géphez nem RDP. Például a webhely-pont virtuális hálózatnak van, és nem létrehozni a kapcsolatot a számítógépről, ha nem tud csatlakozni a virtuális gép szerint DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>A virtuális számítógépemre virtuális hálózatban az idegen helyszíni kapcsolat esetén nem forgalmat a saját virtuális el, hogy a kapcsolaton keresztül?

nem. Csak a forgalmat, amelynek a cél IP a virtuális hálózati helyi hálózati IP-címtartományok megadott lévő fog hajtsa végre a virtuális hálózati átjáró. Forgalom esetében a cél IP, amely a virtuális hálózaton belül maradjon a virtuális hálózaton belül. Egyéb forgalmat a nyilvános hálózatok között a terheléselosztó küldött, vagy az Azure virtuális Magánhálózati átjáró keresztül küldött kényszerített tunneling használata esetén. Ha a hibaelhárítás, fontos győződjön meg arról, hogy a helyi hálózaton keresztül az átjáró küldeni kívánt felsorolt összes tartományok. Győződjön meg arról, hogy a helyi hálózaton címtartományokat nem áll átfedésben bármelyik a címtartományokat a virtuális hálózaton. Is érdemes ellenőrizni, hogy a DNS-kiszolgáló esetén a neve megoldása a helyes IP-címére.


## <a name="virtual-network-faq"></a>Virtuális hálózati – gyakori kérdések

A [Virtuális hálózati – gyakori kérdések](../virtual-network/virtual-networks-faq.md)az további virtuális hálózati adatokat tekintheti meg.
 
