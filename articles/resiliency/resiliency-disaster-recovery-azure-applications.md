<properties
   pageTitle="Azure-alkalmazásokhoz vészhelyreállítás |} Microsoft Azure"
   description="Technikai áttekintés és a Microsoft Azure vészhelyreállítás alkalmazások tervezésével kapcsolatos részletesebb információkat."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Microsoft Azure-ra épülő alkalmazások vészhelyreállítás

Mivel nagy elérhetősége ideiglenes hiba kezelésével kapcsolatos, a katasztrofális alkalmazás funkcióvesztés az vészhelyreállítás (DR). Például érdemes megvizsgálni az alkalmazási példát, ahol terület megszakad. Ebben az esetben szeretne az alkalmazásnak a futtatására, vagy az Azure régió kívül az adatok eléréséhez csomaggal rendelkezik-e. A csomag végrehajtását magában foglalja a személyek, folyamatok és támogató alkalmazások, amelyek lehetővé teszik a rendszert a függvény. Az üzleti és technológia tulajdonosai, akik a rendszer katasztrófa működési módjának meghatározása is alapján határozza meg a funkciók a szolgáltatás során katasztrófa. A használható funkciók körét a szintet néhány űrlapokat készíthet: teljesen nem érhető el, részben elérhető (funkciók csökkent vagy feldolgozás késleltetett), vagy teljesen érhető el.

##<a name="azure-disaster-recovery-features"></a>Azure katasztrófa helyreállítási szolgáltatásai

Elérhetőség szempontok, az Azure rendelkezik, amely a támogatási vészhelyreállítás [tűrőképessége technikai útmutató](./resiliency-technical-guidance.md) . Kapcsolat áll fenn is Azure és katasztrófa helyreállítási elérhetősége funkciók közül egyesek között. Például szerepkörök hibafa tartományok kezelése növeli az alkalmazás elérhetőségét. Hiba esetén nem kezelt hardver anélkül, hogy a kezelés válna egy "visszaállításához összeomlást követő" során. A megfelelő alkalmazás elérhetősége elérhető szolgáltatásokat és a stratégiák része egy fontos katasztrófa nyelvi ellenőrző, az alkalmazás. Ez a cikk azonban túl általános elérhetősége problémákat mutat, katasztrófa események további komoly (és parancsa).

##<a name="multiple-datacenter-regions"></a>Több adatközponthoz területek

Azure világszerte sok régióban adatközpontokkal kezeli. Ez az infrastruktúra számos katasztrófa helyreállítási lehetőséget, például a rendszer által biztosított geo-replikáció Azure tárolási másodlagos régiók támogat. Azt is jelenti, hogy könnyen és olcsón terjeszthet egy felhőalapú szolgáltatásba több helyre a világon. Ez a költség és megnehezítheti a saját adatközpontokkal futó több régióban összehasonlítása. Adatok és szolgáltatások telepítésével több területre védi az alkalmazás a fő kimaradások egyetlen területen.

##<a name="azure-traffic-manager"></a>Azure forgalom Manager

Területhez kötött hiba történik, amikor szolgáltatások vagy egy másik tartományban lévő telepítések kell átirányíthatja a forgalmat. A továbbítás manuálisan végezheti el, de hatékonyabb lehet az automatizált eljárással. Ebben a feladatban készült Azure forgalom Manager. Abban az esetben, ha az elsődleges régió nem sikerült automatikusan kezelheti a felhasználó forgalom egy másik területére feladatátvételének felhasználhatja azt. Adatforgalom-kezelés az általános stratégia fontos része, mivel fontos a forgalom Manager alapvető megértéséhez.

