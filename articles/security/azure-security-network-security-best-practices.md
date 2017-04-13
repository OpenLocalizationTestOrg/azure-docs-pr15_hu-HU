<properties
   pageTitle="Azure hálózati biztonsággal kapcsolatos gyakorlati tanácsok |} Microsoft Azure"
   description="Ez a cikk gyakorlati tanácsok a hálózati biztonsági használatával beépített Azure lehetőségeket biztosít."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Azure hálózati biztonsággal kapcsolatos gyakorlati tanácsok

Microsoft Azure lehetővé teszi, hogy csatlakozzon a virtuális gépeken futó és berendezések más hálózati eszközök úgy, hogy a Azure virtuális hálózatokon. Az Azure virtuális hálózat egy virtuális hálózati szerkezetet, amely lehetővé teszi, hogy csatlakozzon a virtuális hálózati kártya közötti engedélyezve hálózati eszközök TCP alapú kommunikáció engedélyezése virtuális hálózathoz. Azure virtuális gépeken futó Azure virtuális hálózathoz csatlakoztatva tud csatlakozni az Azure virtuális hálózatához, különböző Azure virtuális hálózatok, az interneten, vagy akár saját a helyszíni hálózaton eszközökön.

Ebben a cikkben ölelik fog Azure hálózati biztonsággal kapcsolatos gyakorlati tanácsok gyűjteménye. A vevők változat például saját maga, és a gyakorlati tanácsok az Azure hálózattal élmény származik.

Az egyes a legjobb megismerheti:

- Mi a legjobb módszer az
- Miért szeretné engedélyezni, hogy a legjobb
- Mi lehet az eredményt, ha nem sikerül ahhoz, hogy a legjobb módszer
- Célszerű lehet kiváltása
- Hogyan talál ahhoz, hogy a legjobb módszer

Ez a cikk Azure hálózati biztonsággal kapcsolatos gyakorlati tanácsok alapul egy konszenzus véleményt és Azure platform és a szolgáltatás beállítása, ez a cikk készült időben léteznek. Vélemények és -technológiák időbeli változását, és ez a cikk a változások követése érdekében rendszeresen frissül.

Azure hálózati biztonsággal kapcsolatos gyakorlati tanácsok a jelen cikkben tárgyalt a következők:

- Logikailag szakasz alhálózat
- Útválasztási viselkedésének szabályozásához
- Kényszerített Tunneling engedélyezése
- Virtuális hálózati készülékek használata
- Biztonsági kivételével DMZs üzembe helyezése
- Információk megjelenítése az internethez dedikált WAN-kapcsolatok elkerülése
- Üzemidőt és a teljesítmény optimalizálása
- Globális terheléselosztás használata
- Azure virtuális gépeken futó RDP elérésének letiltása
- Azure-biztonság otthon engedélyezése
- A adatközponthoz kiterjeszti az Azure


## <a name="logically-segment-subnets"></a>Logikailag szakasz alhálózat

