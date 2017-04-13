<properties
    pageTitle="Figyelése, diagnosztizálása és tároló elhárítása |} Microsoft Azure"
    description="Tárterület analytics, ügyféloldali naplózás és egyéb külső eszközök azonosítása, diagnosztizálása, és Azure tároló kapcsolatos hibák elhárítása a funkciók használhatók."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Figyelése, diagnosztizálása és a Microsoft Azure tároló – problémamegoldás

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>– Áttekintés

Diagnosztizálása és hibaelhárítás a felhőalapú környezetben is elosztott alkalmazás lehet komplexebb, mint a hagyományos környezetben. Alkalmazások a mobileszközön, vagy valamilyen kombinációját, ezek a helyszíni PaaS vagy IaaS infrastruktúra telepíthetők. Általában az alkalmazás hálózati forgalmának engedélyezésére előfordulhat, hogy bejárásához és nyilvános hálózatok és az alkalmazás használhat több tárterületet technológiák, például a Microsoft Azure tároló tábla, BLOB, sorok vagy egyéb adatok mellett fájlok tárolja, például a relációs adatbázisok dokumentum és.

Ilyen alkalmazáskezelési sikeresen kell ezzel kapcsolatban beérkező figyelje meg őket, és megtudhatja, hogyan diagnosztizálása és őket, és a kapcsolódó technológiák részletekbe menően hibaelhárítása. Azure tároló szolgáltatások felhasználóként meg kell folyamatosan figyelheti a tároló szolgáltatást használja az alkalmazás (például a szokásos válaszidő lassabb) viselkedés váratlan módosításokat, és naplózás részletes adatokat szeretne gyűjteni és elemzése a mélység probléma. Mind figyelhető és naplózható kap diagnosztika információk alapján megállapíthatja, hogy az alkalmazás hibát észlelt probléma a kiváltóok segítségével. Ezután elhárításában, és a megfelelő lépéseket sokat tehet ismételt azt határozza meg. Azure tároló alapszintű Azure szolgáltatás, és a legtöbb megoldások, amelyek ügyfelei telepítse az Azure infrastruktúra fontos részét képezi. Azure tároló figyelés, diagnosztizálása és a felhőalapú alkalmazásokban tároló hibaelhárítás egyszerűsítése érdekében funkciókat tartalmazza.

> [AZURE.NOTE] A zóna felesleges tároló (ZRS) replikációs típusú tároló számla a mértékek vagy naplózás képesség engedélyezve van a problémának jelenleg nincs. Is az Azure-fájlok szolgáltatás naplózás egyelőre nem támogatja.

Rutinszerzést útmutató végpontok közötti hibaelhárítási Azure tárterület-alkalmazásokban olvassa el a [Végpont hibaelhárítási Azure tárolási mértékek és a naplózást, a AzCopy, és a üzenet Analyzer](storage-e2e-troubleshooting.md)című témakört.

+ [– Bevezetés]
    + [Hogyan vannak rendezve, ez az útmutató]
+ [A tároló szolgáltatás figyelése]
    + [Szolgáltatás állapota figyelése]
    + [Kapacitásának figyelése]
    + [Elérhetőség ellenőrzése]
    + [A teljesítmény figyelése]
+ [Tárterület problémák diagnosztizálása]
    + [A szolgáltatás szolgáltatásállapot-hiba]
    + [Teljesítménnyel kapcsolatos problémák]
    + [Hibák diagnosztizálása]
    + [Tárterület irányító kapcsolatos problémák]
    + [Tárterület naplózási eszközök]
    + [Hálózati naplózási eszközök használatával]
+ [Végpont nyomkövetés]
    + [Naplóadatok használatával történik]
    + [Ügyfél-azonosító kérés]
    + [Kiszolgáló kérelem azonosítója]
    + [Időbélyegeket]
+ [Hibaelhárítási útmutatás]
    + [Mértékek magas AverageE2ELatency és az alacsony AverageServerLatency megjelenítése]
    + [Mértékek alacsony AverageE2ELatency és az alacsony AverageServerLatency megjelenítése, de az ügyfél tapasztalható nagy késést]
    + [Mértékek magas AverageServerLatency megjelenítése]
    + [Az üzenet kézbesítését várólista a váratlan késések tapasztalja]
    + [Mértékek PercentThrottlingError növekedését megjelenítése]
    + [Mértékek PercentTimeoutError növekedését megjelenítése]
    + [Mértékek PercentNetworkError növekedését megjelenítése]
    + [Az ügyfél HTTP 403 (tiltott) üzenet érkezik]
    + [Az ügyfél fogadja üzeneteket HTTP 404-es (nem található)]
    + [Az ügyfél HTTP 409 (Ütközés) üzenet érkezik]
    + [Mértékek megjelenítése a kis PercentSuccess meg, vagy analytics naplóbejegyzések ClientOtherErrors állapotú tranzakció műveletei]
    + [Kapacitás mértékek megjelenítése váratlan növekedését kapacitás tárterület-használat]
    + [Amit tapasztal, nagy számú csatolt VHD virtuális gépeken futó a váratlan újraindítása]
    + [A probléma merül fel a tárhely irányító használja a fejlesztés és tesztelése]
    + [Akkor is problémákba ütközik az Azure SDK telepítése a .NET rendszerhez]
    + [Egy tárhelyszolgáltatáshoz egy másik problémája van]
    + [A Windows és Linux Azure fájlok problémák](storage-troubleshoot-file-connection-problems.md)
+ [Függelékek]
    + [1. melléklet: Fiddler segítségével rögzítheti a http- és HTTPS-forgalom]
    + [2. melléklet: A wireshark eszközben használatával rögzíti a hálózati forgalmat]
    + [3. melléklet: Microsoft üzenet Analyzer használatával hálózati forgalmának engedélyezésére rögzítése]
    + [. Melléklet 4: Excel használatával megtekintheti a mértékek, és jelentkezzen be az adatok]
    + [. Melléklet 5: Visual Studio Team Services alkalmazás háttérismeretek figyelés]

## <a name="introduction"></a>– Bevezetés

Ez az útmutató megtudhatja, hogy miként Azure tároló Analytics, például szolgáltatások használata ügyféloldali naplózás a az Azure tároló ügyfél tárak és egyéb külső eszközök azonosítása, diagnosztizálása és Azure tároló – problémamegoldás kapcsolatos problémákat.

![][1]

*Figyelés, diagnosztika, és hibaelhárítási szám 1*

Ez az útmutató célja elsősorban a fejlesztők az Azure tároló Services és az informatikai szakemberek és a felelős ilyen online szolgáltatások kezelése online szolgáltatások olvasható. Ez az útmutató céljai vannak:

- Segít karbantartása: az állapot és a teljesítmény Azure tárterület-fiókjában.
- Az adja meg a szükséges folyamatok és eszközök könnyebben eldöntheti, hogy ha ügy vagy probléma az alkalmazásokban Azure tárolóhoz vonatkozik.
- Biztosítson értekezletekre útmutatást Azure tárolóhoz kapcsolatos problémák megoldása.

### <a name="how-this-guide-is-organized"></a>Hogyan vannak rendezve, ez az útmutató

A szakasz "[a tárhely szolgáltatás felügyelete a]" leírja, hogy miként figyelheti az állapot és a teljesítmény a Azure tároló szolgáltatások Azure tárolási Analytics mértékek (tárolási mértékek) használatával.

A szakasz "[felismerése tároló problémák]" Azure tároló Analytics naplózás (tárhely naplózás) használatával problémáinak diagnosztizálása ismerteti. Azt is megtudhatja, hogy miként például a tárterület-ügyfél könyvtár az ügyfél-tárak közül található berendezések használatának .NET vagy a Java Azure SDK ügyféloldali naplózás engedélyezése.

A szakasz "[-végpont nyomkövetési]" ismerteti, hogyan hozhatók különböző naplófájlok és mérőszámok adatok tárolt adatok számára.

A szakasz "[Hibaelhárítás útmutatást]" nyújt az egyes előforduló általános tárolási kapcsolatos problémák hibaelhárítási útmutatást.

A "[függelékek]" a hálózati csomag adatok elemzéséhez a HTTP-/ HTTPS-üzenetek és a Microsoft üzenet Analyzer használatával történik a naplóadatokat Fiddler elemzése más eszközöket, például a wireshark eszközben és a Netmon használatának vonatkozó információkat tartalmazza.


## <a name="monitoring-your-storage-service"></a>A tároló szolgáltatás figyelése

