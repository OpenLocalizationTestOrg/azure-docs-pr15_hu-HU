<properties
   pageTitle="Ismertetése microservices |} Microsoft Azure"
   description="Miért olyan microservices megközelítés a felhőben alkalmazások létrehozásába fontosak a modern alkalmazások fejlesztése bejegyzései, és hogyan Azure Service háló cél platformot áttekintése"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="mfussell"/>

# <a name="why-a-microservices-approach-to-building-applications"></a>Miért egy microservices alkalmazások létrehozásába megközelíthető?
Szoftverfejlesztőknek, mint nincs semmilyen az, hogy megítélése szerint hogyan faktoring összetevő részre céljával kapcsolatos. A központi mintákat objektum, tájolását, szoftver vízkivételeket és componentization. Ma a factorization általában osztályok és a megosztott tárak és technológia rétegek közötti kapcsolatok formájában. A többszintű megközelítés általában a háttéradatbázist áruházból, középszintű üzleti logikai funkcióinak és előtér-felhasználói Felületet venni. Az utolsó évek során megváltozott milyen *van* , hogy azt, szoftverfejlesztő, mint vannak létrehozása a felhőben, és a vállalat által vezérelt elosztott alkalmazások.

A módosítását az üzleti igények a következők:

- Építse fel, és a méretezés szolgáltatás működni annak érdekében, hogy elérje a felhasználók új földrajzi régióban (például).
- Gyorsabb kézbesítve funkcióit és képességeit engedélyezni szeretné a vevői igények megválaszolása Agilis módon.
- Továbbfejlesztett erőforrás-kihasználtság költségek csökkentése érdekében.

Ezek az üzleti igények *hogyan* készíthet alkalmazásokat a azt vannak hatással.

