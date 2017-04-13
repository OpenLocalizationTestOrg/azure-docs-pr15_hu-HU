<properties
    pageTitle="Hasonlóan a IoT átjáró SDK rendelkező eszköz |} Microsoft Azure"
    description="Azure IoT átjáró SDK forgatókönyv Windows használata az Azure IoT átjáró SDK használatával a szimulált eszközről küldő telemetriai mutatja be."
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT átjáró SDK (beta) – eszköz a felhőbe üzenetek küldése a Windows használata szimulált eszközön

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Készítése és a minta futtatása

Mielőtt hozzákezdene, a következőket kell tennie:

- [A fejlesztői környezet beállítása] [ lnk-setupdevbox] a SDK használata a Windows számára.
- [Hozzon létre egy IoT központi] [ lnk-create-hub] az Azure-előfizetése, szüksége lesz az útmutató elvégzéséhez a központi nevét. Ha nem rendelkeznek fiókkal, létrehozhat egy [ingyenes fiókot] [ lnk-free-trial] a mindössze néhány perc.
- Két eszközök hozzáadása a IoT hubon keresztül csatlakozott, és jegyezze fel a azonosítókhoz és az eszköz billentyűk. A használható [eszköz Intéző vagy iothub-explorer] [ lnk-explorer-tools] eszköz eszközén hozzáadása az előző lépésben létrehozott IoT hubon keresztül csatlakozott, valamint igényelhetnek.

A minta létrehozásához:

1. Nyisson meg egy **VS2015 Fejlesztőeszközök parancssorából** parancssort.
2. Nyissa meg a legfelső szintű mappa a helyi példányt az **azure-iot-átjáró-sdk** tárházba.
3. Futtassa a **Eszközök\\build.cmd** parancsfájl. A parancsfájl létrehoz egy Visual Studio megoldásfájlt, a megoldást hoz létre, és a vizsgálatok futtatása. A Visual Studio megoldás **összeállítása** mappában a helyi példányt az **azure-iot-átjáró-sdk** tárházba a található.

A minta futtatása:

Nyissa meg a fájlt egy szövegszerkesztőben **minták\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** az **azure-iot-átjáró-sdk** tárházba, a helyi példányban található. Ez a fájl a modulokat a minta átjáró konfigurálása:

- A **IoTHub** modul a IoT-hubhoz csatlakozik. Meg kell adnia, hogy adatokat küld a IoT-központját. Konkrétan a IoT központi nevét adja meg az **IoTHubName** értéket, és **azure-devices.net** **IoTHubSuffix** értéke. Az egyik **átviteli** értéke: "HTTP", "AMQP" vagy "MQTT". Figyelje meg, hogy jelenleg csak "HTTP" megosztja egy TCP-kapcsolat üzenetekhez eszközt. Ha az érték az "AMQP" vagy "MQTT", az átjáró megőrzi IoT-hubon keresztül csatlakozott az egyes külön TCP kapcsolatot.
- A **leképezési** modul szimulált eszközről a MAC-címek rendel a IoT központi eszköz azonosítóját. Győződjön meg arról, hogy **deviceId** megegyeznek az azonosítók a IoT hubhoz felvett két eszközök, illetve **deviceKey** értékeket tartalmaz a billentyűparancsok két eszközről.
- A **BLE1** és **BLE2** modulok szimulált eszközök. Fontos tudni, hogyan MAC címük megegyeznek a **hozzárendelés** modulban.
- A **naplózási** modul az átjáró tevékenység naplózza a fájlba.
- Az alább látható módon **modul elérési útja** értékek feltételezik, hogy a a IoT átjáró SDK tárat a **c** meghajtó gyökerébe klónozva. Ha másik helyre töltötte le, akkor módosítania kell a **modul elérési útja** értékek ennek megfelelően.
- A **hivatkozások** tömb a JSON-fájl alján a **leképezéshez** , és a **hozzárendelés** modul **IoTHub** modulba a **BLE1** és **BLE2** modulok csatlakozik. Is biztosítható, hogy minden üzenet a **naplózó** modul van-e jelentkezve.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
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
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
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

1. A parancssorablakban nyissa meg a helyi példányt az **azure-iot-átjáró-sdk** tárházba a legfelső szintű mappájára.
2. A következő parancsot:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. A használható [eszköz Intéző vagy iothub-explorer] [ lnk-explorer-tools] eszköz olyan üzenetre, amelyet az átjáró fogad IoT központi figyelése.


## <a name="next-steps"></a>Következő lépések

Ha szerezhet a IoT átjáró SDK csomagjában talál egy speciális megértését és kód példák kísérletezni szeretne, látogasson el a következő Fejlesztőeszközök oktatóanyagok és erőforrások:

- [A valós eszközről a IoT átjáró SDK csomagjában talál az eszköz a felhőbe küldésére][lnk-physical-device]
- [Azure IoT átjáró SDK][lnk-gateway-sdk]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 