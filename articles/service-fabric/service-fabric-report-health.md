<properties
   pageTitle="Adja hozzá egyéni szolgáltatás háló állapotjelentések |} Microsoft Azure"
   description="Ismerteti, hogyan küldhet egyéni állapotjelentések Azure Service háló állapot szervezetek. Javaslatok ad megtervezése és minőségű állapotjelentések végrehajtása."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="add-custom-service-fabric-health-reports"></a>Adja hozzá egyéni szolgáltatás háló állapotjelentések
Azure Service háló zászlókat sérült fürt és konkrét személyek alkalmazás feltételek verziójához [állapot modell](service-fabric-health-introduction.md) vezet be. A rendszerállapot modell **állapot közötti** (rendszer összetevőit és watchdogs) használja. A cél, könnyen és gyorsan diagnosztizálása és javítása. Szolgáltatás szövegírói kell figyelni a állapota előzetes. Bármelyik feltétel, amely hatással lehet a rendszerállapot kell jelenteni, különösen akkor, ha segítheti a legfelső szintű közelébe jelző problémákat. A rendszerállapot adatait sok időt is mentheti, és hibakeresés és a vizsgálat egyszer a szolgáltatás munka lépéseket a méretezés a felhőben (személyes vagy Azure).

A szolgáltatás háló közötti monitor érdeklődésre számot tartó feltételek azonosította. Azok a helyi nézeten alapuló feltételektől jelentést. Az [állapot tárolása](service-fabric-health-introduction.md#health-store) összesíti küldött összes közötti határozza meg, hogy-e globálisan megfelelő személyek állapot-adatok. A modell szolgálja gazdag, rugalmas és könnyen használható. A állapotjelentések minősége határozza meg, hogy az állapot megtekintése a fürt pontosságát. Téves tévesen megjelenítése a sérült problémák negatív hatással lehet a frissítéseket és más szolgáltatások, amelyek állapot adatokat. Ezek a szolgáltatások példák javítása és figyelmeztető mechanizmusok. Adja meg a jelentéseket készíthet a feltételt a lehető legjobb módon érdeklődésre számot tartó rögzítése ezért néhány gondolkodási van szükség.

Tervezhet és megvalósítása a jelentés az állapot, watchdogs, és a rendszer összetevőit kell:

- A feltétel érdeklik a folyamatos figyelése módjának és hatása meg a fürt vagy alkalmazás funkciókat. Az információk alapján, döntse el, az állapot Jelentéstulajdonság és a rendszerállapot.

- Megállapíthatja, hogy a [szervezet](service-fabric-health-introduction.md#health-entities-and-hierarchy) a jelentés vonatkozik.

- Határozza meg, hol található a jelentési kész, a belül a szolgáltatás vagy egy belső és külső figyelő.

- Definiáljon bejelentő azonosítja.

- Jelentéskészítési stratégia, válassza a rendszeres vagy a áttűnések. Az ajánlott módszereket megegyezik rendszeres időközönként, egyszerűbb kód igényel, és azt kevésbé érzékenyek hibákat.

- Mennyi ideig a jelentést a sérült feltételek maradjanak az állapot áruházban, és hogyan kell bejelölve határozzák meg. Ezen információk alapján adja meg a jelentés idő élő és eltávolítása a jelszólejárati a viselkedés.

Említett, jelentése történik:

- A felügyelt szolgáltatás háló szolgáltatás kópia.

- Belső watchdogs szolgáltatás háló szolgáltatásként (például szolgáltatás háló állapot nélküli feltételek figyeli és szolgáltatás jelentések problémák) rendszerbe. A watchdogs lehet egy csomópontjait rendszerbe vagy a megfigyelt szolgáltatás affinitized is lehet.

- A szolgáltatás háló csomópontok futtatni, de belső watchdogs szolgáltatás háló szolgáltatásként *nincs* implementálva.

- Az erőforrás eltávolítása *kívül* a szolgáltatás háló fürt (például felügyeleti szolgáltatásba, például a Gomez) probe külső watchdogs.

> [AZURE.NOTE] Beépített a fürt kitölti a rendszer összetevők által küldött állapotjelentések. További a [rendszer állapotjelentések használata az alábbiakkal](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). A felhasználó jelentéseket a [rendszerállapot személyek](service-fabric-health-introduction.md#health-entities-and-hierarchy) , a rendszer által létrehozott kell elküldeni.

Egyszer az állapot jelentés a Tervező nincs bejelölve, állapotjelentések elküldésének egyszerűen. Jelentés állapota [FabricClient](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.aspx) használható, ha a fürt nem [biztonságos](service-fabric-cluster-security.md) , vagy ha a háló ügyfél rendelkezik-e rendszergazdai jogosultságait. Ez történik az API-k [FabricClient.HealthManager.ReportHealth](https://msdn.microsoft.com/library/system.fabric.fabricclient.healthclient.reporthealth.aspx)használatával, Powershellen keresztül, vagy a többi. A köteg nagyobb teljesítmény jelentések konfigurációs belül.

> [AZURE.NOTE] Jelentés állapot szinkron, és ez azt jelenti, hogy csak az adatérvényesítési munka ügyféloldali. A FAKT, hogy a jelentés az állapot ügyfél által elfogadása vagy a `Partition` vagy `CodePackageActivationContext` objektumok nem jelenti azt, hogy a tárolóban lévő sor. Aszinkron küldött, és az egyéb jelentések esetleg kötegelt. A kiszolgálón feldolgozása továbbra is előfordulhat: lehet, hogy a sorszám elavult, a szervezet, amelyen a jelentés kell alkalmazni már töröl, stb.

## <a name="health-client"></a>Ügyfél állapota
A állapotjelentések elküldi az állapot tároló állapot ügyfél, amelyben lakik belül a háló ügyfél keresztül. Az állapot ügyfél állítható be a következő:

- **HealthReportSendInterval**: A késleltetés a megjelenik a jelentés az ügyfél és az idő az állapot Store elküldött között. Használt köteg jelentések egy üzenettel, hanem egy üzenet küldése minden jelentéshez. A kötegelés javítja a teljesítményt. Alapértelmezés: 30 másodperc.

- **HealthReportRetrySendInterval**: az intervallumra, amelynél az állapot ügyfél újraküldi halmozott állapot jelenti az állapot Store. Alapértelmezés: 30 másodperc.

- **HealthOperationTimeout**: az időkorlát-jelentés az állapot tároló küldött üzenet. Üzenet időtúllépés történik, ha az állapot ügyfél azt próbálkozások, amíg az állapot tároló megerősíti, hogy a jelentés feldolgozása. Alapértelmezés: két percig tart.

> [AZURE.NOTE] A jelentések kötegelt vannak, ha a háló ügyfél kell tartandó az legalább az HealthReportSendInterval annak érdekében, hogy azok kapnak. Ha az üzenet elvész, vagy az állapot tároló nem alkalmazza őket a tranziens hibák miatt, a háló ügyfél kell tartandó már biztosítani neki a névjegyadatok próbálkozzon újra.

Az ügyfélgépen pufferelési megnyitja a jelentések egyediségét figyelembe. Ha például egy adott hibás reporter jelez ugyanahhoz a személyhez azonos tulajdonsága másodpercenként 100 jelentések, a jelentések felülírják utolsó verziójával. Csak egy kimutatást legfeljebb létezik a ügyfél várakozási sorban található. Ha kötegelés be van állítva, a jelentés az állapot tároló küldött egy / Küldés időköz csak egy. Ez a jelentés az utolsó hozzáadott jelentést, amely a szervezet legfrissebb állapotát tükrözi.
Az összes konfigurációs paramétereket lehet megadott mikor `FabricClient` állapot kapcsolódó bejegyzéseket a kívánt értékeket tartalmazó [FabricClientSettings](https://msdn.microsoft.com/library/azure/system.fabric.fabricclientsettings.aspx) megkerülhetők jön létre.

A következő háló ügyfél hoz létre, és itt adhatja meg, hogy a jelentések küld-e, amikor hozzáadja őket. Újrapróbálkozások időkorlátok és a hibákról is meg kell ismételni fordulhat elő, 40 másodpercenként.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Paramétereket fürtre kapcsolat Powershellen keresztül létrehozásakor adható meg. A következő helyi fürtre kapcsolat indítása:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

> [AZURE.NOTE] Annak érdekében, hogy a nem engedélyezett szolgáltatások nem jelentés állapota a szervezetek a fürt szemben, a kiszolgáló beállítható úgy, hogy csak a védett ügyfelektől érkező kérések fogadja el. A `FabricClient` jelentéskészítési kell biztonsági engedélyezte engedélyezni szeretné a fürt (például hitelesítéssel Kerberos vagy tanúsítvány) kommunikálni. További információ a [fürt biztonsági](service-fabric-cluster-security.md).

## <a name="report-from-within-low-privilege-services"></a>Jelentés az alacsony jogosultság szolgáltatások belül
A szolgáltatás háló szolgáltatásokat, amelyek nem rendelkezik rendszergazdai hozzáférés a fürthöz, belül nyilvántartástól állapot keresztül az aktuális környezetben a szervezetek `Partition` vagy `CodePackageActivationContext`.

- Az állapot nélküli-szolgáltatások [IStatelessServicePartition.ReportInstanceHealth](https://msdn.microsoft.com/library/system.fabric.istatelessservicepartition.reportinstancehealth.aspx) használatával az aktuális szolgáltatás példány jelentést.

- Az állapot-nyilvántartó-szolgáltatások [IStatefulServicePartition.ReportReplicaHealth](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.reportreplicahealth.aspx) segítségével aktuális replika jelentést.

- [IServicePartition.ReportPartitionHealth](https://msdn.microsoft.com//library/system.fabric.iservicepartition.reportpartitionhealth.aspx) használatával az aktuális partíciót személyhez jelentést.

- [CodePackageActivationContext.ReportApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportapplicationhealth.aspx) jelentheti a jelenlegi alkalmazás használata

- Használat [CodePackageActivationContext.ReportDeployedApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth.aspx) is jelenteni az alkalmazást, az aktuális csomópont rendszerre telepíthető.

- Használat [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth.aspx) jelentheti a szolgáltatás csomag az aktuális alkalmazás az aktuális csomópont rendszerre telepíthető.

> [AZURE.NOTE] Belső a `Partition` és a `CodePackageActivationContext` tartsa lenyomva az ujját a rendszerállapot ügyfélnek, amely van beállítva alapértelmezett beállításokkal. Ugyanezek magyarázata [állapot](service-fabric-report-health.md#health-client) ügyfél alkalmazása - jelentések elküldött időzítőt, így az objektumokat, hogy a jelentést küldhet a névjegyadatok életben őrizze és kötegelt.

## <a name="design-health-reporting"></a>Állapot jelentéskészítés tervezése
Kiváló minőségű-jelentések létrehozása az első lépés a feltételeket, amelyeket hatással lehet a szolgáltatás állapotát jelző azonosítja. Bármelyik feltétel, amely segít a szolgáltatásra vagy fürt jelző hibák – elindul, vagy a még jobb, mielőtt a probléma történik – is esetleg dollárban milliárd menteni. Kevesebb időt lefelé többek között a előnyöket nyújtja, és kevesebb éjszakai órát töltött vizsgálat alatt, és a problémák és magasabb szintű felhasználói elégedettséget javítása.

Miután a feltételeket azonosítja, figyelő szövegírói ki kell deríteni a legjobb módszer a terhelést és használhatóságát közötti egyensúly követésére. Például érdemes megvizsgálni szolgáltatás, amely tartalmaz néhány ideiglenes fájlokat a megosztott használó összetett számítások. Egy figyelő figyelni a megosztás kattintva győződjön meg róla, hogy elég hely érhető el. Azt is figyelik fájl-vagy az értesítéseket. Esetleg jelentés egy figyelmeztető üzenetet, ha egy előzetes megkapják és jelzett hibát, ha a megosztás megtelt. A figyelmeztetés, a javítás rendszert sikerült indítsa el a megosztott régebbi fájlok törlése. A hiba a javítás rendszert a szolgáltatás replika sikerült áthelyezése másik csomópontra. Figyelje meg, hogyan a feltétel államok leírt értelmez állapota: a feltételnek megfelelő tekinthető állapotának vagy megsérült (figyelmeztetés vagy a hiba).

A felügyeleti részletek vannak beállítva, miután egy figyelő író kell tudni, hogyan tudnak megvalósítani a figyelő. Ha a meghatározott feltételek a belül a szolgáltatást, a figyelő lehet maga a megfigyelt szolgáltatás részét. Például a szolgáltatáskód ellenőrizheti a megosztás használatát, és ezután jelentés helyi háló ügyfél használatával, minden alkalommal próbál írni egy fájlt. Az eljárás előnye, hogy jelentéskészítés egyszerű. Ügyelni kell a szolgáltatást, bár érintő hibák figyelő megakadályozása.

Jelentéskészítés magából a megfigyelt szolgáltatás lehetőség nem mindig. Előfordulhat, hogy egy figyelő belül a szolgáltatás nem tud feltárása a feltételeket. Nem lehet, hogy a meghatározás adatok és a összefüggés. Lehet, hogy az általános felügyeleti feltételek magas. A feltételek is nem lehet egy adott szolgáltatáshoz, de inkább befolyásolhatja a kapcsolati szolgáltatásai között. Egy másik, hogy a fürt watchdogs rendelkeznek, mint külön folyamatot. A watchdogs figyelheti a feltételeket és a jelentést, egyszerűen a fő szolgáltatásokat semmilyen módon megtartásával. Például a következő watchdogs sikerült kell végrehajtani, állapot nélküli szolgáltatásként ugyanabban az alkalmazásban, minden csomóponton vagy a szolgáltatás azonos csomóponton rendszerbe.

Időnként előfordulhat egy fut a fürt figyelő lehetőség nem vagy. Ha a felügyelt feltétel megegyezik a elérhetősége vagy a szolgáltatás funkciók felhasználók megtekinthessék, célszerű a watchdogs van a felhasználó ügyfelek ugyanazon a helyen. Van azok tesztelheti a műveletek ugyanúgy felhasználók hívja őket. Ha például egy figyelő kívül a fürt él és problémák kérelmek a szolgáltatás, amely ellenőrzi a késés és az eredmény helyességét is. (Számológép szolgáltatáshoz, például nem 2 + 2 szerepelniük 4 lehetővé teszi időtartama?)

Miután a figyelő részletek kialakítása kell használata mellett dönt egyedi módon azonosító forrás Azonosítót. Ha a fürt élő több watchdogs azonos típusú, vagy azok kell jelentést más szervezetek, vagy azonos személlyel a jelentést, ha azok gondoskodnia kell arról, hogy a forrás azonosító és a tulajdonság nem azonos. Ezzel a módszerrel a jelentéseket is megtalálhatók. Az állapotjelentés tulajdonsága irányítsa a megfigyelt feltétel. (A fenti példa a tulajdonság lehet **ShareSize**.) Ha több jelentést ugyanezt a feltételt alkalmazni, a tulajdonság tartalmaznia kell néhány dinamikus információt, amely lehetővé teszi a jelentések való együttműködéshez. Ha például több megosztás kell ellenőrizhető, a tulajdonság neve **ShareSize-megosztásnév**lehet.

> [AZURE.NOTE] Az állapot tároló kell *nem* szeretné, hogy állapot adatai használható. Állapot, csak a rendszerállapot kapcsolódó adatokat kell jelenteni, ez az információ entitás állapot értékelése hatással van. Az állapot tároló nem készült egy általános célú tárolót megegyezik. Adatok összesítése be a rendszerállapot állapot kiértékelés logika használ. Állapot (például állapot az OK gombra a állapotának jelentése) nem kapcsolódó információk küldése nincs hatással a összesített rendszerállapot, de az állapot tároló teljesítményének negatív hatással lehet.

A következő döntési pont jelentés melyik entitás be kapcsolva. A legtöbb esetben az egyértelmű, a feltétel alapján. A szervezet legjobb lehetséges granularitással kell választania. Ha a megadott feltétel hatással van az összes partíciót kópiája, jelentést partíciót, nem pedig a szolgáltatást. Előfordulhatnak olyan sarok esetek, ha több gondolkodási van szükség, bár. Ha a feltétel egy egyed, például kópiában hatással van, de óhaját az, hogy a megjelölt replika leírási_idő, hosszának több feltétel, majd a partíciót kell megadni. Egyéb esetben a replika törlésekor társított összes jelentés fog kiüríteni a áruházból. Ez azt jelenti, hogy figyelő szövegírói is kell figyelni a szervezet és a jelentést a élettartamának. Törlése kell, amikor a jelentés kell kiüríteni a áruházból (például ha már nem érvényes entitás jelentett hiba).

Nézzük meg, hogy a címzett pontok lehet leírt mindazokat példát. Fontolja meg a szolgáltatás háló alkalmazások tevődik össze a fő állapot-nyilvántartó állandó szolgáltatás és a másodlagos állapot nélküli szolgáltatások csomópontjait (minden feladat típusú egy másodlagos szolgáltatás típusa) rendszerre telepíthető. A fő van formátumú másodlagos zónák hajtja végre parancsokat tartalmazó feldolgozás várólista. A formátumú másodlagos zónák hajtsa végre a bejövő felkérést, és küldje el újra nyugtázó jeleket. Több feltétel, amely ellenőrizhető hossza a fő feldolgozás várakozási sorban található. A fő várólista hossza eléri a, ha egy figyelmeztető üzenetet jelenteni. A figyelmeztető üzenet jelzi, hogy a formátumú másodlagos zónák nem tudják kezelni a betöltés. Ha a sor eléri a maximális hossza és parancsok megszakadnak, hibaüzenet jelenik, mint a szolgáltatás nem tudja visszaállítani. A jelentések lehet a tulajdonság **QueueStatus**. A figyelő él belül a szolgáltatást, és kattintson a fő elsődleges replikát rendszeres elküldött. A time to live két perc, és rendszeres 30 másodpercenként elküldött. Ha az elsődleges megszakad, a jelentés automatikusan törlődnek áruházból. A szolgáltatás replika esetén de azt deadlocked van, vagy a jelentés lejár más problémákat, az állapot áruházból. Ebben az esetben a szervezet kiértékeli a hiba.

Egy másik feltétel figyelhető feladat végrehajtását időre szükség. A fő elosztja a tevékenységek a formátumú másodlagos zónák alapján a tevékenység típusát. Attól függően, hogy a tervezés a diaminta a formátumú másodlagos zónák a tevékenységek állapotjelentéséhez sikerült lekérdezik. Azt is is megvárja, amíg vissza nyugtázó jelek küldésére, ha az azok végzett formátumú másodlagos zónák. A második lehetőséget választja kell ügyelni feltárása azokról a helyzetekről, amikor formátumú másodlagos zónák die és az üzenetek elvesznek. Egy lehetőség van a mesteralakzat szeretne küldeni a ping kérést azonos másodlagos, amely visszaküldi állapota. Ha nincs állapot nem érkezett, a fő hiba tartja, és alapján újraütemezi a tevékenységhez. Ez a jelenség feltételezi, hogy a tevékenységek idempotent.

A felügyelt feltétel fordítható figyelmeztetésként, ha a tevékenység nem történik meg egy megadott idő (**t1**, például a 10 perc). Ha a tevékenység nem befejeződik az idő (**t2**, például 20 perc), a felügyelt feltétel hibaként fordítható. Ez a jelentés többféleképpen lehet végrehajtani:

- A fő elsődleges replikát rendszeres magát a jelentést. A várakozási sorban található egy tulajdonságot az összes folyamatban lévő tevékenységhez is. Ha legalább egy tevékenység tovább tart, a jelentés a tulajdonság **PendingTasks** állapota figyelmeztető vagy hiba, szükség szerint. Ha nincs függő tevékenységek vagy minden feladat végrehajtását lépések, jelentés állapota az OK gombra. A feladatok olyan állandó. Ha az elsődleges megszakad, az újonnan előléptetett elsődleges is megfelelően jelentést.

- Egy másik figyelő folyamat (a felhőben vagy külső) ellenőrzi a feladatokat (a kívülre, az eredménytől függően kívánt tevékenység) megjelenítéséhez, hogy befejeződött. Nem betartják küszöbértékek, ha a jelentés a fő szolgáltatás küldi. Jelentés is küldi el, amely tartalmazza a tevékenység azonosítóját, például a **PendingTask + taskId**tevékenységeket. Jelentések csak a sérült államok is szerepel. Adja meg a time live néhány perc, és a jelentések karbantartása ahhoz járnak eltávolítandó megjelölése.

- A másodlagos feladatként éppen futó jelentések esetén futtassa a vártnál hosszabb ideig tart. A szolgáltatás példánya a tulajdonság **PendingTasks**jelentések. A jelentés pinpoints a szolgáltatás példánya problémákat tartalmazó, de azt nem rögzítése a helyzet, ahol a példány elhalálozik. A jelentések majd is törlődnek. A másodlagos szolgáltatás azt is jelentést. Ha a másodlagos a tevékenység befejeződött, a másodlagos példány törli a jelet a jelentést a áruházból. A jelentés nem rögzítése a helyzet, ahol a visszaigazoló üzenet elvész, és a tevékenység nem fejeződött be a fő szempontjából.

Azonban a jelentési a fenti esetek befejeződött, alkalmazás állapot rögzítése a jelentések vannak, ha állapot kiértékelt.

## <a name="report-periodically-vs-on-transition"></a>Jelentés rendszeres összehasonlítása áttűnés
A rendszerállapot jelentéskészítési modell használatával watchdogs elküldheti jelentések rendszeresen vagy áttűnések. Az ajánlott módszereket figyelő jelentéskészítéshez oka rendszeres időközönként, a kód sokkal egyszerűbbé és kevesebb jobban hibákat. A watchdogs kell törekedjen mindössze lehetséges helytelen jelentések kiváltó hibák elkerülése érdekében. Helytelen *sérült* jelentések hatással állapot értékelést és -esetek állapot, beleértve a frissítések alapján. Helytelen *megfelelő* jelentéseket a fürt nemkívánatosnak tartott adatközpontokban hiba elrejtéséhez.

Ismétlődő tartalmazó jelentések készítéséhez, a figyelő az időzítő végrehajthatók. Timer visszahívás, a a figyelő az állapot ellenőrzése és aktuális állapotát alapján jelentést küldeni. Nem látható, hogy melyik jelentés korábban elküldésének vagy bármely optimalizálásokat értelmez üzenetküldés nincs szükség. Az állapot ügyfél rendelkezik kötegelés logika, ami segítheti a teljesítmény. Amíg az állapot ügyfél életben legyen, akkor próbálkozások belső mindaddig, amíg a jelentés az állapot tároló elfogadja vagy a figyelő ugyanazt a személy, tulajdonság és adatforrás újabb jelentést hoz létre.

Áttűnések a jelentési szükséges állam ügyeljen kezelését. A figyelő bizonyos feltételeknek figyeli, és a jelentések csak a feltételek változásakor. Az eljárás a fejjel, hogy kevesebb jelentések van szükség. Mi a hátrányuk érték, hogy a figyelő logika összetett. A figyelő kell karbantartása feltételek vagy a jelentések, úgy, hogy is vizsgálni állapotának megváltozásakor határozza meg. A feladatátvétel ügyelni kell elküldésekor jelentés, amely esetleg nem lettek elküldve korábban (aszinkron, de még nem küldte el az állapot Store). A sorszám kell lennie, egyre növekvő. Ha nem, mint elavult elutasították a jelentéseket. Az adatok elvesztését felmerülő hol ritkán szinkronizálási szükség lehet a bejelentő állapotát és az állapot tároló állapota között.

Áttűnések a jelentési értelemszerű keresztül, a reporting Services `Partition` vagy `CodePackageActivationContext`. Ha a helyi objektum (kópiának vagy a telepített service csomag / alkalmazás rendszerbe) van távolítja el, annak a jelentéseket is törlődnek. A automatikus karbantartása visszaállítja reporter és az állapot-tár szinkronizálása van szükség. Ha a jelentés szülő partíciót vagy a szülő-alkalmazás, kell ügyelni kell a feladatátvevő elkerülése érdekében a rendszerállapot tárolóban lévő elavult jelentések. Logika hozzá kell adnia a helyes állapotban, és törölje a jelet a jelentés áruházból, amikor már nincs szükség.

## <a name="implement-health-reporting"></a>Állapot jelentéskészítés megvalósítása
Ha a szervezet és a jelentés részletei törlése, állapotjelentések küldése történik az API-t, a PowerShell vagy a többi.

### <a name="api"></a>API
Jelentés az API-k, szüksége állapot-jelentés létrehozása adott személy írja be ide szeretné a jelentést. A jelentés adni egy állapot ügyfél. Azt is megteheti, hozzon létre egy rendszerállapot adatait, és adja át a jelentéskészítési módszerek megoldására `Partition` vagy `CodePackageActivationContext` jelentheti a jelenlegi szervezetek.

A következő példa bemutatja periodikus egy figyelő belül a fürt a jelentési. A figyelő ellenőrzi, hogy egy külső erőforrás webböngészőn keresztül elérhetők belül csomópontot. Az erőforrás van szüksége a szolgáltatás jegyzék az alkalmazáson belül. Az erőforrás nem érhető el, ha is a más szolgáltatásokhoz, az alkalmazáson belül továbbra is működik megfelelően. Emiatt a jelentés küldi el a telepített szolgáltatást csomag entitás 30 másodpercenként.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>A PowerShell
A * *Küldés-ServiceFabric*EntityType*HealthReport ** állapotjelentések küldése.

A következő példa bemutatja periodikus csomóponton Processzor értékeket a jelentés. A jelentések kell-e küldeni 30 másodpercenként, és live két perc időpontjának rendelkeznek. Járjon le, ha a bejelentő van problémák leírását, így a csomópont kiértékeli a hiba. Ha a Processzor küszöbértéket, egy figyelmeztetés állapotának rendelkezik. Ha a Processzor marad küszöbértéket hibaként jelentett több, mint a beállított idő. Egyéb esetben bejelentő küld egy állapotának az OK gombra.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Az alábbi példa a replikán tranziens figyelmeztetés jelentést. Először kap a partíciót és kattintson a replika azonosító a szolgáltatás iránt érdeklődik azt. Ezután elküldi jelentés a **PowershellWatcher** a tulajdonság **ResourceDependency**. A jelentés érdeklődésre számot tartó csak két percig, és törlődik a áruházból automatikusan.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>TÖBBI
Küldje el a bejegyzés összehívások nyissa meg a kívánt személy és van-e a szervezet az állapot jelentés leírása használja a többi állapotjelentések. Ha például megtudhatja, hogy miként többi [fürt állapotjelentések](https://msdn.microsoft.com/library/azure/dn707640.aspx) vagy [szolgáltatás állapotjelentések](https://msdn.microsoft.com/library/azure/dn707640.aspx)küldése. Az összes szervezetek támogatottak.

## <a name="next-steps"></a>Következő lépések

Az állapot adatai alapján, szolgáltatás szövegírói és fürt/rendszergazdái érdemes elképzelnie módszerek az információk felhasználása. Például ezeket beállíthatja állapot állapot alapján értesítések nagyon gyenge hálózati minőség problémák tájékozódást segíti, mielőtt azok kiváltó kimaradások, hogy. A rendszergazdák is beállíthatja javítás rendszerek a hibák elhárításához automatikusan.

[Bevezetés háló szolgáltatás állapota figyelése](service-fabric-health-introduction.md)

[Szolgáltatás háló állapotjelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

[Hogyan lehet jelenteni, és ellenőrizze a szolgáltatás állapota](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Hibaelhárításhoz rendszer állapotjelentések](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Figyelésére és szolgáltatások helyileg diagnosztizálása](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md)
