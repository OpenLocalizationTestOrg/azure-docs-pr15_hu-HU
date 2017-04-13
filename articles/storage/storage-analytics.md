<properties
    pageTitle="Tárterület Analytics segítségével naplókról és a mértékek adatgyűjtés |} Microsoft Azure"
    description="Tárterület Analytics lehetővé teszi a szolgáltatások tárolási mértékek adatok nyomon követése és naplógyűjtés Blob várólista és táblázat tárhelyként használható."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="storage-analytics"></a>Tárterület-elemzés

## <a name="overview"></a>– Áttekintés

Azure tároló Analytics naplózás hajt végre, és mérőszámok adatok biztosít a tárterület-fiók. Az adatok segítségével nyomon követése kérések használatát trendek elemzése és problémáinak diagnosztizálása a tárterület-fiókjával.

Tároló Analytics használatához engedélyezze külön-külön az egyes szolgáltatásokhoz figyelni kívánt. Engedélyezheti újra az [Azure-portálon](https://portal.azure.com). A részletekért olvassa [Monitor egy tárterület-fiókkal az Azure-portálon](storage-monitor-storage-account.md). Tárterület Analytics programozás útján a REST API-t vagy a ügyfél tár keresztül engedélyezheti is. Tárterület Analytics ahhoz, hogy az egyes szolgáltatásokhoz a [Blob-tulajdonságok beolvasása](https://msdn.microsoft.com/library/hh452239.aspx), a [Várólista-tulajdonságok beolvasása](https://msdn.microsoft.com/library/hh452243.aspx), a [Tábla-tulajdonságok beolvasása](https://msdn.microsoft.com/library/hh452238.aspx)és a [Fájl-tulajdonságok beolvasása](https://msdn.microsoft.com/library/mt427369.aspx) műveletek használata

A jól ismert blob (a naplózás) és a jól ismert táblázatok (a mértékek), amely érhetők el a Blob és táblázat szolgáltatás API-k használata az összesített adatok tárolja.

Tárterület Analytics van, amely független a tárterület-fiókja teljes korlátját tárolt adatok mennyiségét legfeljebb 20 TB mennyiségben. További információt a számlázást és az adatok adatmegőrzési házirendek lásd: a [tárterület Analytics és a számlázásra](https://msdn.microsoft.com/library/hh360997.aspx). Tárterületkorlátok fiókkal kapcsolatos további tudnivalókért olvassa el az [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md)című témakört.

Tárterület Analytics- és egyéb eszközök segítségével azonosítása, diagnosztizálása és Azure tároló kapcsolatos hibák elhárítása a részletes útmutató olvassa el a [Monitor, diagnosztizálása, és a Microsoft Azure tároló – problémamegoldás](storage-monitoring-diagnosing-troubleshooting.md)című témakört.


## <a name="about-storage-analytics-logging"></a>Tudnivalók a tárhely Analytics naplózás

Tárterület Analytics sikeres és sikertelen kérések részletes információt jegyez egy tárhelyszolgáltatáshoz. Ezt az információt a Lync-egyedi kérelmeket, és egy tárhelyszolgáltatáshoz problémáinak diagnosztizálása használható. Kéréseket legjobb mindent végeztetünk naplózza.

Naplóbejegyzések tároló szolgáltatási tevékenység esetén csak jönnek létre. Például ha egy tároló fiókban tevékenység Blob szolgáltatásában, de nem a táblázat vagy várólista szolgáltatásait, csak a Blob-szolgáltatás kapcsolatos naplók létrejön.

Tárterület Analytics naplózás nem áll rendelkezésre az Azure-fájl szolgáltatás.

### <a name="logging-authenticated-requests"></a>A naplózás hitelesített kérések

A következő típusú hitelesített kérések naplózza:

- A sikeres kérelmeket.

- Nem sikerült kérések, többek között a időtúllépés, szabályozásának, hálózati, engedélyezését és más hibákat.

- A megosztott Access aláírás (Társítások), többek között a sikertelen és sikeres kérelmeket használó kérések.

- Analytics adatainak kérelmeket.

Tárterület Analytics magát, például a napló létrehozása vagy törlése, által kérelmek nem jelentkezett be. A naplózott adatok teljes listáját a [tárolási Analytics bejelentkezett műveletek és állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx) és [Analytics napló formátumot](https://msdn.microsoft.com/library/hh343259.aspx) témakörök ismertetését.

### <a name="logging-anonymous-requests"></a>Naplózás névtelen kérések
A következő típusú névtelen kérések naplózza:

- A sikeres kérelmeket.

- Kiszolgáló hibákat.

- Az ügyfél- és kiszolgálóoldali időtúllépést.

- Nem sikerült GET kérés 304 hibakóddal (nincs módosítás).

Minden más sikertelen névtelen kérelmek nem jelentkezett be. A naplózott adatok teljes listáját a [tárolási Analytics bejelentkezett műveletek és állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx) és [Analytics napló formátumot](https://msdn.microsoft.com/library/hh343259.aspx) témakörök ismertetését.

### <a name="how-logs-are-stored"></a>Hogyan naplók találhatók.
Az összes naplók blokk BLOB-tárolóban $logs, amely automatikusan hoz létre, ha engedélyezve van a tárhely Analytics tárterület-fiók nevű tárolja. A $logs tárolóban található blob névtere a tárterület-fiókot, például: `http://<accountname>.blob.core.windows.net/$logs`. Ez a tároló nem lehet törölni tároló Analytics engedélyezése után tartalmát törölheti, ha.

>[Azure.NOTE] A $logs tároló nem jelenik meg, ha művelet bejegyzésére tároló végre, például a [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) módszerrel. Meg kell közvetlenül elérhető legyen. Például használhatja a [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) módszer a BLOB a eléréséhez a `$logs` tároló.
Kérések be van jelentkezve, mint a tárhely Analytics feltöltődnek köztes eredmények blocks megegyezik. Rendszeres időközönként tároló Analytics a négyzetek véglegesítése és elérhetővé blob-ként.

Ismétlődő rekordok azonos órára létrehozott naplók lehetnek. Meghatározhatja, hogy egy rekord esetén ismétlődő, jelölje be a **kérelemazonosító** és a **művelet** számát.

### <a name="log-naming-conventions"></a>Jelentkezzen be a elnevezési szabályai
Minden egyes napló írása a következő formátumban történik.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Az alábbi táblázat ismerteti a neve az egyes attribútumok.

| Attribútum         | Leírás                                                                                                                                                                                   |
|----------------   |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   |
| < szolgáltatás neve >    | A tároló szolgáltatás neve. Példa: blob, táblázat vagy várólista.                                                                                                                          |
| YYYY              | A négy számjegyű év, a napló. Példa: 2011.                                                                                                                                           |
| MM                | A két számjegyű hónap a napló. Példa: 07.                                                                                                                                             |
| DD                | A két számjegyű hónap a napló. Példa: 07.                                                                                                                                             |
| hh                | A két számjegyű az óra, amely jelzi a kezdő óra a naplókat UTC 24 órás formátumban. Példa: 18.                                                                                     |
| mm                | A két számjeggyel szám, amely jelzi a kezdő percet a naplók. Az aktuális verziójának tároló Analytics nem támogatja ezt az értéket, és az érték mindig 00.     |
| <counter>         | A nulla alapú számláló hat számjegynél, amely jelzi a tárhelyszolgáltatáshoz az az időszak óránként által generált napló BLOB számát. A számláló 000000 kezdődik. Példa: 000001.     |

Az alábbiakban megtalálja az előző példákban kombináló teljes minta napló nevét.

    blob/2011/07/31/1800/000001.log

Az alábbiakban megtalálja az előző napló eléréséhez használható URI minta.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Tárterület kérelmének be van jelentkezve, amikor a létrejövő neve hibához az órát, amikor a kívánt művelet befejeződött. Ha például egy GetBlob kérelmet fejeződött be a 6:30 óra a 7/31/2011 a napló volna kell írni a következő előtaggal:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Jelentkezzen be a metaadat-alapú
A metaadatok, mely tartalmazza a blob naplóadatok azonosítása használható valamennyi napló BLOB tárolja. Az alábbi táblázat ismerteti az egyes metaadatok attribútum.

| Attribútum     | Leírás                                                                                                                                                                                                                                                   |
|------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| LogType       | Ismerteti, hogy a napló tartalmaz-e olvasható, írja be vagy törlése a műveletek kapcsolatos információk. Ez az érték is elhelyezhet, egy típusa vagy a vesszővel elválasztott mindhárom kombinációi. Például 1: írja be; Példa 2: Olvasás, írás; Például: 3: olvassa el, írja be, és törlése.    |
| Kezdés időpontja     | A legkorábbi lehetséges időpontja a naplóban éééé formájában tétel-MM-DDThh:mm:ssZ. Példa: 2011-07-31T18:21:46Z.                                                                                                                                             |
| Befejezés időpontja       | A legújabb idő a naplóban éééé formájában tétel-MM-DDThh:mm:ssZ. Példa: 2011-07-31T18:22:09Z.                                                                                                                                               |
| LogVersion    | A napló formátum verziója. Jelenleg az egyetlen támogatott érték 1.0.                                                                                                                                                                                     |

Az alábbi lista az előző példákban teljes minta metaadatok jeleníti meg.

- LogType írási =

- Kezdés időpontja = 2011-07-31T18:21:46Z

- Befejezés időpontja = 2011-07-31T18:22:09Z

- LogVersion = 1,0

### <a name="accessing-logging-data"></a>Naplózás adatok elérése

Az összes adat a `$logs` tároló a Blob szolgáltatással API-khoz is elérhető, beleértve az Azure által biztosított .NET API-khoz tár felügyelt. A tároló fiók rendszergazda olvasási és a naplók törlése, de nem létrehozhat vagy frissíteni őket. A napló metaadatok és a bejelentkezési név is használható, ha a naplózási lekérdezése. Lehetséges, hogy egy adott óra naplók jelenjen meg a megfelelő sorrendben, de a metaadatok mindig adja meg az időszak naplóbejegyzések a naplót. Emiatt használható naplófájl neve és a metaadat-alapú adott naplózási keresésekor.

## <a name="about-storage-analytics-metrics"></a>Tudnivalók a Analytics tárolási mértékek

Tárterület Analytics összesített tranzakció statisztika és kapacitásbeli feldolgozó kapcsolatos kéréseit az egy tárhelyszolgáltatáshoz mértékek tárolhat. Tranzakcióinak kell jelenteni, és az API művelet szintre egyaránt a tárhely szolgáltatás szintjén, és a kapacitás jelenteni a tárhely szolgáltatás szintjén. Mértékek adatokat használva elemzése tároló szolgáltatás használatát, a tárolás ellen kérések problémáinak diagnosztizálása és szolgáltatást használó alkalmazások a teljesítmény javítása érdekében.

Tároló Analytics használatához engedélyezze külön-külön az egyes szolgáltatásokhoz figyelni kívánt. Engedélyezheti újra az [Azure-portálon](https://portal.azure.com). A részletekért olvassa [Monitor egy tárterület-fiókkal az Azure-portálon](storage-monitor-storage-account.md). Tárterület Analytics programozás útján a REST API-t vagy a ügyfél tár keresztül engedélyezheti is. Használja a **Szolgáltatás Tulajdonságok beolvasása** műveletek tároló Analytics ahhoz, hogy az egyes szolgáltatásokhoz.

### <a name="transaction-metrics"></a>Transaction mérőszámok

Robusztus adatok az egyes tárhelyszolgáltatáshoz óránkénti vagy perc időközönként rögzítését és API-műveletek, például a bejövő adatok/kilépési, az elérhetőség, a hibák, a kért és vonatkozó kérés százalékértékek kategorizált. Megtekintheti a részleteiről a [Tárhely Analytics mértékek táblázat séma](https://msdn.microsoft.com/library/hh343264.aspx) témakör listáját.

Transaction adatok két szinten – a szolgáltatás és az API művelet szintjének rögzítését. A szolgáltatás szintjén engedélyeiről összes statisztika kért API-műveletek vannak írt egy táblázat személyhez óránként akkor is, ha nincs kérések végzett a szolgáltatás. A API művelet szintjén statisztika csak kerülnek entitás, ha a művelet a kért adott órán belül.

Például a Blob-szolgáltatása **GetBlob** műveletet hajt végre, ha tárolási Analytics mértékek fog jelentkezzen be a kérelem és bele az összesített adatok mind a Blob-szolgáltatás, valamint a **GetBlob** műveletet. Jó helyen jár, ha nincs **GetBlob** művelet során az órát van szükség, entitás nem írandó a `$MetricsTransactionsBlob` az adott művelet.

A felhasználói kérések és a tárhely Analytics maga által kérelmek tranzakció mértékek rögzítése. Például rögzítése tároló Analytics kéréseit naplókról és a táblázat szervezetek írni. Hogyan e kérelmek számlázva vannak kapcsolatos további tudnivalókért olvassa el a [Analytics tárhely és a számlázásra](https://msdn.microsoft.com/library/hh360997.aspx)című témakört.

### <a name="capacity-metrics"></a>Kapacitás mérőszámok

>[AZURE.NOTE] Kapacitás mértékek jelenleg a Blob-szolgáltatás csak érhetők el. Az a táblázat és várólista szolgáltatás kapacitás mértékek tároló Analytics jövőbeli verzióiban elérhető lesz.

Kapacitás adatok naponta rögzítését egy tárterület-fiókkal Blob-szolgáltatás, és két tábla entitás kerülnek. Egy személy számára a felhasználói adatok statisztikai adatokkal szolgál, és a más statisztikai adatokat tartalmaz a `$logs` tároló Analytics által használt blob-tárolóban. A `$MetricsCapacityBlob` táblázat tartalmazza a következő adatokat:

- **Kapacitás**: a tárterület-fiók Blob-szolgáltatás bájtban által használt tárterület mennyiségét.

- **ContainerCount**: a tárterület-fiók Blob szolgáltatásban blob-tárolók számát.

- **ObjectCount**: a tárterület-fiók Blob szolgáltatásban vállalt és nem véglegesített tiltása vagy lap BLOB számát.

Többet szeretne tudni a kapacitás mértékek olvassa el a [Tárhely Analytics mértékek táblázat séma](https://msdn.microsoft.com/library/hh343264.aspx)című témakört.

### <a name="how-metrics-are-stored"></a>Hogyan vannak tárolva a mértékek

Az összes mértékek az egyes a tároló szolgáltatást adatai három tábla szolgáltatás van fenntartva: tranzakció információt egy táblázat, perc tranzakció információt egy táblázat és egy másik tábla Kapacitás információt. Tárterület-használati adatainak kapacitás információ áll, és tranzakció és perc tranzakció adatai kérések és válaszok adatainak áll. Óra mértékek, perc mértékek és a tárterület-fiók Blob-szolgáltatás kapacitás érhetők el az alábbi táblázatban ismertetett módon nevű táblázatokban.

| Mértékek szint                         | Táblázatnevek                                                                                                                   | Verziók                                                                                                                        |
|------------------------------------   |-----------------------------------------------------------------------------------------------------------------------------  |---------------------------------------------------------------------------------------------------------------------------------------------- |
| Óránkénti mértékek, elsődleges helye      |  $MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue                                                  | 2013-08-15 csak megelőző verziók. A nevek továbbra is használhatók, miközben váltani az alább felsorolt táblák használata ajánlott.  |
| Óránkénti mértékek, elsődleges helye      | $MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue               | Az összes verzió, beleértve a 2013-08-15 karaktersorozatot.                                                                                                               |
| MINUTE mértékek, elsődleges helye      | $MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue         | Az összes verzió, beleértve a 2013-08-15 karaktersorozatot.                                                                                                               |
| Óránkénti mértékek, a másodlagos helyre    | $MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue         | Az összes verzió, beleértve a 2013-08-15 karaktersorozatot. Olvasás-elérés geo felesleges replikációs engedélyezve kell lennie.                                                    |
| MINUTE mértékek, a másodlagos helyre    | $MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue   | Az összes verzió, beleértve a 2013-08-15 karaktersorozatot. Olvasás-elérés geo felesleges replikációs engedélyezve kell lennie.                                                    |
| Kapacitás (csak a Blob szolgáltatás)          | $MetricsCapacityBlob                                                                                                          | Az összes verzió, beleértve a 2013-08-15 karaktersorozatot.                                                                                                               |


Az alábbi táblázat automatikusan létrejönnek, ha engedélyezve van a tárhely Analytics tároló fiókot. Hozzáférhetők keresztül névtere a tárterület-fiókot, például:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Mértékek adatok elérése

A mértékek táblázatokban minden adat a táblázat szolgáltatással API-khoz is elérhető, beleértve az Azure által biztosított .NET API-khoz tár felügyelt. A tárhely fiók rendszergazda olvasási és szervezetek táblázat törlése, de nem létrehozhat vagy frissíteni őket.

## <a name="billing-for-storage-analytics"></a>Tárterület Analytics számlázás

Tárterület fióktulajdonos; szerint engedélyezve van a tárhely Analytics akkor alapértelmezés szerint nincs engedélyezve. Adatok mértékek az szolgáltatásai tároló fiók íródott. Emiatt minden írási művelet tároló Analytics által elvégzett számlázható. Emellett a tárolási mértékek adat által használt mérete is számlázható.

Az alábbi műveletek tároló Analytics által elvégzett számlázható:

- A naplózás BLOB létrehozása kérések. 

- Kérés létrehozása a táblázat szervezetek való dimenzió mentén.

Adatok adatmegőrzési szabály állította be, ha nem az előfizetést terhelő törlés tranzakciók tároló Analytics régi naplózási és mérőszámok adatok törlését. Azonban törlése az ügyfél tranzakcióinak számlázható. Adatmegőrzési házirendek kapcsolatos további tudnivalókért témakörben [tároló Analytics adatainak adatmegőrzési szabály beállítása](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Számlázható kérések ismertetése

Minden kérelme-fiók tárhelyszolgáltatáshoz számlázható vagy a nem számlázható. Tárterület Analytics naplózza minden egyes kérés végzett szolgáltatás, beleértve az állapot üzenet, amely jelzi, hogy a kérelem kezelésének módját. Hasonlóképpen tároló Analytics tárolja a mértékek szolgáltatás és az API a rajtuk végzett adatműveleteket szolgáltatás, beleértve a százalékértékek és bizonyos állapot üzenetek számát. Közös ezek a funkciók segítséget nyújtanak a számlázható kérések elemzése, végezze el a javítása az alkalmazás a és a szolgáltatások kérések problémáinak diagnosztizálása. Számlázási kapcsolatos további tudnivalókért olvassa el a [Ismertetése Azure tároló számlázás - sávszélesség, a tranzakciókat, és a kapacitás](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)című témakört.

Tároló Analytics adatainak megtekintve [tárolási Analytics bejelentkezett műveletek és állapotüzenetek](https://msdn.microsoft.com/library/azure/hh343260.aspx) témakörnek a táblák is használhatja a megállapítható, hogy milyen kérések számlázható. Ezután összehasonlíthatja a naplók és mérőszámok adatok megjelenítéséhez, ha Ön után díj lett felszámítva az egy adott kérés állapot üzeneteket. Vizsgálja meg a tárhelyszolgáltatáshoz vagy egyéni API művelet elérhetőség az előző témakör táblát is használhatja.

## <a name="next-steps"></a>Következő lépések

### <a name="setting-up-storage-analytics"></a>Tárterület Analytics beállítása
- [A tároló fiókot az Azure-portálon figyelése](storage-monitor-storage-account.md)
- [Engedélyezése és konfigurálása a tárhely Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Tárterület Analytics naplózás  
- [Tudnivalók a tárhely Analytics naplózás](https://msdn.microsoft.com/library/hh343262.aspx)
- [Tárterület Analytics napló formázása](https://msdn.microsoft.com/library/hh343259.aspx)
- [Tárterület Analytics jelentkezve, műveletek és állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Tárolási Analytics mértékek
- [Tudnivalók a tárolási mértékek-elemzés](https://msdn.microsoft.com/library/hh343258.aspx)
- [Tárterület Analytics mértékek táblázat séma](https://msdn.microsoft.com/library/hh343264.aspx)
- [Tárterület Analytics jelentkezve, műveletek és állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx)  
