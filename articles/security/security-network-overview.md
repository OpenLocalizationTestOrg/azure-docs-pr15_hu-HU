<properties
   pageTitle="Azure hálózati biztonsági áttekintése |} Microsoft Azure"
   description=" Ez a cikk egyszerűen átláthatja a Microsoft Azure nyújtotta előnyöket a hálózati biztonsági területén. Elvégezheti a szükséges alapvető hálózati biztonsági fogalmak és követelmények és információk egyszerű magyarázatainak a Azure nyújtotta előnyöket a következő területi felosztás. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Azure hálózati biztonsági – áttekintés

Microsoft Azure egy robusztus hálózati infrastruktúrát, hogy támogassa az alkalmazás és a szolgáltatás kapcsolódási követelmények tartalmazza. A hálózati kapcsolat mindenre Azure, a helyszíni között található erőforrások között, és a Azure erőforrásokat, és hogy, és az internetről és Azure.

Ez a cikk az a célja, hogy könnyebben átláthatja a Microsoft Azure nyújtotta előnyöket a hálózati biztonsági területén. Itt elvégezheti a szükséges alapvető hálózati biztonsági fogalmak és követelmények egyszerű magyarázatainak. Azt is, tájékoztatást nyújt Azure nyújtotta előnyöket a következő területi felosztás. Vannak egyéb tartalmak, amely lehetővé teszi, hogy egy mélyebb ismertetése a területeket, amely érdekli az első számos hivatkozásokat.

Ez a cikk Azure hálózati biztonsági áttekintése összpontosul a következőket:

- Azure hálózatok
- Hálózati hozzáférés-vezérlés
- Távoli hozzáférés és idegen helyszíni kapcsolódási biztonságos
- Elérhetőség
- Naplózás
- Névfeloldás
- DMZ-architektúra
- Azure biztonság otthon

## <a name="azure-networking"></a>Azure hálózatok

Virtuális gépeken futó a hálózati kapcsolat szükséges. Ezt a követelményt támogatásához Azure virtuális gépeken futó Azure virtuális hálózathoz kapcsolódik van szükség. Az Azure virtuális hálózaton egy logikai szerkezetet, a fizikai Azure hálózati háló épülő. Az összes olyan logikai Azure virtuális hálózat legyen elkülönítve más Azure virtuális hálózatokon. Ezzel az elemzéssel biztosítására, hogy a forgalmat a központi telepítés nem érhető el egyéb Microsoft Azure-ügyfeleknek.

tudj meg többet:

