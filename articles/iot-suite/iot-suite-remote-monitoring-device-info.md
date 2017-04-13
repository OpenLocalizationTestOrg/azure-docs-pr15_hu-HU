<properties
 pageTitle="Eszköz információk metaadatok a távoli felügyeleti megoldásban |} Microsoft Azure"
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
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Távoli nyomon követése eszköz információk metaadatok előre megoldás

Az Azure IoT csomagot előre beállított megoldás figyelése távoli eszköz metaadatok kezeléséhez megközelítés mutatja be. Ez a cikk ismerteti a megoldás lehetővé teszi azok megértéséhez szükséges megközelítés:

- Milyen eszközt metaadatok a megoldást tárolja.
- A megoldás hogyan kezeli az eszköz metaadatokat.

## <a name="context"></a>Környezet

A távoli felügyeleti előre beállított megoldás használ [Azure IoT központi] [ lnk-iot-hub] ahhoz, hogy az adatok küldése a felhőbe eszközöket. IoT központi tartalmaz egy [eszköz identitás beállításjegyzék] [ lnk-identity-registry] IoT központi való hozzáférés szabályozása. A központi IoT eszköz identitás beállításjegyzék a távoli eszköz információk metaadatok tároló megoldás-specifikus *eszköz beállításjegyzék* figyelése külön. A távoli felügyeleti megoldás használ egy [DocumentDB] [ lnk-docdb] végrehajtásához az eszköz rendszerleíró eszköz információk metaadatok tárolására szolgáló adatbázis. A [Microsoft Azure IoT hivatkozás architektúra] [ lnk-ref-arch] ismerteti a szerepkör a eszköz beállításjegyzék egy tipikus IoT megoldás.

> [AZURE.NOTE] A távoli felügyeleti előre beállított megoldást továbbra is az eszköz identitás beállításjegyzék eszköz rendszerleíró szinkronban. Az azonos eszközazonosítót segítségével mindkét egyértelműen azonosítják az egyes a IoT elosztóhoz csatlakoztatott eszközt.

A [IoT központi eszköz management előzetes] [ lnk-dm-preview] szolgáltatások hozzáadása IoT-hubon keresztül csatlakozott, az eszköz információk felügyeleti funkciókat, a jelen cikkben leírt hasonlítanak. Jelenleg a távoli felügyeleti megoldást csak általában elérhető (kiadás) szolgáltatásokat használ IoT-központban.

## <a name="device-information-metadata"></a>Metaadat-alapú adatok eszköz

Eszköz információk metaadatok JSON dokumentum az eszköz rendszerleíró DocumentDB adatbázisban tárolt szerkezete a következő:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: magára az eszközre tulajdonságokból ír, és az eszközt a szervezet számára az adatok. Más példa eszköz tulajdonságainak gyártó, modell, és sorozatszám tartalmazza. 
- **DeviceID**: az egyedi eszközazonosítót. Ez az érték megegyezik a IoT központi eszköz identitás beállításjegyzékben.
- **HubEnabledState**: állapotát az eszköz IoT-központban. A kezdeti értéke **Null** mindaddig, amíg az eszköz először csatlakozik. A megoldás portálon a **null** érték megfelelője az eszköz alatt "regisztrált, de nincs jelen."
- **CreatedTime**: az eszköz az idő hozták létre.
- **DeviceState**: állapotát az eszköz által jelzett.
- **UpdatedTime**: az idő, az eszköz legutolsó frissítés a megoldást portálon keresztül.
- **SystemProperties**: A megoldás portál ír a Rendszertulajdonságok és az eszköz nem ismeri a tulajdonságokból. Egy példa rendszer tulajdonság a **ICCID** , ha a megoldás engedélyezett és SIM engedélyező eszközkezelésről szolgáltatás csatlakozik.
- **Parancsok**: a parancsok listája az eszköz támogatja. Az eszköz megadja ezt az információt a megoldást.
- **CommandHistory**: a küld a távoli felügyeleti megoldás az eszközt, és azokat a parancsokat állapotának parancsok listája.
- **IsSimulatedDevice**: jelző, amely azonosítja szimulált eszközként egy eszközt.
- **azonosító**: ehhez a dokumentumhoz eszköz DocumentDB egyedi azonosítója.

