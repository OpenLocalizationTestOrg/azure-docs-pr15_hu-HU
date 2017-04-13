<properties
    pageTitle="Hasonlóan a IoT átjáró SDK rendelkező eszköz |} Microsoft Azure"
    description="Azure IoT átjáró SDK forgatókönyv Linux használatával az Azure IoT átjáró SDK használatával a szimulált eszközről küldő telemetriai mutatja be."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-linux"></a>Azure IoT átjáró SDK (beta) – segítségével Linux szimulált eszközön eszköz a felhőbe az üzenetek küldése

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Készítése és a minta futtatása

Mielőtt hozzákezdene, a következőket kell tennie:

- [A fejlesztői környezet beállítása] [ lnk-setupdevbox] a Linux a SDK csomagjában talál.
- [Hozzon létre egy IoT központi] [ lnk-create-hub] az Azure-előfizetése, szüksége lesz az útmutató elvégzéséhez a központi nevét. Ha nem rendelkeznek fiókkal, létrehozhat egy [ingyenes fiókot] [ lnk-free-trial] a mindössze néhány perc.
- Két eszközök hozzáadása a IoT hubon keresztül csatlakozott, és jegyezze fel a azonosítókhoz és az eszköz billentyűk. A használható [eszköz Intéző vagy iothub-explorer] [ lnk-explorer-tools] eszköz eszközén hozzáadása az előző lépésben létrehozott IoT hubon keresztül csatlakozott, valamint igényelhetnek.

A minta létrehozásához:

1. Nyissa meg a rendszerhéj.
2. Nyissa meg a helyi példányt az **azure-iot-átjáró-sdk** tárházba, a legfelső szintű mappában.
3. A **tools/build.sh** parancsfájl futtatásához. A parancsfájl a **cmake** segédprogram hozzon létre egy **összeállítása** a legfelső szintű mappájára a helyi példányt az **azure-iot-átjáró-sdk** tárházba nevű mappát, és egy makefile létrehozásához. A parancsprogram a megoldást hoz létre, majd a vizsgálatok futtatása.

> [AZURE.NOTE]  Minden alkalommal futtatja a **build.sh** parancsfájlt, törli, és ezután létrehozza a legfelső szintű mappájára a helyi példányt az **azure-iot-átjáró-sdk** tárházba **összeállítása** mappájában.

A minta futtatása:

Egy egyszerű szövegszerkesztő programban nyissa meg a helyi példányt az **azure-iot-átjáró-sdk** tárházba a fájl **samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json** . Ez a fájl a modulokat a minta átjáró konfigurálása:

- A **IoTHub** modul a IoT-hubhoz csatlakozik. Meg kell adnia, hogy adatokat küld a IoT-központját. Konkrétan a IoT központi nevét adja meg az **IoTHubName** értéket, és **azure-devices.net** **IoTHubSuffix** értéke. Az egyik **átviteli** értéke: "HTTP", "AMQP" vagy "MQTT". Figyelje meg, hogy jelenleg csak "HTTP" megosztja az összes eszközt üzenetek egy TCP-kapcsolat. Ha az érték az "AMQP" vagy "MQTT", az átjáró megőrzi IoT-hubon keresztül csatlakozott az egyes külön TCP kapcsolatot.
- A **leképezési** modul szimulált eszközről a MAC-címek rendel a IoT központi eszköz azonosítóját. Győződjön meg arról, hogy **deviceId** megegyeznek az azonosítók a IoT hubhoz felvett két eszközök, illetve **deviceKey** értékeket tartalmaz a billentyűparancsok két eszközről.
- A **BLE1** és **BLE2** modulok szimulált eszközök. Fontos tudni, hogyan MAC címük megegyeznek a **hozzárendelés** modulban.
- A **naplózási** modul az átjáró tevékenység naplózza a fájlba.
- Az alább látható módon **modul elérési útja** értékek feltételezik, hogy a minta futtatja a helyi példányt az **azure-iot-átjáró-sdk** tárházba legfelső szintjén.
- A **hivatkozások** tömb a JSON-fájl alján a **leképezéshez** , és a **hozzárendelés** modul **IoTHub** modulba a **BLE1** és **BLE2** modulok csatlakozik. Is biztosítható, hogy minden üzenet naplózza a **naplózó** modul.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "./build/modules/iothub/libiothub_hl.so",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "./build/modules/identitymap/libidentity_map_hl.so",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "./build/modules/simulated_device/libsimulated_device_hl.so",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "./build/modules/logger/liblogger_hl.so",
            "args":
            {
                "filename":"./deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}

```

A beállítási fájl végzett módosítások mentéséhez.

A minta futtatása:

1. A rendszerhéj nyissa meg a legfelső szintű mappa a helyi példányt az **azure-iot-átjáró-sdk** tárházba.
2. A következő parancsot:

    ```
    ./build/samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ./samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```

3. A használható [eszköz Intéző vagy iothub-explorer] [ lnk-explorer-tools] eszköz olyan üzenetre, amelyet az átjáró fogad IoT központi figyelése.

## <a name="next-steps"></a>Következő lépések

Ha szerezhet a IoT átjáró SDK csomagjában talál egy speciális megértését és kód példák kísérletezni szeretne, látogasson el a következő Fejlesztőeszközök oktatóanyagok és erőforrások:

- [A valós eszközről a IoT átjáró SDK csomagjában talál az eszköz a felhőbe küldésére][lnk-physical-device]
- [Azure IoT átjáró SDK][lnk-gateway-sdk]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [A IoT megoldás az alapoktól kezdve biztonságos mentése][lnk-securing]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-create-hub]: iot-hub-create-through-portal.md