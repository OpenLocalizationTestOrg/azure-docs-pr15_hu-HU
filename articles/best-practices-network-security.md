<properties
   pageTitle="Microsoft cloud services és a hálózati biztonsági |} Microsoft Azure"
   description="Ismerje meg, hogy néhány főbb rendelkezésre álló szolgáltatások támaszkodva hoznak létre biztonságos hálózati környezetekben Azure-ban"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft cloud services és a hálózati biztonsági

A Microsoft cloud services előadásához hyperscale szolgáltatásokat és a infrastruktúra, a vállalati szintű funkciók és a hibrid kapcsolat számos lehetőségből választhat. Vevők az interneten keresztül, vagy az Azure készült ExpressRoute magánhálózati kapcsolatot biztosít az alábbi szolgáltatások eléréséhez adhatja meg. A Microsoft Azure platform lehetővé teszi a felhasználóknak zökkenőmentes azok infrastruktúra kiterjeszti a felhőben, és többszintű architektúrákban készíthet. Ezenkívül harmadik felek engedélyezheti továbbfejlesztett lehetőségeket nyújtó biztonsági szolgáltatások és virtuális berendezések. A tanulmány áttekintést nyújt a biztonság és építészeti problémák ügyfelek érdemes figyelembe venni, a Microsoft cloud services webböngészőn keresztül készült ExpressRoute használata esetén. Kiterjed is biztonságosabbá szolgáltatások létrehozásának Azure virtuális hálózatokban.

## <a name="fast-start"></a>Gyors megkezdése
A következő összefüggés diagram milyen módokon kérheti fel, hogy egy adott példa az Azure platform elérhető számos biztonsági technikákat. Gyorsan elérhető referenciaként keresse meg a leginkább megfelelő az eset példát. Részletesebb magyarázatokat továbbra is a papír keresztül olvasási.
![Biztonsági beállítások folyamatábrája][0]