> [AZURE.NOTE] Eszközök adatainak is tartalmazhatnak metaadatok ahhoz a telemetriai IoT központi küld az eszközön. A távoli felügyeleti megoldás a telemetriai metaadatok használ, ha testre szeretné szabni, hogy hogyan jelenítse meg az irányítópulton a [dinamikus telemetriai][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Életciklus

Első létrehozásakor egy eszközt a megoldást portálon, a megoldás egy bejegyzést a eszköz beállításjegyzékben, ahogy azt korábban hoz létre. Az információk nagy kezdetben a stubbed, és a **HubEnabledState** értéke **null**. Ezen a ponton a megoldást is létrehoz egy bejegyzést, az eszköz az eszköz identitás beállításjegyzékében, amely hoz létre a billentyűk az eszközön használja a IoT központi hitelesítést végezni.

Amikor először kapcsolódik egy eszközt a megoldást, eszköz információk üzenetet küld. Ez az eszköz információk üzenet eszközök tulajdonságait, például a gyártó, modell száma és időértékét tartalmazza. Egy eszköz tájékoztató üzenetet is beleértve parancs paramétereket vonatkozó adatokat támogatja az eszköz a parancsok listája. Ha a megoldás ezt az üzenetet kap, az eszköz információk metaadat-alapú eszköz beállításjegyzékbeli frissíti.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>A megoldás portálon eszköz adatainak megtekintése és szerkesztése

Az eszközök listája az megoldás portálon jeleníti meg az alábbi eszköz tulajdonságainak oszlopok: **állapot**, **DeviceId**, **gyártó**, **Modell száma**, **Sorozatszám**, **belső vezérlőprogram**, **Platform**, **processzor**és **RAM telepítve**. Az eszköz tulajdonságainak **Szélesség** és **hosszúság** a meghajtó a helyet, a Bing-térkép az irányítópulton. 

![Eszközök listája][img-device-list]

Ha a megoldás portálon **Eszköz** ablaktáblában a **Szerkesztés** gombra kattint, ezeket a tulajdonságokat módosíthatja. A Tulajdonságok szerkesztése frissíti az a rekordot, az eszköz a DocumentDB adatbázisban. Azonban egy eszközt egy frissített eszköz információ üzenetet küld, ha felülírja a megoldást portál módosításai. **Hostname (állomásnév)**, **HubEnabledState**, **CreatedTime**, **DeviceState**és a megoldás portálon **UpdatedTime** tulajdonságok **DeviceId**nem szerkesztheti, mert csak az eszköznek hitelesítésszolgáltató tulajdonságokból fölé.

![Eszköz szerkesztése][img-device-edit]

A megoldás portal segítségével egy eszközt eltávolít a megoldás. Amikor egy eszközt eltávolít, a megoldást eltávolítja az eszköz információk metaadatok a megoldást eszköz beállításjegyzékéből, valamint eszköz bejegyzést IoT központi eszköz identitás beállításjegyzékbeli. Mielőtt eszköz eltávolításához le kell tiltania.

![Eszköz törlése][img-device-remove]

## <a name="device-information-message-processing"></a>Eszköz információk üzenetek feldolgozása

Eszköz tájékoztató üzenetek eszköz által küldött üzenetek telemetriai különböznek. Eszköz tájékoztató üzenetek írja be például eszköz tulajdonságainak, az eszköz reagálni tud a parancsok és a minden parancs előzmények információkat. IoT központi magát a metaadatok található egy eszköz tájékoztató üzenetet nem tud, és az üzenet feldolgozásával módon, mint bármely eszközön a felhőbe üzenetek feldolgozásával. A távoli felügyeleti megoldás, egy [Azure Értékáram-elemzés] [ lnk-stream-analytics] (ASA) feladat beolvassa az üzenetek IoT központból. A **DeviceInfo** Értékáram-elemzés feladat tartalmazó üzenetek szűrőit **"Objektumtípus": "DeviceInfo"** és továbbítja őket a webes feladat futó **EventProcessorHost** host példány. Logika a **EventProcessorHost** példány az eszközazonosítót használja a DocumentDB rekord keresse meg a kívánt eszközre, és frissítse a rekordot. Az eszköz rendszerleíró rekord most már megtalálhatók az információkat, például eszközök tulajdonságait, a parancsok és a parancs előzmények.

> [AZURE.NOTE] Egy eszköz tájékoztató üzenetet egy normál eszköz a felhőbe üzenetet. A megoldás ASA lekérdezések használatával eszköz tájékoztató üzenetek és telemetriai üzenetek közötti különbséget tesz.

## <a name="example-device-information-records"></a>Példa eszköz információk rekordok

A távoli felügyeleti előre beállított megoldás használ eszköz információs rekordokat kétféle: a megoldást, és a rekordok egyéni eszközök csatlakozik a megoldást üzembe szimulált eszközök rekordokat.

### <a name="simulated-device"></a>Szimulált eszközön

A következő példa bemutatja a JSON eszköz adatokat tartalmazó rekord egy szimulált eszközhöz. Ez a rekord egy értékre van állítva az **UpdatedTime**, amely azt jelzi, hogy az eszköz üzenetet küldött a **DeviceInfo** IoT-hubon keresztül csatlakozott. A rekord tartalmaz néhány gyakori eszközök tulajdonságait, hat definiálja parancsok a szimulált eszközök támogatják, és a **IsSimulatedDevice** jelző túllépését **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Egyéni eszköz

Az alábbi példa a JSON eszköz adatokat tartalmazó rekord egy egyéni eszközhöz jeleníti meg, és a **IsSimulatedDevice** jelző be van állítva **a 0**van. Láthatja, hogy az egyéni eszköz támogatja-e két parancsot, és, hogy a megoldás portál elküldte **SetTemperature** parancs az eszközre.

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

A következő üzenet jelenik meg a JSON **DeviceInfo** frissítenie kell a eszköz információk metaadatokat küldött eszköz:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Következő lépések

Miután befejezte a tanulási hogyan testre szabhatja az előre definiált megoldások, megismerheti, egyéb funkciók közül egyesek és az előre beállított IoT csomagja megoldások lehetőségeit:

- [Előre beállított prediktív karbantartási megoldás – áttekintés][lnk-predictive-overview]
- [Gyakori kérdések IoT csomagja][lnk-faq]
- [IoT biztonsági alapoktól][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