- [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Hálózati hozzáférés-vezérlés
Hálózati hozzáférés-vezérlés az act korlátozza az adott eszközök vagy alhálózat belül az Azure virtuális hálózati kapcsolat áll. A hálózati hozzáférés szabályozása a cél, győződjön meg arról, hogy a virtuális gépeken futó és szolgáltatások hozzáférhetők csak felhasználó és eszköz számára, amelyet őket elérhető. Vezérlőelemek elérése alapul engedélyezése vagy tiltása döntések kapcsolatokhoz az a virtuális gép vagy a szolgáltatás.

Azure támogatja a hozzáférés-vezérlés különböző típusú. Ezek a következők:

- Hálózati réteg szabályozása
- Vezérlőelem irányítása és tunneling kényszerített
- Virtuális hálózati biztonsági készülékek

### <a name="network-layer-control"></a>Hálózati réteg szabályozása
Bármely biztonságos telepítési hálózati hozzáférés-vezérlés valamilyen intézkedés szükséges. A hálózati hozzáférés szabályozása a cél, győződjön meg arról, hogy a virtuális gépeken futó és a hálózati szolgáltatások e virtuális gépeken futó kommunikálni csak más hálózati eszközök, hogy azok kommunikálniuk kell, és más kapcsolatfelvételi le vannak tiltva.

Ha alapvető hálózati szintű hozzáférés-vezérlés (IP-cím és a TCP- és UDP-protokollok alapján), majd használható hálózati biztonsági csoportokat. A hálózati biztonsági csoport (NSG) egyszerű csomag szűrési tűzfalat, és lehetővé teszi a hozzáférést a [5-rekord](https://www.techopedia.com/definition/28190/5-tuple)alapján. NSGs alkalmazás réteg ellenőrző nem rendelkeznek, vagy hitelesített vezérlőelemek elérése.

tudj meg többet:

- [Hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Útvonal vezérlő és a kényszerített alagutazást
Az azt jelenti, hogy az Azure virtuális hálózatokon útválasztási viselkedésének szabályozásához kritikus hálózati biztonságát és vezérlő lehetőség. Ha útválasztás nincs megfelelően beállítva, alkalmazások és szolgáltatások a virtuális gépen futó előfordulhat, hogy csatlakozni nem szeretne csatlakozni, velük eszközök többek között a tulajdonosa, és lehetséges támadók által üzemeltetett eszközökön.

Azure hálózat támogatja az azt jelenti, hogy a hálózati forgalmat a Azure virtuális hálózatok útválasztási viselkedése testreszabása. Ez lehetővé teszi, hogy módosítja az alapértelmezett útválasztási táblázat elemeit az Azure virtuális hálózat. Irányításának útválasztási viselkedés gondoskodhat arról, hogy minden forgalom az egy bizonyos eszközt, vagy az eszközök csoport belépett vagy hagyja Azure virtuális hálózaton keresztül egy bizonyos helyre.

Ha például egy virtuális hálózati biztonsági készülék esetleg a Azure virtuális hálózaton. Győződjön meg arról, hogy minden forgalom az Azure virtuális hálózati halad-e, hogy virtuális biztonsági készülék át szeretné. Ehhez konfigurálása a [Felhasználó által definiált útvonalak](../virtual-network/virtual-networks-udr-overview.md) Azure-ban.

[Kényszerített tunneling](https://www.petri.com/azure-forced-tunneling) egy mechanizmusa annak érdekében, hogy a szolgáltatások nem engedélyezett az interneten eszközökre való kapcsolat létrehozására használható. Figyelje meg, hogy ez különbözik bejövő kapcsolatokkal elfogadása és majd válaszolni. Előtér-webkiszolgálón kell az internethez hosts-összehívás megválaszolása, és így Internet kifejezéskészletébe forgalom engedélyezett ezek webkiszolgálóra bejövő és a webkiszolgálón is válaszolhat.

Mi nem szeretné engedélyezni a kimenő kérni előtér-webkiszolgálón nem. Megkeresések jelenthetnek biztonsági kockázatot, mert ezek a kapcsolatok letöltése kártevőt felhasználható. Akkor is, ha szeretné, hogy ezek a kimenő kérelmek az internethez kezdeményezéséhez előtér-kiszolgálók, érdemes lehet őket, hogy használatba veheti az URL-CÍMÉT, szűrés és naplózás, a helyszíni webhely proxyk folyamatát.

Ehelyett szeretné használni kívánt kényszerített tunneling elkerüléséhez. Ha engedélyezi a kényszerített tunneling, összes kapcsolatot az interneten keresztül a helyszíni átjáró vannak kényszerített. Beállíthatja, hogy a kényszerített tunneling által a felhasználó által definiált utakat lehetőségeinek kihasználása.

tudj meg többet:

- [Mik azok a felhasználó által definiált útvonalak és IP-továbbítási](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtuális hálózati biztonsági készülékek
Hálózati biztonsági csoportokat, a felhasználó által definiált útvonalak és a kényszerített tunneling nyújt a hálózati és átviteli rétegek [OSI modell](https://en.wikipedia.org/wiki/OSI_model)a biztonsági szintet, miközben lehetnek esetek, amikor a nagyobb, mint a hálózaton szinten biztonsági engedélyezni szeretné.

Ha például a biztonsági követelményeknek tartalmazhatja:

- Hitelesítés és az engedélyezés előtt, hogy az alkalmazás hozzáférést
- Behatolási észlelési és behatolási válasz
- Magas szintű protokollok alkalmazás réteg ellenőrzése
- URL-ek szűrése
- Hálózaton szinten víruskereső és antimalware
- Levélszemét bot védelme
- Alkalmazás hozzáférés-vezérlés
- További DDoS védelmet (fölött a védelem biztosított az Azure háló magát DDoS)

Egy Azure partner megoldást segítségével elérheti a továbbfejlesztett hálózati biztonsági funkciókról. A [Microsoft Azure piactéren](https://azure.microsoft.com/marketplace/) megtalálhatók, és "biztonsági" és "hálózati biztonsági" keresése a legfrissebb Azure partner hálózati biztonsági megoldások is megkeresheti.

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Biztonságos távoli hozzáférés és a helyi közötti kapcsolat
A telepítő, konfigurációs és Azure erőforrások igényeinek megfelelően távolról elvégzendő kezelése. Ezenkívül érdemes rendelkező összetevők a helyszíni [hibrid informatikai](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) megoldások telepítése és Azure nyilvános felhőbeli. Ez a helyzet biztonságos távoli hozzáférés szükséges.

Azure hálózat támogatja az alábbi biztonságos távelérési jelenik meg:

- Az egyéni munkaállomások csatlakoztatása az Azure virtuális hálózaton
- A helyszíni hálózaton kapcsolódás virtuális magánhálózattal Azure virtuális hálózathoz
- A helyszíni hálózaton csatlakoztatása az Azure virtuális hálózati dedikált WAN hivatkozással
- Azure virtuális hálózatok csatlakoztatása egymást

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Az egyéni munkaállomások csatlakoztatása az Azure virtuális hálózaton
Lehetnek olyan helyzet, amikor ahhoz, hogy egyes fejlesztők és a műveletek személyzeti virtuális gépeken futó és Azure szolgáltatások kezelése. Például az Azure virtuális hálózaton virtuális gép hozzáféréssel kell rendelkeznie, és a Yammer biztonsági házirendje nem teszi lehetővé az egyes virtuális gépeken futó RDP vagy SSH távoli hozzáférés. Ebben az esetben a pont-webhely virtuális Magánhálózati kapcsolat is használhatja.

A webhely-pont virtuális Magánhálózati kapcsolatot a ahhoz, hogy a felhasználó és a Azure virtuális hálózat közötti személyes és a biztonságos kapcsolat beállítása a [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protokollt használja. Miután a virtuális Magánhálózati kapcsolat létrejött, a felhasználó fogja tudni RDP vagy SSH Azure virtuális hálózat virtuális gépi be a VPN-kapcsolaton keresztül (feltételezve, hogy a felhasználó hitelesíthet és engedélyezett).

tudj meg többet:

- [A PowerShell használatá virtuális hálózati pont a webhely-kapcsolat beállítása](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>A helyszíni hálózaton kapcsolódás virtuális Magánhálózattal Azure virtuális hálózattal
Érdemes a hálózathoz való csatlakozáshoz a teljes vállalati hálózatra vagy részeit, az Azure virtuális. A hibrid informatikai közös: az esetek hol cégek [kiterjesztése a helyszíni adatközponthoz Azure be](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Sok esetben cégek fog tárolni az Azure és az alakzatok helyszíni, például amikor megoldást tartalmazza az Azure és a háttér-adatbázisokat a helyszíni előtér-webkiszolgálón szolgáltatás részei. Is, hogy található Azure irányításának ezek "határokon helyszíni" kapcsolatok típusú erőforrások több biztonságossá tétele és Azure Active Directory tartományi vezérlők kiterjeszti például esetek engedélyezése.

Ehhez egy úgy, hogy a [webhely VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn)használata. Egy webhely VPN és webhely-pont VPN közötti különbség, hogy virtuális Magánhálózattal pont-webhely egyetlen eszközt egy Azure virtuális hálózathoz csatlakozik, miközben egy webhely VPN (például a helyszíni hálózaton) teljes hálózathoz csatlakozik egy Azure virtuális hálózat. Webhely VPN adatai és az Azure virtuális hálózat nagyon biztonságos IPsec alagutas mód VPN protokollt használja.

tudj meg többet:

- [Hozzon létre egy erőforrás-kezelő VNet a webhely virtuális Magánhálózati kapcsolat az Azure-portálon](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Tervezése és kialakítása VPN átjáró](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>A helyszíni hálózaton csatlakoztatása az Azure virtuális hálózat dedikált WAN hivatkozással
Idegen helyszíni kapcsolódási engedélyezése webhely pont- és webhely virtuális Magánhálózati kapcsolatot érvényesek. Azonban néhány szervezetek vegye figyelembe az alábbi hátrányai is:

- Virtuális Magánhálózati kapcsolatot adatok áthelyezése az interneten keresztül – ez közzététele, hogy ezek a kapcsolatok, az adatok áthelyezésére nyilvános hálózaton keresztül felügyeli potenciális biztonsági problémákról. Ezenkívül megbízhatóságának és az Internet-kapcsolatot elérhetősége nem lehet garantálni.
- Azure virtuális hálózatok virtuális Magánhálózati kapcsolatot sávszélesség korlátozott egyes alkalmazások és a céljára, ahogy a max meg a 200Mbps körül tekinthetők meg.

Azoknak a szervezeteknek, általában van szüksége a legmagasabb szintű biztonságát és hozzáférhetőségét határokon helyszíni kapcsolataikat dedikált WAN-kapcsolatok használatával csatlakozás távoli webhelyek. Azure lehetővé teszi a hálózathoz való csatlakozáshoz a helyszíni hálózaton az Azure virtuális használható dedikált nagytávolságú használja. Ez a készült Azure ExpressRoute keresztül érhető el.

tudj meg többet:

- [Technikai áttekintés készült ExpressRoute](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Azure virtuális hálózatok csatlakoztatása egymást
Sok Azure virtuális hálózatok használatát a telepítési lehetőség. Nincsenek számos oka lehet annak, miért megteheti. Az okok egyike lehet kezelés; egyszerűsítése érdekében egy másik lehet biztonsági okokból. A motiváció vagy az erőforrások elhelyezése másik Azure virtuális hálózatok indoklását, függetlenül lehetnek esetek, amikor azt szeretné, hogy a hálózatok összekapcsolódjon minden egyes erőforrások.

Egyik lehetőség az egyik Azure virtuális hálózaton csatlakozhat egy másik hálózaton Azure virtuális szolgáltatások "hurok vissza" szolgáltatások lenne az interneten keresztül. A kapcsolat szeretne elindítani egy hálózaton Azure virtuális, hajtsa végre az interneten, és majd térjen vissza az a cél Azure virtuális hálózati. Ez a beállítás a kapcsolatot a biztonsági problémákról belső bármely internetes kommunikáció közzététele.

Hozzon létre egy Azure virtuális hálózati – Azure virtuális hálózati hely közötti VPN jobb választás lehet. A Azure virtuális hálózati – Azure virtuális hálózati hely közötti VPN az azonos [IPsec alagutas mód](https://technet.microsoft.com/library/cc786385.aspx) protokollt használó megegyezik a fent említett határokon helyszíni webhely virtuális Magánhálózati kapcsolat.

Az egy Azure virtuális hálózati – Azure virtuális hálózati hely közötti VPN előnye, hogy a virtuális Magánhálózati kapcsolat folyamatban van-e az Azure hálózati háló; keresztül az interneten keresztül nem csatlakozni. Ez ad egy webhely VPN adatai, amely az interneten keresztül összehasonlítva biztonsági további réteget.

tudj meg többet:

- [VNet-VNet kapcsolat beállítása erőforrás-kezelő Azure és a PowerShell használatával](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Elérhetőség
Elérhetőség biztonsági programok fő része. Ha a felhasználók és a rendszer nem fér hozzá őket szükséges eléréséhez a hálózaton keresztül, a szolgáltatás lehet tekinteni sérül. Azure, amely támogatja a következő módszerek magas rendelkezésre állás hálózati technológiák foglalja magában:

- HTTP-alapú terheléselosztási
- A hálózati szintű terheléselosztás
- Globális terheléselosztási

A célja, hogy több eszközzel közötti kapcsolatok egyenletesen terjesztése mechanizmusa terheléselosztás. A szervezeti céloknak való terheléselosztás a következők:

- Növelje elérhetősége –, amikor egyenleg kapcsolatok minden több eszközön töltse be, az eszközök közül egy vagy több előfordulhat, hogy nem érhető el, a hátralévő online eszközökön futó szolgáltatások továbbra is a tartalmat a szolgáltatásból
- A teljesítmény növelése –, különféle eszközökön keresztül betöltésekor egyenleg kapcsolatok egyetlen eszközt nem kell hajtania a processzor találatot. Ehelyett a tartalom felszolgálásához feldolgozás és a memóriában igényeivel kiterjed különféle eszközökön keresztül.

### <a name="http-based-load-balancing"></a>HTTP-alapú terheléselosztási
Azoknak a szervezeteknek, futtassa a szolgáltatások webes gyakran kívánja, hogy egy HTTP-alapú terheléselosztó elé biztosítása érdekében a megfelelő szintű teljesítmény és magas elérhetősége érdekében webes szolgáltatások. Hagyományos hálózat alapú terheléselosztókkal, alkalmazással szemben a terheléselosztási HTTP-alapú által elvégzett döntéseket terheléselosztókkal alapuló a HTTP protokoll, nem pedig a hálózati és átviteli réteg protokollok jellemzői.

Ahhoz, hogy Ön a HTTP-alapú terheléselosztási a webes szolgáltatás, Azure teszi lehetővé az Azure alkalmazás átjáró. Az Azure alkalmazás átjáró támogatja:

- HTTP-alapú terheléselosztás – betöltés terheléselosztó döntéseket alapján készülnek jellemző speciális a HTTP-jegyzőkönyv
- Cookie-alapú munkamenet affinitás – ezzel a funkcióval ellenőrzi, hogy az ügyfél és a kiszolgáló közötti sértetlen marad, egy adott terheléselosztó mögött kiszolgálókon létrehozott kapcsolatok. Ez a adatblokkok stabilitás tranzakciók.
- Az SSL kiürítése – Ha az ügyfél kapcsolat jön létre a terheléselosztó, hogy az ügyfél és a terheléselosztó közötti munkamenet használatával a HTTPS titkosított (SSL /) protokoll. Azonban a teljesítmény növelése érdekében lehetősége van a terheléselosztó és az érintett webkiszolgálóra betöltés terheléselosztó használata a HTTP (titkosítatlan) protokoll mögött közötti kapcsolat. A továbbiakban "SSL kiürítése", mert a web kiszolgálók mögött a terheléselosztó nem a titkosítási felügyeli processzor terhelést, és megoldást ezért látnia kell szolgáltatáskérések gyorsabban.
- URL-alapú tartalom útválasztás – Ez a szolgáltatás lehetővé teszi a a terheléselosztó döntések hová a célhely URL-címe alapján kapcsolatok továbbítsa számára. Ez rugalmasságot sokkal több mint megoldások, amelyek IP-címeken alapuló terheléselosztó döntéseket betöltése.

tudj meg többet:

- [Alkalmazás átjáró – áttekintés](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>A hálózati szintű terheléselosztás
Alkalmazással szemben a HTTP-alapú terheléselosztás, hálózaton szinten terheléselosztás döntéseket betöltés terheléselosztó IP-címet és a portszámot (TCP vagy UDP) címek alapján.
Kaphatnak a hálózaton szinten terheléselosztási Azure-ban használatával az Azure betöltése terheléselosztó előnyeit. A Azure terheléselosztó néhány főbb jellemzők a következők:

- IP-címet és a portszámot számok hálózaton szinten terheléselosztás alapján
- Minden alkalmazás protokollnak támogatása
- Az Azure virtuális gépeken futó egyenlege betöltése és felhőbeli szolgáltatások szerepkör-példányok
- Internetes (külső terheléselosztás) és az Internet is használhatók szemben lévő (belső terheléselosztás) alkalmazások és a virtuális gépeken futó
- Figyelés, végpontot, azt határozza meg, ha a terheléselosztó mögött a szolgáltatások bármelyikét váltak nem érhető el

tudj meg többet:

- [Internetes szemben lévő terheléselosztó több virtuális gépeken futó vagy a szolgáltatások között](../load-balancer/load-balancer-internet-overview.md)
- [Belső betöltés terheléselosztó – áttekintés](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globális terheléselosztási
Egyes szervezetek is szükség van a legmagasabb szintű elérhetősége lehetséges. A cél elérjen egy úgy, hogy host alkalmazások globálisan elosztott adatközpont esetén. Az alkalmazás a világszerte található adatközpontokban üzemelteti, esetén lehetséges egy teljes geopolitikai terület válnak elérhetetlenné, és továbbra is az alkalmazás és működik.

Mellett az elérhetőség előnnyel globálisan elosztott adatközpont esetén alkalmazások üzemeltetnie kap is elérheti teljesítmény előnyeit. A teljesítmény előnyökkel jár szerezhető be egy mechanizmusa, amely a kérelem a szolgáltatást a az eszközt, amelyet a kérés legközelebb esik adatközponthoz irányítja használatával.

Globális terheléselosztás adja meg a következő előnyökkel jár. Az Azure-kaphatnak a globális terheléselosztási Azure forgalom kezelővel előnyeit.

tudj meg többet:

- [Mi az forgalom Manager?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Naplózás
Naplózás a hálózaton szinten bármelyik hálózati biztonsági forgatókönyvet kulcs függvény. Az Azure-naplózható információk naplózása hálózaton szinten megszerezni a hálózati biztonsági csoportok kapott adatok alapján. A naplózás NSG kap adatait:

- Naplókat – ezek a naplók megtekintéséhez az Azure-előfizetésekhez küldött összes művelet szolgálnak. Ezek a naplók alapértelmezés szerint engedélyezve vannak, és az Azure portál belül használható.
- Az eseménynaplók – ezek a naplók információt nyújtanak az alkalmazott milyen NSG szabályokat.
- Számláló naplók – ezek a naplók lehetővé teszik, hogy hány alkalommal NSG szabályok volt alkalmazott megtagadhatja vagy forgalmának engedélyezésére.

[Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), a hatékony adatkezelő képi megjelenítés eszközben megtekintéséhez és elemzéséhez ezeket a naplókat is használhatja.

tudj meg többet:

- [Log elemzésének hálózati biztonsági csoportok (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Névfeloldás
Névfeloldás, akkor a kritikus függvény az összes szolgáltatás Ön üzemelteti Azure-ban. A biztonsági szemszögéből biztonságos neve felbontás függvényének kérések a helyek közül a támadó webhelyre átirányítása támadó vezethet. Biztonságos névfeloldás a felhőben tárolt szolgáltatások felvétele szükséges.

A cím kell névfeloldás két típusa van:

- Belső névfeloldás – belső névfeloldás az Azure virtuális hálózatokat vagy a helyszíni hálózatokat szolgáltatások használják. Belső névfeloldásra használt nevek nem érhetők el az interneten keresztül. Az optimális biztonsági fontos, hogy a belső neve felbontás séma nem érhető el a külső felhasználók számára.
- Személyek és a helyszíni és Azure virtuális hálózatok kívüli eszközök külső névfeloldás – külső névfeloldás használják. Ezek a neveket, amelyeket láthatók az internethez, irányítsa át a felhőalapú szolgáltatások kapcsolat használják.

A belső névfeloldás két lehetőség áll rendelkezésére:

- Az Azure virtuális hálózati DNS-kiszolgáló – amikor létrehoz egy új Azure virtuális hálózati DNS-kiszolgáló létrejön. A DNS-kiszolgáló nevének a hálózaton, hogy Azure virtuális gépeken futó oldhatja meg. A DNS-kiszolgáló nem állítható be, és az Azure háló manager, így téve biztonságos neve felbontás megoldást kezeli.
- Jelenítse meg a saját DNS-kiszolgáló – azt, hogy elhelyez egy DNS-kiszolgálót, a saját kiválasztása a Azure virtuális hálózaton. A DNS-kiszolgáló lehet az Active Directory integrált DNS-kiszolgáló vagy Azure partnertől, amely a Microsoft Azure piactéren lévő szerezhető be által biztosított saját DNS-kiszolgáló megoldást.

tudj meg többet:

- [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)
- [A virtuális hálózati (VNet) által használt DNS-kiszolgálók kezelése](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

A külső DNS-feloldási két lehetőség áll rendelkezésére:

- A saját külső DNS server helyszínen szolgáltató
- A saját külső DNS-kiszolgáló állomás szolgáltató

Nagy szervezetekben fog üzemelteti saját DNS servers helyszíni. Ehhez, mert a hálózati szakértelmét és végrehajtásához globális jelenléti rendelkeznek.

A legtöbb esetben célszerűbb megbízhat egy szolgáltatót a DNS-névfeloldás szolgáltatást. Ezek a szolgáltatók hálózati szakértelmét és a névfeloldás szolgáltatást a nagyon magas állásának globális jelenléti rendelkezik. Elérhetőség elengedhetetlen a DNS-szolgáltatások, mert ha nem sikerül a névfeloldás szolgáltatást, senki sem tudják elérni az internetes szolgáltatások.

Azure biztosít a könnyen hozzáférhető és a külső DNS-megoldást performant Azure DNS formájában. A külső neve felbontás megoldás megnyitja a világszerte Azure DNS-infrastruktúrát előnyeit. Lehetővé teszi a tartományt az Azure eszközeivel azonos hitelesítő adatait, API, és a többi Azure szolgáltatások, számlázási tárolni. Azure részeként a erős biztonsági funkciók a platform épített is örökli.

tudj meg többet:

- [Azure DNS – áttekintés](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ-architektúra
Sok vállalati szervezetek DMZs használatával létrehoz egy pufferelési-zónát az Internet és a szolgáltatások között a hálózat szakaszokhoz. A hálózat DMZ részét minősülnek alacsony zónában, és nincs értékes eszköz, hogy a hálózati szakasz kerül. A szokásos láthatja a hálózati biztonsági eszközök, amelyek a DMZ szakaszában a hálózati kártya és a virtuális gépeken futó, és fogadja el az internetről bejövő kapcsolatok szolgáltatásokat tartalmazó csatlakozik egy másik hálózati kapcsolat.

Számos változatok DMZ tervezés és a döntési bevezetését tervezi a DMZ, és kattintson az DMZ használja, ha úgy dönt, hogy közül válasszon, hogy milyen típusú alapján a hálózati biztonsági követelményeknek.

tudj meg többet:

- [A Microsoft-Felhőszolgáltatások és a hálózati biztonság](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Azure biztonság otthon
Biztonság otthon segítségével megakadályozhatja, hogy, észleli és veszélyek válaszolni, és itt növeli a betekintést kap abba, és szabályozni kell, az Azure erőforrások biztonsága. Integrált adatvédelem figyelése és a Csoportházirend kezelése az Azure-előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

Azure biztonság otthon segítségével optimalizálása, és figyelemmel követheti a hálózati biztonsági szerint:

- Hálózati biztonsági javaslatok kezeléséről
- A hálózati biztonsági beállítások állapotának ellenőrzése
- Jelenthet alapú hálózati veszélyek mindkét végpont és a hálózaton szinten

tudj meg többet:

- [Azure biztonság otthon – bevezetés](../security-center/security-center-intro.md)
