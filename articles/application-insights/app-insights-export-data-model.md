<properties 
    pageTitle="Alkalmazás háttérismeretek adatmodell" 
    description="A JSON folyamatos Exportálás az exportált, és szűrőként használt tulajdonságokat ismerteti." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Alkalmazás háttérismeretek exportálás adatmodell

Ez a táblázat tulajdonságainak telemetriai az [Alkalmazás az összefüggéseket](app-insights-overview.md) SDK küldünk a portálra. Az adatok [Folytonos exportálása](app-insights-export-telemetry.md)kimenetét tulajdonságokból talál.
Tulajdonságszűrők [Metrikus Explorer](app-insights-metrics-explorer.md) és [Diagnosztikai keresési](app-insights-diagnostic-search.md)is megjelennek.

Megjegyzés: a címzett pontok:

* `[0]`az alábbi táblázat azt jelzi, a pont, az elérési út hol kell; Tárgymutató beszúrása de még mindig nem 0.
* Idő időtartamok vannak mikroszekundum, így 10000000 tized == 1 másodperc.
* Dátumok és időpontok UTC, és az ISO formátum kapnak`yyyy-MM-DDThh:mm:ss.sssZ`

Vannak, amelyek bemutatják, hogyan lehet a segítségükkel több [minták](app-insights-export-telemetry.md#code-samples) .



## <a name="example"></a>Példa

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Környezet

Az összes típusú telemetriai csatoltak, a környezet szakaszt. Nem az összes ezeket a mezőket az összes adatpont továbbítja.



|Elérési út|Típus|Jegyzetek|
|---|---|---|
| Context.Custom.Dimensions [0]  | objektum]  | Egyéni tulajdonságok paraméter meg karakterlánc kulcs – érték párokká. Kulcs maximális hossza 100, értékekkel, maximális hossz 1024. 100-nál több egyedi értékeket, a tulajdonság is szeretne keresni, de nem használható a szegmens. Max 200 billentyűk per ikey.  |
| Context.Custom.Metrics [0]  | objektum]  | Beállítása TrackMetrics és egyéni mérések paraméter által megadott értékekkel. Kulcs maximális hossza 100, értékek lehetnek numerikus. |
| context.data.eventTime | karakterlánc | UTC |
| context.data.isSynthetic | logikai érték | Kérelem érkezik bot vagy webhely teszten jelenik meg. |
| context.data.samplingRate | szám | A portál küldött SDK által generált telemetriai százaléka. Tartomány 0.0-100.0.|
| Context.Device | objektum | Ügyfél eszközön |
| Context.Device.Browser | karakterlánc | IE Chrome,...: |
| context.device.browserVersion | karakterlánc | A Chrome 48.0,...: |
| context.device.deviceModel | karakterlánc | |
| context.device.deviceName | karakterlánc | |
| Context.Device.ID | karakterlánc | |
| Context.Device.Locale | karakterlánc | en-GB, de – DE... |
| Context.Device.Network | karakterlánc | |
| context.device.oemName | karakterlánc | |
| context.device.osVersion | karakterlánc | A Host operációs rendszer |
| context.device.roleInstance | karakterlánc | Kiszolgáló állomás azonosítója |
| context.device.roleName | karakterlánc | |
| Context.Device.Type | karakterlánc | A böngésző számítógépen... |
| Context.Location | objektum | Clientip származik. |
| Context.location.City | karakterlánc | Ha már ismert, clientip, származás  |
| Context.location.ClientIP | karakterlánc | Utolsó nyolcszög van anonimizált pedig 0. |
| Context.location.Continent | karakterlánc | |
| Context.location.Country | karakterlánc | |
| Context.location.province | karakterlánc | Megye |
| Context.Operation.ID | karakterlánc | Az azonos művelet azonosító elemekre oszlopcímkeként jelennek meg a kapcsolódó elemek a portálon. Általában a kérelem azonosítója. |
| Context.Operation.Name | karakterlánc | URL-cím vagy kérés |
| context.operation.parentId | karakterlánc | Lehetővé teszi, hogy a beágyazott kapcsolódó elemek. |
| Context.Session.ID | karakterlánc | Az azonos forrásból származó műveletek csoport azonosítója. Művelet nélkül 30 percen belül a munkamenet végén jeleket. |
| context.session.isFirst | logikai érték | |
| context.user.accountAcquisitionDate | karakterlánc | |
| context.user.anonAcquisitionDate | karakterlánc | |
| context.user.anonId | karakterlánc | |
| context.user.authAcquisitionDate | karakterlánc | [Hitelesített felhasználó](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | logikai érték | |
| internal.data.documentVersion | karakterlánc | |
| Internal.Data.ID | karakterlánc | |



## <a name="events"></a>Események

Egyéni események [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event)által generált. 


|Elérési út|Típus|Jegyzetek|
|---|---|---|
| [0] események száma | egész szám | 100 / ([mintavételnél](app-insights-sampling.md) ráta). Ha például 4 =&gt; 25 %-át. |
| esemény [0] neve | karakterlánc | Esemény neve.  Legfeljebb 250. |
| esemény [0] URL-címe | karakterlánc | |
| esemény [0] urlData.base | karakterlánc | |
| esemény [0] urlData.host | karakterlánc | |

## <a name="exceptions"></a>A kivételek

Jelentések [Kivételek](app-insights-asp-net-exceptions.md) Server és a böngészőben. 


|Elérési út|Típus|Jegyzetek|
|---|---|---|
| [0] basicException összeállítás | karakterlánc | |
| [0] basicException száma | egész szám | 100 / ([mintavételnél](app-insights-sampling.md) ráta). Ha például 4 =&gt; 25 %-át. |
| [0] basicException exceptionGroup | karakterlánc | |
| [0] basicException exceptionType | karakterlánc | |karakterlánc | |
| [0] basicException failedUserCodeMethod | karakterlánc | |
| [0] basicException failedUserCodeAssembly | karakterlánc | |
| [0] basicException handledAt | karakterlánc | |
| [0] basicException hasFullStack | logikai érték | |
| [0] basicException azonosító | karakterlánc | |
| [0] basicException módszer | karakterlánc | |
| üzenet basicException [0] | karakterlánc | Kivételhiba üzenete. Legfeljebb 10 KB.|
| [0] basicException outerExceptionMessage | karakterlánc | |
| [0] basicException outerExceptionThrownAtAssembly | karakterlánc | |
| [0] basicException outerExceptionThrownAtMethod | karakterlánc | |
| [0] basicException outerExceptionType | karakterlánc | |
| [0] basicException outerId | karakterlánc | |
| basicException [0] [0] parsedStack összeállítás | karakterlánc | |
| basicException [0] [0] parsedStack fájlnév | karakterlánc | |
| basicException [0] [0] parsedStack szint | egész szám | |
| basicException [0] [0] parsedStack sor | egész szám | |
| basicException [0] [0] parsedStack módszer | karakterlánc | |
| [0] basicException Papírhalom | karakterlánc | Legfeljebb 10 KB|
| typeName basicException [0] | karakterlánc | |



## <a name="trace-messages"></a>Üzenetek nyomon követése

[TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)és a [kártya naplózás](app-insights-asp-net-trace-logs.md)által küldött.


|Elérési út|Típus|Jegyzetek|
|---|---|---|
| üzenet [0] naplózó_neve | karakterlánc ||
| üzenet [0] paraméterei | karakterlánc ||
| üzenet [0] nyers | karakterlánc | Log üzenet maximális hossza 10 KB. |
| üzenet [0] severityLevel | karakterlánc | |



## <a name="remote-dependency"></a>Távoli függőség

TrackDependency által küldött. Jelentés teljesítményének és látogatottságának [függőségek hívásainak](app-insights-asp-net-dependencies.md) a kiszolgálón lévő használja, és AJAX felhívja a böngészőben.

|Elérési út|Típus|Jegyzetek|
|---|---|---|
| aszinkron remoteDependency [0] | logikai érték | |
| [0] remoteDependency baseName | karakterlánc |  |
| [0] remoteDependency Parancs_neve | karakterlánc | Ha például "Kezdőlap/index" |
| [0] remoteDependency száma | egész szám | 100 / ([mintavételnél](app-insights-sampling.md) ráta). Ha például 4 =&gt; 25 %-át. |
| [0] remoteDependency dependencyTypeName | karakterlánc | HTTP, SQL,...: |
| [0] remoteDependency durationMetric.value | szám | Válasz függőség kiegészítésének hívást közötti út időtartama |
| [0] remoteDependency azonosító | karakterlánc | |
| [0] remoteDependency neve | karakterlánc | URL-címe. Legfeljebb 250.|
| [0] remoteDependency resultCode | karakterlánc | a HTTP típusú függés |
| a siker remoteDependency [0] | logikai érték | |
| [0] remoteDependency típusa | karakterlánc | HTTP, Sql,...: |
| [0] remoteDependency URL-címe | karakterlánc |  Legfeljebb 2000 |
| [0] remoteDependency urlData.base | karakterlánc | Legfeljebb 2000 |
| [0] remoteDependency urlData.hashTag | karakterlánc | |
| [0] remoteDependency urlData.host | karakterlánc | Legfeljebb 200 |


## <a name="requests"></a>Kérések

[TrackRequest](app-insights-api-custom-events-metrics.md#track-request)által küldött. A szabványos modulok ezzel a kiszolgáló válaszidő jelentések, a kiszolgáló mért. 


|Elérési út|Típus|Jegyzetek|
|---|---|---|
| kérés [0] száma | egész szám | 100 / ([mintavételnél](app-insights-sampling.md) ráta). Példa: 4 =&gt; 25 %-át. |
| [0] durationMetric.value kérése | szám | Válasz érkező összehívásokban ideje. 1e7 == 1s |
| [0] kérelem azonosítója | karakterlánc | A művelet azonosítója |
| kérés [0] neve | karakterlánc | KÉRÉSE vagy KÜLDÉS + alap URL-címét.  Legfeljebb 250 |
| [0] responseCode kérése | egész szám | HTTP-válasz ügyfélnek küldött |
| a siker kérelem [0] | logikai érték | Alapértelmezett == (responseCode &lt; 400) |
| [0] kérelem URL-címe | karakterlánc | A host nem beleértve |
| [0] urlData.base kérése | karakterlánc | |
| [0] urlData.hashTag kérése | karakterlánc |  |
| [0] urlData.host kérése | karakterlánc | |


## <a name="page-view-performance"></a>A lap nézet teljesítmény

A böngésző által küldött. Az idő egy weblapra, az értekezlet-összehívást megjelenítése teljes (kivéve az aszinkron AJAX-hívások) kezdeményezése felhasználótól feldolgozása mérése.

Helyi értékek megjelenítése az ügyfél operációs rendszer és böngésző verziószáma. 


|Elérési út|Típus|Jegyzetek|
|---|---|---|
| [0] clientPerformance clientProcess.value | egész szám | A lap megjelenítése a HTML-fogadása végére időt. |
| [0] clientPerformance neve | karakterlánc | |
| [0] clientPerformance networkConnection.value | egész szám | A hálózati kapcsolat létrehozásához szükséges idő. |
| [0] clientPerformance receiveRequest.value | egész szám | A kérelem küld a HTML-kód elhagyása válaszadáskor fogadása végére időt. |
| [0] clientPerformance sendRequest.value | egész szám | Az idő a HTTP-összehívás küldése venni. |
| [0] clientPerformance total.value | egész szám | A lap megjelenítése a kérelem elküldése elkezdhessék ideje. |
| [0] clientPerformance URL-címe | karakterlánc | A kérés URL-címe |
| [0] clientPerformance urlData.base | karakterlánc | |
| [0] clientPerformance urlData.hashTag | karakterlánc | |
| [0] clientPerformance urlData.host | karakterlánc | |
| [0] clientPerformance urlData.protocol | karakterlánc | |

## <a name="page-views"></a>Lap nézetek

TrackPageView() vagy [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Elérési út|Típus|Jegyzetek|
|---|---|---|
| nézet [0] száma | egész szám | 100 / ([mintavételnél](app-insights-sampling.md) ráta). Ha például 4 =&gt; 25 %-át. |
| [0] durationMetric.value megtekintése | egész szám | Tetszés szerint beállított trackPageView() vagy startTrackPage() – érték stopTrackPage(). Nem ugyanaz, mint clientPerformance értékeket. |
| [0] nézet neve | karakterlánc | Lap címét.  Legfeljebb 250 |
| nézet [0] URL-címe | karakterlánc | |
| [0] urlData.base megtekintése | karakterlánc | |
| [0] urlData.hashTag megtekintése | karakterlánc | |
| [0] urlData.host megtekintése | karakterlánc | |



## <a name="availability"></a>Elérhetőség

[Elérhetőség webes vizsgálatok](app-insights-monitor-web-app-availability.md)jelentést.

|Elérési út|Típus|Jegyzetek|
|---|---|---|
| elérhetőség [0] availabilityMetric.name | karakterlánc | elérhetőség |
| elérhetőség [0] availabilityMetric.value | szám |1.0 vagy 0.0 |
| elérhetőség [0] száma | egész szám | 100 / ([mintavételnél](app-insights-sampling.md) ráta). Ha például 4 =&gt; 25 %-át. |
| elérhetőség [0] dataSizeMetric.name | karakterlánc | |
| elérhetőség [0] dataSizeMetric.value | egész szám | |
| elérhetőség [0] durationMetric.name | karakterlánc | |
| elérhetőség [0] durationMetric.value | szám | Vizsgálat alatt. 1e7 == 1s |
| elérhetőség [0] üzenet | karakterlánc | Diagnosztikai hiba |
| elérhetőség [0] eredménye | karakterlánc | Fázis/Fail: |
| elérhetőség [0] runLocation | karakterlánc | Http kötelező GEO forrása |
| elérhetőség [0] testName | karakterlánc | |
| elérhetőség [0] testRunId | karakterlánc | |
| elérhetőség [0] testTimestamp | karakterlánc | |




## <a name="metrics"></a>Mértékek

Hozza létre TrackMetric().

A metrikus érték található context.custom.metrics[0]

Példa:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Metrikus értékek

Metrikus értékek, mind a metrikus jelentések és máshol, egy szabványos objektum szerkezettel jelentik. Példa:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Jelenleg - bár ez előfordulhat, hogy módosítja a jövőben – az összes érték jelenteni a szabványos SDK modulból `count==1` , és csak a `name` és `value` mezők hasznosak lehetnek. Az csak esetben, ha azokat más lenne lenne, ha a saját TrackMetric hívások írási amely, a további paramétereket állíthat be. 

A többi mező célja lehetővé teszi a mértékek összesítenie kell a SDK csökkentheti a forgalmat a portálra. Több egymást követő kiejtés például sikerült átlagos minden metrikus jelentés elküldése előtt. Ezután szeretné kiszámítani, a min, a maximum, a szórás és a összesített értéke (összegét vagy átlagát), és állítsa darab jelöli a jelentés értékek számát. 

A fenti táblázatban azt van hiányzik a ritkán használt mezők száma, a min, a maximum, a szórás és a sampledValue.

Előre összesítése mértékek, hanem [mintavételnél](app-insights-sampling.md) Ha telemetriai számának csökkentése kell is használhatja.


### <a name="durations"></a>Időtartamok

Azzal a különbséggel egyébként eseteket időtartamok egy mikroszekundum, tized jelennek meg, hogy 10000000.0 azt jelenti, hogy 1 másodperc.



## <a name="see-also"></a>Lásd még:

* [Alkalmazás Hírcsatornájában](app-insights-overview.md) 
* [Folytonos exportálása](app-insights-export-telemetry.md)
* [Mintakódok](app-insights-export-telemetry.md#code-samples)