Az alábbi ábrán a felhasználók csatlakozhatnak a forgalom Manager (__http://myATMURL.trafficmanager.net__) és a kivonat adta meg az aktuális webhely URL-címeit, (__http://app1URL.cloudapp.net__ és __http://app2URL.cloudapp.net__) URL-cím. Útvonal számára a feltételei beállításaitól függően felhasználók küld a megfelelő tényleges webhelyet, ha a házirend szabja meg. Házirend-beállítások ciklikus, feladatátvevő és működésére. Az Ez a cikk azt lesz érintett csak a feladatátvevő lehetőség.

![Továbbítás Azure forgalom Manager keresztül](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Forgalom Manager konfigurálásakor esetén adja meg az új forgalom Manager DNS előtagot. Ez az URL-cím előtag, hogy meg kell adnia a felhasználók a szolgáltatás eléréséhez. Adatforgalom Manager most abstracts terheléselosztási egy szinttel feljebb és nem a régió szintjén. A forgalom Manager DNS kezeli az összes telepítésekhez CNAME rendeli hozzá.

Belül forgalom-kezelő adhatja meg, hogy a felhasználók továbbítja a hiba akkor fordul elő, amikor a telepítések prioritását. Forgalom Manager figyeli végpontjait a telepítések és a jegyzetek, ha az elsődleges telepítési nem sikerül. A hiba a forgalom felettes elemzi a telepítések elsőbbségi listáját, és irányítja a felhasználók a listában lévő következő egy.

Bár a forgalom Manager úgy dönt, hogy hova kell lépnie egy című témakörében, eldöntheti, hogy feladatátvevő tartománya-e inaktív vagy aktív, miközben Ön nem feladatátvevő módban. Ezt a funkciót tartalmaz Azure forgalom Manager műveletek el. Forgalom Manager az elsődleges webhely hibát észlel, és azt a webhelyet, feladatátvevő összesíti. Adatforgalom Manager összesíti, függetlenül attól, hogy, hogy a webhely jelenleg egyszerre szolgál felhasználók-e.

Hogyan működik a Azure forgalom Manager kapcsolatos további tudnivalókért tekintse át:

 * [Adatforgalom szolgáltatásának áttekintése](../traffic-manager/traffic-manager-overview.md)
 * [Feladatátvevő útválasztási mód konfigurálása](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Azure katasztrófa felhasználási területei

Az alábbi szakaszok katasztrófa esetek számos különböző típusú terjed ki. Régió szintű szolgáltatás megszakadása sem csak alkalmazásszintű hibák okát. Gyenge Tervező vagy felügyeleti hibák is vezethet kimaradások. Fontos, hogy a hiba a lehetséges okok fontolja meg a tervezés és helyreállítási csomagja szakaszainak tesztelés során. Egy jó terv Azure szolgáltatások előnyeit, és az alkalmazás-specifikus stratégiák augments őket. A kiválasztott válasz határozza meg az alkalmazást, fontosságát a helyreállítási pont cél (Készletben), és a helyreállítási idő cél (RTO).

###<a name="application-failure"></a>Alkalmazáshiba

Azure forgalom Manager automatikusan kezeli az állomásgép virtuális mögöttes hardver vagy operációs rendszerrel szoftver eredményeként létrejövő hibák. Azure hoz létre egy új példányát a szerepkör a kiszolgálón működik, és hozzáadja őket a terheléselosztó típust. Ha nagyobb, mint egy szerepkör-példányok száma, Azure feldolgozás a többi szerepkör példánya fut közben a hibás csomópont cseréje értékeként.

Vannak, amelyek akkor aktiválódnak, függetlenül a hardver vagy operációs rendszerrel hibák komoly alkalmazáshibák. Az alkalmazás a hibás logika vagy adatintegritási problémák okozta katasztrofális kivételek miatt előfordulhat, hogy nem. Be kell építenie elég telemetriai a kódot be, hogy egy felügyeleti rendszer hiba feltételek észleli, és a rendszergazda alkalmazás értesítést. Olyan rendszergazda, aki rendelkezik teljes ismeretekkel katasztrófa helyreállítási folyamat egy feladatátvételi folyamat meghívásához döntés teheti. Azt is megteheti a rendszergazda is egyszerűen fogadja el a kritikus hibák megoldására az elérhetőség üzemszünetek.

###<a name="data-corruption"></a>Adatsérülésekkel

Azure automatikusan tárolja az SQL Azure-adatbázis és Azure tároló adatok háromszor redundantly különböző hiba tartományok ugyanabban a régióban belül. Ha a replikáció geo használ, az adatok további háromszor vannak tárolva más területére. Azonban a felhasználók és az alkalmazás módosítja az elsődleges másolása az adatokat, ha az adatok gyors replikálja a többi példányt. Sajnos ennek eredményeképpen sérült adatok három példányát.

Kezelheti az adatok potenciális sérült, ha két lehetőség közül választhat. Első lépésként egy egyéni biztonsági stratégia kezelheti. A biztonsági másolatok Azure vagy a helyszíni függően a vállalati követelményeknek vagy irányítási előírásokat tárolhat. Egy másik, hogy az új pont és az idő visszaállítása beállítás használata SQL-adatbázis visszaállítása. További információ című a [vészhelyreállítás adatok stratégiák](#data-strategies-for-disaster-recovery).

###<a name="network-outage"></a>Hálózati üzemszünetek

Ha a Azure hálózat részei nem érhető el, előfordulhat, hogy nem használhat az alkalmazás és az adatok. Egy vagy több szerepkör-példányok az hálózati hibák miatt nem érhetők el, ha a Azure használja az alkalmazás hátralévő elérhető példányát. Ha az alkalmazás nem tud hozzáférni az adatok miatt az Azure hálózati üzemszünetek, esetleg futtathatja csökkentett módban helyileg gyorsítótárban tárolt adatok felhasználásával. Az alkalmazás csökkentett módban fut a katasztrófa helyreállítási stratégia tervezővel szüksége. Az egyes alkalmazások ez nem lehet gyakorlati.

Egy másik, hogy tárolja az adatokat egy másik helyre, amíg a helyreáll a kapcsolat. Csökkentett mód lehetőség nem, a további beállításokat is alkalmazás legrövidebb leállás vagy egy alternatív terület áttérni. A tervezés csökkentett módban futó alkalmazás annyi üzleti döntést technikai egy. A tárgyalt további, a [csökkent alkalmazások](#degraded-application-functionality)szakaszában.

###<a name="failure-of-a-dependent-service"></a>A függő szolgáltatás hiba

Azure sorolunk periodikus legrövidebb leállás sok szolgáltatásokat biztosít. Fontolja meg az [Azure vgx.dll gyorsítótár](https://azure.microsoft.com/services/cache/) példaként. Több elem bérlői szolgáltatást az alkalmazás gyorsítótárazási szolgáltatásokat nyújt. Fontos fontolja meg, mi történik az alkalmazásban, ha a függő szolgáltatás nem érhető el. Számos módon ebben az esetben hasonlít a hálózati üzemszünetek forgatókönyvet. Jó helyen jár, hogy egyes szolgáltatások független eredményez potenciális javított teljes csomagja.

Azure vgx.dll gyorsítótár tartalmaz, az alkalmazás a belül a szolgáltatás előnyökkel jár katasztrófa helyreállítási felhő üzembe gyorsítótárazás. A szolgáltatás első lépésként most már fut a szerepkörök, amelyek a üzembe helyi. Így Ön jobban figyelésére és a gyorsítótár állapotának kezelése a felhőalapú szolgáltatást a teljes adatkezelési folyamatok részeként. A következő típusú gyorsítótárazás is elérhetővé teszi a új szolgáltatásokat. Az új funkciók egyik magas elérhetőségét a gyorsítótárban tárolt adatokat. Ezzel az elemzéssel megőrzése a gyorsítótárban tárolt adatokat, ha egyetlen csomópont nem sikerül egy ismétlődő másolatok többi csomóponton megtartásával.

Ne feledje, hogy magas elérhetősége átviteli kevesebb késés növeli a írások a másodlagos példány frissítése miatt. Azt is, amellyel mindegyik elemhez memória ellátja, úgy, hogy megtervezése. Ez a példa bemutatja, hogy minden függő szolgáltatás lehetnek a teljes elérhetősége és ellenállás az katasztrofális hibák javítása funkcióját.

Az egyes függő szolgáltatás ismernie kell a szolgáltatási zavarok következményeit. Gyorsítótárazási példában esetleg mindaddig, amíg a gyorsítótár visszaállíthatja az adatok eléréséhez közvetlenül egy adatbázisból. Ez a csökkentett módban a teljesítmény szempontjából a következő lesz, de teljes funkcionalitással volna adatok kapcsolatban.

###<a name="region-wide-service-disruption"></a>Régió szintű a szolgáltatási zavarok

Az előző hibák elsősorban lett kezelheti az azonos Azure régión belüli hibák. Jó helyen jár el kell készítenie is, hogy van-e a szolgáltatási zavarok a teljes régió lehetőséget. A terület szintű a szolgáltatási zavarok fordul elő, ha az adatok helyileg felesleges példányainak nem érhetők el. Ha engedélyezte a replikáció geo, vannak BLOB és egy másik régióbeli táblák három további másolatok. Ha Microsoft deklarálása az elveszett régió, Azure amelyek összes a DNS-bejegyzések geo replikált régió.

>[AZURE.NOTE] Ügyeljen arra, hogy nincs ezt a folyamatot minden szabályozható, és csak a körzet szintű a szolgáltatási zavarok tapasztalható is. Emiatt más alkalmazás-specifikus biztonsági stratégiák elérhetőségét a legmagasabb szintű eléréséhez kell támaszkodhat. További információ című a [vészhelyreállítás adatok stratégiák](#data-strategies-for-disaster-recovery).

###<a name="azure-wide-service-disruption"></a>Azure-wide a szolgáltatási zavarok

A katasztrófára, figyelembe kell vennie a lehetséges katasztrófák teljes körét. A legszigorúbb szolgáltatás megszakadása közül szeretne foglalnak magukban összes Azure régiók egyidejű. Akárcsak a többi szolgáltatás megszakadása dönthet, ebben az esetben fogja szánjon ideiglenes legrövidebb leállás kockázata. Széles körű szolgáltatás megszakadása régiók átterjedő sokkal parancsa, mint elszigetelt szolgáltatás megszakadása függő szolgáltatások vagy egyetlen régiók kell lennie.

Azonban egyes kulcsfontosságú-alkalmazásokhoz, előfordulhat, hogy eldöntheti, hogy az ebben az esetben, valamint biztonsági tervet kell. Eseményt tervbe tartalmazhatnak olyan sikertelenül fölé egy [alternatív felhő](#alternative-cloud) és egy [hibrid a helyszíni és felhőbeli megoldás](#hybrid-on-premises-and-cloud-solution)szolgáltatásokat.

###<a name="degraded-application-functionality"></a>Csökkentett alkalmazás működésének

Egy jól megtervezett alkalmazás általában használ, amelyek egymással bár módszerektől összekapcsolt adatcsere mintázatok végrehajtásának modulok gyűjteménye. Egy DR környezetbarát alkalmazáshoz a modul szintjén tevékenységek szétválasztása. Ez a megakadályozhatja, hogy az állásidőt azzal egy függő szolgáltatás leállítása a teljes alkalmazás. Például érdemes megvizsgálni kereskedelmi webalkalmazás vállalat y. A következő modulokat jelenthetnek az alkalmazást:

 * __Termékkatalógus__ lehetővé teszi a felhasználóknak tallózással keresse meg a termékek.
 * __Bevásárlókocsiba__ lehetővé teszi a felhasználóknak a bevásárlókocsi termékek hozzáadása és eltávolítása.
 * __Rendelés állapot__ oszlopban a felhasználó rendelések szállítási állapotát.
 * __Rendelés Beküldési__ a vásárlási munkamenet finalizes a rendelés törlesztőrészlet útján.
 * __Rendelések feldolgozása__ adatintegritás sorrendjének ellenőrzi, és a mennyiség elérhetősége ellenőrzi.

Amikor az alkalmazás a modul egy függő megszakad, hogyan a modul működik mindaddig, amíg az adott rész helyreállítása? Egy jól szervezett rendszer megvalósítása elkülönítési határai tervezéskor mind futásidőben a Tevékenységek szétválasztása keresztül. Minden hiba, valamint a helyreállítható nem helyreállítható kategorizálásával. Lép, hogy lefelé a modul nem helyreállítható hibákat, de csökkentheti a Helyrehozható hiba alternatívák keresztül. A nagy elérhetősége tárgyalt, elrejtheti a felhasználó problémák kezelő hibáit és alternatív műveletek végrehajtása. Egy további komoly a szolgáltatási zavarok során az alkalmazás nem használhatók teljesen. Azonban egy harmadik, hogy továbbra is a karbantartási csökkentett módban dolgozó felhasználóknak.

Például ha az adatbázist tároló rendelések megszakad, a feldolgozási sorrendben modul nem képes a tranzakciók eladási feldolgozása. Attól függően, hogy a architektúra célszerűbb lehet nehezen vagy megírhatja az alkalmazás, a folytatáshoz sorrendben Beküldési és rendelések feldolgozása részei. Az alkalmazás nem alkalmas kezelni, ebben az esetben, ha a teljes alkalmazás előfordulhat, hogy offline módba lépne.

Azonban azonos ebben az esetben, akkor lehet, hogy a termék adatokat tárolja egy másik helyre. Ebben az esetben a termékkatalógus modul továbbra is használhatók a termékek megtekintéséhez. Csökkentett módban az alkalmazás továbbra is elérhető funkciókat, például a termékkatalógus megtekintése a felhasználók számára elérhető. Más az alkalmazás részei azonban nem érhetők el, például a rendezés vagy a készlet lekérdezések.

Csökkentett üzemmód egy másik változata a teljesítményt, hanem funkciók középre igazítása. Például érdemes megvizsgálni forgatókönyvön, ahol termékkatalógus kerül a gyorsítótárba Azure vgx.dll gyorsítótár keresztül. Gyorsítótár nem érhető el, ha az alkalmazás előfordulhat, hogy közvetlen megnyitásához a kiszolgáló tároló katalógus termékinformációk beolvasásához. De lehet, hogy a hozzáférést a gyorsítótárazott verzió lassabb. Emiatt az alkalmazás teljesítményének teljesítménye csökkent, amíg teljesen helyreáll a gyorsítótárazás szolgáltatás.

Eldöntése, hogy mennyi-alkalmazás továbbra is csökkentett módban függvény üzleti döntés és technikai döntés is. Az alkalmazás kell is döntenie kell, hogyan kell a átmeneti problémák a felhasználók értesítése. Ebben a példában az alkalmazás előfordulhat, hogy engedélyezi a termékek megtekintése, és még bevásárlókocsi fel őket. Jó helyen jár amikor a felhasználó megkísérel a vásárlás, az alkalmazás értesíti a felhasználót, hogy az értékesítési modul átmenetileg nem érhető el. Nem ideális az ügyfél, de egy alkalmazásszintű a szolgáltatási zavarok akadályozza meg.

##<a name="data-strategies-for-disaster-recovery"></a>Adatok stratégiák vészhelyreállítás

Adatok megfelelően kezelése az a nagyon nehéz terület, hogy a megfelelő bármelyik katasztrófa helyreállítási csomagban. Adatok visszaállítása része is a helyreállítási folyamat, amely általában elég időt a legtöbb. Adatok helyreállítás hiba és egységesebb nehéz kihívásokkal hiba után vonhat csökkenés elrendezésű naptárakra lehetőség közül választhat.

A tényezők egyike szükséges a vagy egy példányát az alkalmazás-adatok karbantartása. Az adatok hivatkozás, és egy másodlagos webhelyén tranzakció alapú célra fogja használni. Egy helyszíni beállítása egy drága és hosszadalmas tervezési folyamat több területi katasztrófa helyreállítási stratégia végrehajtásához szükséges. A legtöbb felhő szolgáltatók, beleértve a Azure-kényelmesen, könnyen engedélyezése az alkalmazások több területre telepítését. E régiók földrajzilag oly módon, hogy több területi a szolgáltatási zavarok rendkívül ritka kell van meghatározva. Adatok kezelése különböző régiók vonatkozó stratégia az összes katasztrófa helyreállítási csomagot egyik részt vevő tényezők közül.

Az alábbi szakaszokból megtudhatja, hogy katasztrófa helyreállítási technikákat kapcsolódó adatok biztonsági másolatokat, hivatkozási adatok és tranzakció alapú adatok.

###<a name="backup-and-restore"></a>Biztonsági mentési és visszaállítási

Az alkalmazás adatok rendszeres biztonsági mentés néhány katasztrófa helyreállítási forgatókönyvet támogatja. A különböző tároló erőforrásokat különféle technikákat, amelyekkel szükség.

Az egyszerű, normál és prémium SQL-adatbázis rétegek használatba veheti a pont és az idő visszaállítása az adatbázis visszaállításához. További tudnivalókért lásd: [áttekintése: a felhő üzleti folytonosságot és az adatbázis vészhelyreállítás SQL-adatbázissal](../sql-database/sql-database-business-continuity.md). Egy másik, hogy az aktív Geo-replikáció használata SQL-adatbázis. Ezzel automatikusan replikálja adatbázis módosításainak másodlagos adatbázisok ugyanabban a Azure régióban vagy akár más Azure területére. Ezzel a megoldással egy potenciális ahelyett, hogy néhány a cikkben található további kézi adatok szinkronizálási módszert. További tudnivalókért lásd: [áttekintése: SQL adatbázis aktív Geo replikációs](../sql-database/sql-database-geo-replication-overview.md).

Is használni egy további kézi megközelítés biztonsági mentése és visszaállítása. Az adatbázis Másolás paranccsal hozzon létre egy másolatot az adatbázisról. Ez a parancs szeretne lépni a biztonsági egységessége kell használnia. Az importálás/exportálás szolgáltatás SQL Azure-adatbázis is használhatja. Azure Blob-tárolóban lévő tárolt fájlokhoz BACPAC exportáló adatbázisokat támogatja ezt.

A beépített redundancia Azure tárolási ugyanabban a régióban hoz létre két replikái a biztonságimásolat-fájlt. Azonban fut a biztonsági mentés gyakoriságának határozza meg, hogy a Készletben, amely katasztrófa esetek elveszhetnek adatok mennyiségét. Tegyük fel, biztonsági mentése az órát tetején, és a katasztrófa felső részén az óra két perccel fordul elő. Elveszti 58 perc után az utolsó biztonsági másolat történt adatait. Is és a régió szintű a szolgáltatási zavarok vírusvédelmet, másolja a BACPAC fájlokat egy másik területére. Ha majd ezeket az alternatív terület biztonsági másolatok visszaállítása lehetőséget. További részletekért lásd: [áttekintése: a felhő üzleti folytonosságot és az adatbázis vészhelyreállítás SQL-adatbázissal](../sql-database/sql-database-business-continuity.md).

Azure tárolására kidolgozása saját egyéni biztonsági mentés vagy a számos külső biztonságimásolat-eszközök egyikének használatára. Ne feledje, hogy a legtöbb alkalmazás látványtervek további bonyodalmainak amikor a tároló erőforrásokat hivatkozás egymást. Például érdemes megvizsgálni tartalmazó oszlop blob mutató Azure-tárolóban lévő SQL-adatbázishoz. Ha a biztonsági másolatok nem egyszerre történik, az adatbázis esetleg a mutatót a blob, amely nem készült biztonsági másolat előtt a hibát. Az alkalmazás vagy Vészhelyreállítási terv elkészítése folyamatok kezelése a eltérést tapasztal a helyreállítás kell végrehajtania.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Hivatkozási adatok mintájának vészhelyreállítás

Hivatkozási adatok, amely támogatja az alkalmazás működésének csak olvasható adatokat. A szokásos nincs hatással a gyakran. Bár biztonsági mentés és visszaállítás kezelése a régió szintű szolgáltatás megszakadása módja, a RTO viszonylag hosszú. Egy másodlagos területet az alkalmazás telepítésekor néhány stratégiák javíthatja a RTO hivatkozási adatok számára.

Mivel a hivatkozást az adatok módosítását ritkán, javíthatja a RTO a hivatkozási adatok, a másodlagos régióban állandó másolatának megtartásával. Ez megszünteti a biztonsági mentés visszaállítása katasztrófa esetén szükséges időt. A többszörös-régió katasztrófa helyreállítási követelményeknek, telepítenie kell az alkalmazás és a több régióban együtt hivatkozási adatok. [Hivatkozási adatok mintájának magas elérhetősége](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability)említett, hivatkozási adatok szerepe magát, külső tárhely, illetve kombinációját is telepítheti.

A hivatkozás telepítési adatmodell belül számítási csomópontok implicit módon eleget katasztrófa helyreállítási. Hivatkozási adatok telepítési SQL-adatbázishoz igényel, hogy egyes régiókra beállítaná a hivatkozási adatok másolatát. Az azonos stratégia Azure tároló vonatkozik. Telepíteni kell egy bármely hivatkozási adatok az elsődleges és másodlagos régiók Azure-tárolóban lévő tárolt példányát.

![Elsődleges és másodlagos régiók hivatkozási adatok kiadványba](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

A saját alkalmazás-specifikus biztonsági eljárások az összes adatot, beleértve a hivatkozási adatok be kell állítani. Különböző régiók GEO replikált másolatok csak egy régió szintű a szolgáltatási zavarok használják. Hibaelhárításra elkerülése érdekében a másodlagos régió üzembe az alkalmazás adatok kulcsfontosságú részei. Példa a topológia az [aktív-passzív modell](#active-passive)talál.

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Vészhelyreállítás tranzakció alapú adatok mintájának

A teljes funkcionalitású katasztrófa mód stratégia végrehajtásához a másodlagos régió tranzakció alapú adatok aszinkron replikáció. A gyakorlati idő windows, amelyeken belül a replikáció akkor fordulhat elő, az alkalmazás Készletben jellemzői határozza meg. Előfordulhat, hogy továbbra is visszaállítani a replikáció ablak során az elsődleges régióból megszakadt adatokat. Akkor lehet, hogy is a másodlagos régió később egyesítés.

Az alábbi architektúra példák ötletek feladatátvevő helyzetben tranzakció alapú adatok kezelésében különböző módokon nyújtanak. Fontos tudni, hogy ezek a példák ne legyenek teljes körű. Köztes tárolási helye, például a sorok például az Azure SQL-adatbázis kell cserélni. Előfordulhat, hogy a sorokat, saját maguk Azure-tárhely és a Azure Service Bus sorban várakozó (lásd: [Azure sorban várakozó szolgáltatás Bus sorban várakozó – képest és ellentétben](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Kiszolgálói tárhely célok is eltérőek lehetnek, például az Azure táblákat az SQL-adatbázis helyett. Dolgozó szerepkörök mindezek mellett, a különböző lépéseket közvetítő lehet beszúrni. A fontos tudnivaló a nem pontosan emulálására a következő architektúrákban, de a helyreállítás tranzakció alapú adatok és a kapcsolódó modulok alternatívák különböző szempontok.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Replikációs tranzakció alapú adatok vészhelyreállítás előkészítése

Fontolja meg az Azure tároló sorban várakozó tranzakció alapú adatok tárolására használó alkalmazást. Ebben a csoportban adhatja dolgozó szerepkörök tranzakció alapú adatfeldolgozás leválasztott architektúrákban server-adatbázishoz. A tranzakciók valamilyen ideiglenes gyorsítótárba, ha az előtér-szerepkörök kér, hogy az adatok azonnali lekérdezés használni ehhez. Az adatvesztés hibatűrést szinttől függően előfordulhat, hogy válassza való replikáció a sorokat, az adatbázis vagy az összes tároló erőforrás. Ismétlésekkel csak adatbázist Ha az elsődleges régió megszakad, továbbra is visszaállíthatja az adatokat az sorban várakozó amikor visszatér az elsődleges régió.

Az alábbi ábra mutatja architektúrája, ahol az adatbázishoz a régiók szinkronizálja.

![Replikációs tranzakció alapú adatok vészhelyreállítás előkészítése](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

A legfontosabb beavatkozás igazolására szolgáló eljárás a architektúra végrehajtási a replikáció stratégia területek között. Az Azure SQL-adatok szinkronizálási szolgáltatás lehetővé teszi, hogy ilyen típusú replikáció. A szolgáltatás azonban még előzetes van, és éles környezetben nem ajánlott. További tudnivalókért lásd: [áttekintése: a felhő üzleti folytonosságot és az adatbázis vészhelyreállítás SQL-adatbázissal](../sql-database/sql-database-business-continuity.md). Gyártási alkalmazások egy külső megoldás fektetni vagy a saját replikációs logika létrehozása a kódot. Attól függően, hogy a architektúra a replikáció lehet kétirányú, amely szintén összetettebb.

Az egyik lehetséges végrehajtása tehetik használni, a köztes várólista az előző példában. A dolgozó szerepkört, amely az adatokat, a végleges tároló célhelyre dolgozza fel előfordulhat, hogy a változtatásokat az elsődleges régió és a másodlagos területhez tartozik. Ezek nem trivial feladatokat, és replikációs kódok teljes útmutatást a jelen cikk túlra. A fontos pontot, amely sok az időt, és hogyan az adatok, bizonyos másodlagos régió teszteléséhez célszerű kiemelése. További feldolgozásra és vizsgálata biztosítja, hogy az feladatátvevő és helyreállítási folyamat megfelelően kezelni minden lehetséges adatok következetlenségekre, illetve ismétlődő tranzakciók.

>[AZURE.NOTE] Ez a papír a legtöbb koncentrál platform szolgáltatásként (PaaS). Hibrid alkalmazásokhoz további replikációs és elérhetőség beállítások használata azonban Azure virtuális gépeken futó. Ezeket az alkalmazásokat hibrid (IaaS) szolgáltatás SQL Server tárolni az Azure virtuális gépeken infrastruktúra használni. Az SQL Server, például AlwaysOn rendelkezésre állási csoportok vagy a napló szállítási így hagyományos elérhetőség az alábbi módszerek. Néhány technikák, például AlwaysOn, csak a helyszíni SQL Server-példányok és Azure virtuális gépeken futó között működik. További tudnivalókért olvassa el a [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)című témakört.

####<a name="degraded-application-mode-for-transaction-capture"></a>Tranzakció rögzítési módjának csökkentett alkalmazás

Fontolja meg egy második architektúra működő csökkentett módban. A másodlagos régió, az alkalmazás az összes a funkciókat, például üzleti üzletiintelligencia-, jelentés vagy a kiürítés sorban várakozó inaktiválja. Üzleti követelmények által meghatározott tranzakció alapú munkafolyamatok, csak a legfontosabb típusú elfogadja. A rendszer rögzíti a tranzakciókat, és írja a sorok. A rendszer előfordulhat, hogy halasztani az adatfeldolgozás a szolgáltatási zavarok kezdeti szakaszában. Ha az elsődleges régió, a rendszer a várt időben ablakon belül javítása a dolgozó szerepkörök elsődleges régióban lemerülhetnek a sorokat. Ez az eljárás a szükségtelenné adatbázis egyesítését. Ha az elsődleges régió a szolgáltatási zavarok túl a megengedett ablak kerül, az alkalmazás elindíthatja a sorok feldolgozása.

Ebben az esetben az adatbázist a másodlagos a után az elsődleges aktiválódik lennie egyesített növekményes tranzakció alapú adatokat tartalmazza. Az alábbi ábra mutatja a stratégia ideiglenes tranzakció alapú adatok tárolására, amíg az elsődleges régió helyreáll.

![Tranzakció rögzítési módjának csökkentett alkalmazás](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Adatok kezelési technikái rugalmassá Azure-alkalmazásokhoz további ismertetéséhez című [Failsafe: rugalmassá felhő architektúrákban útmutatást](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Telepítési topológiák vészhelyreállítás

A terület szintű a szolgáltatási zavarok lehetőségét kulcsfontosságú alkalmazások elő kell készítenie. Ehhez a többszörös-régió telepítési stratégia beépítése üzemeltetési tervezés.

Több-régió telepítések IT-pro folyamatok az alkalmazás és a hivatkozási adatok közzététele a másodlagos régió katasztrófa után előfordulhat, hogy járnak. Ha az alkalmazás azonnali feladatátvevő, a telepítési folyamatot előfordulhat, hogy vagy jár, az aktív/passzív beállítása egy aktív beállítása. Az ilyen típusú telepítési az alternatív régióban futó alkalmazás meglévő példánya van. Azure forgalom Manager például útválasztási eszköz terheléselosztó szolgáltatások DNS szintre biztosít. Képes szolgáltatás megszakadása észleli és különböző régiók szükség esetén a felhasználók irányítja.

A sikeres Azure vészhelyreállítás része van architecting, hogy a helyreállítási az elejétől megoldásba. Az a felhő helyreállítása a katasztrófa alatti hibák, amelyek nem érhető el, a hagyományos szolgáltatója a további lehetőségeket kínál. Kifejezetten dinamikus és gyorsan hozzárendelhet erőforrásokat más területére. Emiatt nem fizet sokkal üresjárati erőforrások fennáll hibájának várakozás közben.

Az alábbi szakaszok különböző telepítési topológiák vészhelyreállítás terjed ki. Általában van egy útján megnövelt költség és összetettsége további elérhetőség.

###<a name="single-region-deployment"></a>Egyetlen területi telepítési

Egy egyetlen területi telepítés nem igazán egy katasztrófa helyreállítási topológia, de célja, hogy a más architektúrákban a kontrasztot. Egyetlen területi telepítések gyakoriak alkalmazások Azure-ban. Az ilyen típusú telepítési viszont nem egy komoly versenyző katasztrófa helyreállítási tervhez.

Az alábbi ábra szemlélteti egy egyetlen Azure régióban futó alkalmazást. Azure forgalom Manager és a hiba és frissítés tartományok használatát az alkalmazás a régión belüli elérhetősége növelése

![Egyetlen területi telepítési](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Ebben az esetben célszerű látható, hogy az adatbázis-e az egyetlen hiba pont. Annak ellenére, hogy az Azure replikálja a az adatok belső kópiákkal különböző hiba tartományok, ez az összes azonos régióban fordul elő. Az alkalmazás nem ellen egy Katasztrofális hiba. Ha a régió megszakad, összes hibafa tartományt Nyissa le – beleértve az összes szolgáltatás példányainak és tárolási erőforrásokat.

Az összes, de a legalább kritikus alkalmazások mivel ezek a csomagot, az alkalmazások terjesztése több területek között. Érdemes RTO fontolja meg, és korlátozásokat, figyelembe véve, mely telepítés topológia használandó költség.

Most tekintsünk át egy most adott mintázatok feladatátvevő támogatása a különböző területei között. Ezek a példák az összes két sorterület segítségével a folyamat ismertetik.

###<a name="redeployment-to-a-secondary-azure-region"></a>Másodlagos Azure területére újratelepítés

A másodlagos területére újratelepítés mintát csak az elsődleges régió alkalmazások és az adatbázisok operációs rendszert futtató tartalmaz. A másodlagos régió nem egy automatikusan átveszi van beállítva. Így katasztrófa fordul elő, ha meg kell léptetőnyíl vezérlőelem be az új szolgáltatás a részei. Ide tartoznak egy felhőalapú szolgáltatásba feltöltése Azure-üzembe helyezése a felhőbeli szolgáltatástól, az adatok visszaállítása és módosításáról a DNS-a forgalom átirányítása.

Bár ez több területi beállítások a legtöbb elérhető, rajta a legrosszabb RTO tulajdonságait. A szolgáltatás csomag és az adatbázis biztonsági másolatok ebben a modellben vannak tárolva használatát vagy a helyszíni, vagy a másodlagos régió az Azure Blob tároló példányával. Azonban kell új szolgáltatás telepítése, és visszaállíthatja az adatokat fog megjelenni a művelet előtt. Még ha az adatátvitel biztonsági tárhelyről teljesen automatizálhatja a frissítésjelző be az új adatbázis-környezet fogyaszt sok időt. Adatok áthelyezése a biztonságimásolat-lemez tárhelyről az üres adatbázis másodlagos régió része a legtöbb drága a helyreállítási folyamat. El kell végeznie ezt azonban ahhoz, hogy az új adatbázis-működési állapota, mert azt nem replikált.

A legjobb megközelítés, hogy a szolgáltatás csomagok tárolását Blob-tárolóhoz a másodlagos régióban. A szükségtelenné Azure, vagyis, hogy mi történik, amikor rendszerbe állítják egy helyszíni fejlesztési számítógépről a csomag feltöltése. Gyorsan telepítheti a szolgáltatás csomagok új felhőszolgáltatásokba Blob-tárolóból PowerShell-parancsfájlokat használatával.

Ez a beállítás akkor gyakorlati csak egy nagy RTO elviseli nem kritikus alkalmazások esetében. Például ez működhet olyan alkalmazás, amely lehet néhány órát, de ismét a 24 órán belül kell futnia.

![Másodlagos Azure területére újratelepítés](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Aktív-passzív

Az aktív-passzív mintája, a választási lehetőségek, sok cégek alkalmazást. A minta és a költség viszonylag kis növekedése a RTO fölé a újratelepítés minta javítása biztosít.
Ebben az esetben van ismét egy elsődleges és másodlagos Azure területet. Összes forgalmat kerül az aktív telepítési elsődleges régió. A másodlagos régió jobban készített a vészhelyreállítás, mert a két régió fut az adatbázist. Ezenkívül egy szinkronizálási mechanizmusa van közöttük. Ezt a megközelítést készenléti is magában foglalja a két változatok: a csak adatbázis megközelítés vagy a teljes környezetet alakítana ki a másodlagos régióban.

####<a name="database-only"></a>Csak az adatbázis

Az az aktív-passzív mintát első változat csak az elsődleges régió telepített felhő szolgáltatásalkalmazás tartalmaz. Azonban nem kedvelik-e a újratelepítés mintázat a két régió vannak szinkronizálva az adatbázis tartalmát. (További tudnivalókért lásd: a szakasz [vészhelyreállítás tranzakció alapú adatok mintájának](#transactional-data-pattern-for-disaster-recovery)a.) Ha katasztrófa fordul elő, akkor kevesebb aktiválási követelményeknek. Indítsa el az alkalmazást a másodlagos régióban, a csatlakozási_karakterlánc módosítsa az új adatbázis és a forgalom átirányítása a DNS-bejegyzések módosítása.

A újratelepítés mintát, például kell már tárolt a szolgáltatás csomagok gyorsabb telepítéshez, a másodlagos régióban Azure Blob-tárolóban lévő. A újratelepítés minta eltérően nem merülnek fel, hogy az adatbázis visszaállítása a művelethez a terhelést a legtöbb. Az adatbázis készen áll arra, és folytonos. Ezzel menti a jelentős időmennyiséget, így egy elérhető DR mintázatot. Érdemes emellett a leggyakrabban használt DR mintázatot.

![Csak az aktív-passzív adatbázis](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Teljes kópia

A második változata, az aktív-passzív mintát az elsődleges régió, mind a másodlagos régió kell a teljes telepítés. A telepítési a felhőszolgáltatások és a szinkronizált adatbázis tartalmazza. Jó helyen jár csak az elsődleges régió aktívan kezeli a hálózati kérelmek a felhasználóktól. A másodlagos régió aktívvá válik, csak ha az elsődleges régió a szolgáltatási zavarok. Ebben az esetben minden új hálózati kérés átirányítja a másodlagos területére. Azure forgalom Manager automatikusan kezelheti a feladatátvételi.

Feladatátadás nagyobb, mint az adatbázis-csak változat, mert a szolgáltatás már telepítve van. A minta egy nagyon kicsi RTO biztosít. A másodlagos feladatátvevő régió készen közvetlenül azután, hogy az elsődleges régió hiba kell lennie.

Gyorsabban válaszidő, valamint a minta tartalmaz előre lefoglalása és üzembe helyezése a biztonsági szolgáltatások előnyeit. Nem kell aggódnia amiatt, hogy a régió, a szóköz katasztrófa az új példányok lefoglalása nélkül. Fontos: Ha a másodlagos Azure régió van közelítő kapacitásának: az. Nincs garancia (szolgáltatói szerződés), hogy azonnal fogja tudni bármelyik tartományban lévő új felhőszolgáltatások számos telepítése nem.

Ez a legjobb válasz idő és a modellt, az elsődleges és másodlagos régiókban hasonló skála (szerepkör-példányok száma) kell rendelkeznie. Az előnye ellentétben fizet a nem használt számítási példányok költséges, és ez nem lehet a legtöbb körültekintően pénzügyi választási lehetőségek. Emiatt gyakrabban verzióját szeretné használni egy kicsit kicsinyített felhőszolgáltatások másodlagos régió. Ezután gyorsan átveszi és szükség esetén a másodlagos példányban méretezése. A feladatátvételi folyamat kell automatizálása, úgy, hogy az elsődleges régió nem érhető el, miután aktiválta a attól függően, hogy a terhelést a további előfordulásait. Ez lehet foglalnak egy autoscaling mechanizmusa, például a [virtuális gép skála állítja be](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Az alábbi ábra mutatja a modellt, ha az elsődleges és másodlagos régiók tartalmaz-e az aktív-passzív mintázat egy teljesen telepített felhőszolgáltatásba.

![Aktív-passzív, a teljes replika](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Aktív-aktív

Most, amelyet meg esetén valószínűleg tudja, a mintázatok alakulása: a RTO csökkenő növeli a költségek és összetettsége. Az aktív-aktív megoldás ténylegesen töréspontok a költség való tekintettel tendencia figyelhető.

Az aktív-aktív mintát a felhőszolgáltatások és az adatbázis teljes mértékben telepítését a két régióban. Az aktív-passzív modell eltérően a két régió fogadni felhasználói forgalmat. Ez a beállítás a leggyorsabb helyreállítási időt együttesen. A szolgáltatásokkal már méretezése kezelheti a betöltés az egyes régiókra egy részét. DNS már engedélyezve van a másodlagos régió használni. Annak megállapítása, és a megfelelő terület irányítja a felhasználók hogyan további összetettsége van. Ciklikus ütemezési esetleg. További valószínű, hogy bizonyos felhasználók használna adataik elsődleges példányát helye adott régióban.

Abban az esetben, ha feladatátvételt egyszerűen tiltsa le a DNS az elsődleges területre. Ez a forgalom az összes a másodlagos területére.

Még a modellben létezik néhány változatok. Ha például a következő ábrán egy a modellben hol a az elsődleges régió tulajdonosa a fő másolatot az adatbázisról. A két régióban felhőszolgáltatások elsődleges adatbázis írni. A másodlagos példányban erről a az elsődleges vagy a replikált adatbázist. Ebben a példában a replikáció módon történik.

![Aktív-aktív](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Van egy aktív-aktív architektúrájú az előző ábrán downside. A második régió kell hozzáférést az adatbázishoz, az első régióban, mivel a mesterlapok másolása ott található. Teljesítmény jelentősen kikapcsolása kívüli terület származó adatok elérésekor esik. Idegen-régió adatbázis hívásokat ezeket a hívások a teljesítmény javítása érdekében stratégia kötegelés bizonyos típusú figyelembe. További tudnivalókért lásd: [SQL-adatbázis alkalmazás teljesítmény javítása érdekében kötegelés használatáról](../sql-database/sql-database-use-batching-to-improve-performance.md).

Egy alternatív architektúra előfordulhat, hogy magában foglalja a saját adatbázis közvetlen hozzáférés területenként. A modell kétirányú replikációs bizonyos típusú van szükség az adatbázisok az egyes régiókra szinkronizálása.

Aktív-aktív szerkezetében nincs szükség lehet annyi példányok elsődleges régió, az aktív-passzív mintázat módon. Az aktív-passzív architektúrában elsődleges régió, ha 10 példányok, szükség lehet a csak az aktív-aktív-architektúrában mindegyik régióban 5. A két régió most oszthat meg a betöltés. Ez lehet egy költség megtakarítási keresztül az aktív-passzív mintát akkor, ha egy meleg készenléti passzív régió, a Várakozás a feladatátvevő 10 példányok.

Látja, hogy mindaddig, amíg vissza az elsődleges régió, a másodlagos terület az új felhasználók hirtelen magasra kaphat. Ha 10 000 felhasználó minden-kiszolgálón Ha az elsődleges régió a szolgáltatási zavarok, a másodlagos régió váratlanul rendelkezik 20 000-felhasználók kezelése. A másodlagos régió szabályok figyelése kell észleli a növekvő és a példányok dupla a másodlagos régióban. [Hiba észlelése](#failure-detection)a szakasz kapcsolatban további tájékoztatást.

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hibrid a helyszíni és felhőbeli megoldás

Egy további stratégia vészhelyreállítás azt javasoljuk, hogy a tervezővel egy hibrid alkalmazást futtató a helyszíni és felhőbeli. Attól függően, hogy az alkalmazás az elsődleges régió lehet, hogy bármelyik helyen. Fontolja meg az előző architektúrákban, és egy helyszíni helyként az elsődleges és másodlagos régió Képzelje el.

A hibrid architektúrákban néhány kihívásokkal kapcsolatban vannak. Ez a cikk a legtöbb először a PaaS architektúra mintázatok foglalkozott. Azure-ban tipikus PaaS alkalmazások Azure-specifikus szerkezeteket, például a szerepkörök, a felhőszolgáltatások és a forgalom kezelő támaszkodhat. Az ilyen típusú PaaS alkalmazás helyszíni megoldás létrehozása egy szignifikánsan architektúra lenne szükség. Ez nem lehet management vagy költség perspektíva megoldható.

Azonban a vészhelyreállítás hibrid megoldást a hagyományos architektúrákban a felhőhöz egyszerűen áthelyezett kevesebb kihívásokkal kapcsolatban van. Ez a IaaS használó architektúrákban igaz. IaaS alkalmazások használata a virtuális gépeken futó, beállíthatja, hogy a helyszíni közvetlen egyenértékű funkcióiról a felhőben. Virtuális hálózatok az való csatlakozáskor a felhőben gépeken futó helyszíni hálózati erőforrások is használhatja. Ezzel megnyitja, fel, amelyek nem lehetséges csak PaaS alkalmazásokkal számos oka lehet. Ha például SQL Server kihasználhassa katasztrófa helyreállítási megoldások például AlwaysOn rendelkezésre állási csoportok és az adatbázis-tükrözéshez. A részletekért olvassa [magas rendelkezésre állásának és a Azure virtuális gépeken futó SQL Server-helyreállítás](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

IaaS megoldások is biztosít egy könnyebben elérési utat a helyszíni alkalmazások Azure tartományként a feladatátvevő lehetőséget. Lehet, hogy egy működő alkalmazás meglévő helyszíni területen. Jó helyen jár mi történik, ha nem rendelkezik a források kezelése feladatátvevő földrajzilag külön terület? Előfordulhat, hogy virtuális gépeken futó és virtuális hálózatokat az Azure-ban futó alkalmazás használata mellett dönt. Ebben az esetben folyamatokra, amely a felhőbe adatok szinkronizálása. Az Azure környezetben lesz a másodlagos régió feladatátvevő használható. Az elsődleges régió marad a helyszíni alkalmazást. IaaS architektúrákban és lehetőségeivel kapcsolatos további tudnivalókért lásd: a [virtuális gépeken futó dokumentációt](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Alternatív felhő

Vannak helyzetek, ahol még megbízhatóságát a Microsoft Cloud, előfordulhat, hogy nem felel meg belső megfelelőségi szabályok vagy házirendeket szervezete igénylő. Még a legjobb előkészítése és a tervezés során katasztrófa biztonsági rendszerekhez végrehajtásához tartoznak rövid, ha a globális szolgáltatási zavarok egy felhőalapú szolgáltató.

Hasonlítsa össze az elérhetőségét követelmények a költség és összetettsége feleljen jobb elérhetőség: szeretné. A kockázat elemzést végezhet, és a megoldás RTO és Készletben határozza meg. Ha az alkalmazás bármely legrövidebb leállás nem elviselni, értelme lehet való fontolja meg egy másik felhőalapú megoldás. Kivéve, ha a teljes Internet megszakad, előfordulhat, hogy egy másik felhő megoldást továbbra is érhető el, ha az Azure globálisan elérhetetlenné válik.

A hibrid forgatókönyv a a feladatátvevő telepítések az előző katasztrófa helyreállítási architektúrákban is előfordulhatnak belül egy másik felhőalapú megoldás. Csak a megoldások, amelyek RTO lehetővé teszi, hogy nagyon kevés, ha vannak ilyenek, legrövidebb leállás alternatív felhő DR webhelyek kell használni. Figyelje meg, hogy használó DR webhely kívüli Azure megoldást konfigurálása, fejlesztése, üzembe helyezését és karbantartását több munkához van szükség. Érdemes emellett nehezebben határokon-felhő architektúrákban ajánlott eljárások a végrehajtásához. Bár a felhőben platformokon hasonló magas szintű fogalmak van, az API-k és architektúrákban eltérőek.

Ha meg szeretné osztani a más platformokon között DR, volna értelme, hogy a megoldás tervezése hardverabsztrakciós rétegek tervezővel. Ha így tesz, nem szükséges fejlesztése, és abban az esetben, ha katasztrófa különböző felhő platformokon ugyanazt a kérelmet két különböző verzióinak kezelése. És a hibrid forgatókönyv az Azure virtuális gépeken futó vagy Azure tároló szolgáltatás használata valószínűleg egyszerűbb a ebben az esetben a felhőben-specifikus PaaS látványtervek használatát.

##<a name="automation"></a>Automatizálási

Néhány, a mintázatok, amely csak tárgyalt kell az offline telepítések rövid aktiválása és a rendszer az egyes részek visszaállításához. Automatizálási vagy scripting támogatja az erőforrások igény aktiválása és megoldások gyors telepítése. Ebben a cikkben DR kapcsolatos automatizálási [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)egyenértékű, de a [Szolgáltatás felügyeleti REST API -t](https://msdn.microsoft.com/library/azure/ee460799.aspx) is beállítást.

Parancsfájlok fejlesztése segít, hogy az Azure nem átlátszó kezeli DR részei kezelése. Azt előállító egységes eredmények minden alkalommal, amikor, amely kis méretűre állítása a emberi hiba hozzájuthat a szükséges előnyeit. Az idő, a rendszer és az azt alkotó alakzatok in the midst of katasztrófa újraépítéséhez is DR parancsfájlok problémákat előre definiált csökkenti. Nem kívánt próbálja meg manuálisan tudni, hogyan állíthat vissza a webhely, lefelé, és elveszett pénzt percenként lép.

Miután létrehozta a parancsfájlok, tesztelését többször elejétől a végéig. Miután megbizonyosodott róla a alapvető funkcióit, győződjön meg arról, hogy a [katasztrófa szimulációk](#disaster-simulation)tesztelése. Ezzel az elemzéssel Kihúzás a parancsfájlok vagy folyamatok hibákat.

Automatizálási együtt a legjobb, ha egy PowerShell-parancsfájlokat vagy parancssori kezelőfelületről parancsfájlok Azure vészhelyreállítás tárházából. Jól megjelölése, és az egyszerű mátrix kategorizálása őket. Több személy kezelheti a tárházba és a parancsfájlok verziószámozás kijelöli. Dokumentum mellett paramétereket és a parancsfájlok használatát példák magyarázata őket. Gondoskodjon arról, hogy őrizze meg ez a dokumentáció szinkronizálja a az Azure telepítések is. Ez aláhúzás, egy személy felelős a tár minden részének problémákat célját.

##<a name="failure-detection"></a>Hibaészlelési technológia

Elérhetőség és a helyreállítás problémáit megfelelően kezelendő észleli és hibák diagnosztizálása kell lennie. Fiókkezelőként hajtsa végre a kiszolgáló speciális és telepítési felügyeletet igényel, így gyorsan tudja meg, amikor a rendszer vagy részei váratlanul lefelé. Eszközök, amelyek a tekintse meg a felhőbeli szolgáltatástól és függőségét általános állapotának ellenőrzése végre tud hajtani a munka egy részét. Egy Microsoft eszköze [System Center 2016-ban](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Külső eszközök is megadhatja az ellenőrzési képességeit. A legtöbb felügyeleti megoldások nyomon követheti a fő teljesítménymutatók számláló és a szolgáltatások elérhetőségét.

Habár elengedhetetlen ezeket az eszközöket, ezek nem feleslegessé tervezését és a jelentéskészítő belül egy felhőalapú szolgáltatásba. Kell megfelelően használni kívánja Azure diagnosztika. Egyéni teljesítmény számláló vagy esemény naplóbejegyzések is az általános stratégia részét. Ezzel a megoldással további adatok gyors diagnosztizálása a problémát, és visszaállíthatja a teljes funkciók hibák során. További mértékek, amely az ellenőrző eszközök segítségével alkalmazás állapotáról is tartalmaz. További tudnivalókért lásd: [Azure diagnosztika segítségével, így az Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md). "Állapot modell" átlagos megtervezése ismertetéséhez című [Failsafe: rugalmassá felhő architektúrákban útmutatást](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Katasztrófa szimulációk

Tesztelés szimulációk magában foglalja a létrehozott kis valós életben esetek, amikor a munka padló megfigyelni, hogy a csapattagok reagálásra. Szimulációk is látható, hogyan megoldásokkal hatékonyak a helyreállítási csomagban. Végez szimulációk oly módon, hogy a létrehozott eseteket közben is valós esetek, amikor például kicsit tényleges üzleti nem zavarhatják.

Fontolja meg az alkalmazás elérhetősége problémák manuálisan hasonlóan "kapcsolótáblához" típusú architecting. Például – finom kapcsoló, az elindítani adatbázis az access Kivételek hatóköre egy rendezési modul úgy, hogy hibásan okoz. A hálózati kapcsolat szintjén készíthet hasonló könnyű megközelítésnek az egyéb modulokat.

A szimulációk kiemeli a megfelelő javított problémák megoldásához. Az szimulált jelenik meg teljes egészében vezérelhető kell lennie. Ez azt jelenti, hogy akkor is, ha a helyreállítási terv úgy tűnik, hogy a hibás, visszaállíthatja a helyzet vissza normál bármely jelentős károkozás nélkül. Fontos is, hogy mikor és hogyan lehet végrehajtani a szimulációk gyakorlatok kapcsolatos magasabb szintű management tájékoztat. Ez a terv tartalmaznia kell az idő vagy információforrások, amelyek a szimulációk vizsgálat futása közben válhat rendszer vonatkozó információkat. A Vészhelyreállítási terv elkészítése olyan tesztelése esetén hogy, amikor fontos is határozza meg, hogyan kell mérni sikeres.

Vannak más technikákkal katasztrófa helyreállítási tervek tesztelése is használhatja. Azonban őket többsége egyszerűen megváltoztatott verziók ezeket a munkalapokon. A fő motive Ez a tesztelés mögött ki szeretné számítani hogyan megoldható, és hogyan alkalmazható a helyreállítási csomagot. A részletek holes fel az egyszerű helyreállítási csomagban a fókusz tesztelése vészhelyreállítás.

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a [vészhelyreállítás és a Microsoft Azure-ra épülő alkalmazások elérhetőséget](./resiliency-disaster-recovery-high-availability-azure-applications.md)egy cikksorozat része. Az előző témakör a sorozat [Microsoft Azure-ra épülő alkalmazások magas elérhetőségét](./resiliency-high-availability-azure-applications.md).