Azure-féle megközelítése microservices kapcsolatos további tudnivalókért olvassa el a [Microservices: az a felhő hajtott alkalmazás forradalom](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Egységes microservice Tervező megközelítés összehasonlítása
Összes alkalmazás verzióinformációk. A sikeres alkalmazások által éppen hasznos lehet ahhoz, hogy mások is alapkoncepciójára. Sikertelen az alkalmazások nem is alapkoncepciójára, és végül már nem támogatja. A kérdés válik: mennyi tudja a vonatkozó követelményeket ma, és mi azok lesz a jövőben? Ha például tegyük fel, hogy egy jelentéskészítő alkalmazást, egy részleg hoz létre. Biztos abban, hogy az alkalmazás továbbra is a vállalat hatálya alá, és a jelentések rövid életű lesz. A választási lehetőségek megközelítés eltér, mondjuk videotartalmak kézbesítése ügyfelek millió tízesre szolgáltatás kiépítése. Időnként előfordulhat az első valamit, amit az ajtó meg fogalmat igazolására a vezetői varianciaanalízis, az alkalmazás újabb áttervezett ismeretek. Kis pont túlságosan mérnöki valamit, hogy soha nem használta módja van. A szokásos mérnöki csökkentés. Kézzel a vállalatok, akik az a felhő épület beszélni, az elvárásoknak esetén NÖV és használatát. A probléma oka, hogy a legjobb exponenciális és a skála is járhat. Azt szeretné, engedélyezni szeretné prototípusának gyorsan közben is arra, hogy azt is a jövőbeli sikeres foglalkozik elérési útját. Ez a lean indítási megközelítés: összeállítása, mérésére, ismerje meg, találta.

Során az ügyfél-kiszolgáló korszak azt gondozás alatt az adott technológiák minden réteg használatával többszintű alkalmazások létrehozásába pontosításához. A kifejezés "egységes" alkalmazás felmerült, az alábbi módszerekkel. A felületek között a réteg kell gondozás alatt, és egy további szorosan ahhoz belül minden réteg összetevők közötti használt tervezés. Fejlesztők számára készült és figyelembe venni néhány végrehajtható és DLL-ek egymáshoz kapcsolódó tárak lefordítva osztályok. Előnyökkel ilyen egységes tervezés megközelítés. Gyakran egyszerűbb a tervezése és -összetevők, mivel ezeket a hívásokat gyakran fölé IPC közötti gyorsabb hívások. Ezenkívül Mindenki azt vizsgálja, egyetlen termékre, amely további személyek-erőforrás hatékony kell azt. Mi a hátrányuk, hogy egy szoros kapcsoló és többszintű rétegek eredmények között nem méretezni a az egyes összetevők. Ha kell végrehajtani a javítások és frissítések, akkor várja meg, hogy mások a tesztelés befejezéséhez. Érdemes nehezebben lehet Agilis.

Microservices cím ezek downsides és további szorosan igazítása az előző üzleti követelményeknek, de a is upsides és downsides is. Microservices előnyei, hogy mindegyik egyszerűbb üzleti funkcióit, mely méretezni a felfelé vagy lefelé, a próba, telepítése, és kezelheti önállóan általában ágyaz be. Egy fontos microservice megközelítés előnye, hogy a csoportoknak hajtott több, mint a vállalati verzió felhasználási területei technológiával, amely elősegíti az többszintű megközelítés. A gyakorlatban a kisebb csoportok kidolgozása egy microservice bármely technológiákkal választanak ügyfél példa alapján. Más szóval a szervezet nem kell technikai egységes alkalmazások megőrzéséhez szabványosíthatja. Saját szolgáltatások teheti meg azokat a csapata szakértői alapján mi van ilyesmire lehetőség, és mi az a megoldandó probléma leginkább megfelelő egyéni csoportokat. A gyakorlatban kellene egy sor olyan ajánlott technológiák például egy adott NoSQL tár vagy a webes alkalmazás keretrendszer, célszerű.

Microservices: Mi a hátrányuk kezelése a nagyobb szervezetek külön számát, és további összetett telepítések és verziószámozás kezelése szolgál. A microservices közötti hálózati forgalmat a megfelelő hálózati késések és megnöveli. Sok chatty, részletes szolgáltatások, akkor a teljesítmény nightmare egy receptje. Eszközök megtekintése nélkül függőségeket, akkor nehéz lehet "Lásd:" a teljes rendszerben. Szabványok: Mi ellenőrizze a microservice megközelítés munkahelyi történő kommunikációhoz megegyeznek, és hogy csak a következőkre lesz szüksége, egy szolgáltatásból az alternatív helyett kemény szerződést. Fontos, hogy látható a névjegyek a tervezés, határozza meg, mivel a szolgáltatások frissítése egymástól függetlenül. Egy másik leírás szereznek tervezése és a microservices megközelítés nem "külön SOA."


***A legegyszerűbb a microservices Tervező megközelítés az olyan szolgáltatásokat, minden egyes független módosításait, és egyezményes előírásoknak kommunikációs leválasztott szövetségének.***


További felhő alkalmazások előállítása, mint a személyek Fedezze fel, hogy a dekompozíciós, az általános alkalmazás független, a forgatókönyv csak szolgáltatások be-e a hosszú távú jobb megoldást jelenthet.

## <a name="comparison-between-application-development-approaches"></a>Alkalmazás fejlesztési az alábbi módszerek összehasonlítása

![Szolgáltatás háló platform-alkalmazások fejlesztése][Image1]

1. Egy egységes alkalmazás tartomány nyelvspecifikus funkciókat tartalmaz, és a szokásos módon elosztja funkcionális rétegeket, például a webes, üzleti és adatok.

2. Egy egységes alkalmazás klónozhatja, akkor a kiszolgálók/VMs/tárolók több méretarányát.

3. Egy microservice alkalmazás funkciókkal való kisebb szolgáltatások külön összekapcsolt szót.

4. Ki telepítésével az egyes szolgáltatások független, méretezze át ezt a megközelítést létrehozása az alábbi szolgáltatások példányai kiszolgálók/VMs/tárolók keresztül.


Az egy microservice tervezése megközelítés nem az összes projekt esetében egy panacea, de jobban igazítása az üzleti célok ismertetett. A egységes megközelítés kezdődő is elfogadható, ha tudja, hogy később nem lesz a kód átdolgozási microservice látványterv be, ha szükséges lehetőséget. Gyakrabban egy egységes alkalmazás kezdődő és lassan célszerű megszakítani szakaszban, a funkcionális területen kell lennie, méretezhető, vagy Agilis kezdve.

Összefoglalva, a microservice megközelítés, az alkalmazás hány kisebb szolgáltatások írhat.  A szolgáltatások fürt gépek között rendszerbe tárolók futnak. Kisebb csoportok példa koncentrál szolgáltatás fejlesztése és független teszteléséhez verzió, telepítése, és méretezni az egyes szolgáltatásokhoz, hogy az alkalmazás egészének mérete is lehet alapkoncepciójára.

## <a name="what-is-a-microservice"></a>Mi az a microservice?

Különböző meghatározása microservices, és adja meg a saját nézőpontok és definíciók jó erőforrások számát keresése az interneten biztosít. A legtöbb microservices jellemzői azonban körben vannak elfogadott:

- Felhasználói vagy vállalati példa beágyazására. Mi az a problémamegoldó?
- Egy kis mérnöki csoport által fejlesztett.
- Bármely programozási nyelven, és bármelyik keretrendszer használja.
- (Nem kötelező) állapot, amelyek mindkettő független verziószámmal, telepítette, és méretezése és kód állnak.
- Más microservices együttműködhet pontosan meghatározott fejrészek és protokollok fölé.
- Egyedi neve (URL-ek) oldja fel a helyükre.
- Továbbra is egységes és hibák jelenlétében érhető el.

Az alábbi jellemzők is Összefoglalva:

***Kis, egymástól függetlenül verzióval és méretezhető ügyfél fókuszáló szolgáltatásokat, amelyek egymással jól meghatározható felülettel rendelkező szabványos protokollon keresztül Microservice alkalmazások tevődnek össze.***


Az előző szakaszban az első két pontokat hatálya alá tartozó azt, és bontsa ki a most azt és jelezhető, hogy a többiek.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Bármely programozási nyelven, és bármelyik keretrendszer használata
Fejlesztők számára hogy meg kell ingyenes bármilyen nyelv vagy keretrendszer szeretnénk, attól függően, hogy a szakértelem vagy az egyéni igényeknek a szolgáltatás kiválasztása. Bizonyos szolgáltatások az mindenekelőtt egyéb C++ teljesítmény előnyei előfordulhat, hogy érték. Más szolgáltatások a C# vagy Java felügyelt fejlesztési Kezeléstechnikai legfontosabb lehet. Egyes esetekben előfordulhat egy adott külső tár, adatokat tároló technológia és az azt jelenti az ügyfelek számára a szolgáltatás ki.

Miután kiválasztotta a technológia, akkor jár műveleti vagy az életciklusuk kezelését, és a méretezés a szolgáltatás.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Lehetővé teszi, hogy kódot és állapot kell független verziószámmal, telepítette, és méretezése  

Azonban úgy dönt, hogy írja be a microservices, a kód és tetszőlegesen a kell független telepítése, frissítése és méretezése. Az ténylegesen a nehezebb problémák megoldására, mivel rendszereznie kell technológiák választási közül. Méretezési, megértéséhez partíciót (vagy shard) módját a kód és az állapot, akkor nehéz. Használatakor a kód és az állapot külön technológiák (amely a ma általában végezze el), a telepítési parancsfájlokat a microservice kell tudja őket mindkét méretezés kezelésében. Ez akkor is agility és rugalmasság, így tudja frissíteni a microservices részét anélkül, hogy frissíteni az összeset egyszerre.
Visszatérés a egységes microservice megközelítés és egy pillanatra, az alábbi ábra mutatja állam tárolásáról megközelítése közötti különbségeket.

#### <a name="state-storage-between-application-styles"></a>Állapot tároló alkalmazás stílusok között
![Szolgáltatás háló platform állam tárhely][Image2]

***A bal oldalon a egységes megoldás, egy adatbázist, és az adott technológiák rétegek van.***

***A jobb oldalon a microservices megközelítés, az összekapcsolt microservices, ahol a microservice általában adatbázisokban állam különféle technológiák használnak és grafikonon van.***

Egységes megközelítésben a szokásos van az alkalmazás által használt egy adatbázist. Az az előnye, hogy az egyetlen helyet biztosít, így egyszerűen telepítése. Minden összetevő beállíthatja, hogy tárolja az állapotába egy táblát. A csoportok kell lennie, az állapot, amely bonyolulttá különválasztó szigorú. Nincsenek elkerülhetetlenül temptations új oszlop hozzáadása a meglévő ügyfelek tábla, a táblák közötti illesztés, és a tárhely rétegben függések létrehozása. Ez történik, ha Ön nem méretezze át az egyes összetevők. Egyes szolgáltatásokhoz a microservices megközelítés kezeli, és saját állam tárolja. Egyes szolgáltatásokhoz a felelős méretezés kód és a együtt az igényeit a szolgáltatás állapotát. Egy downside, hogy nézetek vagy lekérdezések létrehozása szükség esetén az alkalmazás adatok meg kell lekérdezése elemezve állam tárolja. A szokásos ez megoldása úgy, hogy egy külön microservice, amely hoz létre egy nézetet microservices körében. Ha az adatok több alkalmi lekérdezések végre kell minden microservice vegye figyelembe az adatok írásához a adattárolási offline elemzéséhez szolgáltatás be.

A verziószámozás az egy microservice telepített verziója úgy, hogy több különböző verzióival telepíthető, és egymás mellett futtatható. A verziószámozás az esetekben, ahol a frissítés során nem sikerül egy microservice újabb verzióját, és visszaállíthatja egy korábbi verzióját kell megoldást. A verziószámozás más forgatókönyv A és B-stílus tesztelés, ahol a különböző felhasználók tapasztalható a szolgáltatás különböző verzióira hajt végre. Ha például általában egy adott új funkciók kipróbálása előtt körben közbeni további ügyfelek számára microservice frissítése. Microservices életciklus-kezelése, miután ez most életre us őket közötti kommunikációt.


### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Hogyan kommunikáljon a többi microservices, hogy pontosan meghatározott felületek és protokollok

Ez a témakör, ebben az esetben a kis beavatkozást igényel, mivel az elmúlt 10 év szokások leíró közzétett szolgáltatás készült architektúra teljes körű irodalom. Általánosságban elmondható szolgáltatás kommunikációs használja a többi megközelítés a HTTP és a TCP-protokollok és XML- vagy JSON szerializálási formátum. A felület-szempontjából szól a webhelyen tervezési koncepciója magában foglal. De semmi nem leállítása, használja a bináris protokollok vagy a saját adatformátumok. Nehezebb ideje használata a microservices, amennyiben ezek nyíltan érhető el, hogy mások készíteni.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Egy egyedi nevet (URL-CÍMÉT), oldja fel a helyén van

Ne feledje, hogy hogyan azt megtartása amely arról tájékoztat, hogy a microservice megközelítés például a webes? A webhelyen, például a microservice felderíthetőnek kell címmel rendelkező bárhol fut. Ha meg vannak gondolat gépek, és egy adott microservice melyiket fut, fog menjen hibás gyorsan. Megegyező módon, hogy DNS oldja fel egy adott URL-t egy adott számítógépre az a microservice kell lennie egy egyedi nevet, úgy, hogy az aktuális helyre feltárható. Microservices kell tenni őket a infrastruktúra szolgáltatást futtató azokat a független címmel rendelkező neveket. Ez azt jelenti, hogy van-e egy hogyan telepíti a szolgáltatást, és hogyan kiderül, mivel szükség van a szolgáltatás rendszerleíró közötti kölcsönhatás. Ugyanígy, amikor egy számítógépre nem sikerül a beállításjegyzék szolgáltatás kell mondani, amelyen most már fut a szolgáltatás. Ekkor velünk a következő témakört: címtárfrissítések és következetességét.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Konzisztens és hibák jelenlétében elérhető marad

Váratlan hiba kezelésének az egyik a nagyon nehéz problémák megoldására, különösen elosztott rendszerben. Nagy, a kód, amely azt írja be a fejlesztők szerint kezeli a kivételek, és meg is hol a legtöbb van töltött idő tesztelése. De a probléma bonyolultabb, mint a hibák kezelésének kódírás. Mi történik, ha a számítógépen, amelyen a microservice fut nem sikerül? Nemcsak szükség van-e microservice hiba (nehezen probléma saját maga) feltárása, de is szükség van valami a microservice újraindításához. Egy microservice rugalmassá hibákra, és indítsa újra a gyakran elérhetősége okokból egy másik számítógépen kell. Ez is megtalálható lefelé állapotot mentette a microservice nevében, ahol azt állíthatja ezt az állapotot, és hogy lehet sikeresen indítani. Ez azt jelenti nem kell lennie az (a folyamat újraindul) a számítási címtárfrissítések, valamint a címtárfrissítések, a megye vagy adatok (nem az adatok elvesztését, és az adatok egységes).

Tűrőképessége problémák vannak jóváírása más alkalmazási eseteit, például egy alkalmazása a frissítés során hibák időpontjainak során. A microservice használata a telepítés, a rendszer nem kell állítani. Meg kell majd döntse el, hogy az továbbra is előrelépés újabb verziójával, vagy inkább állítsa vissza a korábbi verzióra megőrzéséhez konzisztens állapotba. Kérdések például, hogy vannak-e elég érhető el, hogy mozgatása előre gépek, és hogy hogyan állíthatja helyre a microservice kell figyelembe kell venni a korábbi verzióival. A rendszerállapot adatait engedélyezni szeretné a következő döntések elhelyezése a microservice ehhez.

### <a name="reports-health-and-diagnostics"></a>Jelentések állapot és diagnosztika

Tűnhet egyértelmű, és gyakran kihagyott van, de nagyon fontos, hogy egy microservice jelentések, az állapot és diagnosztika. Ellenkező esetben nem kis betekintést-műveletek perspektívából. Különböző szolgáltatások független használatával történik a diagnosztikai események és gépi óra dönt táblázatkapcsolat esemény sorrendjének kezelése akkor nehéz. Az adott dokumentumkészletből ugyanúgy, egyezményes protokollok és adatformátumok egy microservice másokkal jelenik meg a bejelentkezési állapot és a diagnosztikai események végződő végül lekérdezése és megtekintéséhez esemény tárban hogyan szabványügyi szükség. Microservices megközelítésben van, amely a különböző csoportokhoz azzal egyetlen naplózás formátumot billentyűt.  Szükség van egy diagnosztikai események megtekintése az alkalmazásban egészének egységes megközelítése.

Állapot eltér diagnosztika. A microservice jelentéskészítés az aktuális állapotát a megfelelő műveletet az állapota. Jó példa működik együtt rendelkezésre álljon való frissítés és a telepítési mechanizmusok. A szolgáltatás jelenleg a egy folyamat összeomlik miatt megsérült vagy a gép indítsa újra a rendszert, de továbbra is működik. Az utolsó lépésként szüksége, hogy ez legyen rosszabb frissít. A legjobb megközelítés nyomozásban először végezze el vagy állíthatja helyre a microservice időt is. Egy microservice állapot eseményeinek us tájékoztatni döntések, és gyakorlatilag azt mondja, segítve ezzel önálló öngyógyító szolgáltatások engedélyezése.

## <a name="service-fabric-as-a-microservices-platform"></a>Szolgáltatás háló, mint egy microservices platform

Azure Service háló kiderült termékek mezőbe szokásos egységes stílussal, amely kézbesítése előadásához szolgáltatások a Microsoft áttérés ki. Az épület és működő, nagyméretű szolgáltatások, például Azure SQL-adatbázisok és DocumentDB, a felület szolgáltatás háló alakú. A platform evolved időbeli további és további szolgáltatások elfogadott azt. Fontosabb, szolgáltatás háló volna bárhol futtatásához: nem csak Azure, de a Windows Server-telepítések önálló is.

***A szolgáltatás háló célja összeállítását, és egy szolgáltatást futtató kemény problémamegoldó és infrastruktúra erőforrásainak hatékony használatát, hogy a csoportoknak a microservices megközelítés üzleti problémák orvosolható.***

Szolgáltatás háló segít a microservices megközelítés az alkalmazások két fő területen biztosít:

- A platform, amely telepítése, frissítése, észleli és indítsa újra a rendszer szolgáltatásokat nyújt a szolgáltatások nem sikerült, SRV felfedezése, állapot kezelése és Lync-állapota. Az alábbi rendszer szolgáltatások engedélyezése az előzőekben microservices jellemzői számos gyakorlatilag.

-  Programozási API-k és keretek, alkalmazások, például microservices létrehozásához: [megbízható szereplők és megbízható szolgáltatásokat](service-fabric-choose-framework.md). Természetesen a microservice összeállításához kódrészleteket lehetőség is használhatja. De az alábbi API-hoz a feladat egyszerűbb tétele és azok integrálása a platform, a mélyebb szinten. Ezzel a módszerrel, például az állapot és diagnosztika információkat kaphat vagy használatba veheti a beépített magas elérhetőségét.

***Szolgáltatás háló agnostic meg, hogyan készíthető el a szolgáltatás, és bármelyik technológia is használhatja. Azt azonban biztosít beépített programozási API-hoz, amelyek megkönnyítik a microservices összeállítása.***

### <a name="are-microservices-right-for-my-application"></a>Azok a microservices jobbra a az alkalmazás?

Esetleg. Azt a tapasztalt volt, hogy, a Microsoft további és további csapatok meg a össze a felhőben business okokból a közülük történjen realizálása véve egy microservice hasonló módon előnyeit. A Bing, például van már megtervezése microservices évek keresés. Egyéb a csoportoknak a microservices megközelítés volt új. Azok találhatók, amelyek volt a merevlemez-problémák megoldása a erőssége core részeinek kívüli. Az oka annak, hogy a szolgáltatás háló húzó szerzett szolgáltatások készítéséhez megválasztott technológia.

Szolgáltatás háló célja csökkentheti a bonyodalmainak a microservice megközelítés alkalmazások tudnivalóit, sok költséges redesigns, hogy nem rendelkezik folyamatát. Indítsa el a kisvállalati, szükség esetén, szolgáltatások érvénytelenítése újak hozzáadása és az ügyfél használatát –, hogy a megoldás is alapkoncepciójára méretezni. Is tudja, hogy valójában nincsenek számos más még megoldandó problémák, hogy több udvariasan microservices a legtöbb fejlesztők számára. Tárolók és programozási modell szereplő példák az adott irányba kis lépéseket, és azt biztos abban, hogy a könnyebb további újítás jelentkezik.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

* További információ:
    * [Szolgáltatás háló terminológia – áttekintés](service-fabric-technical-overview.md)
    * [Microservices: Az alkalmazás forradalom hajtott a felhőben](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)


[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