[Azure virtuális hálózatokat](https://azure.microsoft.com/documentation/services/virtual-network/) hasonlítanak a helyi hálózathoz kapcsolódik, a helyszíni hálózaton. Az Azure virtuális hálózati mögött lényege, hogy hoz létre egy egyetlen IP-cím térköz-alapú magánhálózati amelyen elhelyezheti a [Azure virtuális gépeken futó](https://azure.microsoft.com/services/virtual-machines/). Az osztály (10.0.0.0/8), a B osztályú (172.16.0.0/12) és osztály C (192.168.0.0/16) tartományok a saját IP cím helyek érhető el.

Mi hasonló megadható, hogy a helyszíni, bizonyára tudni szeretné, hogy a nagyobb címterületet szakaszokhoz alhálózat be. Az alhálózathoz létrehozása [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) alapú alhálózatok elvek is használhatja.

Továbbítás alhálózat közötti automatikusan megtörténik, és nem kell manuálisan állítsa be a útválasztási táblák. Az alapértelmezett azonban, hogy nincsenek-e vezérlők nélküli hálózati hozzáférés a hoz létre az Azure virtuális hálózati alhálózat közötti. Létrehozásához az hálózati vezérlőelemek elérése alhálózat között kell elhelyezni egy üzenetet a alhálózat közötti.

A feladat végrehajtásához használható formázási egyik [Hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs az 5 összetevője (a forrás IP-címe, forrásport, cél IP, célport és 4 protokollnak) használó egyszerű állapot-nyilvántartó csomagellenőrző eszközök hozhat létre, engedélyezése és tiltása megközelítés szabályainak hálózati forgalmának engedélyezésére. Engedélyezze, illetve elutasítása egyetlen IP-címet, és több IP-címekről vagy akár és a teljes alhálózat a forgalmat.

Hálózati hozzáférés-vezérlés alhálózat közötti NSGs verzióval lehetővé teszi, információforrások, amelyek az azonos biztonsági zóna vagy a szerepkör a saját alhálózat tartozó elhelyezni. Például érdemes egy egyszerű 3 szintű alkalmazás, amely egy webes réteg, az alkalmazás logika réteg és egy adatbázis szint rendelkezik. Virtuális gépeken futó tartozó minden ezeket a meghatározási be saját alhálózat helyezi el. NSGs majd forgalmat a alhálózat közötti szabályozására használhatja:

- Webes réteg virtuális gépeken futó csak akkor kezdeményezhet az alkalmazás logika gépek-kapcsolatot, és csak ideiglenesen elfogadhatja az internetről kapcsolatok
- Alkalmazás logika virtuális gépeken futó csak akkor kezdeményezhet adatbázis réteg kapcsolatokat, és csak elfogadhatja a webes réteg kapcsolatok
- Adatbázis réteg virtuális gépeken futó kapcsolatot bármi kívül saját alhálózathoz nem kezdeményezzen, és csak ideiglenesen elfogadhatja az alkalmazás logika réteg kapcsolatok

Hálózati biztonsági csoportokat, és hogyan használhatja őket az Azure virtuális hálózatok logikailag szakaszokhoz kapcsolatos további információért olvassa el a cikk a [Mi az hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Útválasztási viselkedésének szabályozásához

Virtuális gép Azure virtuális hálózaton helyezésekor észre fogja venni, hogy az a virtuális gép csatlakozhat más virtuális gép Azure virtuális hálózaton, még akkor is, ha a különböző alhálózathoz virtuális gépeken futó vannak. Miért lehet az oka az, hogy van-e rendszer útvonalak alapértelmezés szerint engedélyezve vannak, amelyek lehetővé teszik a kommunikáció típusa gyűjteménye. Ezek az alapértelmezett útvonalak engedélyezze a virtuális gépeken futó ugyanazon a hálózaton Azure virtuális kapcsolatokhoz egymással, és az interneten (csak az internethez kimenő kommunikáció).

Az alapértelmezett rendszer útvonalak sok telepítési esetekben hasznosak, miközben vannak a helyzet, amikor a útválasztási konfiguráció a telepítési testreszabása. Ezekre a beállításokra lehetővé teszi, hogy állítsa be a következő ugrás címét meghatározott célok eléréséhez.

Azt javasoljuk, hogy amikor rendszerbe állítják virtuális biztonsági készülék, amely fogja megvitatjuk, újabb legjobb módszer a felhasználó által definiált útvonalak konfigurálása.

> [AZURE.NOTE] felhasználó által definiált útvonalak nem szükséges, és az alapértelmezett rendszer útvonalak a legtöbb esetben fognak működni.

További információ a felhasználó által definiált útvonalak és konfigurálásáról őket a [felhasználó által definiált útvonalak és IP-továbbítási](../virtual-network/virtual-networks-udr-overview.md)témakör elolvasásával talál.

## <a name="enable-forced-tunneling"></a>Kényszerített Tunneling engedélyezése

Megértéséhez kényszerített tunneling, akkor hasznos, ha meg szeretné érteni, hogy milyen "felosztása tunneling" van.
A leggyakoribb példa osztott tunneling virtuális Magánhálózati kapcsolat látható. Tegyük fel, hogy a virtuális Magánhálózati kapcsolat létrehozása a szobában a vállalati hálózathoz. A kapcsolat lehetővé teszi, hogy csatlakozzon a vállalati hálózaton erőforrások és a vállalati hálózaton erőforrásokra mutató összes kommunikációs hajtsa végre a virtuális Magánhálózati alagutas.

Mi történik, ha meg szeretné erőforrásokhoz az interneten? Ha osztott tunneling engedélyezve van, ezeket a kapcsolatokat lépjen közvetlenül az interneten, és nem a virtuális Magánhálózati alagutas keresztül. Néhány biztonsági szakértők fontolja meg, erre a kockázatot, és ezért javasoljuk, hogy a felosztott tunneling tiltható le, és a minden kapcsolat lehetőséget, az interneten szánt valamint azok szánt vállalati erőforrások, lépjen a virtuális Magánhálózati alagutas keresztül. Az ezzel előnye, hogy az Internet-kapcsolatot majd kényszerített nem arra az esetre, ha a VPN-ügyfél csatlakozik az internethez, a virtuális Magánhálózati alagutas kívüli vállalati hálózat biztonsági eszközökön keresztül.

Most vegyük visszaadni ebben a virtuális gépeken Azure virtuális hálózaton. Az alapértelmezett útvonalak Azure virtuális hálózat lehetővé teszi, hogy virtuális gépeken futó kezdeményezése az internetes forgalmat. Ez túl megfelelhet biztonsági kockázatot, ezeket a kimenő kapcsolatokat virtuális gép homonimaszerű webcímmel felületének növelheti és támadók kapcsolatos kell károkozásra.
Emiatt azt javasoljuk, hogy engedélyezze a kényszerített tunneling a virtuális gépeken amikor határokon helyszíni kapcsolatban van a Azure virtuális hálózat és a helyszíni hálózaton között. Az előadás tartományok helyileg kapcsolódási e Azure hálózati ajánlott eljárások a dokumentumon belül.

Ha nem rendelkezik a helyszíni közötti kapcsolatot, ellenőrizze, hogy akkor kihasználhatja hálózati biztonsági csoportok (korábban tárgyalt) vagy az Azure virtuális biztonsági készülékek (a cikkben leírt next) kimenő kapcsolatok megakadályozására az internetről származó az Azure virtuális gépeken futó hálózati.

Ha többet szeretne megtudni a kényszerített bújtatásra, és hogyan engedélyezhető, olvassa el a [PowerShell- és erőforrás-kezelő Azure konfigurálása kényszerített Tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md)használatáról.

## <a name="use-virtual-network-appliances"></a>Virtuális hálózati készülékek használata

Hálózati biztonsági csoportokat és a felhasználó által definiált útválasztás megadhatja az egyes mértéke a hálózati és átviteli rétegek [OSI modell](https://en.wikipedia.org/wiki/OSI_model)a hálózati biztonság, miközben létezhetnek azokról a helyzetekről, ahol fogja szeretné, vagy az objektumhalomban magas szintű biztonsági engedélyeznie kell lennie. Ilyen esetben azt javasoljuk, hogy virtuális hálózati biztonsági készülékek Azure partnerek által biztosított rendszerbe.

Azure hálózati biztonsági készülékek tartana biztonsági szintjei jelentősen továbbfejlesztett mi hálózaton szinten vezérlőelemek által biztosított fölé. A hálózati biztonsági funkciók virtuális hálózati biztonsági készülékek által biztosított a következők:

- Firewalling
- Behatolási észlelési/behatolási megelőzése
- Biztonsági kezelése
- Alkalmazás vezérlő
- Hálózati alapú rendellenességet kimutatására
- Webes szűrése
- Víruskereső
- Botnet védelme

Ha szüksége van egy magasabb szintű hálózati biztonsági hálózati szintű hozzáféréssel vezérlőkkel szerezhet be, majd azt javasoljuk, hogy vizsgálja meg, és Azure virtuális hálózati biztonsági készülékek telepítése.

Megtudhatja, milyen Azure virtuális hálózati biztonsági készülékek érhetők el, és saját képességeinek ismertetése, látogasson el a [Microsoft Azure piactéren](https://azure.microsoft.com/marketplace/) és a "biztonsági" és "hálózati biztonsági" keresése.

##<a name="deploy-dmzs-for-security-zoning"></a>Biztonsági kivételével DMZs üzembe helyezése
A DMZ vagy "peremhálózat" célja, hogy adja meg a biztonsági az eszközök és az Internet között egy további réteget fizikai vagy logikai hálózati megrajzolt. A DMZ szándékának, hogy csak a kívánt forgalmat a hálózati biztonsági eszköz múltbeli és a Azure virtuális hálózatba engedélyezett a speciális hálózati hozzáférés ellenőrző eszközök helyére a DMZ hálózati szélén.

DMZs hasznosak, mert a hálózati access control management figyelés, bejelentkezés, és Azure virtuális hálózatához szélén a eszközökön jelentéskészítés összpontosíthat. Itt volna általában engedélyeznie DDoS megelőzése, behatolási észlelési/behatolási megelőzése rendszerek (Azonosítók/IP-CÍMEI), tűzfalszabályokat és házirendeket, webes szűrési, hálózati antimalware és további. A hálózati biztonsági eszközök között az Internet és Azure virtuális hálózatához ülnie, és mindkét hálózaton felületet van.

Amikor egy DMZ egyszerű felépítésének, is meg vannak számos különböző DMZ tetszetős, például: párhuzamosan, tri környezetbe áthelyezett, többszintű-környezetbe áthelyezett, és másokkal.

Azt javasoljuk az összes magas biztonsági telepítésekhez, hogy akkor érdemes megfontolni a DMZ tartalmasabbá teszi a Azure erőforrások hálózati biztonsági szintjét.

Ha többet szeretne tudni a DMZs és Azure-ban történő telepítéséről őket, olvassa el a cikk [a Microsoft-Felhőszolgáltatások és a hálózati biztonság](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Információk megjelenítése az internethez dedikált WAN-kapcsolatok elkerülése
Sok szervezetek választotta a hibrid informatikai továbbításához. A hibrid informatikai a vállalat információk eszközeinek vannak az Azure, miközben mások marad, hogy a helyszíni. A legtöbb esetben a miközben más összetevő marad a helyszíni az Azure szolgáltatás bizonyos összetevői fog futni.

Az informatikai forgatókönyv a hibrid van általában bizonyos típusú határokon helyszíni kapcsolat. Ez határokon helyszíni kapcsolatszolgáltatások lehetővé teszi, hogy a vállalati hálózathoz való csatlakozáshoz a helyszíni hálózatok Azure virtuális. Két határokon helyszíni kapcsolódási megoldások állnak rendelkezésre:

- Webhely VPN
- Készült ExpressRoute

[Webhely VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) a helyszíni és egy Azure virtuális hálózat közötti virtuális személyes kapcsolat jelöli. A kapcsolat esedékes az interneten keresztül, és lehetővé teszi, hogy a hálózat és Azure között egy titkosított hivatkozás "alagutas" információk. Webhely VPN egy biztonságos, elért technológia, amely évtizedek a vállalatok a különféle méretű rendszerbe. A végrehajtott alagutas titkosítási [IPsec alagutas mód](https://technet.microsoft.com/library/cc786385.aspx)használatával.

Egy megbízható, a megbízható és a létrehozott technológia ugyan a webhely VPN a forgalmat a alagutas belül bejárása az internethez. Ezeken kívül sávszélesség viszonylag korlátozott legfeljebb 200Mbps kapcsolatban.

Ha kivételes szintjét biztonsági vagy a teljesítmény a határokon helyszíni kapcsolatokat kér, azt javasoljuk, hogy a készült Azure ExpressRoute-et az idegen helyszíni kapcsolódási. Készült ExpressRoute egy dedikált WAN a helyszíni helyre vagy a egy Exchange-szolgáltató közötti kapcsolat. Mivel telco kapcsolatot, az adatok az interneten keresztül nem utazási, és ezért nem érhető el a potenciális kockázatokat az internetes kommunikáció.

Ha többet szeretne tudni a készült Azure ExpressRoute működése és telepítéséről, olvassa el a cikk [Készült ExpressRoute technikai áttekintés](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Üzemidőt és a teljesítmény optimalizálása
Bizalmas, integritását és elérhetőség (CIA) közé tartoznak a háromrészes, az aktuális leginkább segíti elő biztonsági modell. Bizalmas titkosítással kapcsolatos pedig adatvédelmi, integritását miként gondoskodhat arról, hogy az adatok jogosulatlan személyek nem változik, és miként gondoskodhat arról, hogy jogosult személyek elérésére jogosultak hozzáférhet az elérhetőség. Ezekre a területekre közül bármelyik nem sikerült egy potenciális szerződésszegés biztonsági jelöli

A gondolatot elérhetőségét a üzemidőt és a teljesítmény tekintsék. A szolgáltatás nem működik, ha az adatok nem érhető el. Ha a teljesítmény így gyenge, hogy az adatok használhatatlanná válik, és azt is vegye figyelembe az adatok nem lesznek elérhetők. Emiatt a biztonsági szemszögéből azt kell tennie bármilyen azt is használatával biztosítható, hogy a szolgáltatások optimális üzemidőt és a teljesítmény.
Elérhetőség és a teljesítmény javítása érdekében használja a népszerű és hatékony módszer környezetbe terheléselosztás. Terheléselosztás a hálózati forgalmának engedélyezésére elosztása szolgáltatás részét képező kiszolgálóin módszer. Ha például esetén előtér-webkiszolgálón a szolgáltatás részét képező segítségével terheléselosztás elosztása a forgalmat a több előtér-webkiszolgálón.

A forgalom eloszlása növeli a elérhetősége, mivel egy webkiszolgálón nem érhető el, ha a terheléselosztó ne küldjenek forgalom ennek a kiszolgálónak és forgalmat továbbra is online kiszolgálók irányítsa át. Mivel a processzor, a hálózati és a terhelést kérések felszolgálásához van elosztva a betöltés memória kiegyensúlyozott kiszolgálók terheléselosztás segít teljesítmény elérése érdekében.

Azt javasoljuk, hogy Ön alkalmaznia terheléselosztás amikor lehetőségek közül választhat, illetve tetszés szerint a szolgáltatás. Azt fogja cím, az alábbi szakaszok megfelelőségét.
Az Azure virtuális hálózaton szinten Azure nyújt a három elsődleges betöltése terheléselosztó beállítások:

- HTTP-alapú terheléselosztási
- Külső terheléselosztási
- Belső terheléselosztási

## <a name="http-based-load-balancing"></a>HTTP-alapú terheléselosztási
Milyen-kiszolgálót, küldje el a HTTP protokoll jellemzői hibák kapcsolatos döntések HTTP-alapú terheléselosztás alapozza. Azure-alkalmazás átjáró neve előbb HTTP-terheléselosztó tartalmaz.

Azt javasoljuk, hogy meg velünk Azure alkalmazás átjáró esetén:

- Az azonos felhasználó/ügyfél munkamenetből kérések elérje a háttéradatbázist virtuális ugyanarra a gépre igénylő alkalmazásokat. Példák volna vásárlás, a bevásárlókocsi-alkalmazások és webes levelezési kiszolgálóját.
- Webes kiszolgálófarm az SSL lemondási terhelést ingyenes alkalmazás átjáró [SSL kiürítése](https://f5.com/glossary/ssl-offloading) szolgáltatás kihasználva kívánt alkalmazást.
- Alkalmazások, például a tartalomkézbesítési hálózatai igénylő azonos hosszabb ideig futó TCP-kapcsolaton továbbíthatók és betöltése több HTTP-kérések kiegyensúlyozott másik háttéradatbázis kiszolgálókhoz.

Ha többet szeretne tudni az Azure alkalmazás átjáró működéséről, és hogyan használhatja azt a a környezetekben, olvassa el a cikk [Alkalmazás átjáró áttekintése](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Külső terheléselosztási
Külső terheléselosztás mi történjen, amikor az internetről bejövő kapcsolatokkal kerül a az Azure virtuális hálózaton található kiszolgálók között. Azure külső terheléselosztó adja meg ezt a lehetőséget, és azt javasoljuk, hogy használjuk, nincs szükség a Beragadó munkamenetek vagy SSL kiürítése.

Alkalmazással szemben a HTTP-alapú terheléselosztás, a külső betöltése terheléselosztó adatokat használja, a hálózati és átviteli rétegek OSI hálózati modell döntések egyenleg kapcsolat betöltése mely kiszolgálón.

Azt javasoljuk, hogy használható külső terheléselosztás bármikor [állapot nélküli alkalmazások](http://whatis.techtarget.com/definition/stateless-app) fogad, bejövő felkérést az internetről.

További tudnivalók a Azure külső terheléselosztó működése, és adja meg, hogyan telepítheti a, olvassa el a következő cikk: [első lépések létrehozása egy internetes szemben lévő terheléselosztó az erőforrás-kezelő PowerShell használatával](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Belső terheléselosztási
Belső terheléselosztás hasonlít a külső terheléselosztás és ugyanezzel a módszerrel használatával betöltése mögött azokat a kiszolgálókat egyenleg-kapcsolatot. Az egyetlen különbség, hogy a terheléselosztó ebben az esetben fogadja a kapcsolatokat a virtuális gépeken futó, amelyek nem az interneten. A legtöbb esetben a kapcsolatok, a terheléselosztás elfogadott Azure virtuális hálózati eszközök által kezdeményezett.

Azt javasoljuk, hogy az esetek, amelyek előnyei a ezt a lehetőséget, például, hogy mikor kell életbe egyenleg kapcsolatok SQL-kiszolgálók vagy belső webkiszolgálón betöltése a belső terheléselosztási használja.

Ha többet szeretne tudni az Azure belső terheléselosztás működéséről, és hogy miként telepítheti azt, olvassa el a következő cikk: [első lépések létrehozása egy belső terheléselosztó PowerShell használatával](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Globális terheléselosztás használata
Nyilvános felhő számítógépes tesz lehetővé globálisan üzembe található adatközpontokkal világszerte összetevőket rendelkező alkalmazások meghatározva. Ez a lehetőség a Microsoft Azure Azure a globális adatközponthoz jelenléte miatt. A terheléselosztási korábban említett technológiák alkalmazással szemben globális terheléselosztási lehetővé teszi, hogy a szolgáltatások elérhetővé, ha a teljes adatközpontokkal válhat nem érhető el.

Globális terheléselosztási Azure-ban [Azure forgalom Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)kihasználva a következő típusú elérheti. Kezelő forgalom megkönnyíti mindenre betöltése a szolgáltatások, a felhasználó helye alapján egyenleg-kapcsolatot.

Ha például ha a felhasználó kérelmének végez a az Európai Unió a szolgáltatásban, a kapcsolat átirányításához a szolgáltatások székhelye az Európai Unió adatközponthoz. Ebben a részben a forgalom Manager globális betöltése terheléselosztó segít a teljesítmény javítása érdekében, mivel a Kapcsolódás a legközelebbi adatközponthoz gyorsabban csatlakoztatása az Adatközpont esetén, amelyek távol.

Az elérhetőség oldalon globális terheléselosztás azt ellenőrzi, hogy a szolgáltatás érhető el, ha egy teljes adatközponthoz elérhető legyen.

Például legyen-e az Azure adatközponthoz környezeti okok miatt vagy kimaradások (például területi hálózati hibák) miatt nem érhető el, ha a kapcsolatok volna kell átirányítva a következőre a legközelebbi online adatközponthoz. A globális terheléselosztás van megvalósítani, amelyek a forgalom Manager hozhat létre DNS-házirendek nyújtotta előnyök kihasználása.

Azt javasoljuk, hogy felhő megoldások fejleszt körben elosztott hatóköre keresztül több területre, és a legmagasabb szintű lehetséges üzemidőt igényel-et a forgalom Manager.

Ha többet szeretne megtudni az Azure forgalom Manager és a azt telepítéséről, olvassa el a cikk [forgalom Manager](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Azure virtuális gépeken futó RDP/SSH elérésének letiltása
Érdemes lehet elérni az Azure virtuális gépeken futó [Távoli asztali Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) és a [Biztonságos rendszerhéj](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protokollok használatával. Ezek a protokollok virtuális gépeken futó kezelése távoli helyekről lehetővé teszik, és az Adatközpont számítások szabványos.

A potenciális biztonsági problémát ezek a protokollok használatával az interneten keresztül támadók való hozzáféréshez Azure virtuális gépeken futó használható különböző [ellen](https://en.wikipedia.org/wiki/Brute-force_attack) technikákat alkalmaz. A támadók hozzáférhet, amikor a virtuális gép használata indítási pontként betörjön más gépek a Azure virtuális hálózaton, vagy akár szemben az Azure-Ön kívüli hálózati eszközök.

Emiatt azt javasoljuk, hogy tiltsa le közvetlen RDP és SSH hozzáférést szeretne a Azure virtuális gépeken futó az internetről. Az internetről RDP és SSH közvetlen hozzáférés letiltása után más Távfelügyelet az alábbi virtuális gépeken futó eléréséhez használt is lehetőség van:

- Webhely-pont VPN
- Webhely VPN
- Készült ExpressRoute

[Pont-pont VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) egy távoli eléréséhez VPN ügyfél/kiszolgáló közötti kapcsolat másik kifejezés. Egy webhely-pont VPN lehetővé teszi, hogy egy felhasználó az interneten keresztül csatlakozhat az Azure virtuális hálózat. Miután létrejött a webhely-pont kapcsolat, a felhasználó fogja tudni csatlakozhat RDP vagy SSH bármely virtuális gépeken futó pont-webhely VPN keresztül csatlakozik a felhasználó Azure virtuális hálózaton. Ez azt feltételezi, hogy a felhasználó jogosult elérni ezeket virtuális gépeken futó.

Webhely-pont VPN a felhasználónak kétszer a virtuális gépen való csatlakozás előtt azonosítania oka nagyobb biztonságot közvetlen RDP vagy SSH kapcsolatok. Első lépésként hitelesítést végezni (és engedélyezett) kell-e a felhasználó a pont-webhely virtuális Magánhálózati kapcsolat; második, a felhasználónak kell hitelesítést végezni (és engedélyezhető) SSH, vagy RDP-munkamenet.

A [webhely VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) egy teljes hálózat egy másik hálózathoz csatlakozik az interneten keresztül. A helyszíni hálózaton csatlakozhat az Azure virtuális hálózati hely közötti VPN is használhatja. Egy webhely VPN, a felhasználók a üzembe helyez a helyszíni hálózaton fogja tudni a webhely VPN-kapcsolaton keresztül RDP vagy SSH protokoll használatával csatlakozhat virtuális gépeken futó a Azure virtuális hálózaton, és nem szükséges közvetlen RDP vagy SSH hozzáférés engedélyezése az internetről.

Egy dedikált nagytávolságú segítségével a webhely VPN hasonló funkciókat nyújtson. A főbb különbségek 1. a kijelölt nagytávolságú nem bejárása, az interneten, és a 2. dedikált WAN-kapcsolatok jellemzően további stabil és performant. Azure dedikált nagytávolságú megoldást [készült ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/)formájában biztosít.

## <a name="enable-azure-security-center"></a>Azure-biztonság otthon engedélyezése
Azure biztonság otthon segítségével megakadályozhatja, hogy, észleli és veszélyek válaszolni, és itt növeli a betekintést kap abba, és szabályozni kell, az Azure erőforrások biztonsága. Integrált adatvédelem figyelése és a Csoportházirend kezelése az Azure-előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

Azure biztonság otthon segítségével optimalizálása, és figyelemmel követheti a hálózati biztonsági szerint:

- Hálózati biztonsági javaslatok kezeléséről
- A hálózati biztonsági beállítások állapotának ellenőrzése
- Jelenthet alapú hálózati veszélyek mindkét végpont és a hálózaton szinten

Azt javasoljuk, hogy engedélyezze az Azure biztonság otthon összes az Azure telepítések.

Ha többet szeretne tudni az Azure biztonság otthon, és hogy miként kapcsolhatja be a telepítési, olvassa el a cikk [Azure biztonság otthon – bevezetés](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Biztonságos kiterjeszti az Adatközpont Azure
Sok vállalati informatikai szervezetek keresi az a felhő helyett a helyszíni adatközpontokkal növő kibontásához. A kiterjesztett meglévő informatikai infrastruktúrát kiterjesztése jelöl, be a nyilvános felhőben. Idegen helyszíni kihasználva csatlakozási beállítások ajánlatos az Azure virtuális hálózatokat tekinti a helyszíni hálózati infrastruktúrát csak egy másik alhálózat.

Azonban kell először megoldást tervezése és kialakítása problémák sok van. Ez akkor különösen fontos hálózati biztonsági területén. A legjobb módszerek az megértéséhez, hogy hogyan megközelíthető-e ilyen látványterv egyik példát.

A Microsoft a [Adatközponthoz bővítmény hivatkozás architektúra ábrája](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) és a biztosítékot támogató útmutatóból megtudhatja, hogy milyen lenne ki egy adatközponthoz meghosszabbítás hozott létre. Ezzel a megoldással egy példa hivatkozás alkalmazására, amely egy biztonságos vállalati adatközponthoz kiterjesztése a felhőbe megtervezése is használhatja. Azt javasoljuk, hogy tekintse át a dokumentum első arról, biztonságos megoldást fő összetevői.

Biztonságos kiterjeszti az Adatközpont Azure kapcsolatos további információért tekintse meg a videó [A adatközponthoz bővítése a Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
