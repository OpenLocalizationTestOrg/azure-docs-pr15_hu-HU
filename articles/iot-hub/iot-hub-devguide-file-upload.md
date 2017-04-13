<properties
 pageTitle="Fejlesztőeszközök útmutató – a többi fájlfeltöltéshez |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató – a eszközről fájlfeltöltéskor IoT hubhoz"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>A eszközről fájlok feltöltése

## <a name="overview"></a>– Áttekintés

A részletes [IoT központi végpontjait] a[ lnk-endpoints] cikk eszközök kezdeményezhet fájlfeltöltések keresztül egy eszköz elérésű végpontot értesítést küld (**/devices/ {deviceId} / fájlok**).  Amikor egy eszközt a feltöltött IoT kezdőképernyő központjában értesítést küld, a IoT központi üzenetként is kap egy szolgáltatás elérésű végpontot (**/messages/servicebound/filenotifications**) keresztül, a fájl feltöltésekről tájékoztató értesítések hoz létre.

Kereskedelmi üzenetek magát IoT hubon keresztül, hanem IoT központi inkább működik-e egy forgalmazó társított Azure tárterület-fiókhoz. Egy eszközt kéri a tárhely jogkivonat, amely a fájl az eszközön töltse fel kívánja az IoT központból. Az eszközön használja a Társítások URI feltölteni a fájlt tároló, és amikor a feltöltés befejeződött az eszköz értesítést küld befejezésének IoT központi. IoT központi ellenőrzi, hogy a fájl feltöltése, és egy fájl feltöltése értesítés hozzáadja az új szolgáltatás elérésű fájl értesítési üzenetben végpontot.

Mielőtt IoT központi fájl feltöltése a eszközről, meg kell adnia a központi [Azure-tárolóhoz] társításával[ lnk-associate-storage] azt a fiókot.

Az eszköz ezután is, [egy feltöltés inicializálni] [ lnk-initialize] , és kattintson a [IoT központi értesítést] [ lnk-notify] a feltöltés befejezésekor. Ha szükséges, amikor egy eszközt jelzi, hogy helyesek-e a feltöltés IoT központi, a szolgáltatás hozhat létre [értesítő üzenet][lnk-service-notification].

### <a name="when-to-use"></a>Mikor érdemes használni

Ha kell feltölteni a fájlt egy eszközt a háttéradatbázist szolgáltatásban helyett IoT hubon keresztül az üzenet elküldése IoT központi funkció használata

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Azure tárterület-fiókkal társíthat IoT központi

A fájl feltöltése funkciónak a használatához először Azure tároló fiók hozzá kell rendelnie az IoT-központban. Ehhez az [Azure portálon]keresztül vagy[lnk-management-portal], vagy a [IoT központi erőforrás szolgáltató REST API -khoz]keresztül programozás útján[lnk-resource-provider-apis]. Miután a IoT központi Azure tároló fiókkal társított a szolgáltatás ad, Társítások URI eszközre Ha az eszköz kezdeményez egy fájl feltöltése kérelmet.

> [AZURE.NOTE] Az [Azure IoT központi SDK] [ lnk-sdks] automatikusan kezelheti a Társítások URI beolvasása, a fájl feltöltése és értesítésével arról, hogy egy befejezett feltöltött IoT kezdőképernyő központjában.

## <a name="initialize-a-file-upload"></a>Egy fájl feltöltése inicializálni

IoT hubhoz végpont kifejezetten eszközök feltölteni a fájlt tároló egy Társítások URI kérhet. Az eszköz elindítja fájl feltöltése a IoT-hubon keresztül csatlakozott, a bejegyzés küldésével `{iot hub}.azure-devices.net/devices/{deviceId}/files` a következő JSON szervezethez:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT központi adja eredményül a következő, hogy az eszközt használó feltölteni a fájlt:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Elavult: egy fájl feltöltése a GET inicializálni

> [AZURE.NOTE] Ez a szakasz ismerteti, hogyan Társítások URI fogadása IoT központból elavult funkciókat. A fent leírt bejegyzés módszer használja.