[Példa 1: A hálózati biztonsági csoportokat (NSGs) alkalmazások védelme érdekében peremhálózat (más néven DMZ, demilitarizált zóna és árnyékolt alhálózat) hozhat létre.](#example-1-build-a-simple-dmz-with-nsgs)</br>
[2 példa: A tűzfalat és NSGs alkalmazások védelme érdekében peremhálózat összeállítása.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Például: 3: A tűzfalat, a felhasználó által definiált útvonal (UDR) és a NSG hálózatok védelme érdekében peremhálózat összeállítása.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Példa 4: Adja hozzá a hibrid kapcsolatot a webhely, a virtuális készülék virtuális magánhálózat (VPN).](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Példa 5: Adja hozzá a hibrid kapcsolatot a webhely, az Azure az átjárók VPN.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Példa a 6: Adja hozzá a hibrid kapcsolatot a készült ExpressRoute.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Példák a virtuális hálózatok magas rendelkezésre állásának és szolgáltatás láncolás közötti kapcsolatok felvétele a dokumentum a következő néhány hónapokban hozzáadódik.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>A Microsoft megfelelőség és infrastruktúrájának védelme
A Microsoft tett szükséges a vállalati felhasználók megfelelőségi kezdeményezések támogató vezetés módjában. Az alábbiakban néhány a megfelelőségi tanúsítványok az Azure: ![Azure megfelelőségi jelvények][1]

További információ a megfelelőségi információk a [Microsoft adatvédelmi központ](https://azure.microsoft.com/support/trust-center/compliance/).

A Microsoft hyperscale futtatásához a globális szolgáltatásokhoz szükséges felhőalapú infrastruktúrájának védelme átfogó megközelítése tartalmaz. A Microsoft felhőalapú infrastruktúrájának magában foglalja a hardver, a szoftver, a hálózatok, és felügyeleti és műveletek oktatói mellett a fizikai adatközpontokkal.

![Azure biztonsági szolgáltatások][2]

Ez a módszer az ügyfelek számára, azok a a Microsoft cloud services telepítése biztonságosabbá alapot nyújt. A következő lépésként tervezése, és hozzon létre egy biztonsági architektúrája, az alábbi szolgáltatások védelme ügyfelek számára.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Hagyományos biztonsági architektúrákban és szegélyhálózatot
Habár a felhőalapú infrastruktúrájának védelme erősen azoknál a Microsoft, ügyfelek kell is beállíthat védelmet a felhőszolgáltatások és erőforrás-csoportokat. A biztonság többrétegű megközelítés biztosít a legjobb védekezés. A külső hálózat biztonsági zóna belső hálózati erőforrások megakadályozza, egy nem megbízható hálózat. A szegéllyel és a hálózat között az internetes és a védett vállalati informatikai infrastruktúrát ülnie részei peremhálózat hivatkozik.

A vállalati hálózatban az alapvető infrastruktúra erősen dúsítjuk a kialakítását több rétegek biztonsági eszközök elemre. Eszközök és a házirend kényszerítési pontok áll rétegekre szegélyét. Eszközök a következők lehetnek: tűzfalak, elosztott megtagadását szolgáltatás (DDoS) megelőzése, behatolási észlelési vagy védelem rendszerek (Azonosítók/IP-CÍMEI) és a virtuális Magánhálózati eszközök. Házirend-kényszerítés is igénybe vehet a tűzfal házirendek, az access vezérlő vezérlési listákat vagy adott útválasztás formájában. Az első sort a hálózatot, közvetlenül a bejövő forgalom az internetről elfogadása a védelmet további jogos kérések téve a hálózatba e mechanizmusok a továbbfejlesztett fájlblokkolás támadásokkal és a kártékony forgalom kombinációi. A forgalom közvetlenül a peremhálózat erőforrások irányítja. Anyagerőforráshoz, majd "beszél" erőforrásokat a hálózaton, a következő határ adatérvényesítéshez áthaladó mélyebb első. Mivel ez a hálózat részét bizonyos űrlap védelem mindkét oldalon, általában az internethez van téve a legkülső réteg neve a peremhálózat. A következő ábrán látható a vállalati hálózathoz, a két biztonsági határai példa egy alhálózat peremhálózat.

![A vállalati hálózathoz peremhálózat][3]

Vannak olyan sok architektúrákban peremhálózat, a egy egyszerű terheléselosztó elé webes farm, változatosabb mechanizmusok a minden forgalom blokkolása megjelenítése és a vállalati hálózathoz mélyebb rétegei oszlopazonosító ellátott több alhálózat peremhálózat való végrehajtásához használja. Hogyan a peremhálózat épül attól függ, hogy a szervezet és az általános kockázat hibatűrést igényeinek.

Ügyfelek a munkaterhelésekből nyilvános felhőket áthelyezni, mint rendkívül fontos hasonló funkciók támogatása a külső hálózat architektúra megfeleljen a megfelelőség és biztonsági követelményeknek Azure-ban. A dokumentum hogyan ügyfelek az Azure-ban egy biztonságos hálózati környezet nyújthat a útmutatásokat tartalmaz. A peremhálózat koncentrál, de átfogó vitafórum sok hálózati biztonsági szempontjait is tartalmazza. Az alábbi kérdésekre adott vitafórum tájékoztatja:

- Hogyan lehet az Azure peremhálózat épül?
- Melyek a peremhálózat összeállításához Azure funkciót?
- Hogyan lehet a háttéradatbázist munkaterhelésekből védett?
- Hogyan vannak internetes kommunikáció szabályozott a feladatok Azure-ban?
- Hogyan tudja a helyszíni hálózatok védett a telepítések Azure-ban?
- Amikor natív Azure biztonsági funkciók használ, és a külső készülékek vagy szolgáltatások?

Az alábbi ábra mutatja a különféle rétegeket Azure ad az ügyfelek biztonsági. Ezeket a rétegeket az Azure platform magát a natív és a szolgáltatások ügyfél által definiált:

![Azure biztonsági architektúrája][4]

Bejövő az internetről Azure DDoS nagyméretű támadások elleni Azure védi. A következő réteg ügyfél által definiált nyilvános végpontok, amely alapján határozza meg, mely forgalom továbbíthatja a felhőben szolgáltatáson keresztül, a virtuális hálózathoz. Natív Azure virtuális hálózati elkülönítési biztosítja, minden más hálózatokon teljes elkülönítési, valamint, hogy csak forgalmat beállított felhasználói elérési útját és módszerek keresztül. Ezek görbék és módszerek a következő réteget, ahol NSGs UDR és hálózati virtuális készülékek használható az alkalmazás telepítéshez, kattintson a védett hálózati védelme biztonsági határai létrehozásához.

A következő szakaszban az Azure virtuális hálózatok áttekintése. Ezek a virtuális hálózatok felhasználók által létrehozott, és mi a telepített munkaterhelésekből csatlakozik. Virtuális hálózatok az összes hálózati biztonsági funkciói meg kell határoznia az Azure-ban a felhasználói telepítések védelme peremhálózat alapján.

## <a name="overview-of-azure-virtual-networks"></a>Azure virtuális hálózatok áttekintése
Internetes forgalmat elérheti az Azure virtuális hálózatok, mielőtt az alábbi két réteg biztonsági belső az Azure platformra:

1.  **DDoS védelme**: DDoS védelem védi az Azure platform magát nagyméretű internetes támadások Azure fizikai hálózatból réteg. Ezeket a támadásokkal próbál több "bot" csomópontokat használatával olyan internetes szolgáltatás elborít. Azure lévő összes bejövő internetkapcsolat szita robusztus DDoS védelem tartalmaz. A DDoS védelem réteghez nincsenek felhasználói konfigurálható attribútumai, ezért nem érhető el az ügyfélnek. Azure védi megegyezik egy nagyméretű támadások platform, de nem közvetlenül védi egyéni vevő alkalmazás. További rétegek címtárfrissítések a honosított támadások elleni ügyfél által beállítható. Például A felhasználói lett elleni egy nyilvános végpont nagyméretű DDoS homonimaszerű webcímmel és, ha Azure szolgáltatás kapcsolatok letiltja. A felhasználói másik virtuális hálózaton keresztül nem sikerült, vagy a szolgáltatás nem részt vevő a támadások visszaállításához service végpontot. Megjegyzendő, hogy egy ügyfél hatással lehet a végpontra bár nem végpontra kívül más szolgáltatások érint. Ezeken kívül más ügyfelek és szolgáltatások volna lásd: az adott homonimaszerű webcímmel nincs hatással.
2.  **Szolgáltatási végpontok**: végpontok engedélyezése cloud services vagy az erőforrás csoportok, hogy nyilvános Internet IP-címek és a portokra elérhetővé tett. Az endpoint irányítja a forgalmat a belső címet és a Azure virtuális hálózaton portot hálózati cím fordítási-(hálózati Címfordítást) fogja használni. Ez a külső forgalom a virtuális hálózatba való elsődleges elérési útját. A szolgáltatás végpontok, hogy a felhasználó konfigurálható meghatározhatja, hogy mely forgalom átadott, és hol és hogyan akkor van lefordítani a virtuális hálózaton.

Forgalom eléri a virtuális hálózat, ha vannak olyan sok olyan szolgáltatást, play mappába érkező. Azure virtuális hálózatok az ügyfelek számára, azok a feladatok és a hol alkalmazza az alapvető hálózati szintű biztonsági csatolása foundation. Azure magánhálózat (ezek olyan virtuális hálózati felirat) szerepel az ügyfeleknek a következő funkciók, illetve a tulajdonságokat tartalmazó:
 
- **Adatforgalom elkülönítési**: egy virtuális hálózaton a forgalom elkülönítési határ az Azure platform. Virtuális gépeken futó (VMs) egy virtuális hálózati közvetlenül egy másik virtuális hálózaton VMs az nem lehet kommunikálni, még akkor is, ha mindkét virtuális hálózatok azonos ügyfél által létrehozott. A kritikus tulajdonság, amely biztosítja az ügyfél VMs, és a kommunikáció marad Magánjellegű virtuális hálózaton belül.
- **Multitier topológia**: virtuális hálózatok segítségével a felhasználók multitier topológia definiálása alhálózat lefoglalása és különböző elemek vagy a saját feladatok "rétegek" külön címet szóközöket kijelölő. A logikai csoportok topológiák engedélyezése az ügyfelek számára a terhelést alapuló másik access szabályzat és is szabályozhatja, hogy a réteg közötti forgalom.
- **Idegen helyszíni kapcsolat**: ügyfelek hozhat létre virtuális hálózatot, és több a helyszíni webhely vagy más virtuális hálózatok határokon helyszíni összekapcsolását Azure-ban. Ehhez a vevők Azure virtuális Magánhálózati átjárók vagy a külső hálózat virtuális készülékek használható. Azure támogatja a szabványos IPsec/IKE protokollok és készült ExpressRoute magán kapcsolat használata a webhelyen a webhely-(S2S) VPN adatai.
- **NSG** lehetővé teszi a felhasználóknak a (hozzáférés-vezérlési listák) szabályok létrehozásához, engedélyeknek a kívánt szintre: hálózati kapcsolatok, egyéni VMs vagy virtuális alhálózat. Ügyfelek szabályozhatja függvényében vagy megtagadja a közlemény virtuális hálózatból rendszerekből ügyfél hálózaton keresztül határokon helyszíni kapcsolat, a feladatok között, illetve irányítsa át az internetes kommunikáció.
- **UDR** és **IP-továbbítási** segítségével a felhasználók között egy virtuális hálózaton belül különböző rétegek a kapcsolati elérési utak megadása. Ügyfelek telepítheti a tűzfalat, Azonosítók/IP-CÍMEI és egyéb virtuális készülékek, és keresztül biztonsági oszlopazonosító házirend érvénybe lépéséig, Képletvizsgálat és ellenőrzés e biztonsági készülékek hálózati forgalmat.
- A Microsoft Azure piactéren **hálózati virtuális készülékek** : biztonsági készülékek, például tűzfalak, terheléselosztókkal és Azonosítók/IP-CÍMEI érhetők el a Microsoft Azure piactéren és a virtuális képgyűjtemény. Ügyfelek telepítheti e készülékek be a virtuális hálózatok, és konkrétan a biztonsági határai (beleértve a külső hálózat alhálózat) állással biztonságos hálózati környezetbe befejezéséhez.

A szolgáltatások és funkciók hogyan a külső hálózat architektúrája sikerült létrehozni az Azure példája a következőket:

![Az Azure virtuális hálózat peremhálózat][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Külső hálózati jellemzői és követelmények
A peremhálózat a hálózatot, közvetlenül kapcsolatba, az internetről származó kapcsolati eleje lett tervezve. A bejövő csomagokat kell flow keresztül biztonsági készülékek, a tűzfal, az Azonosítók és IP-CÍMEI, például a háttéradatbázist kiszolgálók elérése előtt. A feladatok Internet kötött csomagok is is flow házirend érvénybe lépéséig, ellenőrzési és naplózási céljából, a peremhálózat biztonsági készülékek – a hálózat elhagyása előtt. Emellett a peremhálózat üzemeltetheti az idegen helyszíni VPN átjárók virtuális hálózatok ügyfél és a helyszíni hálózatok között.

### <a name="perimeter-network-characteristics"></a>Külső hálózati jellemzői
Az előző ábra hivatkozik, egy jó peremhálózat jellemzőinek vannak az alábbi képlettel történik:

- Internetes:
    - A külső hálózat alhálózat magát az internetes, közvetlenül kommunikálni az interneten.
    - Nyilvános IP-címei, VIP és/vagy szolgáltatás végpontok átadni internetes forgalmat a hálózati előtér- és eszközök.
    - Az internetről bejövő forgalom áthaladt biztonsági eszközök előtt más erőforrások: az előtér-hálózaton.
    - Ha engedélyezve van a kimenő üzenetek biztonsági, forgalom áthaladt biztonsági eszközök, utolsó lépésként, az internethez átadása előtt.
- Védett hálózati:
    - Nincs közvetlen elérési út az internetről az alapvető infrastruktúra nem.
    - Az alapvető infrastruktúra csatornák kell bejárása biztonsági eszközök, például NSGs, a tűzfal vagy a virtuális Magánhálózati eszközök révén.
    - Egyéb eszközök nem kell egymással, áthidalhatják a internetes és a core infrastruktúra.
    - Az internetes és a védett hálózati szemben lévő határai a peremhálózat (például a két tűzfal ikon az előző ábrán látható) biztonsági eszközök ténylegesen lehet egy egyetlen virtuális a készülék eltérő szabállyal vagy felületek minden határérték. (Ez azt jelenti, hogy egy eszközt, amelyekbe logikailag elválasztott, terhelés kezelésére a peremhálózat mindkét korlátai.)
- Egyéb gyakori eljárások és korlátok:
    - Munkaterhelésekből nem kell tárolni üzleti fontos információkat.
    - Az Access és a külső hálózat konfigurációk és telepítések frissítéseinek korlátozódik csak a meghatalmazott rendszergazdák számára.

### <a name="perimeter-network-requirements"></a>Külső hálózati vonatkozó követelmények
Ahhoz, hogy ezek a jellemzők, kövesse az alábbiakat virtuális hálózati igényeknek megfelelően alakítható sikeres peremhálózat végrehajtásához:

- **Alhálózat architektúra:** Egy teljes alhálózat a peremhálózat választani az egyéb alhálózat virtuális ugyanabba a hálózatba, kitűzött célja, hogy a virtuális hálózat megadása Ezzel biztosíthatja, hogy a forgalmat a peremhálózat és más belső vagy magánjellegű alhálózat rétegek flow tűzfalon vagy Azonosítók/IP-CÍMEI virtuális készüléket a felhasználó által definiált útvonalak alhálózat határán között.
- **NSG:** Kell, hogy nyissa meg az interneten való kommunikáció engedélyezése a külső hálózat alhálózat magát, de nem, amely jelent meg kell a vevők NSGs kihagyásával. Kövesse a hálózati felületek érhető el az interneten minimalizálásához közös biztonsági tanácsokat. A távoli címtartományai férhetnek hozzá a környezetekben vagy bizonyos alkalmazás protokollok és nyitott portokat zárolni. Előfordulhat, hogy körülmények között azonban, amelyben ez nem mindig lehetőség. Például ha a vevők egy külső webhelyen Azure-ban, a peremhálózat engedélyezze a bejövő webes kérelmek a nyilvános IP-címek, de csak nyíljon meg a webes alkalmazás portokat: TCP:80 és TCP:443.
- **Útválasztás táblázat:** A külső hálózat alhálózat magát látnia kell az internethez kommunikáció közvetlenül, de ne engedélyezze, hogy és a hátsó end vagy a helyszíni hálózatok közvetlen közötti kommunikáció nélkül, a tűzfal vagy a biztonsági készülék keresztül.
- **Biztonsági készülék beállításai:** Átirányíthatja, és nézze meg a csomagok között a peremhálózat és a többi a védett hálózatok, például a tűzfalat, Azonosítók és IP-CÍMEI biztonsági készülékek eszközök lehet, hogy több környezetbe áthelyezett. Előfordulhat, hogy rendelkeznek a peremhálózat és a háttér-alhálózat külön NIC. A hálózati kártyákat a peremhálózat kommunikáció közvetlenül és a megfelelő NSGs és a külső hálózat útválasztási táblázat az internetről. A csatlakozás a háttéradatbázist alhálózat NIC további korlátozta NSGs és a megfelelő háttéradatbázist alhálózat útválasztási táblák.
- **Biztonsági készülék funkciókat:** A biztonsági készülékek általában a peremhálózat rendszerbe hajtsa végre a következő funkciók:
    - Tűzfal: Kényszerítése tűzfalszabályokat vagy a hozzáférési szabályok számára a bejövő felkérést.
    - Észlelése és megelőzésére veszélyt: észlelését és az internetről rosszindulatú támadásokkal enyhítő.
    - Naplózás: a naplózáshoz és elemzési részletes naplók karbantartása.
    - Fordított proxykiszolgáló: a bejövő felkérést átirányítása a megfelelő háttéradatbázist kiszolgálókhoz. Ez a magában foglalja a hozzárendelés, és a cél-címek a előtér-eszközökön fordítási általában tűzfalak, a háttéradatbázist kiszolgáló címre.
    - A proxykiszolgáló továbbítása: hálózati Címfordítást kezeléséről, valamint a virtuális hálózaton belül az internethez kezdeményezni kommunikációs naplózásának elvégzéséhez.
    - Útválasztó: Átirányításának bejövő és idegen-alhálózat forgalmat a virtuális hálózaton belül.
    - Virtuális Magánhálózati eszköz: eljáró az idegen helyszíni VPN átjárók határokon helyszíni virtuális Magánhálózati kapcsolat ügyfél helyszíni hálózatok és Azure virtuális hálózatok között.
    - VPN-kiszolgáló: Azure virtuális hálózatok csatlakoztatása VPN-ügyfelek elfogadása.

>[AZURE.TIP] A következő két csoportok külön megtartani: a személyek hitelesítettek arra, hogy a külső hálózat biztonsági fogaskerék és a személyek jogosult alkalmazások fejlesztése, telepítési vagy műveletek rendszergazdaként. Ezek a csoportok külön lehetővé teszi, hogy egy szétválasztására, és megakadályozza, hogy egy személy hagyhassák az alkalmazások biztonsági és a hálózati biztonsági funkciók.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Hálózati határai készítésekor kéri kérdések
Ebben a részben kivéve, ha kifejezetten említett, a "hálózatok" kifejezés Azure virtuális magánhálózat előfizetés rendszergazdák által létrehozott. A kifejezés nem hivatkozik, az alapul szolgáló fizikai hálózatok Azure belül.

Azure virtuális hálózatok is gyakran használják hagyományos helyszíni hálózatok meghosszabbítása. Akkor lehet, hogy végrehajtsa a webhely vagy a készült ExpressRoute hibrid hálózati megoldásokat külső hálózat architektúrákban. Ez a hálózati biztonsági határai létrehozásakor fontos tényező.

Az alábbi három kérdések olyan fontos, hogy egy hálózati peremhálózat és több biztonsági határai szeretne összeállítani választ.

#### <a name="1-how-many-boundaries-are-needed"></a>1) hány határai van szükség?
Az első döntési pont döntse el, hány biztonsági határai adott helyzetben van szükség:

- Egyetlen oszlopazonosító jobboldali: egyet az előtér-peremhálózat a virtuális hálózat és az Internet között.
- Két határai: egy internetes oldalán a peremhálózat, a másik pedig a külső hálózat alhálózat és a háttér-alhálózat az Azure virtuális hálózatok között.
- Három határai: a peremhálózat és a háttér-alhálózat között egy a peremhálózat internetes oldalán, és egy között a háttéradatbázist alhálózat és a helyszíni hálózaton.
- N határai: egy változó számot. Attól függően, hogy a biztonsági követelményeknek nincs igazán tetszőleges számú biztonsági határai alkalmazható egy adott hálózaton.

A szám és a határai típusát a vállalat kockázat hibatűrést és az adott esetben megvalósítandó alapján szükséges változhatnak. Ez a gyakran együtt a kockázat és a megfelelőség csoport, a hálózati és a platform csoport és a-alkalmazás fejlesztőcsapatához szervezeten belül több csoport által végzett közös döntés gyakran. Biztonság, a szóban forgó adatok és a használt technológiák ismerete számára kell egy Ismerkedés a döntési ahhoz, hogy a megfelelő biztonsági hatékonyabb védelem minden végrehajtásához.

>[AZURE.TIP] Használja a legkisebb számú határai, amely egy adott helyzetben biztonsági követelményeknek. A további szegélyek nehéz több műveletek és hibaelhárítási lehet, és a kezelés terhelést felügyeli házirendjeivel a több oszlopazonosító idővel. Azonban nem elegendő határai növeli a kockázat. A fennmaradó keresése fontos.

![Hibrid három biztonsági határai-hálózaton:][6]

A fenti ábrán látható három biztonsági oszlopazonosító hálózati magas szintű nézetének. A határokat a peremhálózat és az interneten, az Azure előtér- és személyes alhálózat és az Azure háttéradatbázist alhálózat és a helyszíni vállalati hálózat között van.

#### <a name="2-where-are-the-boundaries-located"></a>2) hol találhatók a határokat?
Miután határai számát dönt, végrehajtásukhoz hol található a következő döntés. Általában vannak három választási lehetőség:
- Az internetes közvetítő szolgáltatás (például felhőalapú webes alkalmazás tűzfalat, amelyek nem tárgyalja a dokumentum) használatával
- Natív funkciók és/vagy a hálózati virtuális készülékek használatával Azure-ban
- A helyszíni hálózaton fizikai eszközök használata

Pusztán Azure hálózatokon a választható nézetbeállítás a natív Azure szolgáltatások (például Azure terheléselosztókkal) vagy a hálózati virtuális készülékekre gazdag partner ökológiai az Azure (például jelölőnégyzet pont tűzfalak).

Oszlopazonosító jobboldali Azure és a helyszíni hálózaton között van szükség, ha a biztonsági eszközök mindkét oldalán a kapcsolat (vagy mindkét oldalára) találhatók. Így egy kell döntést arra a helyre, biztonsági fogaskerék a helyére.

Az előző ábrán az Internet-a-peremhálózat és az első-a-háttéradatbázis határai teljesen kezelnek Azure, és kell natív Azure-szolgáltatásokat vagy a hálózati virtuális készülékek. Lehet, hogy a biztonsági eszközök közötti Azure (háttéradatbázist alhálózat) és a vállalati hálózat határán vagy Azure oldalán, vagy a helyszíni oldalán, vagy akár mindkét oldalon eszközök kombinációi. Jelentős előnyei és hátrányai komolyan figyelembe kell venni vagy beállítást is lehet.

Tegyük fel például meglévő fizikai biztonsági fogaskerék használata a helyszíni hálózaton oldalon tartalmaz az az előnye, hogy nincs új fogaskerék van szükség. Csak konfigurálás van szüksége. A, azonban hátránya, hogy minden forgalom kell térjen vissza az Azure az a biztonsági fogaskerék láthatja, hogy a helyszíni hálózaton. Így merülnek fel Azure-Azure forgalom sikerült jelentős késés, és a teljesítményt befolyásoló alkalmazás és a felhasználó tapasztalható, ha volt vissza kelljen biztonsági házirendeket alkalmaznak a helyszíni hálózaton.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) módját végrehajtják a határokat?
Minden egyes biztonsági határ valószínűleg lesz különböző képesség követelmények (például Azonosítók és tűzfalszabályokat oldalán lévő Internet peremhálózat, de csak hozzáférés-vezérlési listák a peremhálózat és a háttér-alhálózat között). Mely eszközöket használja annak eldöntésében attól függ, hogy az eset és a biztonsági követelményeknek. A következő szakaszban példák 1, 2 és 3 megtudhatja, hogy bizonyos beállításokat, amelyek felhasználhatók. A rendelkezésre álló gyakorlatilag bármilyen forgatókönyv megoldása számtalan lehetőségek az Azure belső hálózati funkciók és eszközök Azure-ban elérhető a partner ökológiai áttekintése jeleníti meg.

Egy másik kulcs végrehajtása döntési pont, hogy hogyan Azure kapcsolatba a helyszíni hálózaton. Érdemes használni az Azure virtuális átjáró vagy a hálózati virtuális készülék? A beállításokkal részletesebben (4, 5 és 6 példák) a következő szakaszban tárgyalja.

Emellett a forgalom belül Azure virtuális hálózatok között szükség lehet. Ez a helyzet a későbbi időpontban kerül.

Ha már elsajátította az előző kérdésekre adott válaszok, a [Gyorsindítási](#fast-start) szakasz segítségével, hogy mely példák a legmegfelelőbb az adott példa azonosítása.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Példa: Épület biztonsági határai Azure virtuális hálózatokkal
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Példa 1: NSGs alkalmazások védelmet peremhálózat összeállítása
[Vissza az elejéhez gyors](#fast-start) | [részletes útmutatást ebben a példában összeállítása][Example1]

![Bejövő NSG ellátott peremhálózat][7]

#### <a name="environment-description"></a>Környezet leírása
Ebben a példában az előfizetést, amely tartalmazza a következő:

- Két cloud services: "FrontEnd001" és "BackEnd001"
- Virtuális hálózatot, "CorpNetwork", a két alhálózat: "FrontEnd" és "Kódmentes"
- Mindkét alhálózathoz alkalmazott hálózati biztonsági csoport
- A Windows server, amely az alkalmazás webkiszolgáló ("IIS01")
- Két Windows-kiszolgálók megjelenítő háttéradatbázist alkalmazáskiszolgálók ("AppVM01", "AppVM02")
- A Windows server, amely a DNS-kiszolgáló ("DNS01")

Parancsfájlok és egy erőforrás-kezelő Azure-sablon című témakörben talál [részletes Szerkesztés utasításokat][Example1].

#### <a name="nsg-description"></a>NSG leírása
Ebben a példában egy NSG csoport összeállítása és majd betöltött hat szabályokkal.

>[AZURE.TIP] Általánosságban elmondható, célszerű a meghatározott "Engedélyezése" szabályok létrehozása először több általános "Megtagadás" szabályok követ. Az adott prioritás szerint melyiket kezdődni. Miután forgalom kiderül, hogy egy adott szabályt szeretne alkalmazni, nincs további szabályok értékeli ki. NSG szabályok (az alhálózathoz szemszögéből) bejövő és kimenő irányban alkalmazhat.

Deklaratív a következő szabályok éppen készültek bejövő:

1.  Belső DNS-forgalmat (port 53) engedélyezett.
2.  RDP-forgalmat (port 3389) az internetről virtuális gépi engedélyezve van.
3.  HTTP (80-as port) az internetről forgalom webkiszolgálóra (IIS01) engedélyezett.
4.  Bármely (az összes portok) érkező forgalmat IIS01 AppVM1 az engedélyezett.
5.  Minden forgalom (az összes portok) az internetről az egész virtuális hálózat (mindkét alhálózat) van megtagadja.
6.  Bármely (az összes portok) érkező forgalmat az előtér-alhálózat a háttéradatbázist az alhálózathoz van megtagadja.

A következő szabályok kötött minden alhálózathoz, ha a HTTP felkérés volt az internetről az érintett webkiszolgálóra, mindkét szabályok 3 bejövő (lehetővé teszi) és 5 (megtagadja) szeretne alkalmazni. De szabály 3 prioritása magasabb, mert csak szeretne alkalmazni, és 5 szabály nem kellene kerülhet play. Így a HTTP-kérés szeretné tenni az érintett webkiszolgálóra. Ha ugyanazt a forgalmat a DNS01 kiszolgáló elérésére próbált, szabály 5 (megtagadja) az első alkalmazni, és szeretné átadni a kiszolgáló nem engedélyezi a forgalmat. Szabály 6 (megtagadja) akadályozza az előtér-alhálózat a beszélgetésben a háttéradatbázist alhálózat (kivéve az 1 és 4 szabályok engedélyezett forgalom). Ez a háttéradatbázist hálózati védi, abban az esetben, ha a támadó gyengíti a webalkalmazás az előtér. A támadó korlátozott szeretné a háttéradatbázis (csak az erőforrások, a AppVM01 kiszolgálón elérhetővé tett) "védett" hálózat elérését.

Van olyan alapértelmezett kimenő szabályt, amely lehetővé teszi, hogy a forgalmat ki az internethez. Ebben a példában azt is lehetővé teszi a kimenő forgalmának és nem az minden kimenő szabályok módosításával. Mindkét irányba forgalom zárolásához, a felhasználó által definiált útválasztás szükség (lásd: például: 3).

#### <a name="conclusion"></a>Elfogadásáról
A bejövő forgalmat a háttéradatbázist alhálózat elkülönítése viszonylag egyszerű és egyszerű módja: az. További tudnivalókért olvassa el a [részletes Szerkesztés utasításokat][Example1]. Többek között az alábbi lépéseket:

- A PowerShell-parancsfájlokat ellátott peremhálózat létrehozásának módját.
- Hogyan lehet a peremhálózat-Azure erőforrás-kezelő sablonnal össze.
- Minden egyes NSG parancs részletes leírását.
- Részletes forgalom továbbításához esetben megjelenítő arról, hogy a forgalmat hogyan van engedélyezni vagy tiltani a rétegekre.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>2 példa: A tűzfalat és NSGs alkalmazások védelme érdekében peremhálózat összeállítása
[Vissza az elejéhez gyors](#fast-start) | [részletes útmutatást ebben a példában összeállítása][Example2]

![Bejövő peremhálózat NVA és NSG][8]

#### <a name="environment-description"></a>Környezet leírása
Ebben a példában az előfizetést, amely tartalmazza a következő:

- Két cloud services: "FrontEnd001" és "BackEnd001"
- Virtuális hálózatot, "CorpNetwork", a két alhálózat: "FrontEnd" és "Kódmentes"
- Mindkét alhálózathoz alkalmazott hálózati biztonsági csoport
- Ebben az esetben a tűzfal a hálózati virtuális készülék csatlakozik az előtér-alhálózat
- A Windows server, amely az alkalmazás webkiszolgáló ("IIS01")
- Két Windows-kiszolgálók megjelenítő háttéradatbázist alkalmazáskiszolgálók ("AppVM01", "AppVM02")
- A Windows server, amely a DNS-kiszolgáló ("DNS01")

Parancsfájlok és egy erőforrás-kezelő Azure-sablon című témakörben talál [részletes Szerkesztés utasításokat][Example2].

#### <a name="nsg-description"></a>NSG leírása
Ebben a példában egy NSG csoport összeállítása és majd betöltött hat szabályokkal.

>[AZURE.TIP] Általánosságban elmondható, célszerű a meghatározott "Engedélyezése" szabályok létrehozása először több általános "Megtagadás" szabályok követ. Az adott prioritás szerint melyiket kezdődni. Miután forgalom kiderül, hogy egy adott szabályt szeretne alkalmazni, nincs további szabályok értékeli ki. NSG szabályok (az alhálózathoz szemszögéből) bejövő és kimenő irányban alkalmazhat.

Deklaratív a következő szabályok éppen készültek bejövő:

1.  Belső DNS-forgalmat (port 53) engedélyezett.
2.  RDP-forgalmat (port 3389) az internetről virtuális gépi engedélyezve van.
3.  A hálózati virtuális készülék (tűzfal) bármely internetes forgalmat (az összes portok) engedélyezett.
4.  Bármely (az összes portok) érkező forgalmat IIS01 AppVM1 az engedélyezett.
5.  Minden forgalom (az összes portok) az internetről az egész virtuális hálózat (mindkét alhálózat) van megtagadja.
6.  Bármely (az összes portok) érkező forgalmat az előtér-alhálózat a háttéradatbázist az alhálózathoz van megtagadja.

A következő szabályok kötött minden alhálózathoz, ha a HTTP felkérés volt az internetről származó tűzfalat, mindkét szabályok 3 bejövő (lehetővé teszi) és 5 (megtagadja) szeretne alkalmazni. De szabály 3 prioritása magasabb, mert csak szeretne alkalmazni, és 5 szabály nem kellene kerülhet play. Így a HTTP-kérés szeretné tenni a tűzfalon keresztül. Ha ugyanazt a forgalmat a IIS01 kiszolgáló elérésére próbált annak ellenére, hogy be van kapcsolva az előtér-alhálózat, szabály 5 (elutasítása) szeretne alkalmazni, és szeretné átadni a kiszolgáló nem engedélyezi a forgalmat. Szabály 6 (megtagadja) akadályozza az előtér-alhálózat a beszélgetésben a háttéradatbázist alhálózat (kivéve az 1 és 4 szabályok engedélyezett forgalom). Ez a háttéradatbázist hálózati védi, abban az esetben, ha a támadó gyengíti a webalkalmazás az előtér. A támadó korlátozott szeretné a háttéradatbázis (csak az erőforrások, a AppVM01 kiszolgálón elérhetővé tett) "védett" hálózat elérését.

Van olyan alapértelmezett kimenő szabályt, amely lehetővé teszi, hogy a forgalmat ki az internethez. Ebben a példában azt is lehetővé teszi a kimenő forgalmának és nem az minden kimenő szabályok módosításával. Mindkét irányba forgalom zárolásához, a felhasználó által definiált útválasztási szükség (lásd: például: 3).

#### <a name="firewall-rule-description"></a>Tűzfal szabály leírása
A tűzfalat, a továbbítási szabályokban kell létrehozni. Mivel ez a példa csak a forgalom az Internet a kötött a tűzfal és az érintett webkiszolgálóra, hogy a csak egy átirányításának majd hálózati címfordító cím szabály van szükség.

A továbbítási szabály fogadja el a tűzfalon keresztül elérje a HTTP (port 80 és 443-as HTTPS) próbált találatok bármilyen bejövő forrás címet. A tűzfal helyi felületén kívül küldött, és megnyílik a 10.0.1.5 IP-címét, a webkiszolgáló.

#### <a name="conclusion"></a>Elfogadásáról
Ez a meglehetősen egyszerű védelme az alkalmazás a tűzfalat, és a bejövő forgalmat a háttéradatbázist alhálózat elkülönítése lehetőséget. További tudnivalókért olvassa el a [részletes Szerkesztés utasításokat][Example2]. Többek között az alábbi lépéseket:

- A PowerShell-parancsfájlokat ellátott peremhálózat létrehozásának módját.
- Ez a példa egy erőforrás-kezelő Azure sablonnal létrehozásának módját.
- Parancs és a tűzfalat NSG szabályok részletes leírását.
- Részletes forgalom továbbításához esetben megjelenítő arról, hogy a forgalmat hogyan van engedélyezni vagy tiltani a rétegekre.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Például: 3: A tűzfalat, UDR és NSG hálózatok védelme érdekében peremhálózat összeállítása
[Vissza az elejéhez gyors](#fast-start) | [részletes útmutatást ebben a példában összeállítása][Example3]

![Kétirányú peremhálózat NVA, NSG és UDR][9]

#### <a name="environment-description"></a>Környezet leírása
Ebben a példában az előfizetést, amely tartalmazza a következő:

- Három cloud services: "SecSvc001", "FrontEnd001" és "BackEnd001"
- Virtuális hálózatot, "CorpNetwork", a három alhálózat: "SecNet", "FrontEnd" és "Kódmentes"
- Ebben az esetben a tűzfal a hálózati virtuális készülék a SecNet alhálózathoz csatlakoztatott
- A Windows server, amely az alkalmazás webkiszolgáló ("IIS01")
- Két Windows-kiszolgálók megjelenítő háttéradatbázist alkalmazáskiszolgálók ("AppVM01", "AppVM02")
- A Windows server, amely a DNS-kiszolgáló ("DNS01")

Parancsfájlok és egy erőforrás-kezelő Azure-sablon című témakörben talál [részletes Szerkesztés utasításokat][Example3].

#### <a name="udr-description"></a>UDR leírása
Alapértelmezés szerint az alábbi rendszer útvonalak meghatározásuk szerint:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

A VNETLocal értéke mindig az adott hálózat virtuális hálózat megadott cím prefix(es) (Ez azt jelenti, hogy változik virtuális hálózatról virtuális hálózati, attól függően, hogy hogyan minden adott virtuális hálózati van megadva). A hátralévő rendszer útvonalak statikusak, és az alapértelmezett jelzett a táblázatban.

Ebben a példában útválasztási két tábla is hozott létre, minden egyes előtér- és alhálózat lenne. Az adott alhálózat szükséges statikus útvonalakkal betöltött minden táblát. Ebben a példában céljából minden táblának három utakat, hogy minden forgalom (0.0.0.0/0) közvetlenül a tűzfalon keresztül (a következő ugrás = virtuális készülék IP-cím):

1. A következő ugrási helyi alhálózat forgalom definiált a tűzfalon keresztül működjenek helyi alhálózat forgalmának engedélyezésére.
2. Virtuális hálózati forgalmának engedélyezésére, a következő ugrási jelenti tűzfal; Ez a beállítás felülírja az alapértelmezés szerinti szabályt, amely lehetővé teszi a helyi virtuális hálózati forgalmának engedélyezésére átirányítása közvetlenül a.
3. Minden megmaradt forgalmat (0 és 0) a következő ugrási határozható meg, mint a tűzfalon keresztül.

>[AZURE.TIP] Nem rendelkezik a helyi alhálózat bejegyzést a UDR megszakadnak a helyi alhálózat kommunikáció. 
> - Ebben a példában, VNETLocal mutató 10.0.1.0/24 nélkülözhetetlen, minden egyéb esetben pedig csomag elhagyása az érintett webkiszolgálóra (10.0.1.4) egy másik helyi kiszolgálóhoz (például) 10.0.1.25 szánt meghiúsul, ahogy azok fognak megkapni keresztül, a NVA, amely küld azt az alhálózathoz, és az alhálózathoz újra elküldi azt a NVA és így tovább.
> - Hurok valószínűleg több-a hálózati kártya készülékek, amelyeket közvetlenül csatlakoztatott minden alhálózathoz, azok is kommunikál, amely gyakran hagyományos, a helyszíni, készülékek általában magasabb. 

Az útválasztási táblázatok létrehozása után azok a alhálózathoz kötött. Az előtér-alhálózat útválasztási tábla, miután létrehozott, illetve kötött az alhálózathoz, így néz ki:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR most az átjáró alhálózat, amelyen a készült ExpressRoute áramkör csatlakoztatta alkalmazhatók.
>
> Példa a ahhoz, hogy a peremhálózat készült ExpressRoute vagy a hely közötti hálózati jelennek meg a példákban a 3 és 4.


#### <a name="ip-forwarding-description"></a>IP-továbbítási leírása
A kereső UDR szolgáltatás IP-továbbítási. Ez a beállítás egy virtuális készülék, amely lehetővé teszi, hogy kifejezetten nem címezve a készülék adatforgalmat, majd a hálózati forgalmat a végső címzetthez továbbítása a.

Például AppVM01 érkező forgalmat a DNS01 kiszolgáló módosít egy kérelmet, ha UDR volna átirányítása Ez az a tűzfalon keresztül. Az IP-továbbítási engedélyezve a forgalmat a DNS01 cél (10.0.2.4) fogadja el a készülék (10.0.0.4), és kattintson a végső címzetthez (10.0.2.4) továbbítsák. IP-továbbítási engedélyezve van a tűzfalon, nélkül forgalom volna nem fogadja el a készülék annak ellenére, hogy az útvonal táblának a tűzfalat, mint a következő ugrás. A virtuális készüléket használ, fontos, hogy ne feledje, hogy engedélyezze az IP-továbbítási UDR együtt.

#### <a name="nsg-description"></a>NSG leírása
Ebben a példában egy NSG csoport összeállítása és majd betöltött egyetlen szabályok. Ebben a csoportban majd kötött csak az előtér és alhálózat (nem a SecNet). A következő szabályt deklaratív éppen beépített:

- Minden forgalom (az összes portok) az internetről a teljes virtuális hálózathoz (az összes alhálózat) van megtagadja.

Ebben a példában használt NSGs, bár a fő célja, a másodlagos réteg helytelen a manuális konfiguráció ellen védelmet. A cél, az összes bejövő forgalom az internetről az előtér- vagy a háttéradatbázist alhálózathoz blokkolása. Adatforgalom kell csak flow az SecNet alhálózat a tűzfalon keresztül (majd, ha szükséges, megjelenítheti az előtér- vagy a háttéradatbázist alhálózat). Plusz helyen UDR szabályokkal minden forgalom webhelyé fejeződött be az előtér- vagy a háttéradatbázist alhálózat volna meg a irányítja a tűzfal (köszönetnyilvánító UDR). A tűzfal módon jelenik meg egy aszimmetrikus folyamat szerint, és szeretné húzza a kimenő forgalmának engedélyezésére. Így az alábbi három réteg védelme a alhálózat biztonság:
- Nyissa meg a FrontEnd001 és BackEnd001 végpontok cloud services.
- A NSGs forgalom elutasítja az internetről.
- A tűzfal-húzással történő áthelyezéséről aszimmetrikus forgalmat.

Ebben a példában a NSG vonatkozó egy érdekes pont, hogy csak egy szabályt, amely, hogy megtagadja az internetes forgalmat a teljes virtuális hálózatot, ideértve a biztonság alhálózat tartalmazza. Azonban csak a NSG kötött az előtér és alhálózat, mivel a szabály nem feldolgozásának forgalmat a bejövő biztonsági az alhálózathoz. Emiatt forgalom fog flow biztonsági az alhálózathoz.

#### <a name="firewall-rules"></a>Tűzfalszabályokat
A tűzfalat, a továbbítási szabályokban kell létrehozni. Mivel a tűzfal blokkolja a vagy a hívásátirányítás minden bejövő, kimenő és a belüli virtuális hálózati forgalmának engedélyezésére, sok tűzfalszabályokat van szükség. Az összes bejövő forgalmat is, találati a biztonsági szolgáltatása nyilvános IP-címét (a különböző portok), a tűzfal feldolgoztatni. A legjobb a logikai flow, mielőtt beállítaná a alhálózat diagram és elkerülése érdekében a tűzfalszabályokat átdolgozási újabb. Az alábbi ábrán az ebben a példában a tűzfalszabályokat logikai megjelenítése:
 
![A tűzfal szabályok logikai megtekintése][10]

>[AZURE.NOTE] A hálózati virtuális készülék használt alapján, a kezelés portokat változhatnak. Ebben a példában a Barracuda NextGen tűzfal hivatkozott, 22, 801 és 807 használó. Nézzen meg a készülék szállító dokumentációjában pontos kezelése a használt eszköz használható portok.

#### <a name="firewall-rules-description"></a>Tűzfal szabályok leírása
Az előző logikai ábrán a biztonsági alhálózathoz nem jelenik meg. Ennek oka a tűzfal legyen az egyetlen erőforrás adott alhálózat, és ez az ábra, jelenik meg, a tűzfalszabályokat és hogyan logikailag engedélyezése vagy tiltása a forgalom, a tényleges útválasztásos elérési. Is, a külső portokat a RDP-forgalmat, nagyobb kijelölt volt portokat (8014 – 8026), és a helyi IP-címek könnyebb olvashatóság érdekében az utolsó két bájt némileg igazítani kiválasztásának (például helyi kiszolgáló címét 10.0.1.4 társítva külső port 8014). Minden újabb nem ütköző portokat és azonban felhasználható.

Ez a példa a hét típusú szabályok szükséges:

- Külső szabályok (a bejövő forgalom):
  1.    Adatkezelési szabály: Ez alkalmazás átirányítás szabály lehetővé teszi, hogy a forgalmat a hálózati virtuális készülék az adatkezelési portokat át.
  2.    RDP szabályairól (egyes Windows server esetén): (egy adott kiszolgáló) négy szabályok RDP keresztül az egyes kiszolgálók kezelésének engedélyezése. Ez lehet is lehet egyszerre kezelni be egy szabályt, attól függően, hogy a funkciók a hálózat virtuális készülék használatban.
  3.    Alkalmazás forgalom szabályokat: két szabályok, az előtér-webes forgalom az első, és a második a háttéradatbázist forgalmához (például webkiszolgálót adatok réteg). Szabályok konfigurációjától függ, hogy a hálózat architektúrája (ahol a kiszolgálók kerülnek) adatforgalom és a flow (irányát a forgalom, és amelyek portok használt).
      - Az első szabály lehetővé teszi, hogy a tényleges alkalmazás forgalom az application server eléréséhez. Miközben a többi szabály engedélyezi a biztonság és kezelése, az alkalmazás forgalom szabályokat kell, mi engedélyezése külső felhasználóknak vagy a szolgáltatások elérése az alkalmazásokat. Ebben a példában nincs egyetlen webkiszolgálóra 80-as port. Egyetlen tűzfal alkalmazás szabály így webes kiszolgálók belső IP-címét a külső IP-átirányítja a bejövő forgalmat. A belső kiszolgáló hálózati Címfordítást keresztül a forgalmat munkamenet szeretné lefordítani.
      - A második szabály lehetővé teszi az érintett webkiszolgálóra AppVM01 server (de nem AppVM02) felvegye a háttéradatbázist szabályt minden olyan portot található.
- Belső szabályairól (belüli virtuális hálózati forgalmának engedélyezésére)
  4.    Internetes szabály kimenő: Ez a szabály lehetővé teszi, hogy a forgalmat a hálózatról, a kijelölt hálózatok át. Ez a szabály az általában egy alapértelmezett szabályt a már a tűzfalat, de letiltott állapotba kerül. Ez a példa engedélyezni kell a szabályt.
  5.    DNS-szabály: Ez a szabály lehetővé teszi, hogy csak a DNS-kiszolgáló átadni DNS (port 53) forgalmat. A környezet le van tiltva a háttér előtér a legtöbb forgalmat. Ez a szabály kifejezetten lehetővé teszi, hogy DNS bármely helyi alhálózat.
  6.    Alhálózat szabály alhálózat: Ez a szabály azt javasoljuk, hogy bármely kiszolgáló engedélyezése a háttéradatbázist alhálózat az előtér-alhálózat (de nem a Fordított sorrend) bármely kiszolgálóhoz való csatlakozáshoz.
- Biztonságos szabály (forgalmához, amely nem felel meg az előző bármelyikét):
  7.    Minden forgalom szabály tiltása: a végleges szabály (prioritás) számát tekintve mindig meg kell lennie, és ilyen a forgalom folyamat nem felelnek meg, ha az előző szabályok a szabály által a program eltávolítja. Ez egy alapértelmezett szabály, és általában aktiválva van. Módosítás nélkül általában van szükség.

>[AZURE.TIP] A második alkalmazás forgalom szabály egyszerűsítése érdekében ebben a példában, minden olyan portot engedélyezett. Valós esetben a legjobban megfelelő portokkal és címtartományok használandó Ez a szabály homonimaszerű webcímmel felületének csökkentése érdekében.

Az előző szabályok létrehozása után fontos, hogy minden forgalom fog kell engedélyezni vagy tiltani tetszés szerint biztosításához szabály prioritásának áttekintése. Ebben a példában a szabályok elsőbbségi sorrendben vannak.

#### <a name="conclusion"></a>Elfogadásáról
Az összetettebb, de a tökéletes védelme és a hálózat, mint az előző példákban elkülönítése lehetőséget. (Például 2 védi csak az alkalmazás, és csak a elkülöníti alhálózat példa 1). A tervezés lehetővé teszi, hogy mindkét irányban forgalom figyelemmel kísérésére és nem csak a bejövő application server védi, de kényszeríti a hálózat biztonsági házirendje az összes kiszolgálón a hálózaton. Is attól függően, hogy a használt készülék teljes forgalom naplózási és a Jelenléti állapot érhető el. További tudnivalókért olvassa el a [részletes Szerkesztés utasításokat][Example3]. Többek között az alábbi lépéseket:

- A példa peremhálózat a PowerShell-parancsfájlokat létrehozásának módját.
- Ez a példa egy erőforrás-kezelő Azure sablonnal létrehozásának módját.
- Minden egyes UDR, NSG leírása részletes parancs és a tűzfalat szabályt.
- Részletes forgalom továbbításához esetben megjelenítő arról, hogy a forgalmat hogyan van engedélyezni vagy tiltani a rétegekre.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Példa 4: Hibrid kapcsolatot a webhely, a virtuális készülék virtuális magánhálózat (VPN) hozzáadása
[Vissza az elejéhez gyors](#fast-start) |} Részletes szerkesztés elérhető hamarosan

![Peremhálózat NVA csatlakoztatott hibrid hálózattal][11]

#### <a name="environment-description"></a>Környezet leírása
A hálózati virtuális készülék (NVA) használata hibrid networking valamelyik példák 1, 2 vagy 3 ismertetett a peremhálózat típusai, adhatók meg.

Az előző ábrán látható, mint az interneten (webhely-) a virtuális Magánhálózati kapcsolat a helyszíni hálózaton csatlakoztatása az Azure virtuális hálózaton keresztül egy NVA szolgál.

>[AZURE.NOTE] Ha az Azure nyilvános Peering beállítás engedélyezve van az készült ExpressRoute használja, statikus útvonal kell létrehozni. Ez a vállalati internetes meg és nem keresztül készült ExpressRoute WAN NVA VPN IP-címe kell irányítja. A hálózati Címfordítást szükséges a készült ExpressRoute Azure nyilvános Peering funkciót a VPN-munkamenet megszakítható.

Miután a virtuális Magánhálózati helyen, a NVA válik, az összes hálózatok és alhálózat központi központban. A tűzfal továbbítási szabályok határozza meg, mely forgalom engedélyezettek, van lefordítva hálózati Címfordítást keresztül, a rendszer átirányítja vagy megszakadnak (még ha a helyszíni hálózaton és Azure között forgalom).

Forgalom kellene tekinteni gondosan, optimalizálhatók, vagy a tervezés mintát által csökkent attól függően, hogy az adott használati eset.

A környezet, például: 3 beépített használatával, és töltse fel a webhely virtuális Magánhálózati hibrid hálózati kapcsolat hoz létre a következő Tervező:

![A NVA peremhálózat csatlakozik egy webhely VPN használata][12]

A helyszíni útválasztó vagy bármely más hálózati eszköz, hogy kompatibilis-e VPN-, a NVA lenne a VPN-ügyfél. A fizikai eszközök kezdeményezése és a virtuális Magánhálózati kapcsolat a NVA fenntartása felelős lenne.

Logikailag a NVA, hogy a hálózat fog kinézni négy külön "biztonsági zónák" szabályokkal az éppen között a zónák forgalom az elsődleges igazgató NVA:

![Logikai hálózati NVA perspektívából.][13]

#### <a name="conclusion"></a>Elfogadásáról
A felvett webhely VPN hibrid hálózati kapcsolaton Azure virtuális hálózathoz bővíthetik a helyszíni hálózaton az Azure biztonságos módon. A virtuális Magánhálózati kapcsolatot használ, a forgalom titkosítva van, és továbbítja az interneten keresztül. Ebben a példában a NVA hivatkozási, és kezelheti a biztonsági házirend központi helyet biztosít. További tudnivalókért olvassa el a részletes Szerkesztés útmutatót (szolgáltatnak). Többek között az alábbi lépéseket:

- A példa peremhálózat a PowerShell-parancsfájlokat létrehozásának módját.
- Ez a példa egy erőforrás-kezelő Azure sablonnal létrehozásának módját.
- Részletes forgalom folyamat alkalmazási eseteit, hogyan forgalmat a lényegét megjelenítő.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Példa 5: A webhely, az Azure átjáró VPN hibrid kapcsolat hozzáadása
[Vissza az elejéhez gyors](#fast-start) |} Részletes szerkesztés elérhető hamarosan

![Az átjáró hibrid csatlakoztatott hálózati peremhálózat][14]

#### <a name="environment-description"></a>Környezet leírása
Azure virtuális Magánhálózati átjárón hibrid networking bővíthető valamelyik példák 1 vagy 2 ismertetett peremhálózat típusa.

Ahogy a fenti ábrán látható, az interneten (webhely-) a virtuális Magánhálózati kapcsolat a helyszíni hálózaton csatlakoztatása az Azure virtuális hálózaton keresztül az Azure virtuális Magánhálózati átjáró szolgál.

>[AZURE.NOTE] Ha az Azure nyilvános Peering beállítás engedélyezve van az készült ExpressRoute használja, statikus útvonal kell létrehozni. Ez a vállalati internetes meg és nem keresztül készült ExpressRoute WAN NVA VPN IP-címe kell irányítja. A hálózati Címfordítást szükséges a készült ExpressRoute Azure nyilvános Peering funkciót a VPN-munkamenet megszakítható.

A következő ábrán látható a két hálózati szegélyek ezt a beállítást. Az első oldal a NVA és NSGs szabályozhatja a forgalom belüli Azure hálózatok és Azure és az Internet között. A második széle az Azure virtuális Magánhálózati átjáró, amely egy teljesen külön és elszigetelt hálózatát él a helyszíni és Azure között.

Forgalom kellene tekinteni gondosan, optimalizálhatók, vagy a tervezés mintát által csökkent attól függően, hogy az adott használati eset.

A beépített példa 1 környezet használatával, és töltse fel a webhely virtuális Magánhálózati hibrid hálózati kapcsolat hoz létre a következő Tervező:

![A csatlakoztatott-kapcsolaton keresztül csatolt készült ExpressRoute átjáró peremhálózat][15]

#### <a name="conclusion"></a>Elfogadásáról
A felvett webhely VPN hibrid hálózati kapcsolaton Azure virtuális hálózathoz bővíthetik a helyszíni hálózaton az Azure biztonságos módon. A natív Azure virtuális Magánhálózati átjáró használ, a forgalom IPSec titkosítva, és az interneten keresztül irányítja. Emellett az Azure virtuális Magánhálózati átjáró megadhatja az egy alsó költség beállítás (nincs további licencelési külső NVAs a költség). Ez a leggazdaságosabb példában 1, ahol nem NVA használják. További tudnivalókért olvassa el a részletes Szerkesztés útmutatót (szolgáltatnak). Többek között az alábbi lépéseket:

- A példa peremhálózat a PowerShell-parancsfájlokat létrehozásának módját.
- Ez a példa egy erőforrás-kezelő Azure sablonnal létrehozásának módját.
- Részletes forgalom továbbításához alkalmazási eseteit, hogyan forgalmat a lényegét megjelenítő.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Példa 6: Adja hozzá a hibrid kapcsolatot a készült ExpressRoute
[Vissza az elejéhez gyors](#fast-start) |} Részletes szerkesztés elérhető hamarosan

![Az átjáró hibrid csatlakoztatott hálózati peremhálózat][16]

#### <a name="environment-description"></a>Környezet leírása
Hibrid hálózat-kapcsolaton keresztül csatolt készült ExpressRoute magán peering bővíthető valamelyik példák 1 vagy 2 ismertetett peremhálózat típusa.

Ahogy a fenti ábrán látható, a helyszíni és az Azure virtuális hálózat közötti közvetlen kapcsolat készült ExpressRoute magán peering biztosít. Adatforgalom transits csak a service provider és a Microsoft Azure hálózat, soha ne érintse meg az Internet.

>[AZURE.NOTE] Vannak bizonyos korlátozások készült ExpressRoute, dinamikus útválasztás az Azure virtuális átjáró használt bonyolultsága miatt UDR használata esetén. Ezek a következők:
>
>- Az átjáró alhálózathoz, amelyen az készült ExpressRoute csatolt Azure virtuális átjáró csatlakoztatva van UDR nem alkalmazható.
>- Az készült ExpressRoute csatolt Azure virtuális átjáró nem lehet a többi UDR NextHop eszköz kötött alhálózat.
>
>
<br />

>[AZURE.TIP] Készült ExpressRoute használatával továbbra is a vállalati hálózati forgalom elhagyja a biztonság erősítése céljából az internethez, és lényegesen nagyobb teljesítmény. Lehetővé teszi a is szolgáltatás szintű rendelkezést készült ExpressRoute szolgáltatótól. Az Azure átjáró továbbíthatja készült ExpressRoute, legfeljebb 2 Gb/s, mivel a webhely VPN, az Azure átjárók maximális sebesség 200 Mb/s.

A következő ábrán látható, ezt a beállítást választja a környezet most már két hálózati szegélyeket. A NVA és NSG szabályozhatja forgalom belüli Azure hálózatok és Azure és az Internet között, míg az átjáró a helyszíni és Azure között egy teljesen külön és elszigetelt hálózatát él.

Forgalom kellene tekinteni gondosan, optimalizálhatók, vagy a tervezés mintát által csökkent attól függően, hogy az adott használati eset.

A példa 1 beépített környezet használatával, és töltse fel az egy készült ExpressRoute hibrid hálózati kapcsolat hoz létre a következő Tervező:

![A csatlakoztatott-kapcsolaton keresztül csatolt készült ExpressRoute átjáró peremhálózat][17]

#### <a name="conclusion"></a>Elfogadásáról
A felvett egy készült ExpressRoute magán Peering hálózati kapcsolaton is kiterjesztésére a helyszíni hálózaton az Azure magasabb elvégzéséhez módon biztonságos, alsó késés. Is ahogy az ebben a példában a natív Azure átjáró körét egy alsó költség (nem további licencelési külső NVAs együtt) lehetőséget. További tudnivalókért olvassa el a részletes Szerkesztés útmutatót (szolgáltatnak). Többek között az alábbi lépéseket:

- A példa peremhálózat a PowerShell-parancsfájlokat létrehozásának módját.
- Ez a példa egy erőforrás-kezelő Azure sablonnal létrehozásának módját.
- Részletes forgalom folyamat alkalmazási eseteit, hogyan forgalmat a lényegét megjelenítő.

## <a name="references"></a>Hivatkozások
### <a name="helpful-websites-and-documentation"></a>Hasznos webhelyek és a dokumentáció
- Az Access Azure az Azure erőforrás-kezelő:
- A PowerShell Azure elérése: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtuális hálózati dokumentáció: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Biztonsági csoport dokumentáció hálózati: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Felhasználó által definiált útválasztási dokumentáció: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtuális átjárók: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- Webhely VPN adatai: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- (A ne felejtse el, olvassa el az "Első lépések" és "Hogyan a" szakaszok) készült ExpressRoute dokumentáció: [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Biztonsági beállítások folyamatábrája"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure megfelelőségi jelvények"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure biztonsági szolgáltatások"
[3]: ./media/best-practices-network-security/dmzcorporate.png "A DMZ vállalati hálózatban"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Azure biztonsági architektúrája"
[5]: ./media/best-practices-network-security/dmzazure.png "A DMZ-Azure virtuális hálózaton"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hibrid három biztonsági határai-hálózaton:"
[7]: ./media/best-practices-network-security/example1design.png "A NSG bejövő DMZ"
[8]: ./media/best-practices-network-security/example2design.png "Bejövő DMZ NVA és NSG"
[9]: ./media/best-practices-network-security/example3design.png "Kétirányú DMZ NVA, NSG és UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "A tűzfal szabályok logikai megtekintése"
[11]: ./media/best-practices-network-security/example4designoptions.png "A NVA DMZ csatlakoztatott hibrid hálózati"
[12]: ./media/best-practices-network-security/example4designs2s.png "A DMZ a NVA csatlakozik egy webhely VPN használata"
[13]: ./media/best-practices-network-security/example4networklogical.png "Logikai hálózati NVA perspektívából."
[14]: ./media/best-practices-network-security/example5designoptions.png "Azure átjáró DMZ csatlakoztatott hálózati hely közötti hibrid"
[15]: ./media/best-practices-network-security/example5designs2s.png "A DMZ Azure átjáróval VPN webhely használata"
[16]: ./media/best-practices-network-security/example6designoptions.png "Azure átjáró DMZ csatlakoztatott készült ExpressRoute hibrid hálózati"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "Azure,-kapcsolaton keresztül csatolt készült ExpressRoute átjáró DMZ"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