Ha ismeri a Windows teljesítményét figyelve, érdemes elképzelnie tárolási mértékek, hogy egy Windows teljesítmény Monitor számláló Azure tároló megfelelőjét. Tárolási mértékek találja a mértékek (számláló a Windows teljesítmény Monitor terminológiájában), például a szolgáltatások elérhetőségét, szolgáltatási kérelmek teljes számát, vagy a szolgáltatás sikeres kérelmeket százalékát teljes készletével. A rendelkezésre álló mérési módja miatt teljes listáját a [Tárhely Analytics mértékek táblázat séma](http://msdn.microsoft.com/library/azure/hh343264.aspx)témakörben talál. Megadhatja, hogy szeretné-e a tároló szolgáltatás összegyűjtése és mérőszámok óránként vagy percenként összesítése. Mértékek engedélyezése és figyelheti a tárterület-fiókokat kapcsolatban további tudnivalókért olvassa el a [engedélyezése a tárolási mértékek és mérőszámok adatainak megtekintése](http://go.microsoft.com/fwlink/?LinkId=510865)című témakört.

Megadhatja, hogy melyik óránkénti mértékek szeretne megjeleníteni az [Azure-portálon](https://portal.azure.com) , és állítsa be szabályokat, amelyek értesítik rendszergazdák által e-mail amikor egy óránkénti metrikus meghaladja az egy adott küszöbértéknél. További tudnivalókért lásd: az [Értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). 

A tároló szolgáltatás által gyűjtött mértékek használata ajánlott a munkamennyiség, de nem rögzíthet minden tárolási műveletet.

Az Azure-portálon megtekintheti a mértékek, például elérhetőségét, kérések teljes és átlag késési tároló fiókot. Értesítés szabály is be van állítva a rendszergazda figyelmeztet, ha az elérhetőség egy bizonyos szintre: alá esik. Az adatok megjelenítését egy lehetséges terület vizsgálat a táblázat szolgáltatás sikeres százalékos nem érik el a 100 %-os (további tudnivalókért lásd: a szakasz "[Mértékek megjelenítése a kis PercentSuccess meg, vagy analytics naplóbejegyzések ClientOtherErrors állapotú tranzakció műveletek]").

Az Azure alkalmazások annak érdekében, hogy azok a megfelelő és végrehajtása által elvárt módon folyamatosan kell figyelni:

- Az alkalmazás, amely lehetővé teszi, hasonlítsa össze az aktuális adatokat, és kiválogathatja azokat a jelentős módosításokat a Azure tárolási és az alkalmazás az egyes eredeti mértékek létrehozásáról. Az eredeti mértékek értékeket, sok esetben lesz adott alkalmazást, és létre kell hoznia őket az alkalmazás tesztelése teljesítmény közben.
- Minute mértékek felvétele és használata őket a Lync-aktívan a váratlan hibák és rendellenességeinek, például az a hiba kiugrásainak megfelelő számolja meg, vagy díjak kérése.
- Óránkénti mértékek rögzítése, és használatuk átlagos értékek követésére átlagos hibaérték száma, és mértékek kérése.
- Vizsgálat alatt potenciális problémákra diagnosztikai eszközökkel tárgyalt későbbi szakaszában "[felismerése tároló problémák]."

A diagramok, az alábbi ábrán 3 bemutatják, hogyan átlagolása, amelyre az óránkénti mértékek elrejtheti kiugrásainak megfelelő tevékenységet. Az óránkénti mértékek jelennek meg stabil díjának kérelmek, a mértékek felfedése zajlik valójában ingadozásokat percet közben.

![][3]

Ez a szakasz hátralévő milyen kell figyelni mértékek mutatja be, és miért.

### <a name="monitoring-service-health"></a>Szolgáltatás állapota figyelése

Az [Azure Portal](https://portal.azure.com) segítségével a tárhelyszolgáltatáshoz (és más Azure szolgáltatások) állapotának megtekintése az Azure régiókban a világon. Ez lehetővé teszi, hogy ha a vezérlőelem-Ön kívüli problémát gyengíti a tároló szolgáltatás a régió, használja az alkalmazás azonnal láthatják.

Az [Azure-portálon](https://portal.azure.com) is megadhatja a különböző Azure szolgáltatások befolyásoló eseményekkel kapcsolatos értesítések.
Megjegyzés: Ez az információ volt korábban elérhető az [Azure Service irányítópult](http://status.azure.com)a korábbi adatokkal együtt.

Az [Azure-portálon](https://portal.azure.com) (belső kivételének megfigyelés) az Azure adatközpontokkal belül a rendszerállapot adatait gyűjti össze, miközben Ön lehet venni elfogadó külső a megközelítés hozzáférő rendszeres az Azure által üzemeltetett webalkalmazás több helyről szintetikus tranzakciók létrehozására. Külső eljárás a Visual Studio Team Services [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) és az alkalmazás az összefüggéseket által kínált szolgáltatások példák. A Visual Studio Team Services alkalmazás háttérismeretek kapcsolatos további tudnivalókért lásd "[függelék 5: az alkalmazás az összefüggéseket a Visual Studio Team Services figyelése](#appendix-5)."

### <a name="monitoring-capacity"></a>Kapacitásának figyelése

Tárolási mértékek csak tárolja a blob-szolgáltatás kapacitás mértékek, mert BLOB általában számlája tárolt adatok legnagyobb arányát (írási időben még nem lehetséges a tárolási mértékek figyelheti a táblák és a sorok kapacitása). A **$MetricsCapacityBlob** táblázatban megtalálhatja ezeket az adatokat, ha engedélyezte a Blob-szolgáltatás figyelése. Tárolási mértékek rekordok naponta egyszer ezeket az adatokat, és a **RowKey** értékének segítségével határozza meg, hogy a sor tartalmaz-e a felhasználói adatok (érték- **adatok**) vagy analytics-adatok (érték **analytics**) vonatkozik entitás. Minden egyes tárolt entitás a tárhelyet (**kapacitás** mért bájtban) és az aktuális számát (**ContainerCount**) tárolók és BLOB (**ObjectCount**) információkat tartalmaz, a tárterület-fiókot használ. A kapacitás mérési módja miatt a **$MetricsCapacityBlob** táblában tárolt kapcsolatos további tudnivalókért olvassa el a [Tároló Analytics mértékek táblázat séma](http://msdn.microsoft.com/library/azure/hh343264.aspx)című témakört.

> [AZURE.NOTE] Ezeket az értékeket, hogy a tárterület-fiók kapacitás határain megközelíti-előrejelző kell figyelni. Az Azure-portálon figyelmeztetési szabályokat, hogy értesítést küldjön, ha összesítő tároló meghaladja, vagy a megadott küszöbértékek alá esik is hozzáadhat.

Különböző tároló objektumok, például BLOB méretének becslése című témakörben kaphat segítséget [Ismertetése Azure tároló számlázási – sávszélesség, a tranzakciókat, és a kapacitás](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)blogbejegyzésből.

### <a name="monitoring-availability"></a>Elérhetőség ellenőrzése

Kell figyelni a tárhely szolgáltatások elérhetőségét a tárterület-fiókjában az óránkénti vagy perc mértékek táblázatoknak **rendelkezésre állás** oszlopban szereplő érték megfigyelésével – **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. Az **Elérhetőség** oszlop, amely jelzi a szolgáltatásra vagy az API művelet a sor (a **RowKey** azt jelzi, hogy a sor tartalmazza a szolgáltatás egészének vagy egy adott API művelet mértékek) jelöli az elérhető százalékos értéket tartalmaz.

Bármilyen érték kisebb, mint 100 %-os azt jelzi, hogy egyes tároló kérések nem működnek. Láthatja, hogy miért azok nem működnek a többi oszlop megjelenítése a számok, például **ServerTimeoutError**hiba különböző típusú kérelmek mértékek adatok vizsgálata alapján. **Elérhetőség** eshet ideiglenes 100 %-os tranziens kiszolgáló időtúllépése például ok miatt, miközben a szolgáltatás lép partíciót jobb terheléselosztási összehívás; kell várt az újraküldés logikája az ügyfélalkalmazás olyan időszakos feltételek kell kezelni. A cikk [tárolási Analytics bejelentkezett műveletek és állapotüzenetek](http://msdn.microsoft.com/library/azure/hh343260.aspx) megjeleníti a tárolási mértékek tartalmazó az **Elérhetőség** számítási tranzakció típusát.

Az [Azure-portálra](https://portal.azure.com)értesítési szabályok azt jelzi, ha a megadott küszöbértékét alá esik szolgáltatás **elérhetősége** is hozzáadhat.

Ez az útmutató "[Hibaelhárítás útmutatást]" Csatornakezelési néhány gyakori tároló szolgáltatás problémák megoldásához elérhetőségét.

### <a name="monitoring-performance"></a>A teljesítmény figyelése

A tároló szolgáltatások a teljesítmény figyelését, a következő mérési módja miatt az óránkénti és percek mértékek táblázatok is használhatja.

- A **AverageE2ELatency** és **AverageServerLatency** oszlop értékei a átlagos idő megjelenítése a tárhelyszolgáltatáshoz vagy API művelettípus tart a feldolgozása. **AverageE2ELatency** azt méri, az időnek olvasni a kérelmet, és küldje el a kérelem feldolgozásához idő mellett a kérdésre adott választ tartalmazó végpontok közötti időtartama (tehát tartalmazza a hálózati késés után a kérelem eléri a tároló szolgáltatás); **AverageServerLatency** azt méri, imént feldolgozási idejének, és ezért kizárja a bármely, az ügyfél kommunikáció kapcsolatos hálózati késés. A szakasz "[Mértékek megjelenítése magas AverageE2ELatency és az alacsony AverageServerLatency]" című részben Ez az útmutató a vitafórum-miért előfordulhat, hogy egy jelentős e két érték közötti különbséget.
- A **TotalIngress** és **TotalEgress** oszlop értékei bájtok, lesz, és ki a tárhelyszolgáltatáshoz, illetve a megadott API művelet típusú keresztül fog adatok mennyiségét mutatják.
- A **TotalRequests** oszlopban lévő értékek megjelenítése a tároló szolgáltatás API művelet fogadó kérések száma. **TotalRequests** , amely a tárhelyszolgáltatáshoz kapja kérések száma.

Általában fog figyelheti az alábbi értékek közül váratlan megváltozott, azt jelzi, hogy vizsgálat igénylő problémát.

Az [Azure-portálon](https://portal.azure.com)azt jelzi, ha a teljesítménymutatók közül az alatt a szolgáltatás őszi riasztási szabályok felvétele, vagy nem haladja meg a megadott küszöbértékét.

Ez az útmutató "[Hibaelhárítás útmutatást]" Csatornakezelési teljesítménnyel kapcsolatos néhány gyakori tároló szolgáltatásproblémák.


## <a name="diagnosing-storage-issues"></a>Tárterület problémák diagnosztizálása

Számos módon, hogy esetleg tudomást szerez az problémáinak az alkalmazásban, ezek a következők:

- Fő hiba, amelyek hatására az alkalmazás összeomlik vagy leáll.
- Jelentős módosításokat az eredeti értékek a mértékek figyelése "[a tárhely szolgáltatás figyelése].", az előző szakaszban leírtak szerint
- A felhasználó az alkalmazás jelzi, hogy bizonyos adott művelet nem hajtható végre, vártnak vagy, hogy egyes szolgáltatások nem működik.
- A naplófájlok vagy az értesítési néhány más módszer segítségével megjelenő az alkalmazáson belül generált hibák.

Általában Azure tárolása kapcsolatos problémákat sorolhatók négy fő kategóriába egyikét:

- Az alkalmazás probléma van a teljesítményt, vagy a felhasználók által jelzett, vagy a tár a teljesítménymutatók változásai fel.
- Egy vagy több régióban az Azure tároló infrastruktúra probléma van.
- Az alkalmazás hibát, vagy a felhasználók által jelzett, vagy a hiba darab mérési módja miatt figyelheti egyikében növelésével feltárt léptek fel.
- Fejlesztés és a teszt során előfordulhat, hogy használ-e a helyi tároló irányító; előfordulhatnak problémák elhárításához, amely a tárhely irányító használatát vonatkoznak.

Az alábbi szakaszok tagolása az alábbi lépések kell diagnosztizálása és az egyes ezekben a kategóriákban kapcsolatos problémák megoldása. A "[Hibaelhárítás útmutatás]" című szakaszában ez az útmutató részletesebb néhány gyakori problémákra, előfordulhatnak.

### <a name="service-health-issues"></a>A szolgáltatás szolgáltatásállapot-hiba

Szolgáltatásállapot-hiba jellemzően a vezérlőelem-Ön kívül. Az [Azure-portálon](https://portal.azure.com) folyamatban lévő problémák megoldásához információt tartalmaz az Azure szolgáltatásokkal, ideértve a tárterület-szolgáltatásokat. Ha, inaktív olvasásra Geo felesleges tárolására, a tárhely fiók létrehozásakor, megelőzve az adatok, a fő tartózkodási helyén nem érhető el az alkalmazás sikerült váltson ideiglenesen a csak olvasható másolatot az kiegészítő helyén. Ehhez az alkalmazáshoz kell tud váltani az elsődleges és másodlagos tárolási helye használata között, és is tudjanak dolgozni a csökkentett szolgáltatáskészletű üzemmódban csak olvasható adatokkal. Az Azure tároló ügyfél tárak szabályzat kialakítása újrapróbálkozási, amely a másodlagos tárhelyről tudja olvasni, abban az esetben, ha sikertelen az elsődleges tárhelyről olvasási teszi lehetővé. Az alkalmazás kell tartsa szem előtt, hogy a másodlagos helyre adatai ahányat egységes. További információért tekintse meg az [Azure tároló redundancia beállítások](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)és olvasási hozzáférést Geo felesleges tárhelyet.

### <a name="performance-issues"></a>Teljesítménnyel kapcsolatos problémák

Lehet, hogy az alkalmazás teljesítményének szubjektív, különösen a felhasználó szemszögéből. Ezért fontos, hogy az eredeti mértékek elérhető Észreveheti, ha előfordulhat, hogy a teljesítményproblémát. Számos tényező hatással lehet a ügyfél alkalmazás perspektíva Azure tárhelyszolgáltatása teljesítményét. Ezek a tényezők a tároló szolgáltatás, az ügyfél vagy a hálózati infrastruktúrát; előfordulhat, hogy működjön. Ezért fontos, hogy a teljesítményproblémát forrását azonosítására szolgáló stratégia.

Miután azonosította, hogy a teljesítményproblémát a mérési módja miatt az az oka valószínűleg helyét, majd segítségével a naplófájlok diagnosztizálása és hiba elhárítása További részletes információkat találja.

A szakasz "[Hibaelhárítás útmutatást]" Ez az útmutató a itt olvashat néhány gyakori teljesítmény kapcsolatos problémák, előfordulhatnak.

### <a name="diagnosing-errors"></a>Hibák diagnosztizálása

Az alkalmazás felhasználóinak értesítheti, az ügyfélalkalmazás által jelzett hibákat. Tárolási mértékek is rögzíti a tárhely szolgáltatásokból, például **NetworkError**, **ClientTimeoutError**vagy **AuthorizationError**különböző hiba típusok számát. Miközben a tárolási mértékek csak rekordok típusai eltérő hiba számát, részletesebb információkat egyes kérések szerezhet be kiszolgálóoldali, az ügyféloldali és a hálózati naplók vizsgálata alapján. Általában a HTTP-állapotkód a tároló szolgáltatás által visszaadott meg fogalmat ad feltüntetése, miért nem sikerült a kérést.

> [AZURE.NOTE] Ne feledje, hogy, hogy meg kell várt szakaszos hibák: például a hibák miatt átmeneti hálózati feltételek vagy alkalmazáshibák.

Az alábbi források állnak tároló kapcsolatos állapot és a hiba kódok megértéséhez hasznos:

- [Gyakori REST API-val hibakódok esetén](http://msdn.microsoft.com/library/azure/dd179357.aspx)
- [BLOB szolgáltatás hibakódok esetén](http://msdn.microsoft.com/library/azure/dd179439.aspx)
- [Várólista szolgáltatást hibakódok esetén](http://msdn.microsoft.com/library/azure/dd179446.aspx)
- [Táblázat szolgáltatás hibakódok esetén](http://msdn.microsoft.com/library/azure/dd179438.aspx)
- [Fájl szolgáltatás hibakódok esetén](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Tárterület irányító kapcsolatos problémák

Az Azure SDK tartalmaz egy tároló irányító fejlesztési munkaállomáson futtatását is lehetővé teszi. A irányító keltő a szűrő megőrzi az Azure tároló szolgáltatások viselkedése a legtöbb és hasznos fejlesztés és próba során úgy meg egy Azure-előfizetést és Azure tárterület-fiók nélkül Azure tároló szolgáltatást használó alkalmazások futtatásához.

Ez az útmutató "[Hibaelhárítás útmutatást]" Csatornakezelési gyakori problémákat talált a tárhely irányító használatával.

### <a name="storage-logging-tools"></a>Tárterület naplózási eszközök

Tárterület naplózás kiszolgálóoldali naplózás az Azure tárterület-fiókja tárterület-összehívások biztosít. Kiszolgálóoldali naplózás engedélyezése és elérni a naplóadatokat kapcsolatban további tudnivalókért lásd: [tároló naplózás engedélyezése és a naplóadatokat elérése](http://go.microsoft.com/fwlink/?LinkId=510867).

A .NET rendszerhez, a tárhely ügyfél tár lehetővé teszi, hogy az alkalmazás által elvégzett tárolási műveletek vonatkozik ügyféloldali naplóadatok összegyűjtése. További tudnivalókért olvassa el a [ügyféloldali naplózás a .NET-ügyfél tárral tároló](http://go.microsoft.com/fwlink/?LinkId=510868)című témakört.

> [AZURE.NOTE] Bizonyos körülmények között (például Társítások engedélyezési hibák) a felhasználó jelenthetik, amelynek kérelem adatokat nem a található a tárhely kiszolgálóoldali naplók hibát. A naplózás funkciók a tárterület-ügyfél tár használatával vizsgálja meg, ha a probléma oka az, az ügyfél, vagy vizsgálja meg a hálózati eszközök figyelése hálózati használatával.

### <a name="using-network-logging-tools"></a>Hálózati naplózási eszközök használatával

A forgalom az alapul szolgáló hálózati feltételek és az adatokat, az ügyfél- és kiszolgálóoldali cseréje részletes információt nyújt az ügyfél és a kiszolgáló közötti is rögzíthet. Hasznos hálózati naplózás eszközök közé tartoznak:

- [Fiddler](http://www.telerik.com/fiddler) , amely lehetővé teszi, hogy vizsgálja meg, az élőfejek és a http- és HTTPS kérelem és válasz üzenetek tartalom adatok proxy hibakeresési ingyenes webhelyet. További tudnivalókért lásd: [függelék 1: Fiddler segítségével rögzítheti a http- és HTTPS-forgalom](#appendix-1).
- [Microsoft Network Monitor (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) és a [wireshark eszközben](http://www.wireshark.org/) ingyenes hálózati protokoll gázelemzők, amelyek lehetővé teszik sokféle hálózati protokollokat csomag részletes információinak megtekintése. A wireshark eszközben kapcsolatos további tudnivalókért lásd: "[mintája a 2: segítségével a Wireshark rögzíti a hálózati forgalmat](#appendix-2)".
- Microsoft üzenet Analyzer levő adatbázisban felülírja a Netmon, és nemcsak a hálózati csomag adatok rögzítése, megtekintése és elemzése a más eszközökről rögzített naplóadatokat segít a Microsoft eszköz. További tudnivalókért lásd: "[függelék 3: segítségével a Microsoft üzenet Analyzer rögzíti a hálózati forgalmat](#appendix-3)".
- Ha szeretne egy egyszerű kapcsolódási tesztje választógombot, ellenőrizze, hogy az ügyfélgép csatlakozhat az Azure tárhelyszolgáltatáshoz a hálózaton keresztül szeretne végrehajtani, nem ezt megteheti a szabványos **ping** eszközzel az ügyfélgépen. A [ **tcping** eszköz](http://www.elifulkerson.com/projects/tcping.php) segítségével azonban ellenőrizze a kapcsolatot.

Sok esetben problémát diagnosztizálása elegendő a naplóadatokat a tárhely naplózás és a tárterület-ügyfél tár, de egyes esetekben előfordulhat, a hálózati naplózás eszközökben nyújtó részletes tudnivalókat. Például használata Fiddler HTTP és HTTPS-üzenetek megtekintéséhez lehetővé teszi küldeni, valamint a tárhely szolgáltatásokat, amelyek lehetővé teszik, hogy vizsgálja meg, hogyan az ügyfélalkalmazás próbálkozások tárolási műveletek az élőfej- és tartalom adatok megjelenítéséhez. Protocol (protokoll) gázelemzők, például a wireshark eszközben a csomag szintjén úgy megtekintése a TCP-adatok, amelyek lehetővé teszik, hogy az elveszett csomagok és kapcsolódási problémák elhárítása működnek. Üzenet Analyzer HTTP és a TCP-rétegek is működnek.

## <a name="end-to-end-tracing"></a>Végpont nyomkövetés

Végpont nyomkövetés naplófájlok számos egy olyan hasznos eljárás vizsgálja meg a potenciális problémákra. Is használhatja a dátum/idő adatokat a mértékek adatokból ember keresése az a részletes információt, amely segít a probléma megoldásához a naplófájlok feltüntetése.

### <a name="correlating-log-data"></a>Naplóadatok használatával történik

Hálózati követi nyomon az ügyfélalkalmazásokban naplók megtekintésekor, és a kiszolgálóoldali tároló naplózás rendkívül fontos engedélyezni szeretné a összehangolására kéri a különböző naplófájlok között. A naplófájlok számos különböző mező, amelyek hasznos korrelációs azonosítót tartalmaz. Az ügyfél-azonosító a leghasznosabb használja összehangolására bejegyzések a különböző naplókban mezőt. Egyes esetekben azonban célszerű lehet kiszolgáló kérelem azonosító vagy időbélyegeket. A következő szakaszokban a beállításokkal kapcsolatos további részleteket.

### <a name="client-request-id"></a>Ügyfél-azonosító kérés

A tároló ügyfél tár automatikusan létrehoz egy egyedi ügyfél-kérés azonosító összes kérés.

- A ügyféloldali naplókban, amely a tárterület-ügyfél tár hoz létre az ügyfél-azonosító minden naplóbejegyzés kérésére vonatkozó **Kérés ügyfél-azonosító** mezőjében jelenik meg.
- Hálózati nyomkövetéshez kapcsolódó például egy Fiddler által rögzített az ügyfél-azonosító akkor látható, a kérelem során az **x-ms-ügyfél-kérés-id** HTTP élőfej értékként.
- A kiszolgálóoldali naplózás tároló naplóban az ügyfél-azonosító az ügyfél kérelem azonosító oszlopában látható.

> [AZURE.NOTE]Az azonos ügyfél-azonosító megosztására, mert az ügyfél ezt az értéket oszthatnak ki (bár a tárterület-ügyfél tár automatikusan hozzárendel egy új értéket) több kérések lehetőség. Az ügyféltől visszatérések, amíg az összes kísérletek megosztása az azonos ügyfél-azonosító. A köteg az ügyféltől küldött, amíg a köteg van egyetlen ügyfél kérelem azonosítója.


### <a name="server-request-id"></a>Kiszolgáló kérelem azonosítója

A tároló szolgáltatás automatikusan létrehozza a kiszolgáló kérelem azonosítók.

- A kiszolgáló kérelem azonosítója a kiszolgálóoldali naplózás tároló napló **Kérelem azonosítója** oszlopfejlécen jelenik meg.
- A hálózati nyomkövetési, például egy Fiddler által rögzített a kiszolgáló kérelem azonosítója formában jelenik meg a válaszüzenetek az **x-ms-kérés-id** HTTP élőfej értéket.
- A ügyféloldali naplókban, amely a tárterület-ügyfél tár hoz létre a kiszolgáló kérelem azonosítója jelenik meg a naplóbejegyzés, a kiszolgáló válasz részletes **Művelet szöveg** oszlopában.

> [AZURE.NOTE] A tároló szolgáltatás mindig rendel egy egyedi kiszolgáló kérelem azonosítója minden kérelem érkezik, minden újrapróbálkozási kísérlet az ügyféltől és minden kötegben szereplő művelet tartalmazza a egyedi kiszolgáló kérelem azonosítóját.

Ha a tárhely ügyfél tár egy **StorageException** okoz, az ügyfél, a **RequestInformation** tulajdonság a **ServiceRequestID** tulajdonságait tartalmazó **RequestResult** objektumot tartalmaz. Egy **RequestResult** objektum egy **OperationContext** példányával is elérheti.

A kódot az alábbi minta bemutatja, hogyan állíthatja **ClientRequestId** egyéni érték a kérelem **OperationContext** objektum csatolása a tároló szolgáltatás. Azt is megtudhatja, hogy miként az **ServerRequestId** érték beolvasása a válaszüzenetet.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Időbélyegeket

Időbélyegeket keresse meg a kapcsolódó naplóbejegyzések, de legyen óvatos, ha az ügyfél és a kiszolgálón, előfordulhat, hogy létezik között bármilyen időbeállításainak is használhatja. Keresett kell plusz vagy mínusz 15 percet, hogy a megfelelő kiszolgálóoldali bejegyzéseket, az időbélyegző, az ügyfél alapján. Ne feledje, hogy, hogy a mértékek tartalmazó BLOB blob metaadatait azt jelzi, a mértékek, a blob; tárolt időtartomány Ez akkor hasznos, ha sok mértékek BLOB perc vagy a óra.

## <a name="troubleshooting-guidance"></a>Hibaelhárítási útmutatás

Ez a szakasz segít a diagnosztikai és Azure tároló szolgáltatások használata során felmerülő néhány gyakori hibák elhárítása az alkalmazás. Az alábbi lista használatával keresse meg a problémáról vonatkozó információkat.

**Döntési fa – hibaelhárítás**

----------

A probléma az egyik a tároló szolgáltatást a teljesítmény összekapcsolása?

- [Mértékek magas AverageE2ELatency és az alacsony AverageServerLatency megjelenítése]
- [Mértékek alacsony AverageE2ELatency és az alacsony AverageServerLatency megjelenítése, de az ügyfél tapasztalható nagy késést]
- [Mértékek magas AverageServerLatency megjelenítése]
- [Az üzenet kézbesítését várólista a váratlan késések tapasztalja]

----------

A problémát a tárterület-szolgáltatások elérhetősége összekapcsolása?

- [Mértékek PercentThrottlingError növekedését megjelenítése]
- [Mértékek PercentTimeoutError növekedését megjelenítése]
- [Mértékek PercentNetworkError növekedését megjelenítése]

----------

Az ügyfélalkalmazás fogad HTTP (például 404) 4XX választ egy tárhelyszolgáltatáshoz?

- [Az ügyfél HTTP 403 (tiltott) üzenet érkezik]
- [Az ügyfél fogadja üzeneteket HTTP 404-es (nem található)]
- [Az ügyfél HTTP 409 (Ütközés) üzenet érkezik]

----------

[Mértékek megjelenítése a kis PercentSuccess meg, vagy analytics naplóbejegyzések ClientOtherErrors állapotú tranzakció műveletei]

----------

[Kapacitás mértékek megjelenítése váratlan növekedését kapacitás tárterület-használat]

----------

[Amit tapasztal, nagy számú csatolt VHD virtuális gépeken futó a váratlan újraindítása]

----------

[A probléma merül fel a tárhely irányító használja a fejlesztés és tesztelése]

----------

[Akkor is problémákba ütközik az Azure SDK telepítése a .NET rendszerhez]

----------

[Egy tárhelyszolgáltatáshoz egy másik problémája van]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Mértékek magas AverageE2ELatency és az alacsony AverageServerLatency megjelenítése

A alatt az eszköz figyelése [Azure portál](https://portal.azure.com) példa szemlélteti az **AverageE2ELatency** lényegesen nagyobb, mint a **AverageServerLatency**esetén.

![][4]

Ne feledje, hogy a tároló szolgáltatás csak számítja ki a metrikus egységek **AverageE2ELatency** sikeres kérelmeket, eltérően **AverageServerLatency**, egyúttal az időpontot, az ügyfél megnyitja az adatok küldéséhez és fogadásához a visszaigazoló a tárhely szolgáltatásból tartalmazza. Ezért **AverageE2ELatency** és **AverageServerLatency** eltérése miatt az ügyfélalkalmazás lassú válasz vagy a hálózati feltételek miatt lehet.

> [AZURE.NOTE] A tároló naplózás naplóadatokat a egyes tároló műveletekhez **E2ELatency** és **ServerLatency** is megtekintése.

#### <a name="investigating-client-performance-issues"></a>Ügyfél teljesítményét érintő hibák kivizsgálása

Az ügyfél lassan válaszol lehetséges okai a rendelkezésre álló kapcsolatok vagy témák korlátozott számú, illetve ha éppen alacsony, például a Processzor, a memóriahasználat vagy a hálózati sávszélesség-erőforrások tartalmazzák. Előfordulhat, hogy a probléma megoldásához, az ügyfél kód hatékonyabb (például segítségével a tárhelyszolgáltatáshoz aszinkron hívások) módosítása, vagy nagyobb virtuális gép (a további magmintákat és több memóriát) használatával.

A tábla- és várólista szolgáltatásokra a Nagle algoritmus is okozhatja magas **AverageE2ELatency** **AverageServerLatency**képest: kapcsolatban további információk a bejegyzés [Nagle's algoritmus nem könnyen megjegyezhető kis kérések felé](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). A kód Nagle algoritmus letilthatja a **System.Net** névtér az **ServicePointManager** osztály használatával. Meg kell ehhez, bármilyen hívásokat a táblázatra vagy az alkalmazásban, mivel ez nem befolyásolja a kapcsolatok már várólista szolgáltatások megnyitása előtt. Az alábbi példa a **Application_Start** módszer dolgozó szerepkörbe származik.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Ellenőrizze, hány kéri az ügyfélalkalmazás adatlapot megtekintéséhez az ügyféloldali naplókban és a jelölőnégyzet az általános .NET kapcsolódó az ügyfélprogram, például a Processzor, .NET szemétgyűjtő, hálózati-kihasználtság vagy memória a teljesítmény szűk. Kiindulási pontként hibaelhárítási .NET ügyfélalkalmazásokban olvassa el a [Hibakeresés, nyomon követése, és adatainak összegyűjtése](http://msdn.microsoft.com/library/7fe0dd2y)című témakört.

#### <a name="investigating-network-latency-issues"></a>Hálózati késés hibák kivizsgálása

A hálózat okozza nagy végpontok közötti késést általában átmeneti feltételek miatt. Mindkét tranziens és állandó hálózati hibák, például a kihagyott csomagokat, azokat is vizsgálja meg a következő eszközöket, például a wireshark eszközben vagy a Microsoft üzenet Analyzer használatával.

Hálózati hibák elhárítása a wireshark eszközben használatával kapcsolatos további tudnivalókért lásd: "[függelék 2: segítségével a wireshark eszközben rögzíti a hálózati forgalmat]."

Hálózati hibák elhárítása a Microsoft üzenet Analyzer használatával kapcsolatos további tudnivalókért lásd: "[függelék 3: segítségével a Microsoft Message Analyzer rögzíti a hálózati forgalmat]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Mértékek alacsony AverageE2ELatency és az alacsony AverageServerLatency megjelenítése, de az ügyfél tapasztalható nagy késést

Ebben az esetben a legvalószínűbb oka elérje a tárhelyszolgáltatáshoz tárterület-összehívásokban késleltetést. Miért az ügyfél kérései nem készít keresztül, a blob-szolgáltatás kell vizsgálja meg.

Az ügyfél, hogy késleltetné a kérelem küld egyik lehetséges oka az, hogy nincsenek-e a rendelkezésre álló kapcsolatok vagy témák korlátozott számú.

Is ellenőrizze, hogy az ügyfél több próbálkozás hajt végre, és okát vizsgálja meg, ha ez így. Annak megállapításához, hogy az ügyfél több próbálkozás hajt végre, a következőkre van lehetősége:

- Vizsgálja meg a tárhely Analytics naplókat. Ha több próbálkozás is fennáll, látni fogja az azonos ügyfél-azonosító, de a másik kiszolgáló kérelem azonosítók több műveletet.
- Tanulmányozza az ügyfélnek. A részletes naplózás jelzi, hogy történt-e egy újra gombra.
- A kód hibakeresési, és ellenőrizze a kérelem társított **OperationContext** objektum tulajdonságait. A művelet megismétlése rendelkezik, a **RequestResults** tulajdonság több egyedi kiszolgáló kérelem azonosítók tartalmazza. Ellenőrizheti, hogy az egyes kérelme kezdési és befejezési időpontot. További tudnivalókért lásd: a kód minta [kiszolgáló kérelem azonosítója]szakaszában.

Ha problémák lépnek fel nem az ügyfélprogramban, érdemes vizsgálja meg a potenciális hálózati hibák például csomag veszteség. Eszközök, például a wireshark eszközben vagy a Microsoft üzenet Analyzer segítségével vizsgálja meg a hálózati problémákat.

Hálózati hibák elhárítása a wireshark eszközben használatával kapcsolatos további tudnivalókért lásd: "[függelék 2: segítségével a wireshark eszközben rögzíti a hálózati forgalmat]."

Hálózati hibák elhárítása a Microsoft üzenet Analyzer használatával kapcsolatos további tudnivalókért lásd: "[függelék 3: segítségével a Microsoft Message Analyzer rögzíti a hálózati forgalmat]."

### <a name="metrics-show-high-AverageServerLatency"></a>Mértékek magas AverageServerLatency megjelenítése

Magas **AverageServerLatency** blob letöltési kérések, amíg a tárhely naplózás naplók megtekintéséhez, hogy vannak-e az azonos blob (vagy több BLOB) ismételt kérelem kell használnia. Blob a feltöltési kérelmeket, érdemes vizsgálja meg, milyen blokk méretét az ügyfél által használt (például blokkolja a kisebb méretű 64 ezer eredményezhet költségek kivéve, ha az olvasás is a 64 KB-nál kisebb szövegadattömb), és ha több ügyfél megakadályozza az azonos blob párhuzamosan vannak feltöltése. Azt is ellenőrizze kiugrásainak megfelelő kérelmeket, amelyeknél a meghaladó száma a perc mérési módja miatt a egy második méretezhetőség cél: is megjelenik a "[Mértékek megjelenítése PercentTimeoutError növekedését]."

Ha nagy **AverageServerLatency** blob-kérelmek letöltése, ha vannak ismételt kérelmek, az azonos blob vagy BLOB láthatók, majd vegye figyelembe gyorsítótár-e BLOB Azure gyorsítótár vagy az Azure tartalom kézbesítési hálózati (CDN). Feltöltés kérelmekhez javíthatja a átviteli nagyobb méretű blokkokból álló használatával. Lekérdezések táblázatokat hogy az is lehetséges ügyféloldali gyorsítótárazás ügyfelek, amely a műveleteket az ugyanazon lekérdezés, ha az adatok nem változnak gyakran végrehajtásához.

Magas **AverageServerLatency** értékeket a hibajelenség rosszul megtervezett táblát vagy lekérdezést, hogy a Keresés eredménye, vagy hajtsa végre a levélszemét hozzáfűző/prepend mintát is lehet. További információt a "[Mértékek megjelenítése PercentThrottlingError növekedését]" talál.

> [AZURE.NOTE] Egy teljes feladatlista teljesítmény ellenőrzőlistát itt talál: [Microsoft Azure tárhely a teljesítmény és méretezhetőség feladatlista](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Az üzenet kézbesítését várólista a váratlan késések tapasztalja

Ha között a az alkalmazás hozzáadása egy sorba üzenet és az idő a sorból olvasható elérhetővé válik késleltetést tapasztal, akkor az alábbi lépéseket a probléma diagnosztizálása kell vennie:

- Ellenőrizze, hogy a sorba az alkalmazás az üzenetek sikeresen veheti fel. Ellenőrizze, hogy az alkalmazás van nem újbóli próbálkozás **AddMessage** módszer többször sikeres előtt. A tároló ügyfél tár naplókban láthatók a bármely ismételt próbálkozások tárolási műveletek.
- Ellenőrizze, nincs óra nem ferdeség között a dolgozó szerepkört, hogy az üzenet hozzáadása a várakozási sorban található, és a dolgozó szerepkört, amely beolvassa az üzenetet, amely lehetővé teszi, hogy a sorból jelennek meg, ha van-e késleltetést feldolgozása.
- Ellenőrizze, hogy ha a dolgozó szerepkört, amely beolvassa az üzeneteket a sorból van-e hibás. Egy várólista ügyfél felhívja a **GetMessage** módszer, de nem válaszol az visszaigazolást, ha az üzenet marad nem látható, kattintson a sor **invisibilityTimeout** lejárta. Ezen a ponton az üzenet lesz elérhető, újra feldolgozásra.
- Ellenőrizze, hogy ha a sor hossza egyre idővel. Ez akkor fordulhat elő, ha nem rendelkezik a megfelelő munkatársak elérhető az összes olyan üzenetre, amelyet más munkatársak helyez a sorban a feldolgozása. Azt is ellenőrizze a mértékek, ha törlés kérelmeket hibás és üzeneteket, amelyek jelezhetik dequeue számának ismételt megjelenítéséhez kísérletek az üzenet törlése sikertelen volt.
- Vizsgálja meg a tárhely naplózás naplók rendelkező nagyobb, mint várható **E2ELatency** és **ServerLatency** értékek hosszabb időszakra vetített állapotával szokottnál várólista műveletekhez.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Mértékek PercentThrottlingError növekedését megjelenítése

Szabályozási hiba lép fel, ha egy művelet túllépi a tárhelyszolgáltatáshoz méretezhetőség céljainak. A tároló szolgáltatás végzi, annak érdekében, hogy nincs egyetlen ügyfél vagy egy bérlői költségére mások a szolgáltatást használhatja. Többet olvassa el a [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md) méretezhetőség célok tároló fiókok és a teljesítmény célok belül tároló fiókok partíciók tájékoztatást.

Ha a **PercentThrottlingError** mérőszám növekedését nem működnek a szabályozási hibával összehívások százalékos megjeleníteni, akkor vizsgálja meg a két helyzetben:

- [PercentThrottlingError tranziens növelése]
- [Állandó növekedése PercentThrottlingError hiba]

Gyakran előfordul **PercentThrottlingError** növekedését tároló kérések száma növekedését egyszerre, illetve közben először töltse be az alkalmazás tesztelése. Ez lehet is cikkét magát az ügyfél "503 kiszolgáló elfoglalt" vagy "500 művelet időtúllépése" HTTP állapotüzenetek tároló műveletek.

#### <a name="transient-increase-in-PercentThrottlingError"></a>PercentThrottlingError tranziens növelése

Ha kiugrásainak, hogy az alkalmazás magas tevékenység időszakok egybe megfelelő **PercentThrottlingError** értékének láthatók, végre kell hajtania az exponenciális (lineáris) vissza stratégiájának kialakításában ismétlés kikapcsolása az ügyfélprogram: ezzel csökkentse a partíciót azonnali terhelését és az alkalmazás simítja a forgalom kiugrásainak megfelelő súgó. A tároló ügyfél-tár használata újrapróbálkozási házirendek megvalósításáról kapcsolatos további tudnivalókért olvassa el a [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx)című témakört.

> [AZURE.NOTE] Kiugrásainak, amely nem egybe magas tevékenység időszakok száma az alkalmazás megfelelő **PercentThrottlingError** értékének is megjelenhet: legvalószínűbb oka, a terheléselosztás javítható partíciót áthelyezése tárhelyszolgáltatáshoz.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Állandó növekedése PercentThrottlingError hiba

Ha megjelenik a következő **PercentThrottlingError** folyamatosan magas érték egy állandó növelése a mennyiségű tranzakció, illetve a kezdeti betöltés végrehajtásakor azt vizsgálja, az alkalmazás a, majd hogyan az alkalmazás által használt tárterület partíciók, és hogy közelítő azt a méretezhetőség célok tárterület-fiók szükséges. Például láthatók a hibák (Ez egy olyan partíciót számít) várólista szabályozásának, ha ezután figyelembe további sorok használatával húzza szét a tranzakciók több partíciót keresztül. Táblázat hibák szabályozásának láthatók, ha szüksége fontolja meg inkább a partícionáló megváltoztatására ügyleteit széttáró több partíciót végig a partíciót kulcs értékeit szélesebb köre használatával. Egy közös a probléma oka a prepend/hozzáfűző levélszemét mintázat ahol partíciót kulcsként jelölje ki a dátumot, és kattintson az adott napi összes adatot egy partíciót írása: a betöltés, ez eredményezhet egy írási szűk. Fontolja meg egy másik particionáló Tervező vagy értékelni, hogy segítségével blob-tárolóhoz jobb megoldás lehet-e meg. Érdemes ellenőrizheti, hogy mi a szabályozásának eredményeként kiugrásainak megfelelő a forgalmat a, és vizsgálja meg a módon is a minta kérelmek simítás.

Több partíciót a tranzakciókat, elosztása, ha továbbra is kell szem előtt a tárterület-fiókjában méretezhetőség határértékek. Például a legfeljebb 2000 1KB üzenetek másodpercenként feldolgozása tíz sorban várakozó használt fogja a 20 000 üzenetek másodpercenként a tárterület-fiókom általános korlátja. Ha több, mint 20 000 szervezetek másodpercenként folyamat van szüksége, figyelembe több tárterület-fiók használatával. Is szerepelnie kell szem előtt, hogy a kérelmek és a szervezetek méretének amikor a tároló szolgáltatás lehetővé a az ügyfelek hatással van: Ha nagyobb kérések és szervezetek van, akkor előfordulhat, hogy kell szabályozott hamarabb.

Hatékony Lekérdezéstervező okozhat, hogy a táblázat partíciót méretezhetőség korlátozások találati is. Például az szűrő, amely csak kijelöli a szervezetek egy százalék partíciók, de, amely ellenőrzi a partíciók összes entitás lekérdezés kell minden személy elérése. Minden személy, olvassa el az adott partíciót; tranzakciók száma lesz beleszámítanak. ezért egyszerűen érheti el a méretezhetőség célokat.

> [AZURE.NOTE] A teljesítmény tesztelése kell beállíthatja, hogy minden olyan hatékony lekérdezések tervezésére, az alkalmazás.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Mértékek PercentTimeoutError növekedését megjelenítése

A mértékek növekedését **PercentTimeoutError** a tárterület-szolgáltatások egyik mutatják. Egy időben az ügyfél tároló műveletek "500 művelet időtúllépése" HTTP állapotüzenetek nagy mennyiségű kap.

> [AZURE.NOTE] A tárhely szolgáltatásként ideiglenes időtúllépést balances kérések betölteni egy partíciót új kiszolgálóra való áthelyezésével jelenhetnek meg.

A **PercentTimeoutError** mérőszáma a következő mérési módja miatt összesítését: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**és **SASServerTimeoutError**.

A kiszolgáló időtúllépése hiba a kiszolgálón okozza. Az ügyfél időtúllépései fordulhat elő, mert a kiszolgáló műveletet túllépte az ügyfél; időkorlát a tárhely ügyfél tár használó ügyfél például beállíthatja művelet időtúllépés tulajdonságával **ServerTimeout** **QueueRequestOptions** osztály.

Kiszolgáló időtúllépése azt jelzik, hogy a tárhelyszolgáltatáshoz igénylő további vizsgálat probléma. Használhatja a mértékek, tekintheti meg, ha meg vannak szerezze meg a szolgáltatás méretezhetőség határértékei és bármely kiugrásainak megfelelő a forgalmat, előfordulhat, hogy később a probléma azonosításához. Ha a probléma nem folyamatos, lehet, hogy miatt terheléselosztási tevékenységet a szolgáltatásban. Ha a probléma állandó, és nem az alkalmazás szerezze meg a szolgáltatás a méretezhetőség korlátozások okozta, érdemes előléptetése egy ügyfélszolgálati probléma. Az ügyfél időtúllépései döntse el, ha az idő az ügyfél és a vagy módosítása az időtúllépés értéke az ügyfél-megfelelő értékre van állítva, vagy vizsgálja meg, hogyan javíthatja a teljesítmény a tárhely szolgáltatásban, a műveletek például optimalizálása a táblázat lekérdezések vagy az üzenetek méretének csökkentése.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Mértékek PercentNetworkError növekedését megjelenítése

A mértékek növekedését **PercentNetworkError** a tárterület-szolgáltatások egyik mutatják. A **PercentNetworkError** mérőszáma a következő mérési módja miatt összesítését: **NetworkError** **AnonymousNetworkError**és **SASNetworkError**. Ezek fordulhat elő, amikor a tároló szolgáltatás hálózati hibát észlel, ha az ügyfél tároló kérést.

Ez a hiba legvalószínűbb oka egy ügyfél leválasztása időtúllépés a tárhely szolgáltatásban lejárta előtt. Az ügyfél megértéséhez, hogy miért és mikor az ügyfél bontja a tárhelyszolgáltatáshoz kell vizsgálja meg a kódot. Vizsgálja meg a hálózati kapcsolat problémákat az ügyféltől a wireshark eszközben, Microsoft üzenet Analyzer vagy Tcping is használhatja. Ezek az eszközök [függelékek]témakörben olvashat.

### <a name="the-client-is-receiving-403-messages"></a>Az ügyfél HTTP 403 (tiltott) üzenet érkezik

Az ügyfélalkalmazás van értesítő HTTP 403 (tiltott) hiba, ha egy legvalószínűbb oka, hogy az ügyfelet használ egy lejárt megosztott Access aláírás (Társítások) tároló kérelem küld (bár más okok óra ferdeség, érvénytelen kulcsok és üres fejlécek). Ha egy lejárt Társítások kulcs a probléma okát, azokat a bejegyzéseket a kiszolgálóoldali naplózás tároló naplóadatokat nem jelenik meg. A következő táblázat mutatja a ügyféloldali naplókban a tárhely ügyfél tárban, amely azt mutatja be, ez a probléma előforduló által generált mintája:

Forrás|Részletességi|Részletességi|Ügyfél-azonosító kérés|A művelet szöveg
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Információk|3|85d077ab-...|A művelet kezdődő hely elsődleges hely módban PrimaryOnly.
Microsoft.WindowsAzure.Storage|Információk|3|85d077ab-...|Szinkronizált https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14 kérelem indítása&amp;sr = c&amp;si = mypolicy&amp;aláírás OFnd4Rd7z01fIvh 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % = 3D&amp;api-verzió = 2014-02-14.
Microsoft.WindowsAzure.Storage|Információk|3|85d077ab-...|Várakozás a válaszra.
Microsoft.WindowsAzure.Storage|Figyelmeztetés|2|85d077ab-...|Kivételhiba lépett fel várakozás közben: A távoli kiszolgáló hibát: tiltott (403).
Microsoft.WindowsAzure.Storage|Információk|3|85d077ab-...|Válasz érkezett. Állapotkód 403, kérése ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, tartalom-MD5 = =, ETag =.
Microsoft.WindowsAzure.Storage|Figyelmeztetés|2|85d077ab-...|Kivételhiba lépett fel a művelet során: A távoli kiszolgáló hibát: tiltott (403).
Microsoft.WindowsAzure.Storage|Információk|3 |85d077ab-...|Annak ellenőrzése, ha van-e a műveletet ismételni. Újrapróbálkozások száma = 0, akkor a HTTP-állapotkód 403, a kivétel = = a távoli kiszolgáló hibát: tiltott (403).
Microsoft.WindowsAzure.Storage|Információk|3|85d077ab-...|Az elsődleges, a hely mód alapján a következő helyre van beállítva.
Microsoft.WindowsAzure.Storage|Hibaüzenet|1|85d077ab-...|Az újbóli próbálkozás újrapróbálkozási házirend nem engedélyezte. A távoli kiszolgáló a hibás hibát: tiltott (403).

Ebben az esetben célszerű kivizsgáljuk, miért a biztonsági jogkivonat érvényessége hamarosan lejár az ügyfél a token küld a kiszolgálóval előtt:

- A szokásos nem kell beállítania a kezdő időpontot, amikor egy ügyfél azonnal használni egy Társítások hoz létre. Ha vannak kis óra eltérések a fogadó a pontos idő és a tároló szolgáltatás használata a Társítások létrehozása, akkor lehetséges, hogy a tárhelyszolgáltatáshoz fogadásához egy Társítások, amely még nem érvényes.
- Egy nagyon rövid lejárati idő a egy Társítások nem kellene. A host létrehozása a Társítások és a tároló szolgáltatás kisvállalati óra eltérések ismét egy korábbi vártnál látszólag hamarosan lejár Társítások vezethet.
- A verzió paramétert a Társítások kulcs tartalmaz (például **ÜKtgE 2015-04-05 =**) felel meg a verziójának a tárhely ügyfél-tár esetén? Azt javasoljuk, hogy mindig a [Tárhely ügyfél tár](https://www.nuget.org/packages/WindowsAzure.Storage/)legújabb verzióját használja.
- A tároló hívóbetűk követező létrehozásakor, ha ez lehet érvénytelenné bármelyik meglévő Társítások tokenek. Ez lehet, hogy a program hibát, ha hoz létre, és a hosszú lejárati idő a gyorsítótár ügyfélalkalmazásai Társítások tokenek.

Alkalmazás használatakor a tárterület-ügyfél tár Társítások tokenek létrehozásához, majd akkor is könnyen érvényes token össze. Jó helyen jár Ha használja a tároló REST API-val és a Társítások tokenek kézzel megépítése gondosan olvassa el a témakör [a megosztott Access aláírás hozzáférés delegálása](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>Az ügyfél fogadja üzeneteket HTTP 404-es (nem található)
Ha az ügyfélalkalmazás-HTTP 404-es (nem található) üzenetet kap, a kiszolgálóról, ez azt jelenti, hogy az objektum az ügyfél (például egy személy, tábla, blob, tároló vagy várólista) próbált nem létezik a tároló szolgáltatás. Vannak számos oka lehet ennek, például:

- [Az ügyfél vagy egy másik folyamat korábban törli az objektum]
- [A megosztott Access-aláírás (Társítások) engedélyezési probléma]
- [Ügyféloldali JavaScript-kód nincs engedélye az objektum elérésére]
- [Hálózati hiba]

#### <a name="client-previously-deleted-the-object"></a>Az ügyfél vagy egy másik folyamat korábban törli az objektum
Az esetek, amikor az ügyfél próbál olvasni, frissíthet és törölhet egy tárhelyszolgáltatáshoz adatainak általában egyszerűen a kiszolgálóoldali naplók a azonosítása egy korábbi művelet a szóban forgó objektum törölt a tárhely szolgáltatásból. A naplózott adatok nagyon gyakran jeleníti meg, hogy egy másik felhasználó vagy folyamat törli az objektumot. A kiszolgálóoldali naplózás tároló naplóban a Művelettípus és a kért objektum-kulcs oszlopok megjelenítése objektum törlésekor az ügyfél.

Amikor egy ügyfél próbál objektum beszúrása esetben nem lehet azonnal egyértelmű miért ennek eredménye (nem található) HTTP 404-es választ tekintve, hogy az ügyfél hoz létre egy új objektum. Jó helyen jár Ha az ügyfél legyen tudja megkeresni a blob-tárolóhoz, ha az ügyfélnek kell lennie tudja megkeresni várólista üzenet létrehozása blob hoz létre, és az ügyfél van sor hozzáadása azt kell tennie a tábla kereséséhez.

Használhatja a ügyféloldali naplókban a tárterület-ügyfél tárból egy részletes ismertetése Ha az ügyfélnek adott kérést küld a tároló szolgáltatás eléréséhez.

A következő ügyféloldali naplókban a tárhely ügyfél tár által generált a probléma mutatja be, ha az ügyfélnek nem találja a tároló hozza létre azt a blob. A napló részleteit a következő tároló műveletek tartalmazza:

Kérelem azonosítója|Művelet
---|---
07b26a5d-...|**DeleteIfExists** módszer a blob-tárolóhoz törlése. Megjegyzés: ezt a műveletet tartalmaz a **címsor** kérelmének a tároló meglétének ellenőrzése.
e2d06d78...|**CreateIfNotExists** módszer a blob-tárolóhoz létrehozásához. Figyelje meg, hogy a ezt a műveletet, amely ellenőrzi, hogy a tároló megléte **vezetője** kérelmet is tartalmaz. A **címsor** 404-es üzenet adja eredményül, de továbbra is.
de8b1c3c-...|**UploadFromStream** módszer a blob létrehozásához. A **ELHELYEZNI** kérelem sikertelen 404-es üzenettel

Naplóbejegyzések:

Kérelem azonosítója |  A művelet szöveg
---|---
07b26a5d-...|Szinkronizált https://domemaildist.blob.core.windows.net/azuremmblobcontainer kérelem indítása.
07b26a5d-...|StringToSign HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, Jun 2014-es 03 = 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Várakozás a válaszra.
07b26a5d-... | Válasz érkezett. Állapotkód = 200, kérése ID = eeead849... Tartalom-MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Válasz fejlécek Folytatás a többi a művelet sikerült, feldolgozott.
07b26a5d-... | Válasz szervezet letöltése.
07b26a5d-... | A művelet sikeresen befejeződött.
07b26a5d-... | Szinkronizált https://domemaildist.blob.core.windows.net/azuremmblobcontainer kérelem indítása.
07b26a5d-... | StringToSign DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, Jun 2014-es 03 = 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Várakozás a válaszra.
07b26a5d-... | Válasz érkezett. Állapotkód = 202, kérése ID = 6ab2a4cf-..., a tartalom-MD5 =, ETag =.
07b26a5d-... | Válasz fejlécek Folytatás a többi a művelet sikerült, feldolgozott.
07b26a5d-... | Válasz szervezet letöltése.
07b26a5d-... | A művelet sikeresen befejeződött.
e2d06d78-... | Aszinkron https://domemaildist.blob.core.windows.net/azuremmblobcontainer kérelem indítása.</td>
e2d06d78-... | StringToSign HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, Jun 2014-es 03 = 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Várakozás a válaszra.
de8b1c3c-... | Szinkronizált https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt kérelem indítása.
de8b1c3c-... |  StringToSign helyezése =... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:TUE, Jun 2014-es 03 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Felkészülés a kérelem adatok írásához.
e2d06d78-... | Kivételhiba lépett fel várakozás közben: A távoli kiszolgáló hibát: (404) nem található.
e2d06d78-... | Válasz érkezett. Állapotkód = 404, kérése ID = 353ae3bc-..., a tartalom-MD5 =, ETag =.
e2d06d78-... | Válasz fejlécek Folytatás a többi a művelet sikerült, feldolgozott.
e2d06d78-... | Válasz szervezet letöltése.
e2d06d78-... | A művelet sikeresen befejeződött.
e2d06d78-... | Aszinkron https://domemaildist.blob.core.windows.net/azuremmblobcontainer kérelem indítása.
e2d06d78-...|StringToSign helyezése =... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:TUE, Jun 2014-es 03 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Várakozás a válaszra.
de8b1c3c-... | Írás kérelem adatokat.
de8b1c3c-... | Várakozás a válaszra.
e2d06d78-... | Kivételhiba lépett fel várakozás közben: A távoli kiszolgáló hibát: (409) ütközés.
e2d06d78-... | Válasz érkezett. Állapotkód 409, kérése ID = = c27da20e-..., a tartalom-MD5 =, ETag =.
e2d06d78-... | Letöltés hiba válasz törzsébe.
de8b1c3c-... | Kivételhiba lépett fel várakozás közben: A távoli kiszolgáló hibát: (404) nem található.
de8b1c3c-... | Válasz érkezett. Állapotkód = 404, kérése ID = 0eaeab3e-..., a tartalom-MD5 =, ETag =.
de8b1c3c-...| Kivételhiba lépett fel a művelet során: A távoli kiszolgáló hibát: (404) nem található.
de8b1c3c-... | Az újbóli próbálkozás újrapróbálkozási házirend nem engedélyezte. A távoli kiszolgáló a hibás hibát: (404) nem található.
e2d06d78-... | Az újbóli próbálkozás újrapróbálkozási házirend nem engedélyezte. A távoli kiszolgáló a hibás hibát: (409) ütközés.

Ebben a példában az naplója azt mutatja, hogy az ügyfél van (de8b1c3c-...); kihagyásos **CreateIfNotExists** módszer (... kérelem azonosító e2d06d78) a **UploadFromStream** metódus kéréseivel kérései Ez történik, mert az ügyfélalkalmazás aszinkron meghívása a ezeket a módszereket. Annak érdekében, hogy akkor hozzon létre a tároló előtt töltse fel a tároló blob semmilyen adatot az ügyfél lévő aszinkron kódot kell módosítani. Ideális esetben minden tároló előre kell létrehoznia.

#### <a name="SAS-authorization-issue"></a>A megosztott Access-aláírás (Társítások) engedélyezési probléma

Ha az ügyfélalkalmazás megkísérli a szükséges engedélyekkel, a művelet nem tartalmazó Társítások termékkulccsal, a tárhelyszolgáltatáshoz HTTP 404-es (nem található) üzenetben az ügyfél adja eredményül. Egy időben is látni fogja nullától különböző érték **SASAuthorizationError** az a a mérési módja miatt.

A következő táblázat mutatja a tárhely naplózás naplófájlból minta kiszolgálóoldali napló üzenet:

<table>
  <tr>
    <td>Kérés kezdési ideje</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Művelet</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Kérés állapota</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>HTTP-állapotkód</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Hitelesítés típusa</td>
    <td>Társítások</td>
  </tr>
  <tr>
    <td>A szolgáltatás típusa</td>
    <td>BLOB</td>
  </tr>
  <tr>
    <td>Kérelem URL-címe</td>
    <td>
    https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; si = mypolicy&amp;amp; aláírás = XXXXX&amp;amp; api-verzió 2014-02-14 =&amp;amp;</td>
  </tr>
  <tr>
    <td>Kérés azonosító fejléce</td>
    <td>a1f348d5-8032-4912-93EF-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Ügyfél-azonosító kérés</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Miért az ügyfélalkalmazás próbál meg nem kapta engedélyeinek művelet végrehajtása kell vizsgálja meg.

#### <a name="JavaScript-code-does-not-have-permission"></a>Ügyféloldali JavaScript-kód nincs engedélye az objektum elérésére

A JavaScript-ügyfelet használ, és a tárhelyszolgáltatáshoz HTTP 404-es üzenetek ad vissza, ha bejelöli a következő JavaScript hibákat a böngészőben:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Az Internet Explorerben az F12 fejlesztőeszközök segítségével nyomon követheti a üzenetváltások a böngészős és az tárhelyszolgáltatáshoz referenciaként célszerű ügyféloldali JavaScript problémák.

A hibák oka az, hogy a böngészőben, amely az weblapon megakadályozza, hogy elérje az API-t a tartományból származik, az oldal egy másik tartományban található [ugyanazt az origin házirendet](http://www.w3.org/Security/wiki/Same_Origin_Policy) biztonsági korlátozás hajtja végre.

A JavaScript probléma megkerüléséhez Cross Origin erőforrások megosztása (CORS) beállíthatja a tárterület-szolgáltatás az ügyfél éri el. További tudnivalókért lásd: [Azure tárolása határokon származási erőforrások megosztása (CORS) támogatása](http://msdn.microsoft.com/library/azure/dn535601.aspx).

A következő kódot minta megtudhatja, hogy miként állíthatja be a blob szolgáltatást a blob-tároló szolgáltatásban blob eléréséhez a Contoso tartományban futó JavaScript engedélyezése:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Hálózati hiba

Bizonyos körülmények között megszakadt hálózati csomagokat, azokat a HTTP 404-es üzenetek Visszatérés az ügyfél tárhelyszolgáltatáshoz vezethet. Például az ügyfélalkalmazás entitás törlése a táblázat szolgáltatásból jelenik a tárhely kivétel jelentéskészítés throw ügyfél-"HTTP 404 (nem található)" állapotüzenetet a táblázat szolgáltatásból. Ha a táblázatot a táblázat tárhelyszolgáltatáshoz vizsgálatához látni, hogy a szolgáltatást a szervezet did törlése, kérésének.

A kivétel az ügyfél adatait tartalmazza a táblázat szolgáltatás a kérelem rendelt kérelem azonosítója (7e84f12d...): Ez az információ segítségével keresse meg a kérés részleteinek a tárhely kiszolgálóoldali naplók a kereséssel, a naplófájl **kérés azonosító fejlécében** oszlopában. A mérési módja miatt azonosítása, amikor ez például hibák fordul elő, és keresse meg a naplófájlokat, a mérőszámok felvett ezt a hibát időben is használhatja. A naplóbejegyzés jeleníti meg, hogy a törlése sikertelen volt egy "HTTP (404) ügyfél más hiba" Állapot üzenettel. Az azonos naplóbejegyzést is tartalmazza az **ügyfél-kérés-id** (813ea74f...) oszlopában az ügyfél által létrehozott kérelem azonosító.

A kiszolgálóoldali napló is tartalmaz egy másik bejegyzésre azonos értékű **ügyfél-kérés-id** (813ea74f...) a sikeres törléssel azonos személlyel, valamint a azonos ügyfélprogramból. A sikeres törlési művelet történtek nagyon hamarosan a hibás kérés törlése előtt.

Ebben az esetben a legvalószínűbb oka, hogy az ügyfél küldött a szervezet törlés kér a táblázat szolgáltatás, amely sikerült, de nem kapta visszaigazolást a kiszolgálóról (talán miatt egy ideiglenes hálózati probléma) értéke. Az ügyfél majd automatikusan az újból a művelettel (ugyanazt az **ügyfél-kérés-azonosító**), és a kísérletek nem sikerült, mert a szervezet már törlődött.

Ha a probléma gyakran előfordul, meg kell vizsgálja meg a miért nem működnek az ügyfél nyugták fogadásához a táblázat szolgáltatásból. Ha a probléma nem folyamatos, meg kell található, a "HTTP (404) nem található" hiba és bejelentkezés az az ügyfél, de engedélyezi az ügyfél, a folytatáshoz.

### <a name="the-client-is-receiving-409-messages"></a>Az ügyfél HTTP 409 (Ütközés) üzenet érkezik

A következő táblázat mutatja a kiszolgálóoldali naplófájlból két ügyfél műveletekhez kivonat: **DeleteIfExists** azonnal követ **CreateIfNotExists** blob-tárolóhoz azonos nevű használatával. Megjegyzendő, hogy minden ügyfél művelet eredménye két összehívásokban a kiszolgálóra küldött, először a tároló létezésének ellenőrzése, hogy **GetContainerProperties** kérelmének az **DeleteContainer** vagy **CreateContainer** kérésre követi.

Időbélyeg|Művelet|Eredmény|Tároló neve|Ügyfél-azonosító kérés
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|CreateContainer|409|mmcont|bc881924-...

A kódot a ügyfélalkalmazás törli, és azonnal létrehozza az eredeti néven blob-tároló: a **CreateIfNotExists** módszer (ügyfél kérés ID bc881924-...) ahányat sikertelen a HTTP 409 (Ütközés) hiba miatt. Amikor egy ügyfél törli, blob tárolók, táblázatot vagy a neve előtt egy rövid időre van sorban várakozó ismét elérhetővé válik.

Az ügyfélalkalmazás egyedi tároló nevét kell használni, amikor azt hoz létre új tárolók, ha a törlés/hozza létre újra minta közös.

### <a name="metrics-show-low-percent-success"></a>Mértékek megjelenítése a kis PercentSuccess meg, vagy analytics naplóbejegyzések ClientOtherErrors állapotú tranzakció műveletei

A **PercentSuccess** mérőszám rögzíti készültségi szintjének műveletek, amelyekre sikeres a HTTP-állapotkód alapján. Az állapot együtt tevékenységének 2XX megszámolása sikeres, mivel az állapot együtt az 3XX, 4XX és 5XX műveletek sikertelen számítanak, és az alsó **PercentSucess** metrikus értékét. Ezeket a műveleteket a kiszolgálóoldali tárolási naplófájlok **ClientOtherErrors**tranzakció állapotban rögzítése.

Fontos, hogy ne feledje, hogy ezek a műveletek sikeresen befejeződött, és így nincsenek hatással a többi mértékek, például az elérhetőség. A műveletek, amely a sikeres végrehajtás, de az sikertelen a HTTP-állapot kódok járhat, többek között:
- **ResourceNotFound** (Nem található 404), például a blob, amely nem létezik a GET kérésekben.
- **ResouceAlreadyExists** (409 ütközés), például egy **CreateIfNotExist** műveletet, ahol már létezik az erőforrás a.
- **ConditionNotMet** (Nem 304 módosult), például egy feltételes műveletet, például amikor egy ügyfél- **ETag** értéket és egy HTTP **If-nincs – egyezés** fejlécén kérése a képet, csak akkor, ha az utolsó művelet óta változásáról küld-e a.

Gyakori REST API hibakódok, a tárterület-szolgáltatások lapon [Közös REST API-hibakódok](http://msdn.microsoft.com/library/azure/dd179357.aspx)visszaadó listáját megtalálhatja.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapacitás mértékek megjelenítése váratlan növekedését kapacitás tárterület-használat


Hirtelen jelenik meg, ha nem várt változásai kapacitás használatát a tárterület-fiókjában kiderítheti a okok első megjeleníti az elérhetőség mértékek; például növekedését kérések használja, az alkalmazás adott törlési művelet, előfordulhat, hogy rendelkezik várhatóan kell szabadítson fel némi tárhelyet előfordulhat, hogy nem kell a várt módon működnek (például azért, mert a munkaterületi használt Társítások tokenek lejárt) blob-tárolóhoz összegének növekedését vezethet sikertelen törlési számát.

### <a name="you-are-experiencing-unexpected-reboots"></a>Tapasztal az Azure virtuális gépeken futó, nagy számú csatolt VHD váratlan újraindítása

Azure virtuális gép (virtuális) sok csatolt VHD szereplő ugyanazzal a tárterület-fiókkal rendelkezik, ha sikerült haladhatja meg a méretezhetőség célok az egyes tároló ügyfélhez a virtuális lefagyását okozza. Nézze meg a percek mérési módja miatt a tárterület-fiókhoz (**TotalRequests**/**TotalIngress**/**TotalEgress**) az kiugrásainak megfelelő, amelyekre a méretezhetőség célok tároló fiókot. Lásd: a tárterület-fiók meghatározásában, ha szabályozásának segítségért "[Mértékek megjelenítése PercentThrottlingError növekedését]" szakasz történt.

Az általános minden egyes beviteli vagy egy virtuális virtuális számítógépről a Kimenet művelet megfelelője az alapul szolgáló lap blob **Lap első** vagy **Helyezze lap** műveletek. Emiatt a becsült IOPS környezetben segítségével akkor is van egy egyetlen tárolási fiókban az adott tevékenysége alapján az alkalmazás hány VHD finomhangolása. Több mint 40 lemezre kellene egy egyetlen tárterület-fiókban nem javasoljuk. [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md) információt talál az aktuális méretezhetőség célok tároló fiókok, különösen az összes kérelmet ráta és a tárterület-fiók esetén összesített sávszélesség.
Ha a tárterület-fiók túllépte a méretezhetőség célok, csökkentheti a tevékenység minden egyes számlára több különböző tárterület-fiók a VHD kell jelölje.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>A probléma merül fel a tárhely irányító használja a fejlesztés és tesztelése

Általában használja a tárhely irányító fejlesztés alatt, és ellenőrizze, hogy az Azure tároló fiók kötelező elkerülése érdekében. A gyakori kérdések a tárhely irányító használata közben fellépő a következők:

- ["X" funkció nem működik a tárhely irányító a]
- [Hiba "érték közül a HTTP-fejlécek nem szerepel a megfelelő formátumban" a tárhely irányító használata esetén]
- [Rendszergazdai jogosultságokkal a tárhely irányító futtatásához van szükség.]

#### <a name="feature-X-is-not-working"></a>"X" funkció nem működik a tárhely irányító a

A tároló irányító nem támogatja a funkciók a Azure tároló szolgáltatások, például a szolgáltatás. További tudnivalókért lásd: a [használata az Azure tároló irányító fejlesztés és tesztelése](storage-use-emulator.md).

Használja az Azure tárhelyszolgáltatáshoz a felhőben, amely nem támogatja a tárhely irányító funkciókat.

#### <a name="error-HTTP-header-not-correct-format"></a>Hiba "érték közül a HTTP-fejlécek nem szerepel a megfelelő formátumban" a tárhely irányító használata esetén

Az alkalmazás, amely a tárterület-ügyfél tárral szemben a helyi tároló irányító tesztelése és módszer meghívja, például **CreateIfNotExists** hibaüzenettel a "érték közül a HTTP-fejlécek nem szerepel a megfelelő formátumban." Ez azt jelzi, hogy a tárhely irányító, használja a verziója nem támogatja a tárhely ügyfél-tár esetén verzióját. A tároló ügyfél tár hozzáadása van az összes kérelmek a fejléc **x-ms-verzió** . A tároló irányító nem ismeri fel az **x-ms-verzió** fejlécében az érték, ha a rendszer elutasítja a kérést.

A tároló tár ügyfél naplók küld, **x-ms-verzió élőfej** érték is használhatja. Ha a Fiddler kérelmek nyomon követheti az ügyfél-alkalmazásokból, az érték az **x-ms-verzió élőfej** is megjelenik.

Ebben az esetben általában fordul elő, ha telepíti, és a tárterület-ügyfél tár legújabb verzióját használja a tárhely irányító frissítése nélkül. Meg kell a tároló irányító legújabb verziójának telepítéséhez vagy felhőbeli tárhelyről használata helyett a irányító teszteléshez és.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Rendszergazdai jogosultságokkal a tárhely irányító futtatásához van szükség.

Kéri a rendszergazdai hitelesítő adatok a tárhely irányító futtatásakor. Ez csak történik, amikor a program a tárhely irányító először inicializálja. Miután a tárhely irányító van inicializálni, szükségtelen futtassa újra a rendszergazdai jogokkal.

További tudnivalókért lásd: a [használata az Azure tároló irányító fejlesztés és tesztelése](storage-use-emulator.md). Figyelje meg, hogy a Visual Studióban, rendszergazdai jogokkal is szüksége lesz a tárhely irányító is inicializálni is.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Akkor is problémákba ütközik az Azure SDK telepítése a .NET rendszerhez

A tároló irányító telepíteni a helyi számítógépre szeretné kísérli meg telepíteni a SDK csomagjában talál, meghiúsul. A telepítési napló az alábbi üzenetek valamelyikét tartalmazza:

- CAQuietExec: Hiba: nem fér hozzá az SQL-példány
- CAQuietExec: Hiba: nem lehet adatbázis létrehozása

A meglévő LocalDB telepítési probléma oka. Alapértelmezés szerint a tárhely irányító segítségével LocalDB adatok továbbra is fennáll, amikor azt keltő szűrő megőrzi a Azure tároló szolgáltatást. Az alábbi parancsok futtatásával egy parancssorablakot előtt a SDK telepíteni szeretné az alaphelyzetbe állíthatja a LocalDB példány.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

A **Törlés** parancs bármely régi adatbázisfájlok eltávolítja a tárhely irányító korábbi telepítéseit.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Egy tárhelyszolgáltatáshoz egy másik problémája van

Ha az előző hibaelhárítási szakaszok nem adja meg a problémát tapasztal a tárhelyszolgáltatáshoz, el kell fogadnia a következő megközelítés diagnosztizálása és hibaelhárítás a problémát.

- Jelölje be a mértékek, annak ellenőrzéséhez, hogy a várt viszonyítási tevékenysége bármilyen változás van. A mértékek, az akkor határozza meg, hogy a probléma ideiglenes (tranziens) vagy állandó, és mely tárolási műveletek gyengíti a problémát.
- A mértékek információk segítségével keresése a kiszolgálóoldali naplóadatok hibák történnek részletesebb információt nyújt segítséget. Ez az információ segíthet a hibaelhárítási, és a probléma megoldásához.
- Ha az adatokat a kiszolgálóoldali naplók nem elegendő sikeresen elhárítani a problémát, a tárhely ügyfél tár ügyféloldali naplók vizsgálja meg a az ügyfélalkalmazás és a például Fiddler, Wireshark és Microsoft üzenet Analyzer vizsgálja meg a hálózati eszközök is használhatja.

Fiddler használatával kapcsolatos további tudnivalókért lásd: "[függelék 1: Fiddler segítségével rögzítheti a http- és HTTPS-forgalom]."

Wireshark használatával kapcsolatos további tudnivalókért lásd: "[függelék 2: segítségével a wireshark eszközben rögzíti a hálózati forgalmat]."

Microsoft üzenet Analyzer használatával kapcsolatos további tudnivalókért lásd: "[függelék 3: segítségével a Microsoft Message Analyzer rögzíti a hálózati forgalmat]."

## <a name="appendices"></a>Függelékek

A függelékek számos eszközt, akkor azt tapasztalhatja hasznos diagnosztizálása és Azure tároló (és más szolgáltatások) problémák vannak ismertetik. Ezek az eszközök amelyek nem részei az Azure tárhely, és a harmadik féltől származó termékeket. Például az alábbi függelékek tárgyalt eszközök nem vonatkoznak bármely támogatási szerződés, előfordulhat, hogy a Microsoft Azure vagy Azure tárhely, és ezért a frissítési folyamat részeként megvizsgálja licencelési és támogatási elérhető beállítások a szolgáltatóktól ezek közül az eszközök.

### <a name="appendix-1"></a>1. melléklet: Fiddler segítségével rögzítheti a http- és HTTPS-forgalom

[Fiddler](http://www.telerik.com/fiddler) hasznos eszköz elemzése a http- és HTTPS forgalom az ügyfélalkalmazás és használja az Azure tárhelyszolgáltatáshoz között.

> [AZURE.NOTE] Fiddler is dekódolását HTTPS-forgalom; olvassa el körültekintően megértéséhez, hogy ezt nem, és a biztonsági következmények megértéséhez Fiddler át.

Ebben a függelékben egy rövid áttekintése a következő konfigurálása Fiddler rögzíti a forgalmat a helyi számítógépen, amelyen telepítve van a Fiddler és Azure tároló szolgáltatások között.

Miután Fiddler van indul el, indít el rögzítése a http- és HTTPS-forgalmat a helyi számítógépre. Az alábbiakban néhány hasznos parancsokat Fiddler szabályozása:

- Állítsa le, és a forgalmat a Rögzítés indítása. A fő menüben válassza a **fájl** , és válassza a **Rögzítés forgalom** válthat a rögzítés be- és kikapcsolása.
- Rögzített forgalom adatokat is menteni. A fő menüben válassza a **fájlt**, kattintson a **Mentés**gombra, és kattintson az **Összes munkamenetet**: Ez lehetővé teszi, hogy mentse a forgalmat a munkamenet archív mappák. A munkamenet Archívumokhoz újabb elemzési verzió ismételt betöltése, vagy küldje el, ha szükséges, a Microsoft támogatási.

Ha szeretné korlátozni, amely jellemzi Fiddler forgalmának összege, szűrők a **szűrők** lapon konfigurálható is használhatja. Az alábbi képernyőképen látható szűrő, amely a **contosoemaildist.table.core.windows.net** tároló végpont adatforgalom rögzíti:

![][5]

### <a name="appendix-2"></a>2. melléklet: A wireshark eszközben használatával rögzíti a hálózati forgalmat

[Wireshark](http://www.wireshark.org/) egy hálózati protokoll analyzer, amely lehetővé teszi, hogy a hálózati protokollokat széles köre csomag részletes információinak megtekintése.

Az alábbi eljárás bemutatja, hogyan részletes csomag adatainak a forgalmat a helyi gépről való rögzítésére amelyen telepítette wireshark eszközben a táblázat szolgáltatás a Azure tárterület-fiókjában.

1.  Indítsa el a wireshark eszközben a helyi számítógépre.
2.  **Indítsa el** csoportjában válassza a helyi hálózati kapcsolat vagy felületek, hogy kapcsolódik az internethez.
3.  Kattintson a **rögzítési beállítások**gombra.
4.  Szűrő hozzáadása a **Rögzítési szűrő** mezőben lévő értéket. **Host contosoemaildist.table.core.windows.net** például fog konfigurálni csak azok a csomagok küld vagy fogad a táblázat szolgáltatás végpontjának **contosoemaildist** tároló fiók rögzítése a wireshark eszközben. Nézze meg a [Rögzítés szűrők teljes listáját](http://wiki.wireshark.org/CaptureFilters).

    ![][6]

5.  Kattintson a **Start**gombra. Wireshark átirányítása összes csomagok küldeni vagy a táblázat szolgáltatás végpontjának az ügyfélalkalmazás használata a helyi számítógépen most.
6.  Amikor befejezte, kattintson a fő menü **Rögzítés** , majd **állítsa le**.
7.  A rögzített adatok a wireshark eszközben rögzítése fájl mentéséhez a fő menüben kattintson a **fájl** , majd **mentése**elemre.

WireShark kiemeli a hibák, amelyek szerepelnek a **packetlist** ablakban. Is használhatja a **Szakértői szintig információ** ablak (kattintson az **elemzés**, majd a **Szakértői szintig információ**) megtekintése a útmutatója.

![][7]

Kiválaszthatja a TCP-adatok megjelenítése az alkalmazási réteg láthassa a jobb gombbal a TCP-adatok a kiválasztása, **hajtsa végre a TCP-adatfolyam**is. Az különösen akkor hasznos, ha, rögzített a kiírása rögzítési szűrő nélkül. További tudnivalókért lásd: [követően a TCP-adatfolyam] (http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [AZURE.NOTE] A wireshark eszközben használatával kapcsolatos további tudnivalókért lásd: a [Wireshark eszközben felhasználók útmutató](http://www.wireshark.org/docs/wsug_html_chunked).

### <a name="appendix-3"></a>3. melléklet: Microsoft üzenet Analyzer használatával hálózati forgalmának engedélyezésére rögzítése

A Microsoft üzenet Analyzer segítségével rögzítheti a http- és HTTPS-forgalom Fiddler hasonló módon, és a wireshark eszközben hasonló módon rögzíti a hálózati forgalmat.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Használja a Microsoft üzenet Analyzer webes nyomkövetési munkamenet konfigurálása

Egy webes nyomkövetési munkamenet használata a Microsoft üzenet Analyzer HTTP és HTTPS forgalom konfigurálása, futtassa a Microsoft üzenet Analyzer alkalmazást, és a **fájl** menüben kattintson a **Rögzítés/nyomkövetési**. Használható nyomkövetési esetek listájában válassza a **Webes Proxy**. Kattintson a **Nyomkövetési forgatókönyv konfigurációjának** panelen az **HostnameFilter** mezőbe a tárhely végpontok (megjeleníthetők a nevek az [Azure-portálon](https://portal.azure.com)) nevét adja. Azure tároló fiókja neve **contosodata**, célszerű a **HostnameFilter** mezőben lévő értéket szeretné például hozzáadása a következő:

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Egy szóközt elválasztja a állomásnevekké.

Ha készen áll a kezdésre nyomkövetési adatok gyűjtése, kattintson a **Start** gombra.

A Microsoft üzenet Analyzer **Web Proxy** követés kapcsolatos további tudnivalókért olvassa el a [Microsoft-PEF-WebProxy szolgáltató](http://technet.microsoft.com/library/jj674814.aspx)című témakört.

A beépített **Web Proxy** nyomon követésében Microsoft üzenet Analyzer alapuló Fiddler; ügyféloldali HTTPS-forgalom rögzítheti és titkosítatlan HTTPS-üzeneteket jeleníthet meg. A **Web Proxy** követés működik, az összes HTTP és HTTPS-alapú forgalmat, amely azt hozzáférést biztosít titkosított üzenetek helyi proxy konfigurálásával.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Használja a Microsoft üzenet Analyzer hálózati hibák diagnosztizálása

A a Microsoft üzenet Analyzer **Web Proxy** követés rögzítése a HTTP/HTTPs-forgalom az ügyfélalkalmazás és a tároló szolgáltatás között részleteit, mellett is használhatja a beépített **Helyi kapcsolat réteg** követés hálózati csomag információk rögzítése. Ez lehetővé teszi, hogy a hasonló, amely a wireshark eszközben rögzítése és diagnosztizálása adatok rögzítése hálózati problémák például kihagyott csomagokat, azokat.

Az alábbi képernyőképen látható az egyes **tájékoztató** üzenetek **Helyi kapcsolat réteg** nyomkövetési **DiagnosisTypes** oszlopban. Az üzenet a Részletek gombra kattintva egy ikonra mutat, a **DiagnosisTypes** oszlopban látható. Ebben a példában a kiszolgáló üzenet #305 újraküldött, mert azt nem kapta visszaigazolást az ügyféltől:

![][9]

A nyomkövetés-munkamenetet a Microsoft üzenet Analyzer létrehozásakor a naplóban zajt mennyiségének csökkentésére szűrőket is megadhat. **Rögzíthet és nyomon követése** lapon, ahol megadhatja a követés gombra a **Microsoft-Windows-NDIS-PacketCapture**melletti **beállítás** hivatkozásra. Az alábbi képernyőképen látható, hogy a TCP-forgalmat három tároló szolgáltatások IP-címek szűri a konfiguráció:

![][10]

A Microsoft üzenet Analyzer helyi kapcsolat réteg követés kapcsolatos további tudnivalókért olvassa el a [Microsoft-PEF-NDIS-PacketCapture szolgáltató](http://technet.microsoft.com/library/jj659264.aspx)című témakört.

### <a name="appendix-4"></a>. Melléklet 4: Excel használatával megtekintheti a mértékek, és jelentkezzen be az adatok

Számos eszközt lehetővé teszi a tárolási mértékek adatok letöltése a Azure táblatárolóhoz tagolt formátumban, amely megkönnyíti a betöltheti az adatokat az Excelbe megtekintésre és elemzés. Azure blob-tárolóhoz tároló naplózási adatainak már betöltheti az Excelbe tagolt formátumban. Jó helyen jár szüksége lesz az adatokat [Tároló Analytics napló formátum](http://msdn.microsoft.com/library/azure/hh343259.aspx) és [Tároló Analytics mértékek táblázat séma](http://msdn.microsoft.com/library/azure/hh343264.aspx)alapján megfelelő oszlopfejlécek hozzáadása.

A tároló naplózás adatok importálhatja az Excelbe, blob-tárolóból letöltése után:

- Az **adatok** menü kattintson **A szövegből**gombra.
- Tallózással keresse meg a naplófájl szeretné megtekinteni, és kattintson az **Importálás**gombra.
- A lépés: 1 a **Szövegbeolvasó varázslóban**válassza a **tagolt**.

A lépés: 1, a **Szövegbeolvasó varázslóban**, jelölje be a **pontosvessző** csak elválasztóként, és válassza a, a **szövegjelölők**idézőjel. Kattintson a **Befejezés gombra** , majd válassza ki az adatokat a munkafüzet helyét.

### <a name="appendix-5"></a>. Melléklet 5: Visual Studio Team Services alkalmazás háttérismeretek figyelés

Az alkalmazás az összefüggéseket funkció Visual Studio Team Services a teljesítmény és elérhetőség ellenőrzése részeként is használhatja. Ez az eszköz van lehetősége:

- Ellenőrizze, hogy a webes szolgáltatás elérhető és válaszol. -E az alkalmazást egy webhelyre vagy egy eszköz alkalmazás, amely egy webes szolgáltatást használja, azt az URL-cím néhány percenként ellenőrizze a világon helyekről, és tudatja, ha probléma merül fel.
- Gyorsan diagnosztizálása bármely teljesítménnyel kapcsolatos problémák vagy a webszolgáltatás kivételek. Megtudhatja, emberalakot is megjeleníthet Processzor vagy más erőforrások van folyamatban, Papírhalom halad beolvasása kivételek és könnyen keresni az napló nyomkövetések. Ha az alkalmazás teljesítményének elfogadható korlátozások alá esik, akkor is küldhetnek Önnek e-mailben. Figyelheti a .NET rendszerhez és a Java webszolgáltatásokhoz.

A további információt találhat [Mi az alkalmazás az összefüggéseket?](../application-insights/app-insights-overview.md).

<!--Anchors-->
[– Bevezetés]: #introduction
[Hogyan vannak rendezve, ez az útmutató]: #how-this-guide-is-organized

[A tároló szolgáltatás figyelése]: #monitoring-your-storage-service
[Szolgáltatás állapota figyelése]: #monitoring-service-health
[Kapacitásának figyelése]: #monitoring-capacity
[Elérhetőség ellenőrzése]: #monitoring-availability
[A teljesítmény figyelése]: #monitoring-performance

[Tárterület problémák diagnosztizálása]: #diagnosing-storage-issues
[A szolgáltatás szolgáltatásállapot-hiba]: #service-health-issues
[Teljesítménnyel kapcsolatos problémák]: #performance-issues
[Hibák diagnosztizálása]: #diagnosing-errors
[Tárterület irányító kapcsolatos problémák]: #storage-emulator-issues
[Tárterület naplózási eszközök]: #storage-logging-tools
[Hálózati naplózási eszközök használatával]: #using-network-logging-tools

[Végpont nyomkövetés]: #end-to-end-tracing
[Naplóadatok használatával történik]: #correlating-log-data
[Ügyfél-azonosító kérés]: #client-request-id
[Kiszolgáló kérelem azonosítója]: #server-request-id
[Időbélyegeket]: #timestamps

[Hibaelhárítási útmutatás]: #troubleshooting-guidance
[Mértékek magas AverageE2ELatency és az alacsony AverageServerLatency megjelenítése]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Mértékek alacsony AverageE2ELatency és az alacsony AverageServerLatency megjelenítése, de az ügyfél tapasztalható nagy késést]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Mértékek magas AverageServerLatency megjelenítése]: #metrics-show-high-AverageServerLatency
[Az üzenet kézbesítését várólista a váratlan késések tapasztalja]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Mértékek PercentThrottlingError növekedését megjelenítése]: #metrics-show-an-increase-in-PercentThrottlingError
[PercentThrottlingError tranziens növelése]: #transient-increase-in-PercentThrottlingError
[Állandó növekedése PercentThrottlingError hiba]: #permanent-increase-in-PercentThrottlingError
[Mértékek PercentTimeoutError növekedését megjelenítése]: #metrics-show-an-increase-in-PercentTimeoutError
[Mértékek PercentNetworkError növekedését megjelenítése]: #metrics-show-an-increase-in-PercentNetworkError

[Az ügyfél HTTP 403 (tiltott) üzenet érkezik]: #the-client-is-receiving-403-messages
[Az ügyfél fogadja üzeneteket HTTP 404-es (nem található)]: #the-client-is-receiving-404-messages
[Az ügyfél vagy egy másik folyamat korábban törli az objektum]: #client-previously-deleted-the-object
[A megosztott Access-aláírás (Társítások) engedélyezési probléma]: #SAS-authorization-issue
[Ügyféloldali JavaScript-kód nincs engedélye az objektum elérésére]: #JavaScript-code-does-not-have-permission
[Hálózati hiba]: #network-failure
[Az ügyfél HTTP 409 (Ütközés) üzenet érkezik]: #the-client-is-receiving-409-messages

[Mértékek megjelenítése a kis PercentSuccess meg, vagy analytics naplóbejegyzések ClientOtherErrors állapotú tranzakció műveletei]: #metrics-show-low-percent-success
[Kapacitás mértékek megjelenítése váratlan növekedését kapacitás tárterület-használat]: #capacity-metrics-show-an-unexpected-increase
[Amit tapasztal, nagy számú csatolt VHD virtuális gépeken futó a váratlan újraindítása]: #you-are-experiencing-unexpected-reboots
[A probléma merül fel a tárhely irányító használja a fejlesztés és tesztelése]: #your-issue-arises-from-using-the-storage-emulator
["X" funkció nem működik a tárhely irányító a]: #feature-X-is-not-working
[Hiba "érték közül a HTTP-fejlécek nem szerepel a megfelelő formátumban" a tárhely irányító használata esetén]: #error-HTTP-header-not-correct-format
[Rendszergazdai jogosultságokkal a tárhely irányító futtatásához van szükség.]: #storage-emulator-requires-administrative-privileges
[Akkor is problémákba ütközik az Azure SDK telepítése a .NET rendszerhez]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Egy tárhelyszolgáltatáshoz egy másik problémája van]: #you-have-a-different-issue-with-a-storage-service

[Függelékek]: #appendices
[1. melléklet: Fiddler segítségével rögzítheti a http- és HTTPS-forgalom]: #appendix-1
[2. melléklet: A wireshark eszközben használatával rögzíti a hálózati forgalmat]: #appendix-2
[3. melléklet: Microsoft üzenet Analyzer használatával hálózati forgalmának engedélyezésére rögzítése]: #appendix-3
[. Melléklet 4: Excel használatával megtekintheti a mértékek, és jelentkezzen be az adatok]: #appendix-4
[. Melléklet 5: Visual Studio Team Services alkalmazás háttérismeretek figyelés]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
