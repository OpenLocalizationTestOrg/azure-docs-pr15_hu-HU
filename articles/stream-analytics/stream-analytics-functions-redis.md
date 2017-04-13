<properties
    pageTitle="Az Azure vgx.dll gyorsítótárba, Azure függvények az Azure Értékáram-elemzés használata kimeneti |} Microsoft Azure"
    description="További tudnivalók az Azure függvény használatát csatlakoztatott szolgáltatás Bus várólista egy a kimenet Értékáram-elemzés feladat Azure vgx.dll gyorsítótár kitöltéséhez."
    keywords="adatfolyam-gyorsítótár vgx.dll, szolgáltatás bus várólista"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Azure adatfolyam Analytics adatainak tárolása az Azure vgx.dll gyorsítótárban Azure függvény használata

Azure Értékáram-elemzés segítségével gyorsan fejlesztése, és a valós idejű háttérismeretek eszközök, érzékelők, infrastruktúra és alkalmazások vagy bármilyen adatfolyam adatok eléréséhez alacsony költség megoldások telepítése. Lehetővé teszi a különböző használati eset például valós idejű kezelése és figyelése, parancs vezérlő, csalás észlelése, csatlakoztatott autók és sok egyebet. Számos olyan esetek érdemes egy elosztott adatok tárba, például egy Azure vgx.dll gyorsítótár által Azure Értékáram-elemzés outputted adatok tárolására.

Tegyük fel, hogy távközlési vállalata részei. Hol érkező ugyanazzal az identitással egyszerre több hívások időt, de más földrajzilag SIM csalás feltárása próbál helyek. Vannak szolgáltatásait, az összes a potenciális csalárd telefonhívások tárolása az Azure vgx.dll gyorsítótár. Ez a blogban azt útmutatásokat találhat hogyan lehet egyszerűen a feladatot végrehajthatná. 

## <a name="prerequisites"></a>Előfeltételek
Töltse ki a [Valós idejű csalás észlelési] [ fraud-detection] segédlet az ASA

## <a name="architecture-overview"></a>Architektúra – áttekintés
![Architektúra képernyőképe](./media/stream-analytics-functions-redis/architecture-overview.png)

Ahogy a fenti ábrán látható, Értékáram-elemzés lehetővé teszi, hogy a folyamatos átvitelű lekérdezett, és olyan eredménye küldött bemeneti adatok. A kimenet alapján, Azure funkciók majd válthat valamilyen esemény típusú. 

Ez a blogban azt koncentráljon pontosabban a kiváltó esemény gyorsítótárba csalárd adatokat tároló, vagy a folyamat Azure függvények részét.
Miután elkészült a [Valós idejű csalás észlelési] [ fraud-detection] oktatóanyagban van egy beviteli (esemény központi), a lekérdezés és a már beállított és operációs rendszert futtató olyan eredménye (blob-tárolóhoz). Ez a blog azt módosítsa a kimenet Bus várólista szolgáltatást használja helyette. Ezt követően a várólista az Azure függvény csatlakozás azt. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Hozzon létre, és csatlakoztassa a szolgáltatás Bus várólista kimeneti
Szolgáltatás Bus várólista létrehozásához kövesse a lépéseket, 1 és 2 .NET szakaszának [Első lépések a szolgáltatás Bus sorban várakozó][servicebus-getstarted].
Most vegyük a várólista csatlakoztatása a Értékáram-elemzés feladat a korábbi csalás észlelési segédlet létrehozott.



