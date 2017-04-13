<properties
 pageTitle="Távoli figyelés előre megoldás forgatókönyv |} Microsoft Azure"
 description="Az Azure IoT leírását a megoldás távoli figyelemmel kísérésére és a saját architektúra előre."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Távoli figyelés előre megoldás útmutató

## <a name="introduction"></a>– Bevezetés

A figyelés [megoldás előre] IoT csomagja távoli[ lnk-preconfigured-solutions] --végpont megvalósítása figyeli operációs rendszert futtató távoli helyeken több gépekhez megoldás. A megoldás egyesíti a vállalati eset általános nyújtani kulcsfontosságú Azure szolgáltatások, és felhasználhatja azt kiindulási pontként saját végrehajtásához. [Testreszabhatja] [ lnk-customize] saját meghatározott üzleti megfeleljen a megoldást.

Ez a cikk végigvezeti a elemeivel kulcsfontosságú ahhoz, hogy ha meg szeretné érteni, hogy hogyan működik a távoli felügyeleti megoldás. Ez a Tudásbázis nyújt segítséget:

- A megoldás kapcsolatos problémák megoldása.
- Tervezze meg a saját igényekhez megoldás testreszabása. 
- A saját Azure szolgáltatásokat használó IoT-megoldás tervezése.

## <a name="logical-architecture"></a>Logikai architektúra

Az alábbi ábra az előre beállított megoldás logikai összetevői ismerteti:

