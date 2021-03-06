<properties 
    pageTitle="Az Azure-portálon a tárolási mértékek engedélyezése |} Microsoft Azure" 
    description="Hogyan engedélyezhető a tárolási mértékek a Blob, várólista, táblázat vagy fájl szolgáltatások" 
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

# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Engedélyezése a tárolási mértékek és mérőszámok adatainak megtekintése

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>– Áttekintés

Alapértelmezés szerint a tárolási mértékek a tárterület-szolgáltatás nincs engedélyezve. Figyelés, vagy az [Azure klasszikus portálon](https://manage.windowsazure.com), a Windows PowerShell, engedélyezheti vagy programozás útján API tárolási keresztül.

Ha engedélyezi a tárolási mértékek, választania kell az adatokat egy adatmegőrzési időszak: Ez az időszak határozza meg, hogy mennyi tárterületet szolgáltatás követi a mértékek és a költségek, a helyet, hogy szükséges tárolja őket. Általában kell használni a dolgozók rövidebb adatmegőrzési időszak mint óránkénti mértékek perc mértékek miatt a jelentős plusz térközt a mértékek perc szükséges. Az adatmegőrzési időszak úgy, hogy van elegendő idő elemezheti az adatokat, és töltse le a minden kapcsolat nélküli elemzés vagy jelentési célból megtartani kívánt mértékek kell választania. Ne feledje, hogy Ön fog is számlát kapni a mértékek adatok letöltése a tárterület-fiókjából.

## <a name="how-to-enable-storage-metrics-using-the-azure-classic-portal"></a>Hogyan engedélyezhető a tárolási mértékek az Azure klasszikus portál használatával

Az [Azure klasszikus portálon](https://manage.windowsazure.com)a lap vezérlőre tárolási mértékek tárterület-fiók használata. Figyelemmel kísérésére, beállíthatja, hogy a szint és az adatmegőrzési időszak BLOB, táblázatok és sorok mindegyikéhez napokban. Minden esetben szintje az alábbiak egyikét:

- A kikapcsolva – Nem mértékek begyűjtési.

- Minimális – Tárolási mértékek gyűjti össze a mértékek, például a bejövő adatok/kilépési, elérhetősége, időtartam és sikeres százalékos értékek vannak összesíti a Blob, a táblázat és a várólista szolgáltatásokra egyszerű sorozatát.

- Részletes – Tárolási mértékek gyűjti össze a mértékek, amely tartalmazza a azonos mérési módja miatt egyes tároló API művelet kívül a szolgáltatás szintű mérési módja miatt teljes sorozatát. Részletes mértékek engedélyezése alkalmazás műveletek során előforduló problémák közelebb elemzését.

Figyelje meg, hogy az Azure klasszikus portálon nem jelenleg engedélyezi perc mértékek konfigurálása a tárterület-fiókjában; engedélyezni kell a PowerShell használatá perc mértékek vagy programozás útján.


## <a name="how-to-enable-storage-metrics-using-powershell"></a>Hogyan engedélyezhető a tárolási mértékek PowerShell használatával

A helyi számítógépre PowerShell használatával állítsa be a tárolási mértékek a tárterület-fiókjában aktuális beállításainak módosítása az Azure PowerShell-parancsmag Get-AzureStorageServiceMetricsProperty beolvasni a jelenlegi beállításokat, és a Set-AzureStorageServiceMetricsProperty parancsmag használatával.

A parancsmagok vannak, amelyek tárolási mértékek a következő paraméterek használata:

- MetricsType lehetséges értékei óra és perc.

- ServiceType lehetséges értékei Blob, a várakozási sora és a táblázathoz.

- MetricsLevel lehetséges értékei None (egyenértékű az Azure klasszikus portálon kikapcsolva), a szolgáltatás (egyenértékű a minimális az Azure klasszikus portálon) és a ServiceAndApi (egyenértékű a részletes az Azure klasszikus portálon).

A következő parancs például perc mérési módja miatt a blob-szolgáltatás alapértelmezett tároló fiókban vált, az adatmegőrzési időszak állítsuk be öt nappal az:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

A következő parancs olvassa be az aktuális óránkénti mértékek szint és az adatmegőrzési nap a blob-szolgáltatás alapértelmezett tároló fiókban:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Az Azure PowerShell-parancsmagok használata az Azure-előfizetése, és jelölje ki az alapértelmezett tárterület-fiókot szeretne használni, hogyan témakörben beállításáról olvashat: [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Hogyan engedélyezhető a tárolási mértékek programozás útján

A következő C# kódtöredékének megtudhatja, hogy miként mértékek és Blob szolgáltatás a tárterület-ügyfél tárat a .NET rendszerhez naplózásának engedélyezése:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days. 
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);
    
## <a name="viewing-storage-metrics"></a>Tárolási mértékek megtekintése

Lync-tároló fiókját a tárolási mértékek állította be, amikor a mértékek rögzít egy sor olyan jól ismert táblázatok a tárterület-fiókjában. Az Azure klasszikus portálon a tárterület-fiók a Monitor lap használatával megtekintheti az óránkénti mértékek, mint a diagram elérhetővé válnak. Ezen a lapon az Azure klasszikus portálon a következőkre van lehetősége:

- Jelölje ki a mely mértékek szeretne ábrázolni a diagramon (elérhető mértékek választható függ, hogy választotta a részletes vagy minimális figyelése a szolgáltatás a lap).


- Jelölje ki a mértékek, a diagramon megjelenített időtartomány.


- Válassza az abszolút és relatív skála használatával rajzolja meg a mérési módja miatt.


- Állítsa be az e-mailben értesítéseket, hogy értesítést küldjön, amikor egy adott mérőszám eléri bizonyos értéket.


Ha azt szeretné, töltse le a mértékek, a hosszú távú tárterületre vagy helyben elemzése őket, szüksége lesz eszköz használata, vagy írja be a táblák olvasható kód. Le kell töltenie a percek elemzéshez mérési módja miatt. A táblák nem jelennek meg, a lista összes táblát a tárterület-fiókjában, de közvetlenül a nevük elérhetők legyenek. Számos külső tároló böngészési eszközök tudatában legyenek ezek a táblázatok, és lehetővé teszi azok meg őket közvetlenül (lásd: a blogbejegyzésből [Microsoft Azure tároló kezelők](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) elérhető eszközök listáját).

### <a name="hourly-metrics"></a>Óránkénti mérőszámok
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>MINUTE mérőszámok
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapacitás
- $MetricsCapacityBlob

Az alábbi táblázat a [Tárhely Analytics mértékek táblázat séma](https://msdn.microsoft.com/library/azure/hh343264.aspx)séma minden részlet talál. Az alábbi példa sorok megjelenítése az elérhető oszlopok csak egy részhalmazát, de néhány fontos szolgáltatásának a tárolási mértékek menti a mértékek módját szemlélteti:

| PartitionKey  |       RowKey       |                    Időbélyeg | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Elérhetőség | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      felhasználó; Az összes      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | felhasználó; QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7,8                  | 100            |
| 20140522T1100 |  felhasználó; QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | felhasználó; UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

A mintaadatok perc mértékek a partíciót billentyűt használja az idő perc felbontásban. A sor kulcs azonosítja, amely a sor található információ típusa, és ez tevődik össze két csatolt információkat, a hozzáférés típusát és a kérelem típusát:

- Az access-típus a felhasználó vagy a rendszer, ahol felhasználói összes felhasználói kérések a tárhelyszolgáltatáshoz hivatkozik, és a rendszer által tároló Analytics-kérelmek hivatkozik.

- A kérés típusa esetben az összes, amelyben egy olyan összesítő sor, vagy az adott API, például QueryEntity vagy UpdateEntity azonosítja.


A fenti mintaadatokat az összes rekordot a egyetlen MINUTE (11:00 de kezdve) jeleníti meg, így QueryEntities kérések száma plusz QueryEntity számát kér plusz számát UpdateEntity kéri, akár hét, amely az összes hozzáadása jelenik meg a felhasználó: minden sor. Hasonlóképpen, származhat a felhasználó: minden sorban az átlagos végpontok közötti várakozási 104.4286 által kiszámítása ((143.8 * 5) + 3 + 9) és 7.

A Monitor lapon az Azure klasszikus portálon értesítések beállításával figyelembe, hogy a tárolási mértékek automatikusan értesíti a felhasználót a tárterület-szolgáltatások működésének bármely fontos változásairól. Tárterület explorer eszköz használatával a mértékek adatok tagolt formátumban letöltése, ha a Microsoft Excel segítségével elemezheti az adatokat. Tekintse meg a [Microsoft Azure tároló kezelők](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) közzé a rendelkezésre álló tárhely explorer eszközök listája az.



## <a name="accessing-metrics-data-programmatically"></a>Mértékek adatok programozás útján elérése

A következő listaelem minta C# kód, amely fér hozzá a percek mérési módja miatt tartomány percben, és megjeleníti az eredményt ablak konzolban jeleníti meg. Akkor használja az Azure tároló tár 4-es verzióját, amely tartalmazza a CloudAnalyticsClient osztály egyszerűsíti a tárolási mértékek tábláiban elérése.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    
    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
    select entity;
    
    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }
    
    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
    
    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Melyik a díjakat tegye meg merülnek fel, ha engedélyezi a tárolási mértékek?

Írja be létrehozása a táblázat szervezetek vonatkozó mérőszámok kérések terheli az alkalmazandó összes Azure tárolási műveletek szabványos kamatlába.

Olvasás és törlés kérések mértékek adatok ügyfél által is számlázható szabványos kamatlába mellett. Ha egy adatmegőrzési van beállítva, nem előfizetést terhelő Azure tároló régi mértékek adatok törlését. Jó helyen jár Ha analytics adatainak töröl, a fiók terheli törlési műveletekre vonatkozó.

A mértékek táblák által használt kapacitása is számlázható: becslése mértékek adatok tárolására szolgál kapacitás mértéke az alábbi is használhatja:

- Ha minden óra szolgáltatás használja a minden API-ja minden szolgáltatáshoz, majd 148KB adatot vannak tárolva a mértékek tranzakció táblázatokban óránként Ha engedélyezte a szolgáltatás és az API szintű összefoglaló.

- Ha minden óra szolgáltatás használja a minden API-ja minden szolgáltatáshoz, majd 12KB adatot vannak tárolva a mértékek tranzakció táblázatokban óránként Ha engedélyezte a csak szolgáltatás szintű összefoglaló.

- BLOB kapacitás táblának két sorok hozzáadott minden nap, (Ha a felhasználó naplók csatlakozott): Ez azt jelenti, hogy mindennap az alábbi táblázat méretének növelése körülbelül 300 bájt.

## <a name="next-steps"></a>A következő lépéseket:
[Tárhely Analytics naplóadatok elérése és naplózásának engedélyezése](https://msdn.microsoft.com/library/dn782840.aspx)
 