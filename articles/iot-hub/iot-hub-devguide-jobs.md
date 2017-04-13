<properties
 pageTitle="Fejlesztőeszközök útmutató - feladatok |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató – a-hubhoz csatlakozik több eszközön futtatásához feladatok ütemezése"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>A feladatok ütemezése több eszközön (előzetes verzió)

## <a name="overview"></a>– Áttekintés

Előző bekezdésre által leírtak Azure IoT központi lehetővé teszi, hogy az építőelemek számos ([eszköz kettős tulajdonságainak és a címkék] [ lnk-twin-devguide] és a [felhő-eszköz módszerek][lnk-dev-methods]).  Általában IoT vissza vége alkalmazások engedélyezése eszköz rendszergazdák és a felelősök frissítéséhez és tömegesen és ütemezett egyszerre IoT eszközök használata.  Feladatok beágyazására ütemezés egyszerre eszköz kettős frissítése és eszközök csoportja elleni C2D módszerek végrehajtását.  Például operátor, akinek kezdeményezése és nyomon követheti a feladat indítsa újra az eszközök 43 és padló 3 összeállítását, amely nem a tevékenységek az épület zavaró egyszerre egy sor vissza vége alkalmazás használja.

### <a name="when-to-use"></a>Mikor érdemes használni

Fontolja meg feladatokkal használatával: a megoldást vissza befejezése ütemezése és nyomon követni kell a egy sor olyan eszközt a következő tevékenységek valamelyikét:

- Eszköz kívánt kettős tulajdonságainak módosítása
- Eszköz kettős címkék frissítése
- C2D módszerek meghívása

## <a name="job-lifecycle"></a>Feladat életciklus

A megoldás háttér által kezdeményezett és IoT központi által fenntartott a feladatokat.  A feladat keresztül szolgáltatás elérésű URI kezdeményezhet (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) és a tevékenységállapotok keresztül szolgáltatás elérésű URI-végrehajtó feladaton lekérdezés (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Után egy feladat kezdeményezése feladatokhoz lekérdezése lehetővé teszi, a vissza vége alkalmazás frissítése futó feladatok állapotának.

> [AZURE.NOTE] Kezdeményez egy feladatot, tulajdonságok neve és értékek csak tartalmazhat USA-beli ASCII-nyomtatható alfanumerikus, kivéve a következő beállítás: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Hivatkozás témakörök:

A következő hivatkozást témakörök nyújtanak feladatok használatáról további információt.

## <a name="jobs-to-execute-direct-methods"></a>Közvetlen módszerek végrehajtani a feladatokat

A következő beállítás [közvetlen módszer] végrehajtása a HTTP 1.1 kérés részleteinek[ lnk-dev-methods] a feladat használatával eszközök csoportja:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Feladatok eszköz kettős tulajdonságainak módosítása

Az alábbiakban a HTTP 1.1 kérés részleteinek feladat használatával eszköz kettős tulajdonságainak módosítása:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>A feladatok előrehaladását lekérdezése

Az alábbi [Feladatok lekérdezése]HTTP 1.1 kérés részleteinek[lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
A válasz a continuationToken kapja.  

## <a name="jobs-properties"></a>Feladatokat tulajdonságok

Tulajdonságok és azok leírását tartalmazza, felhasználható lekérdezése esetén feladat vagy projekt eredmények listáját a következő:

| A tulajdonság | Leírás |
| -------------- | -----------------|
| **jobId** | Alkalmazás által biztosított azonosító-e a tevékenységen. |
| **Kezdés időpontja** | Alkalmazás által biztosított a projekt kezdési időpontjának (ISO-8601). |
| **Befejezés időpontja** | IoT központi dátuma (ISO-8601) Ha a feladat befejezése adni. Csak azt követően a feladat elérné a "elvégzett" állapot érvényes. | 
| **típus** | Feladatok típusú: |
| | **scheduledUpdateTwin**: A használni kívánt kettős tulajdonságok vagy a címkék frissítése feladatot. |
| | **scheduledDeviceMethod**: A használt eszköz metódus kettős csoportja a feladatot. |
| **állapot** | A feladat aktuális állapota. Állapot lehetséges értékei: |
| | **függőben** : ütemezése, és a feladat szolgáltatás felvételre váró. |
| | **ütemezett** : a jövőben időpontra ütemezett. |
| | **futó** : jelenleg aktív feladat. |
| | **megszakítva** : feladat meg lett szakítva. |
| | **nem sikerült** : nem sikerült feladatot. |
| | **Befejezett** : feladat befejeződött. |
| **deviceJobStatistics** | A feladat végrehajtását statisztikájának. |

A villámnézetben a deviceJobStatistics objektumot csak akkor érhető el a feladat befejezése után.

| A tulajdonság | Leírás |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | A feladat eszközök száma. |
| **deviceJobStatistics.failedCount** | Ha nem sikerült a feladat eszközök száma. |
| **deviceJobStatistics.succeededCount** | Ha a feladat sikeres eszközök száma. |
| **deviceJobStatistics.runningCount** | A projekt éppen futó eszközök száma. |
| **deviceJobStatistics.pendingCount** | A feladat futtatása váró eszközök száma. |


### <a name="additional-reference-material"></a>További anyagminta

A Fejlesztőeszközök útmutatóban hivatkozás témakörök a következők:

- [IoT központi végpontok] [ lnk-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez.
- [Szabályozási és kvóták] [ lnk-quotas] a IoT központi szolgáltatás és számíthat, amikor a szolgáltatás használatához a szabályozási viselkedés vonatkozó kvóták ismerteti.
- [IoT központi eszközök és szolgáltatások SDK] [ lnk-sdks] a különböző nyelvi SDK sorolja fel, egy eszköz és a szolgáltatás-alkalmazások használata a IoT központi fejlesztésekor használja.
- [Lekérdezési nyelv twins, módszerek és feladatok] [ lnk-query] ismerteti a lekérdezésnyelv IoT központi eszköz twins, módszerek és feladatok kapcsolatos adatok kinyerése is használhatja.
- [IoT központi MQTT támogatási] [ lnk-devguide-mqtt] IoT központi támogatási további információt tartalmaz a MQTT protokollhoz.

## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, próbálja meg néhány a jelen cikkben ismertetett fogalmak, amelyek érdekelhetik a következő IoT központi oktatóprogram során:

- [Ütemezés, és a feladatok][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