1. Az Azure-portálon nyissa meg a **kimeneti értékeket** , a a feladat lap, és válassza a **Hozzáadás** elemre a lap tetején.

    ![Kimeneti értékeket hozzáadása](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Válassza a **Szolgáltatás Bus várólista** **gyűjtése** , és kövesse a képernyőn megjelenő utasításokat. Ne felejtse el, válassza a szolgáltatás Bus várólista [Szolgáltatást Bus sorban várakozó – első lépések]a létrehozott névtere[servicebus-getstarted]. Amikor elkészült, kattintson a "jobb" gombra.
3. Adja meg az alábbi értékeket:
    - **Esemény szerializáló formátum**: JSON
    - **Kódolás**: UTF8
    - **Formátum**: elválasztott sor

4. Kattintson a **Létrehozás** gombra a forrás hozzáadása, és ellenőrizze, hogy Értékáram-elemzés sikeresen a tárhely fiókhoz csatlakozhat.

5. A **lekérdezés** lap cserélje ki az aktuális lekérdezést az alábbi. *[: A szolgáltatás BUS neve]* cserélje le a 3 létrehozott kimeneti nevére. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Hozzon létre egy Azure vgx.dll gyorsítótár
Hozzon létre egy Azure vgx.dll gyorsítótár [használata Azure vgx.dll gyorsítótár módjáról] a .NET szakasz[ use-rediscache] mindaddig, amíg a csoport neve ***a gyorsítótár-ügyfelek beállítása***.
Miután elkészült, új vgx.dll gyorsítótár van. Az **összes beállításai**csoportban jelölje be a **hívóbetűk** és a megjegyzések lefelé az ***elsődleges kapcsolati karakterláncot***.

![Architektúra képernyőképe](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Hozzon létre egy Azure függvény
Hajtsa végre [az első Azure függvény létrehozása] [ functions-getstarted] oktatóprogram a Azure függvények használata. Ha már van egy Azure függvény, akinek szeretne használni, majd ugorjon a következő [Vgx.dll gyorsítótár írás](#Writing-to-Redis-Cache)

1. A portálon válassza az alkalmazás szolgáltatásokat bal oldali navigációs sávjából, majd kattintson az Azure függvény alkalmazás nevére, a függvény alkalmazás webhely eléréséhez.
    ![Képernyőkép a alkalmazáslistában szolgáltatások függvény](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Kattintson a **Új függvény > ServiceBusQueueTrigger – C#**. A következő mezők kövesse az alábbi utasításokat:
    - **Várólista neve**: neve megegyezik a sorban az [Első lépések a szolgáltatás Bus sorban várakozó] létrehozásakor a megadott név[ servicebus-getstarted] (nem a szolgáltatás bus neve). Ellenőrizze, hogy a várakozási sorban található, amelyhez csatlakozik a Értékáram-elemzés eredménye használ.
    - **Szolgáltatás Bus kapcsolat**: válassza a **Hozzáadás a kapcsolati karakterlánc**. Keresse meg a kapcsolati karakterláncot, a klasszikus portált, és jelölje be a **Szolgáltatást**, a létrehozott szolgáltatás bus, illetve a **Kapcsolat ADATAIT** a képernyő alján. Győződjön meg róla, hogy a főképernyőn ezen a lapon. Másolja és illessze be a kapcsolati karakterlánc. Nyugodtan írja be a minden kapcsolat nevét.
    
        ![Képernyőkép: szolgáltatás bus kapcsolat](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: válassza a **kezelése**


3. Kattintson a **Létrehozás** gombra

## <a name="writing-to-redis-cache"></a>Gyorsítótár vgx.dll írása
Most már van egy Azure függvény, amely beolvassa a szolgáltatás Bus sorból létrehozott. Összes még hátralévő feladatokat az, hogy ezeket az adatokat a vgx.dll gyorsítótár írni a függvény használatával. 

1. Jelölje ki az újonnan létrehozott **ServiceBusQueueTrigger**, és a jobb felső sarokban kattintson a **függvény alkalmazás beállításai** gombra. Válassza a **Nyissa meg az App szolgáltatás beállításai > Beállítások > alkalmazás beállításainak**

2. A kapcsolati karakterláncot csoportban hozzon létre egy nevet a **név** szakaszban. Illessze be az elsődleges kapcsolati karakterláncot, az **létrehozása egy vgx.dll gyorsítótár** lépésben az **érték** csoportban található. Válassza az **egyéni** , ahol megjelölésekor **SQL-adatbázishoz**.

3. A képernyő tetején kattintson a **Mentés** gombra.

    ![Képernyőkép: szolgáltatás bus kapcsolat](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Most már térjen vissza az alkalmazás szolgáltatás beállításai, és válassza a **Eszközök > alkalmazás szolgáltatás szerkesztő (előzetes verzió) > a > Ugrás**.

    ![Képernyőkép: szolgáltatás bus kapcsolat](./media/stream-analytics-functions-redis/app-service-editor.png)

5. Egy tetszés szerinti szerkesztőben **project.json** a következő nevű JSON-fájl létrehozása, és mentse a merevlemezre.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Ez a fájl feltöltése át a legfelső szintű directory, a függvény (nem WWWROOT). Meg kell jelennie **project.lock.json** automatikusan nevű fájl jelenik meg, amely megerősíti, hogy, hogy a Nuget csomagok "StackExchange.Redis" és "Newtonsoft.Json" importálva.

7. A **run.csx** fájl cserélje ki az előre létrehozott kód a következő kódot. A lazyConnection függvény cserélje ki az **adatokat tárolja a vgx.dll gyorsítótárba**a 2 létrehozott nevét "Csatlakoztatva neve".

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Indítsa el a Értékáram-elemzés feladat

1. A telcodatagen.exe-alkalmazások indításakor. A használatát a következőképpen történik:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Kattintson a adatfolyam Analytics feladat fel a portálon, **Indítsa el** a lap tetején.

    ![Képernyőkép: a projekt kezdési](./media/stream-analytics-functions-redis/starting-job.png)

3. A megjelenő **Indítsa el a feladat** lap **gombra** , és kattintson a **Start** gombra, a képernyő alján. A feladat állapota megváltozik, kezdési és bizonyos idő módosításai futtatása után.
 
    ![Képernyőkép a kezdő feladat idő kiválasztása](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Futtassa a megoldást, és jelölje be az eredmények
Visszalépés a **ServiceBusQueueTrigger** lap, ekkor jelennie napló kimutatások. Ezek a naplók megjelenítése a szolgáltatás Bus várólista mozgatófogantyúit, hogy az adatbázisba valamit, amit kapott, és a be szeretné olvasni az idő használata a kulcsként ki!

Győződjön meg arról, hogy az adatok a vgx.dll gyorsítótár, lépjen az új portálon vgx.dll gyorsítótár lapjára (mint az előző lépésben [létrehozása az Azure vgx.dll gyorsítótár](#Create-an-Azure-Redis-Cache) látható), és válassza a konzolt.

Most már vgx.dll parancsok meggyőződni arról, hogy adatokat valójában gyorsítótár is írhat.

![Konzol vgx.dll képernyőképe](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Következő lépések
Azure függvények új szempont érdeklik, hogy Értékáram-elemzés közös hajthatja végre, és azt a projektet meg az új lehetőségek a feloldja. Ha a kívánt következő esetleges visszajelzéseit, nyugodtan az [Azure UserVoice webhely](https://feedback.azure.com/forums/270577-stream-analytics)használatával.

Ha új Microsoft Azure, azt meghívni, hogy próbálja ki az [ingyenes Azure próbaverzió fiók](https://azure.microsoft.com/pricing/free-trial/)feliratkozna szerint. Ha még kezdő az Értékáram-elemzés, majd azt meghívása, [az első Értékáram-elemzés feladat](stream-analytics-create-a-job.md)létrehozásához.

Ha már szükség súgó vagy kérdései vannak, Küldés a [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) vagy [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fórumokon. 

Az alábbi forrásokat is látható:

- [Azure függvények Fejlesztői segédlet](../azure-functions/functions-reference.md)
- [Azure függvények C# Fejlesztői segédlet](../azure-functions/functions-reference-csharp.md)
- [Azure függvények F # Fejlesztői segédlet](../azure-functions/functions-reference-fsharp.md)
- [Azure függvények NodeJS Fejlesztői segédlet](../azure-functions/functions-reference.md)
- [Azure függvények eseményindítók és kötések](../azure-functions/functions-triggers-bindings.md)
- [Azure vgx.dll gyorsítótár figyelése](../redis-cache/cache-how-to-monitor.md)

Követésére összes a legújabb hírek és szolgáltatásokkal, hajtsa végre a [@AzureStreaming](https://twitter.com/AzureStreaming) a Twitteren.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