IoT hubhoz két többi végpontok egy a get a Társítások URI értesíti a IoT kezdőképernyő központjában egy befejezett feltöltött tárolására és a többi fájlfeltöltéshez támogatja. Az eszköz elindítja fájl feltöltése a IoT-hubon keresztül csatlakozott a GET küldésével `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. A központi visszatér Társítások URI adott fel kell tölteni a fájlt, és egy korrelációs Azonosítót használható, amikor a feltöltés befejeződött.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Értesítés egy befejezett fájl feltöltésének IoT központi

Az eszköz a fájl feltöltése tárterület az Azure tároló SDK felelős. Miután a feltöltés befejeződött, az eszköz küld egy BEJEGYZÉSBEN az IoT-központban, a `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` a következő JSON szervezethez:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Értékének `isSuccess` van egy logikai típusú, amely a fájl feltöltése sikeresen befejeződött-e vagy sem. Állapotkódja `statusCode` a tárhely, a fájl a Feltöltés állapota és a `statusDescription` felel meg a `statusCode`.

## <a name="reference-topics"></a>Hivatkozás témakörök:

Az alábbi hivatkozás témakörök tartalmaznak, való fájlfeltöltéskor a eszközről további információt.

## <a name="file-upload-notifications"></a>Fájl feltöltésekről tájékoztató értesítések

Amikor egy eszközt feltölt egy fájlt, és értesítést küld a feltöltés befejezésének IoT központi, a szolgáltatás tetszés szerint készít értesítő üzenet, amely tartalmazza a fájl nevét és tárolási helyét.

A [Végpontok]című cikkben ismertetett módon[lnk-endpoints], IoT központi fájl feltöltésekről tájékoztató értesítések – a szolgáltatás elérésű végpont (**/messages/servicebound/fileuploadnotifications**) üzenetként továbbítja. A fájl feltöltésekről tájékoztató értesítések fogadása szemantikáját ugyanaz, mint a felhőben-eszköz üzeneteket, és a ugyanazon [üzenet életciklus][lnk-lifecycle]. Minden üzenet a fájl feltöltése értesítés végpont beolvasása JSON rekordot az alábbi tulajdonságok:

| A tulajdonság | Leírás |
| -------- | ----------- |
| EnqueuedTimeUtc | Az értesítés létrehozásának jelző időbélyeg. |
| DeviceId | **DeviceId** az eszközt, amely a feltölteni a fájlt. |
| BlobUri | A feltöltött fájl URI. |
| BlobName | A feltöltött fájl neve. |
| LastUpdatedTime | A fájl legutóbb frissítésekor jelző időbélyeg. |
| BlobSizeInBytes | A feltöltött fájl méretét. |

**Példa**. Ez a fájl feltöltése értesítő üzenet egy példa törzsében.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Fájl feltöltése értesítési beállítások

Minden egyes IoT központi elérhetővé teszi a fájl feltöltésekről tájékoztató értesítések a következő beállítási lehetőségei:

| A tulajdonság | Leírás | Tartomány- és alapértelmezett |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Meghatározza, hogy fájl feltöltésekről tájékoztató értesítések ír-e a fájl értesítések végpont. | Logikai. Alapértelmezett: igaz. |
| **fileNotifications.ttlAsIso8601** | Alapértelmezett TTL (élettartam) fájl feltöltésekről tájékoztató értesítések. | ISO_8601 intervallum 48 H felfelé (minimális 1 perc). Alapértelmezés: 1 óra értéket. |
| **fileNotifications.lockDuration** | Rögzített időtartam, hogy a fájl feltöltése értesítések várólista. | 5 – 300 másodperc (legalább 5 másodperc). Alapértelmezés: 60 másodperc. |
| **fileNotifications.maxDeliveryCount** | Kézbesítési maximális száma a fájl feltöltése értesítés várólista. | 1 és 100 között. Alapértelmezett: 100. |

## <a name="additional-reference-material"></a>További anyagminta

A Fejlesztőeszközök útmutató hivatkozás témakörök a következők:

- [IoT központi végpontok] [ lnk-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez.
- [Szabályozási és kvóták] [ lnk-quotas] a IoT központi szolgáltatás és számíthat, amikor a szolgáltatás használatához a szabályozási viselkedés vonatkozó kvóták ismerteti.
- [IoT központi eszközök és szolgáltatások SDK] [ lnk-sdks] a különböző nyelvi SDK sorolja fel, egy eszköz és a szolgáltatás-alkalmazások használata a IoT központi fejlesztésekor használja.
- [Lekérdezési nyelv twins, módszerek és feladatok] [ lnk-query] ismerteti a lekérdezésnyelv IoT központi eszköz twins, módszerek és feladatok kapcsolatos adatok kinyerése is használhatja.
- [IoT központi MQTT támogatási] [ lnk-devguide-mqtt] IoT központi támogatási további információt tartalmaz a MQTT protokollhoz.

## <a name="next-steps"></a>Következő lépések

Most már rendelkezik megtanulta IoT elosztót használ eszközökön a fájlok feltöltése, amelyek érdekelhetik a fejlesztői útmutató a következő témakörök:

- [Eszköz identitások IoT-központban kezelése][lnk-devguide-identities]
- [IoT központi való hozzáférés szabályozása][lnk-devguide-security]
- [Állapot és a beállítások szinkronizálása az eszköz twins használatával][lnk-devguide-device-twins]
- [Közvetlen metódus eszközön][lnk-devguide-directmethods]
- [A feladatok ütemezése több eszközön][lnk-devguide-jobs]

Ha azt szeretné, próbálja meg néhány a jelen cikkben ismertetett fogalmak, amelyek érdekelhetik a következő IoT központi oktatóprogram során:

- [A felhőbe IoT központi rendelkező eszközökön a fájlok feltöltése][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
