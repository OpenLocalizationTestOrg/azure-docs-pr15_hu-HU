<properties
    pageTitle="Kettős tulajdonságainak használata |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként kettős tulajdonságainak használata"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Oktatóprogram: Használni kívánt tulajdonságok eszközök (előzetes verzió) konfigurálása

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Ez az oktatóanyag végén a két Node.js konzol alkalmazások vannak telepítve:

* **SimulateDeviceConfiguration.js**, alkalmazás a szimulált eszközön, megvárja, amíg a kívánt konfiguráció frissítése, és a jelentések egy szimulált konfigurációs frissítési folyamat állapotát.
* **SetDesiredConfigurationAndQuery**, a .NET konzol alkalmazás futtatásához végéről vissza, amely akkor állítja be a kívánt konfiguráció egy eszközön, és a lekérdezések a konfigurációs frissítési folyamat szólnak.

> [AZURE.NOTE] A cikk [IoT központi SDK] [ lnk-hub-sdks] használható eszköz és a háttér-alkalmazások létrehozásához a különböző SDK információt tartalmaz.

Oktatóprogram elvégzéséhez az alábbiakra van szükség:

+ Microsoft Visual Studio 2015.

+ NODE.js verzió 0.10.x vagy újabb verziójában.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

Ha követni, az [első lépések az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már van egy eszköz engedélyezett felügyeleti központban, és egy eszközt identitás nevű **myDeviceId**; és [a szimulált eszköz-alkalmazás létrehozása] kihagyhatja[ lnk-how-to-configure-createapp] szakaszban.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>A szimulált eszköz-alkalmazás létrehozása

Ebben a részben hoz létre, amelyek másként **myDeviceId**a hubhoz csatlakozik, megvárja, amíg a kívánt konfiguráció frissítése és frissítések majd jelentéseket a szimulált konfigurációs frissítési folyamat Node.js konzol alkalmazást.

1. Hozzon létre egy új üres nevű **simulatedeviceconfiguration**. Hozzon létre egy új, az alábbi parancs használatával a parancssorban package.json fájlt a **simulatedeviceconfiguration** mappában. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **simulatedeviceconfiguration** mappában, telepítse az **azure-iot-eszköz**és **azure-iot-eszköz-mqtt** csomag a következő parancs futtatásával:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **SimulateDeviceConfiguration.js** fájl **simulatedeviceconfiguration** mappában.

4. A következő kód hozzáadása a **SimulateDeviceConfiguration.js** fájlt, és a kapcsolati karakterlánc másolta a **myDeviceId** eszköz identitás létrehozásakor **{eszköz kapcsolati karakterlánc}** helyőrzőt helyettesítő:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    Az **ügyfél** vezérlőnek vezérléséhez az eszközről eszköz twins szükséges összes módszereket. Az előző kód után az **ügyfél** objektum állíthatja olvassa be a kettős **myDeviceId**az, és csatolja a kívánt tulajdonságokat frissítés leíró. A kezelő ellenőrzi, hogy van egy tényleges konfigurációs módosítás-kérési összehasonlíthatja a configIds, akkor egy módszer, amely elindítja a konfigurációs módosítást elindítja.

    Ne feledje, hogy az egyszerűség kedvéért az előző kód használja egy csomagolásukkor alapértelmezett a kiindulási konfiguráció. A valós alkalmazás valószínűleg töltenének be, hogy a konfigurációs egy helyi tárból.
    
    > [AZURE.IMPORTANT] A kívánt tulajdonság módosítást események vannak mindig kibocsátott egyszer eszköz csatlakozáskor, ügyeljen arra, hogy ellenőrizze, hogy van egy tényleges módosítást kívánt tulajdonságok bármely művelet végrehajtása előtt.

5. Az alábbi módszerek előtt hozzáadása a `client.open()` meghívása:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    A **initConfigChange** módszer a helyi kettős objektum jelentett tulajdonságai frissíti a config frissítési kérelem állítja az állapot **függőben lévő**, majd frissíti az eszköz kettős a szolgáltatás. Miután sikeresen frissítette a kettős, szimulál, amely a **completeConfigChange**végrehajtását megszakítja hosszú futó folyamatot. Ez a módszer frissíti a helyi kettős jelentett tulajdonságok **sikeres** az állapot beállítása és eltávolítása a **pendingConfig** objektumot. A szolgáltatás a kettős majd frissíti.

    Megjegyzés: sávszélességet, mentéséhez, bejelenteni tulajdonságait módosítani kell cserélni a teljes dokumentum helyett (elnevezett **javítás** a fenti kód), csak a tulajdonságok megadásával frissülnek.

    > [AZURE.NOTE] Ebben az oktatóanyagban nem szimulálhatja bármely viselkedés egyidejű konfigurációs frissítéseit. Néhány konfigurációs frissítési folyamat cél konfiguráció módosítások igazodik, miközben fut-e a frissítés, mások kell kattintania a sorból vagy mások is hibát elvetése lehet. Ügyeljen arra, hogy fontolja meg a kívánt a konkrét beállítási folyamat működését, és a megfelelő logika hozzáadása a konfigurációs módosítást kezdeményezése előtt.

6. Az eszköz app futtatásával:

        node SimulateDeviceConfiguration.js

    Meg kell jelennie a üzenet `retrieved device twin`. Hagyja meg az alkalmazást futtató.

## <a name="create-the-service-app"></a>A szolgáltatás-alkalmazás létrehozása

Ez a szakasz létrehoz egy .NET konzol alkalmazás, amely a *kívánt tulajdonságokat* a kettős **myDeviceId** új telemetriai konfigurációs objektummal társított frissíti. Ezután az eszköz twins a központi tárolt lekérdezések és a kívánt és jelenteni az eszköz konfigurációk közötti különbséget jeleníti meg.

1. A Visual Studióban hozzáadása vizuális C# klasszikus Windows asztal projekt az aktuális megoldás a **New** project-sablon segítségével. A projekt **SetDesiredConfigurationAndQuery**neve.

    ![Új képi C# klasszikus Windows asztal projekt][img-createapp]

2. Kattintson a jobb gombbal a **SetDesiredConfigurationAndQuery** projekt megoldás Explorerben, és válassza a **Nuget csomagok kezelése**.

3. **Nuget csomag kezelő** ablakban győződjön meg arról, hogy be van-e jelölve **Belefoglalás előzetes** , **microsoft.azure.devices**keresni, jelölje ki a **telepíteni** telepítése a a **Microsoft.Azure.Devices** *előzetes* verziójának csomagolása, és fogadja el a használati feltételeket. Ez az eljárás letöltések, telepíti, és hozzáadja a [Microsoft Azure IoT szolgáltatás SDK] hivatkozás[ lnk-nuget-service-sdk] Nuget csomag és függőségét.

    ![Nuget csomag kezelőjének ablaka][img-servicenuget]

4. Adja hozzá a következő `using` kimutatások **Program.cs** fájl tetején:

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. Az alábbi mezők hozzáadása a **Program** osztály. Helyőrző értékét cserélje le a kapcsolati karakterláncot az IoT-központban, az előző részben létrehozott az.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Az alábbi módon adja hozzá a **Program** osztály:

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    A **beállításjegyzék** vezérlőnek eszköz twins a szolgáltatásból vezérléséhez szükséges összes módszereket. Az előző kódot, után a **beállításjegyzék** objektum állíthatja olvassa be az **myDeviceId**a kettős, és a kívánt tulajdonságait frissíti egy új telemetriai konfigurációs objektummal.
    Ezt követően minden 10 másodperc azt kérdezi le az a központi tárolt twins és nyomtatja ki a kívánt és jelentett telemetriai beállításokat. Olvassa el a [IoT központi lekérdezési nyelv] [ lnk-query] akiket minden eszközén sokoldalú jelentések készítése című témakörből.

    > [AZURE.IMPORTANT] Ez az alkalmazás minden 10 másodperc szemléltető célokra IoT központi kérdezi le. Lekérdezések használata a felhasználó elérésű jelentések készítése minden több eszközön, és nem észleli azokat a módosításokat. Ha a megoldás igényel eszköz események valós idejű értesítések használata [eszköz a felhőbe üzenetek][lnk-d2c].

7. Végül adja hozzá a következő sort a **fő** módszer:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. Az **F5 billentyűt** , és használja a Visual Studio .NET alkalmazás kell jelennie a jelentett konfigurációja, módosítsa a **munka sikerét** **függőben** **sikeres** ismét az új aktív a Futtatás a **SimulateDeviceConfiguration.js** futtatása, küldje el a gyakoriság öt perc helyett 24 óra.

    > [AZURE.IMPORTANT] Nincs a felfelé az eszköz jelentés művelet és a lekérdezés eredménye között egy perc késés. Ez a ahhoz, hogy a lekérdezés infrastruktúra egyidejű nagyon magas skála. Beolvashatja egy egyetlen kettős egységes nézeteinek használja a **getDeviceTwin** módszert a **beállításjegyzék** osztály.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban legyen a kívánt konfiguráció *szükséges tulajdonságok* vissza végéről, és egy eszközt alkalmazását észleli, hogy a változást, hasonlóan *bejelentett* tulajdonságként Jelentéskészítés állapota a kettős több lépés frissítés folyamat írt.

Az alábbi források segítségével megtudhatja, hogy miként:

- telemetriai küldése eszközökről való az [első lépések a IoT központi] [ lnk-iothub-getstarted] oktatóanyagban
- ütemezése vagy nagy adathalmazok eszközök el műveletek [használata feladatok ütemezése és közvetítése eszköz műveletek] hajthatók végre[ lnk-schedule-jobs] oktatóprogram.
- szabályozhatja az eszközök interaktív (például bekapcsolása a felhasználó által felügyelt alkalmazásból ventilátor), és a [használat közvetlen módszerek] [ lnk-methods-tutorial] oktatóprogram.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