![Logikai architektúra](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Szimulált eszközök

Az előre beállított megoldás a szimulált eszközön jelöl (például épület légkondicionáló vagy létesítmény air kezelési egység) hűtési eszköz. Az előre beállított megoldás telepítésekor akkor is automatikusan kiépítése négy szimulált eszközökön futó az [Azure WebJob][lnk-webjobs]. Szimulált eszközök megkönnyítik a feltárása a megoldást üzembe bármely fizikai eszközök nélkül viselkedését. A tényleges fizikai eszközök üzembe helyezéséhez lásd: a az [eszközét, hogy a távoli csatlakozás figyelése előre beállított megoldás] [ lnk-connect-rm] oktatóprogram.

Minden egyes szimulált eszközön küldhet a következő üzenet típusú IoT központi:

| Üzenet  | Leírás |
|----------|-------------|
| Indítási  | Az eszköz indításakor adatait tartalmazó magát vissza végén **eszköz-információ** üzenetet küld. Ezeket az adatokat az eszközazonosítót, az eszköz metaadatai, a parancsok listáját tartalmazza az eszköz támogatja, és az eszköz szoftver jelenlegi konfigurációjának megfelelően. |
| Jelenléti adatok | Egy eszközt rendszeres jelentést, hogy az eszköz is észleli a az a személy jelenléti **jelenléti** üzenetet küld. |
| Telemetriai | Egy eszközt rendszeres időközönként, amely a hőmérsékleti és szimulált érzékelőktől az eszközről gyűjtött páratartalom szimulált értékének jelentések **telemetriai** üzenetet küld. |


Szimulált eszközök küldje el az alábbi eszköz tulajdonságainak **eszköz-információ** üzenetben:

| A tulajdonság               |  Cél |
|------------------------|--------- |
| Eszköz azonosítója              | Az azonosító megadott vagy rendelt, amikor egy eszközt a megoldást hoz létre. |
| Gyártó           | Eszközgyártó |
| Modell száma           | Az eszköz modell száma |
| Sorszám          | Az eszköz dátumértékét |
| Belső vezérlőprogram               | Aktuális verziójának belső vezérlőprogram az eszközön |
| Platform               | Az eszköz platform-architektúra |
| Processzor              | Az eszköz futtatása processzor |
| A telepített RAM          | Az eszközre telepített RAM mennyisége |
| Központi engedélyezett állapota      | Az eszköz a IoT központi state tulajdonság |
| Létrehozva           | Az eszköz a megoldás létrehozásának |
| Feltöltés időpontja           | Legutóbbi tulajdonságok lettek frissítve az eszköz |
| Szélesség               | Az eszköz szélesség helye |
| Hosszúság              | Az eszköz hosszúság helye |

A simulatort használja osztja fel ezeket a minta értékű szimulált eszközök tulajdonságait.  Minden alkalommal, amikor a simulatort használja egy szimulált eszközt előkészíti az eszközre az előre definiált metaadatok bejegyzések a IoT-hubon keresztül csatlakozott. Fontos tudni, hogyan ezzel felülírja a metaadat-alapú frissítéseiről az eszköz portálon.


Szimulált eszközök képes kezelni az megoldás irányítópult a IoT hubon keresztül küldött, az alábbi parancsokat:

| Parancs                | Leírás                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | A _ping_ küld életben érdemes ellenőrizni az eszközön   |
| StartTelemetry         | Elindítja az eszköz telemetriai küldése                 |
| StopTelemetry          | Az eszköz telemetriai küldésének leállítása             |
| ChangeSetPointTemp     | A pont beállítása értéket, amely körül a véletlen adatok jön létre módosítása |
| DiagnosticTelemetry    | Elindítja a eszköz simulatort használja egy további telemetriai érték (externalTemp) küldése |
| ChangeDeviceState      | A bővített state tulajdonság az eszköz változik, és az eszköz adatainak üzenetet küld az eszközről |

Az eszköz parancs nyugta megoldás vissza végén áll rendelkezésre a IoT hubon keresztül.

## <a name="iot-hub"></a>IoT központi

A [központi IoT] [ lnk-iothub] ingests az a felhő a eszközökről küldött adatok és az Azure Értékáram-elemzés (ASA) feladatok számára elérhetővé teszi azt. IoT központi is küld parancsok az eszközöket az eszköz portál nevében. Minden egyes adatfolyam ASA feladat IoT központi fogyasztóvédelmi külön csoportban használja az adatfolyam az üzenetek olvasása készülékekről.

## <a name="azure-stream-analytics"></a>Azure Értékáram-elemzés

A távoli felügyeleti megoldásban [Azure Értékáram-elemzés] [ lnk-asa] (ASA) kiszállítja eszköz üzenetet megkapta a másik háttéradatbázis összetevőt feldolgozás vagy tárolás IoT elosztót. Az üzenetek tartalma alapján meghatározott funkciók különböző ASA feladatok végrehajtása

**Feladat 1: eszköz információ** szűri az eszköz információk üzeneteket a beérkező üzenetek adatfolyam, és elküldi őket egy esemény központi végpontot. Egy eszközt eszköz tájékoztató üzenetek átirányítása az indításkor és **SendDeviceInfo** parancs hatására. A feladat használja az alábbi lekérdezés definíciójának **eszköz-info** üzenetek azonosítása:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

A feladat további feldolgozásra az eredményt küld egy esemény-hubon keresztül csatlakozott.

**Feladat 2: szabályok** kiértékeli a bejövő hőmérséklet és páratartalom telemetriai értékek eszköz küszöbértékek szemben. A szabályok szerkesztőben érhető el a megoldást irányítópulton küszöbértéket vannak beállítva. Minden egyes eszköz/érték pár egy blob, amely Értékáram-elemzés szól a **Hivatkozási adatok**, az időbélyegző tárolja. A feladat hasonlítja össze az eszköz beállítása küszöbértékét elleni bármely nem üres érték. Ha nagyobb, mint a ">" feltétel, a feladat kimenet **riasztási** esemény, amely azt jelzi, hogy a küszöbértékét túllépik az eszközt, érték és időbélyeg értékek biztosít. A feladat használja az alábbi lekérdezés definíciójának telemetriai jelennek meg, amelyek indítson el egy riasztás azonosítása:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

A feladat esemény-hubon keresztül csatlakozott, további feldolgozásra elküldi az eredményt, és a minden riasztás részleteit menti blob-tárolóhoz a hol olvassa el a megoldást irányítópult is riasztási információkat.

**Feladat 3: Telemetriai** meg az eszköz a bejövő telemetriai adatfolyam kétféle módon működik. Az első a eszközökről összes telemetriai üzeneteket küld állandó blobtárolóhoz hosszú távú tárhelyként használható. A második értékek átlag, minimum és maximum páratartalom kiszámítja a 5 perc csúsztatható ablak fölé, és ezeket az adatokat küld a blob-tárolóhoz. A megoldás irányítópult telemetriai adatokat olvas blob-tárolóhoz a diagramok kitöltéséhez. Ezt a feladatot a következő Lekérdezésdefiníció használja:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Esemény hubok

Az **eszköz adatai** , majd a **szabályok** ASA feladatok kimeneti esemény hubok megjelenítheti az **Esemény processzor** fut a WebJob biztos, hogy továbbítani szeretné az adataikat.

## <a name="azure-storage"></a>Azure tárhely

A megoldás Azure blob-tárolóhoz használja a probléma továbbra is fennáll a megoldás eszközökről nyers és összesített telemetriai adatokat. Az irányítópult telemetriai adatokat olvas blob-tárolóhoz a diagramok kitöltéséhez. Értesítések megjelenítéséhez az irányítópulton a adatokat olvas blob-tárolóhoz, hogy a rekordok telemetriai értékek túllépésekor a beállított küszöbértéket. A megoldást is használja a blob-tárolóhoz rögzítheti az beállítja az irányítópulton küszöbértéket.

## <a name="webjobs"></a>WebJobs

Mellett az eszköz szimulátoros oktatás módszereiről szolgáltatója, az WebJobs a megoldás is elhelyezheti egy Azure WebJob fogópontok eszköz tájékoztató üzenetek és a válaszokat parancs futtatása **Esemény processzor** . Használja:

- Eszköz tájékoztató üzenetek frissítése a eszköz beállításjegyzék (DocumentDB adatbázisban tárolt) az aktuális eszköz információkkal.
- Az eszköz frissítése parancs válaszüzenetek parancs előzmények (DocumentDB adatbázisban tárolt).

## <a name="documentdb"></a>DocumentDB

A megoldás DocumentDB adatbázis használja a megoldást csatlakoztatott eszközök adatainak tárolása. Ezt az információt a metaadat-alapú eszköz és eszközök küldeni az irányítópult-parancsok az előzményeket tartalmazza.

## <a name="web-apps"></a>Web Apps alkalmazások

### <a name="remote-monitoring-dashboard"></a>Távoli felügyeleti Irányítópult
A lapon, a webalkalmazásban telemetriai adatokat ábrázolásához PowerBI javascript-vezérlők (lásd: [PowerBI-látványelemek repó](https://www.github.com/Microsoft/PowerBI-visuals)) használja a eszközökről. A megoldás blob-tároló telemetriai adatokat írni a ASA telemetriai feladat használja.


### <a name="device-administration-portal"></a>Eszköz felügyeleti portál

A web app lehetővé teszi, hogy:

- Új eszköz rendelkezni. Ez a művelet az egyedi eszközazonosítót állítja be, és a hitelesítési kulcsot hoz létre. A központi IoT identitás beállításjegyzék és a megoldás-specifikus DocumentDB adatbázist is az eszközre vonatkozó adatok ír.
- Az eszköz tulajdonságainak kezelése. Ez a művelet a meglévő tulajdonságainak megtekintése és frissítése az új tulajdonságokat tartalmazza.
- Küldje el parancsok egy eszközt.
- Az eszköz parancs előzményeinek megtekintése.
- Engedélyezése és letiltása az eszközöket.

## <a name="next-steps"></a>Következő lépések

A TechNet következő blogbejegyzések részletesebb információkat a távoli felügyeleti előre beállított megoldást nyújtanak:

- [IoT – a motorháztető alatt - csomagja távoli figyelése](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [Hozzáadás élő és szimulált eszközök - távoli figyelés - IoT programcsomagban](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Továbbra is az alábbi cikkekben olvasásával IoT csomagja első lépések:

- [Csatlakoztassa az eszközt a távoli felügyeleti előre beállított megoldás][lnk-connect-rm]
- [A azureiotsuite.com webhely engedélyei][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md