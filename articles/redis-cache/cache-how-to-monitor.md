<properties 
    pageTitle="Azure vgx.dll gyorsítótár figyelése |} Microsoft Azure" 
    description="Megtudhatja, hogy miként figyelheti az állapot és a teljesítmény az Azure vgx.dll gyorsítótár-példányok" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Azure vgx.dll gyorsítótár figyelése

Azure vgx.dll gyorsítótár figyelése a gyorsítótár-példányok több lehetőséget nyújt. Is mértékek megtekintése, rögzíteni a Startboard mértékek diagramok, testre szabhatja a dátum és idő cellatartományt figyelése diagramok, hozzáadása és mérőszámok eltávolítása a diagramok és értesítések beállítása, ha bizonyos feltételek teljesülése esetén. Ezek az eszközök lehetővé teszi az Azure vgx.dll gyorsítótár-példányok állapotának figyelheti, és a gyorsítótár alkalmazások kezeléséhez nyújt segítséget.

Gyorsítótár diagnosztika engedélyezésekor Azure vgx.dll gyorsítótár-példányok mértékek körülbelül 30 másodpercenként gyűjtött, és úgy, hogy a mértékek diagramok jelenik meg, riasztási szabályokat kiértékelt tárolt.

Gyorsítótár mértékek begyűjtési, használja a vgx.dll [információ](http://redis.io/commands/info) parancsot. További információt a különböző információ értékek minden egyes használt gyorsítótár metrikus című témakör [elérhető mértékek és a jelentési időszakok](#available-metrics-and-reporting-intervals).

Gyorsítótár mértékek, [tallózással keresse meg](cache-configure.md#configure-redis-cache-settings) a gyorsítótár-példányt az [Azure portál](https://portal.azure.com)megtekintéséhez. A **Mértékek vgx.dll** lap a mértékek Azure vgx.dll gyorsítótár-példányok érhetők el.

![Mértékek vgx.dll][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] A **Mértékek vgx.dll** lap a következő üzenet jelenik meg, kövesse az ahhoz, hogy gyorsítótár diagnosztika [engedélyezése a gyorsítótár diagnosztika](#enable-cache-diagnostics) szakaszban szereplő lépéseket.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

A **Mértékek vgx.dll** lap **Figyelés** diagramok gyorsítótár mértékek megjelenítő tartalmaz. Minden egyes diagramot testre szabható hozzáadásával vagy eltávolítása a mértékek, és megváltoztatja a jelentéskészítési időközt. Megtekintési és a tevékenységek és az értesítések beállítása a **Gyorsítótár vgx.dll** lap tartalmaz egy **Műveletek** szakaszt, amely megjeleníti a gyorsítótár **események** és a **Figyelmeztetési szabályok**.

## <a name="enable-cache-diagnostics"></a>Gyorsítótár diagnosztika engedélyezése

Azure vgx.dll gyorsítótár lehetővé teszi a tárterület-fiókjában tárolt bármely eszközök el szeretné elérni, és közvetlenül az adatfeldolgozás használhassa diagnosztika adatot. Ahhoz, hogy a gyorsítótár diagnosztika gyűjtendő tárolja, és megjelenik az Azure-portálon egy tárterület-fiókot kell beállítania. Gyorsítótárát az ugyanazon régió és az előfizetés megosztása diagnosztika tároló ugyanazt a fiókot, és a konfigurációs megváltozásakor vonatkozik, amelyek az adott régióban az előfizetés minden gyorsítótárát.

Engedélyezheti, és állítsa be a gyorsítótár diagnosztika, nyissa meg a gyorsítótár-példány a **Gyorsítótár vgx.dll** lap. Ha még nem engedélyezettek diagnosztika diagnosztika vonaldiagramot üzenet jelenik meg.

![Gyorsítótár diagnosztika engedélyezése][redis-cache-enable-diagnostics]

Kattintson az üzenetre, a **metrikus** lap megjelenítéséhez, és kattintson a **Diagnosztikai beállítások** engedélyezése és beállítása a gyorsítótár-szolgáltatás példánya diagnosztikai beállításai gombra.

![Diagnosztikai beállítások][redis-cache-diagnostic-settings]

![Diagnosztikai konfigurálása][redis-cache-configure-diagnostics]

Kattintson a gyorsítótár diagnosztika engedélyezése és diagnosztika beállításának megjelenítése **a** gombra.

Kattintson a nyílra a **Tárterület** -fiók jobbra válasszon diagnosztikai adatok tárolására tároló fiókot. A legjobb teljesítmény elérése érdekében válasszon tároló fiókot ugyanabban a régióban, mint a gyorsítótár.

Diagnosztikai beállításai vannak, miután a **Mentés** konfigurációjának mentése gombra. Figyelje meg, hogy eltarthat néhány pillanatig, a módosítások érvénybelépéséhez.

>[AZURE.IMPORTANT] Gyorsítótárát az ugyanazon régió és az előfizetés megosztása diagnosztika tároló ugyanazokat a beállításokat, és a beállítás módosult (diagnosztika engedélyezett/letiltott vagy a tárterület-fiók módosítása) az adott régióban az előfizetés minden gyorsítótárát vonatkozik.

A tárolt mérési módja miatt megtekintéséhez vizsgálja meg a táblákat a tárterület-fiókjában nevek kezdődő `WADMetrics`. A tárolt mérési módja miatt az Azure portálon kívüli elérésével kapcsolatos további tudnivalókért lásd: az [adatok Access vgx.dll gyorsítótár figyelése](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) minta.

>[AZURE.NOTE] Csak a kijelölt tárterület-fiókjában tárolt mértékek jelennek meg az Azure-portálon. Tárterület-fiókok módosításakor a az adatokat tároló korábban beállított fiók letölthető marad, de a meg nem jelenik meg az Azure-portálon.  

## <a name="available-metrics-and-reporting-intervals"></a>Rendelkezésre álló mértékek és intervallumok jelentése

Számos jelentéskészítési intervallumok, beleértve a **régebbi óra**, **ma**, **múlt héten**és **egyéni**a gyorsítótár mértékek jelentett. Az egyes mértékek diagram **metrikus** lap jeleníti meg az átlag, minimum és maximum értékek minden egyes mérőszám arra a diagramra, és néhány mértékek összesen megjelenítése a jelentéskészítési időköz. 

Egyes metrikus két verziója is tartalmaz. Egy mérőszám méri a teljes gyorsítótár és [fürtözés](cache-how-to-premium-clustering.md), a mérőszám, amely tartalmazza az egy második verzióját használó gyorsítótárát a teljesítmény `(Shard 0-9)` a gyorsítótárban egy egyetlen shard neve mértékek teljesítményének. Ha a gyorsítótár 4 shards, például `Cache Hits` van a teljes gyorsítótár-találatok mennyiségét és `Cache Hits (Shard 3)` van-e a gyorsítótár shard csak a találatok.

>[AZURE.NOTE] Még a gyorsítótár esetén a csatlakoztatott aktív ügyfélalkalmazásokban üresjárati néhány gyorsítótár tevékenységét, például csatlakoztatott ügyfelek, a memóriahasználat és a folyamatban lévő művelet jelenhetnek meg. Ez a tevékenység normál Azure vgx.dll gyorsítótár-példány a művelet során.

| Metrikus            | Leírás                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gyorsítótár-találatok        | A megadott jelentéskészítési időközben sikeres kulcs keresések száma. Ez megfelelteti `keyspace_hits` a vgx.dll [információ](http://redis.io/commands/info) parancsot.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Gyorsítótár mért sikertelen találatok      | A megadott jelentéskészítési időközben sikertelen kulcs keresések száma. Ez megfelelteti `keyspace_misses` vgx.dll információ parancsára. Gyorsítótár mért sikertelen találatok nem feltétlenül jelentenek a gyorsítótár problémája van. Például a gyorsítótár-függetlenül programozási mintázatot használ, az alkalmazások formátumban első elem gyorsítótár. Ha az elem nem létezik (gyorsítótár nincs találat), az elem lehívása és a gyorsítótár hozzáadni a következő alkalommal. Gyorsítótár mért sikertelen találatok a gyorsítótár-függetlenül programozási minta rendes működését. Ha a gyorsítótár mért sikertelen találatok értéke nagyobb, mint a várt módon, vizsgálja meg a kitölt és a gyorsítótár beolvassa az alkalmazás logikájának. Ha elemek vannak eltávolítandó miatt memória nyomása a gyorsítótárból, majd néhány gyorsítótár mért sikertelen találatok lehet, de egy Lync-memória nyomása a jobb mérőszám lenne `Used Memory` vagy `Evicted Keys`. |
| Kapcsolódó ügyfelek | A megadott jelentéskészítési időszak alatt a gyorsítótár ügyfélkapcsolatok száma. Ez megfelelteti `connected_clients` vgx.dll információ parancsára. A [kapcsolat korlát](cache-configure.md#default-redis-server-configuration) elérésekor későbbi kapcsolatfelvételi a gyorsítótár sikertelen lesz. Előfordulhat, hogy akkor is, ha vannak nem aktív ügyfélalkalmazás, ott továbbra is a csatlakoztatott ügyfél belső folyamatok és a kapcsolatokkal néhány példányok.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Kizárt kulcsok      | A gyorsítótárból oka az, hogy a megadott jelentéskészítési időközben eltávolítani kívánt elemek számát a `maxmemory` korlát. Ez megfelelteti `evicted_keys` vgx.dll információ parancsára.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Lejárt kulcsok      | Elemek számát a gyorsítótárból lejárt, a megadott jelentéskészítési időszak alatt. Ezt az értéket rendeli `expired_keys` vgx.dll információ parancsára.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Kap              | A megadott jelentéskészítési időszak alatt a gyorsítótárból get-műveletek száma. Ez az érték, akkor a SZUM, az alábbi értékeket az vgx.dll információ a minden parancs: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, és `cmdstat_getrange`, és a jelentéskészítő időközben gyorsítótár-találatok és mért sikertelen találatok összege megegyezik.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Kiszolgáló terhelését vgx.dll | A százalékos aránya, amelyben a vgx.dll kiszolgáló túlterhelt feldolgozása, és nem a üzenetek üresjárati várakozás ciklus. Ha a számláló eléri azt jelenti, hogy a vgx.dll kiszolgáló elérte a teljesítmény plafon, és nem lehet feldolgozni azokat a Processzor 100 bármelyik gyorsabban dolgozhat. Ha nagy vgx.dll kiszolgáló terhelés jelenik majd látni fogja időtúllépés kivételeket az ügyfélprogramban. Ebben az esetben figyelembe méretezés vagy a szétválasztás az adatok több gyorsítótárát be.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Beállítása              | A megadott jelentéskészítési időszak alatt a gyorsítótár beállítása tevékenységek száma. Ez az érték a vgx.dll információ az értékek összegét az alábbi minden parancs: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, és `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Összes művelet  | A megadott jelentéskészítési időszak alatt a gyorsítótár-kiszolgáló által feldolgozott parancsok száma. Ezt az értéket rendeli `total_commands_processed` vgx.dll információ parancsára. Ne felejtse, hogy amikor Azure vgx.dll gyorsítótárával tisztán pub/sub az ott nincs a mértékek `Cache Hits`, `Cache Misses`, `Gets`, vagy `Sets`, lesz, de `Total Operations` mértékek, amelyek a gyorsítótár használatát pub/sub műveletekhez tükrözik.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Használt memória       | A megadott jelentéskészítési időközben kulcs/érték párokká MB gyorsítótár használt gyorsítótár memória összege. Ezt az értéket rendeli `used_memory` vgx.dll információ parancsára. Ez nem része a metaadat-alapú vagy töredezettség.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Használt memória RSS   | Jelentéskészítési megadott időszakban, például töredezettség és metaadatok MB használt gyorsítótár-memória összege. Ezt az értéket rendeli `used_memory_rss` vgx.dll információ parancsára.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| PROCESSZOR               | A Processzor felhasználása az Azure vgx.dll gyorsítótár-kiszolgáló százalékos a megadott jelentéskészítési időszak alatt. Az operációs rendszer ezt az értéket rendeli `\Processor(_Total)\% Processor Time` teljesítménymutató.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Gyorsítótár olvasása        | Adatok mennyiségét olvassa el a gyorsítótárból megabájtban (MB/s) másodpercenként a megadott jelentéskészítési időszak alatt. Ez az érték a hálózati kártya, amelyek támogatják a virtuális gép, amely a gyorsítótár tárolja, és nem adott vgx.dll származik. **Ez a gyorsítótár által használt hálózati sávszélesség felel meg ezt az értéket. Ha szeretne kiszolgálói egymás hálózati sávszélesség-korlátok vonatkozó értesítések beállítása, majd hozza létre segítségével, ezzel `Cache Read` számláló. Lásd [az alábbi táblázat](cache-faq.md#cache-performance) a megfigyelt sávszélesség korlátozásairól rétegek és méretű árak különböző gyorsítótár.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Gyorsítótár írási       | A gyorsítótár megabájtban (MB/s) másodpercenként során a megadott írt adatok mennyiségét intervallum jelentés. Ez az érték a hálózati kártya, amelyek támogatják a virtuális gép, amely a gyorsítótár tárolja, és nem adott vgx.dll származik. A hálózat sávszélessége az ügyféltől a gyorsítótár küldött adatok felel meg ezt az értéket.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Mértékek megtekintéséről, és testre szabhatja a diagramokat

A gyorsítótárat a **Mértékek vgx.dll** lap megtekintheti a mértékek áttekintése. A **Mértékek vgx.dll** eléréséhez lap válassza a **minden beállítások** > **Mértékek vgx.dll**.

![Mértékek vgx.dll][redis-cache-redis-metrics]


A **Mértékek vgx.dll** lap a következő diagramok tartalmazza.

| A diagram metrikus vgx.dll | Megjelenített mérőszámok     |
|------------------|-------------------|
| Gyorsítótár Olvasás és a gyorsítótár írása | Gyorsítótár olvasása |
|                            | Gyorsítótár írási |
| Kapcsolódó ügyfelek      | Kapcsolódó ügyfelek |
| A találatok és mért sikertelen találatok  | Gyorsítótár-találatok        |
|                  | Gyorsítótár mért sikertelen találatok      |
| Teljes parancsok   | Összes művelet  |
| Lekérdezi és halmazok listájával    | Kap              |
|                  | Beállítása              |
| Processzorhasználata         | PROCESSZOR           |
| Memóriahasználat      | Használt memória   |
|                   | Használt memória RSS |
| Kiszolgáló terhelését vgx.dll | Kiszolgáló betöltése   |
| Kulcs száma | Teljes kulcsok |
|           | Kizárt kulcsok |
|           | Lejárt kulcsok |


Részletesebb megjelenítése a mérési módja miatt egy adott diagramra, és testre szabhatja a diagram kattintson a kívánt diagramot a **Mértékek vgx.dll** lap az, hogy a diagram **metrikus** lap megjelenítéséhez.

![Metrikus lap][redis-cache-metric-blade]

Ezeket az értesítéseket, amelyek a diagram által megjelenített mérési módja miatt az, hogy a diagram **metrikus** lap alján felsorolt.

Hozzáadása vagy eltávolítása a mértékek, vagy a jelentéskészítési időközének módosítása, válassza a **Diagram szerkesztése**.

Hozzáadása vagy eltávolítása a diagram a mértékek, kattintson a mérőszám neve melletti jelölőnégyzetet. A jelentés intervallum módosításához kattintson a kívánt időközt. A **diagram típusának**megváltoztatásához kattintson a kívánt stílust. Ha a kívánt módosításokat, kattintson a **Mentés**gombra. 

![Diagram szerkesztése][redis-cache-edit-chart]

Amikor, kattintson a **Mentés** gombra a módosítások megmaradnak, amíg el nem hagyja a **metrikus** lap. Miután, visszatérhet később, látni fogja az eredeti metrikus és időtartomány újra. Diagramok testreszabásáról további tudnivalókért olvassa el a [Monitor szolgáltatás mértékek](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)című témakört.

A mértékek megtekintése egy adott időszakra a diagramon, mutasson az egérrel egy adott sávokhoz vagy a diagram, amely megfelel a kívánt időpontot a pontok, és a mértékek, hogy intervallum jelennek meg.

![Diagram részleteinek megtekintése][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>A fürtözés prémium gyorsítótárat figyelése

Legfeljebb 10 shards prémium gyorsítótárát, amelyek a [fürtözés](cache-how-to-premium-clustering.md) engedélyezve van. Minden egyes shard van a saját mértékek, és ezeket a mértékek mértékek nyújt a gyorsítótár egészének vannak összesíti. Egyes metrikus két verziója is tartalmaz. Egy mérőszám méri a teljes gyorsítótár és a mérőszám, amely tartalmazza az egy második verzióját teljesítmény `(Shard 0-9)` a gyorsítótárban egy egyetlen shard neve mértékek teljesítményének. Ha a gyorsítótár 3 shards, például `Cache Hits` van a teljes gyorsítótár-találatok mennyiségét és `Cache Hits (Shard 2)` van-e a gyorsítótár shard csak a találatok.

Minden egyes felügyeleti diagram a legfelső szintű mérési módja miatt a gyorsítótár a mértékek, az egyes gyorsítótár shard együtt jeleníti meg.

![Monitor][redis-cache-premium-monitor]

Az egér mutatva adatpontok jeleníti meg az adott pontra a részletek idő. 

![Monitor][redis-cache-premium-point-summary]

A nagyobb értékekre jellemzően az összesített értékeket a gyorsítótár közben a kisebb értékeket a egyes mérési módja miatt a shard. Ne feledje, hogy ebben a példában a három shards vannak, és a gyorsítótár-találatok között a shards egyenletesen elosztott.

![Monitor][redis-cache-premium-point-shard]

Kattintson a diagramra kattintva megtekintheti a **metrikus** lap a kibontott nézetét részletek megtekintéséhez.

![Monitor][redis-cache-premium-chart-detail]

Alapértelmezés szerint a minden diagram a legfelső szintű gyorsítótár teljesítmény számláló, valamint a teljesítmény számláló a az egyes shards tartalmazza. Testre szabhatja a **Diagram szerkesztése** lap, az.

![Monitor][redis-cache-premium-edit]

A rendelkezésre álló teljesítmény számláló kapcsolatos további tudnivalókért lásd: a [elérhető mértékek és a jelentési időszakok](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Műveletek és értesítések

A **Műveletek** csoportban a **Gyorsítótár vgx.dll** lap szakaszból **események** és **figyelmeztetési szabályokat** .

![Oeprations][redis-cache-operations-events]

A legutóbbi gyorsítótár műveletek listájának megtekintéséhez kattintson az **események** diagram az **események** lap megjelenítéséhez. Műveletek többek között beolvasása és bezárhatja a hívóbetűk, és az aktiválás és riasztási szabályok felbontásának. Az egyes eseményekkel kapcsolatos további tudnivalókért kattintson a az eseményre az **események** lap.

Események kapcsolatos további tudnivalókért olvassa el az [események megtekintése és a naplókat](../monitoring-and-diagnostics/insights-debugging-with-events.md)című témakört.

A **Figyelmeztetési szabályok** szakaszban a gyorsítótár-példány riasztásainak számát jeleníti meg. Egy szabályt lehetővé teszi, hogy figyelheti a gyorsítótár-példányt, és a kapott e-mailt, amikor csak egy bizonyos metrikus érték eléri a a szabályban megadott küszöbértékét. 

Riasztási szabályok kiértékelése öt percenként, és egy szabályt aktiválásakor bármely konfigurált értesítések fogadására. Aktiválás szabályt, és az értesítések feldolgozása nem azonnal; Előfordulhat, hogy néhány percet, mielőtt aktiválva van egy szabályt, és küldött értesítések késleltetést.

Riasztási szabályok tekinthetők meg és állítsa az adott felügyeleti diagram **metrikus** lap vagy a **riasztási szabályok** lap.

A figyelmeztetési szabályok a következő tulajdonságokat van.

| Figyelmeztetést tulajdonság                 | Leírás                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Erőforrás                            | Az erőforrás által a szabályt értékelik. Ha létrehoz egy szabályt vgx.dll gyorsítótárból, az a gyorsítótár az erőforrás.                                                                                                                                                                                                                                                                                                                                                  |
| név                                | A szabályt belül az aktuális gyorsítótár-példány egyedileg azonosító neve.                                                                                                                                                                                                                                                                                                                                                                                       |
| Leírás                         | A figyelmeztetési szabály leírását.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Metrikus                              | A riasztási szabály ellenőrizendő metrikus. Gyorsítótár mértékek listáját olvassa el a elérhető mértékek és a jelentési időszakok című témakört.                                                                                                                                                                                                                                                                                                                                             |
| Feltétel                           | A feltétel kezelőjét a szabályt. Választási lehetőségek vannak: nagyobb, mint, nagyobb vagy egyenlő, kisebb, kisebb vagy egyenlő                                                                                                                                                                                                                                                                                                                             |
| Küszöbérték                           | Vesse össze a mérőszám a feltétel tulajdonságban meghatározott operátorral értéke. Attól függően, hogy a mérőszám, ez az érték előfordulhat, hogy bájt/második, bájt, % vagy száma.                                                                                                                                                                                                                                                                                     |
| Időszak                              | Megadja, hogy az eredményhalmaz, amelyen a mérőszám az átlagos értéke a szabályt összehasonlító használható. Például ha az időszak alatt az utolsó órát, az előző óra intervallum fölé a mérőszám az átlagos értéke szolgál az összehasonlítás. Ha azt szeretné, ha értesítést szeretne kapni a küszöbértékét tevékenységet a Nyárs miatt teljesülése esetén, rövidebb időszakot szükség. Ha értesítést szeretne kapni a küszöbérték felett tartós tevékenység esetén, használja a hosszabb időszakra. |
| E-mail szolgáltatás, és további rendszergazdák | IGAZ, ha a szolgáltatás rendszergazdája és a közös rendszergazda vannak küldve aktiválásakor az értesítésre.                                                                                                                                                                                                                                                                                                                                                                    |
| További rendszergazdai e-mailben      | Ha értesítést szeretne kapni a figyelmeztető aktiválásakor további rendszergazda választható e-mail címét.                                                                                                                                                                                                                                                                                                                                                                    |

Csak egy értesítést küld egy szabályt az aktiválás. Miután szabály küszöbértékét túllépik, és egy értesítést küld, a szabály nem újból kiértékeli mindaddig, amíg a mérőszám küszöbérték alá esik. Ha a mérőszám később meghaladja a küszöbérték, a figyelmeztető aktiválódik, és egy új értesítést küld.

A riasztási szabályok a gyorsítótár-példányhoz tartozó összes megtekintéséhez kattintson a **Gyorsítótár vgx.dll** lap **riasztási szabályok** részt. Csak az értesítés szabályokat, amelyek egy adott mérőszám megtekintéséhez nyissa meg a diagram, amely tartalmazza az adott metrikus a **metrikus** lap.

![A figyelmeztetési szabályok][redis-cache-alert-rules]

Egy szabályt hozzáadásához kattintson a **metrikus** lap vagy a **riasztási szabályok** lap **értesítés hozzáadása** elemre. 

Írja be a kívánt feltételeknek a **Hozzáadás értesítés** szabály lap, és kattintson az **OK gombra**. 

![Riasztási szabály hozzáadása][redis-cache-add-alert]

>[AZURE.NOTE] Egy szabályt létrehozásakor a **metrikus** lap a **Hozzáadás értesítés** kattintva csak a mérési módja miatt jelenik meg, hogy a lap diagram **metrikus** legördülő jelennek meg. Egy szabályt létrehozásakor a **riasztási szabályok** lap a **Hozzáadás értesítés** kattintva **metrikus** legördülő minden gyorsítótár mérőszám érhetők el.

Egy szabályt mentése után jelenik meg a **riasztási szabályok** lap, valamint a bármely diagramok, a használt mérőszám a figyelmeztetést megjelenítő **metrikus** lap. Ha szerkeszteni szeretné egy szabályt, kattintson a nevére a szabályt a **Szabály szerkesztése** lap megjelenítéséhez. A **Szabály szerkesztése** lap a a szabály tulajdonságainak szerkesztése, törlése vagy letiltása a szabályt, vagy a szabály ismételt engedélyezése, ha korábban le lett tiltva.

>[AZURE.NOTE] A szabály tulajdonságainak végzett módosítások eltarthat egy kis időt, mielőtt azok megjelenjenek a **riasztási szabályok** lap vagy a **metrikus** lap.

Egy szabályt aktiválásakor elküldése e-mailt attól függően, hogy a konfigurációjának a szabályt, és egy figyelmeztető ikon a **Gyorsítótár vgx.dll** lap **riasztási szabályok** kijelzőjéhez.

Egy szabályt el kell hárítani, ha a riasztási feltétel már nem ad vissza IGAZ számít. Miután a szabályt feltétel megoldódott, a figyelmeztető ikon egy pipa cseréli le. További információ a riasztási aktiválásra és megoldások kattintson a **Gyorsítótár vgx.dll** lap az események meg az **esemény** lap a **események** részére.

További információt a figyelmeztetés az Azure [fogadási riasztási értesítés](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)jelenik meg.

## <a name="metrics-on-the-redis-cache-blade"></a>A gyorsítótár vgx.dll lap a mértékek

A **Gyorsítótár vgx.dll** lap a következő kategóriák mértékek jeleníti meg.

-   [Diagramok figyelése](#monitoring-charts)
-   [Diagramok használatát](#usage-charts)

### <a name="monitoring-charts"></a>Diagramok figyelése

A **Figyelés** szakaszban a **találatok és tévesztések**, **kap, és akkor állítja be**, **kapcsolatok**és a **Teljes parancsok** diagramok tartalmaz.

![Diagramok figyelése][redis-cache-monitoring-part]

A **Figyelés** diagramok jeleníti meg a következő mérési módja miatt.

| Diagram figyelése | Gyorsítótár mérőszámok     |
|------------------|-------------------|
| A találatok és mért sikertelen találatok  | Gyorsítótár-találatok        |
|                  | Gyorsítótár mért sikertelen találatok      |
| Lekérdezi és halmazok listájával    | Kap              |
|                  | Beállítása              |
| Kapcsolatok      | Kapcsolódó ügyfelek |
| Teljes parancsok   | Összes művelet  |

A mérési módja miatt megtekintése és testreszabásáról az egyes diagramok ebben a szakaszban a további tudnivalókért lásd a következő [Mértékek megtekintése és mérőszámok diagramok testreszabása](#how-to-view-metrics-and-customize-charts) című szakaszt.

### <a name="usage-charts"></a>Diagramok használatát

A **használatát** szakasz **Vgx.dll kiszolgáló betöltése**, a **Memóriahasználat**, a **Hálózati sávszélesség**és a **Processzorhasználata** diagramok van, és is megjelenik a **árak réteg** gyorsítótár példány.

![Diagramok használatát][redis-cache-usage-part]

A **réteg árak** jeleníti meg a gyorsítótár árak első csoportba tartozó, és egy másik árak réteg a gyorsítótár [méretarányra](cache-how-to-scale.md) használt.

A **használati** diagramok jeleníti meg a következő mérési módja miatt.

| Látogatottsági diagram       | Gyorsítótár mérőszámok |
|-------------------|---------------|
| Kiszolgáló terhelését vgx.dll | Kiszolgáló betöltése   |
| Memóriahasználat      | Használt memória   |
| Hálózati sávszélesség | Gyorsítótár írási   |
| Processzorhasználata         | PROCESSZOR           |

A mérési módja miatt megtekintése és testreszabásáról az egyes diagramok ebben a szakaszban a további tudnivalókért lásd a következő [Mértékek megtekintése és mérőszámok diagramok testreszabása](#how-to-view-metrics-and-customize-charts) című szakaszt.
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



