<properties
   pageTitle="Azure diagnosztika teljesítményét számláló használata |} Microsoft Azure"
   description="Az Azure cloud services vagy a virtuális gép teljesítmény számláló segítségével szűk megkeresése és teljesítményének finomhangolása."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Létrehozása és használata a teljesítmény számláló az Azure alkalmazásokban

Ez a cikk ismerteti a előnyei és a teljesítmény számláló helyezze az Azure alkalmazásba. Adatgyűjtés, szűk megkeresése és a rendszer és az alkalmazás teljesítményének finomhangolása használhatja őket.

Teljesítmény számláló érhető el Windows Serveren, IIS és ASP.NET is gyűjtött, és azt határozza meg, az Azure webes szerepkörök, a dolgozók szerepkörök és a virtuális gépeken futó állapotának. Is létrehozhat, és használja az egyéni teljesítmény számláló.  

Ellenőrizheti, hogy a számláló teljesítményadatokat
1. Közvetlenül az állomáson alkalmazás a távoli asztali webböngészőn teljesítményét Monitor eszközzel
2. A rendszer központ Operations Manager az Azure felügyeleti csomag használata
3. Diagnosztikai adatok eléréséhez más felügyeleti eszközt Azure tároló át. [Áruház és a diagnosztikai adatok megtekintése az Azure-tárolóban lévő](https://msdn.microsoft.com/library/azure/hh411534.aspx) talál további információt.  

További információt a az alkalmazás az [Azure klasszikus portálon](http://manage.azure.com/)a teljesítmény figyelését megtudhatja, [hogy miként Monitor felhőszolgáltatásokhoz](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

További meg szeretné vizsgálni a naplózás stratégia nyomon követése és a diagnosztikai és más technikákkal használatával kapcsolatos problémák elhárítása és Azure alkalmazások optimalizálása létrehozásával kapcsolatban, olvassa el [Hibaelhárítási gyakorlati tanácsok a Azure-alkalmazások fejlesztése](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Engedélyezze a teljesítmény számláló figyelése

Alapértelmezés szerint nincs engedélyezve a teljesítmény számláló vannak. Az alkalmazás vagy indítási tevékenység alapértelmezett diagnosztika ügynök konfigurációja, ha meg szeretné jeleníteni az egyes szerepkör-példány figyelni kívánt jellemző teljesítmény teljesítménymutatói kell módosítani.

### <a name="performance-counters-available-for-microsoft-azure"></a>Microsoft Azure elérhető teljesítményét számláló

Azure egy részét a rendelkezésre álló teljesítmény számláló nyújt a Windows Server, az IIS és az ASP.NET objektumhalomban. Az alábbi táblázat néhány a teljesítmény számláló érdeklődésére Azure-alkalmazásokhoz.

|A számláló kategória: Objektum (példány)|Számláló neve      |Hivatkozás|
|---|---|---|
|.NET CLR kivételek (_globális_)|# Kivételek / mp kiváltott   |Kivétel teljesítmény számláló|
|.NET CLR memória (_globális_)    |A globális Katalógus % idő            |A memória teljesítmény számláló|
|ASP.NET                      |Alkalmazás újraindítása    |ASP.NET teljesítmény számláló|
|ASP.NET                      |Kérés végrehajtása ideje  |ASP.NET teljesítmény számláló|
|ASP.NET                      |Megszakadt kérések   |ASP.NET teljesítményét számláló|
|ASP.NET                      |Dolgozó folyamat újraindítása |ASP.NET teljesítmény számláló|
|ASP.NET-alkalmazások (__teljes__)|Az összes kérelem        |ASP.NET teljesítmény számláló|
|ASP.NET-alkalmazások (__teljes__)|Kéréseket/Sec          |ASP.NET teljesítményét számláló|
|ASP.NET v4.0.30319           |Kérés végrehajtása ideje  |ASP.NET teljesítmény számláló|
|ASP.NET v4.0.30319           |Kérés várakozási idő       |ASP.NET teljesítmény számláló|
|ASP.NET v4.0.30319           |Aktuális        |ASP.NET teljesítmény számláló|
|ASP.NET v4.0.30319           |Kérései         |ASP.NET teljesítmény számláló|
|ASP.NET v4.0.30319           |Elutasított kérések       |ASP.NET teljesítmény számláló|
|A memória                       |Rendelkezésre álló memória        |A memória teljesítmény számláló|
|A memória                       |Lekötött bájt         |A memória teljesítmény számláló|
|Processzor(_Total)            |Processzor kihasználtsága (%)        |ASP.NET teljesítmény számláló|
|TCPv4                        |Csatlakozási hibák     |TCP-objektum|
|TCPv4                        |Létrehozott kapcsolatok |TCP-objektum|
|TCPv4                        |Kapcsolatok alaphelyzetbe állítása       |TCP-objektum|
|TCPv4                        |Elküldött szegmensek/sec       |TCP-objektum|
|Hálózati Interface(*)         |Fogadott/sec      |Hálózati kapcsolat objektum|
|Hálózati Interface(*)         |Elküldött/sec          |Hálózati kapcsolat objektum|
|A hálózati kapcsolat (Microsoft virtuális gép Bus hálózati adapteren _2)|Fogadott/sec|Hálózati kapcsolat objektum|
|A hálózati kapcsolat (Microsoft virtuális gép Bus hálózati adapteren _2)|Elküldött/sec|Hálózati kapcsolat objektum|
|A hálózati kapcsolat (Microsoft virtuális gép Bus hálózati adapteren _2)|Összes bájt/sec|Hálózati kapcsolat objektum|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Létrehozása és hozzáadása egyéni teljesítmény számláló az alkalmazás

Azure létrehozása és módosítása a webes és dolgozó szerepkörhöz egyéni teljesítményét számláló támogatási tartalmaz. A számláló nyomon követésére és az alkalmazás-specifikus viselkedés figyelése használhatók. Hozzon létre, és egyéni teljesítmény számláló kategóriákat és azonosítója törlése indítási tevékenységhez, webes szerepkör vagy magasabb szintű jogosultsággal rendelkező dolgozó szerepkör.

>[AZURE.NOTE] Kód, amely egyéni teljesítmény számláló változtatja meg kell rendelkeznie jogosultsági futtatásához. Ha a kód egy webes szerepkör vagy dolgozó szerepkört, a szerepkör tartalmaznia kell a címke <Runtime executionContext="elevated" /> megfelelően inicializálni a szerepkör a ServiceDefinition.csdef fájlban.

Egyéni teljesítményadatokat számláló elküldheti a diagnosztika ügynök használatával Azure tárolóhoz.

A szokásos számláló teljesítményadatokat az Azure folyamatok hozza létre. A szerepkör vagy dolgozó szerepkör webalkalmazás létre kell hozni egyéni teljesítményadatokat számláló. Lásd: a [Teljesítmény Számláló típusú](https://msdn.microsoft.com/library/z573042h.aspx) egyéni teljesítmény számláló tárolt adatok típusú információt. [PerformanceCounters minta](http://code.msdn.microsoft.com/azure/) hoz létre, és egyéni teljesítményadatokat számláló állít be egy webes szerepkör példát talál.

## <a name="store-and-view-performance-counter-data"></a>Tár és nézet számláló teljesítményadatokat

Azure gyorsítótárát és más diagnosztikai információ számláló teljesítményadatokat. Az adatok figyelemmel kísérésére remote futtatásakor a szerepkör-példányt az távoli asztali access-eszközök, például teljesítményét Monitor megtekintése érhető el. Továbbra is fennáll a szerepkör-példány kívül az adatokat, hogy a diagnosztika agent kell át az adatok Azure tárhelyet. A gyorsítótárban tárolt számláló teljesítményadatok méretkorlátok vonatkoznak azokra az diagnosztika ügynök beállíthatók, vagy egy megosztott korlátját a diagnosztikai adatok részének is beállítható. Pufferelési méretének beállításával kapcsolatos további tudnivalókért olvassa el a [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) és [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx)című témakört. Lásd: a [tár és diagnosztikai adatok megtekintése az Azure-tárolóban lévő](https://msdn.microsoft.com/library/azure/hh411534.aspx) adatok átvitele a tárterület-fiók beállítása a diagnosztika ügynök áttekintése.

Minden egyes konfigurált teljesítmény számláló példány rögzítését egy megadott mintavételnél díjazott, és a mintában szereplő adatokat a tárterület-fiók egy ütemezett átadás kérelmet, vagy igény szerinti átvitele felkérés átkerül. Az automatikus átvitelek amilyen gyakran percenkénti beütemezett lehet. A diagnosztikai agent átviheti a számláló teljesítményadatokat tábla WADPerformanceCountersTable, a tárhely fiók vannak tárolva. Az alábbi táblázat előfordulhat, hogy mindenki számára hozzáférhetők és a szokásos Azure tárolási API módszerek lekérdezett. Lásd: [Microsoft Azure PerformanceCounters minta](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) , például egy lekérdezési és a teljesítmény számláló adatok a WADPerformanceCountersTable táblából megjelenítése.

>[AZURE.NOTE] Attól függően, hogy a diagnosztika ügynök átadás gyakoriság és várólista késés a legutóbbi számláló teljesítményadatokat tároló fiók lehet néhány percig, amíg szorul.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Engedélyezze a teljesítmény számláló diagnosztika konfigurációs fájl használatával

Az alábbi eljárással engedélyezése a teljesítmény számláló az Azure-alkalmazásban.

## <a name="prerequisites"></a>Előfeltételek

Ez a szakasz tartalma feltételezi, hogy a diagnosztika monitor importálja az alkalmazás és a diagnosztikai konfigurációs fájl hozzáadni a Visual Studio megoldás (SDK 2.4 és az alábbi diagnostics.wadcfg vagy diagnostics.wadcfgx SDK 2,5 és a fenti). 1 és 2 [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](./cloud-services-dotnet-diagnostics.md)a lépések megtekintése) további információt.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Lépés: 1: Összegyűjtése és a teljesítmény számláló adatainak tárolása

Miután hozzáadta a diagnosztika fájlt meg a Visual Studio megoldást a webhelycsoport és a teljesítmény számláló adatok tárolásának beállíthatja egy Azure alkalmazásra. Ez történik, a diagnosztikai fájlt teljesítmény számláló hozzáadásával. Diagnosztikai adatok, beleértve a teljesítmény számláló, először gyűjtött, a példányon. Az adatok majd állandó az Azure-táblából szolgáltatásban a WADPerformanceCountersTable táblázatba, így is kell a tároló fiók adhat meg az alkalmazás. Ha az alkalmazás helyi meghajtóra, esetén tesztelése a kiszámítania irányító, is tárolhat diagnosztikai adatok helyileg a tárhely irányító a. Mielőtt diagnosztika adattárolásra kell először az [Azure klasszikus portál](http://manage.windowsazure.com/) és tárterület-fiók létrehozása. A legjobb, keresse meg a tárterület-fiókja ugyanazon a helyen geo – az Azure-alkalmazásként külső sávszélesség-költségek fizetés elkerülése érdekében, és csökkentheti a késés.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>A diagnosztikai fájl teljesítmény Számláló hozzáadása

Vannak olyan sok számlálót is használhatja. A következő példa bemutatja a több teljesítmény számláló webes és dolgozó szerepkör figyelése ajánlott.

Nyissa meg a diagnosztika fájlt (SDK 2.4 és az alábbi diagnostics.wadcfg vagy diagnostics.wadcfgx SDK 2,5 és fölött), és adja hozzá a következő DiagnosticMonitorConfiguration elem:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

A bufferQuotaInMB attribútum, amely meghatározza, hogy a legnagyobb mennyiségű rendszer fájltároló elérhető webhelycsoport adattípust (Azure naplók, az IIS naplók stb.). Az alapértelmezett érték 0. Ha eléri a kvóta, a legrégebbi adatokat törlődik, mint új adatok hozzáadása. A SZUM, az összes bufferQuotaInMB tulajdonságok a OverallQuotaInMB attribútum értékét nagyobbnak kell lennie. Annak megállapítása, hogy mennyi tárhely szükséges az diagnosztika adatgyűjtés lesz a részletesen tárgyalja című beállítási ÜVEGVATTA [Azure-alkalmazások fejlesztésével kapcsolatos gyakorlati](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)tanácsokat hibaelhárítási.

A scheduledTransferPeriod attribútum, amely meghatározza, hogy az adatok továbbítása ütemezett közötti időtartamot, felkerekítve a legközelebbi percet. Az alábbi példákban beállítás PT30M (30 perc). Állítsa be az átadás időszakot kis értékre, például az 1 perc, negatív hatással lehetnek a gyártás teljesítményét az alkalmazás, de gyorsan működik, ha tesztelése megnéz diagnosztika hasznos lehet. Az ütemezett átadás időszak kell kicsi győződjön meg arról, hogy diagnosztikai adatok nem írja felül a példányon, de elég nagy ahhoz, hogy az alkalmazás teljesítményének nincs hatással.

A counterSpecifier attribútum határozza meg, hogy a teljesítményét számláló gyűjthetők össze. A sampleRate attribútum határozza meg, hogy a ráta, amelynél a teljesítményét számláló kell mintát venni, ebben az esetben 30 másodperces.

Miután elhelyezte a gyűjtendő teljesítmény teljesítménymutatói, mentse a módosításokat a diagnosztika fájlt. Következő lépésként meg kell adja meg a tárterület-fiókot, amely a diagnosztikai adatok maradnak a.

### <a name="specify-the-storage-account"></a>Adja meg a tárterület-fiók

Továbbra is fennáll a diagnosztikai adatok Azure tárterület-fiókjába, meg kell adnia a kapcsolati karakterlánc a szolgáltatás konfigurációs (ServiceConfiguration.cscfg) fájl.

Azure SDK 2,5 a tárterület-fiókot is megadható a diagnostics.wadcfgx fájlban.

>[AZURE.NOTE] Ezeket az utasításokat a Azure SDK 2.4 és alatti csak vonatkoznak. Azure SDK 2,5 a tárterület-fiókot is megadható a diagnostics.wadcfgx fájlban.

A kapcsolati karakterláncot beállítása:

1. Nyissa meg a ServiceConfiguration.Cloud.cscfg fájlt a kedvenc szövegszerkesztővel, és adja meg a kapcsolati karakterlánc-tárhelyet. A *fióknév* és *AccountKey* értéket az Azure klasszikus portál tároló fiók kezelése billentyűk irányítópulton találhatók.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Mentse a ServiceConfiguration.Cloud.cscfg fájlt.

3. Nyissa meg a ServiceConfiguration.Local.cscfg fájlt, és ellenőrizze, hogy UseDevelopmentStorage értéke igaz.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Most, hogy vannak beállítva, a kapcsolati karakterláncot, az alkalmazás megmaradnak diagnosztikai adatok a tárterület-fiókjába, amikor az alkalmazás telepítve van.
4. Mentse és készítése a projekthez, majd az alkalmazás telepítéséhez.

## <a name="step-2-optional-create-custom-performance-counters"></a>Lépés: 2: (Nem kötelező) létrehozása egyéni teljesítményét számláló

Az előre definiált teljesítmény számláló kívül saját egyéni teljesítmény számláló Lync-webhely vagy dolgozó szerepkörök is hozzáadhat. Egyéni teljesítmény számláló nyomon követése és az alkalmazás-specifikus viselkedés figyelése és létrehozhatók, illetve indítási tevékenység, webes szerepkör vagy magasabb szintű jogosultsággal rendelkező dolgozó szerepkör törölt is használható.

Az Azure diagnosztika ügynök frissíti a teljesítményét számláló konfiguráció .wadcfg fájlból egy perc múlva kezdve.  Ha egyéni teljesítmény számláló a OnStart módszerrel hoz létre, és a indítási feladatok hosszabb időt vehet igénybe egy percnél végrehajtani, az egyéni teljesítmény számláló fog nem jött létre, amikor az Azure diagnosztika ügynök próbál meg betölteni őket.  Ebben az esetben jelenik meg, hogy helyesen Azure diagnosztika rögzíti a az egyéni teljesítményét számláló kivételével az összes diagnosztikai adatok.  A probléma megoldásához a teljesítménymutatók létrehozása egy indítási tevékenység vagy áthelyezése a indítási feladat néhány működik a OnStart módszerrel a teljesítmény számláló létrehozása után.

A következő lépésekkel hozzon létre egy egyszerű egyéni teljesítmény számláló a "\MyCustomCounterCategory\MyButton1Counter" nevű:

1. Nyissa meg az alkalmazás szolgáltatás-definíciós fájl (CSDEF).
2. A WebRole futtatókörnyezet elem vagy magasabb szintű jogosultságokkal rendelkező végrehajtására WorkerRole elem hozzáadása:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Mentse a fájlt.
4. Nyissa meg a diagnosztika fájlt (SDK 2.4 és az alábbi diagnostics.wadcfg vagy diagnostics.wadcfgx SDK 2,5 és fölött), és adja hozzá a következő a DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Mentse a fájlt.
6. Hozzon létre a egyéni teljesítmény számláló a szerepkör a OnStart módja az alap előtt. OnStart. Az alábbi C# példa létrehoz egy egyéni kategóriát, ha még nem létezik:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Frissítse a számláló az alkalmazáson belül. A következő példa frissíti egy egyéni teljesítmény számláló Button1_Click eseményekről:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Mentse a fájlt.  

Egyéni teljesítményadatokat számláló most fog gyűjti össze a Azure diagnosztika monitort.

## <a name="step-3-query-performance-counter-data"></a>Lépés 3: A lekérdezés számláló teljesítményadatokat

Amikor az alkalmazás telepítve van, és a diagnosztikai monitor futó megkezdi a teljesítmény számláló összegyűjtése és pályától tartós Azure tárolóhoz adatokat. Például a Visual Studio, az [Azure tároló Explorer](http://azurestorageexplorer.codeplex.com/)vagy [Azure diagnosztika Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) által Cerebrata Server Explorer eszközök segítségével a számláló teljesítményadatokat megtekintése a WADPerformanceCountersTable táblázatban. [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [fonetikus](../storage/storage-ruby-how-to-use-table-storage.md)vagy [PHP](../storage/storage-php-how-to-use-table-storage.md)táblázat szolgáltatás is programozottan is lekérdezhetők.

A következő C# Példa egyszerű lekérdezés szemben a WADPerformanceCountersTable tábla, és a diagnosztikai adatok CSV-fájlba menti. Miután a teljesítmény számláló CSV-fájlba menti az nyújtó grafikus funkciók a Microsoft Excel vagy más eszköz egyes az adatok ábrázolása is használhatja. Ügyeljen arra, hogy a abban az Azure SDK a .NET rendszerhez október 2012-és újabb Microsoft.WindowsAzure.Storage.dll, mutató hivatkozás hozzáadása. Az összeállítás telepítve van a % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ címtárhoz.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Személyek hozzárendelése C# használata osztály egyéni **TableEntity**származó objektumok. A következő kódot, amely a teljesítmény számláló a **WADPerformanceCountersTable** táblában egy személy osztály határozza meg.


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Következő lépések
[Nézet további cikkek az Azure diagnosztika] (.. / azure-diagnostics.md)
